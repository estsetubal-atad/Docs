# ATAD | Makefiles

Um *makefile* é um ficheiro (por defeito com o nome "makefile") que contém um conjunto de diretivas utilizadas pela ferramenta de automatização de compilação *make* para gerar um alvo/objetivo (*target*).

Quer o seu programa seja composto por um ou vários ficheiros fonte, o processo é relativamente simples. Dentro do seu diretório de trabalho deve criar um ficheiro (sem extensão) chamado `makefile`. Se pretender fazê-lo através da CLI, utilize o comando `$> touch makefile`.

Deverá obter uma estrutura de ficheiros semelhante a esta:

```markdown
- <WorkingDirectory>
    - main.c
    - time.c
    - time.h
    - makefile
```

De seguida, deve editar o ficheiro *makefile*. Pode definir qualquer número de diretivas para utilizar posteriormente; uma diretiva consiste numa sequência nomeada de comandos. Exemplo do conteúdo de um *makefile*:

```make
default:
    gcc -Wall -o prog main.c time.c
debug:
    gcc -Wall -o prog -g  main.c time.c
clean:
    rm -f prog
```

Pode observar três diretivas, nomeadamente `default`, `debug` e `clean`. Pode escolher quaisquer nomes, desde que sejam significativos. A diretiva `default` é a primeira e, por isso, a diretiva por defeito. Pode invocar estas diretivas utilizando o comando `make`:

```console
$> make
```

Isto executará a diretiva por defeito, ou seja, o comando `gcc -Wall -o prog main.c time.c`.

Se, em alternativa, executar:

```console
$> make debug
```

Será executada a diretiva `debug`, isto é, o comando `gcc -Wall -o prog -g main.c time.c`.

Por fim, se executar:

```console
$> make clean
```

O ficheiro executável `prog` será removido, caso exista.

## Autor e suporte

Bruno Silva ([bruno.silva@estsetubal.ips.pt](mailto:bruno.silva@estsetubal.ips.pt))

Deverá contactar o seu docente de PL para qualquer apoio relativamente a estes conteúdos e procedimentos.
