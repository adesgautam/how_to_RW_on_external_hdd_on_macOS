# how_to_RW_on_external_hdd_on_macOS
This is a "how-to" for using ntfs formatted external HDD on MacOS using free tools available.

----------------------------------------------------------------------------------------------------------------------------

MacOS does not provide write permission on NTFS format as default. So to mount external drives as RW follow the given steps below:

Step 1: Download FUSE for macOS from [here](https://sourceforge.net/projects/osxfuse/files/latest/download)

Step 2: Then we have to install xcode command line tools. For this open terminal and write

        `xcode-select --install`

        Then click *install* when dialogue appears. Click *Agree* when the License Agreement appears. Once thats done proceed to step 3.
        
Step 3: Now we have to install Homebrew which is a package manager for macOS. Copy and paste the following in the terminal.

        `ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`.
        
Step 4: Paste the following in terminal to install ntfs-3g for mac: 

`brew install ntfs-3g`.

Step 5: Now connect your external HDD system and follow the below steps:

(a) `diskutil list`

This will list all the connected drives to the laptop. There will be the name assigned of the disks something like: disk2s1.
            
Note the name of the External HDD drive. If there are 2 partitions then there will be two names like: disk2s1 and disk2s2.
            
b) `sudo mkdir /Volumes/NTFS1`

We have to make a folder for giving a mount point to the disks. This command will make that folder for mounting. If you are trying to mount 2 partitions of your HDD then make another folder using: 

`sudo mkdir /Volumes/NTFS2`

The number of folders will depend on the number of drives you want to mount.
            
(c) `sudo umount /dev/disk2s1`

This command will unmount the drives already mounted by the macOS system as we will again mount them by ntfs-3g in RW mode.
Again if there are 2 partitions mounted then use: 

`sudo umount /dev/disk2s2`

'disk2s1` and `disk2s2` are the names of the drives.
            
(d) `sudo /usr/local/bin/ntfs-3g /dev/disk2s1 /Volumes/NTFS1 -olocal -oallow_other`

This will now unmount the drive named as `disk2s1` to the pre created folder `NTFS1` in RW mode.

Congratulations you are done !

So lets say you have 2 partitions on your HDD formatted as NTFS, then you will be using the following commands in sequence on terminal.
        
1. `diskutil list`
2. `sudo mkdir /Volumes/NTFS1`
3. `sudo mkdir /Volumes/NTFS2`
4. `sudo umount /dev/disk2s1`
4. `sudo umount /dev/disk2s2`
5. `sudo /usr/local/bin/ntfs-3g /dev/disk2s1 /Volumes/NTFS1 -olocal -oallow_other`
6. `sudo /usr/local/bin/ntfs-3g /dev/disk2s2 /Volumes/NTFS2 -olocal -oallow_other`
        
After this the two drives will be mounted in RW mode on your system.

This is a manual process which needs to be done every time when you will connect your external HDD to the system.

# Auto mount the NTFS volumes in RW mode

*This is not recommended as this will involve the replacement of the Apple's NTFS mount tool `/sbin/mount_ntfs`. This will present a security risk for the system.*

For replacing the ntfs-3g tool with the Apple's NTFS mount tool execute the following commands in the terminal. The following commands needs to be executed in the *recovery mode*. To go into the *recovery mode* press *command+R* when the system boots. When in recovery mode go to Utilities and then click on Terminal and enter the following:

`sudo mv "/Volumes/Macintosh HD/sbin/mount_ntfs" "/Volumes/Macintosh HD/sbin/mount_ntfs.orig"`

`sudo ln -s /usr/local/sbin/mount_ntfs "/Volumes/Macintosh HD/sbin/mount_ntfs"`

# Uninstallation 
To uninstall ntfs-3g execute following in terminal:

`brew uninstall ntfs-3g`

And to restore the Apple's NTFS mount tool again go to *recovery* and enter following in the terminal:

`sudo mv "/Volumes/Macintosh HD/sbin/mount_ntfs.orig" "/Volumes/Macintosh HD/sbin/mount_ntfs"`


----------------------------------------------------------------------------------------------------------------------------

This was quite a long process but it involves free tools,but if you want to use a straightforward method then use the Paragon NTFS software which costs $19.95.

## The list of commands is provided in the repository.
