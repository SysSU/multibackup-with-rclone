tar-multibackup
===============


Bash script to backup multiple folders and to clean up old backups based on a retention time. Features configurable post/pre-commands, tar excludes as well as backup retentions. Original script from https://github.com/frdmn/tar-multibackup.

### Installation

    cd /usr/local/src
    git clone https://github.com/frdmn/tar-multibackup.git
    ln -sf /usr/local/src/tar-multibackup/multibackup /usr/local/bin/multibackup
    cp /usr/local/src/tar-multibackup/multibackup.conf ~/.multibackup.conf

### Configuration and usage

* `timestamp` = Format of the timestamp, used in the backup target filename
* `backup_destination` = Local directory which is used to store the archives/backups
* `rclone_destination` = Rclone remote and path where archives/backups will be copied to
* `folders_to_backup` = Array of folders to backup
* `backup_retention` = Retention time how long we should keep the backups
* `pre_commands` = Array of commands that are executed before the backup starts (stop specific service)
* `post_commands` = Array of commands that are executed after the backup finished (start specific service)

### Environment configurations

* `DEBUG` = if set to "true", `set -x` will be set
* `CONFIG` = if you want to use a different configuration file than the 

Example: 

    CONFIG=/tmp/testbackup.conf DEBUG=true multibackup

#### Example configuration 

`vi ~/.multibackup.conf`

    # Timestamp format, used in the backup target filename
    timestamp=$(date +%Y%m%d)

    # Destination where you want to store your backups
    backup_destination="/var/backups"

    # Folders to backup
    folders_to_backup=(
      "/etc"
      "/var/mail"
      "/var/www"
    )

    # Files and folders that are excluded in the tar command
    tar_excludes=()

    # How long to you want to keep your backups (in days)
    backup_retention="+7"

    # Commands that are executed before the backup started
    pre_commands=()

    # Commands that are executed after the backup is completed
    post_commands=()

#### Cronjob setup

To make sure the backup is executed automatically and recurring, we're going to add a simple cronjob:

`vi /etc/cron.d/backup-liveconfig`

    #
    # cronjob to backup LiveConfig, daily at 5:00 am
    #

    0 5 * * *        root       /usr/local/bin/multibackup &>/dev/null

### Version
1.2.0

### Lincense
[MIT](LICENSE)
