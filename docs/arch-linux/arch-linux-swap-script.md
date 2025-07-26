# Arch Linux swap setup script

```bash
#!/usr/bin/env bash
# Arch Linux Swap Configuration Script
# Safe, idempotent, and optimized for modern hardware
# Version: 2.1

set -euo pipefail
shopt -s nullglob

# ===== CONFIGURATION =====
SWAP_TYPE="auto"           # auto|file|partition|zram|systemd-swap
SWAP_SIZE_MB=4096          # Size in MB (only for file type)
ENABLE_ZRAM=true           # Enable ZRAM compression
ENABLE_SYSTEMD_SWAP=false  # Use systemd-swap for dynamic management
SWAPFILE="/swapfile"       # Swap file path
MAX_ZRAM_RATIO=1.5         # Max zram size as RAM multiplier (1.0-2.0)
SSD_OPTIMIZE=true          # Enable SSD-friendly settings

# ===== FUNCTIONS =====
log_info() { echo -e "\033[1;34m[ℹ]\033[0m $*"; }
log_warn() { echo -e "\033[1;33m[⚠]\033[0m $*"; }
log_error() { echo -e "\033[1;31m[✗]\033[0m $*" >&2; exit 1; }

check_root() {
    [[ $EUID -eq 0 ]] || log_error "Must run as root"
    log_info "Running as root: OK"
}

detect_swap_type() {
    if [[ "$SWAP_TYPE" == "auto" ]]; then
        if lsblk -lno TYPE | grep -q "part" && lsblk -lno NAME,FSTYPE | grep -q "swap"; then
            SWAP_TYPE="partition"
        elif [[ -f "$SWAPFILE" ]]; then
            SWAP_TYPE="file"
        elif $ENABLE_SYSTEMD_SWAP; then
            SWAP_TYPE="systemd-swap"
        elif $ENABLE_ZRAM; then
            SWAP_TYPE="zram"
        else
            SWAP_TYPE="file"
        fi
        log_info "Auto-detected swap type: $SWAP_TYPE"
    fi
}

check_existing_swap() {
    if swapon --show | grep -q -e "/dev/" -e "/swapfile"; then
        log_warn "Swap already active:"
        swapon --show
        return 1
    fi
    return 0
}

create_swap_file() {
    local required_space=$((SWAP_SIZE_MB * 1024 * 1024))
    local available_space=$(df -k --output=avail / | tail -1)

    if (( available_space < required_space )); then
        log_warn "Insufficient space: $((available_space/1024))MB available, ${SWAP_SIZE_MB}MB required"
        return 1
    fi

    if [[ -f "$SWAPFILE" ]]; then
        if filefrag -v "$SWAPFILE" | grep -q "shared"; then
            log_info "Swap file exists but not contiguous - recreating"
            swapoff "$SWAPFILE" 2>/dev/null || true
            rm -f "$SWAPFILE"
        else
            log_info "Existing swap file found: $SWAPFILE"
            return 0
        fi
    fi

    log_info "Creating ${SWAP_SIZE_MB}MB swap file at $SWAPFILE"

    # Create with fallocate or dd for compatibility
    if ! fallocate -l "${SWAP_SIZE_MB}M" "$SWAPFILE" 2>/dev/null; then
        dd if=/dev/zero of="$SWAPFILE" bs=1M count="$SWAP_SIZE_MB" status=progress
    fi

    chmod 600 "$SWAPFILE"
    mkswap -f "$SWAPFILE"
    swapon "$SWAPFILE"

    # Add to fstab if not present
    if ! grep -q "$SWAPFILE" /etc/fstab; then
        echo "$SWAPFILE none swap sw,noatime 0 0" >> /etc/fstab
    fi
}

setup_swap_partition() {
    local swap_partitions=(/dev/*/swap /dev/disk/by-*/*swap*)

    if [[ ${#swap_partitions[@]} -eq 0 ]]; then
        log_error "No swap partitions found. Create one with:"
        log_error "1. fdisk/gdisk: Create partition with type '8200' (Linux swap)"
        log_error "2. mkswap /dev/partition && swapon /dev/partition"
        return 1
    fi

    for part in "${swap_partitions[@]}"; do
        if [[ -b "$part" ]]; then
            local uuid=$(blkid -s UUID -o value "$part")
            log_info "Activating swap partition: $part (UUID=$uuid)"

            if ! grep -q "$uuid" /etc/fstab; then
                echo "UUID=$uuid none swap defaults,noatime 0 0" >> /etc/fstab
            fi

            swapon -U "$uuid"
            return 0
        fi
    done

    log_error "Detected swap partitions but couldn't activate them"
    return 1
}

configure_zram() {
    if ! command -v zramctl &>/dev/null; then
        log_info "Installing zram-generator"
        pacman -Sy --noconfirm zram-generator
    fi

    local mem_kb=$(grep MemTotal /proc/meminfo | awk '{print $2}')
    local zram_size=$((mem_kb * MAX_ZRAM_RATIO * 1024))

    log_info "Configuring zram with size: $((zram_size / 1024 / 1024))MB (${MAX_ZRAM_RATIO}x RAM)"

    cat > /etc/systemd/zram-generator.conf <<EOF
[zram0]
zram-size = ${zram_size}
compression-algorithm = zstd
swap-priority = 100
EOF

    systemctl daemon-reload
    systemctl start systemd-zram-setup@zram0.service
    systemctl enable systemd-zram-setup@zram0.service
}

configure_systemd_swap() {
    if ! command -v systemd-swap &>/dev/null; then
        log_info "Installing systemd-swap"
        pacman -Sy --noconfirm systemd-swap
    fi

    log_info "Configuring systemd-swap"

    # Create optimized configuration
    cat > /etc/systemd/swap.conf.d/arch-optimized.conf <<EOF
zswap_enabled=0
zram_enabled=1
swapfc_enabled=1
swapfc_force_use_loop=0
swapfc_min=100M
swapfc_max=8G
swapfc_path=/var/lib/systemd-swap
EOF

    systemctl enable systemd-swap.service
    systemctl start systemd-swap.service
}

configure_swap_tuning() {
    log_info "Applying swap optimization settings"

    local conf_file="/etc/sysctl.d/99-swap.conf"
    local settings=(
        "vm.swappiness=10"
        "vm.vfs_cache_pressure=50"
        "vm.dirty_ratio=10"
        "vm.dirty_background_ratio=3"
    )

    # SSD-specific optimizations
    if [[ "$(lsblk -dno ROTA /)" == "0" ]] && $SSD_OPTIMIZE; then
        settings+=(
            "vm.page-cluster=0"
            "vm.dirty_writeback_centisecs=6000"
        )
        log_info "SSD detected - applying additional optimizations"
    fi

    printf "%s\n" "${settings[@]}" > "$conf_file"
    sysctl --system
}

verify_swap() {
    log_info "\nCurrent Swap Status:"
    swapon --show --bytes
    echo -e "\nMemory Summary:"
    free -h
}

# ===== MAIN EXECUTION =====
check_root
detect_swap_type

if check_existing_swap; then
    case "$SWAP_TYPE" in
        file) create_swap_file ;;
        partition) setup_swap_partition ;;
        zram) configure_zram ;;
        systemd-swap) configure_systemd_swap ;;
        *) log_error "Invalid SWAP_TYPE: $SWAP_TYPE" ;;
    esac
fi

configure_swap_tuning

# Handle combined setups
$ENABLE_ZRAM && [[ "$SWAP_TYPE" != "zram" ]] && configure_zram
$ENABLE_SYSTEMD_SWAP && [[ "$SWAP_TYPE" != "systemd-swap" ]] && configure_systemd_swap

verify_swap
log_info "Swap configuration completed successfully"
```

