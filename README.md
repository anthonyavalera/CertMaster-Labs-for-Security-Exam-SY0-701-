# APPLIED LAB: Performing Digital Forensics

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

4.8 Explain appropriate incident response activities.
4.9 Given a scenario, use data sources to support an investigation.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- https://dftt.sourceforge.net/
- Terminal Emulator
- Sleuth Kit (TSK)

## Steps

### Forensic evaluation of a drive image

An investigation by the IRT (incident response team) of a recent intrusion has stalled because of the lack of evidence. As a security professional, you would like to perform your own evaluation of the known targetted system. At this point, the original system has already been reconstituted. However, a raw image of that system's drive is available for your analysis. In this exercise, you will attempt to find additional evidence of the intrusion by inspecting this drive image.

  - The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser.

1. From your local computer, view the Extended Partition Test page from http://dftt.sourceforge.net/.

  Expand this hint for guidance.
    a. On your local computer, open another tab in your current browser or open a new browser.

    b. In your local browser's address bar on the new tab, enter http://dftt.sourceforge.net/.

      - You can highlight and cut-n-paste this URL from the instructions into the address bar of your local browser.

    c. Select Extended Partition Test.

    d. Look over the contents of the Extended DOS Partition Test page. You may need to return to this page later in this exercise.

    e. Leave the local browser tab open that is focused on dftt.sourceforge.net.

  - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

  - The forensic test file needed for this lab has been made available to your lab environment via the ISO media of Student-Resources-L25.ISO. You DO NOT need to download the forensic test file to your local system.

  - The site dftt.sourceforge.net is the Digital Forensics Tool Testing image repository. This site contains 14 forensic images which can be used to test forensic analysis tools. These images are also useful in practicing and developing skills in using forensic tools and techniques before working on actual crime scene collected evidence. This is just one of many similar repositories of forensic image testing files.

2. Switch back to the browser tab focused on the Security+ Skillable virtual lab environment.

3. Connect to the KALI virtual machine and, if needed, sign in as root using Pa$$w0rd as the password.

