## Ubuntu Update

### Script
```bash
cat << 'EOF' > ~/upgrade.sh
#!/bin/bash

nodenv update
yes n | nodenv install $(nodenv-stable-latest)
nodenv global $(nodenv-stable-latest)
nodenv rehash
npm update -g
nodenv rehash

curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-installer | bash > /dev/null
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
sdk selfupdate
sdk update
sdk upgrade java
sdk upgrade gradle

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
```bash
sudo apt update
sudo apt upgrade -y
. ~/upgrade.sh
```