# GPG Encryption

If you prefer a GUI:

```bash
apt-get install gnupg
```

to run:

```bash
gpa
```


## Generating a new keypair
The command-line option --gen-key is used to create a new primary keypair.

```bash
gpg --gen-key
```

## Listing Keys
```bash
gpg --list-keys
```
There will be a .gnupg folder in home directory containing pubring.gpg file

### Listing Private Keys
```bash
gpg --list-secret-keys
```

## Exporting a public key

Key Export in binary format
```bash
gpg --output alice.gpg --export alice@cyb.org
```

Key Exported in ASCII format (easier to work with)
```bash
gpg --armor --export alice@cyb.org > alice_pub.key
```

## Importing a public key
A public key may be added to your public keyring with the --import option.

```bash
gpg --import bob_pub.key
```

## Validation

Uses a trust model we must set levels for

```bash
gpg --edit-key blake@cyb.org

trust (invoke trust subcommand on the key)
5 (ultimate trust)
y (if prompted)
quit
```

## Encrypt/Decrypt
If you want to encrypt a message to Alice, you encrypt it using Alice's public key, and she decrypts it with her private key. 
If Alice wants to send you a message, she encrypts it using your public key, and you decrypt it with your private key.

### Encrypt

Encrypt for single recipient
```bash
gpg --encrypt --recipient email@address.com filename.txt
```

Encrypt so only you can decrypt
```bash
gpg --encrypt --recipient 'my_name' filename.txt
```

Encrypt so you and others can decrypt
```bash
gpg --encrypt --recipient glenn --recipient 'my_name' filename.txt
```

Encrypt for all members of a group (as defined in gpg.conf)
```bash
gpg --encrypt --recipient journalists filename.txt
```
Group definition in ~/.gnupg/gpg.conf
```bash
group  journalists  =  glenn  laura  ewan  barton
```

### Decrypt

To STDOUT (Text in terminal)
```bash
gpg --decrypt filename.txt.gpg
```

Write to file: (will write to filename.txt)
```bash
gpg filename.txt.gpg
```

### Symetric Cipher Encryption

```bash
gpg --output doc.gpg --symmetric doc
```

## Signing and Verification

Detached (Separate File) Signature
```bash
gpg --output doc.sig --detach-sig doc
```
Inline Signature
```bash
gpg --output doc.sig --sign doc
```

### Verification
If not detached
```bash
gpg --output doc --decrypt doc.sig
```
If detached
```bash
gpg --verify doc.sig doc
```

### Clearsigned documents
--clearsign causes the document to be wrapped in an ASCII-armored signature but otherwise does not modify the document.
```bash
gpg --clearsign doc
```


#### References
[1](https://www.gnupg.org/gph/en/manual/x110.html)
[2](http://blog.ghostinthemachines.com/2015/03/01/how-to-use-gpg-command-line/)