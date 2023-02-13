# Customizando o PowerShell

# Instalando as ferramentas

## Windows Terminal

O Windows Terminal é um aplicativo de terminal moderno, rápido, eficiente, poderoso e produtivo para usuários de ferramentas de linha de comando e shells como Prompt de Comando, PowerShell e WSL. Seus principais recursos incluem várias guias, painéis, suporte a caracteres Unicode e UTF-8, um mecanismo de renderização de texto acelerado por GPU e temas, estilos e configurações personalizados.

- [Install Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=en-us&gl=us)

## PowerShell (core)

O PowerShell é uma solução de automação de tarefas multiplataforma composta por um shell de linha de comando, uma linguagem de script e uma estrutura de gerenciamento de configuração. O PowerShell é executado no Windows, Linux e macOS.

- [Install PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7.3)

## Configurando Oh My Posh 

Um mecanismo de tema de prompt para qualquer shell.

[Oh  My Posh](https://ohmyposh.dev/) é um plug-in gratuito que permite criar um tema e um tema para qualquer shell. Ele tornará seu prompt mais útil, como adicionar informações do Git ao prompt.

Instalar o Oh-My-Posh é muito fácil, execute o comando abaixo em um terminal como administrador do Powershell:

```powershell
winget install JanDeDobbeleer.OhMyPosh
```

Para seguir com a configuração do Oh My Posh precisamos preparar algumas coisas:

1. Temas
2. Fontes
3. Iniciar o Oh My Posh no PowerShell

### 1. Temas

Oh My Posh já traz uma boa coleção de temas prontos para uso que podem ser encontrados aqui: https://ohmyposh.dev/docs/themes

Você também pode pré-visualizar os temas mais comuns no PowerShell usando este comando:

```powershell
Get-PoshThemes
```

Após selecionar um ou mais temas, é recomendado que baixe-os em um diretório local 
(são arquivos JSON), por exemplo em:

```
C:\tools\oh-my-posh\themes
```

### 2. Fontes

Oh My Posh foi projetado para usar [Nerd Fonts](https://www.nerdfonts.com/), que são fontes populares que são preparadas para incluir ícones. Oh My Posh recomenda a fonte *Meslo LGM NF*, mas qualquer Nerd Font deve ser compatível com os temas padrão.

Escolha suas fontes [aqui](https://www.nerdfonts.com/font-downloads) ou direto no [GitHub](https://github.com/ryanoasis/nerd-fonts/releases) (nesse caso lembre-se de realizar o download dos assets da release mais atual).

Após realizar o download, instale as fontes no Windows (*basicamente descompactar o arquivo, selecionar todas as fontes, botão direito e solicitar a instalação*).

**Windows Terminal**

Depois de instalar uma Nerd Font, você precisará configurar o Windows Terminal para usá-la. 

Isso pode ser feito facilmente modificando as configurações do Windows Terminal (atalho padrão: `CTRL + SHIFT + ,`). Em seu arquivo `settings.json`, adicione o atributo `font.face` no atributo defaults em profiles:

```json
{
    "profiles":
    {
        "defaults":
        {
            "font":
            {
                "face": "MesloLGM NF"
            }
        }
    }
}

```

**VS Code**

Ao usar o Visual Studio Code, você precisará configurar o Terminal integrado para fazer uso da Nerd Font também. 

Isso pode ser feito alterando o valor de `Integrated: Font Family` nas configurações do Terminal (atalho padrão: `CTRL + ,` e procure `Integrated: Font Family` ou via `Users -> Features -> Terminal`).

Se você estiver usando as configurações baseadas em JSON, precisará atualizar o valor `terminal.integrated.fontFamily`. Exemplo no caso de MesloLGM NF Nerd Font:

```json
    "terminal.integrated.fontFamily": "MesloLGM NF"
```

### 3. Iniciar o Oh My Posh no PowerShell

Registre o Oh My Posh para carregar sempre que você abrir um novo shell do Powershell. Para fazer isso, edite seu perfil do PowerShell usando este comando:

```powershell
notepad $PROFILE
```
*Observação: caso este arquivo não exista basta cria-lo em:*

```
C:\Users\<user>\Documents\PowerShell\Microsoft.PowerShell_profile.ps1
```

Para dizer ao Powershell para iniciar o Oh My Posh, adicione esta linha ao seu perfil:

*Lembrando que `C:/tools/oh-my-posh/themes` deve ser o diretório onde você salvou os temas no passo 1.*

```powershell
oh-my-posh --init --shell pwsh --config C:/tools/oh-my-posh/themes/1_shell.omp.json | Invoke-Expression
```

Ao fazer alterações neste arquivo, você precisará recarregar o terminal para que as alterações mais recentes sejam aplicadas. Você pode fazer isso recarregando o perfil usando este comando:

```powershell
 . $PROFILE
```

## Instalando Plugins Úteis

### Pré requisitos

Se você ainda não o possui, instale o [Git para Windows](https://git-scm.com/downloads).

Para evitar problemas com o PowerShell ao executar determinados scripts, executamos isso com permissões de administrador no console do PowerShell:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
```

### Plugins

**posh-git**

O [posh-git](http://dahlbyk.github.io/posh-git/) habilita no prompt os repositórios Git pode mostrar a branch atual e o estado dos arquivos (adições, modificações, exclusões).

```powershell
Install-Module posh-git -Scope CurrentUser
``` 

**PSReadLine**

O [PSReadLine](https://github.com/PowerShezll/PSReadLine) possui alguns recursos, o mais interessante é para autocompletar comandos.

```powershell
Install-Module -Name PSReadLine
```

**Terminal Icons**

Outra maneira de embelezar o terminal é instalar [Terminal-Icons](https://github.com/devblackops/Terminal-Icons). Este plug-in adicionará cores e ícones adicionais quando você criar arquivos e pastas em um diretório.

Você pode instalar o módulo usando este comando:

```powershell
Install-Module -Name Terminal-Icons -Repository PSGallery
```
### Meu arquivo $PROFILE

Meu arquivo de perfil final se parece com isso:

```powershell
oh-my-posh init pwsh --config 'C:/tools/oh-my-posh/themes/1_shell.omp.json' | Invoke-Expression

if ($host.Name -eq 'ConsoleHost' -or $host.Name -eq 'Visual Studio Code Host' ) {

    Import-Module PSReadline
    Set-PSReadLineOption -EditMode Windows
    Set-PSReadLineOption -PredictionSource History
  
    Set-PSReadlineOption -Color @{
      "Command"          = [ConsoleColor]::Green
      "Parameter"        = [ConsoleColor]::Gray
      "Operator"         = [ConsoleColor]::Magenta
      "Variable"         = [ConsoleColor]::Yellow
      "String"           = [ConsoleColor]::Yellow
      "Number"           = [ConsoleColor]::Yellow
      "Type"             = [ConsoleColor]::Cyan
      "Comment"          = [ConsoleColor]::DarkCyan
      "InlinePrediction" = '#70A99F'
    }
  
    Set-PSReadLineKeyHandler -Function AcceptSuggestion -Key 'Ctrl+Spacebar'
    Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward
    Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward 
  
    Set-PSReadLineKeyHandler -Key Ctrl+Shift+b `
                         -BriefDescription BuildCurrentDirectory `
                         -LongDescription "Build the current directory" `
                         -ScriptBlock {
      [Microsoft.PowerShell.PSConsoleReadLine]::RevertLine()
      [Microsoft.PowerShell.PSConsoleReadLine]::Insert("dotnet build")
      [Microsoft.PowerShell.PSConsoleReadLine]::AcceptLine()
    }
  }
  
  Import-Module -Name Terminal-Icons
```

## Referencias

- [Oh My Posh](https://ohmyposh.dev/)

- [Oh My Posh - Docs](https://ohmyposh.dev/docs)

- [Nerd Fonts](https://www.nerdfonts.com/)

- [Nerd Fonts - GitHub Releases](https://github.com/ryanoasis/nerd-fonts/releases)

- [Customize your PowerShell prompt like a boss](https://www.jondjones.com/tactics/productivity/customise-your-powershell-prompt-like-a-boss/)

- [How to customize PowerShell](https://medium.com/@ericksk/how-to-customize-powershell-7bc008947ce2)