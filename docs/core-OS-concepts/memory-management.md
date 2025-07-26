---
id: memory-management
title: 🧠 Memory Management in Operating Systems
description: Learn how an OS allocates, protects, and virtualizes memory for applications and processes.
sidebar_position: 4
---

# 🧠 Memory Management in Operating Systems

Memory is a computer’s short-term workspace. An operating system (OS) manages memory to make sure all applications get the space they need—without interfering with one another.

Let’s explore how this works behind the scenes.

---

## 🔄 What Is Memory Management?

Memory management is the process of:
- **Allocating** memory to programs that need it.
- **Tracking** which parts of memory are in use.
- **Freeing** memory when it's no longer needed.
- **Protecting** memory to prevent crashes or data leaks.

---

## 📦 Types of Memory the OS Manages

| Type             | Purpose                                       |
|------------------|-----------------------------------------------|
| **RAM (Main Memory)**  | Fast, volatile memory for running programs     |
| **Cache**         | Ultra-fast memory near the CPU for quick access |
| **Virtual Memory**| Extra “fake” RAM using disk space             |
| **Swap Space**    | A special disk area used to hold inactive data |

---

## 🚀 How Memory Management Works

### 1️⃣ Memory Allocation
When a program starts, it requests memory. The OS:
- Finds available memory blocks
- Allocates them safely
- Records the assignment in a memory table

### 2️⃣ Virtual Memory
If RAM runs out, the OS simulates extra memory by using disk storage (called the **page file** or **swap**). This allows more apps to run, even on limited hardware.

### 3️⃣ Paging and Segmentation
- **Paging:** Memory is split into equal-sized pages. Programs use virtual addresses mapped to physical RAM.
- **Segmentation:** Memory is split by type (code, data, stack). More flexible, but more complex to manage.

### 4️⃣ Protection
Each program has its own memory space. The OS prevents one app from reading or writing another’s memory. This protects against crashes and security bugs.

---

## 🧪 Fun Fact: Segfaults

A “segmentation fault” happens when a program tries to access memory it doesn’t own. The OS blocks it and often kills the program to avoid harm.

---

## 🧠 Summary

| Concept            | What It Means                               |
|--------------------|---------------------------------------------|
| Memory Allocation  | Assigning RAM to programs                   |
| Virtual Memory     | Simulated memory using storage              |
| Paging             | Splitting memory into fixed-size chunks     |
| Segmentation       | Organizing memory into variable sections    |
| Protection         | Keeping memory spaces isolated              |

---

👉 Next up: [File Systems and Storage →](./file-systems)
