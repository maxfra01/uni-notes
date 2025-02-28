# Remote Access Systems: Old and New

In the early days, **remote access** relied on **dial-up lines** using a **Network Access Server (NAS)**. Here, communication happened over switched networks like **RTC** or **ISDN**, then connected to an IP network, typically the **internet**. This setup was simple but not very flexible or secure by modern standards.

Now, **modern network access control (NAC)** systems are significantly more sophisticated. They manage access through various technologies like **Ethernet**, **WiFi**, or **mobile data** (**3G, 4G, 5G**). Key to this setup is the **AAA server**, which handles **Authentication, Authorization, and Accounting** to ensure secure, verified access. A common scenario involves users connecting to an **access point** that communicates with this AAA server for access permissions, whether they’re at home or on the go.

![[Pasted image 20241120103953.png]]

# PPP Authentication Protocols

To achieve authentication we can use the **Point-to-Point Protocol (PPP)**. It plays a central role in encapsulating network packets and carrying them over links. These links could be **physical**, **virtual**, or even **logical links**. PPP authentication occurs in three stages: first, a handshake using the **Link Control Protocol (LCP)**, then actual **user authentication**, and finally **Layer 3 encapsulation**.

Authentication within PPP can happen in various ways. **PAP (Password Authentication Protocol)** simply sends passwords in clear text and is now considered obsolete. **CHAP (Challenge Handshake Authentication Protocol)** is slightly better, introducing challenges during transmission but still lacks flexibility. 

**EAP (Extensible Authentication Protocol)** represents the modern standard, offering support for methods like **OTPs** and **TLS**, enabling secure and robust authentication.

### EAP

The **Extensible Authentication Protocol (EAP)** is a versatile framework designed for **network access authentication**. What sets it apart is its ability to accommodate various **authentication mechanisms** like **MD5 challenges**, **OTPs**, and even **TLS**. EAP is not tied to any specific **link type**—it works with **point-to-point**, **LAN**, and **wireless setups**.

![[Pasted image 20241120104032.png]]

EAP’s architecture emphasizes flexibility. For example, methods like **EAP-TLS** use **certificates** for authentication, while **EAP-TTLS** protects methods by tunneling them over TLS. There’s also **EAP-GPSK**, which relies on **pre-shared keys**, and **AKA-SIM**, particularly useful for **mobile networks**.

# The AAA Model and Network Access Control

The **AAA framework**—**Authentication, Authorization, and Accounting**—is fundamental to secure **network access**. **Authentication** verifies a user’s identity, **Authorization** determines their access level, and **Accounting** tracks resource usage. **AAA servers** like **RADIUS** or **DIAMETER** implement these functions and interact with **NAS devices** to enforce them.

**RADIUS (Remote Authentication Dial-In User Service)** has been a cornerstone in **network security** since 1991. It supports both **physical** and **virtual port access**, with a **client-server model** between NAS and authentication servers. It uses **attributes** in a **TLV format**, making it extensible without major modifications. RADIUS also has robust **failover mechanisms** like secondary servers and **timeout-based retransmissions**.

![[Pasted image 20241120104215.png]]

While RADIUS is effective, **DIAMETER** improves upon it, particularly in **roaming scenarios** between **ISPs**. It adds stronger **security** and **scalability** features. **TACACS+** offers another alternative, with technical advantages over RADIUS, but it’s less widespread due to its proprietary nature.

### Strengths and Weaknesses of RADIUS

RADIUS ensures **security** using **keyed MD5** for packet integrity and **password encryption**. However, it’s not without vulnerabilities. For instance, attackers might intercept **requests** or fake **responses** to allow unauthorized users. **Replay attacks** and **password enumeration** are additional threats. Protections like authenticating NAS devices and securing communication using **shared secrets** mitigate these risks.

Attributes within RADIUS are extensible. For example, **usernames**, **passwords**, and **challenges** are passed in encrypted forms, and access is granted or denied based on these parameters. The protocol also supports advanced setups where it acts as a **proxy**, forwarding requests to other authentication systems like **Windows domain controllers** or **UNIX NIS servers**.

# IEEE 802.1X for Network Access

**IEEE 802.1X** is a **port-based network access control protocol** designed to authenticate devices before allowing them access to a **network**. This is particularly critical in **wireless networks**, where uncontrolled access is a major vulnerability. Early implementations appeared in **Cisco** and **Windows XP** systems, setting the stage for modern adoption.

![[Pasted image 20241120104256.png]]

The protocol uses **controlled** and **uncontrolled ports**. Uncontrolled ports allow only **authentication-related traffic**. Once authentication succeeds, the controlled port opens, granting full network access. This standard integrates well with **EAP** and **RADIUS**, ensuring robust authentication and secure communication in environments like corporate networks or public WiFi, as seen in **eduroam**.

### DHCP and ICMP: Securing Core Network Functions

**DHCP** is a vital but insecure protocol, enabling devices to obtain **network configurations**. Attacks like **rogue DHCP servers** can result in **MITM scenarios** or malicious redirects. Though **RFC-3118** proposes **HMAC-MD5** for message authentication, adoption is rare due to challenges in **key distribution**.

**ICMP**, essential for **network management**, suffers similar issues. Without authentication, it’s vulnerable to **smurfing attacks**, where attackers flood victims using **broadcast messages**. Defenses involve **blocking broadcast packets** and monitoring for **internal threats**.

![[Pasted image 20241120104339.png]]

### Advanced Threats: SYN Flooding and IP Spoofing

**SYN flooding** is a classic **denial-of-service attack**, overwhelming servers by exhausting **connection tables**. Solutions range from reducing **timeouts** to employing **SYN interceptors** or **cookies**, which validate connections without taxing server resources.

![[Pasted image 20241120104407.png]]

**IP spoofing**, another common attack, involves falsifying **packet source addresses**. **RFC-2827** recommends **ingress filtering** to prevent such attacks, ensuring that packets with invalid source addresses are blocked. This not only protects internal systems but also prevents outgoing spoofed packets from impacting others.

### DNS Security and the Case for DNSsec

**DNS**, the backbone of **internet navigation**, lacks inherent security. This makes it vulnerable to **cache poisoning**, where attackers insert malicious entries into **DNS caches**. In response, **DNSsec** introduces **cryptographic measures** to ensure integrity and authenticity. **Kaminsky’s 2008 discovery** of a critical **DNS vulnerability** accelerated DNSsec adoption, making it mandatory for **government domains** in the U.S.t