# How to Create Aliases on Windows

Setting up command aliases in Windows can streamline your workflow, allowing you to use shortcuts for frequently-used commands. Here’s how to create aliases in both Command Prompt and PowerShell.

### 1. **Creating Aliases in PowerShell**

PowerShell’s `Set-Alias` cmdlet allows you to create quick command aliases.

```powershell
Set-Alias alias_name command_to_run
```

**Example**: To create an alias `ll` for listing directory contents (`Get-ChildItem`), run:

```powershell
Set-Alias ll Get-ChildItem
```

This alias will only work for the current session.

To make it permanent, add the `Set-Alias` command to your PowerShell profile file (`$PROFILE`):

```powershell
echo 'Set-Alias ll Get-ChildItem' >> $PROFILE
```

Reload your profile to apply changes:

```powershell
. $PROFILE
```

---

## PowerShell Aliases and Functions

PowerShell’s `Set-Alias` cmdlet works well for simple commands, but for more complex commands or commands that require arguments, use functions instead. Here’s how to set up general aliases and Git shortcuts with functions.

### General Aliases

```powershell
# Simple Aliases
Set-Alias ll "dir"                    # Lists files in a compact format

# Custom Functions
function ~ { Set-Location -Path $HOME }   # Shortcut to the user's home directory
function cat { bat $args }               # Uses `bat` (an enhanced `cat`) for viewing file contents
```

### Git Aliases

Use aliases and functions to streamline Git commands, making version control more efficient.

```powershell
# Basic Git Aliases
Set-Alias g "git"                           # Short alias for `git`
function gs { git status }                  # Displays the current Git status
function gcl { git clone $args }            # Clones a Git repository
function gpu { git push }                   # Pushes commits to a remote repository
function gpl { git pull }                   # Pulls changes from a remote repository
function gf { git fetch }                   # Fetches updates from a remote repository
function ga { git add $args }               # Stages files for commit
function gc { git commit }                  # Commits changes to the repository
function gst { git stash }                  # Stashes uncommitted changes
function grst { git reset }                 # Resets changes in the current branch
function gb { git branch }                  # Lists, creates, or deletes branches
function gco { git checkout $args }         # Switches branches
function gcp { git cherry-pick $args }      # Applies changes from a specific commit
function gm { git merge $args }             # Merges branches
function grb { git rebase $args }           # Rebases commits onto another branch
function gt { git tag }                     # Manages tags in the repository
function gl { git log }                     # Shows commit history
function gr { git remote }                  # Manages remote repositories
function gbs { git bisect }                 # Binary search for problematic commits
```

### Advanced Git Aliases

```powershell
function gss { git status --short }         # Shows a compact Git status
function gba { git branch --all }           # Lists all branches, local and remote
function gplum { git pull upstream master } # Pulls updates from the upstream master branch
function grbc { git rebase --continue }     # Continues a rebase after resolving conflicts
function gcob { git checkout -b $args }     # Creates and checks out a new branch
```

### Important Notes:

- **`Set-Alias`** is used for straightforward command mappings.
- **Functions** are recommended for commands needing arguments or custom behavior.

### Adding Aliases and Functions to Your PowerShell Profile

To load aliases and functions automatically in every PowerShell session:

1. **Open PowerShell**: For system-wide aliases, launch PowerShell as an administrator.
2. **Access Your Profile**: Run the command below to open your profile file in Notepad (or your editor of choice):
   
   ```powershell
   notepad $PROFILE
   ```
   
   If the profile doesn’t exist, PowerShell will prompt you to create it. Select “Yes” to confirm.
3. **Add Aliases and Functions**: Paste the alias and function definitions into the profile file.
4. **Save and Close** the profile file.
5. **Reload the Profile**: To activate your changes immediately, run:
   
   ```powershell
   . $PROFILE
   ```

Now, every time you start PowerShell, your aliases and functions will be available.

For additional information:

- [Set-Alias (Microsoft.PowerShell.Utility) - PowerShell | Microsoft Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/set-alias?view=powershell-7.4)
- [Microsoft.PowerShell.Utility Module - PowerShell | Microsoft Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/?view=powershell-7.4)


