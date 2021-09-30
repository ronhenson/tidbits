---
title: "Vscode_python_import_could_not_be_resolved"
created: 2021-07-19 10:22:25
tags: #ide, #vscode
keywords: vscode, python, pytest, import, pylance
---

# Vscode_python_import_could_not_be_resolved

Problem:  Pylance, "extension, `ms-python.python` Python Intellsense from Microsoft, could not find import of the module I was testing in pytest.  The test ran with no erros within vscode and from the terminal.  The file was in the same folder as the program it was testing.  It was one folder down from the workspace root folder.

## In `.vscode/settings/json`

```config
"python.autoComplete.extraPaths": ["./path-to-your-code"],
```

The above did not work.  The following did work:

```config
"python.analysis.extraPaths": ["./path-to-code/"],
```
