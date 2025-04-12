# Step-by-Step Guide How to set up multi-boot with rEFInd

rEFInd is a boot manager for UEFI computer that will allow you to choose between Windows, Linux and Mac OS X, and other operating systems when you boot your computer, it can auto-detect your installed operating systems and presents a pretty GUI menu these operating systems. rEFInd is one of the most popular multi-boot managers on the market.
The EFI System Partition (also called ESP) is a FAT32 formatted physical partition from where the UEFI firmware launches the UEFI bootloader and application. It is a OS independent partition that acts as the storage place for the EFI bootloaders and applications which the firmware launches them. It is mandatory for UEFI boot. [Download](https://www.rodsbooks.com/refind) latest binary file:rEFInd URL

*MacOS, Windows, and Ubuntu Linux Triple Boot on Mac*.

1. **Prepare Your Mac:**
   - Back up important data before proceeding with the installation.
2. **Partition Your Drive:**
   - Use Disk Utility on macOS to partition your drive for Windows and Linux installations. Create separate partitions for each OS.
3. **Install Windows:**
   - Use Boot Camp Assistant on macOS to install Windows 10. Follow the on-screen instructions to create a Windows partition and install Windows.
4. **Install Ubuntu:**
   - Boot into Ubuntu live USB and install Ubuntu on the partition you created earlier. Follow the Ubuntu installation wizard to set up Ubuntu alongside Windows.
5. **Configure Boot Loader:**
   - Install rEFInd boot manager to manage boot options for macOS, Windows, and Ubuntu.
   - Follow the installation instructions for rEFInd and ensure it detects all installed operating systems.
6. **Triple Boot Selection:**
   - After installing rEFInd, reboot your Mac and use the rEFInd boot menu to select between macOS, Windows, and Ubuntu Linux.
7. **Enjoy Triple Booting:**
   - You can now enjoy triple booting on your MacBook Pro, switching between macOS, Windows, and Ubuntu Linux as needed [[Youtube](https://www.youtube.com/watch?v=G53TBTKEmiA)].

## How to Install rEFInd Boot Loader on Mac for Running Alternative Operating Systems

1. Download the rEFInd boot manager from the official website or GitHub repository. This tool allows you to boot into various operating systems on your Mac [[rEFInd Boot Manager](https://www.rodsbooks.com/refind/installing.html)].
2. Extract the downloaded files and open a terminal window.
3. Navigate to the directory where you extracted the rEFInd files using the "cd" command.
4. Unzip the downloaded binary and Copy that folder to a flash drive.
5. In Recovery Mode (hold Command +R during boot) perform those commands in the Terminal.
   *Run the installation script using the following command*:
   - Type: `cd /`
   - Type: `ls`
   - Type: `cd Volumes/`[name of your flash drive]
   - Type `./refind-install` to execute an install script and Press "Y" in the box there.
   - Remove flash drive

Follow the on-screen prompts during the installation process. You may need to enter your password.
Once the installation is complete, reboot your Mac.
During startup, hold down the Option key on your keyboard to access the boot menu.
Select the rEFInd boot option to boot into the rEFInd boot manager.
From the rEFInd boot menu, you can choose to boot into macOS, Linux, Windows, or any other operating systems installed on your Mac.

*To mount a EFI partition and edit a boot configuration*:

1. Open Terminal
2. Type `mkdir mnt`
3. Type `sudo mount -t msdos /dev/disk0s1 mnt`
4. Type `cd ~/mnt/EFI/refind`
5. Type `gedit refind.conf` (use your favorite Text Editor in this case)
6. Find `scanfor` line to configure a boot sequence

> Before installing rEFInd boot manager, you need to disable the System Integrity Protection (SIP) mode, otherwise rEFInd won't install properly. SIP was added on all MacOS versions starting from El Capitan (10.11) and rEFInd will give you a fat warning if you try to install it without disabling the SIP first. To disable SIP, boot into recovery and in the terminal execute: csrutil disable

## Installing rEFInd on Windows for Running Alternative Operating Systems

*To install rEFInd from Windows, follow these steps:*

1. **Prepare Your System:**
   
   - Ensure your Windows system is fully updated and running properly.

2. **Download rEFInd:**
   
   - Visit the rEFInd official website or GitHub repository to [download]((https://www.rodsbooks.com/refind/installing.html)) the latest version of rEFInd.

3. **Prepare EFI System Partition (ESP):**
   
   - Access the EFI partition where your boot files are stored. This partition is usually hidden.
   - Backup the existing EFI\Boot folder in case you need to revert changes later.

4. **Create Directories:**
   
   - Open Command Prompt as an administrator.
   - Create a suitable directory for rEFInd using the following command:
   
   ```batch
   mkdir X:\EFI\refind
   ```
   
     Replace "X" with the appropriate drive letter where your EFI partition is located.

5. **Copy rEFInd Files:**
   
   - Extract the downloaded rEFInd files.
   - Copy the extracted files to the newly created directory (X:\EFI\refind).

6. **Configure rEFInd:**
   
   - Optionally, modify the refind.conf file to customize rEFInd settings.

7. **Reboot:**
   
   - Restart your system. rEFInd should now appear as a boot option during startup, allowing you to choose between operating systems.

## How to Prepare the EFI System Partition (ESP)

To prepare the EFI System Partition (ESP), access the EFI partition, and back up the existing EFI\Boot folder, follow these steps:

1. **Accessing the EFI Partition**:
   
   - Open an Administrator Command Prompt window by right-clicking the Command Prompt icon and selecting "Run as Administrator".
   - Use Diskpart to identify and select the EFI partition:
   
   ```batch
      diskpart
      list disk
      select disk X  (Replace X with the disk number where the EFI partition resides)
      list partition
      select partition Y  (Replace Y with the partition number of the EFI partition)
   ```

2. **Preparing the EFI System Partition (ESP)**:
   
   - Once the EFI partition is selected, you can perform various operations, such as copying files, creating directories, or modifying existing files.

3. **Backing Up the Existing EFI\Boot Folder:**
   
   - Copy the contents of the EFI\Boot folder to a safe location for backup. You can use the following command to copy the folder and its contents to a backup directory:
   
   ```batch
   robocopy /MIR X:\EFI\Boot C:\Backup\EFI\Boot
   ```
   
     Replace "X" with the drive letter of the EFI partition and "C:\Backup" with the path where you want to store the backup.

4. **Reverting Changes:**
   
   - If you need to revert changes later, simply copy the backed-up EFI\Boot folder back to the EFI partition using the same method.

> By following these steps, you can prepare the EFI System Partition, access the EFI partition to manage boot files, and create a backup of the existing EFI\Boot folder for future use.

1. [Superuser - How to access EFI partition on Windows](https://superuser.com/questions/965751/how-to-access-efi-partition-on-windows-10)
2. [Seven Forums - Move boot files from EFI partition to system partition?](https://www.sevenforums.com/general-discussion/372155-move-boot-files-efi-partition-system-partition.html)
3. [rodsbooks.com - The rEFInd Boot Manager: Installing and Uninstalling rEFInd](https://www.rodsbooks.com/refind/installing.html)
4. [schdck.github.io - Installing rEFInd from Windows](https://schdck.github.io/Installing-refind-from-Windows-10)