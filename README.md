# APPLIED LAB: Implement Backups

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

3.4 Explain the importance of resilience and recovery in security architecture.

### Tools Used

- Command Prompt
- Windows Server Backup
- Disk Management
- Volume Shadow Copy Service (VSS)

## Steps

### Prepare for backup operations

This exercise prepares a second storage device to be used as the backup target and preps several files for backup and restore operations.

1. Connect to the PC10 virtual machine, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

2. Create a full drive volume on Disk 1 named Backup01 with a drive letter F using diskpart. This is the volume that you will use as backup media.

    - Disks and partitions (and volumes) are numbered cardinally, which means the first item is numbered 0 (zero), and the second item is numbered 1 (one). Therefore, disk 0 is the main OS boot drive, while disk 1 is the additional drive that is currently unused.

  Expand this hint for guidance.
    a. Enter cmd into the Windows search bar, and then, on the menu, select Command Prompt.

    b. To open the diskpart utility in interactive mode, run the command: diskpart

    c. Select Yes on the User Account Control window.

    d. To place the focus of the diskpart tool on Disk 1, run the command: select disk 1

    e. To set the disk to online (i.e., available for use), run the command: online disk

    f. To enable disk writing, run the command: attribute disk clear readonly

    g. To create a full drive partition/volume on Disk 1, run the command: create partition primary

    h. To format the partition with NTFS and assign a label, run the command: format fs=ntfs label=“Backup01” quick

    i. To assign drive letter F: to the new partition, run the command: assign letter=f

    j. To exit diskpart, run the command: exit

3. Create 3 copies of the file C:\SETUP\NetworkMiner\Fingerprints\oui.txt into the C:\Users\Jaime\Documents folder and assign new filenames of document01.txt, document02.txt, and document03.txt.

    - The oui.txt file copies will be used as sample files for the backup and restoration operations.

  Expand this hint for guidance.
    a. Run the following command to copy a file to a new name:
    - copy C:\SETUP\NetworkMiner\Fingerprints\oui.txt C:\Users\Jaime\Documents\document01.txt
    a. Run the following command to copy a file to a new name:
    - copy C:\SETUP\NetworkMiner\Fingerprints\oui.txt C:\Users\Jaime\Documents\document02.txt
    a. Run the following command to copy a file to a new name:
    - copy C:\SETUP\NetworkMiner\Fingerprints\oui.txt C:\Users\Jaime\Documents\document03.txt

4. Create 1 copy of file C:\SETUP\NetworkMiner\Fingerprints\oui.txt into the C:\Users\Public\ folder and assign a new filename of document04.txt.

  Expand this hint for guidance.
    a. Run the following command to copy a file to a new name:
    - copy C:\SETUP\NetworkMiner\Fingerprints\oui.txt C:\Users\Public\document04.txt

5. Close the Command Prompt window.

    - Most backup solutions can protect files of any type. You will not be limited to text documents. Typically, the primary limiting factor for backups is the amount of data to be protected vs. the available space on the backup media (i.e., target or save location).

#### Check your work

Confirm that you created a volume to use as the backup media.

Confirm that you copied several files to use as backup samples.

### Protect files using Windows Server Backup

In this exercise, you will perform a one-time backup of a folder and its contents.

1. Connect to the PC10 virtual machine. If needed, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

2. Using Windows Server Backup, perform a one-time backup of C:\Users to drive F:.

  Expand this hint for guidance.
    a. Enter Backup into the Windows search bar, and then, on the menu, select Windows Server Backup.

    b. Select Local Backup from the left pane.

    c. Wait for the interface to update. It may take up to 10 seconds.

    d. Select Backup Once from the right-pane.

    e. On the Backup Once Wizard window, on the Backup Options page, confirm that Different options is selected, then select Next.

    f. On the Select Backup Configuration page, seelcct Custom, then select Next.

    g. On the Select Items for Backup page, select Add Items.

    h. On the Select Items windows, select the plus sign beside Local disk (C:) to expand its contents.

    i. Select the checkbox for Users.

    j. Select OK.

    k. On the Select Items for Backup page, select Next.

    l. On the Specify Destination Type page, select Local drives, then select Next.

    m. On the Select Backup Destination page, use the Backup destination pull-down list to select Backup01 (F:), then select Next.

    n. On the Confirmation page, select Backup.

    - It may take up to 1 minute for the backup process to complete. Wait for the backup to be completed.

3. Once the backup is completed, select Close to exit the Backup Once Wizard.

You should be returned to the Windows Server Backup window.

4. Minimize the Windows Server Backup window, but leave it open.

    - While a one-time backup can be useful, a periodic scheduled backup is more likely to be used in a real-world scenario. A scheduled backup can be created using the Backup Schedule option. The associated wizard will include pages to set the time/date interval of the backups.

#### Check your work

Confirm that you performed a backup.

### Delete a file and restore it from backup

1. Connect to the PC10 virtual machine. If needed, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

2. document01.txt.

  Expand this hint for guidance.
    a. Enter File Explorer into the Windows search bar, and then, on the menu, select File Explorer.

    b. Double-click Documents.

    c. Right-click document01, then select Delete.

    d. Minimize File Explorer.

    e. Right-click Recycle Bin, which should be in the top-left corner of the Desktop.

    f. Select Empty Recycle Bin.

    g. Select Yes on the Delete File pop-up window asking if you are sure you want to permanently delete this file.

    - By emptying the Recycle Bin you are ensuring the file is fully removed from the system and that only a restoration from backup will cause the file to exist again in the Documents folder.

