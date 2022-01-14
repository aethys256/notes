# Setup: macOS Install

## Introduction

These are my personal macOS post-install notes. Feel free to pick-up whatever you might need.
Before starting, do not forget to update macOS to the latest update thanks to the App Store.

## macOS defaults

Refer to `macOS_defaults` instructions.

## Dev Environment

Here is how I setup my development environment.

### Install XCode / XCode Command Line Tools

I do use XCode that's why, if you use it just for the CLI, homebrew will install it for you.
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
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

```bash
brew doctor
```

```bash
brew tap homebrew/cask
brew tap homebrew/cask-versions
brew update && brew upgrade && brew cask upgrade && brew cleanup
brew --version
```

### Basics + NVM + RVM + Java8 + Misc

```bash
# Basics
brew install git git-flow git-lfs gnupg openssl zlib sqlite

# Node - NVM
brew install nvm
mkdir ~/.nvm

# Ruby - RVM
\curl -sSL https://rvm.io/mpapis.asc | gpg --import -
\curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -
\curl -sSL https://get.rvm.io | bash -s stable --ruby

# Python - PyEnv
brew install pyenv

# PHP (Only used as interpreter in the IDE)
brew install php

## Requires password
# Java
brew cask install java
```

```bash
# Setup the initial .profile
# We use .profile as only dotfile, see: https://superuser.com/a/789465
rm ~/.bash_profile; rm ~/.bashrc; rm ~/.mkshrc; rm ~/.profile; rm ~/.zlogin; rm ~/.zshrc;

# Note the quotes around the first EOF to avoid variable expansion :)
# Also, we double check if we already exported to the path using:
# case ":$PATH:" in
#   *:"THE_PATH_TO_SEARCH":*) PATH_IS_ALREADY_IN ;;
#   *) PATH_IS_NOT_IN ;;
# esac
cat << 'EOF' > ~/.profile
## Prompt
export PS1="\[\033[35m\]\t\[\033[m\]-\[\033[36m\]\u\[\033[m\]@\[\033[32m\]\h:\[\033[33;1m\]\w\[\033[m\]\$ "

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
# Add to the PATH: "$(brew --prefix)/sbin"
case ":$PATH:" in
  *:"$(brew --prefix)/sbin":*) ;;
  *) export PATH="$(brew --prefix)/sbin:${PATH}" ;;
esac

## Local binaries
case ":$PATH:" in
  *:"~/.local/bin":*) ;;
  *) export PATH="~/.local/bin:${PATH}" ;;
esac

## Compilation flags (mostly for PyEnv)
export LDFLAGS="${LDFLAGS} -L/usr/local/opt/zlib/lib"
export CPPFLAGS="${CPPFLAGS} -I/usr/local/opt/zlib/include"
export LDFLAGS="${LDFLAGS} -L/usr/local/opt/sqlite/lib"
export CPPFLAGS="${CPPFLAGS} -I/usr/local/opt/sqlite/include"
export PKG_CONFIG_PATH="${PKG_CONFIG_PATH} /usr/local/opt/zlib/lib/pkgconfig"
export PKG_CONFIG_PATH="${PKG_CONFIG_PATH} /usr/local/opt/sqlite/lib/pkgconfig"

## GitHub
export GH_TOKEN="XXXXXXXX"

## Java
export JAVA_HOME=`/usr/libexec/java_home`

## Android
export ANDROID_HOME="${HOME}/Library/Android/sdk"
case ":$PATH:" in
  *:"${ANDROID_HOME}/tools":*) ;;
  *) export PATH="${PATH}:${ANDROID_HOME}/tools" ;;
esac
case ":$PATH:" in
  *:"${ANDROID_HOME}/platform-tools":*) ;;
  *) export PATH="${PATH}:${ANDROID_HOME}/platform-tools" ;;
esac

## QT
case ":$PATH:" in
  *:"/usr/local/opt/qt/bin":*) ;;
  *) export PATH="${PATH}:/usr/local/opt/qt/bin" ;;
esac

## Python (PyEnv)
# Add to the PATH: "${HOME}/.pyenv/shims"
case ":$PATH:" in
  *:"${HOME}/.pyenv/shims":*) ;;
  *) eval "$(pyenv init -)" ;;
esac
alias pyenv-stable-list="pyenv install --list | sed 's/^  //' | grep '^\d' | grep --invert-match 'dev\|a\|b\|rc'"
alias pyenv-stable-latest="pyenv-stable-list | tail -1"
alias pyenv-stable-previous="pyenv-stable-list | tail -2 | head -1"

## NodeJS
# npm Auth Token
export NPM_TOKEN="XXXXXXXX"
# nvm
export NVM_DIR="${HOME}/.nvm"
. "/usr/local/opt/nvm/nvm.sh"
export NVM_VERSION_CURRENT_NODE=$(nvm version node)
export NVM_VERSION_CURRENT_LTS=$(nvm version lts/*)

## Ruby (RVM)
# Add RVM to PATH for scripting. Make sure this is the last PATH variable change.
case ":$PATH:" in
  *:"${HOME}/.rvm/bin":*) ;;
  *) export PATH="${HOME}/.rvm/bin:${PATH}" ;;
esac
# Load RVM into a shell session *as a function*
[[ -s "${HOME}/.rvm/scripts/rvm" ]] && source "${HOME}/.rvm/scripts/rvm"
EOF

cat << 'EOF' > ~/.npmrc
//registry.npmjs.org/:_authToken=${NPM_TOKEN}
registry=https://registry.npmjs.org
EOF
```

