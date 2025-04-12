# Aliases in Windows Command Prompt

Creating persistent aliases in Windows Command Prompt requires using [DosKey](https://web.archive.org/web/20140330024520/http://en.wikipedia.org/wiki/DOSKey) commands for temporary aliases or making a registry modification for persistent aliases.

## Temporary Aliases with `doskey`

1. **Open Command Prompt.**

2. **Create a Temporary Alias** using the following syntax:
   
   ```batch
   doskey alias_name=command
   ```

3. **Example**:
   
   ```batch
   doskey ll=dir
   ```
   
   To show directory contents in wide format:
   
   ```batch
   doskey ll=dir /w
   ```

> *Note*: Temporary aliases only last for the current Command Prompt session and will reset when the session is closed.

## Persistent Aliases with `doskey`

To make aliases persistent across sessions, create a `.bat` or `.cmd` file with your aliases and configure the registry to load it at startup.

1. **Create a Batch File**  
   Save your aliases in a batch file (e.g., `aliases.bat`):
   
   ```bat
   doskey ls=dir
   doskey gp=git push
   ```

2. **Open Registry Editor**  
   Press `Win + R`, type `regedit`, and press Enter.

3. **Navigate to Command Processor Key**  
   Go to:
   
   ```
   HKEY_CURRENT_USER\Software\Microsoft\Command Processor
   ```

4. **Set Up AutoRun**  
   Right-click in the right pane, select `New` > `String Value`, and name it `AutoRun`.
   Double-click `AutoRun` and enter the full path to your batch file, e.g., `C:\Users\YourName\aliases.bat`.

5. **Restart Command Prompt**  
   Close and reopen Command Prompt. Your aliases will now load automatically each time Command Prompt starts.

> Note:
> 
> - When creating an alias to navigate to a directory, you must include `*/d*`.
> - Use the forward slash (`/`) as the path separator, not the backslash (`\`).
> - Ensure there is no space around the `=` sign when defining an alias.

---

**Example templete to get started** 

```batch
@echo off

REM GENERAL ALIASES
doskey ll=dir /w
REM Lists files in a compact, wide format


REM Changes directory to the user's home folder

doskey cat=bat $*
REM Uses `bat` (enhanced `cat`) for viewing file contents

REM GIT ALIASES
doskey g=git
REM Short alias for `git`

doskey gs=git status
REM Shows current git status

doskey gcl=git clone
REM Clones a git repository

doskey gpu=git push
REM Pushes commits to a remote repository

doskey gpl=git pull
REM Pulls changes from a remote repository

doskey gf=git fetch
REM Fetches updates from a remote repository

doskey ga=git add
REM Stages files for commit

doskey gc=git commit
REM Commits changes to the repository

doskey gst=git stash
REM Temporarily stores changes not yet committed

doskey grst=git reset
REM Resets changes in the current branch

doskey gb=git branch
REM Lists, creates, or deletes branches

doskey gco=git checkout
REM Switches to another branch

doskey gcp=git cherry-pick
REM Applies changes from a specific commit

doskey gm=git merge
REM Merges branches together

doskey grb=git rebase
REM Re-applies commits on top of another branch

doskey gt=git tag
REM Creates or lists tags in the repository

doskey gl=git log
REM Shows commit history

doskey gr=git remote
REM Manages remote repository connections

doskey gbs=git bisect
REM Uses binary search to find a problematic commit

REM ADVANCED GIT ALIASES
doskey gss=git status --short
REM Shows a compact status of the repository

doskey gba=git branch --all
REM Lists all branches, local and remote

doskey gplum=git pull upstream master
REM Pulls from the `upstream` master branch

doskey grbc=git rebase --continue
REM Continues a rebase after conflict resolution

doskey gcob=git checkout -b
REM Creates and switches to a new branch
```

To avoid the comment symbols (`::`) being mistakenly interpreted as part of the commands, place each comment on its own line with `REM` (or `::` if desired) preceding it.

---

Multiple `.bat` Files from the `.bat` File Location

To run multiple `.bat` files from a single batch file, follow these steps:

1. **Create a `.bat` file to execute multiple scripts**:
   
   - Open Notepad (or your preferred text editor) and create a new batch file, e.g., `init.bat`.
   
   - In the batch file, add the paths to the other `.bat` files that you want to run. If the `.bat` files are in the same directory as the main batch file, you can use the following commands:
     
     ```batch
     @echo off
     call "file1.bat"
     call "file2.bat"
     call "file3.bat"
     ```
   
   - The `call` command ensures that the script will wait for one batch file to finish before moving on to the next.

2. **Set up the AutoRun key for Command Processor** (optional):
   
   - Open the **Registry Editor** (`regedit`).
   - Navigate to `HKEY_CURRENT_USER\Software\Microsoft\Command Processor`.
   - Add a new `String Value` under `AutoRun` with the path to the `.bat` file you want to run automatically when CMD starts, or update the key to include the batch file execution script.

3. **Ensure correct path references**:
   
   - Ensure that each `.bat` file is correctly referenced with either an absolute path or relative path (relative paths work if the `.bat` files are in the same folder).

This setup will allow you to run multiple batch files in sequence from one main batch file.

---

## Multiple commands under the single AutoRun

To register multiple commands under the single `AutoRun` key in the registry, you can concatenate the commands within the same entry by separating each command with the `&` symbol. This lets you execute multiple commands in sequence, even though the `AutoRun` key only accepts one value. Here’s how to do it:

1. **Modify the AutoRun Entry**:
   
   - Find the `AutoRun` key. If it doesn’t exist, you can create it by right-clicking and selecting **New > String Value** and naming it `AutoRun`.
   
   - Edit the `AutoRun` value, and use the `&` symbol to concatenate commands. For example:
     
     ```plaintext
     command1 & command2 & command3
     ```
     
     Replace `command1`, `command2`, and `command3` with the actual commands or scripts you want to execute.

2. **Save and Exit**:
   
   - Click **OK** to save the changes, and close the Registry Editor.

Now, every time a Command Prompt session starts, it will execute all specified commands in the order they appear.
