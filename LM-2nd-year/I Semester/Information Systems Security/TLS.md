# TLS protocol

First of all, let's consider the classic ISO/OSI net stack: none of the layer provides cryptography for packets. TCP layer provides **reliability** but not security.
**Attackers** can intercept bits at the **physical layer** and rebuild the original packages.

![[Pasted image 20241023112337.png]]

**TLS** (Transport Layer Security) it's a protocol for creating a secure communication channel over the Internet. TLS is a newer versione of the old SSL (Secure Sockets layer): nowadays all versione below 1.2 TLS is considered obsolete and insecure (currently we're migrating to TLS 1.3).
This new protocol is built on top of TCP, running on transport layer: it provides byte stream abstraction between the client and the server and provides security services to application data.

- TLS provides **peer authentication of servers** and optionally peer authentication of clients: this enhances protection against attackers impersonating a server.
- TLS ensures **data confidentiality**, so attackers cannot read the exchanged traffic
- TLS also ensures **data integrity and authentication**, so attackers cannot inject new data or modify it.
- TLS provides protection against **replay, reordering, and filtering/cancellation** attacks**

# TLS phases (high-level)

1. First thing to do is to open a **TCP connection**.
2. Then the **TLS handshake** is composed of:
	- authenticate server and optionally the client
	- negotiate (agree on) the cryptographic algorithms used for key exchange/agreement and data protection (MAC & encryption)
	- establish keys
3. After the TLS handshake, the **actual data are transferred** over the TLS channel that has been established.
4. The **TLS teardown** is used by the application to close the connection. Also, the teardown can be triggered by any error occurring in the previous phases.
5. At the end the **TCP connection is closed**.

![[Pasted image 20241030101747.png]]

![[Pasted image 20241030102311.png]]

### TLS Handshake

In this phase, client and server **exchange random numbers** to be used for the subsequent generation of the keys. They also agree on a set of algorithms for protecting messages and key exchange (confidentiality, data authentication & integrity) - cipher suites.
Perform key exchange/agreement: establish a SECRET (called master secret) derived from another SECRET (called pre-master secret) exchanged by means of public key operations
(DH, RSA).
From the master secret (along with the client random and the server random) the client and the server will derive via a PRF the cryptographic symmetric keys (keys for MAC, keys for
encryption) and the initialization vectors for data protection

![[Pasted image 20241030103033.png]]

### Resume a TLS session

Let's assume we want to resume a TLS connection: if the TLS cryptographic parameters must be **negotiated every time**, then the **computational load** becomes high.
In order to avoid re-negotiation of the cryptographic parameters for each TLS connection, the TLS server can send a **session identifier** (that is, more connections can be part of
the same logical session):
- if the client, when opening the TLS connection (TLS 1.2), sends a valid session-id then the peer authentication and key exchange/agreement part are skipped, keys are derived based on a previously negotiated master-secret and the (new random numbers)

```ad-note
title: TLS Session vs Connection
A **TLS Session** is a logical association between a client and a server. It may span multiple TLS connection (1:N). The session is created by the TLS handshake and it defines a **set of cryptographic parameter** common for serveral connection.

A **TLS Connection** is a transient TLS channel between a client and a server.
Each connections is associated with a single session.
The defined parameters are valid only for the specific connection.

![[Pasted image 20241030103800.png]]

```

Note that the server can reject the use of session-id (always or after a time passed after its issuance)

![[Pasted image 20241030103323.png]]

### TLS Architecture

The TLS is made of:
- Handshake
- Change cipher spec protocol (dismissed in TLS 1.3)
- Alert
- Record

![[Pasted image 20241030104043.png]]

In particular the **TLS record protocol** specifies the format to cryptographically protect the application data, handshake, alert, or change cipher spec messages.

![[Pasted image 20241030104850.png]]

With **TLS Ciphersuite** we are specifying a string containing:
- A key-exchange algorithm (RSA, DH, ...)
- A symmetric encryption algorithm
- An Hash algorithm for MAC
Example:

![[Pasted image 20241105130719.png]]

### Talking to the legitimate server

When the TLS connection is open, the server sent its certificate, so che client knows the server's public key. Then the client will sent a challenge to the server.

![[Pasted image 20241105131836.png]]

The TLS channel is secure because the attackers does not know the keys: encryption and MAC provides confidentiality.
The attacker does not know PS, so he cannot know the master secret.

### Protection against replay

How can we be sure that the attacker hasn’t replayed old records from a past TLS connection?
Every connection uses a different pair of Rc and Rs: since the keys are derived from those, **every time the keys are different**.

How can we be sure that the attacker hasn’t replayed old records from the current TLS
connection or filtered part of them?
Inside each TLS record there is a **unique sequence number**.

# Forward secrecy in TLS

If a server has a valid certificate for both signature and encryption, then it can be used both for peer authentication and also for key exchange.
But if we assume that an attacker could intercept all the encrypted traffic and discover the private server key, then the attacker could decrypt all past, present and future traffic.

What is **forward secrecy**?: the compromise of a server’s (asymmetric) private key
compromises only the current (and eventually future) traffic but **not the traffic that has been exchanged in the past**.
To achieve forward secrecy we could use **ephemeral DH**: for each session the server will generate a DH key on the fly. The other key (long term) is used to sign the temporary key.
If an attacker gain access to the temporary key, then he will be able to decrypt only the traffic of that session.

### TLS then proto

First it is established a TLS channel, then the specific application protocol is run (over the
secure channel).

TLS is often used in combination with HTTP: we add an 's' to the name of the protocol (HTTPS). Client connects to a TCP port on which a TLS process is running, e.g. a web
server expects unprotected HTTP requests on port 80, while it performs a TLS handshake after a TCP connection is established on port 443.
Nowadays it's possible to enforce a secure connection with **redirection**.

### Proto then TLS

The client first starts the the application protocol, then (when needed) server upgrades to a
secure channel through STARTTLS
1. Client established an unencrypted TCP connection
2. Server sends STARTTLS keyword over the connection
3. the client interrupts the application protocol and performs a TLS handshake on the existing TCP connection
4. When the TLS handshake is completed, the client restarts the application protocol

# HTTP over TLS

![[Pasted image 20241105133858.png]]

Typically HTTP is on port 80, while HTTPS is on 443. It is common to have **virtual servers**: different domains mapped to the same IP.
In HTTP it is simple to distinguish between servers but in HTTPS is more complex.

TLS provides **end to end security**: secure communication between to endpoints.
Also there is **no need to trust the intermediaries**: even if everybody between the client and the server is malicious, TLS provides a secure communication channel.

Using **TLS** defends **against most lower-level network attacks**.
Note: end-to-end security does not help if one of the endpoints is malicious.


