# Cryptography

![[Pasted image 20241001164224.png]]

### Symmetric cryptography

This architecture needs a **single key** $K$, shared between a sender and a receiver. This approach has a **low computational cost**, so it's often used for data encryption.
The encrypted message can be computed from the plain text $P$ as:
$$
C = \text{ enc} (K,P)
$$
The receiver can get the plain text with the reverse operation:
$$
P = \text{dec} (K,C) = \text{enc}^{-1}(K,C)
$$
The only problem is how to securely share the common encryption key?
Only the **key needs to be secret**, the algorithm can be public, so it can be widely analysed. This obviously works in trusted systems (no malware).
If an attacker is able to sniff the cipher text, he can only performs a **brute-force attack**.
1. He needs to try every possible key $K$ in the key space
2. He needs to compute $\text{dec} (K,C)$ and look for meaningful results.

Nowadays, the computational power is very high, so we need a **trade-off** between the length of the key and the available computational power and the secrecy of the message.
Longer keys require more time to be brute-forced, but also require more time to be encrypted, so we also need to be **efficient**.

![[Pasted image 20241001164253.png]]

**Symmetric Algorithms** can be divide in:
- **Block** algorithms (DES, AES) divide the data in different blocks of fixed length, each block is then processed one at a time.
- **Stream** algorithms that works on flows of data, each bit (or byte) one at a time.

# Block algorithms
### DES (Data encryption standard)

DES was a **56-bit key length** algorithm with a 64-bit block size. Nowadays DES is **obsolete** (was cracked), but there are more reliable **variants** (3-DES).
The key operation of DES algorithm is the XOR operator, which is the "ideal" confusion operator, also XOR is the inverse of itself.
- 56 (+8 parity bits) bits for the key
- 64 bits data block
- It was designed to be efficient on dedicated hardware

### 3DES

It's a repeated application of DES algorithm: it uses either two or three keys.
Can be used in EDE mode (encrypt decrypt encrypt), and with two keys we have:
$$
C' = \text{enc})K_{1},P \quad \quad C'' = \text{dec}(K_{2},C') \quad \quad C = \text{enc} (K_{1},C'')
$$
With three different keys the approach is:
$$
C' = \text{enc})K_{1},P \quad \quad C'' = \text{dec}(K_{2},C') \quad \quad C = \text{enc} (K_{3},C'')
$$
Note that there is **no double DES algorithm**.

```ad-warning
title: Double DES
No encryption algorithm should be used in its **double** version. This type of algorithm are subject to **meet-in-the-middle** attacks, which can be performed with the following instructions:

![[Pasted image 20241001154445.png]]
```

### AES (Advanced encryption standard)

Proposed on 2 October 2000, AES is an encryption algorithm with different key's lengths.
The **key** can be **128 / 192 / 256 bit** long, the data block is fixed at 128 bit.

# Application of block algorithms

How can be process data larger than the block size?
To encrypt data of size greater than block size:
- **ECB**
- **CBC**
To encrypt data of size smaller than block size:
- Padding
- CFB, OFB (old technique)
- CTR

### ECB (Electronic code block)

This technique encrypt each data block with the same key, so for the $i$-th block we have:
$$
C_{i} = \text{enc} (K, P_{i}) \quad \quad P_{i} = \text{dec}(K, C_{i})
$$
This technique is efficient, because blocks can be processed in parallel, also there is no error propagation.
On the other side, ECB should not be used for **long messages**, because is vulnerable to **"known plain text attacks"** and also because the swapping of two encrypted block can be undetected.

![[Pasted image 20241002102028.png]]

### CBC (Cipher block chaining)

This technique uses memory, because the encryption depends also from the previous step.
The mathematical formula for **Cipher block chaining** is:
$$
C_{i} = \text{enc} (K, C_{i} \oplus C_{i-1})
$$
An **Initialization vector** (IV) is required to start the encryption chain, also known as $C_{0}$. IV must be random, unpredictable and as long as the block size.

![[Pasted image 20241002102056.png]]

