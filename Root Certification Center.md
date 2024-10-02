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
sudo apt install openssl
```

## Setup

### File structure configuration
```shell
sudo mkdir -p /ca/{certs,crl,newcerts,private}
sudo chmod 700 /ca/private
sudo touch /ca/index.txt
sudo echo 1000 > /ca/serial
```
### Open file "/ca/openssl.cnf"
```shell
sudo nano /ca/openssl.cnf
```
Than text this settings:
```shell
[ ca ]
default_ca = CA_default

[ CA_default ]
dir               = /ca
certs             = $dir/certs
crl_dir           = $dir/crl
database          = $dir/index.txt
new_certs_dir     = $dir/newcerts
certificate       = $dir/CA.crt
serial            = $dir/serial
private_key       = $dir/private/CA.key
RANDFILE          = $dir/private/.rand
default_days      = 3650
default_md        = sha256
preserve          = no
policy            = policy_match

[ policy_match ]
countryName             = match
organizationName        = match
organizationalUnitName  = optional
commonName              = supplied

[ req ]
default_bits        = 4096
distinguished_name  = req_distinguished_name
string_mask         = utf8only
default_md          = sha256
x509_extensions     = v3_ca

[ req_distinguished_name ]
countryName                 = Country Name (2 letter code)
countryName_default         = KZ
organizationName            = Organization Name (eg, company)
organizationName_default    = IT Net Kz Inc.
commonName                  = Common Name (eg, fully qualified host name)
commonName_default          = IT Net Kz Inc. Root CA

[ v3_ca ]
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:true
keyUsage = critical, digitalSignature, cRLSign, keyCertSign
```
### Create key, than cert
```shel
cd /ca
```
```shell
sudo openssl genrsa -out private/CA.key 4096
```
```shell
sudo openssl req -config openssl.cnf -key private/CA.key -new -x509 -days 3650 -sha256 -extensions v3_ca -out certs/CA.crt
```
### Create cert for server
```shell
sudo openssl genrsa -out /ca/private/server.key 4096
```
```shell
sudo openssl req -new -key /ca/private/server.key -out /ca/server.csr -subj "/C=KZ/O=IT Net Kz Inc./CN=your.domain.com"
```
```shell
sudo openssl ca -config /ca/openssl.cnf -in /ca/server.csr -out /ca/certs/server.crt -days 365 -batch
```
### Check cert
```shell
openssl verify -CAfile /ca/certs/CA.crt /ca/certs/server.crt
```

## External links
[debian.org - connection](https://www.debian.org/)
[ubuntu.com - connection](https://ubuntu.com/server/docs)
