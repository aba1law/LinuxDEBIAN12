# Log configuraion on Debian 12 with rsyslog

## Requirements

- dhcp logs in /var/log/custom/dhcp.log file
- mail logs in /var/log/custom/mail.log file
- other logs in /var/log/custom/dump.log file

## Preparation

1. Install rsyslog
```shell
sudo apt update && sudo apt upgrade -y
sudo apt install -y rsyslog
```

## Configuration

1. Create files for logs
```shell
cd /var/log
mkdir custom
```
```shell
cd /var/log/custom
mkdir dhcp.log
mkdir mail.log
mkdir dump.log
```
2. Edit rsyslog file
```shell
sudo nano /etc/rsyslog.conf
```
Than text into this file next configs:
```shell
:programname, isequal, "isc-dhcp-server" /var/log/dhcp.logs
:programname, isequal, "postfix" /var/log/mail.logs
:programname, !isequal, "dhcpd" :programname, !isequal, "postfix" /var/log/dump.logs
```
3. Restart rsyslog
```shell
systemctl restart rsyslog
```
4. Check logs
```shell
ls -l /var/log/dhcp.logs
ls -l /var/log/mail.logs
ls -l /var/log/dump.logs
```

## External links
[debian.org - connection](https://www.debian.org/)
[ubuntu.com - connection](https://ubuntu.com/server/docs)


