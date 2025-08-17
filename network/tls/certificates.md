# Certificates

## How Do Browsers Trust Self-Signed Certificates of Root Authorities?

Every OS such as Mac OS or Windows ships with a list of all certificates of root CAs. They are trusted by default. This list gets updated during OS updates.

## How Are Certificates Securely Signed

A certificate signing request (CSR) is made to the CA, which then uses its private key to sign the certificate. Signing process always occurs on the machine that owns the private key.

## Chain of Trust

![chain of trust](/assets/certificate-chain-of-trust.png)  
Root CA signed its certificate with its own private key (self-signed certificate). That means that the owner (subject) and issuer info is the same. Intermediate CA's certificate is hashed, and then signed using root CA's private key, displaying that the root CA trusts the intermediate CA. Root CA, while signing, also adds information to the certificate: marks itself as the issuer, etc. Same goes for the end user certificate, but it's signed by the intermediate CA's private key instead.

### Verifying Chain of Certificates

When a website is opened, the web server sends all intermediate and end user certificates. Verification starts from the end user certificate. First, the browser checks whether the certificate's date is still valid. Then, the signature is checked: the browser looks at the issuer information, and tries to find the intermediate CA that has matching owner (subject) information. When the intermediate CA is found, its public key is used to verify (decrypt and compare) the signature. If the decrypted signature matches the hashed certificate, it means that the signature could've been made by the intermediate CA only, and we can continue up the chain. Now, we have to verify whether we trust the root CA. The browser looks at the issuer information, and tries to find the CA that has matching owner (subject) information. When found, its public key is used to verify (decrypt and compare) the signature. If all is good, we trust that the found root CA signed the intermediate certificate. Root CA is trusted by default, since its included in the OS. In the end, this means that we trust the whole chain `root CA -> intermediate CA -> end user certificate`.  
Each item in the chain checks if it actually signed the certificate with its public key before. **The public keys are taken from the supposed signer's certificate** - we validate whether its private key was actually used to sign. If the signature is confirmed, we continue up the chain until we reach the root CA, meaning we can actually trust the whole chain. This way, in the end, we achieve authentication, crucial to [[tls/index]].

## Certificate Walkthrough

### Subject's Public Key

Subject's Public Key is the Owner's public key that will be used to perform TLS handshake.  
![certificate public key](/assets/certificate-public-key.png)

### Signature Value

Certificate Signature Value is the hashed and encrypted certificate.  
![certificate signature value](/assets/certificate-signature-value.png)

### Signature Verification Clues

SHA-256 Fingerprints field shows us the values used during the signature verification. The **Public Key** of the CA which was used to decrypt the certificate, and the decrypted **Certificate**.  
![certificate sha-256 fingerprints](/assets/certificate-fingerprints.png)
