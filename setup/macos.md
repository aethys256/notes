# Setup: macOS

## Introduction

These are my personal macOS post-install notes. Feel free to pick-up whatever you might need.\
Before starting, do not forget to update macOS to the latest update thanks to the App Store.

## macOS defaults

Refer to `macOS-defaults` instructions.

## Dev Environment

Here is how I setup my development environment.

### Install XCode / XCode Command Line Tools

I do use XCode that's why, if you use it just for the CLI, homebrew will install it for you.\
Once it's installed, `reboot` then do:

```bash
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer

sudo xcodebuild -license accept
```

If you want to install the Command Line Tools yourself, you can also do it using this:

```bash
xcode-select --install

sudo xcode-select --switch /Library/Developer/CommandLineTools
```

### Install brew + cask & cask versions

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

```bash

brew doctor
```

```bash
brew tap homebrew/cask
brew tap homebrew/cask-versions
brew update && brew upgrade && brew upgrade --cask && brew cleanup
brew --version
```

### Basics + NVM + RVM + Java8 + Misc

```bash
# Basics
brew install git git-flow git-lfs gnupg openssl zlib sqlite
git lfs install

# Node - nodenv
brew install nodenv
echo 'eval "$(nodenv init -)"' >> ~/.zprofile

# Ruby - rbenv
brew install rbenv ruby-build
echo 'eval "$(rbenv init - zsh)"' >> ~/.zprofile

# Python - PyEnv
brew install pyenv
cat << 'EOF' >> ~/.zprofile
export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
EOF

# .NET Core SDK + Mono
brew install --cask dotnet-sdk

## Requires password
# Java
brew install openjdk

# Rust
curl -fsSL --tlsv1.2 --proto '=https' https://sh.rustup.rs | sh
```

```bash
# Note the quotes around the first EOF to avoid variable expansion :)
cat << 'EOF' > ~/.zshrc
## Prompt
export PS1="%F{magenta}%*%f %F{blue}%n%f@%F{green}%m%f %F{yellow}%~%k%f $ "

## LANG
export LANG="en_US.UTF-8"
export LANGUAGE="en_US.UTF-8"
export LC_ALL="en_US.UTF-8"

## SSH
# Register the SSH private keys from ~/.ssh as identities
grep -slR "PRIVATE" ~/.ssh | xargs ssh-add &> /dev/null

## SourceTree
# In order to have our PATH correctly setup, we have to open SourceTree from a terminal, so we make an alias for that.
# See https://community.atlassian.com/t5/Bitbucket-questions/SourceTree-Hook-failing-because-paths-don-t-seem-to-be-set/qaq-p/274792
alias ostree="open /Applications/SourceTree.app"

## Homebrew
eval "$(/opt/homebrew/bin/brew shellenv)"

## Authentication Tokens (GitHub + NPM)
export GH_TOKEN="XXXXXXXX"
export NPM_TOKEN_AETHYS="XXXXXXXX"
export NPM_TOKEN_CTA="XXXXXXXX"
export NPM_TOKEN="$NPM_TOKEN_CTA"
USE_NPM_TOKEN_AETHYS () {
    export NPM_TOKEN="$NPM_TOKEN_AETHYS"
}
USE_NPM_TOKEN_CTA () {
    export NPM_TOKEN="$NPM_TOKEN_CTA"
}

## Python (PyEnv)
export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
alias pyenv-stable-list="pyenv install --list | sed 's/^  //' | grep '^\d' | grep --invert-match 'dev\|a\|b\|rc'"
alias pyenv-stable-latest="pyenv-stable-list | tail -1"
alias pyenv-stable-previous="pyenv-stable-list | tail -2 | head -1"

## NodeJS (nodenv)
eval "$(nodenv init -)"
alias nodenv-stable-list="nodenv install --list | grep -v '\-\|nightly\|dev\|next\|a\|b\|rc' | awk '{\$1=\$1};1' | grep '^16.'"
alias nodenv-stable-latest="nodenv-stable-list | tail -1"
alias nodenv-stable-previous="nodenv-stable-list | tail -2 | head -1"

## Ruby (pyenv)
eval "$(rbenv init -)"
alias rbenv-stable-list="rbenv install --list-all | grep -v '\-\|nightly\|dev\|next\|a\|b\|rc' | awk '{\$1=\$1};1' | grep '^3.1'"
alias rbenv-stable-latest="rbenv-stable-list | tail -1"
alias rbenv-stable-previous="rbenv-stable-list | tail -2 | head -1"

## Java
export JAVA_HOME=`/usr/libexec/java_home`

## Android
export ANDROID_HOME="${HOME}/Library/Android/sdk"
export PATH="${PATH}:${ANDROID_HOME}/tools"
export PATH="${PATH}:${ANDROID_HOME}/platform-tools"
EOF

cat << 'EOF' > ~/.npmrc
//registry.npmjs.org/:_authToken=${NPM_TOKEN}
registry=https://registry.npmjs.org
EOF
```

It's time for a `reboot` to be safe.

### Upgrade Ruby & Node LTS & Tools

