# Assisted Lab: Performing Drive Sanitization

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

4.2 Explain the security implications of proper hardware, software, and data asset management.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Terminal Emulator

## Steps

### Prep a portable drive for use

Assume you have just obtained a new external USB storage drive. You want to be able to use the storage device with numerous OSes. You need to create a primary partition using all of the available free space and then format that drive with FAT32. You will then copy some files to the drive.

1. Connect to the KALI and sign in as root using Pa$$w0rd as the password.

2. Open a Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar.

    - It may be helpful to maximize the Terminal window.

    - When the Disconnected pop-up appears stating "The network connection has been disconnected", select Don't show this message again. However, it may re-appear, but not as often. You can select Don't show this message again each time, or you can ignore it.

3. Enter fdisk -l to display the currently present storage devices connected to the Kali VM.

Enter the full disk path name of the storage device with a size of 80 GiB but no existing partition divisions in the text box below:

    - Press Enter on your keyboard after you type in the value or click out of the text box.

4. Enter fdisk <SDB> to initate the tool to create storage partitions.

5. Enter m to display the full help menu of commands for the fdisk utility.

6. Enter p to print (i.e., display) the current partition table.

    - Notice there is no mention of a partition table in the results. This indicates that there are no partitions on this drive…yet. However, the lack of confirmation is unsettling.

7. Enter v to verify the partition table.

    - Notice the results indicate that there are no errors and that the same number of sectors listed by the p command is shown by the v command to be unallocated.

8. Enter n to create a new partition.

9. Enter p in response to the Partition type query.

10. Enter 1 in response to the Partition number query.

11. Press Enter in response to the First sector query to accept 2048 (the default).

12. Press Enter in response to the *Last sector query to accept 167772159 (the default (and the last sector)).

    - The result should be a confirmation that a new partition was created.

13. Enter p print (i.e., display) the current partition table.

    - This output should now include the partition you just created.

Enter the full path name of the new partition in the text box below:

    - Press Enter on your keyboard after you type in the value or click out of the text box.

14. Enter w to write the partition table changes to the storage device and exit fdisk.

15. Enter mkfs -t fat <SDB1> to format the new partition with the FAT32 file format.

    - FAT32 is used in this exercise to simulate a USB drive that will be used to transfer files between various OSes.

16. Enter mkdir /mnt/SalesStorage to create a mounting point folder for the new storage device.

17. Enter mount <SDB1> /mnt/SalesStorage to mount the newly formatted partition to the mount point location.

18. Enter ls -l /mnt/SalesStorage to view the empty mapped storage location.

