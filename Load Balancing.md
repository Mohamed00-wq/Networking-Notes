# 🌐 Load Balancing

<p align="center">
  <img alt="Networking" src="https://img.shields.io/badge/Topic-Networking-blue">
  <img alt="Level" src="https://img.shields.io/badge/Level-Beginner-brightgreen">
  <img alt="Cloud" src="https://img.shields.io/badge/Cloud-AWS-orange">
  <img alt="Language" src="https://img.shields.io/badge/Language-English-lightgrey">
</p>

> A simplified, illustrated summary of Load Balancing — from core theory to a real AWS example.

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

So far we've talked about a single server receiving all the traffic. But what happens if the site becomes popular, and thousands of visitors show up at the same moment? A single server could crash under the load.

A **Load Balancer** is a component that receives all the incoming traffic, then distributes it across multiple servers (instead of one server carrying the entire load).

Two key benefits:

1. **Performance:** Distributing the load means each server only handles a portion of the traffic.
2. **Availability:** If one server goes down, the Load Balancer detects this automatically, stops sending it traffic, and keeps distributing across the rest — so the site doesn't go down entirely.

---

## 2️⃣ Everyday Analogy

Imagine a restaurant with only one order window, and suddenly 100 customers show up at once — a long line, and people waiting for hours.

A **Load Balancer** is like a smart receptionist standing at the door, distributing customers across several windows (servers) based on which one is freest.

And if a particular window "breaks down" (its staff member is absent), the receptionist notices immediately, stops sending customers there, and directs them only to the working windows — the customer doesn't even notice there was a problem.

---

## 3️⃣ Diagram

```
                    Visitors (thousands at once)
                              │
                              ▼
                    [ 🌍 Internet ]
                              │
                              ▼
                  ┌────────────────────┐
                  │    Load Balancer    │
                  │  (health checks     │
                  │   every server)     │
                  └─────────┬──────────┘
             ┌───────────────┼───────────────┐
             │               │               │
             ▼               ▼               ▼
      [ EC2 Server 1 ] [ EC2 Server 2 ] [ EC2 Server 3 ]
        ✅ healthy       ❌ unhealthy      ✅ healthy
      (gets traffic)   (traffic stopped) (gets traffic)
```

---

## 4️⃣ Real-World AWS Example

In AWS, this service is called **ELB (Elastic Load Balancer)**, and it comes in a few types, the most common being:

- **Application Load Balancer (ALB):** Operates at the HTTP/HTTPS level (Layer 7), and can make smart routing decisions (e.g., route `/api` to one set of servers and `/images` to another).
- **Network Load Balancer (NLB):** Operates at a lower level (Layer 4 — TCP/UDP), and is much faster, suitable for very high-throughput traffic.

**Real-world example:** You have 3 EC2 servers in a Public Subnet, and you put them behind a single ALB. Visitors never communicate with any EC2 instance directly (they don't even know its IP) — they only communicate with the **Load Balancer**, which distributes the requests.

The Load Balancer also performs periodic **Health Checks** (e.g., every 30 seconds) on each server, and if a server fails to respond, it's marked "unhealthy" and automatically stops receiving traffic.

---

## 5️⃣ Common Mistakes

- ❌ Assuming a Load Balancer is **only** for distributing load — in reality, its most important benefit is also **High Availability** (if a server goes down, the site stays up).
- ❌ Forgetting to configure the **Health Check** properly — if the check is wrong, the Load Balancer might keep sending traffic to a server that's actually down.
- ❌ Assuming the visitor needs to know each server's IP individually — in reality, the visitor only knows the Load Balancer's address, and it distributes traffic completely transparently.
- ❌ Placing the Load Balancer and all its servers in only **one Availability Zone** — if that entire zone goes down (power outage, disaster...), everything goes down; it's better to spread servers across multiple AZs (a topic we'll cover in the Multi-AZ lesson).

---

## 📝 Quick Summary

| Concept | In short |
|---|---|
| Load Balancer | Distributes incoming traffic across multiple servers |
| Application Load Balancer (ALB) | Layer 7 (HTTP/HTTPS), smart routing decisions |
| Network Load Balancer (NLB) | Layer 4 (TCP/UDP), very high performance |
| Health Check | Periodically verifies each server is responding |
| High Availability | Traffic is rerouted automatically if a server fails |

---

<p align="center">✍️ Part of a Networking & Cloud Notes series</p>
