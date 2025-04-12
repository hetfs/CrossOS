# Command Line and Terminal Basics

# 

## Why Learn Command Line/Terminal?

1. **Greater Control**: Provides precise control over system operations.
2. **Speed & Efficiency**: Offers faster execution of commands compared to graphical interfaces.
3. **Access Remote Servers**: Enables management of remote systems.
4. **Command Line Tools**: Utilizes various tools and utilities for enhanced functionality.

## Terminology

- **Terminal**: Software or hardware used to type and execute commands. Modern terminals are software-based "terminal emulators."
- **Command Line**: The interface within the terminal for executing commands. Often used interchangeably with "command prompt" in Windows.
- **Shell**: The program that runs in the terminal, serving as the "OS" of the terminal environment.

## Evolution of Windows Shells

1. **COMMAND.COM**: The original shell for MS-DOS, later replaced by CMD.EXE.
2. **CMD.EXE**: The default shell in earlier Windows versions, with limited features.
3. **PowerShell**: A more advanced shell with different commands compared to Unix-based shells like Bash and Zsh. Alternatives include Git Bash and WSL (Windows Subsystem for Linux).

## Customizing the Command Line Interface (CLI)

- **Change CLI Color**: Adjust colors in terminal settings or using specific commands.
- **Change CLI Prompt**: Modify the prompt appearance through configuration files.
- **Change CLI Title**: Update the terminal window title in settings or via commands.

## Basic Commands and Navigation

- **Help Command**: Use `name /?` or `help name` for command help.
- **Stat Command**: Opens a new window.
- **Home Directory**: Use `cd -` to return to the home directory.

## Navigating Drives in CMD

To open or switch drives, use the following commands:

1. **Change Drive**: Type the drive letter followed by `:` (e.g., `D:`) to switch to that drive.
2. **Change Directory**: Use `cd /d D:\folder_name` to change both drive and directory simultaneously.
3. **Switch Directory on Same Drive**: Use `cd folder_name` without specifying the drive.

### Troubleshooting Common Issues

- **Cannot Access D Drive**: Use `D:` to switch drives, then navigate using `cd` commands.
- **Changing Directory to Another Drive**: Use `cd /d D:\Docs\Java` to access directories on different drives.
- **Returning to Previous Directory**: Windows does not have `cd -` like Linux; use `pushd` and `popd` instead.

## Additional Tips

- **Navigate Up One Level**: Use `cd ..`.
- **Go to Root Directory**: Use `cd /`.
- **Go to Home Directory**: Use `cd` or `cd ~`.

## Updating PowerShell

- **Using Winget**: Open Windows Terminal and run:
  
  ```sh
  winget install --id Microsoft.Powershell --source winget
  ```

- **Alternative Methods**: Update through the Microsoft Store or download from the PowerShell GitHub repository.
