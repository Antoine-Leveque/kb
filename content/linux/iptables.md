+++
title = 'Iptables'
date = 2024-05-06T10:01:12+02:00
draft = false
+++

## List rules

```bash
iptables -S
```

## Add rules

```bash
iptables -A {INPUT/OUTPUT} -d/s {dest/src} -p {tcp/udp} -m {tcp/udp} --dport/sport {PORT} -j {ACCEPT/DENY}
```

