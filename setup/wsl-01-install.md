# Setup: WSL Install

## Introduction

These are my personal Windows WSL post-install notes, using Ubuntu distribution. Feel free to pick-up whatever you might need.
Before starting, do not forget to update Windows to the latest update.

## Install WSL2 & Ubuntu 20.04

This is how I setup my development environment. This is assuming a pristine Windows installation but you might attempt it on an already used one.
Follow instructions from this [official documentation](https://docs.microsoft.com/en-us/windows/wsl/install-win10) in order to get WSL2.

TL;DR:
```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
Restart.
```powershell
wsl --set-default-version 2
```

Get and launch the distribution from the [Ubuntu 20.04 Microsoft Store](https://www.microsoft.com/store/apps/9n6svws3rx71) page.
If you already have Ubuntu 20.04 and you want to start over, you can unregister it and register it again (you loose every data inside).
Once in the distribution shell, setup the username and password (save it in a credentials manager like KeePass).

## Initial setup
```bash
# Ubuntu
sudo apt update
sudo apt upgrade -y

# Utils
sudo apt install -y coreutils curl file gawk gnupg
cat << 'EOF' >> ~/.bashrc

# prompt
export PS1="\[\033[35m\]\t\[\033[m\]-\[\033[36m\]\u\[\033[m\]@\[\033[32m\]\h:\[\033[33;1m\]\w\[\033[m\]\$ "
EOF

# Fix Windows drive mounting, cf. https://askubuntu.com/a/1242671
sudo tee /etc/wsl.conf > /dev/null << 'EOF'
[automount]
options = "metadata"
EOF
```

Exit every WSL shells and then run `wsl --shutdown` in a PowerShell (or restart the computer).

## cpp + nodenv + rbenv + pyenv + sdkman + Android + .NET Core + Rust

```bash
# Git
sudo apt install -y git git-flow git-lfs
git config --global user.name "Quentin Giraud"
git config --global user.email "dev@aethys.io"
cat << 'EOF' >> ~/.bashrc

# Register the SSH private keys from ~/.ssh as identities, cf. https://stackoverflow.com/a/48509425
# Ensure agent is running
ssh-add -l &>/dev/null
if [ "$?" == 2 ]; then
    # Could not open a connection to your authentication agent.

    # Load stored agent connection info.
    test -r ~/.ssh-agent && eval "$(<~/.ssh-agent)" >/dev/null

    ssh-add -l &>/dev/null
    if [ "$?" == 2 ]; then
        # Start agent and store agent connection info.
        (umask 066; ssh-agent > ~/.ssh-agent)
        eval "$(<~/.ssh-agent)" >/dev/null
    fi
fi
# Load identities
ssh-add -l &>/dev/null
if [ "$?" == 1 ]; then
    # The agent has no identities.
    grep -slR "PRIVATE" ~/.ssh | xargs ssh-add &> /dev/null
fi
EOF
# DO NOT FORGET TO PUT THE SSH KEYS IN the '~/.ssh' FOLDER (import from KeePass or generate them)
# cp /mnt/c/Users/Aethys/Documents/ssh/aethys256-GitHub ~/.ssh/aethys256-GitHub
# chmod 600 ~/.ssh/aethys256-GitHub
# cp /mnt/c/Users/Aethys/Documents/ssh/aethys256-GitHub.pub ~/.ssh/aethys256-GitHub.pub
# chmod 600 ~/.ssh/aethys256-GitHub.pub

# C/C++
sudo apt install -y build-essential gdb lldb

# Node - nodenv
sudo apt-get install g++ make python python3-distutils
curl -fsSL https://raw.githubusercontent.com/nodenv/nodenv-installer/master/bin/nodenv-installer | bash
cat << 'EOF' >> ~/.bashrc

# nodenv
export PATH="$HOME/.nodenv/bin:$PATH"
eval "$(nodenv init -)"
alias nodenv-stable-list="nodenv install --list | grep -v '\-\|nightly\|dev\|next\|a\|b\|rc' | awk '{\$1=\$1};1' | grep '^16.'"
alias nodenv-stable-latest="nodenv-stable-list | tail -1"
alias nodenv-stable-previous="nodenv-stable-list | tail -2 | head -1"
EOF
# Authentications Tokens (replace the XXXXXXXX)
cat << 'EOF' >> ~/.bashrc

# Authentication Tokens (GitHub + NPM)
export GH_TOKEN="XXXXXXXX"
export NPM_TOKEN_AETHYS="XXXXXXXX"
export NPM_TOKEN_MYSG="XXXXXXXX"
export NPM_TOKEN="$NPM_TOKEN_MYSG"
USE_NPM_TOKEN_AETHYS () {
    export NPM_TOKEN="$NPM_TOKEN_AETHYS"
}
USE_NPM_TOKEN_MYSG () {
    export NPM_TOKEN="$NPM_TOKEN_MYSG"
}
EOF
# npmrc file for NPM (also used by Yarn v1)
cat << 'EOF' > ~/.npmrc
//registry.npmjs.org/:_authToken=${NPM_TOKEN}
registry=https://registry.npmjs.org
EOF
# yarnrc.yml file for Yarn v2
cat << 'EOF' > ~/.yarnrc.yml
npmAuthToken: "${NPM_TOKEN}"
npmPublishRegistry: "https://registry.npmjs.org"
npmRegistryServer: "https://registry.npmjs.org"
npmScopes:
    mysg:
        npmPublishRegistry: "https://registry.npmjs.org"
        npmRegistryServer: "https://registry.npmjs.org"
        npmAlwaysAuth: false
        npmAuthToken: "${NPM_TOKEN_MYSG}"
EOF

# Ruby - rbenv
sudo apt-get install -y autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash
cat << 'EOF' >> ~/.bashrc

# rbenv
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
alias rbenv-stable-list="rbenv install --list-all | grep -v '\-\|nightly\|dev\|next\|a\|b\|rc' | awk '{\$1=\$1};1' | grep '^3.1'"
alias rbenv-stable-latest="rbenv-stable-list | tail -1"
alias rbenv-stable-previous="rbenv-stable-list | tail -2 | head -1"
EOF

# Python - pyenv
sudo apt-get install -y build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
curl -fsSL https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
cat << 'EOF' >> ~/.bashrc

# pyenv
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv virtualenv-init -)"
alias pyenv-stable-list="pyenv install --list | grep -v '\-\|nightly\|dev\|next\|a\|b\|rc' | awk '{\$1=\$1};1' | grep '^3.10'"
alias pyenv-stable-latest="pyenv-stable-list | tail -1"
alias pyenv-stable-previous="pyenv-stable-list | tail -2 | head -1"
EOF

