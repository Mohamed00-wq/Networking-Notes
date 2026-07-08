# 🌐 CIDR (Classless Inter-Domain Routing)

<p align="center">
  <img alt="Networking" src="https://img.shields.io/badge/Topic-Networking-blue">
  <img alt="Level" src="https://img.shields.io/badge/Level-Beginner-brightgreen">
  <img alt="Cloud" src="https://img.shields.io/badge/Cloud-AWS-orange">
  <img alt="Language" src="https://img.shields.io/badge/Language-English-lightgrey">
</p>

> A simplified, illustrated summary of CIDR notation — from core theory to a real AWS example.

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

We already used CIDR notation (like `/16`, `/24`, `/17`) in the previous lesson without fully explaining it. Now let's understand it from the ground up.

**CIDR** is a system for writing a network address + its size in a compact, smart notation, instead of writing out the full Subnet Mask like `255.255.255.0`.

But more importantly: CIDR lets you define **any exact network size you need**, instead of being locked into "traditional" fixed sizes (as it used to be under an older system called **Classful Addressing**, which only split networks into fixed-size classes A/B/C).

In other words, CIDR gives you flexibility: you can build a network **exactly** the size you need — not bigger or smaller than necessary.

---

## 2️⃣ Everyday Analogy

Imagine that before CIDR, you were forced to buy land in fixed sizes only: small (100 m²), medium (1000 m²), or large (10,000 m²) — with nothing in between.

If you needed exactly 500 m², you'd have to buy "medium" (1000 m²) and waste half the space!

CIDR comes along and says: "No, you can buy exactly the amount of space you need" — 300 m², 550 m², any reasonable number you choose, with no waste.

---

## 3️⃣ Diagram

```
CIDR notation:   IP_Address  /  Prefix_Length
Example:         10.0.0.0    /  16
                                 ↑
                    Number of bits assigned to the "Network"
                    (the rest of the 32 bits = for hosts)

The bigger the number after /  →  the smaller the network
(more bits for the network = fewer bits for hosts)

/8  → Huge network      (16,777,214 hosts)
/16 → Large network     (65,534 hosts)
/24 → Medium network    (254 hosts)
/28 → Small network     (14 hosts)
/32 → A single device!  (single host)

    Full 32 bits
┌──────────────────────────────────┐
│ Network Part  │   Host Part      │
└──────────────────────────────────┘
     e.g. /24  →  24 bits  │  8 bits
```

---

## 4️⃣ Real-World AWS Example

In AWS, when you create a **Security Group** (a firewall for EC2), you constantly use CIDR to define "who's allowed to connect":

```
Inbound Rule:
Type: SSH
Source: 0.0.0.0/0   ← allowed for everyone on the internet (security risk!)

Source: 41.225.100.50/32  ← allowed for a single device only (a specific IP)

Source: 10.0.0.0/16  ← allowed only for devices inside the VPC
```

Notice:

- `0.0.0.0/0` = every address on the internet (because zero bits are assigned to the network = everything is allowed)
- `/32` = one exact, specific device (because all 32 bits are assigned to the network, leaving nothing for the Host)

---

## 5️⃣ Common Mistakes

- ❌ Using `0.0.0.0/0` in a Security Group for a database or SSH access → this opens the door to the entire world (a very serious and very common security vulnerability in beginner projects).
- ❌ Thinking CIDR and Subnet Mask are two different things — they're the exact same concept, just two different notations for writing it.
- ❌ Forgetting that the smaller the network (e.g. `/28`), the more you need to subtract 2 from the usable host count (because of the reserved Network and Broadcast addresses).

---

## 📝 Quick Summary

| Concept | In short |
|---|---|
| CIDR | A compact notation for network address + size (e.g. `/24`) |
| Bigger number after `/` | Smaller network, fewer available hosts |
| `0.0.0.0/0` | Every IP address on the internet — use with caution |
| `/32` | Exactly one single device |
| CIDR vs Subnet Mask | Same concept, different notation |

---

<p align="center">✍️ Part of a Networking & Cloud Notes series</p>
