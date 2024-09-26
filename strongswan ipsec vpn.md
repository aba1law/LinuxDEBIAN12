# Site-to-Site vpn with Strongswan configuration

## Manual tested on Debian 12

VPN between two firewalls for connection subnets

### Preparation
Make sure that you deploy firewalls and your lan devices already have access to internet, if you configure iptables/nftables on your firewall you must to open ports 4500, 500 for vpn.

### Installation
1. Need to install packages 
```shell
sudo apt update && sudo apt upgrade -y
apt-get install strongswan
```

### Configuration
1. Open folder
```shell
sudo nano /etc/ipsec.conf
```
2. Comment everything in folder and text this configs
```shell
config setup
        charondebug="all"
        strictcrlpolicy=no
        uniqueids = yes

conn %default
conn tunnel
        left=(localaddress)
        leftsubnet=(lansubnet)
        right=(second_firwall_ip)
        rightsubnet=(second_firewall_lan_ip)
        ike=aes256-sha256-ecp256
        esp=aes256-sha256!
        keyingtries=0
        ikelifetime=1h
        lifetime=8h
        dpddelay=30
        dpdtimeout=120
        dpdaction=clear
        authby=secret
        auto=start
        keyexchange=ikev2
        type=tunnel
```
"Ctrl + x -> Y" for save
3. Open folder
```shell
nano /etc/ipsec.secrets
```
4. Text in the folder next settings:
1stfw_ip 2ndfw_ip : PSK 'yourkey'
5. Add static routes
```shell
ip route add 0.0.0.0/0 via "nextfw_ip" dev ens3
```
6. Turn on ip forwarding
```shell
sudo sysctl -w net.ipv4.ip_forward=1
```
7. Restart and check services
```shell
systemctl restart strongswan-starter.service
systemctl restart ipsec
systemctl status ipsec
```

## External links
[debian.org - connection](https://www.debian.org/)
[ubuntu.com - connection](https://ubuntu.com/server/docs)
