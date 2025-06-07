# Encryption

## Asymmetric Encryption  

### Diagram

![asymmetric encryption diagram](/assets/assymetric-encryption.png)

### Use Cases

1. Private & Public key pair may be used for encryption: Public key is used for encryption of data, and only the owner of the private key can use the latter to decrypt encrypted data.
2. Signing data using Private key: the owner of keys signs (encrypts) the data using the Private key, and anyone who has the Public key may verify the signature by decrypting the data.

### Signing & Verifying Signatures

> In case of certificates, certificate is the data in the picture, and is hashed and encrypted by the Certification Authority.

![signing and verifying signatures diagram](/assets/sign-verify-signature.png)

Signing and verifying signature using asymmetric keys has these **positive effects**:

1. It ensures that the data wasn't tampered with (ensured by hashing).
2. If encrypted data with a Private key decrypted with a corresponding Public key matches with the data hash, it means that the data could've been sent by the owner of the Private key only. RSA's properties ensure that it's impossible to create a valid signature without the private key. In other words, it provides authentication.

If the certificate is signed (encrypted with Private key) by CA (certification authority) and we trust CA (decrypted certificate is the same as the hashed certificate that was sent alongside) - we trust the owner of the certificate too.
