---
id: install-arch-linux
title: Installing Arch Linux
description: Beginner-friendly manual Arch Linux install guide with full disk encryption, LVM, and GRUB.
sidebar_position: 4
---

# 🐧 Installing Arch Linux

*Lightweight · Flexible · Cutting-Edge · Customizable · Maximum Control*

Arch Linux is one of the most powerful rolling-release distributions available but it can be tricky to install.
Start with a **clean slate** zero bloat, no assumptions and build your ideal environment from essential tools up.
In this guide, I’ll walk you through Manual Method:

> *"Simplicity is the ultimate sophistication."* — Arch Philosophy

---

## 🧩 Why Choose Arch?

| Feature | Impact | Best For |
| --- | --- | --- |
| **Rolling Release** | Always-current software stack | Early adopters |
| **Minimal Base** | Resource efficiency + purity | Performance enthusiasts |
| **Pacman + AUR** | 80,000+ packages via main/AUR | Developers/power users |
| **DIY Approach** | Total system ownership | Tinkerers & learners |

---

## 🛠️ Installation Pathways

## 🔰 **1. Beginner-Friendly (`archinstall`)**

```bash
archinstall  # Guided TUI installer
```

* **Features**: Auto-partitioning • Filesystem selection • Preconfigured DEs
* **Perks**: Driver auto-detection • Secure defaults • Post-install scripts
> *Perfect for first-timers or rapid deployments*

---

## 🧰 **2. Manual Arch Linux Installation**

The following steps walk you through a **fully manual Arch Linux installation** including encryption, LVM, and GRUB setup.

```bash
fdisk /dev/nvme0n1  # Raw partitioning control
```

* **Security**: Full LUKS encryption • Secure Boot integration
* **Storage**: Flexible LVM volumes (ext4/Btrfs)
* **Customization**: Minimal package selection • Handcrafted GRUB
> *For architects wanting absolute system control*
---

### 🏗️ Build Target

