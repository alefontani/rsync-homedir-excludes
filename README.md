# rsync-homedir-excludes
This project maintains a list of directories and files you probably do not need to back up, which you can pass to the `rsync` command's `--exclude-from` option.

## Usage:

Download the list to tmp:

    wget https://raw.githubusercontent.com/alefontani/rsync-homedir-excludes/master/rsync-homedir-excludes.txt -O /var/tmp/ignorelist

Edit the file if you need:

    nano /var/tmp/ignorelist

Define a Backup directory with trailing slash (I've defined an external HD "Samsung T5" directory):

    backupHomeFolder=/media/$USER/samsung_t5/linuxbackup/$(date -u +"%Y-%m-%d")/home/$USER/

create the Backup directory:

    mkdir -p $backupHomeFolder

rsync!

    rsync -aP --exclude-from=/var/tmp/ignorelist /home/$USER/ $backupHomeFolder

## Alias

When commands are all tested you can make an alias function in you `.bashrc`:

```sh
function backup_t5() {
    backupSupport="/media/$USER/samsung_t5"

    if [ ! -d $backupSupport ]; then
        return 1
    fi

    wget https://raw.githubusercontent.com/alefontani/rsync-homedir-excludes/master/rsync-homedir-excludes.txt -O /var/tmp/ignorelist

    backupHomeFolder="$backupSupport/linuxbackup/$HOST-$(date -u +"%Y-%m-%d")/home/$USER/"
    mkdir -p $backupHomeFolder
    rsync -aP --exclude-from=/var/tmp/ignorelist /home/$USER/ $backupHomeFolder
}
```
