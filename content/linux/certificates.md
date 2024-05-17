+++
title = 'Certificates'
date = 2024-05-17T15:09:03+02:00
draft = false
+++

## Add CA to linux

```
cp cert.crt /usr/local/share/ca-certificates/
update-ca-certificates
```

If result 

```
Updating certificates in /etc/ssl/certs...
rehash: warning: skipping ca-certificates.crt,it does not contain exactly one certificate or CRL
1 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
```

Good Job, else check cert format (crt) or something 
