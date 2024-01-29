+++
title = 'Installation'
date = 2024-01-29T08:57:28+01:00
draft = false
+++

### Keyboard layout

```bash
loadkey fr-latin1
```

### Wifi setup 

```bash
iwctl 
station wlan0 scan
station wlan0 get-networks
station wlan0 connect ${SSID}
```


### Disk 
#### EFI Partition

```bash
Command (m for help): n
Partition number: <Press Enter>
First sector: <Press Enter>
Last sector, +/-sectors or +/-size{K,M,G,T,P}: +100M
Command (m for help): t
Partition type or alias (type L to list all): uefi
```

#### Boot Partition 

```bash
Command (m for help): n
Partition number: <Press Enter>
First sector: <Press Enter>
Last sector, +/-sectors or +/-size{K,M,G,T,P}: +1024M
Command (m for help): t
Partition type or alias (type L to list all): linux
```

#### LUKS Partition

```bash
Command (m for help): n
Partition number: <Press Enter>
First sector: <Press Enter>
Last sector, +/-sectors or +/-size{K,M,G,T,P}: <Press Enter>
Command (m for help): t
Partition type or alias (type L to list all): linux
```

#### Check partition table and write change 

```bash 
Command (m for help): p
```
```bash
Command (m for help): w
```

#### Format EFI and Boot 

```bash
mkfs.fat -F 32 /dev/${efi-disk}
mkfs.ext4 /dev/${boot-disk}
```

#### Setup LUKS

```bash
cryptsetup --use-random luksFormat /dev/${luks-disk}
cryptsetup luksOpen /dev/${luks-disk} cryptlvm
```

#### LVM

```bash
lvcreate --size 8G vg0 --name swap
lvcreate --size 100G vg0 --name root
lvcreate -l +100%FREE vg0 --name home
lvreduce --size -256M vg0/home
```

#### Format LV

```bash
mkswap /dev/vg0/swap
mkfs.ext4 /dev/vg0/root
mkfs.ext4 /dev/vg0/home
```

#### Mount new FS

```bash
mount /dev/vg0/root /mnt
mount --mkdir /dev/<your-disk-efi> /mnt/efi
mount --mkdir /dev/<your-disk-boot> /mnt/boot
mount --mkdir /dev/vg0/home /mnt/home
swapon /dev/vg0/swap
```

### Install base system

```bash
pacstrap -K /mnt base linux linux-firmware openssh git vim sudo
```

### Generate fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

### Chroot

```bash
arch-chroot /mnt 
```

### Timezone

```bash
# See available timezones:
ls /usr/share/zoneinfo/

# Set timezone:
ln -s /usr/share/zoneinfo/Europe/Paris /etc/localtime
hwclock --systohc
```

### Locale

```bash
vim /etc/locale.gen (uncomment en_US.UTF-8 UTF-8)
locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

### Hostname

```bash
echo yourhostname > /etc/hostname
```

### Create user 

```bash
useradd -m -G wheel --shell /bin/bash ${user}
passwd ${user}
visudo
# ---> Uncomment "%wheel ALL=(ALL) ALL"
```

### mkinit 

```bash
pacman -S lvm2
vim /etc/mkinitcpio.conf
# ---> Add 'encrypt' and 'lvm2' to HOOKS before 'filesystems'
```

```bash
mkinitcpio -P
```

### GRUB

```bash
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB
```

in `/etc/default/grub`

```bash
GRUB_CMDLINE_LINUX="cryptdevice=/dev/<your-disk-luks>:cryptlvm root=/dev/vg0/root"
```

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

### NetworkManager

```bash
pacman -S networkmanager
systemctl enable NetworkManager
```

### Exit chroot

```bash
exit
umount -R /mnt
swapoff -a
```

```bash
reboot
```



