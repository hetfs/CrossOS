<response>
# How do you install it with Winget, Choco, and Scoop?

## 🗒️ Answer

To install software using Winget, Choco, and Scoop, follow these steps:

### 1. **Winget (Windows Package Manager)**
- **To install an application:**
  1. Open Command Prompt or PowerShell.
  2. Use the following command:
     ```bash
     winget install <package_name>
     ```
  3. Replace `<package_name>` with the name of the application you want to install. For example, to install Git:
     ```bash
     winget install Git.Git
     ```

### 2. **Choco (Chocolatey)**
- **To install an application:**
  1. Open Command Prompt as an Administrator.
  2. Use the following command:
     ```bash
     choco install <package_name>
     ```
  3. Replace `<package_name>` with the name of the application you want to install. For example, to install Git:
     ```bash
     choco install git
     ```

### 3. **Scoop**
- **To install an application:**
  1. Open PowerShell.
  2. Use the following command:
     ```bash
     scoop install <package_name>
     ```
  3. Replace `<package_name>` with the name of the application you want to install. For example, to install Git:
     ```bash
     scoop install git
     ```

## 🌐 Sources
1. [learn.microsoft.com - Use the WinGet tool to install and manage applications](https://learn.microsoft.com/en-us/windows/package-manager/winget/#:~:text=To%20search%20for%20a%20tool,typing%20winget%20install%20.)
2. [chocolatey.org - chocolatey/choco - the package manager for Windows](https://github.com/chocolatey/choco)
3. [learn.microsoft.com - install command (winget)](https://learn.microsoft.com/en-us/windows/package-manager/winget/install)
