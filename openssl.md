# OpenSSL

### Creating Private Key and Certification

```bash
openssl req -x509 -newkey rsa:4096 -keyout private.key -out name.cert -days 365
```

### Creating Public Key

```bash
openssl rsa -in private.key -pubout -out public.pem
```

### Sign a file

```bash
openssl dgst -sha256 -sign private.key -out filename.sig filename.txt
```

### Verify a signature

```bash
openssl dgst -sha256 -verify someones_public_key.pem -signature signature.sig someones_public_key.pem
```

## AES

### Sending AES key via asymmetric encryption

Assumes that aes.txt has AES key in it

Takes plaintext aes.txt key and encrypts it using someones_public_key
```bash
openssl rsautl -encrypt -pubin -inkey someones_public_key.pem -in aes.txt -out aes_key_encrypted.enc.txt
```

### Decrypting an AES key via asymetric encryption
Decrypts a key that was encrypted using our public key (we are decrypting with our private)
```bash
openssl rsautl -decrypt -inkey my_private_key.key -in aes_key_encrypted.enc.txt > aes.txt
```

### Encrypting
Assuming that aes.txt has aes key in it
```bash
openssl aes-256-cbc -a -salt -in message.txt -out message.enc.txt -kfile aes.txt
```

### Decrypting
Assuming that aes.txt has aes key in it
```bash
openssl aes-256-cbc -d -a -in message_from_sender.enc.txt -out message_from_sender.txt -kfile aes.txt
```


