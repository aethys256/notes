# Miscellaneous

## Commands

### Heroku
#### Login / Swap user
```sh
heroku login
```

### Symbolic link
```sh
ln -s /path/to/original/ /path/to/link
```

### Diff

```sh
diff --unified=3 file1.ext file2.ext > file.diff
```
Where the number after `--unified=` is the number of lines showing the context. To fully output the file, put a big number (> number of lines).

### SSH Key

#### Generate
```sh
ssh-keygen -t rsa -b 4096 -C "comment, usually mail address" -f "filename, usually username-Platform"
```

#### Add
```sh
ssh-add ~/.ssh/PRIVATE-KEY-FILE &> /dev/null
```

## macOS

### SimC building
```sh
# Clean & Pull
git clean -xdf
git pull

# CLI only
make -C engine -j 16 LTO=1 optimized

# CLI + GUI
qmake simulationcraft.pro
make -j 16 LTO=1 optimized

```

### Force enable TRIM
```sh
sudo trimforce enable
```

### Keyboard reset
Sometimes macOS keyboard is confused and there is no other choice than deleting `/Library/Preferences/com.apple.keyboardtype.plist` file.
See: http://eng.raneri.it/blog/2009/01/17/how-to-reset-the-mac-keyboard/

### Unzip multiple archive parts
The easiest way is to cat all of them into a single zip, like this:
```sh
cat /path/to/archive-parts/my-archive.zip.001 /path/to/archive-parts/my-archive.zip.002 /path/to/archive-parts/my-archive.zip.003 > my-archive.zip
```

### Razer Synapse clean reinstall
Step 1: Open the Application Finder. Click on Utilities and launch Terminal. In your terminal: stop and remove launch agents by typing these commands one at a time.
```sh
launchctl remove com.razer.rzupdater

launchctl remove com.razerzone.rzdeviceengine

sudo rm /Library/LaunchAgents/com.razer.rzupdater.plist

sudo rm /Library/LaunchAgents/com.razerzone.rzdeviceengine.plist

```
Step 2: Remove HID kernel extension
```
sudo rm -Rf /System/Library/Extensions/RazerHid.kext

```
Step 3: Manually delete Razer Synapse app from Applications in Finder

Step 4: Delete Razer files from "Application Support" folders:
```
sudo rm -rf /Library/Application\ Support/Razer/

```

Step 5: Restart your Mac.

Step 6: Reinstall Synapse. http://drivers.razersupport.com//index.php?_m=downloads&_a=viewdownload&downloaditemid=2626&nav=0,343,239
