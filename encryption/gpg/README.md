# gpg - OpenPGP encryption and signing tool

## a little theory

### what you can do with GPG, 4 central ideas:

Generation of keypairs, randomly-generated and mathematically linked pairs of
files, one of which is kept permanently secret (the private key) and one of
which is published (the public key). This is the basis of asymmetric key
cryptography.

Managing keys, both your own public and private key, along with other people's
public keys, so that you can verify others' messages and files, or encrypt them
so that only those people can read them. This might include publishing your
public key to online keyservers, and getting people to sign it to confirm that
the key is really yours.

Signing files and messages with your private key to enable others to verify
that a file or message was authored or sighted by you, and not edited in
transmission over untrusted channels like the internet. The message itself
remains readable to everybody.

Encrypting files and messages with other people's public keys. so that only
those people can decrypt and read them with their private keys. You can also
sign such messages with your own private key so that people can verify that it
was sent by you.

### remind me what a subkey is

Subkeys are like the normal keys, except they're bound to a master key pair. A
subkey can be used for signing or for encryption. The really useful part of
subkeys is that they can be revoked independently of the master keys, and also
stored separately from them.

In other words, subkeys are like a separate key pair, but automatically
associated with your main keypair.

GnuPG actually uses a signing-only key as the master key, and creates an
ecryption subkey automatically. Without a subkey for encryption, you can't have
encrypted e-mails with GnuPG at all.

Subkeys make key management easier. The master key pair is quite important: it
is the best proof of your identity online, and if you lose it, you'll need to
start building your reputation from scratch. If anyone else gets access to your
private master key or its private subkey, they can make everyone believe
they are you: they can upload packages in your name, vote in your name, and do
pretty much anything else you can do. You might dislike it. So you should keep
all your private keys safe.

You should keep your private master key very, very safe. However, keeping all
your keys extremely safe is inconvenient: every time you need to sign
something, you need to copy the packages onto suitable portable media, go into
your sub-basement, prove to the armed guards that you are you by using several
methods of biometric and other identification, go through a deadly maze, feed
the guard dogs the right kind of meat, and then finally open the safe, get out
the signing laptop, and sign the packages. Then do the reverse to get back up
to your internet connection.

Subkeys make this easier: you already have an automatically created encryption
subkey and you create another subkey for signing, and you keep those on your
main computer. You publish the subkeys on the normal keyservers, and everyone
else will use them instead of the master keys for encrypting messages or
verifying your message signatures. Likewise, you will use the subkeys for
decrypting and signing messages.

You will need to use the master keys only in exceptional circumstances, namely
when you want to modify your own or someone else's key. More specifically, you
need the master private key:
- when you sign someone else's key or revoke an existing signature
- when you add a new UID or mark an existing UID as primary
- when you create a new subkey
- when you revoke an existing UID or subkey
- when you change the preferences (e.g., with setpref) on a UID
- when you change the expiration date on your master key or any of its subkey,
- when you revoke or generate a revocation certificate for the complete key

## basic usage example

### create a new primary keypair

```bash
gpg --gen-key
```

### list keys

```bash
gpg --list-keys
gpg --list-secret-keys
gpg --list-public-keys
```

### export public key

```bash
gpg --output alice.gpg --export alice@cyb.org
```

OR

```bash
gpg --armor --export [ID] > alice.public.asc
```

### generate the output in an ASCII-armored format

```bash
gpg --armor --export alice@cyb.org
```

### add public key to keyring

```bash
gpg --import blake.gpg
```

### certify a key

```bash
gpg --edit-key blake@cyb.org
```

```
> fpr 
> sign
> check 
```

### encrypt a document

```bash
gpg --output doc.gpg --encrypt --recipient blake@cyb.org doc
```

### decrypt a document

```bash
gpg --output doc --decrypt doc.gpg
```

### encrypt a document with a symmetric cipher

```bash
gpg --output doc.gpg --symmetric doc
```

### sign a document

```bash
gpg --output doc.sig --sign doc
```

### verify a document

```bash
gpg --verify doc.sig 
```

### clearsign a message

```bash
gpg --clearsign message.txt
```

### verify signature

```bash
gpg --verify message.txt.asc
```

### signing and verifying binary files

```bash
gpg --armor --detach-sign archive.tar.gz
gpg --verify archive.tar.gz.asc archive.tar.gz
```

## other use case examples

### keyserver examples

#### search for a key

Assuming `gpg.conf` has a keyserver configured.

```bash
gpg --search-keys blake@cyb.org
```

#### refresh keys

Assuming `gpg.conf` has a keyserver configured.

```bash
gpg --refresh-keys
```

#### Update public key with new signatures from the keyserver

```bash
gpg --keyserver [Key Server URL] --recv-key [ID] 
```

#### Send copy of public key to the keyserver to contribute any new signatures

```bash
gpg --keyserver [Key Server URL] --send-key blake@cyb.org
```

### archive & backup

```bash
tar -cf docksbackup-"$(date +%Y-%m-%d)".tar $HOME/Documents
gpg --encrypt [filename].tar
scp [filename].tar.gpg [remote]
```

### download and import public key

```bash
curl [url] | gpg --import
```
