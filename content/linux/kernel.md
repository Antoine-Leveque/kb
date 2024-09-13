+++
title = 'Kernel'
date = 2024-09-13T10:06:57+02:00
draft = false
+++

# How to change used Kernel

## Verify kernel & headers install 

```bash
dpkg --list | grep linux-headers
```

result : 
```bash
ii  linux-headers-5.15.0-119         5.15.0-119.129                          all          Header files related to Linux kernel version 5.15.0
ii  linux-headers-5.15.0-119-generic 5.15.0-119.129                          amd64        Linux kernel headers for version 5.15.0 on 64 bit x86 SMP
ii  linux-headers-5.15.0-121         5.15.0-121.131                          all          Header files related to Linux kernel version 5.15.0
ii  linux-headers-5.15.0-121-generic 5.15.0-121.131                          amd64        Linux kernel headers for version 5.15.0 on 64 bit x86 SMP
ii  linux-headers-5.15.0-88          5.15.0-88.98                            all          Header files related to Linux kernel version 5.15.0
hi  linux-headers-5.15.0-88-generic  5.15.0-88.98                            amd64        Linux kernel headers for version 5.15.0 on 64 bit x86 SMP
ii  linux-headers-generic            5.15.0.121.121                          amd64        Generic Linux kernel headers
ii  linux-headers-virtual            5.15.0.121.121                          amd64        Virtual Linux kernel headers
ii  linux-virtual                    5.15.0.121.121                          amd64        Minimal Generic Linux kernel and headers
```

And install the desired one

```bash
apt install linux-headers-5.15.0-121-Generic
```

Same for kernel, with :

```bash
dpkg --list linux-image
```

## Update grub configuration

First, check grub order with : 

```bash
grub-mkconfig | grep -iE "menuentry 'Ubuntu, with Linux" | awk '{print i++ " : "$1, $2, $3, $4, $5, $6, $7}'
```

result
```bash
0 : menuentry 'Ubuntu, with Linux 5.15.0-121-generic' --class ubuntu
1 : menuentry 'Ubuntu, with Linux 5.15.0-121-generic (recovery mode)'
2 : menuentry 'Ubuntu, with Linux 5.15.0-119-generic' --class ubuntu
3 : menuentry 'Ubuntu, with Linux 5.15.0-119-generic (recovery mode)'
4 : menuentry 'Ubuntu, with Linux 5.15.0-88-generic' --class ubuntu
5 : menuentry 'Ubuntu, with Linux 5.15.0-88-generic (recovery mode)'
```

In this case, we will change from 5.15.0.121 to 5.15.0.88. 

Edit `/etc/default/grub` and update `GRUB_DEFAULT` value with `1>4` : Change from fourth entry of submenu were 0 is the main entry of menuentry

run `sudo update-grub` and reboot.

Check with `uname -r`
