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

### Keyboard reset
Sometimes macOS keyboard is confused and there is no other choice than deleting `/Library/Preferences/com.apple.keyboardtype.plist` file.
See: http://eng.raneri.it/blog/2009/01/17/how-to-reset-the-mac-keyboard/

### Unzip multiple archive parts
The easiest way is to cat all of them into a single zip, like this:
```sh
cat /path/to/archive-parts/my-archive.zip.001 /path/to/archive-parts/my-archive.zip.002 /path/to/archive-parts/my-archive.zip.003 > my-archive.zip
```
