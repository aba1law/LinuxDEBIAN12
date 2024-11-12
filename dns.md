```shell
sudo apt update
```
```shell
sudo apt install bind9 dnsutils
```
```shell
sudo nano /etc/bind/named.conf.options
```
```shell
acl "trusted" {
    10.0.0.0/16;
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
```shell
sudo nano /etc/bind/named.conf.local
```
```shell
zone "itnet.kz" {
    type master;
    file "/etc/bind/db.itnet.kz";
};
zone "0.10.in-addr.arpa" {
    type master;
    file "/var/lib/bind/db.reverse";
};
```
```shell
cd /etc/bind
sudo cp /etc/bind/db.local /etc/bind/db.itnet.kz
cp db.127 /var/lib/bind/db.reverse
```
```shell
sudo nano /etc/bind/db.itnet.kz
```
```shell
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     srv1.lab.local. admin.lab.local. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      srv1.lab.local.
srv1    IN      A       172.16.0.6
srv2    IN      A       172.16.0.7
```
```shell
named-checkzone lab.local /var/lib/bind/db.lab.local
```
```shell
nano /var/lib/bind/db.reverse
```
```shell
;
; BIND reverse data file for local loopback interface
;
$TTL    604800
@       IN      SOA     srv1.lab.local. admin.lab.local. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      srv1.lab.local.
6.0     IN      PTR     srv1.lab.local.
7.0     IN      PTR     srv2.lab.local.
```
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
nameserver 10.0.2.2
domain itnet.kz
```
```shell
systemctl restart networking
```
