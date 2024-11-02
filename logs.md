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
sudo mkdir -p /var/log/custom
sudo touch /var/log/custom/dhcp.log
sudo touch /var/log/custom/mail.log
sudo touch /var/log/custom/dump.log
```
```shell
sudo chown -R root:adm /var/log/custom
sudo chmod -R 755 /var/log/custom
sudo chmod 755 /var/log/custom/dhcp.log
sudo chmod 755 /var/log/custom/mail.log
sudo chmod 755 /var/log/custom/dump.log
```
2. Edit rsyslog file
```shell
sudo nano /etc/rsyslog.d
```
Than text into this file next configs:
```shell
:programname, isequal, "dhcpd" /var/log/custom/dhcp.log
:programname, isequal, "mail" /var/log/custom/mail.log
*.*;auth,authpriv.none,mail.none,cron.none /var/log/custom/dump.log
```
3. Restart rsyslog
```shell
systemctl restart rsyslog
```
4. Create test log
```shell
logger -p local0.info -t dhcpd "Test DHCP log message"
logger -p mail.info -t postfix "Test Mail log message"
```
4. Check logs
```shell
tail -f /var/log/custom/dhcp.log
tail -f /var/log/custom/mail.log
```

## External links
[debian.org - connection](https://www.debian.org/)
[ubuntu.com - connection](https://ubuntu.com/server/docs)


