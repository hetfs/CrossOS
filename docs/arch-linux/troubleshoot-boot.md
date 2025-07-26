
## 🧯 Troubleshooting Boot Issues After Install

If your system fails to boot after installation—especially when using **LUKS encryption + LVM + GRUB**—don’t panic. You can **repair it** using the Arch ISO and `arch-chroot`.

### 🚑 Common Boot Failures:

* Black screen or "kernel panic" on startup
* GRUB loads but kernel doesn't boot
* Errors about missing `/dev/mapper` devices or root volume

---

### 🛠️ How to Fix It: Rebuild Initramfs with `arch-chroot`

> 💡 `initramfs` (initial RAM filesystem) is required to unlock encrypted disks and activate LVM at boot. If it's misconfigured or missing required hooks (`encrypt`, `lvm2`), the system won’t boot.

---

### ✅ Step-by-Step Recovery (From USB)

#### 1️⃣ Boot From the Arch USB

* Plug in your **Arch ISO USB**
* Boot into the **Live Environment** (just like during installation)

---

#### 2️⃣ Unlock Your Encrypted Disk

> Replace `nvme0n1p3` with your encrypted LUKS partition

```bash
cryptsetup open /dev/nvme0n1p3 lvm
```

---

#### 3️⃣ Activate LVM Volumes

```bash
vgscan
vgchange -ay
```

---

#### 4️⃣ Mount Your Filesystems

```bash
mount /dev/volgroup0/lv_root /mnt
mount /dev/nvme0n1p2 /mnt/boot        # /boot partition
mount /dev/nvme0n1p1 /mnt/boot/EFI    # EFI system partition
mount /dev/volgroup0/lv_home /mnt/home  # optional, if you use separate /home
```

---

#### 5️⃣ Enter Your Installed System

```bash
arch-chroot /mnt
```

Now you're "inside" your real Arch install — just like after setup.

---

#### 6️⃣ Rebuild initramfs

```bash
mkinitcpio -P
```

> 🧠 This uses `/etc/mkinitcpio.conf`. Make sure it includes the right hooks:
>
> ```ini
> HOOKS=(base udev autodetect modconf block encrypt lvm2 filesystems keyboard fsck)
> ```

---

#### 7️⃣ (Optional) Regenerate GRUB Config

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

---

#### 8️⃣ Exit and Reboot

```bash
exit
umount -R /mnt
reboot
```

---

### 🔍 Still Not Booting?

* Double-check UUIDs in `/etc/default/grub`
* Ensure `cryptdevice=` is set correctly in GRUB kernel line:

  ```bash
  GRUB_CMDLINE_LINUX="cryptdevice=/dev/nvme0n1p3:volgroup0"
  ```
* Re-run:

  ```bash
  grub-install --target=x86_64-efi --efi-directory=/boot/EFI --bootloader-id=GRUB
  ```

---

### 🧠 Summary (What You Just Did)

| Step                 | Command                                  |
| -------------------- | ---------------------------------------- |
| Unlock LUKS          | `cryptsetup open /dev/nvme0n1p3 lvm`     |
| Reactivate LVM       | `vgchange -ay`                           |
| Mount Filesystems    | `mount /dev/volgroup0/lv_root /mnt` etc. |
| Enter System         | `arch-chroot /mnt`                       |
| Rebuild Initramfs    | `mkinitcpio -P`                          |
| Fix GRUB (if needed) | `grub-mkconfig -o /boot/grub/grub.cfg`   |

