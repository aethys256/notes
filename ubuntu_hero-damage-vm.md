# Hero Damage Ubuntu VM

## Setup

### Basics

Install Ubuntu

```sh
sudo apt-get update
sudo apt-get -y upgrade
sudo apt-get install -y git-core
sudo apt-get install -y curl gnupg build-essential

```

Install Guest Additions
Restart
Setup Shared Folders

### RVM
cf: https://github.com/rvm/ubuntu_rvm
```sh
sudo apt-get install -y software-properties-common
sudo apt-add-repository -y ppa:rael-gc/rvm
sudo apt-get update
sudo apt-get install -y rvm

```

Terminal > Edit > Profile Preferences > Command > Run command as login shell
Restart

### Ruby
```sh
rvm install ruby # Password needed

gem install bundler

```

### SimC & SimCScripts

```sh
cd ~/
mkdir github
cd github/
# SimC
git clone https://github.com/simulationcraft/simc.git
cd simc/
chmod +x ./generate_profiles.sh
cd ../
# SimCScripts
git clone https://github.com/Ravenholdt-TC/SimcScripts.git

```

## Run

### ssrun.sh
```sh
cat << 'EOF' > ~/github/ssrun.sh
#!/bin/bash

cd ~/github/simc/
git clean -xdf
git reset
git pull
make -C engine -j $(cat /proc/cpuinfo | grep processor | wc -l) LTO=1 optimized
ln -s engine/simc simc
./generate_profiles.sh

cd ~/github/SimcScripts/
git pull
bundle install
bundle exec ruby Run.rb
EOF
chmod +x ~/github/ssrun.sh

```

### ssclean.sh
```sh
cat << 'EOF' > ~/github/ssclean.sh
#!/bin/bash

cd ~/github/SimcScripts/
git clean -xdf
git reset
git pull
cp ~/Desktop/SimcConfig.yml ~/github/SimcScripts/
bundle install
EOF
chmod +x ~/github/ssclean.sh

```
