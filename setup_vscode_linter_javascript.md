# Setup: VSCode linter for JavaScript

## Step 1: Install eslint from the Terminal

- In your terminal, enter `npm -v` to make sure you have npm installed.

  - If your terminal gives you a number, that means you have npm installed!
  - If your terminal says `bash: npm: command not found`, you must first install node (which includes npm) using homebrew with the command:

    ```
    brew install node
    ```

  - Once you install node, you should be able to enter `npm -v` and see a version number.

- In your terminal, enter `npm install -g eslint @babel/core @babel/eslint-parser @babel/preset-react`
  - _If you have permission errors, you can run the command again with `sudo` in front_
- Check if it works by entering `eslint -v`. If you see a version number, you’re done!

## Step 2: Create an .eslintrc file using VSCode

- In your terminal, run the command: `code ~/.eslintrc`
  - _(This will create a new file named `.eslintrc` in your home directory)_
- Copy the following code into the empty file: 
  ```json
  {
      "rules": {
          "indent": [1, 2],
          "linebreak-style": [1, "unix"],
          "semi": [1, "always"],
          "eqeqeq": [1, "smart"],
          "camelcase": [1, { "properties": "never" }],
          "curly": 1,
          "brace-style": [1, "1tbs"],
          "array-bracket-spacing": [1, "never"],
          "space-before-blocks": [1, "always"],
          "keyword-spacing": ["error", {"before": true, "after": true, "overrides": {}}],
          "no-spaced-func": 1,
          "space-infix-ops": [1, {"int32Hint": false}],
          "no-console": 0,
          "no-unused-vars": 0,
          "vue/no-use-v-if-with-v-for": 0,
          "vue/require-v-for-key": 0
      },
      "env": {
          "es6": true,
          "node": true,
          "browser": true
      },
      "extends": ["eslint:recommended"],
      "plugins": [],
      "parser": "/opt/homebrew/lib/node_modules/@babel/eslint-parser",
      "parserOptions": {
          "sourceType": "module",
          "allowImportExportEverywhere": true,
          "requireConfigFile": false,
          "babelOptions": {
            "presets": ["/opt/homebrew/lib/node_modules/@babel/preset-react"]
          }
      }
  }
  ```
  - Note:
    - _If you have a older Mac, your global `node_modules` may be in a different directory (`/usr/local/lib/node_modules` instead of `/opt/homebrew/lib/node_modules`)._
    - _If you get a "Failed to load parser" error in the next step, change the `/opt/homebrew` paths to `/usr/local` in the "parser" and "presets" options._

- Save the file.

- Now you can use eslint in the terminal! If you are in the terminal in a folder with a JavaScript file in it (for example `test.js`), you can use eslint like so: `eslint test.js`

## Step 3: Install the ESLint VSCode extension

- In the VSCode menu, click on **View -> Extensions**.
- In the search bar, type **eslint** to find the relevant extensions
- Select the **ESLint** extension (by Microsoft), then click the green Install button.

## Bonus: Auto-format your JavaScript every time you save with the prettier tool

- In your terminal, enter `npm install -g prettier`
- In your terminal, run the command: `code ~/.prettierrc`
  - _(This will create a new file named `.prettierrc` in your home directory)_
- Copy the below code into the `.prettierrc` file, then save the file.

  ```json
  {
    "printWidth": 120,
    "singleQuote": false,
    "trailingComma": "es5",
    "bracketSpacing": true,
    "semi": true,
    "htmlWhitespaceSensitivity": "ignore"
  }
  ```

- In the VSCode menu, click on **View -> Extensions**.
- In the search bar, type **prettier** to find the relevant extensions
- Select the **Prettier** extension (by Prettier), then click the green Install button.
- Open a JavaScript file using VSCode (you can make one in the terminal using the command `code temp.js`)
- In the VSCode menu, click on `View -> Command Palette...`
- In the prompt, search for `Format Document` and click or press enter.
- You may then be prompted by to choose which formatter to use. To do so, click the Configure button.
- Then choose Prettier - Code Formatter.

  Now every time you save a JavaScript file, prettier will reformat your code!
