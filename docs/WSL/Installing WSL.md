Here's a rewritten version of your content:

---

### Installing WSL

You can now install everything required to run the Windows Subsystem for Linux (WSL) with a single command. To get started, open PowerShell or Command Prompt as an administrator (right-click and select "Run as administrator"), and enter:

```bash
wsl --install
```

After running this command, restart your machine. This will enable all necessary WSL components and install the default Ubuntu Linux distribution (though you can change this distribution if needed). 

If you’re using an older Windows build or prefer manual setup, refer to the [WSL manual installation guide](#).

### First-Time Setup

When you launch a newly installed Linux distribution for the first time, a console window will open to decompress and store files on your machine. Subsequent launches should take less than a second.

**Note:**  
The `wsl --install` command only works if WSL isn’t already installed. If WSL is installed and you see a help message instead, try running:

```bash
wsl --list --online
```

This command lists all available distributions. To install one, use:

```bash
wsl --install -d <DistroName>
```

For instructions on removing WSL or unregistering a distribution, see [Uninstall WSL](#).

### Changing the Default Linux Distribution

Ubuntu is the default Linux distribution installed with WSL, but you can change this using the `-d` flag. To install a different distribution, run:

```bash
wsl --install -d <DistributionName>
```

To view available Linux distributions, enter:

```bash
wsl --list --online
```

After the initial installation, you can add more distributions with the same command:

```bash
wsl --install -d <DistributionName>
```

**Tip:**  
If you’re using the Linux/Bash command line instead of PowerShell or Command Prompt, include `.exe` in the command:

```bash
wsl.exe --install -d <DistributionName>
```

For troubleshooting installation issues, refer to the [WSL troubleshooting guide](#).

### Setting Up Your Linux User Info

After installing WSL, you’ll need to create a username and password for your Linux distribution. Refer to the [Best practices guide](#) for detailed steps on setting up your WSL environment.

### Best Practices for WSL Setup

For a full guide on setting up your WSL development environment—including configuring Git, using VS Code, customizing Windows Terminal, and enabling GPU acceleration—follow our [Best practices for WSL setup](#).

### Checking Your WSL Version

To see a list of installed Linux distributions and their WSL version, use:

```bash
wsl -l -v
```

To set the default version (WSL 1 or WSL 2) for new installations, use:

```bash
wsl --set-default-version <Version#>
```

To change the default Linux distribution for the `wsl` command, enter:

```bash
wsl -s <DistributionName>
```

For example, to set Debian as the default distribution:

```bash
wsl -s Debian
```

This means that running a command like `wsl npm init` from PowerShell will execute inside Debian.

To run a specific distribution without changing your default, use:

```bash
wsl -d <DistributionName>
```

### Upgrading from WSL 1 to WSL 2

New installations default to WSL 2. To upgrade or downgrade between WSL 1 and WSL 2 for any distribution, use:

```bash
wsl --set-version <DistroName> 2
```

For example:

```bash
wsl --set-version Ubuntu-20.04 2
```

If WSL was manually installed before the `wsl --install` command existed, you may need to enable the virtual machine component for WSL 2 and install the kernel package. Refer to the [WSL command reference](#) and [WSL 1 vs. WSL 2 comparison](#) for more information.

### Running Multiple Linux Distributions

You can install and run multiple Linux distributions in WSL. Available methods include:

- **Windows Terminal (Recommended):**  
   Supports multiple command lines in tabs or panes and allows switching between Linux distributions, PowerShell, and Command Prompt. Customize with color schemes, fonts, and shortcuts. Learn more [here](#).

- **Windows Start Menu:**  
   Type the name of a distribution (e.g., "Ubuntu") to open it in its own console window.

- **Command Prompt or PowerShell:**  
   Type the name of the distribution (e.g., `ubuntu`) or use the `wsl.exe` command for more control:
  
  ```bash
  wsl.exe
  ```
  
   You can also run specific commands with:
  
  ```bash
  wsl [command]
  ```
  
   For example, `wsl -l -v` lists installed distributions, and `wsl pwd` shows the current directory path in WSL.

To exit a WSL session, type:

```bash
exit
```

### Trying the Latest WSL Preview Features

If you want to test new features, consider joining the Windows Insiders Program. This allows you to receive preview builds with the latest updates. You can select from three channels:

- **Dev Channel:** Latest features, but may be unstable.
- **Beta Channel:** Early access with more stability than the Dev channel.
- **Release Preview Channel:** Preview fixes and features close to public release.
