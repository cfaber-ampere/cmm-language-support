# cmm-language-support

Extension for syntax highlighting for cmm files.

## Installation

This extension is not published on the Marketplace. Install it locally by first cloning it into your VS Code extensions directory, then installing from that location.

### Step 1: Clone into your VS Code extensions folder

Stable VS Code extension folders:
- Windows: %USERPROFILE%\.vscode\extensions
- macOS: ~/.vscode/extensions
- Linux: ~/.vscode/extensions

VS Code Insiders:
- Windows: %USERPROFILE%\.vscode-insiders\extensions
- macOS/Linux: ~/.vscode-insiders/extensions

Commands:
- Windows (PowerShell):
  - cd $env:USERPROFILE\.vscode\extensions
  - git clone git@github.com:cfaber-ampere/cmm-language-support.git faberc.cmm-language-support
- macOS/Linux:
  - cd ~/.vscode/extensions
  - git clone git@github.com:cfaber-ampere/cmm-language-support.git faberc.cmm-language-support

### Step 2: Install from Location (Command Palette)

- Open VS Code.
- Open the Command Palette:
  - Windows/Linux: Ctrl+Shift+P
  - macOS: Cmd+Shift+P
- Run: “Extensions: Install from Location…”
- When prompted, select the folder you just cloned (e.g., ~/.vscode/extensions/faberc.cmm-language-support).
- Reload VS Code if prompted.

Notes:
- If you don’t see “Install from Location…”, type “install from” in the Command Palette to find the exact command in your VS Code version.

## Features

- Syntax highlighting for .cmm files.