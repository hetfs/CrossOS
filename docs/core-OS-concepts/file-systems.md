---
id: file-systems
title: 📁 File Systems and Storage
description: Understand how operating systems organize and manage data on storage devices.
sidebar_position: 5
---

# 📁 File Systems and Storage

Storage is where an operating system keeps all your data—permanently. The **file system** is the method the OS uses to organize and manage this data on disks.

---

## 🗂️ What Is a File System?

A **file system** defines:
- How files and directories are stored
- How metadata (like permissions and timestamps) is tracked
- How fast and secure access is performed

It’s like a library catalog—but for all your documents, apps, and system files.

---

## 📦 Common File System Types

| File System | Platform      | Features                                        |
|-------------|---------------|-------------------------------------------------|
| **FAT32**   | Windows, USBs | Simple and widely supported                    |
| **NTFS**    | Windows       | Permissions, encryption, journaling            |
| **ext4**    | Linux         | Fast, reliable, modern journaling              |
| **APFS**    | macOS         | Snapshots, encryption, fast metadata access    |

---

## 🏗️ How a File System Works

### 1️⃣ Disk Partitioning

Before formatting a disk, it's usually divided into **partitions**—logical sections that can hold file systems.

### 2️⃣ Formatting

Each partition is formatted using a file system (e.g., `ext4`, `NTFS`). This sets up:
- A **superblock** (metadata about the file system)
- **Inodes** or **file allocation tables** (indexing structures)
- **Data blocks** (where actual file content lives)

### 3️⃣ Mounting

After formatting, the OS must "mount" the file system—attaching it to a directory like `/` or `/mnt/usb`.

```bash
# Example: Manually mount a USB drive in Linux
sudo mount /dev/sdb1 /mnt/usb
````

---

## 🧭 File Paths

* **Absolute path**: Starts from root (`/home/user/file.txt`)
* **Relative path**: Based on current directory (`../docs/lesson.md`)

The OS resolves these paths to actual disk locations via the file system’s lookup tables.

---

## ⚙️ Advanced Features

Modern file systems support:

* **Journaling** – Logs changes before they happen (helps recovery)
* **Snapshots** – Point-in-time views of data
* **Compression** – Saves space
* **Access Control** – Sets who can read, write, or execute files

---

## 🧠 Summary

| Concept     | Description                                  |
| ----------- | -------------------------------------------- |
| Partition   | A logical section of a disk                  |
| File System | A method for storing and organizing files    |
| Formatting  | Prepares a partition with file system layout |
| Mounting    | Attaches a file system to the OS tree        |
| Journaling  | Logs file changes for recovery               |
| Path        | The address of a file or folder              |

---

👉 Next up: [Device Management →](./device-management)
