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

### Privileges

| Privilege 	| Description |
| :---: | :---: |
|SELECT |	Ability to perform SELECT statements on the table. |
|INSERT |	Ability to perform INSERT statements on the table. |
|UPDATE |	Ability to perform UPDATE statements on the table.| 
|DELETE |	Ability to perform DELETE statements on the table.|
|INDEX 	|Ability to create an index on an existing table.|
|CREATE |	Ability to perform CREATE TABLE statements.|
|ALTER 	| Ability to perform ALTER TABLE statements to change the table definition.|
|DROP 	| Ability to perform DROP TABLE statements.|
|GRANT  | OPTION 	Allows you to grant the privileges that you possess to other users. |
|ALL 	| Grants all permissions except GRANT OPTION.|

#### Add

```sql
GRANT ${PRIVILEGES} ON `db`.`table` TO `user`@`host`;
FLUSH PRIVILEGES;
```
#### Remove

```sql
REVOKE ${PRIVILEGES} ON `db`.`table` FROM `user`@`host`;
FLUSH PRIVILEGES;
```
