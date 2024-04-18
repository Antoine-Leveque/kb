+++
title = 'Cluster'
date = 2024-02-16T10:57:19+01:00
draft = false
+++

### Cluster size

```mysql
SHOW GLOBAL STATUS LIKE 'wsrep_cluster_size';
```
```
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| wsrep_cluster_size | 2     |
+--------------------+-------+
```

### Cluster status

```mysql
SHOW GLOBAL STATUS LIKE 'wsrep_cluster_status';

```
```
+----------------------+---------+
| Variable_name        | Value   |
+----------------------+---------+
| wsrep_cluster_status | Primary |
+----------------------+---------+
```


### Node status 

```mysql
SHOW GLOBAL STATUS LIKE 'wsrep_ready';

```
```
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| wsrep_ready   | ON    |
+---------------+-------+
```

```mysql
SHOW GLOBAL STATUS LIKE 'wsrep_connected';

```
```
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| wsrep_connected | ON    |
+-----------------+-------+
```

```mysql
SHOW GLOBAL STATUS LIKE 'wsrep_local_state_comment';

```
```
+---------------------------+--------+
| Variable_name             | Value  |
+---------------------------+--------+
| wsrep_local_state_comment | Joined |
+---------------------------+--------+
```
