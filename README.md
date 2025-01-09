# Assisted Lab: Configuring Examples of Security Control Types

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

1.1 Compare and contrast various types of security controls.

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

A preventive control attempts to stop an unwanted activity from taking place. In this exercise, you will first perform an undesirable activity. Next, you will implement a preventive control to block that activity. And finally, you will attempt the unwanted activity again to test the preventive control.

A TOOLS folder has been configured on the DC10 server. This folder is designed to hold utilities and data files that must only be accessible to domain and local administrators only. Users without an administrative rule should be prevented from viewing the share.

Verify whether the share has been configured with appropriate permissions by trying to access it using a non-administrative account.

1. Select the PC10 VM. Send Ctrl+Alt+Delete and, select Other user. In the User name box, type Sam. In the Password box, type Pa$$w0rd, and press Enter.

2. From the taskbar, select File Explorer windows 10 icon explorer.png

3. In the File Explorer address bar, enter \\10.1.16.1\TOOLS.

4. You should see the contents of the TOOLS share.

Sam is not an administrator and should not have access. You need to implement a prevention control so that Sam and other non-administrators cannot access this share.

5. Close File Explorer.

6. Select the DC10 VM. Send Ctrl+Alt+Delete and, if needed, sign in as Structureality\Administrator using Pa$$w0rd as the password.

7. In Server Manager, select File and Storage Services, and then select Shares.

8. Right-click the TOOLS share, then select Properties.

9. Select the Permissions tab, and then click Customize permissions.

Which account or group object on the access control list should NOT have been assigned permissions on the share?

Domain Admins
CREATOR OWNER
Users
LocalAdmin
Check your work
Confirm that you implemented a preventive control.
Confirm that you tested a preventive control.

### Configure and test detective controls

A detective control records a log each time an event occurs, regardless of whether that activity is benign or malicious. In this exercise, you will first perform an activity that will not be logged. Next, you will configure logging to record that activity, and then you will perform the activity again. Finally, you will review the log to confirm the record of the activity was created.

Connect to the PC10 virtual machine, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

Jaime is a member of the LocalAdmin group. So, this user account is an administrator on the PC10 system.

Open File Explorer, and from the Quick access pane, select LABFILES.

Right-click the folder empty then select Delete.

The empty folder should no longer be present.

The default configuration of Windows Server 2019 is NOT to prompt to confirm deletions. This setting can be changed through the Properties of the Recycle Bin.

Select the Score button to validate this task:

Right-click Start and select Event Viewer.

Maximize the Event Viewer window.

Double-click Windows Logs to expand its contents.

Select Security from in the Windows Logs expanded contents.

Wait a few moments for the log to be loaded and displayed.

Select Find… in the right pane.

Type empty in the Find what: filed, then select Find Next.

After a few moments of searching, a window will appear stating the search term was not found. Select OK.

The results of the find operation indicate what?

User activity is being tracked
Folder deletion is not being audited
Jamie is an administrator
Users are unable to access empty folders
Select Cancel to close the Find window.

Select Type here to search from the taskbar, type local, then select Local Security Policy from the results.

Double-click Local Policies to expand its contents.

Select Audit Policy from the Local Policies expanded contents.

Right-click Audit object access in the right pane, then select Properties.

Select to mark both the Success and Failure checkboxes, then select OK.

While the main switch for auditing object access activities is now on, auditing will not occur on most file objects until an on-object auditing setting is made.

Close the Local Security Policy window.

The setting change should apply immediately. If the next steps do not result in a record of a folder deletion in the Security log accessed through the Event Viewer, restart PC10 and repeat from here, but you will then need to delete the MARKETING folder.

Return to File Explorer.

Right-click LABFILES in the left pane, then select Properties.

Select the Security tab on the LABFILES Properties window.

Select Advanced.

Select the Auditing tab on the Advanced Security Settings for LABFILES window.

Select Continue since you are an administrator.

Select Add.

Select Select a principle on the Auditing Entry for LABFILES window.

Type everyone in the Enter the object name to select field, then select Check Names.

The field should now display Everyone..

Select OK.

Select Show advanced permissions from the middle area of the Auditing Entry for LABFILES window.

Select to mark the Delete subfolders and files and Delete checkboxes.

