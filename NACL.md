# 🌐 NACL (Network Access Control List)

<p align="center">
  <img alt="Networking" src="https://img.shields.io/badge/Topic-Networking-blue">
  <img alt="Level" src="https://img.shields.io/badge/Level-Beginner-brightgreen">
  <img alt="Cloud" src="https://img.shields.io/badge/Cloud-AWS-orange">
  <img alt="Language" src="https://img.shields.io/badge/Language-English-lightgrey">
</p>

> A simplified, illustrated summary of NACLs — from core theory to a real AWS example.
>
> Now let's complete the full picture: we learned about Security Groups (instance level), and now NACLs (Subnet level).

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

A **NACL** is also a firewall, but it operates at the level of the **entire Subnet**, not just a single instance.

That means: any device inside that Subnet (whether it's one EC2 instance or 100 devices) has its traffic pass through the NACL rules **first**, before it even reaches that device's own Security Group rules.

A crucial and very important difference from Security Groups: a NACL is **Stateless** — meaning it doesn't remember the state of a connection. If you allow Inbound traffic, you must also explicitly allow the matching Outbound traffic for the response, otherwise the response won't go out.

NACLs also support explicit **Deny** rules (you can specifically block a particular IP), unlike Security Groups, which only support **Allow** and have no Deny.

---

## 2️⃣ Everyday Analogy

If a Security Group was "the guard at the door of your specific office," then a NACL is the security guard standing at the **entire building's main gate** (before you even reach your office door).

Everyone who wants to enter any office in the building (any device in the Subnet) has to pass through this main guard first.

But this guard has a problem: he has a poor memory (**Stateless**) — if he let someone in that morning, he won't remember who they are when they leave in the afternoon, so he has to check them again against a completely separate "allowed to leave" (Outbound) list.

---

## 3️⃣ Diagram

```
                         🌍 Internet
                              │
                              ▼
                ┌─────────────────────────┐
                │           NACL           │
                │  (guard at the main      │
                │   gate of the Subnet)    │
                │                          │
                │  Rule 100: DENY  bad-IP  │
                │  Rule 200: ALLOW 443     │
                │  Rule 300: ALLOW 80      │
                └─────────────┬───────────┘
                              │  ✅ passes NACL rules
                              ▼
                ┌─────────────────────────┐
                │      Security Group      │
                │  (guard at each          │
                │   instance's own door)   │
                └─────────────┬───────────┘
                              │  ✅ passes SG rules
                              ▼
                       [ EC2 Instance ]

--------------------------------------------------------------

NACL is Stateless:
  Inbound  allowed → Outbound must be allowed SEPARATELY,
  otherwise the response traffic is blocked on the way out
```

---

## 4️⃣ Real-World AWS Example

A common scenario: you want to block a specific IP known to be attacking your site (say, `203.0.113.99`) from reaching **any** device inside the entire Subnet, without having to edit every single device's Security Group individually.

This is exactly where a NACL is useful:

```
NACL Rules (Public Subnet):
Rule 100: DENY  203.0.113.99/32  ALL Traffic   ← blocks this specific IP
Rule 200: ALLOW 0.0.0.0/0        Port 443       ← allows everyone else on HTTPS
Rule 300: ALLOW 0.0.0.0/0        Port 80        ← allows everyone else on HTTP
```

Note: NACLs evaluate rules **in order** (by rule number, from smallest to largest), and the **first matching rule** stops the evaluation immediately — so rule order matters a lot.

---

## 5️⃣ Common Mistakes

- ❌ Forgetting that a NACL is **Stateless** — many people allow Inbound traffic but forget to allow the matching Outbound traffic, so the traffic "comes in" but the response never "goes out."
- ❌ Thinking a NACL **replaces** a Security Group — in reality, these are two different layers that work together (**Defense in Depth**), not substitutes for one another.
- ❌ Ignoring rule order in a NACL — rules are evaluated in numerical order, so if you place a "Deny" rule after a broad "Allow" rule, the Allow might match first and the Deny rule after it gets skipped.
- ❌ Using NACLs for everything instead of Security Groups — in practice, a Security Group is usually enough; NACLs are used for special cases (like explicitly blocking a specific IP at the network level).

---

## 📝 Quick Summary

| Concept | In short |
|---|---|
| NACL | A firewall that operates at the entire Subnet level |
| Stateless | Inbound and Outbound rules must each be explicitly allowed |
| Deny rules | NACLs support explicit Deny, unlike Security Groups |
| Rule order | Rules are checked in numerical order; first match wins |
| NACL vs. Security Group | NACL = Subnet-wide gate; SG = per-instance door — both work together |

---

<p align="center">✍️ Part of a Networking & Cloud Notes series</p>
