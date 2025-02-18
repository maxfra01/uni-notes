# Authentication vs Identification

**Identification**, also known as recognition, has the goal of establish an **identity** from available information. It provides an answer to the question "Who are you?"

Authorization is the process of **using supported "evidence"** to corroborate an asserted (claimed) **identity**.
This means that authentication provides an answer to the question "Are you who you claim to be?"
The authentication of an "actor" is made by a user that interacts via software with hardware elements. **User authentication** is also called AuthN, which is different from AuthZ (authorization).

The authorization can use different **factors**: we typically have three levels of authentication, known as **1/2/3 factors authentication**. The three levels are:
- **Knowledge**: first factor is based on **something only the user knows** (PIN, password)
- **Ownership**: something **only the user possesses** (Smartphone, smart card, token)
- **Inherence**: something **the user is** (for example a **fingerprint**)
When more than one factor is employed we speak about **MFA (Multi-factor authentication)**.
Each factor has cons:
- Knowledge has risks related to storage and demonstration
- Ownership: theft, cloning, unauthorized use
- Inherence: counterfeiting and privacy
# Digital authentication model

This model was defined by NIST.

![[Pasted image 20241016102604.png]]

The applicant is the person that is trying to authenticate himself.
The applicant is being asked to provide an **authenticator enrollment**, if the process has success the applicant become a subscriber.
The system needs to verify if the enrollment is correctly bind to the identity.
The verifier has the goal to verify the credentials.
# General authentication protocol

1. The server send an **authentication request**
2. The user must submit his username ID
3. Then the server asks for a proof for his identity: the proof is **a function of the secret** of the users
4. The user sends a proof, based on a secret that only the user knows

![[Pasted image 20241022130353.png]]

### Password-based authentication


In this simple case, the proof function is the **identity function**: so the password is **reusable**. The password is transmitted in clear, and also the server stores all password in clear.
This technique has obviously some cons, for example it is subject to sniffing and replay attack.

A possible alternative is storing not the clear password but an **hashed password (digest)** in the database. The function is a cryptographic hash that processes the user password.
In this case we transmit the hash of the password, which is more secure.
This technique is subject to replay attacks (dictionary attacks).

Overall this password-based approach has few pros and lots of cons.
Pros:
- This mechanism is simple for the user, because he only needs to remember a password
Cons:
- A possible problem is storing the password user-side
- The password can be guessable
- Another problem is storing the password server-side
There are also known attack to this architecture: pwd sniffing, pwd DB attacks, pwd guessing, pwd enumeration, pwd duplication, cryptographic aging (new technologies and new attacks), pwd capture via spoofing and phishing, MITM attacks.

Some best practices for password-based authentication:
- Use alphabetic chars, numbers, special chars, use a long password, change it frequently, combine password with other technique such as bio-metrics.
- For the server-side storing, never storing password in clear-text. If we are using encrypted passwords then the DB needs to know the key in clear. Often we introduce an unexpected variation, known as **salt**
- For the client-side, the password must be in the head of the user, but nowadays we use too many passwords, so users could use password managers

### Using a salt

When creating a new user:
1. We ask for a password
2. We generate a random salt, unpredictable, different for each user, containing rarely user or control characters
3. We compute $\text{HP}_{\text{UID}}= \text{hash}(\text{pwd}, \text{salt})$
4. Then we store the triplets $(UID, HP_{UID}, salt)$
This technique protects from the dictionary attacks

# CRA, Challenge response authentication

1. Server sends a **challenge** to a claimant
2. The claimant replies with a response computed using some secret knowledge and a function f 
3. The server compares the response with a solution computed via data associated to the user

![[Pasted image 20241022143210.png]]

Note that the claimant **never send his secret** directly.
However it is required for the challenge to be **non-repeatable** to avoid replay attack.
The function must be **non-invertible**, otherwise a MITM attack can invert the response and gain knowledge about the secret

### Symmetric CRA

In this symmetric scenario the response is computed using a **symmetric function**, some **shared key** $K_{ID}$ and the challenge.
If the response is validated by the server, then the claimant proves that he knows the secret (shared) key.

The easiest implementation uses sha2 hash function, but the key must be known in clear text by the verifier.

### Asymmetric CRA

This approach is based on signing and sending a response based on asymmetric cryptography.

![[Pasted image 20241022144232.png]]

Pros:
- Strongest authentication mechanism
- Does not require storing a key somewhere
- Implemented in already known utilities (ssh, tls, ipsec)
- Cornerstone for authentication in FIDO
Cons:
- Compared to other techniques this is a slow approach
- PKI issues (rusted root, revocation)
- Needs to protect le list of valid users for authentication and integrity

# OTP, One Time Password

With OTP, a single password is valid only for one run of the authentication process. This approach is **immune to sniffing**, but it needs Verifier authentication (MITM). The critical problem is provision passwords to subscribers, which can be done in two ways:
- On "stupid" and insecure work station, for example passwords cards with pre-computed passwords, or crypto token
- On intelligent and secure workstation: automatically computed by an ad-hoc application, often on smartphone and tablet

![[Pasted image 20241022145929.png]]

The password depends upon time ($t$) and user’s secret ($S_{ID}$), so we can compute passwords as:
$$
\text{p(ID,t) } = \text{h(t, SID)}
$$

# FIDO

FIDO is a very recent initiative to simplify the usage of both **asymmetric cryptography and bio metrics** for authentication.
The main idea behind FIDO is to use a **personal device** (e.g. smartphone) to generate a **pair of keys** for asymmetric cryptography.

The first phase is the **registration** of a profile on a certain website: 
- If we choose to authenticate with FIDO, then we can generate the keys with our smartphone
- Then the public key is sent to the server
- The private key is stored on my device
Note that our devices possesses a certain number of private keys, each one for a specific server authentication.

![[Pasted image 20241024110214.png]]

To perform a **login** the server generate an asymmetric challenge, consisting in a random number associated with your name and encrypted with the public key. We can use the private key to solve the challenge and send the response to the server.
Of course, before initiating the challenge we need to provide a username and a password because the server needs to know who i am.

![[Pasted image 20241024110233.png]]

Many advantages in the use of FIDO:
- We are dealing with a **strong authentication** (because of asymmetric technique)
- There are **no 3rd parties** involved in the process, just the client and the server
- There are **no secret stored in served** side, only a public key
- Bio metrics information may be used only on the smartphone of the user in **local** 
- Since a new key pair is generated at each registration we have **no links between different services used by the same user**, also **no link between different accounts owned by the same user**

### FIDO 2.0

In 2017 **FIDO 2.0** was released:

![[Pasted image 20241024111242.png]]

With FIDO 2.0 we have the **Relying Party** which is running RP authentication with an app,
it can be an app or can be directly the browser, web authN JS API allows to bind the RP
app authentication to the browser. The browser is running on a platform(operating system),
the big novelty is that we have two choices, we can have the authenticator inside the same
device or I can have it external using the CTAP protocol which permit use me a roaming
authenticator, this is typically the FIDO keys which can perm