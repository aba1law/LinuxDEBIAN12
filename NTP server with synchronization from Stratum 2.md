# NTP server configuration on Debian 12

## Requirements

We must use stratum 2 servers for synchronization time

## Preparation
Install packages, set ip on devices 

1. IP Check
```shell
ip a
```
2. Installation
```shell
sudo apt update 
sudo apt install -y chrony
```

# Configuration

1. Set timezone
```shell
sudo timedatectl set-timezone Asia/Almaty
```
2. Open file "/etc/chrony/chrony.conf"
```shell
sudo nano /etc/chrony/chrony.conf
```
Than text stratum 2 servers command
```shell
server 2.kz.pool.ntp.org iburst
server 3.kz.pool.ntp.org iburst
```
3. Allow access for lan
```shell
allow (your subnet) 
```
4. Restart service
```shell
sudo systemctl restart chronyd
```

# Configuration for client devices

## Preparation
Install packages, set ip on devices 

1. IP Check
```shell
ip a
```
2. Installation
```shell
sudo apt update 
sudo apt install -y chrony
```

# Configuration

1. Set timezone
```shell
sudo timedatectl set-timezone Asia/Almaty
```
2. Open file "/etc/chrony/chrony.conf"
```shell
sudo nano /etc/chrony/chrony.conf
```
Than text stratum 2 servers command
```shell
server (your ntp server ip) iburst
```
3. Restart service
```shell
sudo systemctl restart chronyd
```
4. Check result
```shell
chronyc sources
chronyc tracking
```

## External links
[debian.org - connection](https://www.debian.org/)
[ubuntu.com - connection](https://ubuntu.com/server/docs)
