`winget`, short for the Windows Package Manager, is a command-line tool for managing software packages on Windows. It allows you to search, install, update, and remove applications directly from the command line, similar to package managers on Linux like `apt` or `yum`.

### Key Features of winget:

1. **Package Management**: Easily install, update, and uninstall software packages using simple commands.

2. **Search**: Find available software packages from the official repository or community repositories.

3. **Automation**: Script software installations and updates, making it useful for setting up new systems or maintaining consistent environments across multiple machines.

4. **Integration**: Works well with other Windows tools and can be integrated into broader automation workflows using PowerShell or batch scripts.

5. **Open Source**: The tool is open source, and the package repository is community-driven, allowing anyone to contribute or request new packages.

### Basic Commands:

1. **Install a Package**:
   
   ```bash
   winget install <package_name>
   ```
   
   Example: Install Visual Studio Code
   
   ```bash
   winget install Microsoft.VisualStudioCode
   ```

2. **Search for Packages**:
   
   ```bash
   winget search <search_term>
   ```
   
   Example: Search for "node.js"
   
   ```bash
   winget search node.js
   ```

3. **List Installed Packages**:
   
   ```bash
   winget list
   ```

4. **Update a Package**:
   
   ```bash
   winget upgrade <package_name>
   ```
   
   Example: Update Git
   
   ```bash
   winget upgrade Git.Git
   ```

5. **Uninstall a Package**:
   
   ```bash
   winget uninstall <package_name>
   ```
   
   Example: Uninstall VLC Media Player
   
   ```bash
   winget uninstall VideoLAN.VLC
   ```

6. **View Package Info**:
   
   ```bash
   winget show <package_name>
   ```
   
   Example: Show details of Google Chrome
   
   ```bash
   winget show Google.Chrome
   ```

7. Manually update the source use
   
   ```bash
   winget source update
   ```

### Getting Started:

- **Installation**: If you're using Windows 10 version 1809 (build 17763) or later, winget is included by default. Otherwise, you can install it from the [Microsoft Store](https://apps.microsoft.com/store/detail/app-installer/9NBLGGH4NNS1).

- **Use Case**: winget is particularly useful for developers, IT professionals, and anyone who wants to streamline the process of installing and managing software on Windows.

### Example:

To install Git, you would run:

```bash
winget install Git.Git
```

This command fetches the latest version of Git from the repository and installs it on your system.

### Install WinGet on Windows Sandbox

Windows Sandbox offers a secure, isolated environment to run applications safely without affecting the host system. Applications installed within the Sandbox remain contained and do not interact with the main system. However, it does not come with WinGet or the Microsoft Store app by default, so you need to manually download and install WinGet from GitHub using PowerShell.

```PowerShell
$progressPreference = 'silentlyContinue'
Write-Information "Downloading WinGet and its dependencies..."
Invoke-WebRequest -Uri https://aka.ms/getwinget -OutFile Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle
Invoke-WebRequest -Uri https://aka.ms/Microsoft.VCLibs.x64.14.00.Desktop.appx -OutFile Microsoft.VCLibs.x64.14.00.Desktop.appx
Invoke-WebRequest -Uri https://github.com/microsoft/microsoft-ui-xaml/releases/download/v2.8.6/Microsoft.UI.Xaml.2.8.x64.appx -OutFile Microsoft.UI.Xaml.2.8.x64.appx
Add-AppxPackage Microsoft.VCLibs.x64.14.00.Desktop.appx
Add-AppxPackage Microsoft.UI.Xaml.2.8.x64.appx
Add-AppxPackage Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle
```

## WinGet CLI Settings

You can configure WinGet by editing the `settings.json` file. Running `winget settings` will open this file in your default JSON editor. If no editor is set, Windows will prompt you to choose one, with Notepad being a suitable default option.