It's time for a `reboot` to be safe.

### Upgrade Ruby & Node LTS & Tools

```bash
# Install Node & Node LTS & Yarn
nvm install node && nvm use node && npm update -g
nvm install --lts && nvm use lts/* && npm update -g && nvm alias default lts/*
nvm list && echo "nvm: $(nvm --version)" && echo "node: $(node --version)" && echo "npm: $(npm --version)"
brew install yarn # Yarn (Node package manager)

# Upgrade RVM and Ruby
rvm get master && rvm reload && yes | rvm upgrade default && rvm use default && gem update
rvm list && rvm --version && ruby --version && bundler --version

# Install latest Python & Pip.
yes n | env PYTHON_CONFIGURE_OPTS="--enable-framework CC=clang" pyenv install $(pyenv-stable-latest)
pyenv global $(pyenv-stable-latest)
pyenv rehash
pip install --upgrade pip
pyenv rehash
pyenv versions && pyenv --version && python --version && pip --version

# Install Python dependencies
pip install bitarray pefile requests fixedint # simc/casc_extract & simc/dbc_extract
pip install SLPP-23 # hero-dbc
pip install setuptools # hero-rotation-generator
pip install pylint # linter used by IDEs

# Others
brew install mongo mysql@5.7 postgresql@11 # This is only for the CLI (use Docker)
brew link mysql@5.7 --force
brew link postgresql@11 --force
brew install libpng # 3rd party post-process lib (like webpack image loaders/plugins)
brew install graphicsmagick # Image resizing
brew install awscli awsebcli # AWS CLI & AWS ElasticBeanstalk CLI
brew install heroku/brew/heroku # Heroku CLI
brew install gradle # Mostly for Android stuff
brew install composer # Composer (PHP package manager)
brew install gpsbabel # GPSBabel (GPS utility)
brew install qt clang-format # QT & ClangFormat (mostly for SimC)
brew install watchman # Needed by Prisma GraphQL Extension for VSCode
```

### Essential casks

```bash
### Essentials ###
brew install --cask google-chrome firefox # Browsers
brew install --cask keepassxc # Password manager
brew install --cask visual-studio-code jetbrains-toolbox # IDEs
brew install --cask sourcetree # Git GUI client
brew install --cask postman # ADE
brew install --cask pgadmin4 # PostgreSQL management
brew install --cask cyberduck # File transfer
brew install --cask slack skype # Messaging
brew install --cask vlc # Multimedia player
brew install --cask the-unarchiver # Archive utility
brew install --cask onyx # System utility
brew install --cask symboliclinker # Symbolic link in context menu
brew install --cask spectacle # Window management app
brew install --cask scroll-reverser # Reverse mouse while keeping natural for trackpad

## Requires password
brew install --cask adobe-air flash-npapi flash-ppapi # Adobe Air & Flash Player NPAPI (Safari & Firefox) & Flash Player PPAPI (Chromium & Opera)
brew install --cask teamviewer chrome-remote-desktop-host # Screen sharing


### Others ###
brew install --cask google-backup-and-sync cryptomator # File synchronization
brew install --cask transmission # Torrent file transfer
```

### Video
```bash
brew install ffmpeg
```

### Markdown

```bash
brew install librsvg
brew install pandoc
brew install --cask basictex

## Requires password
sudo tlmgr update --self
sudo tlmgr install collection-fontsrecommended
```

### Manually

- [Audacity](https://www.audacityteam.org/download/mac/)
- [Battle.net](https://www.blizzard.com/en-us/apps/battle.net/desktop)
- [Discord](https://discordapp.com/download)
- [Docker](https://download.docker.com/mac/stable/Docker.dmg)
- [FileZilla](https://filezilla-project.org/download.php?platform=osx)
- [League of Legends](https://signup.euw.leagueoflegends.com/en/signup/redownload)
- [Logitech Gaming Software](https://support.logi.com/hc/en-001/articles/360025298053-Logitech-Gaming-Software)
- [Microsoft Office](https://www.office.com/apps) + [Microsoft Teams](https://teams.microsoft.com/downloads)
- [MongoDB Compass Comunity Edition](https://www.mongodb.com/download-center/compass)
- [OBS](https://obsproject.com/download)
- [Skyfonts](https://www.monotype.com/products/skyfonts)
- [Spotify](https://www.spotify.com/us/download/mac)
- [Teamspeak](https://teamspeak.com/en/downloads/)
- [Twitch](https://www.twitch.tv/downloads/videos/all)
- [Unity Hub](https://unity3d.com/get-unity/download) + [.NET Core](https://dotnet.microsoft.com/download) + [Mono](https://www.mono-project.com/download/stable/) (Stable channel for VSCode)

### Hackintosh only

```bash
brew install --cask clover-configurator # EFI Bootloader configurator
```

### Others

```bash
brew install --cask istat-menus # Hardware monitoring, free alternative: yujitach-menumeters
brew install --cask db-browser-for-sqlite # Popular browser for SQLite

## Requires password
brew install --cask xquartz inkscape # Vector image edition
```