19. To copy some files to the new storage location, enter the following command:

    - cp -r /usr/share/seclists/* /mnt/SalesStorage/
    - This should take less than 30 seconds to copy.

20. Enter ls -l /mnt/SalesStorage to view the contents copied to the new storage location.

21. Leave the Terminal window open.

#### Check your work

Confirm that you created and formatted a partition on a storage device.

Confirm that you mounted a formatted partition.

Confirm that you copied files to the mounted storage location.

### Delete and undelete a file objects

In this exercise, you will work through the process of deleting a directory and all of its contained files, then attempt to restore or undelete those files.

1. Connect to the KALI and, if needed, sign in as root using Pa$$w0rd as the password.

2. Return to the Terminal window.

3. Enter rm -r /mnt/SalesStorage/Miscellaneous to delete this directory and its contents.

4. Enter ls -l /mnt/SalesStorage to view the contents of the storage location.

    - The Miscellaneous folder should no longer be present.

    - By removing (i.e., deleting) files and folders from a Terminal window, the file objects are not captured in the Trash utility to be recovered easily.

5. Enter testdisk /dev/sdb1 to attempt to undelete the deleted files and folder.

    - The TestDisk tool should open and present /dev/sdb1 for processing.

6. Notice that at the bottom of the interface, the [Proceed ] option is highlighted. Press Enter on your keyboard to select this option.

7. Use your keyboard's down arrow key to select [None    ] as the partition table type, then press Enter on your keyboard.

    - Since you are working against a partition, there are no further sub-partitions.

8. This will result in a display of a FAT32 partition, which will be highlighted. Use your keyboard's right arrow key to highlight [Undelete] at the bottom of the screen, then press Enter on your keyboard.

9. Use your keyboard's down arrow key to highlight Miscellaneous.

10. Type : to select the current highlighted file object.

11. Type capital C to copy the selected file object(s).

12. A listing of the contents of the root account's home folder will be displayed. You need to select an output destination. Use your keyboard's down arrow key to highlight Downloads, press Enter to open the Downloads directory, then type a capital C on your keyboard to set the destination directory.

13. After a few moments, there should be a message of  Copy done!  above the file listing.

14. Press CTRL+C to exit testdisk and return to the Terminal window prompt.

15. Enter ls -l Downloads/Miscellaneous

    - If the directory listing does not display properly, enter reset, then try the command again.

    - Deletion and formatting are not specifically data destruction operations. A deletion marks a file's storage locations (typically clusters, a.k.a. allocation unit, storage allocation unit, or storage node, on an HDD or a block on an SSD) as available for re-use but does not actually remove the original data. Formatting does this across an entire partition. As long as the data clusters/blocks are not overwritten, the data is typically recoverable. To truly remove data, it needs to be overwritten by something else.

16. Leave the Terminal window open.

#### Check your work

Confirm that you deleted a directory and files.

Confirm that you recovered or undeleted a directory and files

### Securely delete files using shred

Now that you know deleted files may be recoverable, you want to test whether a secure deletion will prevent file recovery.

1. Connect to the KALI and, if needed, sign in as root using Pa$$w0rd as the password.

2. Return to the Terminal window.

3. Enter ls -l /mnt/SalesStorage to view the contents of the storage device.

4. Enter ls -lR /mnt/SalesStorage/Passwords to view the recursive contents of the Passwords directory on the storage device.

5. Enter cat /mnt/SalesStorage/Passwords/bt4-password.txt to view the contents of one of the files from within the Passwords directory.

6. Use the following command to use find and shred to securely delete all files within the Passwords directory on the SalesStorage storage location using a zeroization process:

    - find /mnt/SalesStorage/Passwords -type f -exec shred -uvz {} \;
This secure deletion and zeroization process is a bit involved, so it will take up to a minute to complete. You will see the verbose progress output as the operations are performed.

    - The shred utility only accepts individual files as targets. This command combines the find tool's function to locate and identify files as a means to send filenames as input to the shred utility.

7. Enter ls -lR /mnt/SalesStorage/Passwords to view the recursive contents of the Passwords directory on the storage device.

    - Notice the Passwords directory is still present, but all contained files within all sub-directories are no longer present.

8. Enter rm -r /mnt/SalesStorage/Passwords to delete the empty directories.

9. Enter testdisk /dev/sdb1 to attempt to undelete the deleted files and folder.

    - The TestDisk tool should open and present /dev/sdb1 for processing.

10. Notice that at the bottom of the interface, the [Proceed ] option is highlighted. Press Enter on your keyboard to select this option.

11. Use your keyboard's down arrow key to select [None    ] as the partition table type, then press Enter on your keyboard.

    - Since you are working against a partition, there are no further sub-partitions.

12. This will result in a display of a FAT32 partition, which will be highlighted. Use your keyboard's right arrow key to highlight [Undelete] at the bottom of the screen, then press Enter on your keyboard.

13. Use your keyboard's down arrow key to highlight Passwords, then press Enter.

14. The contents of the directory structure still retain the filenames from the Passwords directory.

15. Press your keyboard’s left arrow to return to the previous list of directories.

16. Use your keyboard's down arrow key to highlight Passwords, then press Enter.

17. Type : to select the current highlighted file object.

18. Type capital C to copy the selected file object(s).

19. A listing of the contents of the root account's home folder will be displayed. You need to select an output destination. Use your keyboard's down arrow key to highlight Downloads, press Enter to open the Downloads directory, then type a capital C on your keyboard to set the destination directory.

20. After a few moments, there should be a message of  Copy done!  above the file listing.

21. Press CTRL+C to exit testdisk and return to the Terminal window prompt.

22. Enter ls -l Downloads/Passwords

    - If the directory listing does not display properly, enter reset, then try the command again.

    - Initially, it looks like all of the files were restored. However…

23. Enter cat Downloads/Passwords/bt4-password.txt.

    - Notice the file has no contents. You can attempt to view the contents of any of the supposedly recovered files, but their contents have been shredded (i.e., zeroized).

    - This technique does protect the data, but the filenames, directory names, and file sizes are still retained. And it seems to only address data that is still retained in a standard file.

24. Leave the Terminal window open.

#### Check your work

Confirm that you confirmed the presence of and contents of a file.

Confirm that you securely shredded files.

Confirm that you attempted to recover shredded files but discovered that all content was missing.

### Format a drive to destroy data

It has often been recommended to format a drive to destroy data. In this exercise, you will format a drive and confirm that files are no longer present.

  - However, the native tools available on Kali are insufficient to retrieve data from a drive that has been formatted. This exercise demonstrates that limitation, but with only a single tool. There may be open-source and commercial tools that can recover data after a drive format.

1. Connect to the KALI and, if needed, sign in as root using Pa$$w0rd as the password.

2. Return to the Terminal window.

3. Enter umount /mnt/SalesStorage

    - The command is umount NOT unmount.

4. Enter mkfs -t fat /dev/sdb1 to format the storage device to attempt to sanitize the data.

5. Enter testdisk /dev/sdb1 to attempt to undelete the deleted files and folder.

    - The TestDisk tool should open and present /dev/sdb1 for processing.

6. Notice that at the bottom of the interface, the [Proceed ] option is highlighted. Press Enter on your keyboard to select this option.

7. Use your keyboard's down arrow key to select [None    ] as the partition table type, then press Enter on your keyboard.

    - Since you are working against a partition, there are no further sub-partitions.

8. This will result in a display of a FAT32 partition, which will be highlighted. Use your keyboard's right arrow key to highlight [Undelete] at the bottom of the screen, then press Enter on your keyboard.

9. This should result in a display claiming No file found. Filesystem may be damaged.

    - This indicates that any remaining data is not recoverable using the testdisk utility.

10. Press CTRL+C to exit testdisk and return to the Terminal window prompt.

    - There are some commercial drive recovery utilities and services which may be able to restore data after reformatting. Therefore, formatting is not considered a secure sanitization or data destruction technique. But, for low classification, sensitivity, or value data, it may be sufficient.

11. Leave the Terminal window open.

#### Check your work

Confirm that you formatted a drive to destroy data.

Confirm that you attempted to recover data on a formatted drive.

### Sanitization of a drive

Drive sanitization is the forceful overwriting of the entire drive with alternate data as a means to destroy data and prevent data remnant recovery. In this exercise, you will use dd (i.e., disk duplicator) to destroy all data on a storage device through overwriting.

1. Connect to the KALI and, if needed, sign in as root using Pa$$w0rd as the password.

2. Return to the Terminal window.

3. Since the data on the drive was formatted in the previous exercise, use the following commands to place data on the drive again:

    mount /dev/sdb1 /mnt/SalesStorage
    cp -r /usr/share/seclists/* /mnt/SalesStorage/

4. Enter the following command to perform a zeroization sanitization of the storage device:

    - dd if=/dev/zero of=/dev/sdb bs=1M status=progress
    - This operation will take almost two minutes to complete. You will be able to view the progress as it operates. Remember, the target drive is 80 GiB in size.
    - An alternative is to use the command: dd if=/dev/random of=/dev/sdb bs=1M status=progress. However, the random overwriting progress takes more time.

5. Enter fdisk /dev/sdb to initiate the tool to manage storage partitions.

6. Enter p to print (i.e., display) the current partition table.

    - Notice there is no mention of a partition table in the results. This indicates that there are no partitions on this drive…anymore.

7. Enter v to verify the partition table.

    - Notice the results indicate that there are no errors and that the same number of sectors listed by the p command is shown by the v command to be unallocated.

8. Enter q to exit fdisk.

9. Enter testdisk /dev/sdb to attempt to access the storage device itself.

    - The TestDisk tool should open and present /dev/sdb for processing.

10. Notice that at the bottom of the interface, the [Proceed ] option is highlighted. Press Enter on your keyboard to select this option.

11. Use your keyboard's down arrow key to select [Intel   ] as the partition table type, then press Enter on your keyboard.

12. Use your keyboard's down arrow key to select [ Analyse  ], then press Enter on your keyboard.

    - The result should indicate that there are no partitions on this storage device.

13. Press CTRL+C to exit testdisk and return to the Terminal window prompt.

#### Check your work

Confirm that you copied data onto a storage device.

Confirm that you destroyed data using dd to overwrite the entire storage device.

Confirm that you attempted to recover data on a sanitized drive.
