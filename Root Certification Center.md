# Certification center on debian 12

## Preparation

This manual is tested on Debian 12

- Install packages
- Check network configs
- Setup Cert center

## Installation

###For first install packages
```shell
sudo apt update && sudo apt upgrade
sudo apt install easy-rsa
```

## Setup

### File structure configuration
```shell
mkdir /ca
```
### Create the symlinks with the ln command
```shell
ln -s /usr/share/easy-rsa/* /ca/
```
### Restrict access to your new PKI directory
```shel
chmod 700 /ca
```
### Initialize the PKI
```shell
cd /ca
```
```shell
./easyrsa init-pki
```
You must see 
```shell
init-pki complete; you may now create a CA or requests.
Your newly created PKI dir is: /ca/pki
```
### Create vars file
```shell
cd /ca
nano vars
```
```shell
set_var EASYRSA_REQ_COUNTRY    "KZ"
set_var EASYRSA_REQ_ORG        "IT Net Kz Inc."
set_var EASYRSA_REQ_CN         "itnet.kz"
```
```shell
./easyrsa build-ca
```
Output:
```shell
Enter New CA Key Passphrase:
Re-Enter New CA Key Passphrase:
. . .
Common Name (eg: your user, host, or server name) [Easy-RSA CA]:

CA creation complete and you may now import and sign cert requests.
Your new CA certificate file for publishing is at:
/ca/pki/ca.crt
```
### Check cert
```shell
cat /ca/pki/ca.crt
```

## External links
[debian.org - connection](https://www.debian.org/)
[ubuntu.com - connection](https://ubuntu.com/server/docs)
[digitalocean.com - connection]([https://ubuntu.com/server/docs](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-a-certificate-authority-ca-on-debian-11#step-3-creating-a-certificate-authority))
