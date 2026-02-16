# ATAD | Utilização básica do GitHub CLI

Este tutorial apresenta os comandos básicos do *git* através da CLI (*command line interface*). No entanto, o *VS Code* tem uma integração com *git* bastante competente; documentação disponível em [https://code.visualstudio.com/docs/editor/github](https://code.visualstudio.com/docs/editor/github).

Este documento não aborda *pull requests* nem *issues*.

## Credenciais por defeito do git

**Este não é um passo obrigatório**, pois ser-lhe-ão pedidas as credenciais sempre que necessário, quer na CLI quer no VS Code.

No entanto, se utilizar apenas uma conta de *GitHub*, poderá ser preferível configurar estes dados, pois assim não lhe será pedido o nome de utilizador posteriormente (a palavra-passe é sempre solicitada):

Os valores de `user.name` e `user.email` **devem corresponder** à informação da sua conta de *GitHub*:

```console
$> git config --global user.name "brunomnsilva-estsetubal"
$> git config --global user.email "bruno.silva@estsetubal.ips.pt"
```

> Aparentemente, a partir de 21 de agosto o *GitHub* passará a exigir autenticação por *token*. Se necessário, consulte [https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/) para instruções.

## Clonar a partir do repositório upstream

Isto permite-lhe *clonar* o repositório *upstream* para o seu sistema de ficheiros local.

> Se não for o proprietário do repositório *upstream*, não poderá fazer *commit* nem *push* das alterações.

O URL do repositório está disponível através do botão verde **Code** na página do projeto no *GitHub*.

```console
$> git clone https://github.com/estsetubal-atad/CProgram_Template.git MyCProgram
$> cd MyCProgram
```

Aqui, `MyCProgram` é o diretório que será criado com o código do repositório. Se omitir este argumento, o diretório terá o mesmo nome do repositório.

## Fazer *fork* de um repositório no GitHub

1. Navegue até à página do repositório e faça *fork* do projeto para a sua própria conta (requer *sign in*).

2. Terá agora um repositório com *fork* que lhe pertence e no qual pode fazer *commit* de alterações.

3. Utilize as instruções da secção “Clonar a partir do repositório upstream”.

## Iniciar um novo repositório Git para uma base de código existente

Deve salientar-se que o seu projeto **deverá** incluir um ficheiro `README.md` em *markdown* com a descrição do repositório, caso pretenda torná-lo público.

```console
$> cd /path/to/my/codebase
$> git init      (1)
$> git add .     (2)
$> git commit -m "Initial commit"   (3)
```

1. Cria o diretório `/path/to/my/codebase/.git`.

2. Adiciona todos os ficheiros existentes ao índice.

3. Regista o estado inicial como o primeiro *commit* no histórico.

### Enviar para o GitHub

Primeiro, deve criar um repositório *git* em [https://github.com](https://github.com), de onde obterá o URL a utilizar abaixo:

```console
$> git remote add origin https://github.com/estsetubal-atad/CProgram_Template.git   (1)
$> git branch -M master        (2)
$> git push -u origin master   (3)
```

1. Define o URL do repositório *remote*.

2. Move o código para o ramo (*branch*) `master`.

3. Envia o código para o ramo *remote* `master`.

## Fazer *commit* e *push* num repositório existente

Pode fazer *commit* e *push* de alterações em qualquer repositório que lhe pertença utilizando:

```console
$> git add .    
$> git commit -m "Commit message" 
$> git push -u origin master 
```

Deve substituir "Commit message" por uma breve descrição do conteúdo do seu *commit*. Isto assume também que o ramo *remote* se chama `master`.

## Autor e suporte

Bruno Silva ([bruno.silva@estsetubal.ips.pt](mailto:bruno.silva@estsetubal.ips.pt))

Deverá contactar o seu docente de PL para qualquer apoio relativamente a estes conteúdos e procedimentos.
