# Firewall

A firewall **is a controlled connection between networks at different security
levels**, in other words it's a boundary protection (network filter).
It filters the packets in transit, allowing only the ‘good’ traffic to pass.
The objective it to block the propagation of the attacks originating from the network that has has a lower security level.

A firewall is configurable to filter traffic **according to some policy**:
- an **outbound policy** decides which traffic can exit the network
- an **inbound policy** decides which traffic can enter the network
Remember that this policies are based on **threat model**, where we assume that the users inside network are trusted.

We can make a first distinction on firewall:
- **Egress firewall**: it controls the outgoing traffic, especially to avoid connections to malicious websites or to block employees to access websites not related to work
- **Ingress firewall**: it controls the incoming connections. Typically serves to select the (public) services offered (e.g. Web server)

How can we **determine the direction of a connection?** If the services use some kind of channel then it's very easy (for example for TCP, just look for the first party that initiated the 3-way handshake). Instead, for services that do not exploit connections it's more difficult (e.g. UDP services).

A firewall is not a product that can be purchased: it needs to be designed according to the needs of a company. We need the achieve an **optimal trade off between security and functionalities** (with minimum cost). We need to follow this basic three rules:
- The FW must be the only contact point of the internal network with the external one
- Only the “authorized” traffic can traverse the FW
- The FW must be a highly secure system itself

### Policies

Two basic approaches:
- **Default-deny policy**: by default, **every network service is denied**, unless is has been specifically listed as allowed (allow-list). We start off with a list of few known services that need to be visible to the outside world (judged to be reasonably safe). Users will be denied access to any service not on the list. It's **less convenient** (services really needed in a company might be blocked) but is more secure: if some service (that is safe) has been mistakenly omitted from the allow-list, then **the result is a loss of functionality (availability)** but NOT a security breach
- **Default-allow policy**: by default, **every network service is allowed**, unless is has been specifically listed as denied (deny-list). We start off by allowing outside users access to all internal services, and then block a few that are known to be unsafe (block-list). It requires knowing which services are potentially dangerous (e.g. affected by bugs) in order to block them. It's **more convenient** (everything typically stays working) but is more dangerous: if there is some dangerous service (present or future) that you ‘forget’ to add to the deny-list, then **the result will be a security failure/breach**

### Types of Firewall

![[Pasted image 20241112135721.png]]

A first type of firewall is the **packet filter**, it's historically available on router and **manually configured**. It checks only the IP and TCP/UDP header, comparing the information with a list of **condition/action** rules. where actions can be accept, reject, drop...
Each packet is processed independently from the others

| Pros of packet filter       | Cons of packet filter                       |
| --------------------------- | ------------------------------------------- |
| Independent of applications | Difficult to support dynamic ports services |
| Good scalability            | Easy to fool                                |
| Good performance            | Complex to configure                        |
| Low cost                    | Difficult to make authentication            |

The **application level gateway firewall** is composed by a set of app-level filters: **it inspects** the data at application level (**payload**). This completely breaks the client server model

![[Pasted image 20241112140435.png]]
Pros:
The rules are more fine-grained and simpler.
It may optionally mask the internal IP addresses, provides more protection for the server (against attacks) and may authenticate the client.

Cons:
each application requires a new proxy: so there is a delay in supporting new applications. Often, client modifications are needed
There are also problems with application-level security techniques that do not support
traffic inspection (e.g. TLS)
It's heavy on resources (many proxies mean many processes are running) and low on performance. 


### Screening router architecture

This architecture **exploits the router to filter the traffic at IP** (screening router) and some info from upper levels (e.g. port, protocol): no need for dedicated hardware and proxy, hence no need to modify the applications.
It's simple, easy, cheap and **insecure**: cannot prevent attacks that employ **application- specific vulnerabilities** (e.g. cannot block specific commands). If an application is allowed, all functions within that application will be permitted.

![[Pasted image 20241112141446.png]]

In general we cannot prevent attacks by blocking IP addresses. Moreover there is no user authentication and limited logging resources.
If the packet filter is badly configured we talk about **SPOF**  (due to security bugs/improper configuration of the screening router)
### Dual-homed gateway architecture

This architecture is composed by a **screening router** and a **dual-homed gateway**: a DHG is basically a node with **two network cards**(one towards the external network and the other towards the internal one) and a running **process that decides** which traffic is allowed to pass from a network interface to another.

![[Pasted image 20241112141935.png]]

Doing this way we achieve:
- **Double controls**: both on IP and Application level
- **Easy to implement**
- **Small hardware changes**
- Internal network can be **masquerade**
However:
- It is **unflexible**: all packets are checked twice, even if not necessary
- There a **large work overhead** on the DHG, since it needs to check packets for all applications

### Screened host architecture

In this architecture there is a **bastion** which is a trusted node that runs circuit/application gateway to control the authorized services.
The router blocks traffic INT > EXT unless from the bastion, and blocks traffic EXT > INT unless goes to the bastion.
- More expensive and complex to manage (two systems rather one)
- More flexible
- SPOF
- The host(s) that are directly reachable are potentially subject to some of the attacks that occurred in the screened router architecture (e.g. DOS due to IP spoofing)
So we gained in performance (not all the traffic is checked) but we lost on security.

### Screened subnet architecture

Also called three-legged Firewall, it exploits a DMZ which is the home of the gateway and for public servers.
There is a packet filter from the DMZ to the internal LAN and also a packet filter towards the external network.

![[Pasted image 20241112143155.png]]

It's expensive, because we need three different devices.

### Local firewalls

Local/personal firewalls are **software installed directly on nodes**: w.r.t. the network firewall, this one **limits the processes that try to open connection** with other clients or acting as a server.
It's important to **limit the diffusion of malware and trojans**, or plain configuration mistakes.
In order to be effective, the firewall management must be separated from the system management: **separation of privileges** principle.


# Intrusion Detection Systems (IDS)

By definition, an IDS is a system to **identify individuals using a computer or a network
without authorization**. This definition can be extended to identify authorized users violating their privileges
IDS are based on the following hypothesis: the behavioral **“pattern” of non-authorized users differs from that of the authorized ones**.

IDS can be divided into:
- Passive if they simply perform pattern matching or cryptographic checksum
- **Active IDS** if they actually learn systems behavior, monitor them in real time and perform reactions based on comparison against statistical parameters 

From the topological point of view, we can divide IDS into:
- **Host IDS** (HIDS): they use log analysis and OS monitoring tools
- **Network IDS** (NIDS): they use network traffic monitoring tools

In particular a NIDS is composed of a **sensor** (checks traffic, actions, and logs looking for suspect patterns, generated the relevant security events), a **director** ( coordinates the sensors, manages the security database) and a **IDS message system** (secure and reliable communication among the IDS components)

![[Pasted image 20241112145713.png]]
