+++
title = 'System information'
date = 2024-02-09T15:11:11+01:00
draft = false
+++

### Get size of databases

```mysql

SELECT table_schema "DB Name",
        ROUND(SUM(data_length + index_length) / 1024 / 1024, 1) "DB Size in MB" 
FROM information_schema.tables 
GROUP BY table_schema; 
```
