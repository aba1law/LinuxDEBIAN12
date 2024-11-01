```shell
apt update && upgrade -y
```
```shell
apt install -y isc-dhcp-server
```
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
  option domain-name-servers 8.8.8.8, 8.8.4.4;
  default-lease-time 600;
  max-lease-time 7200;
}
```
```shell
sudo systemctl start isc-dhcp-server
sudo systemctl enable isc-dhcp-server
```
```shell
sudo systemctl status isc-dhcp-server
```
