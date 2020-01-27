# Dev Upgrade Utils (macOS)

## Introduction

These are my personal macOS dev-upgrades notes. Feel free to pick-up whatever you might need.
Before starting, remember that those commands are related to my setup, see `macOS_post-install` for more infos.

## AIO

### Script

```bash
cat << 'EOF' > ~/aio-upgrade.sh
#!/bin/bash

shopt -s expand_aliases
source ~/.profile

brew update && brew upgrade && brew cask upgrade && brew cleanup

nvm install node --reinstall-packages-from=${NVM_VERSION_CURRENT_NODE}
nvm use node && npm i npm -g && npm update -g
nvm install lts/* --reinstall-packages-from=${NVM_VERSION_CURRENT_LTS}
nvm use lts/* && npm i npm -g && npm update -g && nvm alias default lts/*
export NVM_VERSION_CURRENT_NODE=$(nvm version node)
export NVM_VERSION_CURRENT_LTS=$(nvm version lts/*)

rvm get master && rvm reload && rvm reinstall default && yes | rvm upgrade default && rvm use default && gem update

yes n | env PYTHON_CONFIGURE_OPTS="--enable-framework CC=clang" pyenv install $(pyenv-stable-latest)
pyenv global $(pyenv-stable-latest)
yes | pyenv uninstall $(pyenv-stable-previous)
pyenv rehash
pip install --upgrade pip
pyenv rehash

pip install --upgrade bitarray pefile requests fixedint # simc/casc_extract & simc/dbc_extract
pip install --upgrade SLPP-23 # hero-dbc
pip install --upgrade setuptools # hero-rotation-generator
pip install --upgrade pylint # linter used by IDEs

echo -e '\n'
echo '------ Homebrew ------' && brew --version && echo ''
echo '------ NVM ------' && nvm list && echo "nvm: $(nvm --version)" && echo "node: $(node --version)" && echo "npm: $(npm --version)" && echo ''
echo '------ RVM ------' && rvm list && rvm --version && ruby --version && bundler --version && echo ''
echo '------ PyEnv ------' && pyenv versions && pyenv --version && python --version && pip --version && echo ''
EOF
chmod +x ~/aio-upgrade.sh
```

### Command to call the script

```bash
~/aio-upgrade.sh
```

## macOS

### Upgrade brew & casks

```bash
brew update && brew upgrade && brew cask upgrade && brew cleanup
brew --version
```

## Node

### Upgrade Node & Node LTS w/ npm

```bash
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

Note: We reinstall ruby because brew can break it during his update, otherwise it's not needed to reinstall.

```bash
rvm get master && rvm reload && rvm reinstall default && yes | rvm upgrade default && rvm use default && gem update
rvm list && rvm --version && ruby --version && bundler --version
```

### Upgrade a Gemfile

```bash
bundle update
```

## Python

### Upgrade Python

```bash
yes n | env PYTHON_CONFIGURE_OPTS="--enable-framework CC=clang" pyenv install $(pyenv-stable-latest)
pyenv global $(pyenv-stable-latest)
yes | pyenv uninstall $(pyenv-stable-previous)
pyenv rehash
pip install --upgrade pip
pyenv rehash
pyenv versions && pyenv --version && python --version && pip --version

pip install --upgrade bitarray pefile requests # simc/casc_extract & simc/dbc_extract
pip install --upgrade SLPP-23 # hero-dbc
pip install --upgrade setuptools # hero-rotation-generator
pip install --upgrade pylint # linter used by IDEs
```
