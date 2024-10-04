# NTP server configuration on Debian 12

## Requirements

- We must use stratum 2 servers for synchronization time on main NTP server
- We have second NTP server in another office
- Second NTP server must synchronize time from main NTP server
- Configure clients from each office

## Preparation
Install packages, set ip on devices 

1. IP Check
```shell
ip a
```
2. Installation
```shell
sudo apt update 
sudo apt install -y ntpsec
```

# Configuration main NTP server

1. Set timezone
```shell
sudo timedatectl set-timezone Asia/Almaty
```
2. Open file "/etc/ntpsec/ntp.conf"
```shell
sudo nano /etc/ntpsec/ntp.conf
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
sudo systemctl restart ntpsec
```

# Configuration second NTP server

1. Set timezone
```shell
sudo timedatectl set-timezone Asia/Almaty
```
2. Open file "/etc/ntpsec/ntp.conf"
```shell
sudo nano /etc/ntpsec/ntp.conf
```
Than text stratum 2 servers command
```shell
server (ip of main server) iburst
```
3. Allow access for lan
```shell
allow (your subnet) 
```
4. Restart service
```shell
sudo systemctl restart ntpsec
```

# Configuration for client devices

## Preparation
Set ip on devices 

1. IP Check
```shell
ip a
```

# Configuration clients

1. Set timezone
```shell
sudo timedatectl set-timezone Asia/Almaty
```
2. Open file "/etc/systemd/timesync.conf"
```shell
sudo nano /etc/systemd/timesync.conf
```
Than text stratum NTP servers ip
```shell
NTP=(your ntp server ip)
```
3. Restart service
```shell
sudo systemctl restart systemd-timesyncd
```
4. Check result
```shell
timedatectl timesync-status
```

## External links
[debian.org - connection](https://www.debian.org/)
[ubuntu.com - connection](https://ubuntu.com/server/docs)
[server-world.info - connection](https://www.server-world.info/en/note?os=Debian_12&p=ntp&f=1)
[server-world.info - connection](https://www.server-world.info/en/note?os=Debian_12&p=ntp&f=3)
