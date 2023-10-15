# Setup: VSCode

## Install VSCode

- If you haven’t already, download the VSCode .zip file: https://code.visualstudio.com/docs?dv=osx
- Click on the .zip file to unzip the application.
- Drag the Visual Studio Code.app to the Applications folder.

## Add the ability to open VSCode from the terminal

- In the VSCode menu, click on `View -> Command Palette…`
- In the prompt, search for `Shell Command: Install 'code' command in PATH` and click or press enter to run the command.

  _In your terminal you can now open VSCode using the command `code .`_

## Configure your VSCode preferences

- In the VSCode menu, click on `View -> Command Palette...`
- In the prompt, search for `Preferences: Open User Settings (JSON)` and click or press enter.
- In the `settings.json` file that opens, replace the text with the following:
  ```
  {
    "editor.tabSize": 2,
    "files.autoSave": "onFocusChange",
    "editor.formatOnSave": true,
    "editor.wordWrap": "on",
    "editor.parameterHints.enabled": false,
    "explorer.openEditors.visible": 0,
    "rubocop.mode": "onlyRunGlobally",
    "[ruby]": {
      "editor.defaultFormatter": "jnbt.vscode-rufo"
    },
    "[vue]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascriptreact]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    }
  }
  ```
- Save and close the `settings.json` file.

## Useful VSCode keyboard shortcuts

### Level 1

| Shortcut        | Description                                                              |
| --------------- | ------------------------------------------------------------------------ |
| command c       | Copy the highlighted text (copies the whole line if nothing highlighted) |
| command x       | Cut the highlighted text (cuts the whole line if nothing highlighted)    |
| command v       | Paste the copied text                                                    |
| shift command v | Paste the copied text without formatting                                 |
| command z       | Undo                                                                     |
| command n       | Make a new tab                                                           |
| command w       | Close the current tab                                                    |
| command f       | Find text in the current file                                            |
| shift command f | Find text in any available file                                          |

### Level 2

| Shortcut            | Description                                                        |
| ------------------- | ------------------------------------------------------------------ |
| command d           | Select word (repeat select others occurrences)                     |
| command /           | Comment or uncomment the selected lines                            |
| command ]           | Indent the selected lines                                          |
| command [           | Un-indent the selected lines                                       |
| command enter       | Start a new line below (regardless of the current cursor position) |
| shift command enter | Start a new line above (regardless of the current cursor position) |
| command p           | Search and open any available file                                 |
| shift command p     | Search and run any available command in VSCode                     |
| command b           | Toggle sidebar                                                     |

### Level 3

| Shortcut                      | Description                                                                |
| ----------------------------- | -------------------------------------------------------------------------- |
| command d command u           | Undo a highlight (if you command d too far by accident)                    |
| command d command k command d | Skip a highlight (if you don’t want a specific instance to be highlighted) |
| option ↑                      | Move the current line or selected lines up                                 |
| option ↓                      | Move the current line or selected lines up                                 |
| shift command ]               | Switch to next tab                                                         |
| shift command [               | Switch to previous tab                                                     |
| command \                     | Split window into columns                                                  |
| command k command \           | Split window into rows                                                     |
| command k z                   | Toggle zen mode                                                            |
