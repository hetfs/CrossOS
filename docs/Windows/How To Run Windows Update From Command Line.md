---
runme:
  id: 01HM7NNDENZYZK6FFTKK4CPHSJ
  version: v2.2
---

# How To Run Windows Update From Command Line

## 🗒️ Answer
You can run Windows Update from the command line using various methods:

1. **Using UsoClient:** Open an elevated Command Prompt and type `UsoClient StartScan` to initiate Windows Update[[2](https://answers.microsoft.com/en-us/windows/forum/all/start-windows-update-windows-10-from-command-line/c4bec909-05ac-4b24-a0ab-7a83124923a8)].

2. **Using Windows Update Client Commands:** Enter `wuauclt.exe /updatenow` in a Command Prompt to force Windows Update to check for updates and start downloading[[4](https://www.itechtics.com/run-windows-update-cmd/)][[5](https://www.diskpart.com/tips-tricks/windows-update-command-line-1503.html)].

3. **PowerShell Module:** Install the Windows Update Module by running `Install-Module PSWindowsUpdate` in PowerShell. After installation, use `Get-WindowsUpdate` and `Install-WindowsUpdate` cmdlets to check and install updates[[6](https://www.majorgeeks.com/content/page/how_to_run_windows_updates_from_command_line_in_windows_10.html)].

4. **Settings App:** Open Settings with `Win + I`, navigate to Update & Security, and click Check for updates to manually run Windows Update[[3](https://www.minitool.com/backup-tips/windows-update-command-line-021.html)].

## 🌐 Sources
1. [EaseUS - 2 Ways to Force Update Windows 10 to the Latest Build](https://www.easeus.com/computer-instruction/force-update-windows-10.html)
2. [Microsoft Community - Start Windows Update (Windows 10) from Command Line](https://answers.microsoft.com/en-us/windows/forum/all/start-windows-update-windows-10-from-command-line/c4bec909-05ac-4b24-a0ab-7a83124923a8)
3. [MiniTool - Two Efficient Ways to Do Windows Update from Command](https://www.minitool.com/backup-tips/windows-update-command-line-021.html)
4. [iTechtics - How To Run Windows Update From Command Line (CMD)](https://www.itechtics.com/run-windows-update-cmd/)
5. [DiskPart - How to (Force) Run Windows Update From Command Line](https://www.diskpart.com/tips-tricks/windows-update-command-line-1503.html)
6. [MajorGeeks - How to Run Windows Updates from Command Line in Windows 10](https://www.majorgeeks.com/content/page/how_to_run_windows_updates_from_command_line_in_windows_10.html)