4. Mount the Student-Resources-L25.ISO media and copy its contents to /root/Downloads.

  Expand this hint for guidance.
    a. An icon of a DVD labeled as "Student-Resources-L25.ISO" should be on the Kali desktop, but it will be greyed out. Right-click on this DVD icon and select Mount Volume.

    - If the DVD icon is not on the Desktop: Select the Resources tab from the lab interface's Instructions area. On the Resources tab, select the DVD Drive pull-down list and select Student-Resources-L25.ISO. Then, select the Instructions tab to return to the lab steps.

    b. Open a Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar.

      The Terminal window should already be elevated to use root privileges.

    c. Maximize the Terminal window.

    d. Enter the following command to view the contents of the DVD Drive:

      ls /media/cdrom0/
    e. Enter the following command to copy the forensic test image files to the /root/Downloads directory:

      cp /media/cdrom0/* /root/Downloads/
      - This command will copy all of the files from the DVD/ISO for this and other exercises in this lab.

5. Unzip the 1-extend-part.zip file in the /root/Downloads directory, change into the resulting directory, then view a long list of its contents.

  Expand this hint for guidance.
    a. Enter cd /root/Downloads.

    b. Enter ls -l to view the contents of the directory.

    c. Enter unzip 1-extend-part.zip to extract the contents of the zip archive into its default subfolders.

    d. Enter cd 1-extend-part.

    e. Enter ls -l to view the contents of the directory.

  The ext-part-test-2.dd is the forensic test image file that you will be evaluating in this exercise.

  Now that you have access to the drive image from the compromised system, you start your analysis.

6. Use fdisk to display the partition details of the drive image.

  Expand this hint for guidance.
    a. Enter the following to display the partition details of the drive image.

      fdisk -l ext-part-test-2.dd
      In the presentation of partition details from fdisk, notice that the 4th line is listed with a Type of Extended. Therefore, this is not a formattable volume, but it is the 4th primary partition which has been converted into an extended partition and then further subdivided.

b. Notice the size of the extended partition. It is 155232 sectors. The next two items in the fdisk list are logical drives/partitions of size 52353 and 50337 sectors.

    - MBR-based drives are limited to 4 primary partitions. However, the 4th primary partition can be converted into an extended partition. This conversion allows the 4th partition to be subdivided into numerous additional formattable volumes called logical drives (which are often called partitions). This extended partition concept enables drives to support more than four (4) formatted volumes for use by an operating system.

7. You decide to use a different drive image analysis tool to see if you can discover more information. Use testdisk to display the partition details of the drive image.

  Expand this hint for guidance.
    a. Enter the following to display the partition details of the drive image.

      testdisk -l ext-part-test-2.dd
      Notice that this tool shows a total of 7 numbered entries. One more than what fdisk was able to discover.

  - If the output of testdisk is not well organzied by columns, enter reset, then run the testdiskcommand again.

8. Use fiwalk to analyze the drive image.

  Expand this hint for guidance.
    a. Enter the following to view the output of this drive image analysis tool through the less viewer.

      fiwalk ext-part-test-2.dd | less
    - When using the less file viewing utility, press the spacebar to view the next page. You can return to a previous page using b or scroll one line up or down utilizing the arrow keys. When you are finished looking over the results, type q to exit the less viewer.

  - This forensic test image file is crafted with a text file in each partition named after the partition. You can view these filenames for each partition by scrolling through the output and watching for the change in the partition number.

9. Use fsstat to extract more information about the partitions, especially the hidden partition you have discovered. You will also need to use mmls (a TSK tool) to determine the offset of the hidden partition.

  Expand this hint for guidance.
    a. Enter the following:
      fsstat ext-part-test-2.dd
      You should see an error stating that the file system type was not determined. You remember that the fiwalk command displayed the file system type as "fat16".

    b. Enter the following:
      fsstat -f fat16 ext-part-test-2.dd
      An error is displayed, indicating that the magic value is invalid. This indicates that the initial partition table is corrupted and cannot be automatically interpreted by fsstat.

    c. Enter the following to display the layout details of the partitions.
       mmls ext-part-test-2.dd
      The Start column displays the sector offset for each drive division. Also, notice that the hidden extended volume is line 14 in this tool's output display.

    d. Using the sector offset for the 6th partition, enter:
      fsstat -f fat16 ext-part-test-2.dd -o 262143
      This tool displays more information than what you had access to previously, but nothing is very interesting at this point.

  - The fsstat (file system statistics) utility is one of the many tools from The Sleuth Kit (TSK). It can be used to display details of the file system(s) on a drive image. The -f parameter sets the file system type. There are several options (display a list by entering fsstat -f list), including auto-detection options for fat and ext systems. If you don't know the file system, you may need to experiment.

10. Use the TSK tool fls to pull file information from the hidden partition.

  Expand this hint for guidance.
    a. Use another tool from the TSK to pull file information, enter:
      fls -f fat16 ext-part-test-2.dd -o 262143
  - You should see a file named second-3.txt and other items preceded by a dollar sign. These $named items are system-hidden volume management components.

11. Use another TSK tool, istat, to pull inode information from the hidden partition.

  Expand this hint for guidance.
    a. Enter the following:
      istat -f fat16 ext-part-test-2.dd -o 262143 1
      - Notice there is an additional digit at the end of this command, which is the reference for the inode for istat to retrieve.

      You should see an error claiming the Metadata address is too small for image.

    b. Try the next inode by entering:
      istat -f fat16 ext-part-test-2.dd -o 262143 2
      This reveals information about the root directory.

    c. Try the next inode by entering:
      istat -f fat16 ext-part-test-2.dd -o 262143 3
      This inode is for the file in the root of the drive named second-3.txt.

    d. Try the next inode by entering:
      istat -f fat16 ext-part-test-2.dd -o 262143 4
      This inode is for another file in the root of the drive named SECOND-3.txt. But notice this file has timestamps while the other inode entries do not. This could have been a file present on the drive prior to it being converted into a hidden partition, and thus it retained its original timestamps. However, this file was deleted or otherwise removed from this partition and was somehow corrupted, and is unable to be restored.

    e. Try the next inode by entering:
      istat -f fat16 ext-part-test-2.dd -o 262143 5
      This result of an invalid metadata address indicates that you have viewed all of the inode details available.

  - An inode is a file system metadata structure that is used to store and organize file object information, such as file size, owner user, group IDs, permissions, and timestamps.

  - In this forensic image test file of a drive image, this file is a confirmation that you are viewing the contents of the hidden partition, which is the 3rd logical drive in the extended partition. With 3 primaries, this hidden partition is the 6th formattable volume from this drive image.

  - This is a very small drive with only enough content to prove the concept of a hidden partition. A real drive from a production system, even a hidden partition created by an attacker, would likely contain a significant number of inodes.

12. Mount the hidden partition to /mnt/p6 and view the contents.

  Expand this hint for guidance.
    a. You want to mount the hidden partition to perform more evaluation of the contents of that volume. You need to create a virtual device from the drive image file. Enter the following:
      losetup --partscan --find --show ext-part-test-2.dd
      The output of this command should be /dev/loop0. This indicates the virtual device has been created.

    b. Enter mkdir /mnt/p6 to create a directory in /mnt to mount the hidden volume to.

    c. Enter mount /dev/loop0p7 /mnt/p6 to mount the hidden partition.

      - The use of /dev/loop0p7 is to reference the 7th partition table item, which is the 6th partition. Remember that the 4th partition table item is the extended partition, which was originally the 4th primary partition. Its position must still be taken into account when performing mounting.

    d. Enter cd /mnt/p6 to change into the mounted volume.

    e. Enter ls -l to view the contents of the mounted volume.

  You should see the file second-3.txt but not SECOND-3.txt. The SECOND-3.txt file may have been deleted and overwritten, as it is not recoverable from this forensic image test file.

13. Leave the Terminal window open.

From this point, in a real-world investigation, you would likely find interesting files and metadata on the hidden partition that you have recovered access to. Unfortunately, this forensic image test file does not contain anything other than a single filename to confirm the discovery of and recovery of access to the 3rd logical volume from the extended partition.

#### Check your work

Confirm that you analyzed a system's drive image and discovered a hidden partition.

### Recover deleted files

Another investigation by the IRT of a different security incident has also resulted in a lack of evidence. In this instance, there is a drive image of the storage device where it is believed evidence was destroyed by the perpetrator. As a security professional, you would like to perform your own evaluation of the drive image. In this exercise, you will attempt to recover evidence from the drive image.

  - The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser.

1. From your local computer, view the NTFS Undelete (and leap year) Test #1 page from http://dftt.sourceforge.net/.

  Expand this hint for guidance.
    a. On your local computer, open another tab in your current browser or open a new browser.

    b. In your local browser's address bar on the new tab, enter http://dftt.sourceforge.net/.

      - You can highlight and cut-n-paste this URL from the instructions into the address bar of your local browser.

    c. Select NTFS Undelete (and leap year) Test #1.

    d. Look over the contents of the NTFS Undelete (and leap year) Test #1 page. You may need to return to this page later in this exercise.

    e. Leave the local browser tab open that is focused on dftt.sourceforge.net.

  - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

  - The forensic test file needed for this lab has been made available to your lab environment via the ISO media of Student-Resources-L25.ISO. You DO NOT need to download the forensic test file to your local system.

2. Switch back to the browser tab focused on the Security+ Skillable virtual lab environment.

3. Connect to the KALI virtual machine and, if needed, sign in as root using Pa$$w0rd as the password.

4. Unzip the 7-undel-ntfs.zip file in the /root/Downloads directory, change into the resulting directory, then view a list of its contents.

  Expand this hint for guidance.
    a. The Terminal window should still be open.

    b. Enter cd /root/Downloads.

    c. Enter ls -l to view the contents of the directory.

    d. Enter unzip 7-undel-ntfs.zip to extract the contents of the zip archive into its default subfolders.

    e. Enter cd 7-undel-ntfs.

    f. Enter ls -l to view the contents of the directory.

The 7-ntfs-undel.dd is the forensic test image file that you will be evaluating in this exercise.

- The filename of the zip is 7-undel-ntfs.zip, the name of the extract directory is 7-undel-ntfs, but the name of the drive image file is 7-ntfs-undel.dd.

Now that you have access to the drive image from the suspect's system, you start your analysis.

5. Mount 7-ntfs-undel.dd to /mnt/temp7, then view the contents of the image.

  Expand this hint for guidance.
    a. Enter mkdir /mnt/temp7 to create a mount point.

    b. Enter mount 7-ntfs-undel.dd /mnt/temp7 to mount the image to the mount point.

    c. Enter ls -l /mnt/temp7 to view the contents of the drive image.

  The results will show a System Volume Information directory but nothing else. This causes you to think that the suspect may have deleted files that you may be able to recover.

6. Use tsk_recover (a TSK tool) to attempt to automatically recover the deleted files from this drive image into a folder named output. Then, view a list of the recovered files.

  Expand this hint for guidance.
    a. Enter tsk_recover 7-ntfs-undel.dd output to attempt to automatically recover the deleted files from this drive image into a folder named output.

      You should see the statement: Files Recovered: 8.

    b. Enter ls -l output to view the recovered file information.

7. Discover the recovered filenames that are not located in the root of the output recovery directory.

  Expand this hint for guidance.
    a. Enter ls -l output/dir1 to view the contents of the recovered dir1 directory.

    b. Enter ls -l output/dir1/dir2 to view the contents of the recovered dir1/dir2 directory.

  - The undeleted files are now visible and accessible for further investigation. However, the files from this forensic test disk image do not have any visible or useful content to explore.

8. Leave the Terminal window open.

From this point, in a real-world investigation, you would analyze the timestamps and contents of the recovered files. This may lead to other information which could direct the investigation further.

#### Check your work

Confirm that you recovered deleted files from a drive image.

### File carving

The IRT informs you that a second drive's image is available from the suspect's system, where data destruction is suspected. As a security professional, you are eager to attempt data recovery on this drive image. In this exercise, you will work through the tedious process of gaining access to a damaged drive image in hopes of recovering files through file carving. File carving is the forensic process of recovering access to files that are otherwise inaccessible due to corruption, partial data loss (especially headers), deletion, or partition structure damage.

- The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser.

1. From your local computer, view the Basic Data Carving Test #1 page from http://dftt.sourceforge.net/.

  Expand this hint for guidance.
    a. On your local computer, open another tab in your current browser or open a new browser.

    b. In your local browser's address bar on the new tab, enter http://dftt.sourceforge.net/.

      - You can highlight and cut-n-paste this URL from the instructions into the address bar of your local browser.

    c. Select Basic Data Carving Test #1.

    d. Look over the contents of the Basic Data Carving Test #1 page. You may need to return to this page later in this exercise.

    e. Leave the local browser tab open that is focused on dftt.sourceforge.net.

  - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

  - The forensic test file needed for this lab has been made available to your lab environment via the ISO media of Student-Resources-L25.ISO. You DO NOT need to download the forensic test file to your local system.

2. Switch back to the browser tab focused on the Security+ Skillable virtual lab environment.

3. Connect to the KALI virtual machine and, if needed, sign in as root using Pa$$w0rd as the password.

4. Unzip the 11-carve-fat.zip file in the /root/Downloads directory, change into the resulting directory, then view a list of its contents.

  Expand this hint for guidance.
    a. The Terminal window should still be open.

    b. Enter cd /root/Downloads.

    c. Enter ls -l to view the contents of the directory.

    d. Enter unzip 11-carve-fat.zip to extract the contents of the zip archive into its default subfolders.

    e. Enter cd 11-carve-fat.

    f. Enter ls -l to view the contents of the directory.

    The 11-carve-fat.dd is the forensic test image file that you will be evaluating in this exercise.

    Now that you have access to the drive image from the suspect's system, you start your analysis.

5. Mount 11-carve-fat.dd to /mnt/temp11.

  Expand this hint for guidance.
    a. Enter mkdir /mnt/temp11 to create a mount point.

    b. Enter mount 11-carve-fat.dd /mnt/temp11 to mount the image to the mount point.

  This should result in a mounting error. This indicates that something is corrupted in the image and cannot be mounted for direct file system analysis.

6. Use fdisk to display the partition details of the drive image.

  Expand this hint for guidance.
    a. Enter fdisk -l 11-carve-fat.dd to display the partition details of the drive image.
  This should result in a partial presentation of information. Notice that the Disk identifier: value is all zeros, and there is no partition table displayed. Something is wrong with this drive image.

7. Use fiwalk to perform a drive image analysis.

  Expand this hint for guidance.
    a. Enter fiwalk 11-carve-fat.dd to perform a drive image analysis.
  Notice the results include numerous errors, most of which state Possible encryption detected.

8. Use fsstat to display file system statistics of the drive image. You may need to use mmls to determine the partition(s) offset(s).

  Expand this hint for guidance.
    a. Enter fsstat 11-carve-fat.dd to display file system statistics of the drive image.

      This tool should also show an error of Possible encryption detected. However, previously, you needed to define the file system type for this tool to work. Try guessing the file system type. Start with fat16.

    b. Enter fsstat -f fat16 11-carve-fat.dd to display file system statistics of the drive image.

      The error presented now is that there is an Invalid magic value. This indicates that the initial partition table is corrupted and cannot be automatically interpreted by fsstat.

    c. Enter mmls 11-carve-fat.dd to display the layout details of the partitions.

      This tool will provide no results. Therefore, you cannot determine the partition offset values to use with fsstat, fls, or istat.

  You will be unable to obtain information about the drive image with fsstat.

  - As demonstrated in this exercise, it is often necessary to try numerous tools and techniques to recover evidence and access files that have been corrupted, deleted, or otherwise purposefully destroyed. Many forensic tools use different data recovery techniques, so don't give up until you have exhausted all of your options.

9. Use testdisk to use file carving to recover files from the drive image. Store the recovered files in the output sub-folder (which you need to create first).

  Expand this hint for guidance.
    a. Enter mkdir output to create an output folder.

    b. Enter testdisk 11-carve-fat.dd to attempt to open the drive image in testdisk's interactive mode.

      The TestDisk tool should open and present the 11-carve-fat.dd file for processing.

    c. Notice that at the bottom of the interface the [Proceed ] option is highlighted. Press Enter on your keyboard to select this option.

    d. Use your keyboard's down arrow key to select [None    ] as the partition table type, then press Enter on your keyboard.

    e. This will result in a display of an Unknown partition, which is already highlighted. Press Enter on your keyboard.

    f. Use your keyboard's down arrow key to select FAT16 as the partition type, then press Enter on your keyboard.

      - If you don't know the partition type, you may need to guess until to find a working option.

    g. You are returned to the previous screen, which now has the partition labeled as "FAT16". Use your keyboard's right arrow key to highlight [Undelete] at the bottom of the screen, then press Enter on your keyboard.

      The resulting page will have a message of: No file found, filesystems may be damaged.

    h. Type q to exit the Undelete function and return to the previous screen.

    i. Use your keyboard's left arrow key to highlight [  Boot  ] at the bottom of the screen, then press Enter on your keyboard.

    j. The [Rebuild BS] at the bottom of the screen is already highlighted. Press Enter on your keyboard.

      This will attempt to rebuild the boot sector. This could be the cause of the read and access problem of this drive image.

    k. The rebuild function will provide a result that includes the statement "Extrapolated boot sector and current boot sector are different.". The [  List  ] option at the bottom of the screen is already highlighted. Press Enter on your keyboard.

      Notice that a list of files is presented. The rebuild boot sector operation was able to restore access to the drive image. However, this repair is only in memory and has not changed the original .dd file. You need to extract all recoverable files from the in-memory repaired image.

    i. Type a to select all files, then type C to copy the selected files.

      - Be sure to type a capital C. Otherwise, the lowercase version will only copy a single file.

    m. A output directory selection screen is shown. Use your keyboard's down arrow key to highlight the output directory, press Enter on your keyboard to enter the output directory, then type C to copy the files to the current directory.

      This operation should result in a success message above the file list of "Copy done! 15 OK, 0 failed".

    n. Type CTRL+C to break/exit TestDisk.

10. View the contents of the recovered files.

  Expand this hint for guidance.
    a. Enter cd output, then ls -l to view the contents of the recovered files.

      - If the directory listing does not display properly, enter reset, then try the command again.

      You should see the 15 recovered files.

    b. Enter xdg-open haxor2.jpg to open this graphics file to view the picture.

    c. Use your keyboard's up and down arrows to view the other graphics from this directory. When you are finished viewing these pictures, type CTRL+C to exit the graphics viewer.

    d. Enter xdg-open lin_1.2.pdf to open this PDF document to view its contents.

    e. When you are finished looking at this PDF, type CTRL+C to exit.

      - You can open several of these files using xdg-open, including surf.mov. You can unzip wword60t.zip to view its contents.

You have successfully performed file carving to extract the files that were destroyed by the perpetrator. These evidence files should be shared with the IRT and other interested groups so they can continue to perform their investigations.

#### Check your work

Confirm that you recovered files through file carving.
