# mysql-backup

Container that backs up MySQL databases to S3. The container overwrites the same S3 object every time it is run, it is recommended you turn
versioning on to retain access to previous copies.

There is an existing image available on the public registry at [iainmckay/mysql-backup](https://registry.hub.docker.com/u/iainmckay/mysql-backup/).

## Backing up

To backup run:

    $ docker run -e AWS_ACCESS_KEY_ID=key -e AWS_SECRET_ACCESS_KEY=secret -e AWS_DEFAULT_REGION=eu-west-1 -e MYSQL_HOST=127.0.0.1 -e MYSQL_USER=root -e MYSQL_PASSWORD=password iainmckay/mysql-backup backup

You can provide an extra argument with a specific database to backup.

## Restoring

To restore an existing backup run:

    $ docker run -e AWS_ACCESS_KEY_ID="key" -e AWS_SECRET_ACCESS_KEY="secret" -e AWS_DEFAULT_REGION="eu-west-1" -e MYSQL_HOST=127.0.0.1 -e MYSQL_USER=root -e MYSQL_PASSWORD=password iainmckay/mysql-backup restore

It is important to note that if this database already exists on your server, this process will drop it first. You can also provide an extra argument with a specific database to restore.

## Excluding Databases

You can exclude databases from backup/restore by using --exclude.

For example:

	$ docker run -e AWS_ACCESS_KEY_ID="key" -e AWS_SECRET_ACCESS_KEY="secret" -e AWS_DEFAULT_REGION="eu-west-1" -e MYSQL_HOST=127.0.0.1 -e MYSQL_USER=root -e MYSQL_PASSWORD=password iainmckay/mysql-backup --exclude=some_database,another_database restore

## Configuration 

The container can be customized with these environment variables:

Name | Default Value | Description
--- | --- | ---
AWS_ACCESS_KEY_ID | `blank` | Your AWS access key with access to the desired bucket
AWS_SECRET_ACCESS_KEY | `blank` | Your AWS secret access key with access to the desired bucket
AWS_DEFAULT_REGION | `blank` | The AWS region your bucket is in
MYSQL_HOST | 127.0.0.1 | Address the MySQL server is accessible at
MYSQL_PORT | 3306 | Port the MySQL server is accessible on
MYSQL_USER | root | User to connect as
MYSQL_PASSWORD | `blank` | Password to use when connecting
RESTORE_DB_CHARSET | utf8 | Which charset to recreate the database with
RESTORE_DB_COLLATION | utf8_bin | Which collation to recreate the database with
S3_BUCKET | `blank` | The bucket to write the backup to
S3_PATH | mysql | The path to write the backups to
