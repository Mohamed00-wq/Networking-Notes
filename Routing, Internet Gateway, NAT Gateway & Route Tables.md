# 🌐 Routing, Internet Gateway, NAT Gateway & Route Tables

<p align="center">
  <img alt="Networking" src="https://img.shields.io/badge/Topic-Networking-blue">
  <img alt="Level" src="https://img.shields.io/badge/Level-Beginner-brightgreen">
  <img alt="Cloud" src="https://img.shields.io/badge/Cloud-AWS-orange">
  <img alt="Language" src="https://img.shields.io/badge/Language-English-lightgrey">
</p>

> A simplified, illustrated summary of Route Tables, Internet Gateways, and NAT Gateways — from core theory to a real AWS example.
>
> These four topics are closely tied together, and are best understood as one integrated piece — which is exactly how they work together in AWS.

---

## 📑 Table of Contents

- [1️⃣ Simple Explanation](#1️⃣-simple-explanation)
- [2️⃣ Everyday Analogy](#2️⃣-everyday-analogy)
- [3️⃣ Diagram](#3️⃣-diagram)
- [4️⃣ Real-World AWS Example](#4️⃣-real-world-aws-example)
- [5️⃣ Common Mistakes](#5️⃣-common-mistakes)
- [📝 Quick Summary](#-quick-summary)

---

## 1️⃣ Simple Explanation

So far we've learned about IP addresses, splitting networks into Subnets, and how to control who's allowed to connect (Security Groups). But the more important question is: how does data actually **"know its way"** from point A to point B?

- **Route Table:** A list of rules that tells each Subnet, "if you want to reach this destination, go this way." Without it, data has no idea "where to head."
- **Internet Gateway (IGW):** The gateway that connects the entire VPC to the public internet. Without it, no device in the VPC — even one with a Public IP — can reach or be reached from the internet.
- **NAT Gateway:** A special gateway that lets Private devices (with no Public IP) "go out" to the internet (for example, to download updates), but doesn't let anyone from the internet "come in" to them. A one-way road.

---

## 2️⃣ Everyday Analogy

Imagine a large company building in an isolated industrial area:

- **Route Table** = the building's directory signs ("For Accounting, go right; to reach the main street, go left")
- **Internet Gateway** = the building's main entrance that connects it to the public street — without it, the whole building is completely cut off from the outside world
- **NAT Gateway** = a one-way window in the basement (the warehouse): warehouse staff (who don't have an outside access badge) can "look out" through it to order supplies from outside, but no one outside can see them or come in through that same window

---

## 3️⃣ Diagram

```
                         🌍 Internet
                              │
                    [ Internet Gateway (IGW) ]
                              │
                    ┌─────────┴─────────┐
                    │        VPC        │
                    │   10.0.0.0/16     │
                    └─────────┬─────────┘
                              │
        ┌─────────────────────┴─────────────────────┐
        │                                           │
   Public Subnet                              Private Subnet
   10.0.1.0/24                                10.0.2.0/24
        │                                           │
   Route Table (Public)                    Route Table (Private)
   10.0.0.0/16 → local                      10.0.0.0/16 → local
   0.0.0.0/0   → IGW                        0.0.0.0/0   → NAT Gateway
        │                                           │
   [ Web Server ]                    [ NAT Gateway ]───►[ Database ]
        │                                    │
        │                              (outbound only,
        │                            no inbound allowed)
        └───────────────► 🌍 Internet ◄───────┘
```

---

## 4️⃣ Real-World AWS Example

When you create a VPC in AWS, you need to attach each Subnet to its own Route Table:

**Public Subnet's Route Table:**

```
Destination: 10.0.0.0/16  → Target: local (inside the VPC)
Destination: 0.0.0.0/0    → Target: Internet Gateway (igw-xxxxx)
```

That second rule (`0.0.0.0/0 → IGW`) is what "opens" the road to the internet.

**Private Subnet's Route Table:**

```
Destination: 10.0.0.0/16  → Target: local
Destination: 0.0.0.0/0    → Target: NAT Gateway (nat-xxxxx)
```

Here, instead of routing directly to the Internet Gateway, traffic is routed to the **NAT Gateway** (which itself lives in a Public Subnet), and it reaches the internet on the private subnet's behalf.

---

## 5️⃣ Common Mistakes

- ❌ Thinking a **NAT Gateway** and an **Internet Gateway** are the same thing — they're completely different: an IGW allows both inbound and outbound traffic (two-way), while a NAT Gateway only allows outbound traffic (one-way).
- ❌ Forgetting that a NAT Gateway must actually live in a **Public Subnet** (not a Private one) so that it can itself reach the internet.
- ❌ Assuming that adding `0.0.0.0/0 → IGW` to the route table is enough on its own — you also need a **Security Group** that allows the required traffic (the two work together, not as substitutes for each other).
- ❌ Forgetting to actually **associate** the Route Table with the correct Subnet — if you forget this step, the Subnet won't follow the rules you created.

---

## 📝 Quick Summary

| Concept | In short |
|---|---|
| Route Table | Defines where traffic from a Subnet should be directed |
| Internet Gateway | Connects the whole VPC to the public internet (two-way) |
| NAT Gateway | Lets private devices reach the internet outbound-only |
| `0.0.0.0/0 → IGW` | The rule that opens a Public Subnet to the internet |
| `0.0.0.0/0 → NAT Gateway` | The rule that lets a Private Subnet go out (but not receive) |

---

<p align="center">✍️ Part of a Networking & Cloud Notes series</p>