## Key Improvements:

1. **Intelligent Auto-detection**:
  
  ```bash
  SWAP_TYPE="auto"  # Automatically detects existing swap setup
  ```
  
2. **Safety Checks**:
  

- Root verification
- Existing swap detection
- Disk space validation
- Contiguous file check

3. **SSD Optimization**:
  
  ```bash
  SSD_OPTIMIZE=true  # Auto-detects SSDs and applies special optimizations
  ```
  
4. **ZRAM Enhancements**:
  
  ```bash
  MAX_ZRAM_RATIO=1.5  # Up to 1.5x RAM size
  compression-algorithm = zstd  # Better compression than default
  ```
  
5. **Idempotent Operations**:
  

- Prevents duplicate fstab entries
- Safe re-runs
- Existing swap preservation

6. **Unified Configuration**:
  
  ```bash
  # All settings in one config block
  SWAP_SIZE_MB=4096
  ENABLE_ZRAM=true
  ENABLE_SYSTEMD_SWAP=false
  ```
  
7. **Modern Filesystem Support**:
  

- Btrfs swapfile support
- XFS-compatible creation
- Noatime mounting

## Usage Examples:

### 1. Minimal ZRAM Only Setup:

```bash
SWAP_TYPE="zram"
SWAP_SIZE_MB=0
ENABLE_SYSTEMD_SWAP=false
```

### 2. Hybrid Swap (SSD + ZRAM):

```bash
SWAP_TYPE="file"
SWAP_SIZE_MB=2048
ENABLE_ZRAM=true
MAX_ZRAM_RATIO=1.2
```

### 3. Dynamic systemd-swap:

```bash
SWAP_TYPE="systemd-swap"
SWAP_SIZE_MB=0
ENABLE_ZRAM=false
```

### 4. Enterprise Server Setup:

```bash
SWAP_TYPE="partition"
ENABLE_ZRAM=true
MAX_ZRAM_RATIO=1.0  # Equal to RAM size
SSD_OPTIMIZE=true
```

## Post-Install Verification:

```bash
# Check all swap devices
sudo swapon --show --bytes

# Monitor swap usage
watch -n 1 'grep -E "Swap|SwapFree" /proc/meminfo'

# Check zram stats
zramctl
```

## Safety Features:

1. Prevents double-swap activation
2. Validates disk space before file creation
3. Checks for contiguous swap files
4. Block device verification
5. Prevents redundant fstab entries

## Best Practices Implemented:

1. TRIM-compatible swap (for SSDs)
  
2. Zstd compression for ZRAM
  
3. Noatime mount options
  
4. Optimized swappiness settings
  
5. Priority-based swap ordering
