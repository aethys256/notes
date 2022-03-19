# Setup: Microsoft Visual Studio Code

## Extensions

Any extension prefixed by \[WSL] is meant to be installed on WSL rather than locally if you are using it.\
This does not matter for macOS / Linux.

### UI

- \[WSL] [Copy Relative Path and Line Numbers (ezforo.copy-relative-path-and-line-numbers)](https://marketplace.visualstudio.com/items?itemName=ezforo.copy-relative-path-and-line-numbers)
- \[WSL] [EditorConfig for VS Code (editorconfig.editorconfig)](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)
- \[WSL] [GitLens — Git supercharged (eamodio.gitlens)](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- \[WSL] [Live Share (ms-vsliveshare.vsliveshare)](https://marketplace.visualstudio.com/items?itemName=ms-vsliveshare.vsliveshare)
- [Material Icon Theme (pkief.material-icon-theme)](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)
- [Material Theme (equinusocio.vsc-material-theme)](https://marketplace.visualstudio.com/items?itemName=Equinusocio.vsc-material-theme) (Uninstall [Community Material Theme (equinusocio.vsc-community-material-theme)](https://marketplace.visualstudio.com/items?itemName=Equinusocio.vsc-community-material-theme) & [Material Theme Icons (equinusocio.vsc-material-theme-icons)](https://marketplace.visualstudio.com/items?itemName=Equinusocio.vsc-material-theme-icons) that are installed as Pack)
- [Remote Development (ms-vscode-remote.vscode-remote-extensionpack)](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)
- \[WSL] [Terminals Manager (fabiospampinato.vscode-terminals)](https://marketplace.visualstudio.com/items?itemName=fabiospampinato.vscode-terminals)

### C/C++

- \[WSL] [C/C++ (ms-vscode.cpptools)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

### C Sharp

- \[WSL] [C# (ms-vscode.csharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

### GraphQL

- \[WSL] [GraphQL (graphql.vscode-graphql)](https://marketplace.visualstudio.com/items?itemName=GraphQL.vscode-graphql)

### JavaScript / TypeScript

- \[WSL] [ESLint (dbaeumer.vscode-eslint)](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- \[WSL] [Prettier - Code formatter (esbenp.prettier-vscode)](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- \[WSL] [vscode-styled-components (jpoissonnier.vscode-styled-components)](https://marketplace.visualstudio.com/items?itemName=jpoissonnier.vscode-styled-components)
- \[WSL] [Debugger for Chrome (msjsdiag.debugger-for-chrome)](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)

### Lua

- \[WSL] [Lua (sumneko.lua)](https://marketplace.visualstudio.com/items?itemName=sumneko.lua)
- [WoW Bundle (septh.wow-bundle)](https://marketplace.visualstudio.com/items?itemName=Septh.wow-bundle)

### Python

- \[WSL] [Python (ms-python.python)](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
- \[WSL] [Pylance (ms-python.vscode-pylance)](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance)

### Ruby

- \[WSL] [Ruby (rebornix.ruby)](https://marketplace.visualstudio.com/items?itemName=rebornix.Ruby) (will also install [VSCode Ruby (wingrunr21.vscode-ruby)](https://marketplace.visualstudio.com/items?itemName=wingrunr21.vscode-ruby))

### Rust

- \[WSL] [Rust (rls) (rust-lang.rust)](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust)

### Misc

- \[WSL] [AutoIt (damien.autoit)](https://marketplace.visualstudio.com/items?itemName=Damien.autoit)
- [DotENV (mikestead.dotenv)](https://marketplace.visualstudio.com/items?itemName=mikestead.dotenv)
- \[WSL] [gettext (mrorz.language-gettext)](https://marketplace.visualstudio.com/items?itemName=mrorz.language-gettext)
- [Log File Highlighter (emilast.logfilehighlighter)](https://marketplace.visualstudio.com/items?itemName=emilast.LogFileHighlighter)
- \[WSL] [markdownlint (davidanson.vscode-markdownlint)](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)
- [SimulationCraft (mystler.simulationcraft)](https://marketplace.visualstudio.com/items?itemName=Mystler.simulationcraft)
- \[WSL] [Visual Studio IntelliCode (visualstudioexptteam.vscodeintellicode)](https://marketplace.visualstudio.com/items?itemName=VisualStudioExptTeam.vscodeintellicode)
- \[WSL] [YAML (redhat.vscode-yaml)](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)

### DevOps

- \[WSL] [Azure Pipelines (ms-azure-devops.azure-pipelines)](https://marketplace.visualstudio.com/items?itemName=ms-azure-devops.azure-pipelines) (will also install [Azure Account (ms-vscode.azure-account)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account))
- [Docker (ms-azuretools.vscode-docker)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)
- [PowerShell (ms-vscode.powershell)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)

## settings.json

```json
{
    "diffEditor.renderSideBySide": true,
    "editor.fontSize": 14,
    "editor.renderWhitespace": "boundary",
    "editor.semanticHighlighting.enabled": true,
    "editor.suggestSelection": "first",
    "editor.unicodeHighlight.nonBasicASCII": false,
    "eslint.packageManager": "yarn",
    "explorer.confirmDragAndDrop": false,
    "files.autoSave": "onFocusChange",
    "git.allowForcePush": true,
    "git.allowNoVerifyCommit": true,
    "git.autofetch": true,
    "git.branchSortOrder": "alphabetically",
    "git.confirmForcePush": false,
    "git.confirmNoVerifyCommit": false,
    "git.confirmSync": false,
    "git.fetchOnPull": true,
    "git.rebaseWhenSync": true,
    "git.showPushSuccessNotification": true,
    "git.suggestSmartCommit": false,
    "gitlens.codeLens.enabled": false,
    "gitlens.showWelcomeOnInstall": false,
    "gitlens.showWhatsNewAfterUpgrades": false,
    "javascript.suggest.completeFunctionCalls": true,
    "javascript.updateImportsOnFileMove.enabled": "always",
    "Lua.runtime.version": "Lua 5.1",
    "Lua.telemetry.enable": false,
    "Lua.workspace.maxPreload": 1000000,
    "Lua.workspace.preloadFileSize": 10000,
    "material-icon-theme.activeIconPack": "react_redux",
    "npm.packageManager": "yarn",
    "python.languageServer": "Pylance",
    "redhat.telemetry.enabled": false,
    "security.workspace.trust.untrustedFiles": "open",
    "telemetry.telemetryLevel": "off",
    "typescript.locale": "en",
    "typescript.suggest.completeFunctionCalls": true,
    "typescript.tsserver.maxTsServerMemory": 6144,
    "typescript.updateImportsOnFileMove.enabled": "always",
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "workbench.colorTheme": "Material Theme Darker High Contrast",
    "workbench.iconTheme": "material-icon-theme",
    "workbench.tree.indent": 14,
    "[html]": {
        "editor.codeActionsOnSave": {
            "source.fixAll.eslint": true
        },
        "editor.defaultFormatter": "esbenp.prettier-vscode",
    },
    "[javascript]": {
        "editor.codeActionsOnSave": {
            "source.fixAll.eslint": true
        },
        "editor.defaultFormatter": "esbenp.prettier-vscode",
    },
    "[javascriptreact]": {
        "editor.codeActionsOnSave": {
            "source.fixAll.eslint": true
        },
        "editor.defaultFormatter": "esbenp.prettier-vscode",
    },
    "[json]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[markdown]": {
        "editor.codeActionsOnSave": {
            "source.fixAll.eslint": true
        },
        "editor.defaultFormatter": "esbenp.prettier-vscode",
    },
    "[typescript]": {
        "editor.codeActionsOnSave": {
            "source.fixAll.eslint": true
        },
        "editor.defaultFormatter": "esbenp.prettier-vscode",
    },
    "[typescriptreact]": {
        "editor.codeActionsOnSave": {
            "source.fixAll.eslint": true
        },
        "editor.defaultFormatter": "esbenp.prettier-vscode",
    },
    "[vue]": {
        "editor.codeActionsOnSave": {
            "source.fixAll.eslint": true
        },
        "editor.defaultFormatter": "esbenp.prettier-vscode",
    },
}
```
