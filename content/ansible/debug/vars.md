+++
title = 'Variables'
date = 2024-01-26T16:27:44+01:00
draft = false
+++


### Testing vars

```bash
ansible {SRV_NAME} -m debug -a "var=hostvars[inventory_hostname]" -i inventory.yaml 
```
