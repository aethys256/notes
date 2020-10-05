# Scripts: HeroDamage

## SimC & SimCScripts

```bash
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


## ssrun-wsl.sh

```bash
cat << 'EOF' > /mnt/c/Users/Aethys/Documents/herodamage/ssrun-wsl.sh
#!/bin/bash

BASE_DIR=/mnt/c/Users/Aethys/Documents/herodamage

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
ruby Run.rb
EOF
chmod +x /mnt/c/Users/Aethys/Documents/herodamage/ssrun-wsl.sh
```

## ssclean-wsl.sh

```bash
cat << 'EOF' > /mnt/c/Users/Aethys/Documents/herodamage/ssclean-wsl.sh
#!/bin/bash

BASE_DIR=/mnt/c/Users/Aethys/Documents/herodamage

cd ${BASE_DIR}/SimcScripts/
git clean -xdf
git reset --hard
EOF
chmod +x /mnt/c/Users/Aethys/Documents/herodamage/ssclean-wsl.sh
```
