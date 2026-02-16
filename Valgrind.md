# ATAD | Valgrind

*Valgrind* é uma ferramenta de deteção de má gestão de memória. Permite identificar *memory leaks*, erros de libertação de memória, entre outros. Na realidade, o Valgrind é um invólucro (*wrapper*) para um conjunto de ferramentas que realizam diversas tarefas (por exemplo, análise de cache); contudo, aqui focamo-nos na ferramenta por omissão, *memcheck*.

O Memcheck consegue detetar:

* Utilização de memória não inicializada
* Leitura/escrita de memória após ter sido libertada (*free*)
* Leitura/escrita para além dos limites de blocos alocados com `malloc`
* Leitura/escrita em zonas inadequadas da *stack*
* *Memory leaks* — quando os apontadores para blocos alocados com `malloc` são perdidos definitivamente
* Uso incorreto de `malloc/calloc/realloc` versus `free`
* Sobreposição de apontadores `src` e `dst` em `memcpy()` e funções relacionadas

---

Considere o seguinte programa em `test.c`:

```cpp
#include <stdio.h>
#include <stdlib.h>

int main()
{
  char *p;

  // Allocation #1 of 19 bytes
  p = (char *) malloc(19);

  // Allocation #2 of 12 bytes
  p = (char *) malloc(12);
  free(p);

  // Allocation #3 of 16 bytes
  p = (char *) malloc(16);

  return 0;
}
```

```bash
$> gcc -o test -g test.c
```

Isto cria um executável com o nome `test`. Para verificar *memory leaks* durante a execução do programa, execute:

```bash
$> valgrind --leak-check=full ./test
```

Será apresentado no terminal um relatório semelhante ao seguinte:

```markdown
==9704== Memcheck, a memory error detector for x86-linux.
==9704== Copyright (C) 2002-2004, and GNU GPL'd, by Julian Seward et al.
==9704== Using valgrind-2.2.0, a program supervision framework for x86-linux.
==9704== Copyright (C) 2000-2004, and GNU GPL'd, by Julian Seward et al.
==9704== For more details, rerun with: -v
==9704== 
==9704== 
==9704== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 11 from 1)
==9704== malloc/free: in use at exit: 35 bytes in 2 blocks.
==9704== malloc/free: 3 allocs, 1 frees, 47 bytes allocated.
==9704== For counts of detected errors, rerun with: -v
==9704== searching for pointers to 2 not-freed blocks.
==9704== checked 1420940 bytes.
==9704== 
==9704== 16 bytes in 1 blocks are definitely lost in loss record 1 of 2
==9704==    at 0x1B903D38: malloc (vg_replace_malloc.c:131)
==9704==    by 0x80483BF: main (test.c:15)
==9704== 
==9704== 
==9704== 19 bytes in 1 blocks are definitely lost in loss record 2 of 2
==9704==    at 0x1B903D38: malloc (vg_replace_malloc.c:131)
==9704==    by 0x8048391: main (test.c:8)
==9704== 
==9704== LEAK SUMMARY:
==9704==    definitely lost: 35 bytes in 2 blocks.
==9704==    possibly lost:   0 bytes in 0 blocks.
==9704==    still reachable: 0 bytes in 0 blocks.
==9704==         suppressed: 0 bytes in 0 blocks.
```

Vamos analisar o código para perceber o que aconteceu:

* A *Allocation #1* (fuga de 19 bytes) é perdida porque `p` passa a apontar para outro bloco antes de a memória da *Allocation #1* ser libertada com `free`. Para ajudar a localizar o problema, o Valgrind apresenta um *stack trace* indicando onde os bytes foram alocados. No caso da fuga de 19 bytes, a alocação ocorreu em `test.c`, linha 8.

* A *Allocation #2* (12 bytes) não aparece na lista porque foi corretamente libertada com `free`.

* A *Allocation #3* aparece na lista, apesar de ainda existir uma referência (`p`) no momento da terminação do programa. Isto continua a ser uma fuga de memória. Mais uma vez, o Valgrind indica onde ocorreu a alocação (`test.c`, linha 15).

---

O Valgrind consegue detetar vários tipos de erros.
Segue-se uma explicação das mensagens de erro mais comuns do Memcheck.

---

## Explicação das mensagens de erro do Memcheck

Apesar da sua sofisticação interna, o Memcheck apenas deteta essencialmente dois tipos de erros: utilização de endereços ilegais e utilização de valores indefinidos. Ainda assim, isto é suficiente para descobrir muitos problemas de gestão de memória.

