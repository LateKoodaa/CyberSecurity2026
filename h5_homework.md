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

### References

- Karvinen, T. 2023: PGP - Send Encrypted and Signed Message - gpg. https://terokarvinen.com/2023/pgp-encrypt-sign-verify/
- Schneier, B. 2015: Applied Cryptography, 20th Anniversary Edition. Chapter 1 Foundations, 1.1 Terminology. Wiley.
- Schneier, B. 2015: Applied Cryptography, 20th Anniversary Edition. Chapter 2 Protocol Building Blocks, 2.5 Communications Using Public-Key Cryptography and 2.6 Digital Signatures. Wiley.
- Rescorla, E. 2018: RFC 8446, The Transport Layer Security (TLS) Protocol Version 1.3. https://www.rfc-editor.org/rfc/rfc8446
- Android Developers: Sign your app. https://developer.android.com/studio/publish/app-signing
- Google Play Console Help: Use Play App Signing. https://support.google.com/googleplay/android-developer/answer/9842756
- Glick, K. 2017: Enroll for app signing in the Google Play Console & secure your app using Google's robust security infrastructure. Android Developers Blog. https://android-developers.googleblog.com/2017/09/enroll-for-app-signing-in-google-play.html
