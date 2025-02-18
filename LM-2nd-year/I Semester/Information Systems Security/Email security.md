# Message Handling System

When using email we are using what is called **MHS (Message Handling System)** composed of modules: **MUA** (Message User Agent), **MSA** (Message Submission Agent), **MTA** (Message Transfer Agent) and **MS** (Message Store).

![[Pasted image 20241127154802.png]]
This above is a **logical view** of how e-mails work.
- The MUA is the part (the software) that directly interacts with the user
- The MSA is the logical part which is responsible to sending emails
- The MTA can be seen a transfer agent that exchange information with other MTA.
- THE MS is a "staging area" where emails are stored.

In practice several protocols help the communication between these modules. For example the SMTP is the protocol used for sending an email to a mail server (MSA).
In general SMTP is used for **pushing** information to servers, to MTA, to MS..., while POP or IMAP are used as pull operation, for example to retrieve the mails from your post Office (MS).
Nowadays this is the current infrastructure:

![[Pasted image 20241127155649.png]]

In this case we are working with a **Virtual MUA**, installed on a server, accessible through a web browser. In the past, the MUA was installed directly on the PC (e.g. Thunderbird). 


The **SMTP (Simple Mail Transfer Protocol)** runs on TCP on port 25 (called MTA), while when running on port 587 is MSA
The **POP (Post Office Protocol)** runs on TCP port 110, and is used to retrieve emails from the storage. **IMAP (Internet Message Access Protocol)** runs on port TCP 143 and is similar to POP.
The **format of emails** is composed of **pure text** (RFC-822), to handle multimedia content the **MIME protocol** is needed.

The original **RFC-822 standard** has only US-ASCII characters, each one encoded on 7 bits, because the MSB is used for **parity check**. The message is composed of a header (with keywords) and the body (the message itself) separated by a blank line.
In the header we can find: From, Sender, Organization, To, Subject, Date, Received, Message-Id, CC, Bcc, Return-Receipt-To.

# Email security

Several problems with emails: we have **untrusted MTA**, security of **MS**, email encryption, connectionless system, ensuring compatibility with what is already installed.

A relevant problem is **email spamming**, also names UBE or UCE, which is the procedure of sending unwanted messages (composed of unauthorized advertisement or a malware/phishing attacks). The opposite of spam is "ham".
Some spamming strategies:
- Hide the real sender, but using a valid one
- Use a special MTA for sending spam emails
- **Content Obfuscation**

Many organizations use a **mail relay**, which is a node that performs filtering of emails to /from members users of that organization.
We call an **open mail relay** a node that accepts email to/from not only authorized users. A possible way to configure a relay is to perform **authentication of users by MSA**, that could be done via:
- Ip addresses of the MUA, however there are problem with mobile nodes, dynamic IP...
- Value from the "FROM" field, but this can be easily eluded
- SMTP authentication, that requires a specific protocol

The **ESMTP** protocol adds client authentication before mail communication. The SMTP Auth module uses the command AUTH plus the MAIL FROM option to authenticate a client **before accepting message from it**.
1. After the initial **EHLO** command, the server sends the authentication mechanism supported
2. The client chooses one
3. The authentication process is executed
4. If it fails, then the communication channel is closed
**CRAM-MD5** and **DIGEST-MD5** are some the challenge-response methods used into ESMTP.

Nowadays it is also possible to send mails over TLS channels. To start the protocol we can use the EHLO command with **option** **STARTTLS**: if  the TLS negotiation is successful, the protocol status is reset (starts again from EHLO and the extensions supported can be different). If the negotiated security level is insufficient the client sends immediately QUIT and closes the connection and the server responds to each command with code 554 (refused due to low security).

### Security Services for e-mail

The goal of security is to get protection for:
- **Integrity**
- **Authentication**
- **Non repudiation**
- **Confidentiality**

To ensure confidentiality we could use both symmetric and asymmetric strategies but we would like to use a different key for each email.


# MIME

![[Pasted image 20241207162948.png]]

This extension allows new data encodings, for example we could use new chars (not just ASCII). Also the email message could be composed of different distinct part, even in a recursion scheme, where a subpart is a multipart object.
The protection of MIME is ensured by **S-MIME**:

![[Pasted image 20241207163252.png]]

