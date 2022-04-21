---
title: "Vscode Keyboard Shortcuts Customized"
created: 2021-08-12 12:12:17
tags: ide, vscode
keywords: vscode, keyboard shortcuts, foam, markdown
---

# Vscode Keyboard Shortcuts Customized

## `keybindings.json`

### Create new markdown using selected template `.foam/templates/`

```json
  {
    "key": "ctrl+shift+f9",
    "command": "foam-vscode.create-note-from-template"
  },
```

### Create new markdown using the default templates `.foam/templates/new-note.md`

```json
  {
    "key": "ctrl+shift+f11",
    "command": "foam-vscode.create-note-from-default-template"
  },
```

### Jump to next group open editor

```json
  {
    "key": "ctrl+shift+f8",
    "command": "workbench.action.navigateEditorGroups"
  }
```
