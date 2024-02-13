+++
title = 'Slow queries'
date = 2024-02-13T15:24:40+01:00
draft = false
+++


### Check slow queries status

```mysql
show global variables like '%slow_query%';
```

### Enable slow queries

```mysql
set global slow_query_log = 'ON';
set global slow_query_log_file ='/var/log/mysql/slow-query.log';
SET @@GLOBAL.long_query_time = 10;
```

### Check long query configuration

```mysql
show global variables like '%long_query%';
```
