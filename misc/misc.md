# Miscellaneous

## Docker

### Get default Git, NodeJS, NPM and Yarn versions on Node image

```bash
docker run -it node:16 /bin/bash
node --version
npm --version
yarn --version
```

### Clean

```bash
docker-compose down # Stop the container(s)
docker rm -f $(docker ps -a -q) # Delete all containers
docker volume rm $(docker volume ls -q) # Delete all volumes
docker-compose up # Restart the containers
```

## Apache Bench

### Simple request

```bash
ab -n 10000 -c 512 -k http://127.0.0.1:3000/
```

## Symbolic link

```bash
ln -s /path/to/original/ /path/to/link
```

## Diff

```bash
diff --unified=3 file1.ext file2.ext > file.diff
```

Where the number after `--unified=` is the number of lines showing the context. To fully output the file, put a big number (> number of lines).

## SSL

### Convert .pem & .key to .pfx (pkcs12)

This example use Cloudflare Origin CA root certificates for CAfile but the idea is the same.

```bash
openssl pkcs12 -export -out myCertificate.pfx -in myCertificate.pem -inkey myCertificate.key -CAfile origin_ca_rsa_root.pem
```

It will then ask for a password before creating the certificate.

## SSH Key

### Generate

```bash
# Modern system
ssh-keygen -t ed25519 -C "comment, usually mail address" -f "filename, usually username-Platform"

# Legacy system
ssh-keygen -t rsa -b 4096 -C "comment, usually mail address" -f "filename, usually username-Platform"
```

### Add

```bash
ssh-add ~/.ssh/PRIVATE-KEY-FILE &> /dev/null
```

### Config

~/.ssh/config:

```text
Host github.com
	HostName github.com
	User git
	IdentityFile ~/.ssh/aethys256-GitHub
```

## PostgreSQL

### List all collations

```sql
SELECT * FROM pg_collation;
```

### Create Database with UTF8

```sql
CREATE DATABASE "myDatabase" ENCODING 'UTF8' LC_COLLATE = 'en_US.utf8' LC_CTYPE = 'en_US.utf8' TEMPLATE template0;
```

## Python

### Uninstall all pip packages

```bash
pip freeze | xargs pip uninstall -y
```

## Git

### Aliases

```bash
[alias]
 rebase-last = "!b=\"$(git branch --no-color | cut -c3-)\" ; h=\"$(git rev-parse $b)\" ; echo \"Current branch: $b $h\" ; c=\"$(git rev-parse $b)\" ; echo \"Recreating $b branch with initial commit $c ...\" ; git checkout --orphan new-start $c ; git commit -C $c ; git rebase --onto new-start $c $b ; git branch -d new-start ; git gc"
```

### History Clean/Rewrite

```bash
# Clone the repository and fetch everything
git clone git@my-repo.git my-repo
git fetch --all
git pull --all
git lfs fetch --all
git lfs pull --all

# Now make a backup of `my-repo`: `my-repo-bak1`

# Export LFS objects to normal objects
git lfs migrate export --everything --include="*.3dm,*.3ds,*.blend,*.c4d,*.collada,*.dae,*.dxf,*.fbx,*.jas,*.lws,*.lxo,*.ma,*.max,*.mb,*.obj,*.ply,*.skp,*.stl,*.ztl,*.aif,*.aiff,*.it,*.mod,*.mp3,*.ogg,*.s3m,*.wav,*.xm,*.otf,*.ttf,*.bmp,*.exr,*.gif,*.hdr,*.iff,*.jpeg,*.jpg,*.pict,*.png,*.psd,*.tga,*.tif,*.tiff,*.mov,*.mp4,*.webm"
git lfs prune
git reflog expire --expire=now --all && git gc --prune=now --aggressive

# Now make a second backup of `my-repo`: `my-repo-bak2`

# Install `git-filter-repo` and use it inside the git repository to analyze what to remove
python ../git-filter-repo --analyze --force

# Go to `.git\filter-repo\analysis` and define a filtering strategy (you can apply multiple filtering strategies)

# Using regex
python ../git-filter-repo --force --path-regex '.*\.(webm|tif|mp4|bundle|7z|psd|cache|zip|obj|blend1|blend|hlsl|bin|playable|terrainlayer|guiskin|lighting|overrideController|hash|signal|watermesh|config|CopyComplete|wav|png|tga)$' --invert-paths --prune-empty never --prune-degenerate never

# Using paths
python ../git-filter-repo --force --path MyDir/ --path MyOtherDir/ --invert-paths --prune-empty never --prune-degenerate never

# Using blobs size
python ../git-filter-repo --force --strip-blobs-bigger-than 50K --prune-empty never --prune-degenerate never

# It will clear everything including your latest "safe" commit, so you might want to port back what should be kept from your last commit in case you went too far in the cleaning
# You might need to rebase if you have multiple branches

# Now make a third backup of `my-repo`: `my-repo-bak2`

# Import LFS objects from normal objects
git lfs migrate import --everything --include="*.3dm,*.3ds,*.blend,*.c4d,*.collada,*.dae,*.dxf,*.fbx,*.jas,*.lws,*.lxo,*.ma,*.max,*.mb,*.obj,*.ply,*.skp,*.stl,*.ztl,*.aif,*.aiff,*.it,*.mod,*.mp3,*.ogg,*.s3m,*.wav,*.xm,*.otf,*.ttf,*.bmp,*.exr,*.gif,*.hdr,*.iff,*.jpeg,*.jpg,*.pict,*.png,*.psd,*.tga,*.tif,*.tiff,*.mov,*.mp4,*.webm"

# Export/Import might have messed your `.gitattributes`, so update it and commit it.

# Clear your local .git from artefacts
git reflog expire --expire=now --all && git gc --prune=now --aggressive

# Add back the origin you want to push to since `git-filter-repo` might have removed it. KEEP IN MIND THAT FOR GITHUB YOU NEED TO MAKE A NEW REPO TO CLEAN EFFECTIVELY LFS OBJECTS
git remote add origin git@address.git
git push --all

# Delete your local directories and backups if not needed anymore.
```

