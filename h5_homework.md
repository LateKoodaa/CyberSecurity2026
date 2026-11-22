# h5 Alice and Bob

## x) Read and summarize

**Schneier 2015: Applied Cryptography, Chapter 1 Foundations, 1.1 Terminology**

- The basic setting: a sender wants to send a message to a receiver without an eavesdropper being able to read it.
- Encryption turns plaintext into ciphertext and decryption turns it back. Cryptography is about keeping messages secure, cryptanalysis about breaking them.
- Besides keeping messages secret, cryptography should also let the receiver check who sent the message (authentication), that it hasn't been changed (integrity) and that the sender can't later deny sending it (nonrepudiation).
- Keeping the algorithm itself secret is a weak approach. Modern cryptography puts all the security in the key, so the algorithm can be public and openly analysed.
- In symmetric algorithms, both parties use the same secret key, so they have to agree on it beforehand and keep it safe.
- In public-key algorithms, the encryption key is public and the decryption key is private. Anyone can encrypt a message, but only the owner of the private key can read it.

**Schneier 2015: Applied Cryptography, Chapter 2 Protocol Building Blocks, 2.5 Communications Using Public-Key Cryptography**

- Symmetric encryption is like a safe that everyone with the combination can open. Public-key cryptography works more like a mailbox: anyone can drop a letter in with the public key, but only the owner of the private key can take it out.
- Alice can send Bob a secret message without agreeing on a key first. She encrypts it with Bob's public key, and only Bob can decrypt it with his private key.
- Public-key algorithms are slow, so in practice they are mostly used to encrypt keys, not messages.
- In a hybrid cryptosystem, Alice sends Bob a random session key encrypted with his public key, and after that they both use the session key with fast symmetric encryption.
- Merkle's Puzzles was an early way to agree on a key over an insecure channel by making the eavesdropper do much more work than Alice and Bob.
- A good signature should be authentic, unforgeable, not reusable, unalterable and impossible to deny. On computers this is harder than on paper, because files are easy to copy and modify.
- With public-key cryptography, Alice signs with her private key and Bob verifies the signature with Alice's public key. No trusted middleman is needed, except to confirm that the public key really belongs to Alice.

**Karvinen 2023: PGP - Send Encrypted and Signed Message - gpg**

- The article shows how to send an encrypted and signed message with gpg, using a folder and `--homedir .` to simulate a second user on the same computer.
- Both parties create their own key pair and exchange public keys. Public keys can travel over an untrusted channel, but the fingerprint must be checked over a trusted one, for example a phone call, before signing the key as trusted.
- The sender encrypts the message with the recipient's public key and signs it with her own private key. After this, even the sender can't decrypt it.
- The recipient decrypts the message with his private key and verifies the signature with the sender's public key. "Good signature" confirms who sent it.

## a) Pubkey today

1. **HTTPS when browsing:** The website has a key pair and its public key is in a certificate signed by a certificate authority (CA). My browser already trusts the CA's public key, so it can check that the certificate is genuine. During the TLS handshake, the browser and the server agree on a shared session key using temporary Diffie-Hellman keys and the server signs the handshake with its private key. The browser verifies the signature with the public key from the certificate, so it knows it's talking to the real site. After that, the traffic is encrypted with the fast symmetric session key.

2. **Updating apps on my Android phone from Google Play:** Every Android app is signed with an app signing key. The developer signs the app with a private upload key and Google checks it with the matching public certificate to make sure the upload really came from that developer. Google then signs the final app with the app signing key, whose private key is kept in Google's key management service. When my phone installs an update, it checks that the update is signed with the same key as the version already installed. If someone tried to give me a modified app, the signature wouldn't match and the phone would refuse the update.


## b) Encrypt and decrypt a message

### Environment

- Host: Windows 11
- Virtualization: Oracle VirtualBox
- Guest: Debian 13.7 (trixie)
- Tools: GnuPG 2.4.7 (libgcrypt 1.11.0)

I checked the versions with `cat /etc/debian_version` and `gpg --version`.

<img width="675" height="325" alt="image" src="https://github.com/user-attachments/assets/dfa42fcb-555c-4040-84f0-548e2d377076" />

### Simulating two users

To simulate two people, I created two folders, alice and bob, in my home directory. Each folder works as its own gpg home directory with a separate keyring. In real life, Alice and Bob would be different people on different computers.

