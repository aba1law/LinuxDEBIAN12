# Configure DNS server on debian 12

## Preparation

- Install packages
- Configure server

### Install
```shell
sudo apt update
```
```shell
sudo apt install bind9 dnsutils
```
### Edit file with dns settings
```shell
sudo nano /etc/bind/named.conf.options
```
```shell
acl "trusted" {
    10.0.0.0/8;
    localhost;
    localnets;
};

options {
    directory "/var/cache/bind";

    recursion yes;
    allow-query { trusted; };

    forwarders {
        8.8.8.8;
        8.8.4.4;
    };

    dnssec-validation no;

    listen-on-v6 { any; };
    listen-on port 53 { 127.0.0.1; 10.0.1.2; };
};
```
### Create zones
```shell
sudo nano /etc/bind/named.conf.local
```
```shell
zone "int.worldskills.org" {
    type master;
    file "/etc/bind/db.int.worldskills.org";
};
zone "0.10.in-addr.arpa" {
    type master;
    file "/var/lib/bind/db.reverse";
};
```
### Copy content in old files
```shell
cd /etc/bind
sudo cp /etc/bind/db.local /etc/bind/db.int.worldskills.org
cp db.127 /var/lib/bind/db.reverse
```
### Add records in zones
```shell
sudo nano /etc/bind/db.int.worldskills.org
```
```shell
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     srv01.int.worldskills.org. admin.int.worldskills.org. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      srv01.int.worldskills.org.
srv1    IN      A       10.1.10.2
```
```shell
sudo named-checkzone int.worldskills.org /var/lib/bind/db.int.worldskills.org
```
```shell
nano /var/lib/bind/db.reverse
```
```shell
;
; BIND reverse data file for local loopback interface
;
$TTL    604800
@       IN      SOA     srv01.int.worldskills.org. admin.int.worldskills.org. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      srv01.int.worldskills.org.
2.10    IN      PTR     srv01.int.worldskills.org.
```
### Check
```shell
named-checkzone 0.10.in-addr.arpa /var/lib/bind/db.reverse
```
```shell
systemctl restart bind9
```
```shell
systemctl status bind9
```
```shell
nano /etc/resolv.conf
```
```shell
nameserver 10.1.10.2
domain int.worldskill.org
```
```shell
systemctl restart networking
```

## External links
[debian.org - connection](https://www.debian.org/)
[ubuntu.com - connection](https://ubuntu.com/server/docs)
