---
id: device-management
title: 🔌 Device Management
description: How operating systems handle input/output devices, drivers, and communication.
sidebar_position: 6
---

# 🔌 Device Management

From keyboards to GPUs and hard drives, operating systems must manage a variety of **hardware devices** efficiently and safely.

---

## 🧩 What Are Devices?

Devices are classified into:

| Type              | Examples                           |
|-------------------|------------------------------------|
| **Input Devices** | Keyboard, mouse, scanner, webcam   |
| **Output Devices**| Monitor, speakers, printer         |
| **Storage**       | SSD, HDD, USB drives               |
| **Communication** | Network cards, Bluetooth, modems   |

The OS acts as a **middle layer** between apps and the physical device.

---

## ⚙️ Device Drivers

A **device driver** is a small program that:
- Translates OS instructions into device-specific commands
- Handles low-level communication with hardware
- Is usually provided by hardware manufacturers

Drivers operate in **kernel space**, giving them high privileges and direct hardware access.

```txt
Application → OS → Driver → Hardware Device
````

---

## 🔄 I/O Management

The OS uses various strategies to manage Input/Output (I/O):

| Strategy       | Description                                  |
| -------------- | -------------------------------------------- |
| **Buffering**  | Temporary storage to handle speed mismatches |
| **Caching**    | Speeds up repeated access to the same data   |
| **Spooling**   | Queues data for devices like printers        |
| **Polling**    | Repeatedly checks device status (CPU-heavy)  |
| **Interrupts** | Device notifies the CPU only when needed     |

---

## 📚 Device Abstraction

To developers and apps, devices appear as **files**. For example:

* `/dev/sda` → Disk
* `/dev/tty` → Terminal
* `/dev/null` → Bit bucket (discard input)

This **unified file interface** simplifies how software interacts with hardware.

---

## 🧠 Summary

| Concept           | Description                               |
| ----------------- | ----------------------------------------- |
| Device Driver     | Interface between OS and hardware         |
| I/O Strategy      | Methods to efficiently handle device data |
| Abstraction Layer | Treats devices like regular files         |
| Kernel Space      | Privileged area where drivers run         |

---

👉 Next up: [User and Permission Management →](./user-permissions)