<img width="500" height="115" alt="image" src="https://github.com/user-attachments/assets/ab51332f-e3e4-4769-8ea0-0161187ab499" />

`chmod og-rwx` removes all permissions from the group and other users. gpg requires this, because otherwise other users on the same computer could read the secret keys. The result `drwx------` shows that only I, the owner, can access the folders.

### Bob creates a key pair

In this test, Alice sends an encrypted message to Bob. Alice needs Bob's public key to encrypt the message, and Bob needs his own secret key to decrypt it. That's why I started by creating Bob's key pair.

    $ cd ~/bob
    $ gpg --homedir . --gen-key

`--homedir .` makes gpg use the current folder as Bob's keyring instead of my own `~/.gnupg`. `--gen-key` creates a new key pair with the default settings.

<img width="793" height="228" alt="image" src="https://github.com/user-attachments/assets/a3c04933-c9ed-4656-bae0-dbbadb65c2d7" />

gpg created a new keyring file `pubring.kbx` in Bob's folder, which confirms that Bob has his own keyring. gpg then asks for a name and email address to build a user ID, which connects the key to a person.

<img width="1030" height="765" alt="image" src="https://github.com/user-attachments/assets/285716f6-5ae9-4038-8eb7-9b1c27799da2" />

gpg asked for a passphrase to protect Bob's secret key. I used a simple passphrase for testing.

<img width="807" height="361" alt="image" src="https://github.com/user-attachments/assets/315f2a99-216c-4e17-a186-5ca2b57355e5" />

The key pair was created. Unlike in Karvinen's article, where the keys were rsa3072, GnuPG 2.4.7 uses elliptic curve keys by default:

- `pub ed25519 [SC]` is the primary key, used for signing (S) and certifying other keys (C).
- `sub cv25519 [E]` is a subkey used for encryption (E). This is the key Alice will use when she encrypts a message to Bob.
- The long hex string `28048F3C2B6FC8EB17837A8402790F633A7512AC` is the fingerprint of Bob's key. Alice can use it to check that she has the right public key.

### Bob exports his public key

Alice needs Bob's public key, so Bob exports it to a file.

    $ gpg --homedir . --export --armor --output bob.pub
    $ ls
    $ head -4 bob.pub

<img width="762" height="170" alt="image" src="https://github.com/user-attachments/assets/b4f74e3e-a60f-44da-89a7-287fd923d817" />
    
### Alice creates a key pair

To encrypt, Alice only needs Bob's public key. However, she needs her own key pair to sign Bob's key as trusted after checking its fingerprint. I created it the same way as Bob's.

    $ cd ~/alice
    $ gpg --homedir . --gen-key

<img width="791" height="232" alt="image" src="https://github.com/user-attachments/assets/88ab1857-7e93-4dcf-b340-b6447a472211" />

<img width="798" height="440" alt="image" src="https://github.com/user-attachments/assets/40493d3b-d5c3-4bf4-b239-503acc366aef" />

Alice's key pair has the same structure as Bob's: an ed25519 primary key for signing and a cv25519 subkey for encryption. The fingerprint is different, as each key pair is generated from new random data.

### Alice imports Bob's public key

In real life, Bob could send his public key by email or publish it on a web page. Here I simulated this by copying the file to Alice's folder. Then Alice imported it into her keyring.

    $ cd
    $ cp -v bob/bob.pub alice/
    $ cd ~/alice
    $ gpg --homedir . --import bob.pub

<img width="806" height="176" alt="image" src="https://github.com/user-attachments/assets/37941649-ed71-49da-a663-91d2bfd3bfc5" />

### Alice checks Bob's fingerprint

    $ gpg --homedir . --fingerprint

<img width="712" height="313" alt="image" src="https://github.com/user-attachments/assets/27b24a2e-9c3d-4961-a1f5-4b8da164c339" />

Alice's own key has the trust level `[ultimate]`, because she created it herself. Bob's key is `[ unknown]`, because Alice hasn't verified it yet. In real life, Alice would call Bob and ask him to read his fingerprint aloud. Here I compared it to the fingerprint shown when Bob's key was created: `2804 8F3C 2B6F C8EB 1783 7A84 0279 0F63 3A75 12AC`. They match, so Alice has Bob's real public key.

