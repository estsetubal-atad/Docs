# ATAD | Requisitos de Software

Esta unidade curricular requer a utilização da *toolchain* da **GNU Compiler Collection** (`gcc`) e de algumas utilidades adicionais.

A lista completa de software é a seguinte:

* GNU Compiler Collection (GCC)
* GNU Project Debugger (GDB)
* GNU Make
* Git (já instalado na distribuição WSL e em Linux)
* Valgrind
* Doxygen
* Visual Studio Code (IDE):

  * (Extensão) C/C++
  * (Extensão) Doxygen Documentation Generator
  * (Extensão) WSL -- *opcional, depende do ambiente*
  * (Extensão) Dev Containers -- *opcional, depende do ambiente*
  * (Extensão) MinGW C Configuration - *opcional, depende do ambiente*

Os ambientes de desenvolvimento preferenciais que suportam a lista completa de software são:

* Windows Subsystem for Linux (WSL) com Ubuntu LTS 20.04 ou posterior -- **apenas Windows**
* Docker *containers* -- **todos os sistemas operativos**.
* Linux.

> [!IMPORTANT]
> Deverá ter um destes ambientes configurados no seu computador pessoal para aceder a todas as ferramentas exigidas.

O ambiente de desenvolvimento disponível nos computadores da ESTSetúbal poderá não suportar o *Git*, *Valgrind* e *Doxygen*:

* Windows + MinGW (fornece uma adaptação de GCC, GDB e Make em Windows)

> [!WARNING]
> As avaliações PL serão elaboradas exclusivamente neste ambiente.

---

Existem duas formas principais de obter todo o software necessário, apresentadas nas duas secções seguintes:

1. Instalação manual – apenas em ambientes Windows/WSL e Linux; e

2. Utilização de um *docker container* pré-configurado com todo o software instalado – funciona em **qualquer sistema operativo** (sim, incluindo MacOS).

Depois, será necessário instalar o VS Code com algumas extensões iniciais, dependendo da opção anterior, conforme descrito na secção:

3. Instalar o VS Code com Extensões

E, por fim, garantir que temos o `git` instalado:

4. Instalar o git.

No final deste tutorial, prossiga para [Environment](Environment.md).

---

## 1 | Instalação manual (Windows/WSL ou Linux)

### Requisitos

Os requisitos gerais do sistema são os seguintes:

* Instalação atualizada do Windows 10/11 ou uma distribuição Linux recente (as distribuições Linux mais comuns disponibilizam os pacotes necessários);

* 8GB de RAM;

* 5GB de espaço em disco para Windows/WSL (será instalado um ambiente Linux base), ou 500MB adicionais numa máquina Linux.

### Instalação

> **Está numa máquina Linux?**
>
> **Avance para o passo (manual) `6` se estiver num ambiente Linux nativo**. Os comandos apresentados são para sistemas *Ubuntu/Debian*, mas deverá conseguir encontrar facilmente os pacotes equivalentes noutras distribuições e adaptar os comandos ao seu gestor de pacotes.

