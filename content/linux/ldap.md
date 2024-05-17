+++
title = 'Ldap'
date = 2024-05-03T09:36:18+02:00
draft = false
+++

## Example of LDAPSearch command

```bash
ldapsearch -x -H ldap{s}://X.X.X.X:{389|636} -D "${USER}@dc.example.local" -W -b "dc=dc,dc=example,dc=local" cn
```
