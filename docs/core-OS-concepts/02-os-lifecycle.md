---
id: 02-os-lifecycle
title: ⚙️ OS Lifecycle
description: Hardware initialization, kernel setup, runtime management, and graceful shutdown.
sidebar_position: 2
---

# ⚙️ OS Lifecycle – From Boot to Shutdown

What happens between pressing the power button and seeing your desktop?

The operating system (OS) goes through a **structured lifecycle**, transitioning hardware from an idle state into a fully functioning environment—and eventually, powering it down safely. Each stage ensures stability, performance, and data integrity.

---

## 🧩 1. Power-On & Firmware Initialization

At power-up, firmware routines prepare the hardware and identify bootable devices.

### Key Processes

- **POST (Power-On Self-Test):** Runs diagnostics via BIOS or UEFI  
- **Hardware Enumeration:** Detects CPU, memory, and peripherals  
- **Boot Device Selection:** Chooses a bootable medium using priority order  
- **Bootloader Execution:** Transfers control to the OS bootloader (e.g., GRUB, Windows Boot Manager)

> 💡 UEFI offers faster boot, Secure Boot, and support for GPT partitions.

---

## 🧠 2. Kernel Initialization

Once the bootloader hands off control, the kernel takes charge of system setup.

### Initialization Sequence

1. **CPU Mode Switch:** Switches to protected mode (on x86)
2. **Memory Management:** Sets up physical and virtual memory
3. **Hardware Abstraction:** Detects devices and loads drivers (via ACPI)
4. **Root Filesystem Mount:** Enables access to storage volumes
5. **Core Services:** Activates interrupts, process scheduling, and basic security

> 🧠 The kernel stays resident in memory for the OS session.

---

## 🧰 System Services Initialization 

*Windows, Linux, and macOS*

After the kernel is active, the system loads essential services and daemons to support runtime operations, filesystems, networking, logging, and user sessions.

---

## 🪟 Windows: Boot Services and Subsystems

1. **Session Manager (`SMSS.exe`)**
   - First user-mode process
   - Launches `csrss.exe` and `winlogon.exe`
   - Runs `BootExecute` tasks from the registry

2. **Service Control Manager (`services.exe`)**
   - Starts services in dependency order
   - Ensures networking, event logging, and device management

3. **Essential Services**
   - **Device Manager:** Initializes drivers
   - **TCP/IP Stack:** Enables networking
   - **Event Log:** Begins logging system events

4. **Filesystem Setup**
   - Executes `AutoChk` for NTFS checks
   - Mounts volumes using registry mount points

---

## 🐧 Linux: Init Systems and Daemons

1. **Init System**
   - **`systemd`:** Uses `.service` units for fast, parallel startup
   - **SysV Init:** Legacy sequential scripts in `/etc/init.d`

2. **Core Daemons**
   - `udevd`: Manages `/dev` device nodes  
   - `NetworkManager` or `systemd-networkd`: Configures interfaces  
   - `Xorg` or `Wayland`: Starts the graphical interface

3. **Filesystem Mounting**
   - Reads `/etc/fstab` to mount partitions  
   - Activates swap space and root partition

4. **System Logging**
   - `journald` (systemd) or `rsyslog`: Captures logs from kernel and userspace

> ⚡ `systemd` achieves fast boots by starting services in parallel.

---

## 🍏 macOS: `launchd` – Unified Service Manager

1. **`launchd` (PID 1)**
   - Replaces `init`, `cron`, and `xinetd`
   - Manages both system daemons and per-user agents

2. **Service Types**

| Type     | Scope              | Location                                                | GUI Access |
|----------|--------------------|----------------------------------------------------------|------------|
| Daemons  | System-wide (`root`) | `/System/Library/LaunchDaemons`, `/Library/LaunchDaemons` | ❌         |
| Agents   | Per-user session   | `~/Library/LaunchAgents`, `/Library/LaunchAgents`        | ✅         |

3. **`.plist` Job Definitions**
   - XML files that define service behavior
   - Common fields:
     - `Label`: Unique identifier
     - `ProgramArguments`: Binary and arguments
     - `RunAtLoad`, `KeepAlive`, `StartCalendarInterval`

**Example**

