+++
title = 'Users Management'
date = 2024-01-24T11:41:37+01:00
draft = false
+++

### Create user

```mysql
CREATE USER 'user'@'host' IDENTIFIED BY 'password';
```

Use `'%'` to match any host 

### Update user host configuration 

```mysql
UPDATE mysql.user SET Host='${new_host}' WHERE Host='${old_host}' AND User='user';
UPDATE mysql.db SET Host='${new_host}' WHERE Host='${old_host}' AND User='user';
FLUSH PRIVILEGES;
```

### Show connected user

```mysql
SHOW PROCESSLIST;
```
