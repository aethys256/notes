# Scripts: WSL Ubuntu Upgrade

## Introduction

These are my personal WSL Ubutu dev-upgrades notes. Feel free to pick-up whatever you might need.\
Before starting, remember that those commands are related to my setup, see `wsl` for more info.

## Script

```bash
cat << 'EOF' > ~/upgrade.sh
#!/bin/bash

nodenv update
yes n | nodenv install $(nodenv-stable-latest)
nodenv global $(nodenv-stable-latest)
yes | nodenv uninstall $(nodenv-stable-previous)
nodenv rehash
npm update -g
nodenv rehash

curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash > /dev/null
yes n | rbenv install $(rbenv-stable-latest)
rbenv global $(rbenv-stable-latest)
yes | rbenv uninstall $(rbenv-stable-previous)
rbenv rehash
gem update --system
rbenv rehash
gem install bundler

pyenv update
yes n | pyenv install $(pyenv-stable-latest)
pyenv global $(pyenv-stable-latest)
yes | pyenv uninstall $(pyenv-stable-previous)
pyenv rehash
pip install --upgrade pip
pyenv rehash
pip install bitarray pefile requests fixedint # simc/casc_extract & simc/dbc_extract
pip install SLPP-23 # hero-dbc
pip install setuptools # hero-rotation-generator
pip install pylint # linter used by IDEs

sdk flush broadcast
sdk flush archives
sdk flush temp
yes | sdk selfupdate
yes | sdk update
yes | sdk upgrade java
yes | sdk upgrade gradle

rustup update

printf '\n'
echo '------ nodenv ------'
printf "nodenv versions:\n" && nodenv versions && printf "\nnodenv --version: $(nodenv --version)\n" && printf "node --version: $(node --version)\n" && printf "npm --version: $(npm --version)\n" && printf "yarn --version: $(yarn --version)\n"
printf '\n'
echo '------ rbenv ------'
printf "rbenv versions:\n" && rbenv versions && printf "\nrbenv --version: $(rbenv --version)\n" && printf "ruby --version: $(ruby --version)\n" && printf "gem --version: $(gem --version)\n"
printf '\n'
echo '------ pyenv ------'
printf "pyenv versions:\n" && pyenv versions && printf "\npyenv --version: $(pyenv --version)\n" && printf "python --version: $(python --version)\n" && printf "pip --version: $(pip --version)\n"
printf '\n'
echo '------ sdkman ------'
printf "sdk version: $(sdk version 2>&1 | sed '2q;d')\n" && printf "\njava --version:\n" && java --version && printf "\ngradle --version: $(gradle --version 2>&1 | sed '3q;d')\n"
printf '\n'
echo '------ dotnet ------'
printf "dotnet --version: $(dotnet --version)\n"
printf '\n'
echo '------ rustup ------'
printf "rustup --version: $(rustup --version 2>&1 | head -n 1)\n" && printf "rustc --version: $(rustc --version)\n"
EOF
chmod +x ~/upgrade.sh
```

## Commands

### Update

```bash
sudo apt update
sudo apt upgrade -y
. ~/upgrade.sh
```

### Uninstall nodenv version

```bash
yes | nodenv uninstall XXX
```

### Uninstall rubyenv version

```bash
yes | rbenv uninstall XXX
```

### Uninstall pyenv version

```bash
yes | pyenv uninstall XXX
```
