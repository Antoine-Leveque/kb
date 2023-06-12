# INSTALL

## disposition Clavier
```zsh
loadkey fr-latin1
```

## Connexion Wifi 

```zsh
iwctl 
station wlan0 scan
station wlan0 get-networks
station wlan0 connect ${SSID}
```

## timedate config

```zsh
timedatectl set-ntp true
timedatectl status
```

## Gestion de paquet

```zsh
pacman -Sy
Ajout de `Server = http://mirror.archlinux.ikoula.com/archlinux/$repo/os/$arch` à `/etc/pacman.d/mirrorlist`
```

