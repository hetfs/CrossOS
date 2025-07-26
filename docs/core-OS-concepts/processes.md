---
id: processes
title: ⚙️ Processes and Scheduling
description: Understand how operating systems manage running programs and allocate CPU time.
sidebar_position: 10
---

# ⚙️ Processes and Scheduling

A **process** is a running program. Your web browser, text editor, and even background services are all processes. Operating systems manage these processes using **scheduling algorithms** to fairly distribute CPU time.

---

## 🧠 What Is a Process?

A process is made of:
- **Code** – the program instructions
- **Memory** – space for data and variables
- **Resources** – file handles, network connections
- **State** – current execution status (running, sleeping, etc.)

Each process gets its own **address space**, isolating it from others.

---

## 🧵 Processes vs Threads

| Process | Thread |
|--------|--------|
| Independent unit of execution | Lightweight unit inside a process |
| Has its own memory space | Shares memory with sibling threads |
| More overhead to create | Faster to create and switch |

A single process may have **multiple threads** working in parallel.

---

## 🔄 Process Lifecycle

The basic lifecycle looks like this:

```text
New → Ready → Running → Waiting → Terminated
````

* **New** – The OS creates the process.
* **Ready** – Waiting for CPU time.
* **Running** – Actively executing.
* **Waiting** – Waiting for input/output or a lock.
* **Terminated** – Completed or killed.

---

## 📅 Scheduling: How the OS Shares the CPU

The CPU can only run **one process per core at a time**, so the OS uses a **scheduler** to switch between tasks rapidly.

Common scheduling strategies:

* **Round-robin** – Equal time slices for all
* **Priority-based** – Higher-priority tasks run more often
* **Multi-level queues** – Grouped by type (e.g., interactive vs. background)

Most OSes implement **preemptive multitasking**, which lets the scheduler forcibly switch between processes to ensure responsiveness.

---

## 🔍 Process Attributes

Each process has:

* **PID (Process ID)** – Unique ID
* **Parent PID** – ID of the process that launched it
* **User** – Owner of the process
* **State** – Running, Sleeping, etc.
* **Nice value** – Affects scheduling priority

---

## 📁 Process Trees and Forking

Processes are created from other processes:

* **fork()** – Creates a copy of the current process (Unix-like OSes)
* **exec()** – Replaces the current process image with a new program

> On Windows, process creation uses `CreateProcess()` instead of `fork()`.

---

## 🧠 Process Management Tools (CLI)

| OS          | Common Commands                               |
| ----------- | --------------------------------------------- |
| Linux/macOS | `ps`, `top`, `htop`, `kill`, `nice`, `renice` |
| Windows     | `tasklist`, `taskkill`, `wmic`, Task Manager  |
| Android     | `ps`, `top`, `kill`, `logcat`                 |
| iOS         | Internal developer tools only (sandboxed)     |

---

## 🧠 Summary

| Concept   | Description                                      |
| --------- | ------------------------------------------------ |
| Process   | A running instance of a program                  |
| Thread    | A lightweight unit of execution inside a process |
| Scheduler | Chooses which process runs next                  |
| Tools     | Vary by OS for viewing/managing processes        |

---

👉 Next up: [File Systems and Storage →](./file-systems)
