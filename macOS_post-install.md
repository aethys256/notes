# Post Install Walkthrough (macOS)

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
```sh
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer

sudo xcodebuild -license accept

```

If you want to install the Command Line Tools yourself, you can also do it using this:
```sh
xcode-select --install

sudo xcode-select --switch /Library/Developer/CommandLineTools

```

### Install brew + cask & cask versions
```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

```
```sh
brew doctor

```
```sh
brew tap homebrew/cask
brew tap homebrew/cask-versions
brew update && brew upgrade && brew cask upgrade && brew cleanup && brew prune
brew --version

```

### Basics + NVM + RVM + Java8 + Misc
```sh
# Basics
brew install git git-flow git-lfs gnupg openssl zlib

# Node - NVM
brew install nvm
mkdir ~/.nvm

# Ruby - RVM
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
\curl -sSL https://get.rvm.io | bash -s stable --ruby

# Python - PyEnv
brew install pyenv

# PHP (Used as interpreter only in the IDE)
brew install php@5.6 php@7.0 php@7.1 php@7.2
brew link --overwrite php # Will link the latest php, i.e. 7.2 here

## Requires password
# Java + Java8 / JDK 1.8
brew cask install java
brew cask install java8

```
```sh
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
## SSH
# Register the SSH private keys from ~/.ssh as identities
grep -slR "PRIVATE" ~/.ssh | xargs ssh-add &> /dev/null

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

## Java 1.8
export JAVA_HOME=`/usr/libexec/java_home -v 1.8`

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
alias pyenv2-stable-list="pyenv-stable-list | grep '^2.'"
alias pyenv2-stable-latest="pyenv2-stable-list | tail -1"
alias pyenv2-stable-previous="pyenv2-stable-list | tail -2 | head -1"

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
  *) export PATH="${PATH}:${HOME}/.rvm/bin" ;;
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
```sh
# Install Node & Node LTS
nvm install node && nvm use node && npm update -g
nvm install --lts && nvm use lts/* && npm update -g && nvm alias default lts/*
nvm list && echo "nvm: $(nvm --version)" && echo "node: $(node --version)" && echo "npm: $(npm --version)"

# Upgrade RVM and Ruby + install Bundler
rvm get master && rvm reload && yes | rvm upgrade default && rvm use default && gem update && gem install bundler
rvm list && rvm --version && ruby --version && bundler --version

# Install latest Python & Pip, including the 2.x branch. The later is available through python2 command (and pip2)
yes n | env PYTHON_CONFIGURE_OPTS="--enable-framework CC=clang" pyenv install $(pyenv-stable-latest)
yes n | env PYTHON_CONFIGURE_OPTS="--enable-framework CC=clang" pyenv install $(pyenv2-stable-latest)
pyenv global $(pyenv-stable-latest) $(pyenv2-stable-latest)
pyenv rehash
pip2 install --upgrade pip
pip install --upgrade pip
pyenv rehash
pyenv versions && pyenv --version && python --version && pip --version && python2 --version && pip2 --version

# Install Python dependencies
pip install bitarray pefile requests # simc/casc_extract & simc/dbc_extract
pip install SLPP-23 # hero-dbc
pip install setuptools # hero-rotation-generator
pip install pylint # linter used by IDEs

# Others
brew install libpng # Used for any 3rd party postprocess lib (like webpack image loaders/plugins)
brew install graphicsmagick # Useful for image resizing, example: ionic-resources
brew install gradle # Used to compile Android apps
brew install mongo # MongoDB
brew install qt openssl # QT & OpenSSL (used to compile SimC)
brew install clang-format # Clang format (used to format SimC)
brew install awscli # AWS CLI
brew install awsebcli # AWS ElasticBeanstalk CLI
brew install heroku/brew/heroku # Heroku CLI
brew install yarn --without-node # Yarn
brew install gpsbabel # GPSBabel

```

### Essential casks
```sh
brew cask install google-chrome firefox opera # Browsers
brew cask install keepassxc # Password manager
brew cask install visual-studio-code jetbrains-toolbox android-studio # IDEs
brew cask install sourcetree # Git GUI client
brew cask install postman # ADE
brew cask install filezilla cyberduck # File transfer
brew cask install thunderbird slack skype # Messaging
brew cask install vlc # Multimedia player
brew cask install the-unarchiver # Archive utility
brew cask install onyx # System utility
brew cask install symboliclinker # Symbolic link in context menu
brew cask install spectacle # Window management app, need to be added in Accessibility settings
brew cask install scroll-reverser # Reverse mouse while keeping natural for trackpad

## Requires password
brew cask install dotnet-sdk # .NET SDK
brew cask install adobe-air flash-npapi flash-ppapi # Adobe Air & Flash Player NPAPI (Safari & Firefox) & Flash Player PPAPI (Chromium & Opera)
brew cask install teamviewer chrome-remote-desktop-host # Screen sharing

```

### Personal only
```sh
brew cask install twitch discord teamspeak-client # Gaming
brew cask install transmission # Torrent file transfer
brew cask install google-backup-and-sync cryptomator # File synchronization

## Requires password
brew cask install obs # Streaming/Recording
brew cask install virtualbox virtualbox-extension-pack # Virtualization

```
Manually:
```
Audacity
Battle.net
League of Legends
HomeBank
OpenVPN
Skyfonts
```

### Apple only
```sh
brew cask install smcfancontrol # Control the fan speed (& display temperature)

```

### Hackintosh only
```sh
brew cask install clover-configurator # EFI Bootloader configurator

```

### Others
```sh
brew cask install istat-menus # Hardware monitoring, free alternative: yujitach-menumeters
brew cask install db-browser-for-sqlite # Popular browser for SQLite

## Requires password
brew cask install xquartz inkscape # Vector image edition

```

### JetBrains Intellij IDEA plugins
```
.env files support
.ignore
Apache config (.htaccess) support
BashSupport
CSV Plugin
Docker integration
EJS
File Watchers
Handlebars/Mustache
Ideolog
Ini4Idea
JS GraphQL
Kotlin
LiveEdit
Lua
Mongo Plugin
NodeJS
PHP
Prettier
Python
Ruby
String Manipulation
Styled Components
Vue.js
```