The trust database lines show that gpg uses the PGP "web of trust" model. Instead of a central certificate authority, like in HTTPS, users verify and sign each other's keys.

### Alice signs Bob's key

After checking the fingerprint, Alice signs Bob's public key with her own secret key. This marks the key as verified in her keyring.

    $ gpg --homedir . --sign-key "2804 8F3C 2B6F C8EB 1783  7A84 0279 0F63 3A75 12AC"

<img width="935" height="480" alt="image" src="https://github.com/user-attachments/assets/a6fc16a4-0585-43cc-a1cb-8e16a3f37ac6" />

<img width="722" height="340" alt="image" src="https://github.com/user-attachments/assets/18d751f7-60b9-46e0-bce6-e9839b91c066" />

Bob's key is now `[  full  ]` instead of `[ unknown]`, because Alice has signed it. The trust database line `depth: 1 valid: 1` shows that one key, Bob's, is now valid through Alice's signature. Alice is now ready to encrypt a message to Bob.

### Alice writes a message

    $ nano message.txt
    $ cat message.txt

<img width="591" height="120" alt="image" src="https://github.com/user-attachments/assets/0629e0bf-d5b3-4275-a6d4-00f6e5acf0df" />

This is the plaintext, which anyone who can see the file can read.

### Alice encrypts the message to Bob

    $ gpg --homedir . --encrypt --recipient bob@example.com.invalid --armor --output encrypted.asc message.txt
    $ ls
    $ cat encrypted.asc

<img width="937" height="337" alt="image" src="https://github.com/user-attachments/assets/539255ce-517f-4b59-a5d0-7bc5b16f375c" />

`--recipient bob@example.com.invalid` tells gpg to use Bob's public key from Alice's keyring. gpg didn't ask for a passphrase, because encryption only uses Bob's public key, not Alice's secret key.

### Can Alice decrypt her own message?

    $ gpg --homedir . --decrypt encrypted.asc

<img width="752" height="95" alt="image" src="https://github.com/user-attachments/assets/70a5abdf-ebf1-4961-912e-e76ebdb0b47e" />

No. The message was encrypted with Bob's public key (subkey `D49BB2A799E3DDFD`), so only Bob's secret key can decrypt it. Alice doesn't have it, so decryption fails with "No secret key".

### Bob decrypts the message

To simulate sending the message, I copied the encrypted file to Bob's folder. The encrypted file can be sent over an untrusted channel, because nobody can read it without Bob's secret key.

    $ cd
    $ cp -v alice/encrypted.asc bob/
    $ cd ~/bob
    $ gpg --homedir . --decrypt encrypted.asc

<img width="741" height="155" alt="image" src="https://github.com/user-attachments/assets/87c9ed03-6643-43f8-ac9a-8a9fcf5384fb" />

gpg asked for Bob's passphrase to unlock his secret key, and then printed the original message. The decryption worked because Bob has the secret key that matches the public key Alice used.

The message was only encrypted, not signed. Bob knows that nobody else could read it, but he can't be sure it came from Alice. Anyone with Bob's public key could have sent it.

### Summary: who uses which key

First, both Bob and Alice created their own key pairs, each consisting of a public key and a secret key. Bob exported his public key and shared it with Alice. Alice imported the key, checked Bob's fingerprint and signed the key with her own secret key to mark it as verified. To encrypt the message, Alice used Bob's public key, so she didn't need her own secret key or a passphrase at this stage. Finally, Bob decrypted the message with his secret key, which he unlocked with his passphrase. Alice couldn't decrypt the message herself, because only Bob's secret key can open a message encrypted with his public key.

### Security evaluation of gpg

The cryptography in gpg is strong. GnuPG 2.4.7 used modern elliptic curve keys by default and every message is encrypted with its own random session key. In practice, the risks come from how the tool is used:

- Encryption only helps if Alice has Bob's real public key, so the fingerprint must be checked over a trusted channel.
- Bob's secret key is only as safe as his passphrase. My test passphrase was weak, which would be a serious risk in real use.
- The encrypted file shows the recipient's key ID, so others can see who the message is for.
- Without `--sign`, Bob can't know the message came from Alice.
- If Bob's secret key is stolen later, all old messages encrypted to it can be read.

Overall, gpg is secure when used carefully, but I think it depends a lot on the user.

## c) Password management