Select OK to save the settings and close the Auditing Entry for LABFILES window.

Select to mark the Replace all child object auditing… settings checkbox.

Select OK to save the settings and close the Advanced Security Settings for LABFILES window.

Select OK to save the settings and close the LABFILES Properties window.

Return to File Explorer.

Right-click the pcaps folder, then select Delete.

The pcaps folder should no longer be present.

Select the Score button to validate this task:

Minimize File Explorer.

Return to the Event Viewer.

Select Refresh from the right pane.

Select the first entry at the top of the middle pane.

This sets the search-from point for the Find function, which only searches from the currently selected entry to earlier entries (i.e., down).

Select Find… in the right pane.

Type 4660 in the Find what: filed, then select Find Next.

4660 is the Event ID for the event type of object deletion.

Select Cancel to close the Find window.

An audit record of Event ID: 4660 should be selected. In the bottom pane, you should see the statement "An object was deleted".

Oddly, while Event ID 4660 is the record of an object being deleted, it does not contain the actual object's name. For that, you need to find the associated Event ID 4663.

The Event ID 4663 for the folder deletion should be about five records above the currently selected one. Select the lowest record of Event ID 4663 above the currently selected Event ID 4660 record.

The correct Event ID 4663 record should be about five records above the selected Event ID 4660 record.

Once you have selected the Event ID 4663 record, you can view the details in the bottom pane. The General tab has a small scrollable sub-window with details. You should see a line of "Object Name: C\LABFILES\pcaps". This Event ID 4663 record confirms that the object deleted was the C\LABFILES\pcaps folder.

You could also select the Details pane to see most of the same information.

What is the purpose of a detective control?

Deny access to an object
Notify subjects about system policies
Inform users of the proper steps to perform an activity
Create a record of events and activities
Sign out of PC10 by selecting the Start menu, then selecting Jaime (which will be a circle at the top of the menu), then select Sign out. If prompted that there are open programs, select Sign out anyway.

You have successfully implemented a detective control to record object access activity.

Check your work
Confirm that you implemented a detective control.
Confirm that you tested a detective control.

A directive control provides instruction to direct a user towards more compliant behavior. In this exercise, you will configure a directive control in the form of a login warning banner. Finally, you will test this directive control.

Connect to the PC10 virtual machine, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

Jaime is a member of the LocalAdmin group. So, this user account is an administrator on the PC10 system.

Right-click Start, and select Windows PowerShell (Admin). At the UAC prompt, select Yes.

Enter the following code into the Administrator: Windows PowerShell console:

Be sure to press Enter on your keyboard after each entry fully appears in the PowerShell console. There will not be any confirmation.

$BannerText = "This computer system is the property of Structureality Inc. It is for authorized use only. By using this system, all users acknowledge notice of and agree to comply with the Acceptable Use Policy (AUP). Unauthorized or improper use of this system may result in administrative disciplinary action, civil charges/criminal penalties, and/or other sanctions set forth in the AUP. By continuing to use this system, you indicate your awareness of and consent to these terms and conditions. If you are physically located in the European Union, you may have additional rights per the GDPR. Visit the website gdpr-info.eu for more information."

New-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\System" -Name "legalnoticecaption" -Value "Authorized Use Only" -PropertyType "String" -Force | Out-Null

New-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\System" -Name "legalnoticetext" -Value $BannerText -PropertyType "String" -Force | Out-Null

The text of the warning banner in this exercise amalgamates several banners used by various commercial and educational facilities. Be sure to consult with your own legal counsel before setting a warning banner to ensure it complies with laws and regulations.

Select the Score button to validate this task:

Sign out of PC10 by selecting the Start menu, then selecting Jaime (which will be a circle at the top of the menu), then select Sign out. If prompted that there are open programs, select Sign out anyway.

Connect to the PC10 virtual machine, send Ctrl+Alt+Delete.

You should be presented with the login warning banner that was just defined.

What is the goal of directive controls?

Tracking
Prohibition
Defense
Compliance
Read the warning banner, then select OK.

Complete the sign-in process as Jaime using Pa$$w0rd as the password.

You have successfully implemented a directive control to inform personnel of the limitations and restrictions of a controlled system.

Check your work
Confirm that you implemented a directive control.
Confirm that you tested a directive control.

