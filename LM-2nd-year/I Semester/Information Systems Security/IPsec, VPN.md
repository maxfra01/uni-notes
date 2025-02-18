# Need for more security

Which OSI layer is the most appropriate to implement security services?
The lower we go in the stack, the more quickly we can “expel” the intruders … but the fewer the data for the decision (e.g. only the MAC or IP addresses, no user identification, no commands). The upper we go in the stack, the more specific are the security functions (e.g. it’s possible to identify the user, commands, data) and independent from the underlying
network, but application(s) and/or OS must be changed to support security.

If we choose application layer security we have some disadvantage like application-dependency, hard implementation ecc...
If we add security at transport layer it's easier to implement and widely available.
However headers are still exposed, it requires changing in applications, and it protects only end to end.

A more crucial level is network layer (3): it is useful to create VPN.

# Virtual Private Network (VPN)

VPNs are technique that allow the creation of a **private channel over a shared (or untrusted) public channels** and transmission devices.
Why do we need VPN? Because IP was born without accounting security, in fact IP is connectionless, unreliable, does not guarantee order of packets, it is subject to sniffing and spoofing.
We can create a VPN via:
- Private addressing
- Protected routing
- **Cryptographic protection of the network packets (secure IP tunnel)**

In this third technique, **before encapsulation**, packets are protected with:
1. MAC (Integrity + Authentication)
2. Encryption (Confidentiality)
3. Numbering (to avoid replay attacks)
Ff the cryptographic algorithms are strong, and the keys are secure then **the only possible attack is to stop the communications**. This process is also known as **S-VPN (Secure VPN)**.

![[Pasted image 20241105144052.png]]
# IPsec

IPsec is an internet standard for **packets security in IPv4 or IPv6**. There are several components. First of all we have an **Arch** **architecture**. Then two "pseudo **headers**" are defined: **authentication header** and **encapsulating security payload**. Nowadays ESP is extended and provides also the benefits of AH.
Another component is a protocol for **exchanging keys: IKE** (Internet Key Exchange).

Services provides by IPsec:
- **Authentication od IP Packets** (data integrity and data (source) authentication, partial protection against replay)
- **Confidentiality of IP Packets**
Algorithms, keys and other security parameters of IPsec are negotiated ahead of time between sender and receiver of IP stream (manually, or by using IKE protocol).

### Security Association

The security properties of the secure IPsec channel created between the sender and the receiver **are stored in a SA structure**:  the SA is **unidirectional**, it contains all the (security) data necessary for IPsec processing of outgoing and incoming packets.
An **IPsec device** stores all active SA it currently has in a database, called **SAD** (logically divided into outgoing and incoming).
- each SA may have (associated) different security services
- **two SA are needed to get complete protection** of a bidirectional packet flow

![[Pasted image 20241105145411.png]]

The **SAD** contains all active SA and their characteristics.
The **SPD** (Security Policy Database) is a list of security rules to apply to the different packet flows, set by user. Each rule contains a set of selectors and an action. Rules are scanned
by their order and a packet is processed according to the first match.

![[Pasted image 20241105145651.png]]

So when we need to send a package, we first look up in the SPD to choose the policy to apply to that package. Then we look for the algorithm and parameter to apply (so we scan the SAD or we create a new entry).

### IPsec modes of transport

In **Transport mode** the IPsec encapsulation is done by the source of the packets (the sender) and the de-encapsulation is made by the receiver. (**end to end**)
In this case an IPsec header (e.g. AH) is inserted between the original IP header and the original IP payload
- if IPSec header is ESP, a trailer is placed after the original IP payload
- pro: computationally light 
- con: no protection of header variable fields

![[Pasted image 20241105150159.png]]


In **Tunnel mode** the IPsec encapsulation and de-encapsulation are made by gateways along the route between source and destination. This technique is used to create a VPN, (usually) by gateways
- pro: protection of header variable fields
- con: computationally heavy

![[Pasted image 20241105150425.png]]

### AH

The Authentication header provides data integrity, sender authentication and partial protection from replay attack.
The format of AH is the following:

![[Pasted image 20241105154007.png]]

- Next protocol is the **name of the protocol inside AH**
- Payload length is the **length of AH**
- **SPI** points to the SA in the SAD
- Authentication data **ICV**: result of MAC

The authentication data (MAC) is computed over the all fixed fields in the IP packet (including fields in the IP header, fields in the AH header, and the payload, e.g. TCP/UDP header + data).
Before the MAC calculation, all the unpredictable or mutable fields (e.g. TTL) in the IP header are set to 0, while all the predictable fields (e.g. source routing) are set to their arrival value: this process is called **normalization**.

A sender:
1. Locate the outgoing SA in the SAD (if not found drop the packet)
2. Set the SPI in the AH header (also perform normalization)
3. Increment the sequence number
4. Calculate ICV and send

A receiver:
1. Locate the SA according to SPI
2. Check sequence number
3. Calculate authentication value and check with the received value.

![[Pasted image 20241105154419.png]]

Note: 'Authentic server' means that the person who sent the packet is the same who negotiated the key for SAD.

### ESP

ESP header provides both **confidentiality** and **authentication** (not on the IP header).

In the Transfer mode:

![[Pasted image 20241105155339.png]]

- pro: the payload is hidden (including info needed for QoS, filtering, or intrusion detection!)
- con: the header remains in clear

In the Tunnel mode:

![[Pasted image 20241105155402.png]]

- pro: hides both the payload and (original) header
- con: larger packet size

The format of ESP is:

![[Pasted image 20241105155442.png]]

### IPsec replay protection

When a SA is create, the sender initializes the sequence number to 0. **When sending a packet, he increments the sequence number per outgoing packet**.
The sequence number cannot be modified by an attacker because it is protected bu the MAC calculated for AH or by ESP. When a certain number is reached ($2^{32}-1$), a new SA should be negotiated.

At the **receiver**, he checks if it's the **first time he encounter the sequence number**.
To avoid discarding correct package arrived in an **out-of-order** fashion, the receiver maintain a 32 packet **sliding windows**

![[Pasted image 20241109150731.png]]

### IPsec v3

IPsec has been further improved: in the third version now **AH is optional, while ESP is mandatory**. They also introduced **support for single source multicast**.
The also **extended the sequence number to 64 bits**.
- It also has Support for **authenticate encryption**
- Clarification about SA and SPI (**faster lookup**)

# End to end security recap

![[Pasted image 20241109151543.png]]

![[Pasted image 20241109151602.png]]

### Vs VPN security

![[Pasted image 20241109151632.png]]

### Combined approach

![[Pasted image 20241109151707.png]]

![[Pasted image 20241109151727.png]]

### IPsec key management

A very crucial component of IPsec are the keys provided to the two entities used for authentication or encryption.
For key distribution we can use the **Internet Key Exchange (IKE)**: it's a standard protocol for SA establishment and management. It can be executed between two hosts, or two gateways, or both. The protocol exploits the Diffie Hellman protocol for authenticated key exchange.

The IKE is composed of two phase:

![[Pasted image 20241109152639.png]]