## GPSBabel

### Convert GPX Track to GPX Waypoints

```bash
gpsbabel -i gpx -f INPUT.gpx -x transform,wpt=trk -o gpx -F OUTPUT.gpx
```

## Pandoc

### Convert Markdown to PDF

```bash
pandoc file.md --pdf-engine=xelatex -V geometry:margin=1.5in -V urlcolor=blue -o file.pdf
```

## FFmpeg

### 360 Video

```bash
# Credits: https://headjack.io/blog/360-video-cloud-encoding-profiles/

# PC (VR Headset)
ffmpeg -i "monoscopic_video.mp4" -c:v libx264 -crf 18 -x264-params "mvrange=511" -maxrate 120M -bufsize 150M -vf "scale=5760x2880" -pix_fmt yuv420p -c:a aac -b:a 192k -r 30 -movflags faststart "monoscopic_output.mp4"
ffmpeg -i "stereoscopic_video.mp4" -c:v libx265 -crf 17 -maxrate 120M -bufsize 150M -vf "scale=4096x4096" -pix_fmt yuv420p -c:a aac -b:a 192k -r 30 -movflags faststart "stereoscopic_output.mp4"

# Mobile (Cardboard / Autonomous VR Headset): up to 3840×2160 for Stereo
ffmpeg -i "monoscopic_video.mp4" -c:v libx265 -crf 17 -maxrate 80M -bufsize 100M -vf "scale=4096x2048" -pix_fmt yuv420p -c:a aac -b:a 192k -r 30 -movflags faststart "monoscopic_output.mp4"

# Stream: use Mobile for Mono
ffmpeg -i "stereoscopic_video.mp4" -c:v libx265 -crf 20 -x265-params "keyint=60:min-keyint=60" -maxrate 25M -bufsize 35M -vf "scale=3840x2160" -pix_fmt yuv420p -c:a aac -b:a 192k -r 30 -g 60 "stereoscopic_output.mp4"

# WebVR: no Stereo
ffmpeg -i "monoscopic_video.mp4" -c:v libx264 -crf 21 -maxrate 10M -bufsize 15M -vf "scale=1920x1080" -pix_fmt yuv420p -c:a aac -b:a 192k -r 30 -g 60 -keyint_min 60 "monoscopic_output.mp4"
```

### Copy metadata

```bash
exiftool -api largefilesupport=1 -tagsFromFile "source.mov" -all:all "destination.mp4"
```

### MediaInfo

## Sheet Template

```csv
ColumnsCount;21
Column0;General;0;CompleteName;80
Column1;General;0;Format;10
Column2;Video;0;Format/String;10
Column3;Audio;0;Format/String;10
Column4;Text;0;Format/String;10
Column5;General;0;FileSize/String;10
Column6;General;0;Duration/String3;10
Column7;Video;0;BitRate_Mode/String;10
Column8;Video;0;BitRate/String;10
Column9;Audio;0;BitRate_Mode/String;10
Column10;Audio;0;BitRate/String;10
Column11;Video;0;FrameRate_Mode/String;10
Column12;Video;0;Width/String;10
Column13;Video;0;Height/String;10
Column14;Audio;0;SamplingRate/String;10
Column15;Audio;0;BitDepth/String;10
Column16;Audio;0;Channel(s)/String;10
Column19;Video;0;ColorSpace;10
Column17;Video;0;ChromaSubsampling/String;10
Column20;Video;0;colour_range;10
Column18;Video;0;BitDepth/String;10
```