```bash
# Node
git clone https://github.com/nodenv/nodenv-update.git "$(nodenv root)/plugins/nodenv-update"
git clone https://github.com/nodenv/nodenv-package-json-engine.git "$(nodenv root)/plugins/nodenv-package-json-engine"
git clone https://github.com/nodenv/nodenv-aliases.git "$(nodenv root)/plugins/nodenv-aliases"
git clone https://github.com/nodenv/nodenv-each.git "$(nodenv root)/plugins/nodenv-each"
git clone https://github.com/nodenv/nodenv-package-rehash.git "$(nodenv root)/plugins/nodenv-package-rehash"
git clone https://github.com/nodenv/nodenv-npm-migrate.git "$(nodenv root)/plugins/nodenv-npm-migrate"
nodenv install $(nodenv-stable-latest)
nodenv global $(nodenv-stable-latest)
nodenv rehash
npm install --global npm@latest
nodenv rehash
npm install --global yarn
nodenv rehash
printf "nodenv versions:\n" && nodenv versions && printf "\nnodenv --version: $(nodenv --version)\n" && printf "node --version: $(node --version)\n" && printf "npm --version: $(npm --version)\n" && printf "yarn --version: $(yarn --version)\n"

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

# Install Python dependencies
pip install bitarray pefile requests fixedint # simc/casc_extract & simc/dbc_extract
pip install SLPP-23 # hero-dbc
pip install setuptools # hero-rotation-generator
pip install pylint # linter used by IDEs

# Others
brew install mongo postgresql # This is only for the CLI (use Docker)
brew install libpng # 3rd party post-process lib (like webpack image loaders/plugins)
brew install graphicsmagick # Image resizing
brew install ffmpeg # Video conversion
brew install awscli # AWS CLI
brew install gradle # Mostly for Android stuff
brew install gpsbabel # GPSBabel (GPS utility)
brew install qt clang-format # QT & ClangFormat (mostly for SimC)
```

### Markdown

```bash
brew install librsvg
brew install pandoc
brew cask install basictex

## Requires password
sudo tlmgr update --self
sudo tlmgr install collection-fontsrecommended
```

### Manually

#### Development

- [Cyberduck](https://cyberduck.io/download/)
- [DB Browser for SQLite](https://sqlitebrowser.org/dl/)
- [Docker](https://download.docker.com/mac/stable/Docker.dmg)
- [FileZilla](https://filezilla-project.org/download.php?platform=osx)
- [Java](https://www.java.com/en/download/)
- [MongoDB Compass Comunity Edition](https://www.mongodb.com/download-center/compass)
- [pgAdmin](https://www.pgadmin.org/download/pgadmin-4-macos/)
- [Postman](https://www.postman.com/downloads/)
- [Sourcetree](https://www.sourcetreeapp.com/)
- [Unity Hub](https://unity3d.com/get-unity/download) + [.NET Core](https://dotnet.microsoft.com/download) + [Mono](https://www.mono-project.com/download/stable/) (Stable channel for VSCode)

#### Productivity

- [Google Chrome](https://www.google.com/chrome/)
- [Google Chrome Remote Desktop](https://remotedesktop.google.com/)
- [Google Drive](https://www.google.com/drive/download/)
- [KeepassXC](https://keepassxc.org/download/)
- [Microsoft Office](https://www.office.com/apps)
- [Microsoft OneDrive](https://www.microsoft.com/en-us/microsoft-365/onedrive/download)


#### Communication

- [Discord](https://discordapp.com/download)
- [Microsoft Teams](https://teams.microsoft.com/downloads)
- [Slack](https://slack.com/downloads/mac)
- [Telegram](https://desktop.telegram.org/)
- [WhatsApp](https://www.whatsapp.com/download/)

#### Audio / Image / Video

- [Adobe Creative Cloud](https://creativecloud.adobe.com/en/apps/download/creative-cloud)
- [Audacity](https://www.audacityteam.org/download/mac/)
- [Inkscape](https://inkscape.org/release/)
- [NDI Tools](https://www.ndi.tv/tools/#download-tools)
- [OBS](https://obsproject.com/download)
- [Spotify](https://www.spotify.com/us/download/mac)
- [VLC](https://www.videolan.org/vlc/)

#### Utils

- [Corsair iCue](https://www.corsair.com/us/en/icue-mac)
- [iStat Menus](https://beta.bjango.com/mac/istatmenus/)
- [Logitech G Hub](https://www.logitechg.com/en-au/innovation/g-hub.html)
- [NordVPN](https://nordvpn.com/download/)
- [Onyx](https://www.titanium-software.fr/en/onyx.html)
- [Scroll Reverser](https://pilotmoon.com/scrollreverser/)
- [Spectacle](https://www.spectacleapp.com/)
- [SteerMouse](https://plentycom.jp/en/steermouse/download.php)
- [The Unarchiver](https://theunarchiver.com/)

#### Others

- [Battle.net](https://www.blizzard.com/en-us/apps/battle.net/desktop)
- [League of Legends](https://signup.euw.leagueoflegends.com/en/signup/redownload)
- [Transmission](https://transmissionbt.com/download/)
