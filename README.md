# Assisted Lab: Managing Permissions

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.5 Explain the purpose of mitigation techniques used to secure the enterprise.
3.3 Compare and contrast concepts and strategies to protect data.
4.6 Given a scenario, implement and maintain identity and access management.

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

### Set file permissions on Linux

While reviewing file permissions on a Linux system, you discover an important file (demofile.sh) has read, write, and execute access granted to everyone. This important file should be limited to full access for the owner and execute access for the group, but no access for others. In this exercise, you will learn about viewing and settings Linux file permissions, and then you will apply the correct permissions to the important file.

Connect to the KALI and sign in as root using Pa$$w0rd as the password.

Open a Terminal window and then maximize the Terminal window.

Enter pwd to view the present working directory. It should be /root. If not, enter cd ~ to return to the root’s home directory.

The command “cd ~” will return the Terminal prompt to the home directory of the current user. If you are working from an elevated prompt (where you logged in as a non-root user, then used sudo su to elevate to root), then that would be the root user’s home folder which is /root rather than the non-root users home folder (such as the user kali, whose home folder is /home/kali). The root account’s home directory is not contained in the /home directory, where all other user account’s home directories are located.

...less
Enter ls -l to view the contents of the present working directory in long-list format.

Enter ls -l te* to view the contents of the present working directory in long-list format.

Notice that the first portion of the line for the testfile.txt file displays the permissions currently set on this file.

What is the initial permissions setting of testfile.txt?

-rwx--r---
-r---w---x
-rwxrwxrwx
-rw-r--r--
Linux permissions are displayed when viewing a long-list format of a directory listing. The data set starts off with either a dash or a d. The dash indicates the item is a file, while a d indicates the item is a directory. Then there are three groups of three permissions. The first group is for the user owner, the second for the group owner, and the third is for everyone else (usually called others or sometimes called the world). The permissions are read (r), write (w), and execute (x). The permission is granted or assigned when the letter is present in the permissions listing, but when there is a dash (i.e., -), it means the permission is withheld or denied. The technique of using letters to indicate permission settings is called symbolic representation.

...less
Enter chmod u+x testfile.txt. This command will add the execute permission to the user owner.

Enter ls -l te* to view the results.

Notice the change in permissions.

Enter chmod g+w testfile.txt. This command will add the write permission to the group owner.

Enter ls -l te* to view the results.

Notice the change in permissions.

Enter chmod go-r,u-x testfile.txt. This command will remove the read permission from the group owner and others, and it will also remove the execute permission from the user owner.

Enter ls -l te* to view the results.

Notice the change in permissions.

The chmod command also accepts octal representation of the permissions, so you don’t have to edit just one element at a time. Each of the sets of permissions can be viewed almost like binary values where the positions of RWX represent the numbers 421. For example, a permission of read is 4, while a permission of read and write is 6. The numbers between 0 and 7 are used for each of the user owner, group owner, and others positions.

...less
Enter chmod 777 testfile.txt. This command assigns rwx to user owner, group owner, and others.

Enter ls -l te* to view the results.

Notice the change in permissions.

Enter chmod 740 testfile.txt. This command assigns rwx to user owner, r to group owner, and nothing to others.

Enter ls -l te* to view the results.

Notice the change in permissions.

Enter chmod 654 testfile.txt. This command assigns rw to user owner, rx to group owner, and r to others.

Enter ls -l te* to view the results.

Notice the change in permissions.

Enter chmod 644 testfile.txt. This command assigns rw to user owner and r to group owner and r to others.

Enter ls -l te* to view the results.

Notice the change in permissions.

What symbolic representation could be applied to demofile.sh to enforce full access for the user owner, execute access for the group owner, but no access for others?

u+rwx,g+x-rw,o-rwx
u+*,g-rw,o-*
u+rwx,g+x
644
Using octal representation, in the box below, type in the 3-digit octal value to set permissions on demofile.sh to be limited to full access for the owner, execute access for the group, but no access for others.

Octal representation for demofile.sh: 

Press enter after you type in the value or click out of the text box.

Select the Score button to validate this task:

Enter chmod <demofile-chmod> demofile.sh.

If this chmod command is not followed by a 3-digit number, then you did not enter one in the text box in the previous step.

Enter ls -l to view the results.

Visually confirm that the permissions of demofile.sh are now -rwx--x---.

Enter ./demofile.sh to discover the important function of this script.

Use cat demofile.sh to see the code!

Check your work
Confirm that you set Linux file permissions using symbolic notation with chmod.
Confirm that you set Linux file permissions using octal notation with chmod.

### Manage the NTFS permissions of a file

Understanding Windows NTFS file permissions is another key access control security skill. In this exercise, you will view the current permissions of a file, then modify the permissions of that file, and then view the results.

Connect to the PC10 virtual machine and sign in as jaime using Pa$$w0rd as the password.

Select the Type Text icon to enter the associated text into the virtual machine.

Select Type here to search from the taskbar, type powershell, right-click Windows PowerShell from the results, then select Run as administrator.

