# Debian 12 GUI installation

## Preparation

- For first we must install packages

## Installation
1. Install tasksel
```shell
apt update && apt upgrade -y
apt install -y tasksel
```
2. Install gnome
```shell
tasksel install gnome-desktop --new-install
```
3. We must add user because we cant use root
```shell
adduser user
```
4. Reboot system and check result
```shell
reboot
```

## External links
[debian.org - connection](https://www.debian.org/)
[ubuntu.com - connection](https://ubuntu.com/server/docs)
[help.reg.ru - connection](https://help.reg.ru/support/servery-vps/oblachnyye-servery/ustanovka-programmnogo-obespecheniya/kak-ustanovit-graficheskuyu-obolochku-gnome-v-debian)
