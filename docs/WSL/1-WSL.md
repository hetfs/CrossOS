# Setting Up WSL on Windows 10/11

Windows Subsystem for Linux (WSL) is a compatibility layer for running Linux binary executables natively on Windows 10 and Windows 11. It allows you to run a Linux distribution alongside your Windows operating system, providing a full Linux experience without the need for a virtual machine or dual-boot setup.

### Key Features of WSL:

1. **Linux Kernel**: WSL 2, the latest version, includes a real Linux kernel, which improves system call compatibility and performance.

2. **File System Access**: You can access Windows files from within your Linux environment and vice versa.

3. **Integration**: You can run Linux commands and tools alongside Windows applications, and use them in PowerShell or Command Prompt.

4. **Development**: It's particularly useful for developers who need a Linux environment for their work, including those working with tools and languages that are more commonly used on Linux.

### Getting Started with WSL:

1. **Install WSL**: Open PowerShell as an administrator and run:
   
   ```powershell
   wsl --install
   ```
   
   This installs the default Linux distribution (usually Ubuntu) and WSL 2.

2. **List Available Distributions**: To see a list of available Linux distributions, use:
   
   ```powershell
   wsl --list --online
   ```

3. **Install a Specific Distribution**: To install a different distribution, for example, Debian:
   
   ```powershell
   wsl --install -d Debian
   ```

4. **Access WSL**: After installation, you can open your Linux distribution by typing its name in the Start menu or by running `wsl` in PowerShell or Command Prompt.

WSL provides a great way to use Linux tools and environments directly on a Windows machine, making it easier for developers and users who need both operating systems.

## Enabling WSL

### For CMD Users:

1. **Open CMD**: Launch Command Prompt by searching for "CMD" in the Start menu.

2. **Enable WSL**: Type the following command to open the Windows Features window:
   
   ```
   OptionalFeatures.exe
   ```
   
   In the window that appears, check the boxes for **Windows Subsystem for Linux** and **Virtual Machine Platform**, then click **OK**.

3. **Restart**: Reboot your system to apply the changes.

### For PowerShell Users:

1. **Open PowerShell as Administrator**: Right-click the Start button and select **Windows PowerShell (Admin)**.

2. **Enable WSL**: Run the following commands to enable WSL and additional features required for WSL 2:
   
   ```
   Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   ```

3. **Restart**: Restart your system to apply the changes.

**Note**: Windows 11 provides an optimal experience for WSL 2. If you're on Windows 10, ensure your system is updated to the latest version for full WSL 2 support. For more details, visit the official [Microsoft WSL documentation](https://learn.microsoft.com/en-us/windows/wsl/install).

---

## Checking Your WSL Version

1. **Open Terminal**: Press `Windows + X` and choose **Windows Terminal (Admin)** or **Command Prompt (Admin)**.

2. **Check WSL Version**: Run the following command to see a list of installed Linux distributions and their versions:
   
   ```
   wsl -l -v
   ```
   
   - **Version 1** indicates WSL1, while **Version 2** indicates WSL2.
   
   Example output:
   
   ```
   C:\Users\YourUsername>wsl -l -v
   NAME            STATE           VERSION
   * Ubuntu-20.04    Stopped         2
   ```

3. **Set WSL 2 as Default**:
   
   ```
   wsl --set-default-version 2
   ```

4. **Check Windows Build Version**: Ensure your Windows build number is 18917 or higher by typing `winver` in the Run dialog (`Windows + R`). This is required for WSL 2 compatibility.

---

## Troubleshooting: `dism.exe` Not Recognized Error

If you encounter the `dism.exe` not recognized error, follow these steps:

1. **Verify DISM Installation**: Run `dism.exe` in Command Prompt or PowerShell to confirm if it's installed.

2. **Correct the PATH**: Ensure that the `Path` variable under "System variables" includes the path to `dism.exe`, typically found in `C:\Windows\System32`.

3. **Run as Administrator**: Make sure you're running PowerShell or Command Prompt as an administrator.

4. **Repair Windows Image**: If `dism.exe` is missing or corrupt, use the following command to repair your Windows image:
   
   ```
   sfc /scannow
   ```

5. **Reinstall WSL**: If issues persist, consider reinstalling WSL. Follow the steps provided in the [official guide](https://learn.microsoft.com/en-us/windows/wsl/install).

---

## Installing a Linux Distribution on WSL

### List Installed Distributions:

```
wsl --list
```

### List Available Distributions:

```
wsl --list --online
```

### Install a Distribution:

```
wsl --install -d <Distro-Name>
```

## Installing and Setting Up Kali Linux with Win-KeX

### Minimum Setup Requirements:

Ensure your system meets the [minimum setup requirements](https://www.kali.org/docs/troubleshooting/common-minimum-setup/) before proceeding.

### Install Kali Linux:

Run the following command and follow the prompts:

```
wsl --install -d Kali-Linux
```

### Update Kali Linux and Install Win-KeX:

After installing Kali Linux, launch it and update the package lists:

```
sudo apt update
```

Then, install the Kali Win-KeX package:

```
sudo apt install -y kali-win-kex
```

### Launch Win-KeX:

Start the Win-KeX session with the command:

```
kex --win -s
```

This command will launch the graphical interface, enabling you to run Linux GUI applications directly in Windows.

## Additional Resources:

- [Kali Linux Documentation on WSL](https://www.kali.org/docs/wsl/)
- [Kali Linux Release History](https://www.kali.org/releases/)
- [Video Guide on Setting Up WSL for Kali Linux](https://www.youtube.com/watch?v=UXyS-xofGNM)
- [WSL on Windows Server 2022](https://learn.microsoft.com/en-us/windows/wsl/install-on-server#install-wsl-on-windows-server-2022)
