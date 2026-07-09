# 🌐 Network Ports

<p align="center">
  <img alt="Networking" src="https://img.shields.io/badge/Topic-Networking-blue">
  <img alt="Level" src="https://img.shields.io/badge/Level-Beginner-brightgreen">
  <img alt="Cloud" src="https://img.shields.io/badge/Cloud-AWS-orange">
  <img alt="Language" src="https://img.shields.io/badge/Language-English-lightgrey">
</p>

> A beginner-friendly guide to understanding Network Ports, how services communicate, and why ports are essential in networking and AWS.

---

## 📑 Table of Contents

- [1️⃣ Simple Explanation](#1️⃣-simple-explanation)
- [2️⃣ Everyday Analogy](#2️⃣-everyday-analogy)
- [3️⃣ Diagram](#3️⃣-diagram)
- [4️⃣ Common Well-Known Ports](#4️⃣-common-well-known-ports)
- [5️⃣ Real-World AWS Example](#5️⃣-real-world-aws-example)
- [6️⃣ Linux Example](#6️⃣-linux-example)
- [7️⃣ Common Mistakes](#7️⃣-common-mistakes)
- [📝 Quick Summary](#-quick-summary)

---

# 1️⃣ Simple Explanation

Every device on a network has an **IP address** that identifies the device.

But a single device can run many applications at the same time:

- A web server
- An SSH server
- A database
- A mail server

How does the operating system know which application should receive incoming data?

That's the job of a **Port**.

A **Port** is a numbered communication endpoint that tells the operating system which application or service should receive the network traffic.

Think of it this way:

- **IP Address** → Which device?
- **Port** → Which application on that device?

Without ports, every application would receive all incoming traffic, making network communication impossible.

---

# 2️⃣ Everyday Analogy

Imagine an apartment building.

The building's address is:

```
15 Olive Street
```

This is like the **IP Address**.

Inside the building there are many apartments:

```
Apartment 101
Apartment 102
Apartment 103
```

Each apartment represents a **Port**.

A mail carrier can find the building using its address, but they still need the apartment number to deliver the package to the correct resident.

Likewise:

```
IP Address = The building

Port = The apartment number
```

---

# 3️⃣ Diagram

```
                    Internet
                        |
                        |
                 18.200.50.100
                  (Server IP)
                        |
      ---------------------------------------
      |          |          |              |
      |          |          |              |
   Port 22    Port 80    Port 443     Port 3306
     SSH        HTTP       HTTPS        MySQL
      |           |           |            |
      |           |           |            |
   OpenSSH      Nginx       Nginx        MySQL
```

A single server can run many services because each one listens on a different port.

---

# 4️⃣ Common Well-Known Ports

| Port | Protocol / Service | Purpose |
|------:|--------------------|---------|
| 20/21 | FTP | File Transfer |
| 22 | SSH | Secure remote login |
| 23 | Telnet | Remote login (unencrypted) |
| 25 | SMTP | Sending email |
| 53 | DNS | Domain Name Resolution |
| 67/68 | DHCP | Automatic IP assignment |
| 80 | HTTP | Web traffic |
| 110 | POP3 | Receive email |
| 143 | IMAP | Receive & synchronize email |
| 443 | HTTPS | Secure web traffic |
| 3306 | MySQL | MySQL Database |
| 5432 | PostgreSQL | PostgreSQL Database |
| 6379 | Redis | Redis Database |
| 27017 | MongoDB | MongoDB Database |
| 3389 | RDP | Remote Desktop (Windows) |

> 💡 The most important ports for beginners are **22**, **53**, **80**, **443**, and **3306**.

---

# 5️⃣ Real-World AWS Example

Suppose you launch an EC2 instance with the following Public IP:

```
3.120.55.10
```

If someone opens:

```
http://3.120.55.10
```

their browser automatically connects to:

```
Port 80
```

If they open:

```
https://3.120.55.10
```

their browser connects to:

```
Port 443
```

If you connect using SSH:

```bash
ssh ubuntu@3.120.55.10
```

SSH automatically uses:

```
Port 22
```

AWS controls access to these ports using **Security Groups**.

Example:

| Port | Source | Purpose |
|------|--------|---------|
| 22 | Your IP only | SSH |
| 80 | 0.0.0.0/0 | Public Website |
| 443 | 0.0.0.0/0 | Secure Website |

---

# 6️⃣ Linux Example

To see which ports are currently listening:

```bash
ss -tuln
```

Example output:

```
Netid State  Local Address:Port

tcp   LISTEN 0.0.0.0:22
tcp   LISTEN 0.0.0.0:80
tcp   LISTEN 0.0.0.0:443
```

Meaning:

- SSH is listening on Port 22
- HTTP is listening on Port 80
- HTTPS is listening on Port 443

---

# 7️⃣ Common Mistakes

- ❌ Thinking that an IP address alone identifies a service.
- ❌ Opening **all ports** to the internet (`0.0.0.0/0`).
- ❌ Allowing SSH (Port 22) from anywhere instead of restricting it to trusted IP addresses.
- ❌ Confusing **HTTP** with **Port 80**. HTTP is a protocol; Port 80 is the default port it uses.
- ❌ Assuming a service is unreachable because it's down, when the real issue is that the firewall or Security Group is blocking the required port.

---

## 📝 Quick Summary

| Concept | In short |
|---|---|
| IP Address | Identifies a device on a network |
| Port | Identifies a service running on that device |
| HTTP | Uses Port 80 |
| HTTPS | Uses Port 443 |
| SSH | Uses Port 22 |
| DNS | Uses Port 53 |
| Security Group | AWS virtual firewall that controls allowed ports |
| One Server | Can host many services by using different ports |

---

<p align="center">✍️ Part of a Networking & Cloud Notes series</p>
