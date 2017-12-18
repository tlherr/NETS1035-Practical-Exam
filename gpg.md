# GPG Encryption

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

pub  1024D/9E98BC16  created: 1999-06-04 expires: never      trust: -/q
sub  1024g/5C8CBD41  created: 1999-06-04 expires: never
(1)  Blake (Executioner) <blake@cyb.org>

Command> fpr
pub  1024D/9E98BC16 1999-06-04 Blake (Executioner) <blake@cyb.org>
             Fingerprint: 268F 448F CCD7 AF34 183E  52D8 9BDE 1A08 9E98 BC16
A key's fingerprint is verified with the key's owner. This may be done in person or over the phone or through any other means as long as you can guarantee that you are communicating with the key's true owner. If the fingerprint you get is the same as the fingerprint the key's owner gets, then you can be sure that you have a correct copy of the key.
After checking the fingerprint, you may sign the key to validate it. Since key verification is a weak point in public-key cryptography, you should be extremely careful and always check a key's fingerprint with the owner before signing the key.

Command> sign

pub  1024D/9E98BC16  created: 1999-06-04 expires: never      trust: -/q
             Fingerprint: 268F 448F CCD7 AF34 183E  52D8 9BDE 1A08 9E98 BC16

     Blake (Executioner) <blake@cyb.org>

Are you really sure that you want to sign this key
with your key: "Alice (Judge) <alice@cyb.org>"

Really sign?
Once signed you can check the key to list the signatures on it and see the signature that you have added. Every user ID on the key will have one or more self-signatures as well as a signature for each user that has validated the key.

Command> check
uid  Blake (Executioner) <blake@cyb.org>
sig!       9E98BC16 1999-06-04   [self-signature]
sig!       BB7576AC 1999-06-04   Alice (Judge) <alice@cyb.org>
Encrypting and decrypting documents
A public and private key each have a specific role when encrypting and decrypting documents. A public key may be thought of as an open safe. When a correspondent encrypts a document using a public key, that document is put in the safe, the safe shut, and the combination lock spun several times. The corresponding private key is the combination that can reopen the safe and retrieve the document. In other words, only the person who holds the private key can recover a document encrypted using the associated public key.
```

## Encrypt/Decrypt
If you want to encrypt a message to Alice, you encrypt it using Alice's public key, and she decrypts it with her private key. 
If Alice wants to send you a message, she encrypts it using your public key, and you decrypt it with your private key.

### Encrypt

```bash
gpg --output doc.gpg --encrypt --recipient blake@cyb.org doc
```
The --recipient option is used once for each recipient and takes an extra argument specifying the public key to which the document should be encrypted. 
The encrypted document can only be decrypted by someone with a private key that complements one of the recipients' public keys. In particular, you cannot decrypt a document encrypted by you unless you included your own public key in the recipient list.

### Decrypt

```bash
gpg --output doc --decrypt doc.gpg
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