+++
title = 'Replication'
date = 2024-02-09T14:40:21+01:00
draft = false   
+++

### Official documentation 

- https://dev.mysql.com/doc/refman/8.0/en/replication-configuration.html

### Quick setup on 5.7

#### Replication configuration
Each replica must have unique server id differs from source or other replica
```mysql
SET GLOBAL server_id = 21;
```

File configuration 
```mysql
[mysqld]
server-id=21
log-bin=mysql-bin
```
You can't enabled log-bin on a running server

Create dedicated user for replication
```mysql
CREATE USER 'repl'@'%.example.com' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%.example.com';
```

Lock table with read statement
```mysql
FLUSH TABLES WITH READ LOCK;
```

Get the current binary log position
```mysql
SHOW MASTER STATUS\G
*************************** 1. row ***************************
             File: mysql-bin.000003
         Position: 73
     Binlog_Do_DB: test
 Binlog_Ignore_DB: manual, mysql
Executed_Gtid_Set: 3E11FA47-71CA-11E1-9E33-C80AA9429562:1-5
1 row in set (0.00 sec)
```

Dump database
```mysql
mysqldump --all-databases --master-data > dbdump.db
```

Unlock DB
```mysql
UNLOCK TABLES;
```

Restore DB on target replicat
```mysql
mysql -h source < fulldb.dump
```

Setting source configuration on replica server
```mysql
CHANGE MASTER TO MASTER_HOST='source_host_name', MASTER_USER='replication_user_name', MASTER_PASSWORD='replication_password', MASTER_LOG_FILE='recorded_log_file_name', MASTER_LOG_POS=recorded_log_position;
```

Get replication status
```mysql
SHOW SLAVE STATUS\G
```

Start & Stop slave
```mysql
STOP SLAVE;
START SLAVE;
```


