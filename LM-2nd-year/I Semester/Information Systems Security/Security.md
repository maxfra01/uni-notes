# Definition of Security

During years the concept of security has changed: there was an evolution from the concept of physical security of valuable things (money, jewels) to the concept of of security of data, systems.
The definition of security associated with the centralized control and protections can be divided into:
- **Physical** protection of the place where servers are.
- **Logical** protection, such as authentication and authorization
As computers became more dispersed: security was more an issue of keeping the data **secret** (military), **preserving** data, protecting its **integrity** (fundamental for companies).
A first massive internet attack was in 1988, known as **Morris worm**: this program auto-replicated itself and exploited known weaknesses of common utilities.
This attack disabled 10% of all internet-connected systems.
After this event the concept of security changed into the protection of systems from **attackers**; while nowadays computer security protects data.

A simple definition of security is: "Enforcing a desired security property/achieving some goal in presence of an attacker".
The **CIA Triad** is a guiding model or goals to achieve for information security:
- **Confidentiality**: protecting data from unauthorized access
- **Integrity**: data are trustworthy, completed and not been altered from unauthorized users
- **Availability**: data are accessible when you need them

Some other terminologies:
- **Incident**: a security event that compromises one of the element of CIA
- **Data breach**: an incident that results in a disclosure or potential exposure
- **Data disclosure**: a breach for which it was confirmed that data was actually disclosed (not just exposed) to an unauthorized party

# Why security is complex?

1. Nowadays we have a wide range of devices: that means different OS, hardware, software.
2. **Communication** between devices are made with lots of technologies (wireless, satellite, wired...): this means that we need to protect all links, all interfaces, but attackers always search for the weakest link.
3. **Distributed services** such as outsourcing, hosting, farming, cloud, both for computational or storage purpose. Can i trust a provider?
4. **Programming became complex**: complexity is an enemy for security
5. **Lack of data about security incidents**: not all incident are reported (for bad publicity, reputation...)
6. **Usability must be maintained**: user want simple things
7. **Costs** matter: trade-off between usability and costs

# How to secure a system?

Consider a company that provides a service: first step is to analyze the company **assets** (data, software, hardware, human resources). For each assets, the company needs to identify all possible **vulnerabilities**, that can eventually become **threats**.
After these steps described above a company will have a long list of threats and vulnerabilities.
The idea is not to analyze all threats, but consider only the threats with higher impact and higher probability.

![[Pasted image 20240924141515.png]]

### Risk Management

We introduce the concept of **Risk**: it's an index computed in the following way:
$$
\text{Risk} = \text{Impact} \cdot \text{Probability}
$$
We can visualize all risks impact and probability using a **Risk rating matrix**:

![[Pasted image 20240924142202.png]]

In a company, there are specific people to perform a **Risk Management**, which is a fundamental process in every organization. This process must be **precise and repeatable**.
**Framework** composed of 4 phases, proposed by NIST:
- Preparation
- Risk Evaluation
- Identification and implementation of countermeasures
- continuous update of the analysis

![[Pasted image 20240924144411.png]]

![[Pasted image 20240924144638.png]]

The final **audit** phase needs an external party to check if my implementation of risks management: this external person will ask the company documents related to the analysis and management phase.
The security of an organizations is divided in steps:
- **Security** **policy**: aggregate of directives, regulations, rules, and practices that prescribes how an organization manages, protects, and distributes information.
- **Threat model**: figure out who the attackers can be, and what resources they have
- **Security control and procedures**: implementation of my policies in systems
The final **goal**: no way for the attacker to violate the security policy **within threat model**.

### Security policy

It is a vital component in any company applying information security. (also a requirement of legislation)
**IT security policy** is  document spelling out the rules, expectation and overall approach that an organization (system) uses to achieve and maintain C.I.A of its data.
There is no universal model for security policy, but NIST spell out 3 types of format:
1. **Program policy**: this document contains intentions and expectation od senior management in regard to security (non technical)
2. **Issue-specific policy**: build upon generic security policy, it provides more concrete guidance on certain issue.
3. **System-specific policy**: most granular type of security policy, more tehcnical, updated frequently.

![[Pasted image 20240924150414.png]]

In conclusion, **security analysis and management** is an iterative process: design, update threat models and choose countermeasures considering technological trends and attacks evolution, audit is also periodic, technology change (algorithms, protocols), attacks are not static...
# Security properties

