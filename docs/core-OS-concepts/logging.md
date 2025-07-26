---
id: logging
title: 🪵 System Logging and Auditing
description: Explore how operating systems handle logs, events, and auditing to track system behavior and ensure accountability.
sidebar_position: 9
---

# 🪵 System Logging and Auditing

Every operating system keeps **logs**—records of important events, errors, and actions. These logs are crucial for:

- **Debugging problems**
- **Monitoring performance**
- **Detecting security breaches**
- **Auditing user activity**

Let's look at how OSes handle these tasks in general.

---

## 📓 What Are System Logs?

A **system log** is a time-stamped record of system or application activity.

Types of logs:
- **Kernel logs** – Boot process, hardware errors
- **Authentication logs** – Logins, sudo usage
- **Application logs** – Errors, usage info
- **Network logs** – Connections, firewall events
- **Audit logs** – Policy enforcement, system changes

---

## 🛠️ The Role of the Operating System

The OS provides:
- A centralized logging mechanism
- Secure storage for logs
- Permission control over who can read logs
- APIs for applications to log structured messages

---

## 🧩 Common Logging Components (Across OSes)

| Component | Purpose |
|----------|---------|
| **Log daemon** | Collects and stores log messages (e.g., `syslog`, `journald`, `Event Log`) |
| **Log files/database** | Where logs are written to (e.g., `/var/log`, `.evtx`) |
| **Log viewer** | Tools for viewing logs (`journalctl`, `less`, `eventvwr`, `logcat`) |
| **Audit service** | Tracks sensitive actions for security compliance |

---

## 🔄 Log Levels

Logs typically have severity levels:

- `DEBUG` – Detailed internal info
- `INFO` – General application flow
- `WARNING` – Something unexpected happened
- `ERROR` – A failure occurred
- `CRITICAL` – System instability or crashes

> The OS or app can filter or act based on severity.

---

## 🔐 Auditing: More Than Just Logs

**Auditing** is about **who did what, when, and why**.

Operating systems may track:
- User logins and sudo usage
- File access (e.g., tampering with `/etc/passwd`)
- Changes to system config or permissions
- Policy violations or failed access attempts

Auditing can trigger alerts or generate reports for compliance.

---

## 🧠 OS-Specific Logging (Covered Separately)

| OS | Logging System | Audit System |
|----|----------------|--------------|
| **Linux** | `rsyslog`, `systemd-journald` | `auditd`, `ausearch` |
| **Windows** | Event Viewer / Event Tracing | Windows Security Logs |
| **macOS** | Unified Logging System (`log` CLI) | Apple System Integrity Protection |
| **Android** | `logcat` | App-specific audit & sandboxing |
| **iOS** | Unified Logging + sandbox enforcement | App activity tracking |

> We’ll explore each OS logging/auditing system in its own lesson.

---

## 🧠 Summary

| Topic | Role |
|-------|------|
| Logging | Records events and errors |
| Auditing | Tracks sensitive or policy-bound actions |
| OS Role | Centralizes, secures, and filters logs |
| Tools | Vary by OS (`journalctl`, `eventvwr`, `logcat`, etc.) |

---

👉 Next up: [Processes and Scheduling →](./processes)