* **Secure Foundation**: LUKS-encrypted physical volumes
* **Logical Structure**: LVM-managed `/`, `/home`, and swap
* **Boot Integrity**: Hardened GRUB + kernel parameters
* **Minimal Core**: Only essential packages (no bloat)
> 💡 **Pro Tip**: Version-control configs with [dotfiles](https://dotfiles.github.io/) for reproducible setups.

---

## 🚀 Prerequisites
* ✅ Booting in **UEFI mode**
* ✅ Working **internet connection**
* ✅ Complete data **backup**
* ✅ Latest Arch ISO from [archlinux.org/download](https://archlinux.org/download)
* ✅ Verified the ISO signature: [Verify it](https://wiki.archlinux.org/title/Installation_guide#Verify_signature)
* ✅ USB created with [Ventoy](https://github.com/ventoy/Ventoy) or:
  ```bash
  sudo dd bs=4M if=archlinux.iso of=/dev/sdX status=progress oflag=sync
  ```
  > 💡 Replace `/dev/sdX` with your USB path

---

## 🛠️ Installation Steps

## 1️⃣ Boot Environment Setup

Prepare the environment after booting from the Arch USB:
```bash
# ✅ Confirm you are in UEFI mode
ls /sys/firmware/efi/efivars && echo "UEFI mode detected"

localectl list-keymaps # list keymaps

# 🧭 Set keyboard layout (if needed)
loadkeys us    # Change `us` to `de`, `fr`, etc.

setfont ter-123b

# 🕒 Sync system clock
timedatectl set-ntp true

cat /sys/firmware/efi/fw_platform_size # verify efi OS

# Set keyboard layout
loadkeys us
setfont ter-123b

# Sync clock
timedatectl set-ntp true
```

---

## 2️⃣ Network Configuration

If you're connected via Ethernet, you're good to go. For Wi-Fi, follow these steps inside the Arch installation environment.

### 📶 Connecting to Wi-Fi

Identify your wireless interface

```bash
ip addr show
```

Look for a device like `wlan0`, `wlp2s0`, or similar in the output. That’s your Wi-Fi interface.

### 📡 Connect to Wi-Fi Using `iwctl`

```bash
# Step 1: Launch the iwctl interactive prompt
iwctl
```

```bash
# Step 2: Inside iwctl, scan for available networks
[iwd]# station wlan0 scan
[iwd]# station wlan0 get-networks
```

```bash
# Step 3: Connect to a network
[iwd]# station wlan0 connect "Your Network Name" --passphrase "YourPassphrase"
```

```bash
# Step 4: Exit iwctl
[iwd]# exit
```

---

### ⚙️ Adapter Notes

🔁 **Replace `wlan0`** with your actual wireless interface.
  → Find it with:

  ```bash
  iwctl device list
  ```
🔐 **WPA2/WPA3**: Just use `--passphrase`; iwd handles encryption automatically.
🧩 **SSID with spaces/symbols**: Always wrap in quotes (`"Your Network"`).

### ⚡ Quick One-Liner (Non-Interactive)

```bash
iwctl --passphrase="YourPassphrase" station wlan0 connect "NetworkName"
```

### ✅ Confirm Your Connection

```bash
# Check for assigned IP address
ip addr show wlan0

# Ping to test network access
ping -c 3 archlinux.org
```

> 💡 **Pro Tip**: Use TAB in the `iwctl` shell after typing part of the network name:
>
> ```bash
> station wlan0 connect "Net⭾"
> ```

---

## 3️⃣ Disk Partitioning

### 🧭 Step 1: Identify Your Main Disk

Before partitioning, find your primary disk (e.g., `/dev/nvme0n1`, `/dev/sda`).

```bash
lsblk
# or for details overview 
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINTS
```

> 💡 Tip: Look for the drive **without** a mount point. That’s likely your target.

---

### 💽 Step 2: Open `fdisk` to Partition the Disk

```bash
fdisk /dev/nvme0n1  # Replace with your actual disk name!
```

---

### ✏️ Step 3: Create a New Partition Layout

> ⚠️ **Warning:** The steps below **erase all existing data** on the disk.

---

### 🧼 1. Wipe and Create a New GPT Table

```bash
Command: g  # [Enter]
```

---

### 📂 2. Create Partition 1 – EFI System (1 GB)

```bash
Command: n            # [Enter]
Partition number: 1   # [Enter]
First sector:         # [Enter] (default)
Last sector: +1G      # [Enter]
```

---

### 🔁 3. Create Partition 2 – Swap (1 GB)

```bash
Command: n            # [Enter]
Partition number: 2   # [Enter]
First sector:         # [Enter]
Last sector: +1G      # [Enter]
```

---

### 📦 4. Create Partition 3 – Root (Remaining Space)

```bash
Command: n            # [Enter]
Partition number: 3   # [Enter]
First sector:         # [Enter]
Last sector:          # [Enter] (use all remaining space)
```

---

### 🛠️ 5. Set Partition Types

Set the partition type for each partition:

```bash
Command: t            # [Enter]
Partition number: 1   # [Enter]
Hex code: 1           # EFI System (EF00)
```

```bash
Command: t            # [Enter]
Partition number: 2   # [Enter]
Hex code: 19          # Linux Swap (8200)
```

```bash
Command: t            # [Enter]
Partition number: 3   # [Enter]
Hex code: 44          # Linux Root (8304)
```

---

### 🔍 6. Review the Partition Table

```bash
Command: p  # [Enter]
```

> 🧠 Double-check that each partition is correctly sized and typed.

---

### 💾 7. Write Changes and Exit

```bash
Command: w  # [Enter]  ✅ Changes are now saved to disk.
```

> 🔍 Always verify the correct device (`/dev/nvme0n1`, etc.) using `lsblk` before proceeding.

---

**Partition Scheme:**
| Partition | Size  | Type          | Purpose              |
|-----------|-------|---------------|----------------------|
| /dev/nvme0n1p1 | 1GB   | EFI System    | UEFI boot (FAT32)    |
| /dev/nvme0n1p2 | 1GB   | Linux FS      | /boot (ext4)         |
| /dev/nvme0n1p3 | Remaining | Linux LVM   | Encrypted LVM container |

---

## 4️⃣ Encryption & LVM Setup
```bash
# Format partitions
mkfs.fat -F32 /dev/nvme0n1p1
mkfs.ext4 /dev/nvme0n1p2

# Encrypt LVM partition
cryptsetup luksFormat /dev/nvme0n1p3
cryptsetup open /dev/nvme0n1p3 lvm

# Configure LVM
pvcreate /dev/mapper/lvm
vgcreate volgroup0 /dev/mapper/lvm
lvcreate -L 30GB volgroup0 -n lv_root
lvcreate -L 250GB volgroup0 -n lv_home
```

## 5️⃣ Filesystems & Mounting
```bash
# Format volumes
mkfs.ext4 /dev/volgroup0/lv_root
mkfs.ext4 /dev/volgroup0/lv_home

# Mount hierarchy
mount /dev/volgroup0/lv_root /mnt
mkdir /mnt/boot && mount /dev/nvme0n1p2 /mnt/boot
mkdir /mnt/home && mount /dev/volgroup0/lv_home /mnt/home
```

## 6️⃣ Base System Installation
```bash
pacstrap /mnt base base-devel linux linux-headers
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
```

## 7️⃣ System Configuration
**Timezone & Locale:**
```bash
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
hwclock --systohc
vim /etc/locale.gen  # Uncomment en_US.UTF-8
locale-gen
```

**User Setup:**
```bash
passwd  # Set root password
useradd -m -G wheel -s /bin/bash username
passwd username
visudo  # Uncomment %wheel ALL=(ALL:ALL) ALL
```

## 8️⃣ Bootloader & Initramfs
**Initramfs:**
```bash
vim /etc/mkinitcpio.conf  # Add encrypt lvm2 to HOOKS
mkinitcpio -P
```

**GRUB Configuration:**
```bash
# Mount EFI partition
mkdir /boot/EFI && mount /dev/nvme0n1p1 /boot/EFI

# Configure GRUB
echo "GRUB_CMDLINE_LINUX=\"cryptdevice=/dev/nvme0n1p3:volgroup0\"" >> /etc/default/grub
grub-install --target=x86_64-efi --efi-directory=/boot/EFI --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

## 9️⃣ Essential Packages
```bash
pacman -S networkmanager grub efibootmgr lvm2 \
           sudo vim gnome gnome-tweaks openssh \
           nvidia nvidia-utils  # GPU specific
```

## 🔟 Enable Services & Reboot
```bash
systemctl enable gdm NetworkManager sshd
exit
umount -R /mnt
reboot
```

---

## 🎉 Post-Installation
- Complete GNOME setup via GDM
- Verify encrypted partitions:
  ```bash
  lsblk -f
  ```
- Install additional packages with `pacman`

> **Troubleshooting Tip**: If boot fails, boot from USB and `arch-chroot` to regenerate initramfs