# Java (+ Scala, Gradle, ...) - SDKMAN
sudo apt-get install -y curl sed unzip zip
curl -fsSL "https://get.sdkman.io?rcupdate=false" | bash
cat << 'EOF' >> ~/.bashrc

# sdkman
export SDKMAN_DIR="$HOME/.sdkman"
[[ -s "${SDKMAN_DIR}/bin/sdkman-init.sh" ]] && source "${SDKMAN_DIR}/bin/sdkman-init.sh"
export JAVA_HOME="$SDKMAN_DIR/candidates/java/current"
EOF

# Android
sudo apt-get install -y android-sdk
cat << 'EOF' >> ~/.bashrc

# android
export ANDROID_HOME="/usr/lib/android-sdk"
export PATH="$PATH:$ANDROID_HOME/tools/bin"
export PATH="$PATH:$ANDROID_HOME/platform-tools"
EOF

# .NET Core (SDK will automatically install the Runtime)
curl -fsSL https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -o packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install -y apt-transport-https
sudo apt-get install -y dotnet-sdk-3.1

# Rust
curl -fsSL --tlsv1.2 --proto '=https' https://sh.rustup.rs | sh # Type '1' for normal installation
cat << 'EOF' >> ~/.bashrc

# rustup
export PATH="$HOME/.cargo/bin:$PATH"
EOF

# Reload the shell
exec $SHELL
```

## Install Node + Ruby + Python + Java

```bash
# nodenv plugins + Node
git clone https://github.com/nodenv/nodenv-update.git "$(nodenv root)/plugins/nodenv-update"
git clone https://github.com/nodenv/nodenv-package-json-engine.git "$(nodenv root)/plugins/nodenv-package-json-engine"
git clone https://github.com/nodenv/nodenv-aliases.git "$(nodenv root)/plugins/nodenv-aliases"
git clone https://github.com/nodenv/nodenv-each.git "$(nodenv root)/plugins/nodenv-each"
git clone https://github.com/nodenv/nodenv-package-rehash.git "$(nodenv root)/plugins/nodenv-package-rehash"
git clone https://github.com/nodenv/nodenv-npm-migrate.git "$(nodenv root)/plugins/nodenv-npm-migrate"
nodenv install $(nodenv-stable-latest)
nodenv global $(nodenv-stable-latest)
nodenv rehash
npm update -g
nodenv rehash
printf "nodenv versions:\n" && nodenv versions && printf "\nnodenv --version: $(nodenv --version)\n" && printf "node --version: $(node --version)\n" && printf "npm --version: $(npm --version)\n"

