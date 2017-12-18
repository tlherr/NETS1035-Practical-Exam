# Certificates and Signing

## Dependencies

```bash
sudo apt-get install openvpn easy-rsa
```

## Setup CA Directory

```bash
make-cadir ~/openvpn-ca
cd ~/openvpn-ca
```

## Configure Vars

```bash
vim vars
```

Change KEY_COUNTRY, KEY_PROVINCE, KEY_CITY, KEY_ORG, KEY_EMAIL, KEY_OU

```bash
export KEY_NAME="server"
```
 Save and close when done editing
 
## Building CA
 
```bash
source vars
./clean-all
./build-ca
```

## Create the Server Certificate, Key, and Encryption Files

Name "server" should match KEY_NAME above
```bash
./build-key-server server
```

### Diffie Hellman

```bash
./build-dh
```

### HMAC Signature

```bash
openvpn --genkey --secret keys/ta.key
```

## Generate a Client Certificate and Key Pair

Name "client1" can be anything just make sure if changed here also changed below
```bash
cd ~/openvpn-ca
source vars
./build-key client1
```


## OpenVPN Reference
[Open VPN Reference](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-16-04)