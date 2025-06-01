Tags:
- [[Productivity]]
---
## Extensions
- Vim
- VSCode Harpoon
- Catppuccin
- Remote SSH

## User settings JSON
```json
{
    "workbench.colorTheme": "Catppuccin Mocha",
    "files.autoSave": "afterDelay",
    "editor.lineNumbers": "relative", 
    "editor.minimap.enabled": false,
    "terminal.integrated.defaultProfile.windows": "Git Bash", // zsh better
    "vim.handleKeys": {
        "<C-t>": false,
        "<C-p>": false,
        "<C-k>": false,
    },
    "workbench.editor.limit.enabled": true,
    "workbench.editor.limit.value": 1
}
```

## Keybindings JSON
```json
[
    {
        "key": "ctrl+t",
        "command": "workbench.action.toggleSidebarVisibility"
    },
    {
        "key": "alt+a",
        "commands": ["vscode-harpoon.addEditor"]
    },
    {
        "key": "alt+e",
        "commands": ["vscode-harpoon.editEditors"]
    },
    {
        "key": "alt+p",
        "commands": ["vscode-harpoon.editorQuickPick"]
    },
    {
        "key": "alt+1",
        "command": "vscode-harpoon.gotoEditor1"
    }
    {
        "key": "alt+2",
        "command": "vscode-harpoon.gotoEditor2"
    }
    {
        "key": "alt+3",
        "command": "vscode-harpoon.gotoEditor3"
    }
    {
        "key": "alt+4",
        "command": "vscode-harpoon.gotoEditor4"
    }
]
```

---
## References
- 