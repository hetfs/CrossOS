
# Re-enabling PSReadLine in PowerShell

If you see a warning about `PSReadLine` being disabled in your PowerShell session, it could be because **Windows is in screen-reader mode** (an accessibility feature) when your session starts. Here’s how to understand and resolve this issue.

## Problem Context:
- **Using a Standard PowerShell Session**: If you're running a standard PowerShell session in a console window, Windows Terminal, or Visual Studio Code's integrated terminal, you might encounter this warning.
- **Using PowerShell in Visual Studio Code**: When using the [PowerShell extension in Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell), the warning typically doesn’t appear. This is because sessions within the **PowerShell Integrated Console** (provided by the extension) do not perform the screen-reader check (as of version `2021.2.2`), so `PSReadLine` loads without issue. 

## Solution Steps

1. **Manually Re-enable `PSReadLine`**:
   - You can re-enable `PSReadLine` by running `Import-Module PSReadLine` in your session or by adding it to your `$PROFILE` file.
   - However, note that **you’ll still see the warning at startup** because it appears before `$PROFILE` is loaded.

2. **Disabling Screen-Reader Mode**:
   If the screen-reader mode was enabled accidentally or by an application, you can turn it off through the registry.

   - **Registry Method**:
     Run the following command in PowerShell to turn off screen-reader mode persistently:
     ```powershell
     Set-ItemProperty 'registry::HKEY_CURRENT_USER\Control Panel\Accessibility\Blind Access' On 0
     ```
   - **Note**: This change requires a **logoff or reboot** to take effect.

   - **To check if screen-reader mode is enabled**:
     ```powershell
     Get-ItemPropertyValue 'registry::HKEY_CURRENT_USER\Control Panel\Accessibility\Blind Access' On
     ```

3. **Ad-hoc Solution Within an Active Session**:
   If screen-reader mode was turned on within the current session (by an app that crashed or didn’t turn it off), you can disable it immediately without a reboot:

   - Use the following PowerShell command to disable screen-reader mode for future PowerShell sessions within the same OS user session. This uses `Add-Type` to include C# code that can make P/Invoke calls:
     ```powershell
     # Run in PowerShell
     (Add-Type -PassThru -Name ScreenReaderUtil -Namespace WinApiHelper -MemberDefinition @'
       const int SPIF_SENDCHANGE = 0x0002;
       const int SPI_SETSCREENREADER = 0x0047;
       [DllImport("user32", SetLastError = true, CharSet = CharSet.Unicode)]
       private static extern bool SystemParametersInfo(uint uiAction, uint uiParam, IntPtr pvParam, uint fWinIni);
       public static void EnableScreenReader(bool enable) 
       {
         var ok = SystemParametersInfo(SPI_SETSCREENREADER, enable ? 1u : 0u, IntPtr.Zero, SPIF_SENDCHANGE);
         if (!ok) 
         {
           throw new System.ComponentModel.Win32Exception(Marshal.GetLastWin32Error());
         }
       }
     '@)::EnableScreenReader($false)
     ```

### Alternative Manual Method (Registry Editor)

If you prefer to disable screen-reader mode manually:

1. Press **WIN + R** to open "Run" and type `regedit`.
2. Navigate to:
   ```
   HKEY_CURRENT_USER\Control Panel\Accessibility\Blind Access
   ```
3. Find the `On` option, right-click, select **Modify**, and set the value to `0`.
4. Restart your computer for the change to take effect.

---

### Common Warning Message in PowerShell:
When screen-reader mode is enabled, you might see the following warning:
> Warning: PowerShell detected that you might be using a screen reader and has disabled `PSReadLine` for compatibility purposes. If you want to re-enable it, run `Import-Module PSReadLine`.

Disabling screen-reader mode using the above methods should resolve this issue and re-enable `PSReadLine` for a rich command-line experience in PowerShell.