Select Yes on the User Account Control window.

Enter cd c:\LABFILES.

Enter the following command to view the permissions currently assigned to this file.

icacls .\comptia-logo.jpg     
What permissions are assigned to the Users group for the comptia-logo.jpg file?

Modify
none
FullControl
ReadAndExecute
Enter the following command to deny read permission to the user dylan:

icacls .\comptia-logo.jpg /deny dylan:R    
Enter the following command to view the results of your permissions change:

icacls .\comptia-logo.jpg   
Enter the following command to grant full control to a user:

icacls .\comptia-logo.jpg /grant dylan:F
Enter the following command to view the results of your permissions change:

icacls .\comptia-logo.jpg   
Enter the following command to remove specific permissions for a user:

icacls .\comptia-logo.jpg /remove:g dylan
Enter the following command to view the results of your permissions change:

icacls .\comptia-logo.jpg   
After removing the NTFS permissions specific to a user account does not necessarily mean that the user will have no remaining permissions to the object. If the user is a member of any other group that still has defined permissions, then the user will inherit NTFS permissions from those groups. Removing the specific NTFS permissions just eliminates any customizations of NTFS permissions for that specific account.

...less
Leave the PowerShell window open.

Check your work
Confirm that you viewed the NTFS permissions for a file
Confirm that you denied NTFS permissions for the dylan account
Confirm that you granted NTFS permissions for the dylan account
Confirm that you removed the NTFS permissions entry for the dylan account

### Review effective permissions

Setting file permissions for users and/or groups is fairly straightforward. However, if a user has directly assigned permissions to a file and may inherit permissions through group memberships, it can be challenging to understand what permissions a user will end up with. The Effective Access view (a.k.a., effective permissions) can be used to see the current access to a resource for a user or group based on the full accumulation of their assigned or inherited permissions.

Connect to the PC10 virtual machine and sign in as jaime using Pa$$w0rd as the password.

Select the Type Text icon to enter the associated text into the virtual machine.

Select Type here to search from the taskbar, type file explorer, then select File Explorer from the results.

In the left pane, double-click to expand This PC, then select Local Disk (C:).

In the right pane, double-click LABFILES.

Right-click comptia-logo, then select Properties.

On the comptia-logo Properties window, select the Security tab.

The Security tab displays a summary of the NTFS permissions currently defined on the file. These permissions have been inherited from the parent folder.

On the Security tab, select Advanced.

On the Advanced Security Settings for comptia-log window, select the Effective Access tab.

What are the names of the other two tabs on the Advanced Security Settings for comptia-logo window? (Select two)

Auditing
General
Permissions
Details
Next to User/Group, select Select a user.

In the Enter the Object name to select field type dylan, then select OK.

You are returned to the Advanced Security Settings for comptia-logo window with the User/Group set to Dylan (Dylan@structureality.com).

Select View effective access.

Scroll down the window to view the current Effective Access for dylan on this file.

Notice that the user does not currently have Full control, Change permissions, or Take ownership. Therefore, they must not be a member of the Power Users group.

The Effective Access tool is only a predictive utility. It does not actually change group memberships or access permissions. It can be used to test a hypothesis or to troubleshoot access issues. Also, if a file object could be accessed through a share, the user or group's share-assigned permissions are included in the effectiveness evaluation.

Select OK, then OK to return to the File Manager window.

Close File Manager.

Check your work
Confirm that you viewed Effective Access.

### Manage Windows share permissions

In addition to NTFS file permissions, you also need to understand share permissions. A user's ability to access a resource across a network depends upon the combination of their on-object file permissions and the permissions they have from the share. The most restrictive of these two permission settings takes precedence.

Sign in to the PC10 virtual machine as jaime using Pa$$w0rd as the password.

Select the Type Text icon to enter the associated text into the virtual machine.

An administrator PowerShell window should still be open.

Enter the following command to create a new share:

New-SmbShare -Name "LABFILES" -Path "C:\LABFILES" -Description "Share for LABFILES"
Enter the following command to view the current shares:

Get-SMBShare
What is the name of one of the pre-existing shares on PC10? Type one of the names exactly as it appears in the result from Get-SMBShare.

Enter the following command to view the LABFILE share's permissions:

Get-SmbShareAccess -Name "LABFILES"
Enter the following command to grant user dylan change permission on the share:

Grant-SmbShareAccess -Name "LABFILES" -AccountName "dylan" -AccessRight Change
When prompted to confirm, enter Y.

This command will automatically display the permission results after applying the change.

Enter the following command to remove user dylan's specific permission on the share:

Revoke-SmbShareAccess -Name "LABFILES" -AccountName "dylan"
When prompted to confirm, enter Y.

This command will automatically display the permission results after applying the change.

Leave the PowerShell window open.

Check your work
Confirm that you created a Windows share
Confirm that you viewed Windows share permissions
Confirm that you assigned Windows share permissions to a user
Confirm that you removed a user's Windows share permissions