*Ligação:* [Install Linux on Windows with WSL | Microsoft Docs](https://learn.microsoft.com/en-us/windows/wsl/install)

**Pré-requisitos**:

* Certifique-se de que o sistema operativo está atualizado;

* Certifique-se de que a *Virtualization* está ativada na BIOS do seu computador (pesquise: "<marca do seu computador> enable virtualization bios", por exemplo, "thinkpad enable virtualization bios").

#### Procedimento rápido

Deverá conseguir instalar o WSL + Ubuntu 20.04 (ou posterior) com um único comando, conforme indicado na ligação acima. Abra o PowerShell ou a Linha de Comandos do Windows em modo administrador (clique com o botão direito e selecione "Executar como administrador") e introduza:

```console
PS> wsl --install
```

Reinicie o computador e avance para o **passo** `4`.

#### Procedimento alternativo (manual)

1. Abra o PowerShell como Administrador, execute o seguinte comando e reinicie.

```console
PS> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

2. Abra a **Microsoft Store** e procure por "Ubuntu". Instale **Ubuntu LTS 20.04 LTS**.

3. Execute a aplicação *Ubuntu* e aguarde pela instalação.

4. Durante a instalação será solicitado um **utilizador UNIX por omissão**. Introduza um (sem espaços e, preferencialmente, apenas com letras minúsculas) e a respetiva **palavra-passe** (não se esqueça desta palavra-passe!).

5. No final será apresentada a interface de linha de comandos (CLI).

---

#### Atualizar a imagem Ubuntu

6. Atualize os pacotes instalados com:

```console
$> sudo apt update && sudo apt upgrade
```

... este processo poderá demorar alguns minutos.

---

#### Instalar pacotes

7. Execute o seguinte comando para instalar os pacotes necessários:

```console
$> sudo apt install gcc gdb make valgrind doxygen
```

8. Confirme se o *git* já está instalado (se estiver, não é necessário reinstalar):

```console
$> sudo apt install git
$> git --version
```

9. Feche o terminal Ubuntu. Concluído.

---

## 2 | Docker Container (Todos os SO)

Este método disponibiliza uma solução de desenvolvimento em contentor Docker e, se tudo correr bem, permitirá um fluxo de trabalho completo com o VS Code e toda a *toolchain* (gcc, gdb, valgrind e doxygen).

### Requisitos

Os requisitos gerais do sistema são os seguintes:

* Instalação atualizada do Windows 10/11, MacOS ou Linux;

* 8GB de RAM;

* 3GB de espaço em disco.

### Instalação

> [!NOTE]
> Em Windows, será necessário ativar previamente o WSL2. Consulte o passo `1` em "Instalação manual (Windows/WSL ou Linux)".
1. Siga as instruções de instalação do **Docker Desktop** para o seu sistema operativo:

   * [https://docs.docker.com/desktop/install/windows-install/](https://docs.docker.com/desktop/install/windows-install/)

   * [https://docs.docker.com/desktop/install/mac-install/](https://docs.docker.com/desktop/install/mac-install/)

2. Abra um terminal (Powershell ou cmd) e faça o download da imagem `brunomnsilva/docker-atad`:

```console
$> docker pull brunomnsilva/docker-atad:latest
```

3. Concluído.

> [!IMPORTANT]
> A integração entre o VS Code e o *Docker* será realizada através de uma extensão específica (Dev Containers) e de uma pasta de configuração no projeto VS Code.

---

## 3 | Instalar o VS Code com Extensões

1. Faça download e instale a versão **System installer 64bit** a partir de
   [Download Visual Studio Code - Mac, Linux, Windows](https://code.visualstudio.com/download).

2. Execute o VS Code pela primeira vez.

3. Instale as seguintes extensões:
   
   * Se estiver a utilizar WSL, instale:

     > Name: **WSL**
     > ID: ms-vscode-remote.remote-wsl
     > Publisher: Microsoft

   * Se estiver a utilizar *Docker*, instale:

     > Name: **Dev Containers**
     > ID: ms-vscode-remote.remote-containers
     > Publisher: Microsoft

> [!NOTE]
> As restantes extensões serão instaladas "dentro" do ambiente de desenvolvimento escolhido, descritas em [Environment](Environment.md)

---

## 4 | Instalar o Git

Consulte: [https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

Por fim, prossiga para [Environment](Environment.md).

---

## Resolução de Problemas

### WSL

Na maioria dos casos, os problemas resultam de não ter a *Virtualization* ativada nas definições da BIOS.

Referências:

* [https://docs.microsoft.com/en-us/windows/wsl/troubleshooting](https://docs.microsoft.com/en-us/windows/wsl/troubleshooting)

### Docker: Elevado consumo de memória em Windows?

Se verificar um consumo elevado de RAM em Windows, tal deve-se ao comportamento por omissão da alocação de memória do backend WSL2/docker (por exemplo, pode utilizar 80% da RAM disponível, o que é excessivo para as nossas necessidades).

Nesse caso, crie um ficheiro `.wslconfig` no diretório do seu utilizador, por exemplo `C:\Users\Bruno Silva`, com o seguinte conteúdo:

```markdown
[wsl2]
memory=2GB
```

Ajuste o valor máximo de memória de acordo com os recursos da sua máquina.

---

### Nenhum dos métodos funciona?

Consulte [Software Alternative](SoftwareAlternatives.md) para métodos alternativos (suporte reduzido).

---

## Autor e apoio

Bruno Silva ([bruno.silva@estsetubal.ips.pt](mailto:bruno.silva@estsetubal.ips.pt))

Para qualquer dúvida relacionada com estes conteúdos e procedimentos, deverá contactar o seu docente de PL.
