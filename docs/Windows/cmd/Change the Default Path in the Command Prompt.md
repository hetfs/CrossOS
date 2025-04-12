# How to Change the Default Path in Command Prompt

To set a default directory for Command Prompt, use one of the following methods:

## Manual Methods

### 1. Create a Custom Shortcut

- Search for `cmd.exe` in the Start Menu.
- Right-click `cmd.exe` and select "Open File Location."
- Right-click the shortcut, select "Properties," and go to the "Shortcut" tab.
- In the "Start in" field, enter your desired default directory path (e.g., `C:\Your\Path\Here`) and apply the changes.
- Opening Command Prompt from this shortcut will now start in the specified directory.

### 2. Modify the Windows Registry (Advanced)

- Open Registry Editor by pressing `Win + R`, typing `regedit`, and pressing Enter.
- Navigate to `HKEY_CURRENT_USER\Software\Microsoft\Command Processor`.
- Right-click to create a new `String Value` named `AutoRun`.
- Set the value to `cd /d C:\Your\Path\Here` to set the default path.
- Close and reopen Command Prompt to apply the change.

Here's an improved version of your content:

---

## Command Line Method

The command below modifies the Windows Registry to set a default startup directory for Command Prompt:

```shell
reg add "HKEY_CURRENT_USER\Software\Microsoft\Command Processor" /v AutoRun /t REG_SZ /d "IF x%COMSPEC%==x%CMDCMDLINE% (cd /D c:\)"
```

### Explanation

1. **`reg add`**: This command adds or modifies a Registry entry.

2. **`"HKEY_CURRENT_USER\Software\Microsoft\Command Processor"`**: Specifies the registry key location where settings for Command Prompt are stored.

3. **`/v AutoRun`**: Defines the value name "AutoRun," which allows setting a command that runs each time Command Prompt is opened.

4. **`/t REG_SZ`**: Sets the data type to `REG_SZ`, which is a string value.

5. **`/d "IF x%COMSPEC%==x%CMDCMDLINE% (cd /D c:\)"`**:
   
   - This condition checks if `%COMSPEC%` (the environment variable for Command Prompt’s path) matches `%CMDCMDLINE%` (the current Command Prompt command line). 
   - If true, it executes `cd /D c:\`, changing the default directory to `C:\` upon launching Command Prompt.
   
   ---

**Go to User Home Directory**

```batch
reg add "HKEY_CURRENT_USER\Software\Microsoft\Command Processor" /v AutoRun /t REG_SZ /d "if x%COMSPEC%==x%CMDCMDLINE% (cd /d %USERPROFILE%)"
```

**Description**: This command sets the Command Prompt to automatically navigate to the user’s home directory (`%USERPROFILE%`) every time it opens. By adding this entry to the Windows Registry, the Command Prompt is configured to check if the command shell (`%COMSPEC%`) matches the current command line (`%CMDCMDLINE%`). If it does, it changes the directory to the user’s home folder.

---

**Update HOMEDRIVE and HOMEPATH Variables and Go to Home Folder**

```batch
reg add "HKEY_CURRENT_USER\Software\Microsoft\Command Processor" /v AutoRun /t REG_SZ /d "if x%COMSPEC%==x%CMDCMDLINE% (set HOMEDRIVE=%USERPROFILE:~,2% & set HOMEPATH=%USERPROFILE:~2% & cd /d %USERPROFILE%)"
```

**Description**: This command not only navigates to the user’s home directory (`%USERPROFILE%`) but also updates the `HOMEDRIVE` and `HOMEPATH` environment variables. This ensures that these variables reflect the user’s home directory path, which can be useful for scripts or applications that rely on these specific environment variables.

---

**Note**: Modifying system shortcuts can sometimes break existing functionality. For instance, editing the Command Prompt shortcut affected the `Win + X` > `C` key shortcut. (Lesson learned: only modify system files if you’re comfortable fixing them!) Fortunately, a Windows update eventually restored the original settings.

As a workaround, I created a custom Command Prompt shortcut, added it to the Start folder, and pinned it to the taskbar. This way, I can launch my customized Command Prompt without relying on the default `cmd.exe`.

Alternatively, consider using [Windows Terminal](https://learn.microsoft.com/en-us/windows/terminal/) and [configure a starting directory](https://learn.microsoft.com/en-us/windows/terminal/customize-settings/profile-settings#starting-directory) with a specific starting directory configuration. Here’s a sample `settings.json` snippet for Windows Terminal:

```json
{
    // Customize the Command Prompt profile here
    "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
    "name": "Command Prompt",
    "commandline": "cmd.exe",
    "hidden": false,
    "startingDirectory": "C:\\Your\\Path\\Here"
}
```
