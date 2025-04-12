# Hiding the "All Rights Reserved" Message in CMD

## Method 1: Using Windows Terminal Settings

To hide the "All Rights Reserved" message in Command Prompt:

1. Open **Windows Terminal** settings.
2. Navigate to **Settings** > **Command Prompt** > **Command Line**.
3. Append `/k` to the command line string: `%windir%\system32\cmd.exe /k`.

This will configure CMD to start without displaying the copyright message.

## Method 2: Modifying the Windows Registry

You can also remove the copyright notice in `cmd.exe` by modifying the registry. However, be cautious, as this change might impact batch scripts.

1. **Open Registry Editor**  
   - Press `Win + R`, type `regedit`, and press Enter.

2. **Go to Command Processor Keys**  
   - Navigate to:  
     ```
     HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor
     ```  
     and  
     ```
     HKEY_CURRENT_USER\Software\Microsoft\Command Processor
     ```

3. **Set AutoRun to Clear the Screen**  
   - Find or create an `AutoRun` entry as `REG_SZ` or `REG_EXPAND_SZ`.
   - Set its value to `cls`, which will clear the screen each time `cmd.exe` launches, effectively hiding the copyright message but leaving a blank line.

4. **Revert if Necessary**  
   - If any issues arise, delete the `AutoRun` entry to restore the default settings.

