+++
title = 'Backup'
date = 2024-04-24T15:03:00+02:00
draft = false
+++

# USER FOR DUMP

```mysql
CREATE USER 'dumper'@'X.X.X.X' IDENTIFIED BY 'PASSWORD';
GRANT SELECT,LOCK TABLES ON DB.* TO 'dumper'@'X.X.X.X';
```
# DUMP 
```mysql
mysqldump -h ${HOST} -u ${USER} -p ${DB} --add-drop-database --skip-add-locks --skip-lock-tables --single-transaction > /data/dump.sql
```