I chose KeePassXC, a free and open source password manager. It stores all passwords in one encrypted database, which is opened with a master password.

    $ sudo apt-get update
    $ sudo apt-get install keepassxc
    $ keepassxc-cli --version
<img width="485" height="40" alt="image" src="https://github.com/user-attachments/assets/69e14dd9-e24a-4ded-b673-ae1dfbef5187" />

### Creating a database

I started KeePassXC with `keepassxc &` and chose "Create new database". I kept the default encryption settings.

<img width="806" height="632" alt="image" src="https://github.com/user-attachments/assets/5431f3bc-5737-4022-812f-e516358b7337" />

<img width="800" height="547" alt="image" src="https://github.com/user-attachments/assets/fecc81b2-da9d-465b-9b45-ffe32ffd8768" />

I set a master password and saved the database as `Passwords.kdbx` in my home directory. The whole file is encrypted, so without the master password nobody can even see which services are stored in it.

### Adding a password

I added a new entry for a made-up web shop and used the built-in password generator.

<img width="1021" height="767" alt="image" src="https://github.com/user-attachments/assets/180b4275-c385-4da0-9572-879ecd110639" />

<img width="807" height="237" alt="image" src="https://github.com/user-attachments/assets/ee18b027-0f08-4667-8e7e-9db9a0715a21" />

The entry is now saved in the database. The password itself is not shown in the list.

<img width="717" height="317" alt="image" src="https://github.com/user-attachments/assets/8749c89d-ec52-4c0d-8e04-c85f9868ff8d" />

The whole database file is encrypted, including usernames and URLs. I checked this by searching the file for the word "example", which appears in both the username and the URL of my entry:

<img width="545" height="38" alt="image" src="https://github.com/user-attachments/assets/26a5584b-0421-4b7c-8a26-9d8aa154b7d4" />

### Attacks that take advantage of people not using password managers

Without a password manager, people tend to reuse the same password on many sites or choose short, easy-to-remember passwords. Attackers take advantage of both:

- **Credential stuffing:** attackers take usernames and passwords leaked from one site and try them automatically on many other sites. It works because people reuse passwords. A password manager makes it easy to have a unique password for every site, so one leak doesn't open other accounts.
- **Password spraying:** attackers try a few common passwords, like `Password1`, against many accounts. Trying only a few per account helps them avoid lockouts. My generated password would never be on such a list.
- **Dictionary and brute force attacks:** if a password hash is leaked, attackers can try millions of common words and combinations offline. A password like `Password` would be found quickly. A random 20-character password with about 115 bits of entropy would not.
- **Phishing:** a fake login page can trick a person into typing their password. With a browser extension, a password manager only fills in the password on the site saved in the entry's URL, so it won't fill it on a fake site.


### References

- Karvinen, T. 2023: PGP - Send Encrypted and Signed Message - gpg. https://terokarvinen.com/2023/pgp-encrypt-sign-verify/
- Schneier, B. 2015: Applied Cryptography, 20th Anniversary Edition. Chapter 1 Foundations, 1.1 Terminology. Wiley.
- Schneier, B. 2015: Applied Cryptography, 20th Anniversary Edition. Chapter 2 Protocol Building Blocks, 2.5 Communications Using Public-Key Cryptography and 2.6 Digital Signatures. Wiley.
- Rescorla, E. 2018: RFC 8446, The Transport Layer Security (TLS) Protocol Version 1.3. https://www.rfc-editor.org/rfc/rfc8446
- Android Developers: Sign your app. https://developer.android.com/studio/publish/app-signing
- Google Play Console Help: Use Play App Signing. https://support.google.com/googleplay/android-developer/answer/9842756
- Glick, K. 2017: Enroll for app signing in the Google Play Console & secure your app using Google's robust security infrastructure. Android Developers Blog. https://android-developers.googleblog.com/2017/09/enroll-for-app-signing-in-google-play.html
- GnuPG Blog. https://gnupg.org/blog
- OWASP: Credential stuffing. https://owasp.org/www-community/attacks/Credential_stuffing
- MITRE ATT&CK: Brute Force: Password Spraying, T1110.003. https://attack.mitre.org/techniques/T1110/003/
- NCSC: Top tips for staying secure online, Password managers. https://www.ncsc.gov.uk/collection/top-tips-for-staying-secure-online/password-managers
- KeePassXC. https://keepassxc.org/
