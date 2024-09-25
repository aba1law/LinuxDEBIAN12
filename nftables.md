# Configuring nftables on debian12

## Requirements

This manual is tested on Debian 12

### Allow ports 
- TCP 22 (ssh)
- UDP 53 (dns)
- TCP 80,443 (http, https)

## Preparation
Make sure that you turned on ip forwarding and gived ip addresses to all interfaces you need 

1. Open file for IP forward
```shell
sudo sysctl -w net.ipv4.ip_forward=1
```
2. Check
```shell
sudo sysctll -p
```

# Installation & Configuration

First of all you need to install nftables on your device

1. Update your system
```shell
sudo apt update && sudo apt upgrade -y
```
2. Install packages
```shell
sudo apt install nftables -y
```
3. Deny everything we don't need, before you do this commands make sure you are not connected via ssh, if you connected via ssh for first allow ssh traffic
```shell
sudo nft add table inet filter
sudo nft add chain inet filter input { type filter hook input priority 0 \; policy drop \; }
sudo nft add chain inet filter forward { type filter hook forward priority 0 \; policy drop \; }
sudo nft add chain inet filter output { type filter hook output priority 0 \; policy accept \; }
```
4. Allow ssh, dns, http, https, 
```shell
sudo nft add rule inet filter input iifname "lo" accept
sudo nft add rule inet filter input tcp dport 22 accept
sudo nft add rule inet filter input icmp type echo-request accept
sudo nft add rule inet filter input udp dport 53 accept  
sudo nft add rule inet filter input tcp dport 80 accept  
sudo nft add rule inet filter input tcp dport 443 accept 
```
5. Allow lan addresses
```shell
sudo nft add table ip nat
sudo nft add chain ip nat postrouting { type nat hook postrouting priority 100 \; policy accept \; }
sudo nft add rule ip nat postrouting ip saddr 10.0.0.0/24 oifname "ens3" masquerade
sudo nft add rule inet filter forward ip saddr 10.0.0.0/24 oifname "ens3" accept
sudo nft add rule inet filter forward ip daddr 10.0.0.0/24 iifname "ens3" accept
```
6. Save configurations
```shell
sudo nft list ruleset > /etc/nftables.conf
sudo systemctl enable nftables
sudo systemctl restart nftables
```

## More commands
### If you wanna check configs
```shell
sudo nft list ruleset
```

## External links
[debian.org - connection](https://www.debian.org/)
[ubuntu.com - connection](https://ubuntu.com/server/docs)
