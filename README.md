# -Automated-backup-with-mongodump-and-cron-jobs

This is a small script to generate automations from bash, using cron jobs.



**1. First, create your script using nano.** 

`nano bscript.sh`

The nano editor is now open, so it's time to write your script!

```bash
#!/bin/bash

# Configuration
TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
BACKUP_DIR="/your/dir/tobackup"
DB_URI="mongodb://localhost:27017"
DB_NAME="Urdbname"

# Create a dir if doesn't exist
mkdir -p "$BACKUP_DIR" || {
    echo "Error: Can't create the dir $BACKUP_DIR" >> /Path/tolog/name.log
    exit 1
}

# Validate backup
if /usr/local/bin/mongodump --uri="$DB_URI" --db="$DB_NAME" --out="$BACKUP_DIR/$DB_NAME-$TIMESTAMP"; then
    echo "Backup completed: $BACKUP_DIR/$DB_NAME-$TIMESTAMP"
else
    echo "Error: mongodump failed to run" >> /Path/tolog/name.log
    exit 1
fi

# Clean old backups (optional, delete backups older than 7 days)
find "$BACKUP_DIR" -type d -mtime +7 -exec rm -rf {} +

# Finished
echo "Backup process completed successfully"
```

This script includes error validation to handle potential issues.

Make sure to adapt the paths according to your preferences. In this example, I have specified the explicit path to `mongodump`.

**Save and close the NANO editor, then grant execution permissions to your script.**

`chmod +x /route/to/script.sh`

Create a log for the cron job 🤌🏻 *Cron can create it automatically if it has the necessary permissions.*

Open the cron editor. Cron uses the VI text editor by default, so use the appropriate commands.

`crontab -e` 

Add a line to schedule the script execution and set its time interval.

`0 */3 * * * /path/to/script.sh >> /path/to/file.log 2> &1`
Save.

You can run the script manually to test its functionality.

`./script.sh`

Finally, check the path where the backups are saved to verify if new entries have been created.

`ls -l /your/dir/tobackup`
