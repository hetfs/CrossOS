# Install additional languages in Arch Linux

## 1. **Install Locale Packages**
```bash
sudo pacman -S glibc  # Contains base locale data
sudo pacman -S locales # Meta-package for all locales (optional)
```

## 2. **Enable Desired Locales**
Edit `/etc/locale.gen`:
```bash
sudo nano /etc/locale.gen
```
Uncomment your language(s), e.g.:
```ini
en_US.UTF-8 UTF-8   # English (US)
fr_FR.UTF-8 UTF-8   # French
de_DE.UTF-8 UTF-8   # German
ja_JP.UTF-8 UTF-8   # Japanese
zh_CN.UTF-8 UTF-8   # Chinese (Simplified)
```

Generate locales:
```bash
sudo locale-gen
```

## 3. **Set System-Wide Language**
Create `/etc/locale.conf`:
```bash
echo "LANG=en_US.UTF-8" | sudo tee /etc/locale.conf
```
Replace `en_US.UTF-8` with your preferred locale.

## 4. **Set User-Specific Language**
Add to your shell config (`~/.bashrc` or `~/.zshrc`):
```bash
export LANG=en_US.UTF-8
export LC_COLLATE=C
```

## 5. **Install Input Methods (For Non-Latin Languages)**
**For Chinese/Japanese/Korean:**
```bash
# IBus framework
sudo pacman -S ibus ibus-anthy ibus-hangul ibus-libpinyin ibus-rime

# Fcitx5 (modern alternative)
sudo pacman -S fcitx5 fcitx5-gtk fcitx5-qt fcitx5-configtool \
               fcitx5-mozc fcitx5-hangul fcitx5-rime
```

**For Indic/Southeast Asian languages:**
```bash
sudo pacman -S ibus-m17n
```

## 6. **Enable Input Method**
**For IBus:**
1. Add to startup:
   ```bash
   echo 'export GTK_IM_MODULE=ibus' >> ~/.xprofile
   echo 'export XMODIFIERS=@im=ibus' >> ~/.xprofile
   echo 'export QT_IM_MODULE=ibus' >> ~/.xprofile
   echo 'ibus-daemon -drx' >> ~/.xprofile
   ```
2. Configure:
   ```bash
   ibus-setup
   ```

**For Fcitx5:**
1. Add to startup:
   ```bash
   echo 'export GTK_IM_MODULE=fcitx' >> ~/.xprofile
   echo 'export QT_IM_MODULE=fcitx' >> ~/.xprofile
   echo 'export XMODIFIERS=@im=fcitx' >> ~/.xprofile
   echo 'fcitx5 -d' >> ~/.xprofile
   ```
2. Configure:
   ```bash
   fcitx5-configtool
   ```

## 7. **Install Language-Specific Fonts**
**Japanese:**
```bash
sudo pacman -S otf-ipafont
```

**Chinese:**
```bash
sudo pacman -S noto-fonts-cjk
```

**Korean:**
```bash
sudo pacman -S noto-fonts-cjk ttf-baekmuk
```

## 8. **Set Keyboard Layout (Optional)**
Edit `/etc/X11/xorg.conf.d/00-keyboard.conf`:
```ini
Section "InputClass"
    Identifier "system-keyboard"
    MatchIsKeyboard "on"
    Option "XkbLayout" "us,de,fr,jp"
    Option "XkbOptions" "grp:alt_shift_toggle"
EndSection
```
Layout codes: `us` (English), `de` (German), `fr` (French), `jp` (Japanese)

## 9. **Verify Configuration**
Check active locale:
```bash
locale
```
Test input method by switching with:
- IBus: `Super`+`Space`
- Fcitx5: `Ctrl`+`Space`

---

## Troubleshooting
**Fonts not rendering:**
- Install missing fonts: `sudo pacman -S noto-fonts`
- Clear font cache: `fc-cache -fv`

**Input method not starting:**
- Ensure variables are set: `env | grep IM_MODULE`
- Check running processes: `ps aux | grep ibus` or `ps aux | grep fcitx`

**Partial translation:**
Install full language packs:
```bash
sudo pacman -S gnome-l10n-ja  # GNOME Japanese
sudo pacman -S kde-l10n-ja    # KDE Japanese
```