| Property                          | Description                                                                                                                                                                                                                           |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Authentication (simple/mutual)    | Allows to verify my identity                                                                                                                                                                                                          |
| Peer authentication               | Allows to verify that the (communicating) entity is who says it is                                                                                                                                                                    |
| Data / origin authentication      | Allows to verify that the data is coming from an asserted party                                                                                                                                                                       |
| Authorization                     | Guarantees that only authorized parties can use or access specific resources                                                                                                                                                          |
| Integrity                         | Allows to detect if data was modified, filtered, replayed (from the source to the destination, on disk,..)                                                                                                                            |
| Confidentiality, privacy, secrecy | Information is available for reading only to authorized parties. Assures that individuals control or influence what information related to them may be collected and stored and by whom and to whom that information may be disclosed |
| Non repudiation                   | Guarantees that an entity cannot deny that it has performed an action                                                                                                                                                                 |
| Availability                      | Procedures are used to keep track of who does what and when                                                                                                                                                                           |
| Accountability                    | Assures that systems work promptly and authorized entities have access to resources (services, data)                                                                                                                                  |

# Possible attacks

Before describing possible attacks, we need to clarify that:
- **Networks are still insicure** (e.g. traffic sent in clear over network)
- Geographical connections are made upon **shared infrastructure** (so, not dedicated) and third-party routers
- Authentication is often **weak**, password-based
- Software contains **bugs**
- **Malware**

### Packet sniffing (eavesdropping)

This is a **passive attack**, where the attacker can read packages **addressed to another** network node. This attack is quite easy to do in LAN networks or in switching nodes.
- For LAN networks the attacker can use promiscuous mode for his network card
- For switching node: the attacker can read the mirror port on routers
Obviously, the attacker can intercept everything (text, data, password) and also performs traffic analysis (message's length, frequency, location, peer's identity).

### IP / Source address spoofing

The attacker creates IP packets (or Layer-2 packages) which have a **modified source address** in order to either hide the identity of the sender, to impersonate another computer system, or both.
Possible attacks based on IP spoofing: DDoS, data forging (fake email for example), unauthorized access to systems.

### DoS (Denial of Service)

This attack keeps a host **busy** or **drain all its resources**, in order to **block a service** provided by that host.
Example of DoS are: ping bombing, mail/log saturation, TPC SYN attack, DNS flood.
There are no countermeasure to DoS attacks, but oversizing resources and monitoring network can mitigate the effects.

### DDoS (Distributed Denial of Service)

The distributed version of DoS consists in a program running the DoS attack (called **Daemon**, **zombie** or **malbot**) installed on many nodes, in order to create a **botnet**.
Daemons are controlled remotely by a **master**, which has to synchronize the daemons.

![[Pasted image 20240924155838.png]]

DDoS can be performed through IP spoofing, let's suppose that an attacker has spoofed an IP address (the victim's). The attacker can send multiple request to a server that will send tons of data to the victim's machine: if addresses are continuously randomized, malicious request can be difficult to block.

### Shadow servers

The attacker manages to show himself to victims **as a service provider*** without the right to do so. For example we can have DHCP fake server or a DNS shadow server.
Possible attacks are providing "wrong services" or capture sensible data provided to the fake service.
A possible countermeasure is **server authentication**.

To perform DNS shadow attack the attacker needs to:
1. Perform packets sniffing to intercept DNS queries
2. Perform data spoofing to generate fake DNS responses
To succeed, the fake DNS must be faster than the real one, or the real DNS must be out of service (may due to DoS attack)

### ARP poisoning

To perform this attack, a host has to accept an ARP response (sent by the attacker) without an explicit request.
If the host accepts the response, it will overwrite his ARP cache with the malicious MAC address.

### Man In The Middle

The attacker **takes control of a communication channel** to read, insert, delete, block, or manipulate the traffic. Sometimes called connection hijacking or data spoofing.
Can be logical or physical MITM.
Possible countermeasures are confidentiality, authentication, integrity, serialization of **each** package.

# Software vulnerabilities and malware

**Common Vulnerabilities and Exposures** (**CVE**) is a maintained database containing standardized description of public vulnerabilities in software.
For each vulnerability, the NVD (National Vulnerabilities database) assigned a **CVSS**, a **severity score**.

![[Pasted image 20240925105842.png]]

How can attackers find these vulnerabilities?
1. They start scanning for reachable IPs and open ports
2. Then they move to analyzing operating systems and services
3. Then they run test for specific software vulnerabilities

