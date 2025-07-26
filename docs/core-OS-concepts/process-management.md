---
id: process-management
title: 🧵 Process Management – How OS Handles Programs
description: Dive into how an operating system manages processes, multitasking, and scheduling.
sidebar_position: 4
---

# 🧵 Process Management – How OS Handles Programs

Every time you launch an app, run a script, or start a background service, the operating system spins up a **process**—an isolated, scheduled unit of execution.

Understanding process management is key to understanding how modern OSes multitask, stay responsive, and secure your programs.

---

## 📦 What Is a Process?

A **process** is a running instance of a program.

It includes:
- **Code** (the program itself)
- **Memory** (stack, heap, code, and data segments)
- **File descriptors** (open files, sockets)
- **Execution context** (CPU registers, program counter)

---

## 🧬 From Program to Process

When a user runs a program:
1. The OS loads the program’s **executable** into memory.
2. It allocates necessary resources (memory, files, etc.).
3. It creates a **process control block (PCB)** to track it.
4. The **scheduler** adds it to the queue of ready processes.

> The same executable file can run in **multiple processes**, each with its own memory and state.

---

## 🔀 Multitasking and Scheduling

Operating systems use **schedulers** to manage multiple processes on limited CPU cores.

### Scheduling types:
- **Preemptive** (e.g. Windows, Linux): The OS forcibly switches processes after a time slice.
- **Cooperative**: Processes yield control voluntarily (rare today).

### Scheduler goals:
- Maximize CPU usage
- Minimize response time
- Ensure fairness
- Prevent starvation

---

## 📊 Scheduler Flow Diagram

```mermaid
flowchart TD
    A[Ready Queue] -->|Scheduler Picks| B[CPU Executes Process]
    B --> C[Process Completes or Yields]
    C -->|If still active| A
````

---

## 🧵 Threads and Concurrency

A **thread** is a lightweight unit of execution inside a process. All threads:

* Share memory
* Share file handles
* Can run concurrently on multiple cores

A process with one thread = **single-threaded**
With many threads = **multi-threaded**

> Example: A browser may run each tab as a separate process, and each tab may have multiple threads for rendering, JavaScript, and networking.

---

## 📑 Process States

Every process goes through predictable states:

| State          | Description                |
| -------------- | -------------------------- |
| **New**        | Just created               |
| **Ready**      | Waiting to run on the CPU  |
| **Running**    | Currently executing        |
| **Waiting**    | Blocked (I/O, sleep, etc.) |
| **Terminated** | Finished execution         |

These transitions are tracked by the OS using the PCB.

---

## 🔐 Isolation and Security

Processes are isolated to avoid interference:

* Memory is protected using **virtual memory**
* Processes can't access each other's memory space (unless explicitly shared)
* System calls enforce **privilege separation** (user vs. kernel mode)

This ensures system stability and security.

---

## 👨‍👩‍👧 Parent and Child Processes

In UNIX-like systems:

* `fork()` creates a **child process**, duplicating the parent.
* `exec()` replaces a process’s memory with a new program.
* `wait()` allows a parent to pause until a child finishes.

```lua
-- Example: Pseudo-code
pid = fork()
if pid == 0 then
  exec("child-program")
else
  wait(pid)
end
```

---

## 🔁 Killing and Managing Processes

* `kill` (Unix): Send signals like `SIGTERM` or `SIGKILL`
* `taskkill` (Windows): Forcefully terminate processes
* `ps`, `top`, `htop`: List and monitor active processes

System daemons and services often run in the background as **persistent processes** managed by the OS.

---

## 🧠 Summary Table

| Concept       | Meaning                                            |
| ------------- | -------------------------------------------------- |
| Process       | An isolated running program                        |
| Thread        | Lightweight unit within a process                  |
| Scheduler     | OS component that chooses which process runs next  |
| PCB           | Data structure that tracks process info            |
| State Machine | Describes process lifecycle (Ready, Running, etc.) |
| System Calls  | Interface for processes to interact with the OS    |

---

👣 Next up: [Memory Management →](./memory-management)
