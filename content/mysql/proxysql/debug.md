+++
title = 'Debug'
date = 2024-01-31T11:11:03+01:00
draft = false
+++

### ProxySQL Error: connection is locked to hostgroup 11 but trying to reach hostgroup 10

```mysql
set mysql-set_query_lock_on_hostgroup=0;
load mysql variables to runtime;
save mysql variables to disk;
```

Restart proxysql service
