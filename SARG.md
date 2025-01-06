# Squid Analysis Report Generator configuration

## Install packages
```shell
apt update && apt upgrade -y
apt -y install squid apache2 sarg net-tools
```
## Edit file /etc/sarg/sarg.conf
```shell
nano /etc/sarg/sarg.conf
```
--ctrl+w: output_dir /var/lib/sarg

than comment: 
```shell
output_dir /var/lib/sarg
```
uncomment:
```shell
#output_dir /var/www/html/squid-reports
```

--ctrl+w: resolve_ip /or/ ctrl+w: # TAG: resolve_ip yes/no

than add to 
"resolve_ip" -> yes: 
```shell
resolve_ip yes
```
--ctrl+w: charset Latin1

than add under "charset Latin1": 
```shell
charset UTF-8
```

## edit /etc/sarg/exclude_hosts
just add:
```shell
127.0.0.1
www.google.com
```

## edit /etc/apache2/conf-available/sarg.conf
just add:
```shell
<Directory "/var/www/html/squid-reports">
    Require local
    Require ip (this server ip)
</Directory>
```

## create dir
```shell
mkdir /var/www/html/squid-reports ; a2enconf sarg
```

## edit /etc/squid/squid.conf
--ctrl+w: acl CONNECT method CONNECT

than add under: 
```shell
acl lan src (this server ip)
```

--ctrl+w: # TAG: adapted_http_access

than add under "http_access deny all": 
```shell
http_access allow lan
```

--ctrl+w: # TAG: reply_header_access /or/ # No limits.

than add under "#No limits.": 
```shell
request_header_access Referer deny all
request_header_access X-Forwarded-For deny all
request_header_access Via deny all 
request_header_access Cache-Control deny all
```

--ctrl+w: # TAG: cachemgr_passwd /or/ # forwarded_for on

than add: 
```shell
forwarded_for off
```

## restart system
```shell
systemctl restart squid apache2 ; systemctl status squid 
```
```shell
cp -rp /usr/share/fonts/truetype/dejavu /usr/share/fonts/truetype/ttf-dejavu
```
```shell
Generate log reports: /usr/bin/sarg
```
```shell
sarg -x
```

## Test server
text to your browser: http://sarg_server_ip/squid.reports/



