# TLS

RSA is not used for data encryption in HTTPS since it's quite slow and would require RSA key pairs on both sides for bi-directional communication. TLS uses symmetric keys.

## Establishing a TLS session

Web browser connects to the web server via HTTPS. Once TCP session is established, we start establishing a TLS session:  
![establishing tls diagram](/assets/establishing-tls.png)  
1. **Negotiate cipher suite**: web browser sends its supported list of cipher suites, and the web server selects one of them. Cipher suite is a set of protocols that will be used in further TLS communication. Each cipher suite specified how symmetric key for data encryption will be generated. They also specify what algorithm will be used for data encryption/decryption. Cipher suite also includes information about which hashing algorithm is used (usually SHA).
2. **Web server sends its certificate to the web browser**: the whole certificate chain (usually excluding root CA) is sent to the web browser.
3. **Verify server certificate**: the web browser verifies the certificates.
4. **Generate symmetric key for data encryption**: in some cases the key is generated and encrypted on the web browser side, and sent to the web server. Another way is to use Diffie Hellman key exchange algorithm: it doesn't require encryption because this algorithm is designed to generate the same key for both sides via unencrypted public connection.
5. **Send/receive encrypted data**: when both sides possess the encryption key, we can start sending and receiving actual data between web browser & server.

## Concepts

### Delivering Key for Encryption in TLS (Without Diffie Hellman)

![tls encryption key delivery](/assets/tls-encryption-key-delivery.png)  
Drawback: certificate keys have a longer life-span, thus this method is more prone to attacks.

### Delivering Key for Encryption in TLS (With Diffie Hellman)

![tls encryption key delivery](/assets/tls-encryption-dh-key-delivery.png)

### Diffie Hellman Key Generation

Diffie Hellman algorithm uses the modulus one-way function (function that is easy to compute, but hard to invert given the result).  
![diffie hellman key exchange](/assets/diffie-hellman-key-exchange.png)  
Alice generates 3 numbers: `a`, `g`, `p`, while Bob generates only one: `b`. `a` and `b` are considered **private keys** of Alice and Bob. They're not ever transferred over insecure network. `g` and `p` are considered as public keys. In order to decrease the quantity of messages sent between sites, both public keys `g` and `p` are usually generated on one side (the side that initiates the Diffie Hellman key exchange). Alice generates `A`, while Bob generates `B` using public keys and their private keys. Bob then sends `B` to Alice, and they both calculate the key `K`. There's no way to get private keys from `A` and `B` because modulus is used. This way, both sides end up with the same key without revealing it to the outside.

<!-- TODO: Spend more time on this -->
### Elliptic Curve Cryptography

![elliptic curve cryptography](/assets/ec.png)
![elliptic curve cryptography graphs](/assets/ec-graphs.png)  
$P + Q + R = 0$  
$P + Q = -R$  
$P + Q + Q = 0$  
$2Q = -P$

![elliptic curve point addition](/assets/ec-point-addition.png)
![elliptic curve point doubling](/assets/ec-point-doubling.png)

#### Discrete Log Problem

![elliptic curve discrete log problem](/assets/ec-discrete-log-problem.png)  
`G` - starting point on the curve. 
`n` - number of times `G` was added to itself.
`E` - final multiplication result.
If you know starting point `G` and final point `E`, is it possible to get `n`? **No, because there's no inverse operation, and just starting from `G` would be exponential in time complexity, which is impractical for large values of `n`.**

$m(nG) = n(mG)$  
`G` - point on a curve.

If we add a point `n` times to itself, and then add the result `m` times to itself, it's the same as adding a point `m` times to itself, and then adding the result `n` times to itself.

$2(3G) = 3(2G) = 6G$

#### Elliptic Curve Diffie Hellman Exchange

![elliptic curve diffie hellman exchange](/assets/ec-dh-exchange.png)  
Alice generates 2 points `A` and `G`. `a` is considered as private key of Alice. `a` specifies number of times `G` was added to itself. The Alice passes `A` and `G` to Bob, and the latter defines `B` with his own private key `b`, and sends it to Alice. In the end, they get the same key `K` by applying `aB` and `bA` operations. Neither `a` nor `b` can be found out of `A`, `B` and `G`.
Elliptic curve Diffie Hellman exchange needs to be used with authentication, as otherwise it's prone to man-in-the-middle attacks.  
**This is where the certificate comes in. The packets sent are encrypted with the server's private key. The web browser (using its public key) can then verify that the data was sent by the server.**