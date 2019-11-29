# Microsoft Visual Studio Code Settings

## settings.json

```json
{
    "editor.codeActionsOnSave": {
        "source.organizeImports": true
    },
    "editor.codeActionsOnSaveTimeout": 20000,
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.fontSize": 16,
    "editor.renderWhitespace": "boundary",
    "editor.suggestSelection": "first",
    "eslint.autoFixOnSave": true,
    "eslint.packageManager": "yarn",
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        { "autoFix": true, "language": "typescript" },
        { "autoFix": true, "language": "typescriptreact" }
    ],
    "files.autoSave": "onFocusChange",
    "gitlens.codeLens.enabled": false,
    "javascript.updateImportsOnFileMove.enabled": "always",
    "material-icon-theme.activeIconPack": "react_redux",
    "npm.packageManager": "yarn",
    "telemetry.enableTelemetry": false,
    "typescript.locale": "en",
    "typescript.updateImportsOnFileMove.enabled": "always",
    "workbench.colorTheme": "Material Theme Darker High Contrast",
    "workbench.iconTheme": "material-icon-theme",
    "workbench.tree.indent": 16,
}
```