How can defenders react to attacks?
1. Zero day initiative: for example third party acting between who discover the vulnerability and vendors.
2. Vulnerability management: process aiming to discover, manage, apply patches on vulnerabilities
3. MITRE ATT&CK: it's a knowledge base that contains a set of tactics and techniques used by attackers to accomplish a specific object

Here's a list of malware:

### Virus and worm

A **Virus** is a program that is able to infect other programs or files by modifying them to include a possible evolved copy of itself.
A virus attach itself to a host program (or file) that have some form of executable content.
Replication is propagated by **humans (involuntary)** clicking on links or using USB.
Viruses require complicity (that can be involuntary); possible countermeasures:
- User awareness
- Correct configuration of systems
- Antivirus program (updated)

A **Worm** is similar to a virus, but it does damage because it replicates itself until resource saturation. Differently from viruses, worms propagate autonomously and continuously.

### Stealth malware

**Trojan horse**: it's a software that delivers malicious functionalities instead of(or in addition to) correct functionalities. They may perform good actions while performing malicious actions in the background.

**Backdoor**: It's a way to access a program bypassing normal entry points; allows ongoing stealthy remote access to a machine, often by enabling a network service.
Can be stand-alone or integrated into a program.

**Rootkit**: hidden and stealth modified program (library, driver, kernel module, hypervisor) that aim to control a part of the OS or a program

**Keylogger**: malicious program that collect all keystrokes of the user and send them to the attacker (can be used to capture sensitive data)

**Ransomware**: attack where threat actors take control of a target’s assets and demand a ransom in exchange for the return of the assets’s availability and confidentiality.
For example, it can be the encryption of a laptop disk.

# Social engineering

Computer security is not always about computers: humans play a significant role in security, and often they are the **weakest link** of security chain. That's because humans have natural tendency to trust, to make errors, and they have low awareness of security problems.
Social engineering is a **psychological manipulation of people into performing actions or disclosing confidential information**.
- Preferred target is a "naive" users
- Also experienced users are targeted
Attacks are made via mail, phone, or even paper.

An example of social engineering technique is **phishing**: it involves psychological pressure, fear, promise of money or similar...
The goal of phishing is capturing sensitive data or to convince victims to install malware.
Social engineering attacks are not only "digital": baiting and tailgating.

# Security principles

![[Pasted image 20241001141409.png]]

### Simplicity
The goal is to keep designs (both in hardware and software) as **simple and small** as possible. This can be achieved by eliminating unnecessary complexity.
The more complex the design is, the more likely it is to possess exploitable flaws.
"The more complex a system is, the more difficult will be to check its correctness"

### Know your threat model
**Threat model** means that companies need to know who their attackers are and what resources they have: to do so, it's important to know attacker's motivation (MICE):
- **Money**
- **Ideology**
- **Compromise**
- **Ego**

### Open-design
It's a good practice to not rely on secret designs, attacker ignorance, or security by obscurity. We must assume that attackers knows our systems: this invite and encourage open review and analysis.

### Security has a cost
**cost/benefit analysis**: the expected benefit of your defense should be proportional to the expected cost of attack. More security (usually) costs more; If the attack costs more than the reward, the attacker probably won’t do it.
Someone needs to take a decision about this trade-off.

### Defense in depth
Security layers need to stacked: an attacker can access a system only if he successfully breaks into all layer.
But we need to consider that security has a cost

### Least privilege
Security needs to consider what permissions an entity or a program should have.
Then grant only those permissions and for the shortest duration necessary! (no more)

### Separation of privilege
If you need to have a privilege, consider requiring multiple parties to work together to exercise it.

# Stuxnet

It was a prototype of a new kind of attack that affected the physical world, in particular **SCADA systems** attached to infected nodes.
This attack exploited a virus and a worm for Windows.
The target was a nuclear plant in Iran, completely disconnected from internet.

Attack and propagation vector:
- 1 known vulnerability (a patch was available)
- 1 known vulnerability (no patch)
- 2 "zero-day" vulnerabilities

Distribution and propagation:
- USB key as initial attack vector
- shared disks (network share)
- MS-RPC and MS-spool bugs

Likely the first infection via an USB key of maintenance technician disguised a driver with a digital signature validated by Microsoft! (used two different certificates)
This driver gained access from the infected node to the back-end DB thanks to a shared default password on every node.

One important error of this plant was the absence of standard protections (other than physical separation of the plant): no anti-virus, no firewall, no separation of privilege...
There were also unnecessary active services.
