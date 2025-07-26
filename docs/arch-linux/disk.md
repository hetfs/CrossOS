## ⚠️ Important Notes: Disk Partition Naming in Linux

### 🔍 Device Naming Schemes

Linux names storage devices based on their type. Understanding these patterns helps avoid critical mistakes during installation or scripting.

#### 📦 NVMe (PCIe-Based Storage)
- Format: `/dev/nvme0n1pX`
- Meaning:
  - `nvme` – Device type (Non-Volatile Memory Express)
  - `0` – Controller ID
  - `n1` – Namespace ID
  - `pX` – Partition number (`p1`, `p2`, etc.)

**Example**: First partition = `/dev/nvme0n1p1`

#### 💽 SATA (SSD or HDD)
- Format: `/dev/sdX#`
- Meaning:
  - `sd` – Storage device prefix
  - `X` – Drive letter (`a`, `b`, `c`, ...)
  - `#` – Partition number (`1`, `2`, `3`, ...)

**Example**: Second partition = `/dev/sda2`

---

### 🧠 Key Differences: NVMe vs. SATA

| Feature              | NVMe (PCIe)           | SATA (AHCI)        |
|----------------------|------------------------|---------------------|
| **Interface**        | PCI Express             | SATA/AHCI           |
| **Naming Format**    | `/dev/nvme0n1pX`        | `/dev/sdX#`         |
| **Partition Marker** | Includes `p` before number | No `p` prefix     |
| **Speed**            | Much faster (3–7× SATA) | Slower baseline     |
| **Structure**        | Hierarchical (4-part)   | Simpler (3-part)    |

---

### 🛠️ Practical Tips

1. Run `lsblk -f` or `fdisk -l` to double-check drive and partition names.
2. For NVMe, always include the `p` in partition names (e.g., `nvme0n1p1`).
3. In UEFI systems:
   - EFI on NVMe: `/dev/nvme0n1p1`
   - EFI on SATA: `/dev/sda1`
4. GRUB and bootloader configs must match the exact partition path (`root=...`).

> 💡 **Pro Tip**: The `p` in `nvme0n1pX` ensures clarity between the namespace (`n1`) and partition number, which isn’t needed for SATA devices due to their simpler structure.
