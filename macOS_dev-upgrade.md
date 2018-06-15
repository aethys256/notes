# Dev Upgrade Utils (macOS)

## Introduction
These are my personal macOS dev-upgrades notes. Feel free to pick-up whatever you might need.
Before starting, remember that those commands are related to my setup, see `macOS_post-install` for more infos.

## AIO

### Script
```sh
cat << 'EOF' > ~/aio-upgrade.sh
#!/bin/bash

shopt -s expand_aliases
source ~/.profile

brew update && brew upgrade && brew cask upgrade && brew cask cleanup && brew cleanup && brew prune

nvm install node --reinstall-packages-from=${NVM_VERSION_CURRENT_NODE}
nvm use node && npm i npm -g && npm update -g
nvm install lts/* --reinstall-packages-from=${NVM_VERSION_CURRENT_LTS}
nvm use lts/* && npm i npm -g && npm update -g && nvm alias default lts/*
export NVM_VERSION_CURRENT_NODE=$(nvm version node)
export NVM_VERSION_CURRENT_LTS=$(nvm version lts/*)

rvm get master && rvm reload && yes | rvm upgrade default && rvm use default && gem update

yes n | pyenv install $(pyenv-stable-latest)
yes n | pyenv install $(pyenv2-stable-latest)
pyenv global $(pyenv-stable-latest) $(pyenv2-stable-latest)
yes | pyenv uninstall $(pyenv-stable-previous)
yes | pyenv uninstall $(pyenv2-stable-previous)
pyenv rehash
pip install --upgrade pip
pip2 install --upgrade pip
pyenv rehash

echo -e '\n'
echo '------ Homebrew ------' && brew --version && brew cask --version && echo ''
echo '------ NVM ------' && nvm list && echo "nvm: $(nvm --version)" && echo "node: $(node --version)" && echo "npm: $(npm --version)" && echo ''
echo '------ RVM ------' && rvm list && rvm --version && ruby --version && bundler --version && echo ''
echo '------ PyEnv ------' && pyenv versions && pyenv --version && python --version && pip --version && python2 --version && pip2 --version && echo ''
EOF
chmod +x ~/aio-upgrade.sh

```

### Command to call the script
```sh
~/aio-upgrade.sh

```

## macOS

### Upgrade brew & casks
```sh
brew update && brew upgrade && brew cask upgrade && brew cask cleanup && brew cleanup && brew prune
brew --version && brew cask --version

```

## Node

### Upgrade Node & Node LTS w/ npm
```sh
nvm install node --reinstall-packages-from=${NVM_VERSION_CURRENT_NODE}
nvm use node && npm i npm -g && npm update -g
nvm install lts/* --reinstall-packages-from=${NVM_VERSION_CURRENT_LTS}
nvm use lts/* && npm i npm -g && npm update -g && nvm alias default lts/*
export NVM_VERSION_CURRENT_NODE=$(nvm version node)
export NVM_VERSION_CURRENT_LTS=$(nvm version lts/*)
nvm list && echo "nvm: $(nvm --version)" && echo "node: $(node --version)" && echo "npm: $(npm --version)"

```

## Ruby

### Upgrade RVM & Ruby & Gems
```sh
rvm get master && rvm reload && yes | rvm upgrade default && rvm use default && gem update
rvm list && rvm --version && ruby --version && bundler --version

```

### Upgrade a Gemfile
```sh
bundle update

```

## Python

### Upgrade Python
```sh
yes n | pyenv install $(pyenv-stable-latest)
yes n | pyenv install $(pyenv2-stable-latest)
pyenv global $(pyenv-stable-latest) $(pyenv2-stable-latest)
yes | pyenv uninstall $(pyenv-stable-previous)
yes | pyenv uninstall $(pyenv2-stable-previous)
pyenv rehash
pip install --upgrade pip
pip2 install --upgrade pip
pyenv rehash
pyenv versions && pyenv --version && python --version && pip --version && python2 --version && pip2 --version

```
