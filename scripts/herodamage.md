# Scripts: HeroDamage

Those scripts are meant to be run on Debian WSL which is the distribution I use to run HeroDamage simulations.\
Technically it can be adapted to work on any Linux distributions.

## Debian Install

```bash
# Debian
sudo apt update
sudo apt upgrade -y

# Utils
sudo apt install -y coreutils curl gawk
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
sudo apt install -y build-essential

# Ruby - rbenv
sudo apt-get install -y autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-installer | bash
cat << 'EOF' >> ~/.bashrc

# rbenv
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
alias rbenv-stable-list="rbenv install --list-all | grep -v '\-\|nightly\|dev\|next\|a\|b\|rc' | awk '{\$1=\$1};1' | grep '^2.7'"
alias rbenv-stable-latest="rbenv-stable-list | tail -1"
alias rbenv-stable-previous="rbenv-stable-list | tail -2 | head -1"
EOF

# Reload the shell
exec $SHELL

# Ruby
git clone https://github.com/rbenv/rbenv-each.git "$(rbenv root)/plugins/rbenv-each"
git clone https://github.com/tpope/rbenv-aliases.git "$(rbenv root)/plugins/rbenv-aliases"
rbenv install $(rbenv-stable-latest)
rbenv global $(rbenv-stable-latest)
rbenv rehash
gem update --system
rbenv rehash
printf "rbenv versions:\n" && rbenv versions && printf "\nrbenv --version: $(rbenv --version)\n" && printf "ruby --version: $(ruby --version)\n" && printf "gem --version: $(gem --version)\n"

# Ruby gems
gem install bundler
```

## Debian Update

### Script
```bash
cat << 'EOF' > ~/upgrade.sh
#!/bin/bash

curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash > /dev/null
yes n | rbenv install $(rbenv-stable-latest)
rbenv global $(rbenv-stable-latest)
yes | rbenv uninstall $(rbenv-stable-previous)
rbenv rehash
gem update --system
rbenv rehash
gem install bundler
gem install bundler:1.16.5

printf '\n'
echo '------ rbenv ------'
printf "rbenv versions:\n" && rbenv versions && printf '\n'
printf "rbenv --version: $(rbenv --version)\n"
printf "ruby --version: $(ruby --version)\n"
printf "gem --version: $(gem --version)\n"
EOF
chmod +x ~/upgrade.sh
```

### Commands

#### Update

```bash
sudo apt update
sudo apt upgrade -y
. ~/upgrade.sh
```

#### Uninstall rubyenv version

```bash
yes | rbenv uninstall XXX
```

## SimC & SimCScripts

### Initial setup

Choose a folder where the structure will be :

- simc
- simcscripts
- clean.sh
- run.sh
- SimcConfig.yml

In my case it is `/mnt/c/Users/Aethys/Documents/herodamage/xxx`, where `xxx` is the version of HD (`legion`, `bfa`, `shadowlands`, ...).

```bash
# Legion
git clone -b legion-dev https://github.com/simulationcraft/simc.git simc
chmod +x ./simc/generate_profiles.sh
git clone -b legion https://github.com/Ravenholdt-TC/SimcScripts.git simcscripts
git clone -b master https://github.com/herotc/legion.herodamage.com.git herodamage.com

# Battle for Azeroth
git clone -b bfa-dev https://github.com/simulationcraft/simc.git simc
chmod +x ./simc/generate_profiles.sh
git clone -b bfa https://github.com/Ravenholdt-TC/SimcScripts.git simcscripts
git clone -b master https://github.com/herotc/bfa.herodamage.com.git herodamage.com

# Shadowlands
git clone -b shadowlands https://github.com/simulationcraft/simc.git simc
chmod +x ./simc/generate_profiles.sh
git clone -b master https://github.com/Ravenholdt-TC/SimcScripts.git simcscripts
git clone -b master https://github.com/herotc/shadowlands.herodamage.com.git herodamage.com
```

Create the `SimcConfig.yml` file using `simcscripts/SimcConfig_Example.yml`.

### run.sh

```bash
cat << 'EOF' > ./run.sh
#!/bin/bash

BASE_DIR=$(pwd)

cd ${BASE_DIR}/simc/
git clean -xdf
git reset --hard
git pull
make -C engine -j $(expr $(cat /proc/cpuinfo | grep processor | wc -l) / 2) SC_NO_NETWORKING=1 LTO=1 optimized
cp engine/simc simc
./generate_profiles.sh

cd ${BASE_DIR}/herodamage.com/
git clean -xdf
git reset --hard
git pull

cd ${BASE_DIR}/simcscripts/
git pull
bundle install
cp ${BASE_DIR}/SimcConfig.yml ${BASE_DIR}/simcscripts/SimcConfig.yml
ruby Run.rb

cd ${BASE_DIR}
EOF
chmod +x ./run.sh
```

```bash
. ./run.sh
```

### clean.sh

```bash
cat << 'EOF' > ./clean.sh
#!/bin/bash

BASE_DIR=$(pwd)

cd ${BASE_DIR}/simcscripts/
git clean -xdf
git reset --hard

cd ${BASE_DIR}
EOF
chmod +x ./clean.sh
```

```bash
. ./clean.sh
```
