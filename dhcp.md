# Dhcp server on debian 12

## Preparation

- Install packages
- Check network configs
- Setup dhcp server

## Installation

### Packages 
```shell
apt update && upgrade -y
```
```shell
apt install -y isc-dhcp-server
```
### Configure files 
```shell
nano /etc/default/isc-dhcp-server
```
```shell
INTERFACESv4="eth0"
```
```shell
nano /etc/dhcp/dhcpd.conf
```
```shell
subnet 10.0.1.0 netmask 255.255.255.0 {
  range 10.0.1.10 10.0.1.100;
  option routers 10.0.1.1;
  option domain-name-servers 10.0.2.2;
  default-lease-time 600;
  max-lease-time 7200;
}
```
### Restart and test services
```shell
sudo systemctl start isc-dhcp-server
sudo systemctl enable isc-dhcp-server
```
```shell
sudo systemctl status isc-dhcp-server
```

## External links
[debian.org - connection](https://www.debian.org/)
[ubuntu.com - connection](https://ubuntu.com/server/docs)