[winget-cli doc Settings.md](https://github.com/microsoft/winget-cli/blob/master/doc/Settings.md#file-location)

## File Location

Settings file is located in %LOCALAPPDATA%\Packages\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe\LocalState\settings.json

If you are using the non-packaged WinGet version by building it from source code, the file will be located under %LOCALAPPDATA%\Microsoft\WinGet\Settings\settings.json

**Example of `winget` configuration**

🙏 Don't  just copy and paste !!!!!!!

```json
{
  "$id": "https://aka.ms/winget-settings.schema.json",
  "$schema": "https://json-schema.org/draft/2019-09/schema#",
  "title": "Microsoft's Windows Package Manager Settings Profile Schema",
  "definitions": {
    "Source": {
      "description": "Source repository settings",
      "type": "object",
      "properties": {
        "autoUpdateIntervalInMinutes": {
          "description": "Number of minutes before source update",
          "type": "integer",
          "default": 5,
          "minimum": 0,
          "maximum": 43200
        }
      }
    },
    "Visual": {
      "description": "Visual settings",
      "type": "object",
      "properties": {
        "progressBar": {
          "description": "Progress bar display style",
          "type": "string",
          "enum": [
            "accent",
            "rainbow",
            "retro"
          ]
        },
        "anonymizeDisplayedPaths": {
          "description": "Replaces some known folder paths with their respective environment variable",
          "type": "boolean",
          "default": true
        }
      }
    },
    "Logging": {
      "description": "Logging settings",
      "type": "object",
      "properties": {
        "level": {
          "description": "Preferred logging level",
          "type": "string",
          "enum": [
            "verbose",
            "info",
            "warning",
            "error",
            "critical"
          ]
        },
        "channels": {
          "description": "The logging channels to enable",
          "type": "array",
          "items": {
            "uniqueItems": true,
            "type": "string",
            "enum": [
              "fail",
              "cli",
              "sql",
              "repo",
              "yaml",
              "core",
              "test",
              "config",
              "default",
              "all"
            ],
            "maxLength": 20
          },
          "minItems": 0,
          "maxItems": 20
        }
      }
    },
    "InstallPrefReq": {
      "description": "Shared schema for preferences and requirements",
      "type": "object",
      "properties": {
        "scope": {
          "description": "The scope of a package install",
          "type": "string",
          "enum": [
            "user",
            "machine"
          ],
          "default": "user"
        },
        "locale": {
          "description": "The locales of a package install",
          "type": "array",
          "items": {
            "type": "string",
            "pattern": "^([a-zA-Z]{2}|[iI]-[a-zA-Z]+|[xX]-[a-zA-Z]{1,8})(-[a-zA-Z]{1,8})*$",
            "maxLength": 20
          },
          "minItems": 1,
          "maxItems": 10
        },
        "architectures": {
          "description": "The architecture(s) to use for a package install",
          "type": "array",
          "items": {
            "uniqueItems": true,
            "type": "string",
            "enum": [
              "neutral",
              "x64",
              "x86",
              "arm64",
              "arm"
            ],
            "minItems": 1,
            "maxItems": 4
          }
        },
        "installerTypes": {
          "description": "The installerType(s) to use for a package install",
          "type": "array",
          "items": {
            "uniqueItems": true,
            "type": "string",
            "enum": [
              "inno",
              "wix",
              "msi",
              "nullsoft",
              "zip",
              "msix",
              "exe",
              "burn",
              "msstore",
              "portable"
            ],
            "minItems": 1,
            "maxItems": 9
          }
        }
      }
    },
    "InstallBehavior": {
      "description": "Install settings",
      "type": "object",
      "properties": {
        "preferences": { "$ref": "#/definitions/InstallPrefReq" },
        "requirements": { "$ref": "#/definitions/InstallPrefReq" },
        "skipDependencies": {
          "description": "Controls whether package dependencies and Windows Features are skipped during installation",
          "type": "boolean",
          "default": false
        },
        "disableInstallNotes": {
          "description": "Controls whether installation notes are shown after a successful install",
          "type": "boolean",
          "default": false
        },
        "portablePackageUserRoot": {
          "description": "The default root directory where packages are installed to under User scope. Applies to the portable installer type.",
          "type": "string",
          "default": "%LOCALAPPDATA%/Microsoft/WinGet/Packages/"
        },
        "portablePackageMachineRoot": {
          "description": "The default root directory where packages are installed to under Machine scope. Applies to the portable installer type.",
          "type": "string",
          "default": "%PROGRAMFILES%/WinGet/Packages/"
        },
        "defaultInstallRoot": {
          "description": "Default install location to use for packages that require it when not specified",
          "type": "string",
          "maxLength": 32767
        },
        "maxResumes": {
          "description": "The maximum number of resumes allowed for a single resume id. This is to prevent continuous reboots if an install that requires a reboot is not properly detected.",
          "type": "integer",
          "default": 3,
          "minimum": 1
        }
      }
    },
    "UninstallBehavior": {
      "description": "Uninstall settings",
      "type": "object",
      "properties": {
        "purgePortablePackage": {
          "description": "Controls whether the default behavior for uninstall removes all files and directories relevant to this package. Only applies to the portable installerType.",
          "type": "boolean",
          "default": false
        }
      }
    },
    "DownloadBehavior": {
      "description": "Download settings",
      "type": "object",
      "properties": {
        "defaultDownloadDirectory": {
          "description": "The default directory where installers are downloaded to.",
          "type": "string",
          "default": "%USERPROFILE%/Downloads/"
        }
      }
    },
    "Telemetry": {
      "description": "Telemetry settings",
      "type": "object",
      "properties": {
        "disable": {
          "description": "Controls whether telemetry events are written",
          "type": "boolean",
          "default": false
        }
      }
    },
    "Network": {
      "description": "Network settings",
      "type": "object",
      "properties": {
        "downloader": {
          "description": "Control which download code is used for packages",
          "type": "string",
          "enum": [
            "default",
            "wininet",
            "do"
          ],
          "default": "default"
        },
        "doProgressTimeoutInSeconds": {
          "description": "Number of seconds to wait without progress before fallback",
          "type": "integer",
          "default": 60,
          "minimum": 1,
          "maximum": 600
        }
      }
    },
    "Interactivity": {
      "description": "Interactivity settings",
      "type": "object",
      "properties": {
        "disable": {
          "description": "Controls whether interactive prompts are shown by the Windows Package Manager client",
          "type": "boolean",
          "default": false
        }
      }
    },
    "Experimental": {
      "description": "Experimental Features",
      "type": "object",
      "properties": {
        "experimentalCMD": {
          "description": "Reference implementation for an experimental command",
          "type": "boolean",
          "default": false
        },
        "experimentalARG": {
          "description": "Reference implementation for an experimental argument",
          "type": "boolean",
          "default": false
        },
        "directMSI": {
          "description": "Enable use of MSI APIs rather than msiexec for MSI installs",
          "type": "boolean",
          "default": false
        },
        "configuration03": {
          "description": "Enable support for configuration schema 0.3",
          "type": "boolean",
          "default": false
        },
        "configureSelfElevate": {
          "description": "Enable configure commands request elevation as needed",
          "type": "boolean",
          "default": false
        },
        "configureExport": {
          "description": "Enable support for the configure export command",
          "type": "boolean",
          "default": false
        }
      }
    }
  },
  "allOf": [
    {
      "properties": {
        "visual": { "$ref": "#/definitions/Visual" }
      },
      "additionalItems": true
    },
    {
      "properties": {
        "logging": { "$ref": "#/definitions/Logging" }
      },
      "additionalItems": true
    },
    {
      "properties": {
        "source": { "$ref": "#/definitions/Source" }
      },
      "additionalItems": true
    },
    {
      "properties": {
        "installBehavior": { "$ref": "#/definitions/InstallBehavior" }
      },
      "additionalItems": true
    },
    {
      "properties": {
        "telemetry": { "$ref": "#/definitions/Telemetry" }
      },
      "additionalItems": true
    },
    {
      "properties": {
        "network": { "$ref": "#/definitions/Network" }
      },
      "additionalItems": true
    },
    {
      "properties": {
        "interactivity": { "$ref": "#/definitions/Interactivity" }
      },
      "additionalItems": true
    },
    {
      "properties": {
        "experimentalFeatures": { "$ref": "#/definitions/Experimental" }
      },
      "additionalItems": true
    }
  ],
  "additionalProperties": true
}
Memo
Highlight
```

## 🌐 Sources

1. [learn.microsoft.com - Windows Sandbox Overview](https://learn.microsoft.com/en-us/windows/security/application-security/application-isolation/windows-sandbox/windows-sandbox-overview)

2. [learn.microsoft.com - Use the WinGet tool to install and manage applications](https://learn.microsoft.com/en-us/windows/package-manager/winget/)

3. https://github.com/microsoft/winget-cli/releases

4. https://github.com/microsoft/winget-cli

5. [Introduction - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/explore-windows-package-manager-tool/1-introduction)
