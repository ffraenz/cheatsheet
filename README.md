
# cheatsheet

Just another nasty cheatsheet.

Jump to a section:

- [Docker](#docker)
- [Git](#git)
- [Node.js](#nodejs)
- [MySQL](#mysql)
- [SSH](#ssh)
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
sudo chown -R $(whoami):admin /usr/local/lib/node_modules/
sudo chown -R $(whoami):admin /usr/local/n/
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

### Fix `~/.ssh` permissions

```bash
sudo chmod 700 ~/.ssh
sudo chmod 644 ~/.ssh/authorized_keys
sudo chmod 600 ~/.ssh/id_*
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
diskutil eraseDisk MS-DOS "WIN10" GPT disk#
# Mount the ISO file - returning the mounted volume name
hdiutil mount ~/Downloads/Win10_XXXX_XX_English_x64.iso
# Copy ISO content to the drive
cp -rp /Volumes/ISOVolumeName/* /Volumes/WIN10/
# Unmount disks, the ISO file is no longer needed
hdiutil unmount /Volumes/WIN10/
hdiutil unmount /Volumes/ISOVolumeName/
```

### Extend Windows XP trial period

Extending the trial period is limited to 4 times. Boot into safe mode by pressing `F8` while booting up and choosing 'Safe mode'. Login with a regular user, open the start menu and click ‘Run’, then type:

```
rundll32.exe syssetup,SetupOobeBnk
```
