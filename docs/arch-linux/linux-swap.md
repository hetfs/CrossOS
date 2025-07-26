# 🧠 Linux Swap Fundamentals

*Optimize memory usage and system resilience*

Swap is a **dedicated disk-backed memory extension**. It works by moving inactive memory pages from RAM to disk, helping the system stay stable under pressure.

### 🔧 Why Swap Still Matters:

- **💥 Crash Prevention**: Acts as a safeguard against OOM (Out-Of-Memory) errors.

- **💤 Hibernation Support**: Required to suspend RAM contents to disk.

---

## ⚖️ When Swap Is Required vs Optional

| **Scenario**              | **Swap Needed?** | **Why It Matters**                             |
| ------------------------- | ---------------- | ---------------------------------------------- |
| **RAM ≤ 4 GB**            | ✅ Required       | Prevents freezes and crashes under memory load |
| **Hibernate Needed**      | ✅ Required       | Stores RAM snapshot for suspend-to-disk        |
| **RAM 8–16 GB**           | ⚠️ Recommended   | Handles unexpected memory spikes               |
| **RAM ≥ 32 GB (Desktop)** | ⚖️ Optional      | Useful only in rare heavy-load edge cases      |
| **Servers / Containers**  | ⚖️ Optional      | Depends on workload and overcommit settings    |

---

## 🗃️ Swap Partition vs. Swap File

| **Type**      | **Performance**               | **Flexibility** | **Ideal Use Cases**            |
| ------------- | ----------------------------- | --------------- | ------------------------------ |
| **Partition** | Slightly faster `(~2–3%)`       | ❌ Fixed size    | Legacy setups, HDD systems     |
| **File**      | Near-native `(<1% diff on SSD)` | ✅ Resizable     | Modern SSDs, portable installs |

> On NVMe SSDs, swap files are fully TRIM-compatible and often preferable.

---

## 📐 Swap Sizing Guidelines (2024)

| **RAM Size** | **Minimum Swap** | **Recommended** | **For Hibernation** |
| ------------ | ---------------- | --------------- | ------------------- |
| **2 GB**     | 2 GB             | 4 GB            | 6 GB                |
| **4 GB**     | 2 GB             | 4 GB            | 8 GB                |
| **8 GB**     | 1 GB             | 4 GB            | 12 GB               |
| **16 GB**    | 0 GB*            | 2 GB            | 20 GB               |
| **32 GB+**   | 0 GB             | 1 GB            | RAM size + 4 GB     |

> *You can omit swap if using zram or if memory overcommit is tuned appropriately.

---

## 🏗️ Creating Swap During Installation

### 🔹 Method 1: Swap Partition

```bash
cfdisk /dev/nvme0n1
# [New] → Size: +4G → Type: Linux swap → [Write]
```

### 🔹 Method 2: Swap File (Post-Install)

```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=4096 status=progress
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap defaults 0 0' | sudo tee -a /etc/fstab
```

---

## ⚡ Advanced Swap Optimization

### 1. **SSD-Aware Tuning**

```bash
echo 'vm.swappiness=10' | sudo tee /etc/sysctl.d/99-swap.conf
echo 'vm.vfs_cache_pressure=50' | sudo tee -a /etc/sysctl.d/99-swap.conf
```

- Reduces unnecessary writes to SSD

- Keeps frequently used filesystem data cached longer

---

### 2. **ZRAM for In-RAM Compression**

For systems with ≥8 GB RAM:

```bash
sudo pacman -S zram-generator
sudo systemctl enable systemd-zram-setup@zram0
```

- Uses compressed RAM for fast swap

- Great for laptops and multitasking

---

### 3. **Dynamic Swap Manager (Optional)**

```bash
sudo pacman -S systemd-swap
sudo systemctl enable systemd-swap
```

- Automatically creates, resizes, and manages swap files based on usage

---

## 🔍 Check Your Swap Status

```bash
swapon --show               # List active swap areas
free -h                     # Show memory and swap usage
lsblk                       # Visualize swap in partitions
grep Swap /proc/meminfo     # Kernel-level swap metrics
```

---

## 💡 Pro Tips

1. **On SSD/NVMe**, swap files are preferred due to TRIM and flexibility.

2. **Avoid large static swap** on systems with ample RAM — use zram or dynamic swap.

3. **For databases or HPC**, isolate swap to a separate disk if possible.

4. Enable TRIM for swap on SSDs with:
   
   ```bash
   sudo systemctl enable fstrim.timer
   ```

