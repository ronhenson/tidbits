---
title: "Vscode Insiders Settings Json File"
created: 2022-01-24 10:59:51
tags: tags
keywords: template
---

# Vscode Insiders Settings Json File

```json
{
    "editor.suggestSelection": "first",
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "editor.fontFamily": "'Fira Code', 'Source Code Pro', 'DejaVu Sans Mono'",
    "editor.fontLigatures": true,
    "editor.tabSize": 2,
    "editor.codeLensFontFamily": "'Fira Code','Source Code Pro'",
    "editor.inlineHints.fontFamily": "\"Fira Code Retina\"",
    "editor.tabCompletion": "on",
    "editor.wrappingIndent": "indent",
    "editor.insertSpaces": true,
    "todo-tree.tree.showScanModeButton": false,
    "workbench.tree.indent": 20,
    "spring.initializr.defaultJavaVersion": "11",
    "spring.initializr.defaultLanguage": "Java",
    "spring.initializr.defaultPackaging": "JAR",
    "java.configuration.checkProjectSettingsExclusions": false,
    "java.jdt.ls.vmargs": "-XX:+UseParallelGC -XX:GCTimeRatio=4 -XX:AdaptiveSizePolicyWeight=90 -Dsun.zip.disableMemoryMapping=true -Xmx1G -Xms100m -javaagent:\"/home/ronh/.vscode-insiders/extensions/gabrielbb.vscode-lombok-1.0.1/server/lombok.jar\"",
    "java.project.importOnFirstTimeStartup": "automatic",
    "explorer.compactFolders": false,
    "java.refactor.renameFromFileExplorer": "autoApply",
    "diffEditor.ignoreTrimWhitespace": false,
    "editor.minimap.enabled": false,
    "files.trimFinalNewlines": true,
    "files.insertFinalNewline": true,
    "terminal.integrated.cwd": "${workspaceFolder}",
    "terminal.integrated.enableFileLinks": false,
    "json.maxItemsComputed": 45000,
    "angular.experimental-ivy": true,
    "terminal.external.windowsExec": "C:\\Windows\\System32\\bash.exe",
    "debug.console.fontFamily": "'Fira Code', 'Source Code Pro','",
    "terminal.integrated.automationShell.windows": "C:\\Windows\\System32\\bash.exe",
    "terminal.integrated.automationShell.linux": "/usr/bin/bash",
    "javascript.format.semicolons": "insert",
    "[markdown]": {
        "breadcrumbs.showProperties": true,
        "files.trimTrailingWhitespace": true,
        "editor.quickSuggestions": true
    },
    "yaml.customTags": [
        "!And",
        "!And sequence",
        "!If",
        "!If sequence",
        "!Not",
        "!Not sequence",
        "!Equals",
        "!Equals sequence",
        "!Or",
        "!Or sequence",
        "!FindInMap",
        "!FindInMap sequence",
        "!Base64",
        "!Join",
        "!Join sequence",
        "!Cidr",
        "!Ref",
        "!Sub",
        "!Sub sequence",
        "!GetAtt",
        "!GetAZs",
        "!ImportValue",
        "!ImportValue sequence",
        "!Select",
        "!Select sequence",
        "!Split",
        "!Split sequence"
    ],
    "yaml.validate": false,
    "diffEditor.renderSideBySide": false,
    "terminal.integrated.scrollback": 10000,
    "workbench.editorAssociations": {
        "*.ipynb": "jupyter-notebook"
    },
    "window.title": "${dirty}${activeFolderLong}${separator}${rootName}",
    "terminal.integrated.allowChords": true,
    "code-runner.runInTerminal": true,
    "code-runner.terminalRoot": "/mnt/",
    "workbench.editor.enablePreviewFromCodeNavigation": true,
    "workbench.editor.highlightModifiedTabs": true,
    "openInGitHub.defaultBranch": "main",
    "python.insidersChannel": "weekly",
    "notebook.kernelProviderAssociations": [
        {
            "viewType": "jupyter-notebook",
            "kernelProvider": "ms-toolsai.jupyter"
        }
    ],
    "python.pipenvPath": "/usr/bin/pipenv",
    "python.languageServer": "Pylance",
    "joplin.programProfilePath": "~/.config/joplin-desktop",
    "joplin.token": "9c3c222ba3e0d69b4243f93919af3ae0ad9b534dd362b9e66ca09bd31b66dfef59fcbf1a6732a1f5c156f396ef24bcb0136c9fe4e590f9bd71c632b96fb2cd34",
    "remote.containers.dockerPath": "podman",
    "redhat.telemetry.enabled": true,
    "window.titleBarStyle": "custom",
    "markdown-preview-enhanced.breakOnSingleNewLine": false,
    "markdown.extension.tableFormatter.normalizeIndentation": true,
    "markdown-preview-enhanced.previewTheme": "vue.css",
    "foam.edit.linkReferenceDefinitions": "withExtensions",
    "markdownExtended.pdfFormat": "Letter",
    "markdown-wiki-links-preview.descriptionthenfile": true,
    "markdown-wiki-links-preview.previewlabelstyling": "label",
    "markdown.preview.linkify": false,
    "vscodeMarkdownNotes.noteCompletionConvention": "noExtension",
    "vscodeMarkdownNotes.slugifyMethod": "github-slugger",
    "workbench.iconTheme": "vscode-icons",
    "editor.inlayHints.fontFamily": "'Fira Code', 'Source Code Pro', 'Droid Sans Mono', 'Droid Sans Fallback'",
    "terminal.integrated.fontFamily": "'Fira Code', 'Source Code Pro'",
    "terminal.integrated.localEchoExcludePrograms": [
        "vim",
        "vi",
        "nano",
        "tmux",
        "nvim"
    ],
    "terminal.integrated.profiles.linux": {
        "bash": {
            "path": "bash"
        },
        "zsh": {
            "path": "zsh"
        },
        "fish": {
            "path": "fish"
        },
        "tmux": {
            "path": "tmux",
            "icon": "terminal-tmux"
        },
        "pwsh": {
            "path": "pwsh",
            "icon": "terminal-powershell"
        }
    },
    "editor.scrollBeyondLastLine": false,
    "editor.autoIndent": "advanced",
    "editor.detectIndentation": false,
    "foam.decorations.links.enable": true,
    "vscodeMarkdownNotes.previewLabelStyling": "label",
    "vim.enableNeovim": true,
    "markdown.links.openLocation": "beside",
    "highlight.regexes": {
      "((?:<!-- *)?(?:#|// @|//|./\\*+|<!--|--|\\* @|{!|{{!--|{{!) *TODO(?:\\s*\\([^)]+\\))?:?)((?!\\w)(?: *-->| *\\*/| *!}| *--}}| *}}|(?= *(?:[^:]//|/\\*+|<!--|@|--|{!|{{!--|{{!))|(?: +[^\\n@]*?)(?= *(?:[^:]//|/\\*+|<!--|@|--(?!>)|{!|{{!--|{{!))|(?: +[^@\\n]+)?))": {
        "filterFileRegex": ".*(?<!CHANGELOG.md)$",
        "decorations": [
          {
            "overviewRulerColor": "#ffcc00",
            "backgroundColor": "#ffcc00",
            "color": "#1f1f1f",
            "fontWeight": "bold"
          },
          {
            "backgroundColor": "#ffcc00",
            "color": "#1f1f1f"
          }
        ]
      },
      "((?:<!-- *)?(?:#|// @|//|./\\*+|<!--|--|\\* @|{!|{{!--|{{!) *(?:FIXME|FIX|BUG|UGLY|DEBUG|HACK)(?:\\s*\\([^)]+\\))?:?)((?!\\w)(?: *-->| *\\*/| *!}| *--}}| *}}|(?= *(?:[^:]//|/\\*+|<!--|@|--|{!|{{!--|{{!))|(?: +[^\\n@]*?)(?= *(?:[^:]//|/\\*+|<!--|@|--(?!>)|{!|{{!--|{{!))|(?: +[^@\\n]+)?))": {
        "filterFileRegex": ".*(?<!CHANGELOG.md)$",
        "decorations": [
          {
            "overviewRulerColor": "#cc0000",
            "backgroundColor": "#cc0000",
            "color": "#1f1f1f",
            "fontWeight": "bold"
          },
          {
            "backgroundColor": "#cc0000",
            "color": "#1f1f1f"
          }
        ]
      },
      "((?:<!-- *)?(?:#|// @|//|./\\*+|<!--|--|\\* @|{!|{{!--|{{!) *(?:REVIEW|OPTIMIZE|TSC)(?:\\s*\\([^)]+\\))?:?)((?!\\w)(?: *-->| *\\*/| *!}| *--}}| *}}|(?= *(?:[^:]//|/\\*+|<!--|@|--|{!|{{!--|{{!))|(?: +[^\\n@]*?)(?= *(?:[^:]//|/\\*+|<!--|@|--(?!>)|{!|{{!--|{{!))|(?: +[^@\\n]+)?))": {
        "filterFileRegex": ".*(?<!CHANGELOG.md)$",
        "decorations": [
          {
            "overviewRulerColor": "#00ccff",
            "backgroundColor": "#00ccff",
            "color": "#1f1f1f",
            "fontWeight": "bold"
          },
          {
            "backgroundColor": "#00ccff",
            "color": "#1f1f1f"
          }
        ]
      },
      "((?:<!-- *)?(?:#|// @|//|./\\*+|<!--|--|\\* @|{!|{{!--|{{!) *(?:IDEA)(?:\\s*\\([^)]+\\))?:?)((?!\\w)(?: *-->| *\\*/| *!}| *--}}| *}}|(?= *(?:[^:]//|/\\*+|<!--|@|--|{!|{{!--|{{!))|(?: +[^\\n@]*?)(?= *(?:[^:]//|/\\*+|<!--|@|--(?!>)|{!|{{!--|{{!))|(?: +[^@\\n]+)?))": {
        "filterFileRegex": ".*(?<!CHANGELOG.md)$",
        "decorations": [
          {
            "overviewRulerColor": "#cc00cc",
            "backgroundColor": "#cc00cc",
            "color": "#1f1f1f",
            "fontWeight": "bold"
          },
          {
            "backgroundColor": "#cc00cc",
            "color": "#1f1f1f"
          }
        ]
      }
    },
    "markdown.extension.syntax.plainTheme": true,
    "vim.sneak": true,
    "vim.sneakUseIgnorecaseAndSmartcase": true,
    "vim.useSystemClipboard": true,
    "vim.useCtrlKeys": true,
    "vim.incsearch": true,
    "vim.easymotion": true,
    "vim.hlsearch": true,
    "zenMode.hideLineNumbers": false,
    "vim.highlightedyank.enable": true,



    "vim.highlightedyank.textColor": "#9AFE2E",
    "vim.searchHighlightColor": "#F4FA58",
    "vim.searchHighlightTextColor": "#088A4B",
    "terminal.integrated.lineHeight": 0,
    "vscode_custom_css.imports": [
      "file:///home/ronh/.ronhConfig/code-insiders/custom.css"
    ],
    "markdown.preview.fontSize": 15,
    "terminal.integrated.fontSize": 16,
    "workbench.colorCustomizations": {
        "[Night Owl]": {
            "statusBar.background": "#000000",
            "statusBar.foreground": "#f1e31d",
            "statusBarItem.hoverBackground":"#ff0000",
            "tab.activeBackground": "#02455D",
            "tab.inactiveForeground": "#6FB7D2",
            "activityBar.activeBackground": "#02455D",
            "editorCursor.foreground": "#f7d694",
            "terminalCursor.foreground": "#f7d694"
        },
        "window.activeBorder": "#aaebf3"
    },
    "editor.inlineSuggest.enabled": true,
    "workbench.startupEditor": "none",
    "vim.neovimPath": "/usr/bin/nvim",
    "terminal.external.linuxExec": "/usr/bin/kitty",
    "notebook.cellToolbarLocation": {
      "default": "right",
      "jupyter-notebook": "left"
    },
    "terminal.integrated.defaultProfile.linux": "bash",
    "editor.cursorBlinking": "smooth",
    "editor.formatOnPaste": true,
    "editor.acceptSuggestionOnEnter": "off",
    "vscode-neovim.mouseSelectionStartVisualMode": true,
    "vscode-neovim.neovimInitVimPaths.linux": "/home/ronh/.config/nvim/init.vim",
    "vscode-neovim.textDecorationsAtTop": true,
    "vscode-neovim.highlightGroups.highlights": {


      "Directory": "vim",
      "IncSearch": {
        "backgroundColor": "theme.editor.findMatchBackground",
        "borderColor": "theme.editor.findMatchBorder"
      },
      "Search": {
        "backgroundColor": "theme.editor.findMatchHighlightBackground",
        "borderColor": "theme.editor.findMatchHighlightBorder"
      },
      "Visual": {
        "backgroundColor": "theme.editor.selectionBackground"
      },
      "Conceal": "vim",
      "Substitute": "vim"
    },
    "vscode-neovim.neovimExecutablePaths.linux": "/usr/bin/nvim",
    "screencastMode.fontSize": 51,
    "todo-tree.general.tags": [
      "BUG",
      "HACK",
      "FIXME",
      "TODO",
      "XXX",
      "[ ]",
      "[x]"
    ],
    "todo-tree.regex.regex": "(//|#|<!--|;|/\\*|^|^\\s*(-|\\d+.))\\s*($TAGS)",
    "todo-tree.ripgrep.ripgrep": "/usr/bini/rg",
    "todo-tree.highlights.useColourScheme": true,
    "python.envFile": "${workspaceFolder}/dev.env",
    "python.terminal.activateEnvInCurrentTerminal": true,
    "terminal.integrated.tabs.showActiveTerminal": "always",
    "files.autoSaveDelay": 1800000,
    "security.workspace.trust.untrustedFiles": "open",
    "vscode-neovim.neovimExecutablePaths.win32": "C:\\tools\\Neovim\\bin\\nvim.exe",
    "vscode-neovim.neovimInitVimPaths.win32": "C:/Users/ronh/AppData/Local/nvim/init.vim",
    "scm.inputFontSize": 15,
    "markdown.preview.fontFamily": "'Merriweather', 'Droid Sans', sans-serif",
    "editor.fontSize": 16,
    "files.trimTrailingWhitespace": true,
    "git.autofetch": true,
    "editor.stickyTabStops": true,
    "window.zoomLevel": 1,
    "workbench.colorTheme": "GitHub Dark Default"
}
```
