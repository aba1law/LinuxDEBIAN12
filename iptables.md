# Configuring iptables on debian12

## Requirements

This manual is tested on Debian 12

### Allow ports 
- TCP 22 (ssh)
- UDP,TCP 53 (dns)
- TCP 80,443 (http, https)
- TCP 25,465,587 (smtp)
- TCP 143,993 (imap)
- TCP 110,995 (POP3)

### Make sure iptables persist across reboots.
Download iptables-persistent and save config

## Preparation
Make sure that you turned on ip forwarding and gived ip addresses to all interfaces you need 

1. Open file for IP forward
```shell
sudo nano /etc/sysctl.conf
```
Here uncomment "#net.ipv4.ip_forward=1"
Delete #
```shell
net.ipv4.ip_forward=1
```
2. Check ip addresses
```shell
ip a
```


# Installation & Configuration

First of all you need to install iptables on your device

1. Update your system
```shell
sudo apt update && sudo apt upgrade -y
```
2. Install packages
```shell
sudo apt install iptables iptables-persistent -y
```
3. Deny everything we don't need
```shell
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT
```
4. Allow local traffic
```shell
sudo iptables -A INPUT -i lo -j ACCEPT
```
5. Allow connections
```shell
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```
6. Allow ssh, dns, http, https, smtp, imap, pop3...
```shell
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 53 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 53 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 25 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 465 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 587 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 143 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 993 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 110 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 995 -j ACCEPT
```
7. Allow lan addresses
```shell
sudo iptables -t nat -A POSTROUTING -o ens3 -j MASQUERADE
sudo iptables -t nat -A POSTROUTING -s 192.168.0.0/16 -o ens3 -j MASQUERADE
sudo iptables -A INPUT -s 192.168.0.0/16 -j ACCEPT
sudo iptables -A FORWARD -s 192.168.0.0/16 -j ACCEPT
sudo iptables -A FORWARD -d 192.168.0.0/16 -j ACCEPT
```
8. Save configurations
```shell
sudo netfilter-persistent save
```

## More commands
### If you wanna check configs
```shell
sudo iptables -L -v
```
### If you wanna delete
For example delete ssh:
```shell
sudo iptables -D INPUT -p tcp --dport 22 -j ACCEPT
```
### If you wanna reset all configs
```shell
sudo iptables -F
```

## External links
[debian.org - connection](https://www.debian.org/)
[ubuntu.com - connection](https://ubuntu.com/server/docs)

