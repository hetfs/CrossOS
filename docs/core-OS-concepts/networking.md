---
id: networking
title: 🌐 Networking and Sockets
description: Understand how operating systems enable communication through networking interfaces and sockets.
sidebar_position: 8
---

# 🌐 Networking and Sockets

Operating systems don’t just manage local resources—they also handle **network communication**, allowing systems to exchange data over LANs, the internet, or other protocols.

This is where **sockets**, **IP addresses**, and **network stacks** come in.

---

## 🧱 OS Networking Stack (Simplified)

Every OS includes a **network stack** that implements a set of protocols layered as follows:

```

Application Layer     (HTTP, SSH, DNS)
Transport Layer       (TCP, UDP)
Internet Layer        (IP, ICMP)
Link Layer            (Ethernet, Wi-Fi, PPP)

```

The OS coordinates these layers to:

- Route data correctly
- Handle packet transmission
- Support multiple applications using the network simultaneously

---

## 🔌 Sockets: The OS Interface to the Network

A **socket** is the primary API an application uses to communicate over the network.

> Think of a socket as a software endpoint for sending and receiving data.

Each socket is bound to:
- A **protocol** (e.g., TCP or UDP)
- An **IP address**
- A **port number**

### Example: A Web Server
A server might bind to:

```

TCP / 0.0.0.0 / port 80

```

Meaning it accepts TCP traffic from any IP on port 80.

---

## 📦 TCP vs UDP (Handled by OS)

| Feature | TCP | UDP |
|--------|-----|-----|
| Reliable | ✅ | ❌ |
| Ordered | ✅ | ❌ |
| Connection-based | ✅ | ❌ |
| Lightweight | ❌ | ✅ |

TCP is great for web browsing and file transfers.  
UDP is ideal for real-time apps like games or video calls.

> The OS kernel ensures reliability (or not) depending on protocol selection.

---

## 📡 Network Interfaces

Each machine may have multiple **network interfaces**, like:

- Ethernet
- Wi-Fi
- Loopback (`127.0.0.1`)
- VPNs
- Virtual interfaces (e.g., Docker or WSL)

The OS assigns each one:
- An **IP address**
- A **MAC address**
- A **routing table entry**

---

## 📜 Common OS Socket Operations

| Operation | Description |
|----------|-------------|
| `socket()` | Create a new socket |
| `bind()` | Assign IP + port |
| `listen()` | Wait for connections (TCP only) |
| `accept()` | Accept an incoming connection |
| `connect()` | Initiate a connection to a remote server |
| `send()` / `recv()` | Transmit or receive data |

> These are usually wrapped in libraries like `net`, `socket`, or `http`.

---

## 🔐 OS Roles in Networking

The operating system handles:
- **Firewall rules**
- **Interface up/down control**
- **Port blocking**
- **Packet routing**
- **DNS resolution**
- **Encryption passthrough** (e.g., TLS via OpenSSL)

---

## 📱 Cross-Platform Networking Differences

All OSs follow the same core socket principles, but with subtle differences:

| OS | Notes |
|----|-------|
| **Linux** | Uses BSD-like sockets and tools like `iptables`, `netstat`, `ss` |
| **Windows** | Winsock API, `netsh`, layered firewall control |
| **macOS** | BSD sockets with Apple’s security layers |
| **Android/iOS** | Enforced app sandboxing with permission gates for networking |

---

## 🧠 Summary

| Concept | Role |
|--------|------|
| Network Stack | Translates app data into packets |
| Socket | Interface for sending/receiving data |
| TCP/UDP | Transport protocols with trade-offs |
| OS Network Roles | Route, firewall, and protect traffic |

---

👉 Next up: [System Logging and Auditing →](./logging)