3. Restore the document01.txt file from backup.

  Expand this hint for guidance.
    a. Return to the Windows Server Backup window by selecting its icon on the Taskbar.

    b. Select Recover from the right-pane.

    c. On the Recovery Wizard, on the Getting Started page, select This server (PC10), then select Next.

    d. On the Select Backup Date page, the backup that you made in the previous exercise (just minutes ago) should already be selected and visible. Select Next.

    e. On the Select Recovery Type page, select Files and folders, then select Next.

    f. On the Select Items to Recover page, select the plus sign to expand PC10, then expand Local disk (C:), then Users, then jaime.

    g. Select Documents under the jamie folder.

    h. Select document01.txt in the Items to recover: area, then select Next.

    i. On the Specify Recovery Options page, select Original location, leave all other options at their defaults, then select Next.

    j. On the Confirmation page, select Recover.

    k. Once the recovery process is complete, select Close.

4. Confirm the document01.txt is restored.

  Expand this hint for guidance.
    a. Return to the File Manager window by selecting its icon from the Taskbar.

    b. The Documents folder should still be the focus of File Manager.

    c. Double-click document01.

    d. Close Notepad.

5. Close all windows.

#### Check your work

Confirm that you deleted a file.

Confirm that you restored a file from a backup.

### Enable Volume Shadow Copy Service

In this exercise, you will use the Volume Shadow Copy Service (VSS) to restore a previous version of a modified file.

1. Connect to the PC10 virtual machine. If needed, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

2. Enable Volume Shadow Copy Service on Drive C:.

  Expand this hint for guidance.
    a. Right-click the Start Menu, then select Disk Management.

    b. Right-click (C:), then select Properties.

    c. On the Local Disk (C:) Properties window, select the Shadow Copies tab.

    d. On the Shadow Copies tab, select C:\, then select Enable.

    e. On the Enable Shadow Copies window, select Yes.

    f. On the Local Disk (C:) Properties window, you should now see a date and time in the Next Run Time column in the top area. A shadow copy should be made immediately upon enablement. If so, the current time and date will be shown in the bottom Shadow copies of selected volume area.

    g. Select OK to close the Local Disk (C:) Properties window.

    h. Close Disk Management.

3. Force the Volume Shadow Copy Service (VSS) to make a backup of drive C:.

  Expand this hint for guidance.
    a. Enter cmd into the Windows search bar, and then, on the menu, select Run as administrator under Command Prompt.

    b. Select Yes on the User Account Control window.

    c. Run the following command to force VSS to perform a backup:

    - wmic shadowcopy call create Volume=c:\
    a. This command should result in several lines of operation. The second line should state Method execution successful.

    b. Close the Command Prompt.

    - Normally the VSS system will retain a copy of a file just as a change is performed. However, we have not found this process as reliable as claimed, so forcing a shadow operation ensures that the exercise functions as expected.

4. Alter the document04 file from the C:\Users\Public\ folder to add a new first line of changed.

  Expand this hint for guidance.
    a. Enter file explorer into the Windows search bar, and then, on the menu, select File Explorer under Command Prompt.

    b. Select This PC.

    c. Double-click Local Disk (C:).

    d. Double-click Users, then double-click Public.

    e. Double-click the file document04.

    f. Notepad should open, displaying the contents of the document04 file.

    g. The cursor should already be positioned at the start of the first line. If not, move the cursor to the start of the first line using the keyboard arrow keys or by clicking the mouse just before the first character.

    h. Enter changed. Be sure to press Enter on your keyboard to ensure this word is on the first line by itself.

    i. Close Notepad.

    j. Select Save on the Notepad pop-up window asking do you want to save changes.

5. Restore the previous version of document04.

  Expand this hint for guidance.
    a. Right-click document04, then select Restore previous versions.

    b. The document04 Properties window is displayed, with the Previous Versions tab selected.

    c. There should be at least one entry listed for document04 in the File versions: area. Select the top (i.e., most recent) entry.

    d. Select Restore

    e. Select Restore on the Previous Versions pop-up window that asks are you sure you want to restore the previous version.

    f. Select OK on the Previous Versions pop-up window that states the file has been successfully restored to the previous version.

    g. Select OK to close the document04 Properties window.

    - Volume Shadow Copy Service (VSS) will retain previous versions of files, but it is not a backup. If a file is removed from a drive, the VSS cannot be used to restore it. However, Windows File History and potentially a system restore point can be used to restore lost files.

6. Confirm that the previous unedited version of document04 is now restored to the system.

  Expand this hint for guidance.
    a. The File Explorer window should still be open.

    b. Double-click document04

    c. Notepad should open, displaying the contents of the document04 file.

    d. Notice that the change you made previously by adding a new first line containing the word changed is no longer present. This confirms the previous version of this file was restored via VSS.

    e. Close Notepad.

7. Leave File Explorer open.

#### Check your work

Confirm that you enabled the Volume Shadow Copy Service (VSS).

Confirm that you forced a VSS shadow operation.

Confirm that you modified a file, then restored its original version from VSS.
