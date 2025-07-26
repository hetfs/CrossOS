---
id: user-permissions
title: 🧑‍💻 User and Permission Management
description: Learn how operating systems control access to resources through users, groups, and permissions.
sidebar_position: 7
---

# 🧑‍💻 User and Permission Management

Modern operating systems are **multi-user environments**, meaning multiple people or processes can share the same system safely and securely.

To ensure **isolation, privacy, and protection**, the OS must:

- Identify *who* is using the system
- Control *what* they are allowed to do

---

## 👥 Users, Groups, and Roles

At the core of any access control model:

| Concept | Description |
|--------|-------------|
| **User** | An individual account with a unique ID |
| **Group** | A collection of users with shared permissions |
| **Role** | Abstract label for a set of permissions (used in role-based access control, RBAC) |

> 🔐 A user’s *identity* and *group memberships* help determine what resources they can access.

---

## 🛡️ Permission Basics

Most OSs apply **Access Control Lists (ACLs)** or **permission bits** to files, processes, devices, and services.

| Resource | Possible Permissions |
|----------|----------------------|
| **File** | Read / Write / Execute |
| **Device** | Use / Configure |
| **Process** | Signal / Monitor / Kill |
| **Network Port** | Open / Bind / Listen |

Permissions can be granted to:
- **The owner**
- **A group**
- **Everyone (public/other)**

---

## 🔄 Authentication vs Authorization

| Term | Meaning |
|------|---------|
| **Authentication** | Verifying *who* you are (e.g., password, fingerprint, token) |
| **Authorization** | Determining *what* you’re allowed to do |

These two steps often work together in login systems or when accessing secure services.

---

## 🧱 Principle of Least Privilege

The OS follows this **security principle**:

> "Each user or process should be given the minimum privileges necessary to complete its task."

This limits the damage that can occur if something is compromised.

---

## 🧵 Multi-User & Multi-Process Considerations

Operating systems enforce permissions across:

- **Users sharing a machine**
- **Processes running under different user IDs**
- **Network services (e.g., remote SSH login)**

System calls like `fork`, `exec`, and `setuid` require strict permission handling to prevent **privilege escalation**.

---

## 🌐 OS-Specific Implementations

While the core ideas are shared, the implementation details differ by OS:

- **Linux/Unix**: `chmod`, `chown`, `umask`, POSIX ACLs
- **Windows**: NTFS permissions, Access Tokens, Group Policy
- **macOS**: BSD-like permissions with Apple-specific security layers
- **Mobile OSs**: App sandboxing and permission prompts (Android, iOS)

> 📘 Detailed examples are provided in each OS’s dedicated module.

---

## 🧠 Summary

| Concept | Purpose |
|--------|---------|
| Users & Groups | Isolate identity and access |
| Permissions | Control access to system resources |
| Authentication | Confirm identity |
| Authorization | Enforce access rules |
| Least Privilege | Minimize risk of abuse or damage |

---

👉 Next up: [Networking and Sockets →](./networking)
