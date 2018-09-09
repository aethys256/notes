# Hero Damage Windows + Ubuntu WSL

## Setup

### Basics

Install Ubuntu WSL

```sh
sudo apt-get update
sudo apt-get -y upgrade
sudo apt-get install -y git-core
sudo apt-get install -y curl gnupg build-essential

```

Restart

### RVM
cf: https://github.com/rvm/ubuntu_rvm
```sh
sudo apt-get install -y software-properties-common
sudo apt-add-repository -y ppa:rael-gc/rvm
sudo apt-get update
sudo apt-get install -y rvm

```

Restart

### Ruby
```sh
rvm install ruby # Password needed

gem install bundler

```

### SimC & SimCScripts

```sh
cd /mnt/d
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

Create the `SimcConfig.yml` file.

## Run

### ssrun-wsl.sh
```sh
cat << 'EOF' > /mnt/d/github/ssrun-wsl.sh
#!/bin/bash

BASE_DIR=/mnt/d/github

cd ${BASE_DIR}/simc/
git clean -xdf
git reset --hard
git pull
make -C engine -j $(expr $(cat /proc/cpuinfo | grep processor | wc -l) / 2) LTO=1 optimized
cp engine/simc simc
./generate_profiles.sh

cd ${BASE_DIR}/SimcScripts/
git pull
bundle install
cp ${BASE_DIR}/SimcConfig-WSL.yml ${BASE_DIR}/SimcScripts/SimcConfig.yml
bundle exec ruby Run.rb
EOF
chmod +x /mnt/d/github/ssrun-wsl.sh

```

### ssclean-wsl.sh
```sh
cat << 'EOF' > /mnt/d/github/ssclean-wsl.sh
#!/bin/bash

BASE_DIR=/mnt/d/github

cd ${BASE_DIR}/SimcScripts/
git clean -xdf
git reset --hard
EOF
chmod +x /mnt/d/github/ssclean-wsl.sh

```

## macOS Run

### ssrun-macos.sh
```sh
cat << 'EOF' > /Volumes/DATA/github/ssrun-macos.sh
#!/bin/bash

BASE_DIR=/Volumes/DATA/github

cd ${BASE_DIR}/simc/
git clean -xdf
git reset --hard
git pull
make -C engine -j $(expr $(sysctl -n hw.ncpu) / 2) LTO=1 optimized
cp engine/simc simc
./generate_profiles.sh

cd ${BASE_DIR}/SimcScripts/
git pull
bundle install
cp ${BASE_DIR}/SimcConfig-macOS.yml ${BASE_DIR}/SimcScripts/SimcConfig.yml
bundle exec ruby Run.rb
EOF
chmod +x /Volumes/DATA/github/ssrun-macos.sh

```

### ssclean-macos.sh
```sh
cat << 'EOF' > /Volumes/DATA/github/ssclean-macos.sh
#!/bin/bash

BASE_DIR=/Volumes/DATA/github

cd ${BASE_DIR}/SimcScripts/
git clean -xdf
git reset --hard
EOF
chmod +x /Volumes/DATA/github/ssclean-macos.sh

```
