---
title: "Toolbox Vscode Setup"
created: 2022-01-19 11:56:15
tags: container
keywords: containers, toolbox, vscode, code, code-insiders
---

# Toolbox Vscode Setup

links
: [owtaylor/toolbox-vscode git repository](https://github.com/owtaylor/toolbox-vscode)

## Toolbox Visual Studio Code integration

This repository is intended for scripts and hooks to integrate Toolbox with Visual Studio Code.

In particular, it provides a `code.sh` script that:

- If necessary, prompts to install the Flatpak of Visual Studio Code
- If necessary, configures the current toolbox container to work with the Remote Containers Visual Studio Code extension.
- Opens a VSCode window using the remaining command line arguments

### Installation

```bash
git clone https://github.com/owtaylor/toolbox-vscode.git
cd toolbox-vscode
ln -s "$PWD/code.sh" ~/.local/bin/code
```

### Usage

```bash
toolbox enter
cd ~/Source/myproject
code .
```

To recreate the container configuration (perhaps after updating this repository)

code --toolbox-reset-configuration .

### Credits

The configuration that `code.sh` sets up was largely figured out by Laércio de Sousa [lbssousa](https://github.com/lbssousa)
