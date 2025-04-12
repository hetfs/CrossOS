To install MSYS2 using `winget`, the Windows Package Manager, follow these steps:

### Step 1: Open Command Prompt or PowerShell

1. Press `Windows + X` and choose **Windows Terminal** (or **Command Prompt** or **PowerShell**).

2. Ensure you have `winget` installed by running:
   
   ```bash
   winget --version
   ```
   
   If it's not installed, you may need to update Windows to the latest version.

### Step 2: Search for MSYS2

1. To find the exact package name for MSYS2, you can search the `winget` repository:
   
   ```bash
   winget search MSYS2
   ```

### Step 3: Install MSYS2

1. Once you have the package name, install MSYS2 by running:
   
   ```bash
   winget install MSYS2
   ```
   
   Alternatively, you can specify the exact ID if the search result gives a more specific name, such as:
   
   ```bash
   winget install MSYS2.MSYS2
   ```

### Step 4: Follow the On-Screen Prompts

1. During installation, you might be prompted to approve the installer or adjust settings. Follow the prompts to complete the installation.

### Step 5: Verify Installation

1. After installation, you can start MSYS2 by searching for "MSYS2" in the Start menu.

2. Optionally, you can verify the installation by running:
   
   ```bash
   pacman --version
   ```
   
   This will confirm that the package manager `pacman` has been installed, which is essential for managing MSYS2 packages.

### Additional Steps (Optional)

- **Update MSYS2:** Run the following commands to update the package database and installed packages:
  
  ```bash
  pacman -Syu
  ```

- **Install Additional Tools:** You can use `pacman` to install additional development tools, such as:
  
  ```bash
  pacman -S git base-devel mingw-w64-x86_64-toolchain
  ```

That's it! You have successfully installed MSYS2 using `winget` on your Windows machine.

[MSYS2 · GitHub](https://github.com/msys2)
