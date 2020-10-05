# Miscellaneous

## AWS

### ElasticBeanstalk: Use 'npm run ...' during SSH session

```bash
sudo su
export PATH=$PATH:`ls -vdr /opt/elasticbeanstalk/node-install/node-* | head -1`/bin
/opt/elasticbeanstalk/bin/get-config environment | python -c "import json,sys; obj=json.load(sys.stdin); f = open('/tmp/eb_env', 'w'); f.write('\n'.join(map(lambda x: 'export ' + x[0] + '=' + x[1], obj.iteritems())))"
source /tmp/eb_env
rm -f /tmp/eb_env
cd /var/app/current/

# npm run ...
```

## Docker

### Clean

```bash
docker-compose down # Stop the container(s)
docker rm -f $(docker ps -a -q) # Delete all containers
docker volume rm $(docker volume ls -q) # Delete all volumes
docker-compose up # Restart the containers
```

## Heroku

### Login / Swap user

```bash
heroku login
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
ssh-keygen -t rsa -b 4096 -C "comment, usually mail address" -f "filename, usually username-Platform"
```

### Add

```bash
ssh-add ~/.ssh/PRIVATE-KEY-FILE &> /dev/null
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
