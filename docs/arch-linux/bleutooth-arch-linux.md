# Enable Bluetooth in Arch Linux

## 1. **Install Required Packages**
```bash
sudo pacman -S bluez bluez-utils
```
- `bluez`: Bluetooth protocol stack
- `bluez-utils`: Command-line utilities (includes `bluetoothctl`)

## 2. **Start and Enable Services**
```bash
sudo systemctl enable --now bluetooth.service
```
- `enable`: Activates on boot
- `--now`: Starts immediately

## 3. **Load Bluetooth Kernel Module**
```bash
sudo modprobe btusb
```
To make this persistent:
```bash
echo "btusb" | sudo tee /etc/modules-load.d/btusb.conf
```

## 4. **Basic Device Management**
Use `bluetoothctl` to manage devices:
```bash
bluetoothctl
```
Inside the interactive prompt:
```bash
power on          # Turn on Bluetooth
agent on          # Enable agent
default-agent     # Set default agent
scan on           # Search for devices
```
Pair and connect:
```bash
pair [device-MAC]  # Replace with device address
trust [device-MAC] # Auto-connect in future
connect [device-MAC]
```

---

## 5. **Troubleshooting**
**Check adapter status:**
```bash
bluetoothctl show
```

**Restart services if needed:**
```bash
sudo systemctl restart bluetooth
sudo rfkill unblock bluetooth
```

**Check kernel recognition:**
```bash
dmesg | grep -i bluetooth
lsusb | grep -i bluetooth
```

---

## 6. **Optional: GUI Tools**
For graphical management:
```bash
sudo pacman -S blueman  # GTK interface
```
Launch with:
```bash
blueman-applet &  # System tray applet
blueman-manager   # Device manager
```

---

## 7. **Audio Support (A2DP)**
For Bluetooth headphones/speakers:
```bash
sudo pacman -S pulseaudio-bluetooth
pulseaudio -k  # Restart PulseAudio
```
Then reconnect your audio device.

---

## Verify Connection
Check paired devices:
```bash
bluetoothctl devices Paired
```

**Common Fixes:**
- If devices fail to connect:
  ```bash
  sudo systemctl restart bluetooth
  bluetoothctl remove [device-MAC]  # Re-pair device
  ```
- Missing codecs? Install `pulseaudio-alsa` and `pulseaudio-bluetooth-a2dp-gdm-fix`
