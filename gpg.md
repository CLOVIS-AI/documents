# PGP/GPG

PGP (Pretty Good Privacy) is an algorithm for encryption, decryption and signature of documents. GPG (or GnuPG, Gnu Privacy Guard) is GNU's implementation, available for all Linux distributions.

PGP was created in 1991, and is used to share documents safely, or to digitally sign documents. As many other algorithms, it works with pairs of public and private keys.

The private key is used to:

- Sign a document
- Decrypt a document created with the matching public key

The public key is used to:

- Check that a signature was made with the matching private key
- Encrypt a document, that can then only be decrypted with the matching private key

PGP follows a "Web of Trust" architecture: each user tells who they trust or not. This is done by signing someone else's public key (guaranteeing that you trust that they are who they are).

For this reason, PGP is also often used to publish binaries (Debian / APT packages, ArchLinux packages, Android updates), or to prove that you are who are say you are (for example, to stop impersonation with Git).

In this document, you should replace everything written in ALL_CAPS. For all commands that contain a KEY_ID, either the key signature, or the owner's email address, can be used.

## Key handling

### Creating a key pair

To create your key:

```shell
$ gpg --full-generate-key
```

List all locally available keys:

```shell
$ gpg -k  # --list-keys

# The output will look similar to this:
pub   rsa2048 2017-08-16 [SC]
      5DE3E0509C47EA3CF04A42D34AEE18F83AFDEB23
uid           [  full  ] GitHub (web-flow commit signing) <noreply@github.com>

# What it means:
# rsa2048                                  the algorithm used
# 5DE3E0509C47EA3CF04A42D34AEE18F83AFDEB23 the key signature/ID (used to refer to that specific key in commands)
# full                                     how much the key is trusted
```

List all locally available private keys:

```shell
$ gpg -K  # --list-secret-keys
```

### Web of Trust

To let other people use your key, you need to publish it. There are services called _key servers_ which allow anyone to publish their keys.

To send or receive keys:

```shell
# Send a key
$ gpg --send-keys KEY_IDS

# Receive a key
$ gpg --receive-keys KEY_IDS

# To specify another key server than the default, use "--keyserver <KEY SERVER URL>" at the start of the line. Common key servers include:
# - Ubuntu: keyserver.ubuntu.com
# - MIT: pgp.mit.edu
# - OpenPGP: keys.openpgp.org
# - Debian: hkp://keyring.debian.org
# - SKS: pool.sks-keyservers.net (not maintained anymore)
# Some of these are synchronized together.

# Update all local keys (do once in a while, to know if a key has been deleted, has expired, etc)
$ gpg --refresh-keys

# Display the fingerprint (a short number that is easier to read so you can check with the other user)
$ gpg --fingerprint KEY_ID
```

Signing a key means that you guarantee that its owner is who they say they are. You should always check that you have the correct key.

```shell
# Sign their key
$ gpg --sign-key KEY_ID

# Who signed the key?
$ gpg --list-sig KEY_ID
```

You might know for sure that the owner of the key is the correct person, but that doesn't mean that you trust _them_ to properly check who the owners are of the keys they are signing. For that reason, each key is assigned a trust level:

- `unknown`: You do not know whether that person is trustworthy or not (default)
- `none`: You know that they do not check the key validity
- `marginal`: You know that they properly check the key validity
- `full`: You know that they trust the key validity so well, that you trust them as much as you do yourself
- `ultimate`: The trust level for your own keys (keys you possess the private key to)

To set the trust level of a key:

```shell
$ gpg --edit-key KEY_ID  # open the interactive editor
Command> trust  # then, follow the prompts on screen
Command> quit
```

Now, keys are trusted if the path of signature from your key to theirs is shorter than 5 steps, and:

- You signed them yourself, or
- They were signed by someone that you trust fully, or
- They were signed by 3 people that you trust marginally.

[All these values can be changed](https://www.gnupg.org/gph/en/manual/x334.html). Trust levels are local (they are not published, so other users do not know how much you trust them, and it doesn't have any impact on other users). Unlike signing a key, you are not making a promise to anyone.

### Managing keys

#### Export and import

```shell
# Export a public key
# ('armor' means to use hexadecimal, and not raw binary, this is more convenient because you can easily display the key in a file editor or terminal and copy/paste it)
$ gpg --armor --export KEY_ID >FILE.gpg

# Import a public key
$ gpg --import FILE.gpg

# Export a secret key
$ gpg --armor --export-secret-key KEY_ID >FILE.gpg

# Import a secret key
$ gpg --armor --import-secret-key FILE.gpg
```

#### Invalidation

When you create a key, you should also create a revocation certificate:

```shell
$ gpg --gen-revoke KEY_ID >CERTIFICATE_FILE.asc
```

To revoke a key:

```shell
# Import the revocation certificate 
$ gpg --import CERTIFICATE_FILE.asc

# Send the revoked key to the key server of your choice
$ gpg --send-keys KEY_ID  # see the 'Web of Trust' section
# Then, tell everyone who used the key to run 'gpg --refresh-keys'
```

A revoked key can still be used to decrypt documents or check the validity of a signature, but it cannot be used to encrypt documents or sign them.

## Encryption, signature

### File encryption and signature

Encrypt a file:

```shell
# Encrypt a file
$ gpg --output FILE.gpg --encrypt --recipient KEY_ID FILE
# You can use '--recipient KEY_ID' multiple times.

# Decrypt a file
$ gpg --output FILE --decrypt FILE.gpg
```

Signatures that modify the file:

```shell
# Sign and compress the file
$ gpg --output FILE.sig --sign FILE

# Sign the file in cleartext (useful for emails, .txt or .md files)
# Adds a header and a footer to the file
$ gpg --output FILE.sig --cleartext FILE

# Check the signature, uncompress if necessary
$ gpg --output FILE --decrypt FILE.sig
```

Signatures that do not modify the file:

```shell
# Sign the file without modifying it
# Creates a new 'signature' file that goes along the main one
$ gpg --output FILE.sig --detach-sig FILE

# Check a detached signature
$ gpg --verify FILE.sig FILE
```

You can also use GUI tools, like Dolphin, Nautilus, Kleopatra, etc. You can set KMail or Thunderbird (or other mail clients) to encrypt or sign emails.

### Git: signing commits

Git does not check who creates a commit. When you first set Git up, you tell it who you are (`git config user.name`, `git config user.email`), Git does not have any way to check that you're lying. It is also common to push commits from someone else (if you've rebased them), so the SSH key cannot be trusted.

You can set Git to sign your commits with your GPG key, in which case other developers can check who made the commit. This is a good protection against identity theft (if a commit isn't signed, it's not yours).

```shell
# Tell Git which key to use
$ git config --global user.signingKey KEY_ID

# Sign commits automatically
$ git config --global commit.gpgsign true
```

To create a signed tag, use `git tag -s`. You can then upload your public key to [GitLab](https://gitlab.com/-/profile/gpg_keys) or GitHub, which will add a 'verified' icon to your commits in the web interface.
