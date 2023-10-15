# Setup: VSCode linter for Ruby

## Step 1: Install rubocop from the Terminal
- In your terminal, enter `gem install rubocop`
- Check to see if it works by entering `rubocop -v`. If you see a version number, you’re done!
  - If you get permission errors, this generally means you are using the default version of Ruby that comes with the Mac. To avoid these issues, follow the instructions Installing Homebrew and Installing Ruby from here: https://gorails.com/setup/osx. 
  - Once you’re done, enter `gem install rubocop` and it should install properly.

## Step 2: Create a .rubocop.yml file using VSCode
- In your terminal, run the command: `code ~/.rubocop.yml`
  (This will create a new file named .rubocop.yml in your home directory)
- Copy the following code into the empty file: 
  ```yml
  AllCops:
    DisabledByDefault: true

  Layout/IndentationConsistency:
    EnforcedStyle: indented_internal_methods

  Layout/IndentationWidth:
    Width: 2

  Style/MethodDefParentheses:
    EnforcedStyle: require_parentheses

  Naming/MethodName:
    EnforcedStyle: snake_case

  Naming/VariableName:
    EnforcedStyle: snake_case

  Naming/VariableNumber:
    EnforcedStyle: normalcase

  Lint/AssignmentInCondition:
    AllowSafeAssignment: true

  Layout/BlockAlignment:
    EnforcedStyleAlignWith: either

  Layout/EndAlignment:
    EnforcedStyleAlignWith: keyword
    AutoCorrect: false

  Layout/DefEndAlignment:
    EnforcedStyleAlignWith: start_of_line
    AutoCorrect: false

  Lint/InheritException:
    EnforcedStyle: runtime_error

  Lint/UnusedBlockArgument:
    IgnoreEmptyBlocks: true
    AllowUnusedKeywordArguments: false

  Lint/UnusedMethodArgument:
    AllowUnusedKeywordArguments: false
    IgnoreEmptyMethods: true
  ```
- Save the file.

Now you can use rubocop in the terminal! If you are in the terminal in a folder with a Ruby file in it (for example test.rb), you can use rubocop like so: `rubocop test.rb`

## Step 3: Install the Ruby VSCode extension
- In the VSCode menu, click on `View -> Extensions`.
- In the search bar, type `rubocop.vscode-rubocop` to find the RuboCop extension (by RuboCop Headquarters)
- Click the green Install button.

## Bonus: Auto-format ruby code using the rufo gem
- In your terminal, enter the command `gem install rufo`
- In the VSCode menu, click on `View -> Extensions`.
- In the search bar, type `jnbt.vscode-rufo` to find the Rufo - Ruby formatter extension (by jnbt)
- Click the green Install button.