To start the decryption process, the receiver must know the IV, then we can compute the plain text as:
$$
P_{i} = \text{dec} (K,C_{i}) \oplus C_{i-1}
$$
In contrast to ECB, this approach is sequentially (not parallel) and is subject to error propagation.
Pros are:
- No "known plain text attacks" (because the same plain text block results in different ciphertexts)
- No "**swapping** attacks": by exchanging two block, the plain text decrypted will be corrupted.

### Padding

If the size of data is smaller than the block size, it's sufficient to add some **padding bits** to reach the block size. However this creates 2 main problem: Transmission of more data than needed and we need to decide the value of padding bits.
Many options for the bits value: all null bytes, one '1' followed by all zeros or **last byte's value equal to the length of padding**.

![[Pasted image 20241002103915.png]]

Useful Notes on padding:
- Padding offers **minimal integrity control**: if key is wrong or manipulated, than the padding will be incorrect
- Padding is typically applied to the **last block of data** in ECB or CBC
- Even if the plain text is an **exact multiple** of the block, padding **must be added** anyhow to avoid errors in the interpretation of the last block.

### CTS (Cipher text stealing)

This mode permits to encrypt data with block algorithms **without padding**.
Consider the two last block of our data: in particular the last one $P_{n}$ is smaller than block size. $P_{n-1}$ is encrypted with the symmetric key and the output is divided into a head and the **tail** (tail's size is the same of the remaining space in $P_{n}$ block). The cipher $C_{n-1}$ is split into two part; the tail is encapsulated in the remaining space of the $P_{n}$ block. Then we encrypt $P_{n}^*$ and swap orders.

![[Pasted image 20241002104742.png]]

### CTR (Counter mode)

This mode allows to encrypt $N$ bits at a time (called a "group").
There is **no need to apply padding**, it only requires a nonce (one-time number) and a counter, composed by a suitable function. Note that a transmission error causes errors only in one group.

![[Pasted image 20241002105830.png]]

# Stream algorithms

This type of algorithms **operate on a stream of data without requiring the division on blocks**. The ideal algorithm requires a key which is as long as the message to protect. In real applications the algorithms use a pseudo-random key generators (must be unique each time and non-zero), synchronized between the sender and receiver.
Example of stream algorithm: RC4 and SEAL (both old), Salsa20, ChaCha20 (both modern).

![[Pasted image 20241002111345.png]]

# Asymmetric cryptography

This technique introduces a couple of keys, one used for encryption and the other for decryption. Pair of keys is divided in: public and private key.
This kind of approach is expensive (in terms of computational power), so we only use asymmetric algorithms on small data.

```ad-note
title: Elliptic curve cryptography
Since Asymmetric technique is slow, a new approach was invented. In EC algorithms the operations are executed on the surface of a 2D elliptic curve.
**EC**DSA, **EC**DH, **ED**MQV are examples of algorithms that uses elliptic curves.
```

We have two possible strategies:

![[Pasted image 20241008131612.png]]

- The first option **guarantee** Data authentication and integrity: only the sender has access to the correct private key. However anybody can decrypt the message so it does not guarantee confidentiality.
- The second option guarantee **confidentiality**: only the receiver (with the private key) can decrypt correctly the message.

How can we guarantee the correct binding between the public key and the corresponding person?
We can distribute the public key by means of a specific data structure known as **public key certificate** (digital signature). It's kind of a passport, where it's written the binding between a key and its entity. Popular format for a digital signature X.509
We also need information about the certificate **issues** (who emits the certificate).

Asymmetric cryptography is useful for exchanging a **symmetric shared key** in a secure way. 
Let's immagine we need to transfer 1 TB of encrypted data: as we said before, asymmetric algorithms are efficient for small amount of data. A possible solution is to use both technique:
1. asymmetric approach is used to encrypt and share a symmetric key between sender and receiver
2. Symmetric approach uses the generated symmetric key to transfer securely the data

![[Pasted image 20241011185743.png]]

In the example above, K is the symmetric key, generated by X, and shared to Y, by means of asymmetric keys. Then Y can decrypt the message with the symmetric key K.

Cons of this approach:
- It's not optimal that X generates the symmetric key, because it can share the keys with other entities, for example a Z
To solve this problem, DH algorithm was created.
# Diffie-Hellman (DH)

Diffie-Hellman is an asymmetric algorithm used from two entities to agree on a secret key (secret key exchange).
This algorithm allows the exchanging of a shared key between two entities, using an insecure channel. This technique is immune to sniffing attack, but it is vulnerable to the MITM attack.

![[Pasted image 20241008135834.png]]

1. Both A.B choose two numbers $p,g$ (same for both)
2. A and B choose a **private** exponent (X for A, Y for B)
3. A computes $A=g^X \text{mod} \,\,p$, B computes $B=g^Y \text{mod} \, \, p$
4. They exchange $A,B$
5. Both A and B computes the keys $K_{A}=B^X \text{mod} \, \, p$, $K_{B}=A^Y \text{mod} \, \, p$
6. For mathematical properties $K_{A} = K_{B}$

As we said before, MITM attacks can lead to incorrect key exchange.

![[Pasted image 20241008135856.png]]

In order to avoid MITM, we can use **certificate**.

# Digest

Since now we have seen algorithms that guarantee the confidentiality, but in many applications, **message integrity** is a more valuable property.
To ensure that both entities received an **unmodified message** we use a mechanism called **Digest**.

A **message digest** is a **fixed-length** "summary" of the message to be protected. To do so, we use **cryptographic hash functions**.
Input of hash function is a message $m$, the output is $md$ computed by hashing the message $h(m)$.

![[Pasted image 20241011193104.png]]

The ideal cryptographic function is **fast, hard to invert** and also has lower probability to generate **collisions**. Since we need a fixed-length output starting from an arbitrary length, the hash functions **map** items from a very large space (all possible messages space) to a very small one (message digests space).

In practice how we can use digests?
Imagine we need to protect an object A (a document, for example).
1. We compute the hash of A and store it in a **secure space**
2. Now, A can be stored in an insecure place
3. Each time we need to use A, the system computes the hash of A and compares it with the stored one
4. If they are the same it's okay, otherwise we do not have message integrity

Internally, a hash function splits data in N block, then apply a certain function, combined with the previous block value:

![[Pasted image 20241011194322.png]]

Hash algorithms used nowadays are:
- SHA-224, SHA-256, SHA-384, SHA-512 (SHA-2 family, considered good)
- SHA-3 (excellent). 
The name of SHA-2 algorithms corresponds to the size of the generated digest (in bits).
As we said before, collisions must be avoided, so if the hash function is well-designed the probability to generate a collision is:
$$
P \approx \frac{1}{2^\text{N bit}}
$$
Where $\text{N bit}$ is the size of the digest, so bigger digest are better.

```ad-note
THe **birthday** **paradox** is a standard statistical problem. It shows the probability that two person in a room share the same birthday.

![[Pasted image 20241011195715.png]]

As we can see, with 23 people inside a room, the probability is around 50%.
With 70 people, the probability is almost 100%

A N-bit digest algorithm is insecure when more than $2^{N/2}$ digests are generated because the probability $P_A$ to have two messages with the same digest is $P_A \sim 50\%$.
```


# KDF (Key Derivation Function)

Typically, to gain access to a certain service, users insert passphrases guessable and not random. If we need to generate a secret key, we can **derive** it from the user's passphrases.
The **KDF** function takes three parameters:
$$
K = \text{KDF} (P, S, I)
$$
- $P$ is the user's passphrase
- $S$ is the **salt**, its purpose is to make $K$ difficult to guess, given $P$ (it adds more complexity)
- $I$ is the **number of iteration** of the base function

Internally, a KDF is based on cryptographic hash functions.

### PBKDF2

Popular KD function, it takes more than three parameters:
$$
DK = \text{PBKDF}(PRF, PWD, Salt, C, dkLen)
$$
- $DK$: Derived key from the passphrase
- $PRF$: pseudo-random function of two parameters
- $Salt$: cryptographic salt
- $C$: number of iterations
- $dkLen$: desired length of the output key

This algorithm is employed in WPA2 (wireless access)

# MIC, MAC, MID

- To guarantee the integrity of messages, a code is added to the message: **MIC** (Message Integrity Code)
- Often integrity is not useful without authentication, thus the code (ensuring both security properties) is named: **MAC** (Message Authentication Code)
- To avoid replay attacks, a unique identifier can be added to the message: **MID** (Message Identifier)

# Authentication: keyed-digest

In order to guarantee authentication while working with digest, it's possible to calculate a **keyed digest**, which is based not only on data, but also on a secret key.
only who knows the key can compare the transmitted digest with the digest calculated on the received data.

![[Pasted image 20241012101600.png]]

### HMAC

HMAC is a keyed-digest algorithm. It uses an hash function, so the correct name for HMAC algorithm is something like HMAC-SHA256.
It exploits symmetric cryptography, it guarantees data authentication and integrity.

### CBC-MAC

Imagine that we are unable to use hash function, but we need to create a keyed digest. We can use CBC-MAC, that uses symmetric algorithm to calculate a keyed digest.
It uses symmetric encryption; it guarantees data authentication and integrity.
[[#CBC (Cipher block chaining)]]
Starting from an empty initialization vector, we apply the encryption on the data:
$$
\text{for} (k=1\dots N) \, \, \, V_{k} = \text{enc} (K, M_{k}, V_{k-1})
$$
This algorithm is secure only for fixed-length messages.

# Combining secrecy and integrity

In general, with symmetric encryption we guarantee secrecy. With a keyed digest we guarantee integrity, but how can we combine them?
1. **A&E Authenticate and Encrypt** (parallel)
2. **AtE Authenticate then Encrypt**
3. **EtA Encrypt than authenticate**
Notice that incorrect concatenation of secure algorithms can lead to an insecure result!.

### Authenticated Encryption with Associated Data (AEAD)

With a single key we provide both Authentication and confidentiality.
![[Pasted image 20241013205956.png]]

# Digital signature

This approach guarantee both confidentiality and integrity of the message. That is because the signature is computed starting from the digest, that depends on the data!

![[Pasted image 20241013210214.png]]

Note: each user does not have a digital signature but a private key, which can be used to generate an infinite number of digital signatures (one for each different document).
To guarantee the binding between a key and an entity, we can look at  a **public key certificate**, digitally signed by the issues (called **Certification Authority (CA)** ).
This certificate has a limited lifetime and can be revoked at any moment from the CA or the owner.

The **receiver of a message needs to check if a certificate is valid!**

Techniques to revoke certificates: **CRL** (Certificate revoked list) and **OCSP** (On-line certificate status protocol).

# X.509 Certificates

A **Public key certificate** typically binds a key ton an identity.
There are **digitally signed** by a **CA (Certificate Authority)**, and they can be revoked on request by the user and the issuer.
A X.509 Certificate contains:
- The version of the certificate, typically 2 or 3
- The serial number
- The signature algorithm
- The issuer
- The validity (expiration date)
- Other info
- The **CA digital signature**.

A **PKI**, Public key Infrastructure put in place for the creation, distribution and revocation of public key certificates.
Any certificate can be revoked before the expiration date on request from the owner or autonomously by the issuer.

When working with certificates, it is **receiver responsibility** to check the validity of a certificate at a signature time.

![[Pasted image 20241122210217.png]]Who ensure the identity of CA?
CAs are organized in a **hierarchical** model. When we need to verify a certificate we need to go up the hierarchy until we find a **Trusted root CA** which is secure and trusted.

### Certificate revocation

- **CRL (Certificate revocation list)** is a list of revoked certificates, signed by the CA. The location of the CRL is indicated by the CRL distribution point
- **OCSP (Online certificate status protocol)**: is a protocol that tells validity ad the current time.
