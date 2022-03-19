# Misc: macOS

## SimC

### Build

```bash
# Clean & Pull
git clean -xdf
git pull

# CLI only
make -C engine -j $(expr $(sysctl -n hw.ncpu) / 2) LTO=1 optimized

# CLI + GUI
qmake simulationcraft.pro
make -j $(expr $(sysctl -n hw.ncpu) / 2) LTO=1
```

### Debug

```bash
make -C engine -j $(expr $(sysctl -n hw.ncpu) / 2) debug
lldb ./engine/simc XXX.simc
run
```

## Force enable TRIM

```bash
sudo trimforce enable
```

## Get ulimit current values

```bash
ulimit -a
```

## Get ulimit max values

```bash
ulimit -a -H
```

## Increase open files

```bash
ulimit -n 2048
```

## Keyboard reset

Sometimes macOS keyboard is confused and there is no other choice than deleting `/Library/Preferences/com.apple.keyboardtype.plist` file.\
See: <http://eng.raneri.it/blog/2009/01/17/how-to-reset-the-mac-keyboard/>

## Concat multiple video files (using ffmpeg)

`videos.txt`
```bash
file '/path/to/video1.ext'
file '/path/to/video2.ext'
file '/path/to/video3.ext'
```

```bash
ffmpeg -f concat -safe 0 -i videos.txt -c copy video.ext
```

## Splitting a zip into multiple parts

```bash
zip -s 10g 100g-split.zip 100g.zip
```

## Unzip multiple archive parts

The easiest way is to cat all of them into a single zip, like this:

```bash
cat /path/to/archive-parts/my-archive.zip.001 /path/to/archive-parts/my-archive.zip.002 /path/to/archive-parts/my-archive.zip.003 > my-archive.zip
```

If the multi-part was correctly done (i.e. a master .zip), double-clicking on the master .zip should do the trick though.

## Delete a part of a zip (mostly used to remove the annoying __MACOSX folder)

```bash
zip -d Archive.zip __MACOSX/\*
```

## Razer Synapse clean reinstall

Step 1: Open the Application Finder. Click on Utilities and launch Terminal. In your terminal: stop and remove launch agents by typing these commands one at a time.

```bash
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

Step 6: Reinstall Synapse. <http://drivers.razersupport.com//index.php?_m=downloads&_a=viewdownload&downloaditemid=2626&nav=0,343,239>

## Manually install XCode Command Line Tools missing headers in macOS 10.14+

See: <https://github.com/pyenv/pyenv/issues/1219>

```bash
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
```