# Ruby
git clone https://github.com/rbenv/rbenv-each.git "$(rbenv root)/plugins/rbenv-each"
git clone https://github.com/tpope/rbenv-aliases.git "$(rbenv root)/plugins/rbenv-aliases"
rbenv install $(rbenv-stable-latest)
rbenv global $(rbenv-stable-latest)
rbenv rehash
gem update --system
rbenv rehash
printf "rbenv versions:\n" && rbenv versions && printf "\nrbenv --version: $(rbenv --version)\n" && printf "ruby --version: $(ruby --version)\n" && printf "gem --version: $(gem --version)\n"

# Python & Pip
git clone https://github.com/pyenv/pyenv-pip-migrate.git "$(pyenv root)/plugins/pyenv-pip-migrate"
pyenv install $(pyenv-stable-latest)
pyenv global $(pyenv-stable-latest)
pyenv rehash
pip install --upgrade pip
pyenv rehash
printf "pyenv versions:\n" && pyenv versions && printf "\npyenv --version: $(pyenv --version)\n" && printf "python --version: $(python --version)\n" && printf "pip --version: $(pip --version)\n"

# Java
sdk install java
sdk install gradle # Mostly for Android stuff
printf "sdk version:" && sdk version && printf "\njava --version:\n" && java --version && printf "\ngradle --version:" && gradle --version
```

## Other Utilities

```bash
# Node - yarn
curl -fsSL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
sudo apt-get install -y --no-install-recommends yarn

# Ruby gems
gem install bundler

# Python packages
pip install bitarray pefile requests fixedint # simc/casc_extract & simc/dbc_extract
pip install SLPP-23 # hero-dbc
pip install setuptools # hero-rotation-generator
pip install pylint # linter used by IDEs

# Markdown
sudo apt-get install -y pandoc texlive texlive-latex-recommended texlive-full

# NCurses Disk Usage
sudo apt-get install -y ncdu

# AWS CLI
sudo apt-get install -y unzip
curl -fsSL https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o ~/awscliv2.zip
unzip ~/awscliv2.zip
rm ~/awscliv2.zip
~/aws/install -i $HOME/.awscli -b $HOME/.awscli/bin
cat << 'EOF' >> ~/.bashrc

# AWS CLI
export PATH="$HOME/.awscli/bin:$PATH"
EOF
rm -rf ~/aws
# AWS ElasticBeanstalk CLI
sudo apt-get install -y build-essential zlib1g-dev libssl-dev libncurses-dev libffi-dev libsqlite3-dev libreadline-dev libbz2-dev
git clone https://github.com/aws/aws-elastic-beanstalk-cli-setup.git ~/aws-elastic-beanstalk-cli-setup
~/aws-elastic-beanstalk-cli-setup/scripts/bundled_installer
cat << 'EOF' >> ~/.bashrc

# AWS ElasticBeanstalk CLI
export PATH="$HOME/.ebcli-virtual-env/executables:$PATH"
EOF
rm -rf ~/aws-elastic-beanstalk-cli-setup
# Azure CLI
curl -fsSL https://aka.ms/InstallAzureCLIDeb | sudo bash
# Heroku CLI
sudo apt-get install -y apt-transport-https
echo "deb https://cli-assets.heroku.com/apt ./" | sudo tee /etc/apt/sources.list.d/heroku.list
curl -fsSL https://cli-assets.heroku.com/apt/release.key | sudo apt-key add -
sudo apt-get update
sudo apt-get install -y heroku

# Misc
sudo apt-get install -y libpng-dev # 3rd party post-process lib (like webpack image loaders/plugins)
sudo apt-get install -y graphicsmagick # Image resizing
sudo apt-get install -y gpsbabel # GPSBabel (GPS utility)
sudo apt-get install -y clang-format # ClangFormat (mostly for SimC)

# VS Code Extension
curl -fsSL https://aka.ms/vsls-linux-prereq-script -o ~/vsls-reqs && chmod +x ~/vsls-reqs && ~/vsls-reqs && rm ~/vsls-reqs # Live Share

```
