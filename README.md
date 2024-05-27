# postgre-mysql-s3-backup
Backup of PostgreSQL and MYSQL databases to S3 or Local.

**Prerequest**
sudo apt-get install python3 python3-pip postgresql-client mysql-client
pip install slack_sdk psycopg2-binary boto3 requests argparse mysql-connector-python
Note : In MySQL 8.0, caching_sha2_password is the default authentication plugin rather than mysql_native_password

**Usage Information**

This script was written to retrieve backups to Local or Amazon S3 and restore the most current or specified backups from these locations.
To take S3 backups, BACKUP_TYPE=S3 is selected. If you want to take a backup to a local path, specify BACKUP_TYPE=LOCAL.
If you want to back up all databases in Postgre, you must define PG_DATABASE as all. If there are any you want to exclude when backing up all user databases, specify this in the EXCLUDED_DATABASES field. Run the following command for backup.
Available commands
python3 db-backup.py --backup
python3 db-backup.py --backup --specific-db test_dev_core

Database restore operations will not work if PG_DATABASE=all for security reasons. If a single database is specified, you can download the most current backup of this database as follows.
python3 db-backup.py --restore
This process finds the most current backup in S3 or local path and uploads it to the database specified in PG_DATABASE. If there are backups from more than one server in S3, it automatically selects the appropriate one among these backups.
If a special restore operation is to be performed, you can use the following command.
Available commands
python3 db-backup.py --restore --specific-backup core.servename.local_backup_restore_test_backup_2024-05-25_13-18-52.sql.tar.gz --specific-db backup_restore_test
python3 db-backup.py --restore --days 3 --specific-db test_dev_core
It automatically uploads a backup file older than the specified day to the specified database. If there is no backup older than the specified day, it loads the backup that best meets the criteria.
This process uploads a specific backup file to the special database specified in --specific-db.

Be careful when performing restoration operations!

Successful and unsuccessful operations of this script are notified to you via Slack.
