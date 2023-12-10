# Setup: Apple MacOS Terminal Programs

## Configure the Terminal

- Open the terminal (open Finder, then click **Applications -> Utilities -> Terminal.app**)

- Paste in the following command to use zsh as your shell software:

  ```
  chsh -s /bin/zsh
  ```

  _(enter your computer’s password when prompted)_

- Paste in the following commands to add colors and autocompletion to the default Mac terminal:

  ```bash
  echo "export CLICOLOR=1" >> ~/.zshrc
  echo "export LSCOLORS=GxFxCxDxBxegedabagaced" >> ~/.zshrc
  echo "export EDITOR='code -w'" >> ~/.zshrc
  echo "" >> ~/.zshrc
  echo "alias ls='ls -GFh'" >> ~/.zshrc
  echo "" >> ~/.zshrc
  echo "PROMPT='%F{cyan}%~%f %# '" >> ~/.zshrc
  echo "autoload -Uz compinit && compinit" >> ~/.zshrc
  echo "zstyle ':completion:*' matcher-list '' 'm:{a-zA-Z}={A-Za-z}'" >> ~/.zshrc
  echo "zstyle ':completion:*' menu select" >> ~/.zshrc
  echo "zstyle ':completion:*' completer _expand_alias _complete _ignored" >> ~/.zshrc
  echo "gem: --no-document" >> ~/.gemrc
  echo 'Done!'
  ```

- To see the changes take effect, close the current terminal window and open a new one.

## Install Homebrew

**Homebrew is a tool that allows you to install other programs through the terminal.**

- In the terminal, run the following command to install homebrew:

  ```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```

  - If it asks you to install XCode CommandLine Tools, say yes.
  - **If you have a newer Mac with an M1 processor**, you may get a message at the end saying "`Run these two commands in your terminal to add Homebrew to your PATH:`". The commands should look like this:
    ```bash
    echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> $HOME/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
    ```
    - If you see this message in your terminal, copy and paste those commands into your terminal to complete your homebrew installation.

- In the terminal, run the following command to see if everything is installed properly:

  ```bash
  brew -v
  ```

  _(It should show the version installed. If it says `brew: command not found`, something went wrong.)_

## Install Ruby using rbenv

**rbenv is a tool that enables you to install multiple versions of Ruby**

- In the terminal, run the following commands to install and load rbenv:

  ```bash
  brew install rbenv ruby-build
  echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.zshrc
  source ~/.zshrc
  ```

- In the terminal, run the following commands to install ruby using rbenv:

  ```bash
  rbenv install 3.2.2
  ```

  _(this command takes a few minutes to complete)_

  ```bash
  rbenv global 3.2.2
  ```
  
- Close the current terminal window and open a new one.

- In the terminal, run the following command to see if everything is installed properly:

  ```bash
  ruby -v
  ```

  _(It should show the new version installed. If it says `ruby: command not found`, something went wrong.)_

## Install postgres

**postgres is a database used to save data for backend applications**

- In the terminal, run the following commands to install and run postgres:

  ```bash
  brew install postgresql@14
  brew services start postgresql@14
  ```

- In the terminal, run the following command to see if everything is installed properly:

  ```bash
  pg_isready
  ```

  _(It should say `/tmp:5432 - accepting connections`. If it doesn’t say this, something went wrong.)_

## Install Rails

**Rails is a Ruby framework used to make backend web applications**

- In the terminal, run the following commands to install Rails:

  ```bash
  gem install rails -v 7.0.4
  rbenv rehash
  ```

- In the terminal, run the following command to see if everything is installed properly:

  ```bash
  rails -v
  ```

  _(It should show the version installed. If it says `rails: command not found`, something went wrong.)_
  
## Final checks
  
- In the terminal, run the following command to check if you can install a gem:
  ```bash
  gem install http
  ```
  
  _(It should print messages that end with something like `12 gems installed`)_

- In the terminal, run the following commands to check if you can create a rails application:

  ```bash
  rails new test-rails-app --api -d postgresql
  cd test-rails-app
  rails db:create
  rails server
  ```
  
- In a web browser, visit http://localhost:3000 to see your website.

  _(You should see a Rails logo in the middle of the page with your Rails version and Ruby version at the bottom)_

- In your terminal, enter `control c` to stop the rails app.