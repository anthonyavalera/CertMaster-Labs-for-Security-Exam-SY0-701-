# APPLIED LAB: Implement Backups

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

3.4 Explain the importance of resilience and recovery in security architecture.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

### Prepare for backup operations

This exercise prepares a second storage device to be used as the backup target and preps several files for backup and restore operations.

Connect to the PC10 virtual machine, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

Create a full drive volume on Disk 1 named Backup01 with a drive letter F using diskpart. This is the volume that you will use as backup media.

Disks and partitions (and volumes) are numbered cardinally, which means the first item is numbered 0 (zero), and the second item is numbered 1 (one). Therefore, disk 0 is the main OS boot drive, while disk 1 is the additional drive that is currently unused.

Expand this hint for guidance.
Enter cmd into the Windows search bar, and then, on the menu, select Command Prompt.

To open the diskpart utility in interactive mode, run the command: diskpart

Select Yes on the User Account Control window.

To place the focus of the diskpart tool on Disk 1, run the command: select disk 1

To set the disk to online (i.e., available for use), run the command: online disk

To enable disk writing, run the command: attribute disk clear readonly

To create a full drive partition/volume on Disk 1, run the command: create partition primary

To format the partition with NTFS and assign a label, run the command: format fs=ntfs label=“Backup01” quick

To assign drive letter F: to the new partition, run the command: assign letter=f

To exit diskpart, run the command: exit

Create 3 copies of the file C:\SETUP\NetworkMiner\Fingerprints\oui.txt into the C:\Users\Jaime\Documents folder and assign new filenames of document01.txt, document02.txt, and document03.txt.

The oui.txt file copies will be used as sample files for the backup and restoration operations.

Expand this hint for guidance.
Run the following command to copy a file to a new name:
copy C:\SETUP\NetworkMiner\Fingerprints\oui.txt C:\Users\Jaime\Documents\document01.txt
Run the following command to copy a file to a new name:
copy C:\SETUP\NetworkMiner\Fingerprints\oui.txt C:\Users\Jaime\Documents\document02.txt
Run the following command to copy a file to a new name:
copy C:\SETUP\NetworkMiner\Fingerprints\oui.txt C:\Users\Jaime\Documents\document03.txt
Select the Score button to validate this task:

Create 1 copy of file C:\SETUP\NetworkMiner\Fingerprints\oui.txt into the C:\Users\Public\ folder and assign a new filename of document04.txt.

Expand this hint for guidance.
Run the following command to copy a file to a new name:
copy C:\SETUP\NetworkMiner\Fingerprints\oui.txt C:\Users\Public\document04.txt
Select the Score button to validate this task:

Close the Command Prompt window.

Most backup solutions can protect files of any type. You will not be limited to text documents. Typically, the primary limiting factor for backups is the amount of data to be protected vs. the available space on the backup media (i.e., target or save location).

Check your work
Confirm that you created a volume to use as the backup media.
Confirm that you copied several files to use as backup samples.

### Protect files using Windows Server Backup

In this exercise, you will perform a one-time backup of a folder and its contents.

Connect to the PC10 virtual machine. If needed, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

Using Windows Server Backup, perform a one-time backup of C:\Users to drive F:.

Expand this hint for guidance.
Enter Backup into the Windows search bar, and then, on the menu, select Windows Server Backup.

Select Local Backup from the left pane.

Wait for the interface to update. It may take up to 10 seconds.

Select Backup Once from the right-pane.

On the Backup Once Wizard window, on the Backup Options page, confirm that Different options is selected, then select Next.

On the Select Backup Configuration page, seelcct Custom, then select Next.

On the Select Items for Backup page, select Add Items.

On the Select Items windows, select the plus sign beside Local disk (C:) to expand its contents.

Select the checkbox for Users.

Select OK.

On the Select Items for Backup page, select Next.

On the Specify Destination Type page, select Local drives, then select Next.

On the Select Backup Destination page, use the Backup destination pull-down list to select Backup01 (F:), then select Next.

On the Confirmation page, select Backup.

It may take up to 1 minute for the backup process to complete. Wait for the backup to be completed.

Once the backup is completed, select Close to exit the Backup Once Wizard.

You should be returned to the Windows Server Backup window.

Select the Score button to validate this task:

Minimize the Windows Server Backup window, but leave it open.

While a one-time backup can be useful, a periodic scheduled backup is more likely to be used in a real-world scenario. A scheduled backup can be created using the Backup Schedule option. The associated wizard will include pages to set the time/date interval of the backups.

Check your work
Confirm that you performed a backup.

### Delete a file and restore it from backup

Connect to the PC10 virtual machine. If needed, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

Delete document01.txt.

Expand this hint for guidance.
Enter File Explorer into the Windows search bar, and then, on the menu, select File Explorer.

Double-click Documents.

Right-click document01, then select Delete.

Minimize File Explorer.

Right-click Recycle Bin, which should be in the top-left corner of the Desktop.

Select Empty Recycle Bin.