```xml
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>com.example.backup</string>
    <key>ProgramArguments</key>
    <array>
      <string>/usr/bin/rsync</string>
      <string>--archive</string>
    </array>
    <key>StartCalendarInterval</key>
    <dict>
      <key>Hour</key>
      <integer>5</integer>
    </dict>
  </dict>
</plist>
````

4. **Operational Highlights**

* **On-Demand Launching:** Triggered by socket or path activity
* **Crash Throttling:** Prevents constant respawning
* **Environment Settings:** Manages environment variables and paths
* **Logging:** Outputs to `StandardOutPath` and `StandardErrorPath`

5. **Job Control with `launchctl`**

```bash
launchctl bootstrap gui/$UID ~/Library/LaunchAgents/example.plist
launchctl list | grep example
launchctl bootout system/com.example.daemon
```

> 🧠 Use `plutil` to validate and format `.plist` files.

---

## 🧠 System Comparison Summary

| Feature             | Windows      | Linux                    | macOS              |
| ------------------- | ------------ | ------------------------ | ------------------ |
| First User Process  | `SMSS.exe`   | `systemd` / `init`       | `launchd`          |
| Service Format      | Registry     | `.service`, init scripts | `.plist` (XML)     |
| Parallel Startup    | ⚠️ Limited   | ✅ With `systemd`         | ✅                  |
| GUI Session Startup | ❌            | ✅ (via DMs)              | ✅ (via agents)     |
| Logging             | Event Viewer | `journald`, `rsyslog`    | Standard log files |

> ⚙️ Modern systems prefer parallel startup to boost speed.

---

## 👤 4. User Space Initialization

When services are ready, the system becomes interactive.

### Startup Steps

1. **Display Manager (DM)**

   * Shows the graphical login screen (e.g., GDM, LightDM)

2. **Authentication**

   * Uses PAM to validate credentials securely

3. **Session Launch**

   * Loads user profile and environment
   * Starts the desktop environment (GNOME, KDE, Explorer)
   * Initializes session tokens and authorization cookies

---

## 🔄 5. Runtime Management

During regular use, the OS ensures performance and responsiveness.

| Function           | Mechanism                                  |
| ------------------ | ------------------------------------------ |
| Process Scheduling | Priorities, CPU time-slicing               |
| Memory Management  | Paging, caching, heap allocation           |
| I/O Handling       | Buffering, polling, device interrupts      |
| Security Controls  | Sandboxing, ACLs, SELinux, AppArmor        |
| User Interaction   | Manages input, output, and display updates |

---

## 📴 6. Controlled Shutdown

A clean shutdown prevents data loss and ensures safe hardware release.

### Shutdown Steps

1. **End User Sessions**

   * Sends logout signals to running applications

2. **Stop Services**

   * Sends `SIGTERM` followed by `SIGKILL` if unresponsive

3. **Finalize Filesystems**

   * Flushes write buffers
   * Unmounts all volumes safely

4. **Power Down**

   * Triggers ACPI shutdown via firmware

> ⚠️ Forced shutdowns can cause corruption and data loss.

---

## 🗺️ OS Lifecycle Overview

```mermaid
graph LR
  A[🔌 Firmware POST] --> B[🧩 Bootloader]
  B --> C[🧠 Kernel Initialization]
  C --> D[📂 System Daemons]
  D --> E[👤 User Session]
  E --> F[🔄 Runtime Ops]
  F --> G[🛑 Shutdown Sequence]
  G --> H[⭕ Power-Off]
```

---

## 🧾 Lifecycle Phase Summary

| Phase                 | Goal                           | Components                        |
| --------------------- | ------------------------------ | --------------------------------- |
| Power-On              | Initialize hardware            | POST, boot selection              |
| Kernel Initialization | Activate core OS logic         | Memory, drivers, interrupts       |
| System Services       | Launch essential daemons       | Filesystems, logging, networking  |
| User Session          | Enable user interaction        | Display manager, desktop, auth    |
| Runtime Management    | Operate efficiently and safely | Scheduling, memory, I/O, security |
| Shutdown              | Exit cleanly and power off     | Signals, sync, ACPI               |

---

## 🧠 Knowledge Check: OS Lifecycle

<details>

<summary>💡 How does UEFI improve upon BIOS?</summary>
  
✅ Secure Boot, faster startup, GPT support, and pre-OS networking.
  
</details>

<details>
  
<summary>💡 Why initialize memory before loading drivers?</summary>
  
✅ Drivers require memory allocation and virtual address mappings.
  
</details>

<details>
  
<summary>💡 What if services don’t stop during shutdown?</summary>
  
✅ May result in zombie processes, locks, or filesystem errors.
  
</details>

<details>
  
<summary>💡 Why mount filesystems read-only on shutdown?</summary>
  
✅ Prevents metadata corruption during final disk writes.
  
</details>

<details>
  
<summary>💡 How do init systems control service order?</summary>
  
✅ Through dependency rules in service units or startup scripts.
  
</details>

---

👉 Next up: [Bootloaders & Init Systems →](./03-boot-and-init)
