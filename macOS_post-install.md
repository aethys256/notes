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
gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
\curl -sSL https://get.rvm.io | bash -s stable --ruby

# Python - PyEnv
brew install pyenv

# PHP (Used as interpreter only in the IDE)
brew install php@5.6 php@7.2 php@7.3
brew link --overwrite php # Will link the latest php, i.e. 7.3 here

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

## NodeJS
# Automatically call "nvm use" on "cd"
find-up () {
    path=$(pwd)
    while [[ "$path" != "" && ! -e "$path/$1" ]]; do
        path=${path%/*}
    done
    echo "$path"
}
cdnvm(){
    cd "$@";
    nvm_path=$(find-up .nvmrc | tr -d '[:space:]')

    # If there are no .nvmrc file, use the default nvm version
    if [[ ! $nvm_path = *[^[:space:]]* ]]; then

        declare default_version;
        default_version=$(nvm version default);

        # If there is no default version, set it to `node`
        # This will use the latest version on your machine
        if [[ $default_version == "N/A" ]]; then
            nvm alias default node;
            default_version=$(nvm version default);
        fi

        # If the current version is not the default version, set it to use the default version
        if [[ $(nvm current) != "$default_version" ]]; then
            nvm use default;
        fi

        elif [[ -s $nvm_path/.nvmrc && -r $nvm_path/.nvmrc ]]; then
        declare nvm_version
        nvm_version=$(<"$nvm_path"/.nvmrc)

        declare locally_resolved_nvm_version
        # `nvm ls` will check all locally-available versions
        # If there are multiple matching versions, take the latest one
        # Remove the `->` and `*` characters and spaces
        # `locally_resolved_nvm_version` will be `N/A` if no local versions are found
        locally_resolved_nvm_version=$(nvm ls --no-colors $(<"./.nvmrc") | tail -1 | tr -d '\->*' | tr -d '[:space:]')

        # If it is not already installed, install it
        # `nvm install` will implicitly use the newly-installed version
        if [[ "$locally_resolved_nvm_version" == "N/A" ]]; then
            nvm install "$nvm_version";
        elif [[ $(nvm current) != "$locally_resolved_nvm_version" ]]; then
            nvm use "$nvm_version";
        fi
    fi
}
alias cd='cdnvm'
EOF

cat << 'EOF' > ~/.npmrc
//registry.npmjs.org/:_authToken=${NPM_TOKEN}
registry=https://registry.npmjs.org
EOF
```
It's time for a `reboot` to be safe.

### Upgrade Ruby & Node LTS & Tools
```bash
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
pip install bitarray pefile requests fixedint # simc/casc_extract & simc/dbc_extract
pip install SLPP-23 # hero-dbc
pip install setuptools # hero-rotation-generator
pip install pylint # linter used by IDEs

# Others
brew install libpng # 3rd party post-process lib (like webpack image loaders/plugins)
brew install graphicsmagick # Image resizing
brew install awscli # AWS CLI
brew install awsebcli # AWS ElasticBeanstalk CLI
brew install heroku/brew/heroku # Heroku CLI
brew install gradle # Mostly for Android stuff
brew install gpsbabel # GPSBabel (GPS utility)
brew install yarn --ignore-dependencies # Yarn (Node package manager)
brew install composer # Composer (PHP package manager)
brew install qt # QT (mostly for SimC GUI)
brew install clang-format # Clang format (mostly to format SimC)
brew install mongo # MongoDB ---> This is for CLI, use Docker instead of the service
brew install mysql # MySQL ---> This is for CLI, use Docker instead of the service
brew install postgresql # MySQL ---> This is for CLI, use Docker instead of the service
```

### Essential casks
```bash
brew cask install google-chrome firefox opera # Browsers
brew cask install keepassxc # Password manager
brew cask install visual-studio-code jetbrains-toolbox android-studio # IDEs
brew cask install sourcetree # Git GUI client
brew cask install postman # ADE
brew cask install filezilla cyberduck # File transfer
brew cask install slack skype # Messaging
brew cask install thunderbird # Mail client
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
```bash
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
Docker
League of Legends
HomeBank
OpenVPN
Skyfonts
```

### Apple only
```bash
brew cask install smcfancontrol # Control the fan speed (& display temperature)
```

### Hackintosh only
```bash
brew cask install clover-configurator # EFI Bootloader configurator
```

### Others
```bash
brew cask install istat-menus # Hardware monitoring, free alternative: yujitach-menumeters
brew cask install db-browser-for-sqlite # Popular browser for SQLite

## Requires password
brew cask install xquartz inkscape # Vector image edition
```

### JetBrains Intellij IDEA plugins

#### Bundled (but might be needed in WebStorm)
```
Docker Integration
Markdown
```

#### Misc
```
.env files support
.ignore
Apache config (.htaccess) support
BashSupport
CSV Plugin
Database Navigator
File Watchers
GNU GetText files support ​(*.​po)
Ideolog
Ini4Idea
Kubernetes
LiveEdit
String Manipulation
```

#### JavaScript / TypeScript
```
EJS
Handlebars/Mustache
JS GraphQL
Mongo Plugin
NodeJS
Prettier
Styled Components
Vue.js
```

#### PHP
```
Blade
DQL Support
Laravel
PHP
PHP Annotations
PHP composer.​json support
Php Inspections ​(EA Extended)
PHP Toolbox
Symfony Support
Twig Support
```

#### Ruby
```
Ruby
```

#### Python
```
Python
ReStructuredText Support
```

#### Lua
```
Lua
```
