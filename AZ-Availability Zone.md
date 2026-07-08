# 🌐 Availability Zones & Subnets

<p align="center">
  <img alt="Networking" src="https://img.shields.io/badge/Topic-Networking-blue">
  <img alt="Level" src="https://img.shields.io/badge/Level-Beginner-brightgreen">
  <img alt="Cloud" src="https://img.shields.io/badge/Cloud-AWS-orange">
  <img alt="Language" src="https://img.shields.io/badge/Language-English-lightgrey">
</p>

> A simplified, illustrated summary of how Availability Zones relate to Subnets in AWS.

---

## 📑 Table of Contents

- [1️⃣ What Is an Availability Zone?](#1️⃣-what-is-an-availability-zone)
- [2️⃣ How Does a Subnet Relate to an AZ?](#2️⃣-how-does-a-subnet-relate-to-an-az)
- [3️⃣ Where Does the Subnet Mask Come In?](#3️⃣-where-does-the-subnet-mask-come-in)
- [4️⃣ Full Example](#4️⃣-full-example)
- [📝 Exam Rule to Remember](#-exam-rule-to-remember)

---

## 1️⃣ What Is an Availability Zone?

An **Availability Zone (AZ)** is a data center — or a group of data centers — located within the same **Region**.

For example, in AWS:

```
Region: eu-central-1 (Frankfurt)

├── AZ A (eu-central-1a)
├── AZ B (eu-central-1b)
└── AZ C (eu-central-1c)
```

Each AZ is electrically and network-wise isolated from the others, so if one goes down, the others keep running.

---

## 2️⃣ How Does a Subnet Relate to an AZ?

In AWS:

> Every Subnet must live inside **exactly one** Availability Zone.
> You **cannot** create a Subnet that spans across AZ A and AZ B.

**Example:**

We have a VPC:

```
10.0.0.0/16
```

We split it into Subnets:

```
10.0.1.0/24
10.0.2.0/24
10.0.3.0/24
```

Then we distribute them across the AZs:

```
Region: eu-central-1

Availability Zone A
-------------------
Subnet
10.0.1.0/24

Availability Zone B
-------------------
Subnet
10.0.2.0/24

Availability Zone C
-------------------
Subnet
10.0.3.0/24
```

Notice that each Subnet exists inside **only one** AZ.

---

## 3️⃣ Where Does the Subnet Mask Come In?

The **Subnet Mask** (like `/24`) does **not** determine the AZ.

It only determines:

- The **size** of the Subnet
- The **number of IP addresses** within it

For example:

```
10.0.1.0/24
```

means:

- Network name: `10.0.1.0`
- Number of addresses: 256

But whether this network lives in:

- `eu-central-1a`?
- `eu-central-1b`?
- `eu-central-1c`?

...that's decided when you **create the Subnet in AWS**, not by the Subnet Mask.

---

## 4️⃣ Full Example

```
                    VPC
                10.0.0.0/16
                      │
     ┌────────────────┴────────────────┐
     │                                 │
Availability Zone A          Availability Zone B
(eu-central-1a)              (eu-central-1b)
     │                                 │
10.0.1.0/24                  10.0.2.0/24
Public Subnet                Private Subnet
     │                                 │
Web Server                   Database
```

In this example:

- The **VPC** spans across all Availability Zones within the Region.
- Each **Subnet** lives inside only one AZ.
- The **Subnet Mask** (`/24`) only defines the Subnet's size — not its location.

---

## 📝 Exam Rule to Remember

- ✅ A **VPC** can span across all Availability Zones in a Region.
- ✅ A **Subnet** belongs to exactly **one** Availability Zone.
- ✅ A **Subnet Mask** only determines the network's size — it does **not** determine the Availability Zone.

---

<p align="center">✍️ Part of a Networking & Cloud Notes series</p>