Segue-se um resumo do significado das mensagens de erro.

---

### Leitura ilegal / Escrita ilegal

Por exemplo:

```markdown
  Invalid read of size 4
     at 0x40F6BBCC: (within /usr/lib/libpng.so.2.1.0.9)
     by 0x40F6B804: (within /usr/lib/libpng.so.2.1.0.9)
     by 0x40B07FF4: read_png_image__FP8QImageIO (kernel/qpngio.cpp:326)
     by 0x40AC751B: QImageIO::read() (kernel/qimage.cpp:3621)
     Address 0xBFFFF0E0 is not stack'd, malloc'd or free'd
```

Este erro ocorre quando o programa lê ou escreve memória num local que o Memcheck considera inválido. No exemplo, foi feita uma leitura de 4 bytes no endereço `0xBFFFF0E0`, numa biblioteca do sistema (`libpng.so.2.1.0.9`), chamada a partir de outro ponto da mesma biblioteca, que por sua vez foi chamada a partir da linha 326 de `qpngio.cpp`, e assim sucessivamente.

O Memcheck tenta perceber a que corresponde o endereço ilegal. Por exemplo:

* Se o endereço pertencer a um bloco já libertado, informará onde esse bloco foi libertado;
* Se estiver imediatamente após o fim de um bloco alocado (erro típico *off-by-one* em arrays), indicará onde ocorreu a alocação.

Neste exemplo, o endereço está na *stack*, mas não corresponde a uma posição válida (está abaixo do ponteiro de stack, `%esp`). Em alguns casos, isto pode resultar de código inválido gerado pelo compilador.

Note que o Memcheck apenas informa que o programa vai aceder a memória inválida. Não impede o acesso. Se o acesso resultar normalmente numa *segmentation fault*, o programa continuará a falhar — mas o Memcheck apresentará uma mensagem antes disso.

---

### Utilização de valores não inicializados

Por exemplo:

```markdown
  Conditional jump or move depends on uninitialised value(s)
     at 0x402DFA94: _IO_vfprintf (_itoa.h:49)
     by 0x402E8476: _IO_printf (printf.c:36)
     by 0x8048472: main (tests/manuel1.c:8)
     by 0x402A6E5E: __libc_start_main (libc-start.c:129)
```

Este erro é reportado quando o programa utiliza um valor que não foi inicializado — ou seja, cujo conteúdo é indefinido. No exemplo, o valor indefinido é usado internamente pela função `printf()`.

Este erro resulta da execução do seguinte programa:

```cpp
  int main()
  {
    int x;
    printf ("x = %d\n", x);
  }
```

É importante compreender que o programa pode copiar dados não inicializados livremente sem que o Memcheck emita um erro. O erro só é reportado quando o valor indefinido é efetivamente utilizado. Neste caso, `x` não foi inicializada, e o erro surge quando `_IO_vfprintf` precisa de usar o seu valor para o converter numa *string*.

Fontes comuns de dados não inicializados:

* Variáveis locais não inicializadas;
* Conteúdo de blocos alocados com `malloc`, antes de serem inicializados pelo programador.

---

### Libertações ilegais (*Illegal frees*)

Por exemplo:

```markdown
  Invalid free()
     at 0x4004FFDF: free (vg_clientmalloc.c:577)
     by 0x80484C7: main (tests/doublefree.c:10)
     by 0x402A6E5E: __libc_start_main (libc-start.c:129)
     by 0x80483B1: (within tests/doublefree)
     Address 0x3807F7B4 is 0 bytes inside a block of size 177 free'd
     at 0x4004FFDF: free (vg_clientmalloc.c:577)
     by 0x80484C7: main (tests/doublefree.c:10)
     by 0x402A6E5E: __libc_start_main (libc-start.c:129)
     by 0x80483B1: (within tests/doublefree)
```

O Memcheck mantém registo de todos os blocos alocados com `malloc/calloc/realloc`, podendo assim verificar se o argumento passado a `free` é válido.

Neste exemplo, o programa libertou o mesmo bloco duas vezes (*double free*). Tal como nos erros de leitura/escrita ilegal, o Memcheck tenta interpretar o endereço libertado e indica que o bloco já tinha sido libertado anteriormente, facilitando a identificação do erro.

---

## Autor e apoio

Bruno Silva ([bruno.silva@estsetubal.ips.pt](mailto:bruno.silva@estsetubal.ips.pt))

Para qualquer dúvida relacionada com estes conteúdos e procedimentos, deverá contactar o seu docente de PL.