Select Yes on the Delete File pop-up window asking if you are sure you want to permanently delete this file.

By emptying the Recycle Bin you are ensuring the file is fully removed from the system and that only a restoration from backup will cause the file to exist again in the Documents folder.

Select the Score button to validate this task:

Restore the document01.txt file from backup.

Expand this hint for guidance.
Return to the Windows Server Backup window by selecting its icon on the Taskbar.

Select Recover from the right-pane.

On the Recovery Wizard, on the Getting Started page, select This server (PC10), then select Next.

On the Select Backup Date page, the backup that you made in the previous exercise (just minutes ago) should already be selected and visible. Select Next.

On the Select Recovery Type page, select Files and folders, then select Next.

On the Select Items to Recover page, select the plus sign to expand PC10, then expand Local disk (C:), then Users, then jaime.

Select Documents under the jamie folder.

Select document01.txt in the Items to recover: area, then select Next.

On the Specify Recovery Options page, select Original location, leave all other options at their defaults, then select Next.

On the Confirmation page, select Recover.

Once the recovery process is complete, select Close.

Confirm the document01.txt is restored.

Expand this hint for guidance.
Return to the File Manager window by selecting its icon from the Taskbar.

The Documents folder should still be the focus of File Manager.

Double-click document01.

Close Notepad.

Select the Score button to validate this task:

Close all windows.

Check your work
Confirm that you deleted a file.
Confirm that you restored a file from a backup.

### Enable Volume Shadow Copy Service

In this exercise, you will use the Volume Shadow Copy Service (VSS) to restore a previous version of a modified file.

Connect to the PC10 virtual machine. If needed, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

Enable Volume Shadow Copy Service on Drive C:.

Expand this hint for guidance.
Right-click the Start Menu, then select Disk Management.

Right-click (C:), then select Properties.

On the Local Disk (C:) Properties window, select the Shadow Copies tab.

On the Shadow Copies tab, select C:\, then select Enable.

On the Enable Shadow Copies window, select Yes.

On the Local Disk (C:) Properties window, you should now see a date and time in the Next Run Time column in the top area. A shadow copy should be made immediately upon enablement. If so, the current time and date will be shown in the bottom Shadow copies of selected volume area.

Select OK to close the Local Disk (C:) Properties window.

Close Disk Management.

Select the Score button to validate this task:

Force the Volume Shadow Copy Service (VSS) to make a backup of drive C:.

Expand this hint for guidance.
Enter cmd into the Windows search bar, and then, on the menu, select Run as administrator under Command Prompt.

Select Yes on the User Account Control window.

Run the following command to force VSS to perform a backup:

wmic shadowcopy call create Volume=c:\
This command should result in several lines of operation. The second line should state Method execution successful.

Close the Command Prompt.

Normally the VSS system will retain a copy of a file just as a change is performed. However, we have not found this process as reliable as claimed, so forcing a shadow operation ensures that the exercise functions as expected.

Alter the document04 file from the C:\Users\Public\ folder to add a new first line of changed.

Expand this hint for guidance.
Enter file explorer into the Windows search bar, and then, on the menu, select File Explorer under Command Prompt.

Select This PC.

Double-click Local Disk (C:).

Double-click Users, then double-click Public.

Double-click the file document04.

Notepad should open, displaying the contents of the document04 file.

The cursor should already be positioned at the start of the first line. If not, move the cursor to the start of the first line using the keyboard arrow keys or by clicking the mouse just before the first character.

Enter changed. Be sure to press Enter on your keyboard to ensure this word is on the first line by itself.

Close Notepad.

Select Save on the Notepad pop-up window asking do you want to save changes.

Select the Score button to validate this task:

Restore the previous version of document04.

Expand this hint for guidance.
Right-click document04, then select Restore previous versions.

The document04 Properties window is displayed, with the Previous Versions tab selected.

There should be at least one entry listed for document04 in the File versions: area. Select the top (i.e., most recent) entry.

Select Restore

Select Restore on the Previous Versions pop-up window that asks are you sure you want to restore the previous version.

Select OK on the Previous Versions pop-up window that states the file has been successfully restored to the previous version.

Select OK to close the document04 Properties window.

Volume Shadow Copy Service (VSS) will retain previous versions of files, but it is not a backup. If a file is removed from a drive, the VSS cannot be used to restore it. However, Windows File History and potentially a system restore point can be used to restore lost files.

Confirm that the previous unedited version of document04 is now restored to the system.

Expand this hint for guidance.
The File Explorer window should still be open.

Double-click document04

Notepad should open, displaying the contents of the document04 file.

Notice that the change you made previously by adding a new first line containing the word changed is no longer present. This confirms the previous version of this file was restored via VSS.

Close Notepad.

Select the Score button to validate this task:

Leave File Explorer open.

Check your work
Confirm that you enabled the Volume Shadow Copy Service (VSS).
Confirm that you forced a VSS shadow operation.
Confirm that you modified a file, then restored its original version from VSS.
