
# cheatsheet

Just another nasty cheatsheet.

Jump to a section:

- [Docker](#docker)
- [Git](#git)
- [Node.js](#nodejs)
- [MySQL](#mysql)
- [SSH](#ssh)
- [Compression](#compression)
- [Image processing](#image-processing)
- [macOS](#macos)
- [Windows](#windows)

## Docker

### Copy folder between Docker container and host

```bash
# Docker container -> Host
docker cp container_name:/srv/public/content/uploads ~/Downloads/uploads
# Host -> Docker container
docker cp ~/Downloads/uploads container_name:/srv/public/content/uploads
```

### Import and export MySQL database dumps

```bash
# Export or dump database
sudo docker exec -i container_name bash -c 'mysqldump -u $DB_USER -p"$DB_PASS" --default-character-set=utf8mb4 $DB_NAME' > /path/to/$(date +%F)-database.sql
# Import database (Attention!)
sudo docker exec -i container_name bash -c 'mysql -u $DB_USER -p"$DB_PASS" --default-character-set=utf8mb4 $DB_NAME' < /path/to/$(date +%F)-database.sql
```

## Git

### Change remotes

```bash
# List remotes
git remote -v
# Add an 'origin' remote
git remote add origin git@github.com:ffraenz/macos-config.git
# Change the 'origin' remote's URL
git remote set-url origin git@github.com:ffraenz/macos-config.git
# Remove the 'origin' remote
git remote rm origin
```

### Change file name case

```bash
git mv -f readme.md README.md
```

### Configure Git user

Remove the `--global` flag to only configure the current repository.

```bash
git config --global user.name "Fränz Friederes"
git config --global user.email fraenz@frieder.es
git config --global commit.gpgsign true
# Use the last 8 hex-formatted bytes from the fingerprint as signing key id
git config --global user.signingkey 583D205C93C01BC0
```

## Node.js

### Fix global and n package permissions

```bash
sudo mkdir -p /usr/local/n
sudo chown -R $(whoami) /usr/local/n /usr/local/bin /usr/local/lib /usr/local/include /usr/local/share
```

## MySQL

### Clear a database

```mysql
DROP DATABASE name;
CREATE DATABASE name;
```

### Enable or disable general logging

Log every connection and every query that clients send to the MySQL server.

```mysql
SET GLOBAL general_log = 'ON';
SET GLOBAL general_log = 'OFF';
```

Follow default MySQL log:

```bash
tail -f /var/log/mysql/mysql.log
```

## SSH

### Add public key to a host's authorized keys

```bash
ssh-copy-id -i ~/.ssh/id_rsa user@host
```

### Defer public key from existing private key

```bash
ssh-keygen -y -f ~/.ssh/id_rsa > ~/.ssh/id_rsa.pub
```

### Fix `~/.ssh` permissions

```bash
chown -R $USER ~/.ssh
sudo chmod 700 ~/.ssh
sudo chmod 644 ~/.ssh/authorized_keys
sudo chmod 600 ~/.ssh/id_* ~/.ssh/config
```

## Compression

### Tar archive compression and extraction

Create an archive (`-c`) using gzip compression (`-z`) with the given filename (`-f`).
Given the (`-C`) option `tar` changes into the given folder before compressing the target `.` preventing absolute URLs from showing up in the resulting archive.

```bash
tar -zcf archive.tar.gz -C path/to/folder .
```

Extract a tar ball (`-x`) with the given filename (`-f`):

```bash
tar -xf archive.tar.gz
```

### ZIP archive compression and extraction

Create an archive from a folder (`-r`) without invisible Mac resource files (`-X`).

```bash
zip -r -X archive.zip path/to/folder
zip archive.zip file1 file2 file3
```

Extract a ZIP archive into the same folder:

```bash
unzip archive.zip
```

## Image processing

### Create multi-resolution favicon

```bash
# Install prerequisites are available
brew install imagemagick optipng

# Optimize source PNGs and generate ICO file
optipng -o7 *.png
convert favicon-*.png favicon.ico
```

## macOS

### Create a bootable macOS installer on macOS

See [How to create a bootable installer for macOS](https://support.apple.com/en-us/HT201372).

## Windows

### Create a bootable Windows 10 installer on macOS

First download a Windows 10 ISO image [from this Microsoft page](https://www.microsoft.com/software-download/windows10ISO). After that follow these steps:

```bash
# Identify the disk identifier (disk#)
diskutil list
# Erase and format disk
diskutil eraseDisk MS-DOS "WINDOWS10" MBR disk#
# Mount the ISO file - returning the mounted volume name
hdiutil mount ~/Downloads/Win10_XXXX_XX_English_x64.iso
# Copy ISO content to the drive
cp -rp /Volumes/CCCOMA_ISO_VOLUME/* /Volumes/WINDOWS10/
# Unmount disks, the ISO file is no longer needed
hdiutil unmount /Volumes/WINDOWS10/
hdiutil unmount /Volumes/CCCOMA_ISO_VOLUME/
```

### Clean a disk

During Windows installation or when setting up a drive for the first use, you may need to clean or reset all partitions on a drive removing all data from it. For that open `cmd` and run `diskpart`, then type:

```bat
list disk
REM Identify the disk you want to reset
select disk #
REM Caution! The following command is dangerous.
clean
REM Check on the results
list disk
```

### Tunnel Windows Remote Desktop (RDP) through SSH on Linux

When having an an on-premise Linux server to SSH into it is possible to tunnel an RDP connection through SSH port forwarding to remote control a Windows machine from a Mac located outside the network.

Establish port forwarding between the Mac and a remote Linux host:

```
ssh -N -L 13389:[Remote IP]:3389 host
```

Now connect with the RDP client to `127.0.0.1:13389` as if it were the remote server.
