# ATAD | Documentação com Doxygen

**Doxygen** é um gerador de documentação, uma ferramenta para escrever documentação de referência de *software*. A **documentação é escrita no próprio código**, sendo assim relativamente fácil mantê-la atualizada. O Doxygen pode criar referências cruzadas entre a documentação e o código, permitindo que o leitor consulte facilmente a implementação correspondente (*wikipedia*).

## Instalação

Deve garantir que tem o pacote `doxygen` instalado.

```console
$> sudo apt install doxygen
```

## Documentar o código

Utilize a seguinte convenção para todas as suas funções:

```cpp
/**
* @brief Obtém a parte real do número `Complex`.
*
* Esta função recebe um apontador para um número complexo existente
* e devolve, por referência, a parte real do número.
*
* @param c [in] Apontador PtComplex para o número `Complex`
* @param re [out] Apontador para a variável que irá receber o valor
*
* @return COMPLEX_OK e a parte real em 're'
* @return COMPLEX_NULL se 'c' for NULL
*/
int complexRe (PtComplex c, double *re);
```

* `@brief` — utilizado para fornecer um resumo da função (obrigatório);

  * As linhas seguintes podem ser usadas para fornecer detalhes adicionais (opcional);

* `@param` — utilizado para cada parâmetro da função, seguido do *nome do parâmetro* e de *[in]* ou *[out]*, conforme seja parâmetro de entrada ou de saída (por referência), respetivamente;

* `@return` — utilizado para indicar todos os valores de retorno possíveis e efeitos secundários da função (um por cada possibilidade, caso existam múltiplos valores de retorno).

Um exemplo mais completo e complexo pode ser consultado em:
[http://fnch.users.sourceforge.net/doxygen_c.html](http://fnch.users.sourceforge.net/doxygen_c.html)

## Utilização

Depois de o seu código estar documentado, é necessário gerar a documentação. O Doxygen pode produzir vários formatos de saída, mas estaremos interessados apenas na saída em *HTML*, que pode ser visualizada num navegador.

**O ficheiro `Doxyfile` deve estar no mesmo diretório que todos os seus ficheiros fonte**.

### Doxyfile modelo

**Este ficheiro já está incluído** no repositório `CProgram_Template` do *GitHub*.

Para outros projetos, pode descarregar uma versão pré-configurada utilizando `wget` (assumindo que se encontra no diretório de trabalho):

```console
$> wget https://raw.githubusercontent.com/estsetubal-atad/CProgram_Template/master/Doxyfile
```

## Gerar a documentação com Doxygen

O seguinte comando irá gerar a documentação pretendida, dentro de uma pasta `html` criada automaticamente:

```console
$> doxygen Doxyfile
```

### Configuração manual

**Pode ignorar esta secção** se estiver a utilizar o `Doxyfile` pré-configurado.

1. No seu diretório de trabalho (que contém os ficheiros fonte documentados), execute o seguinte comando:

   ```console
   $> doxygen -g
   ```

   Este comando cria o ficheiro de configuração padrão do Doxygen com o nome "Doxyfile".

2. Deve editar o ficheiro "Doxyfile" num editor de texto à sua escolha:

   * Localize "EXTRACT_ALL" e altere o valor para **YES**.
   * Localize "GENERATE_LATEX" e altere o valor para **NO**.
   * Localize "DISABLE_INDEX" e altere o valor para **YES**.
   * Localize "GENERATE_TREEVIEW" e altere o valor para **YES**.

3. Guarde o ficheiro. Pode copiá-lo posteriormente para outros diretórios de trabalho, evitando repetir os passos 1 e 2.

## Autor e suporte

Bruno Silva ([bruno.silva@estsetubal.ips.pt](mailto:bruno.silva@estsetubal.ips.pt))

Deverá contactar o seu docente de PL para qualquer apoio relativamente a estes conteúdos e procedimentos.
