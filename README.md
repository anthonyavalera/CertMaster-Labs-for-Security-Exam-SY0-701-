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

- Server Manager
- Event Viewer
- 

## Steps

### Configure and test preventive controls

A preventive control attempts to stop an unwanted activity from taking place. In this exercise, you will first perform an undesirable activity. Next, you will implement a preventive control to block that activity. And finally, you will attempt the unwanted activity again to test the preventive control.

A TOOLS folder has been configured on the DC10 server. This folder is designed to hold utilities and data files that must only be accessible to domain and local administrators only. Users without an administrative rule should be prevented from viewing the share.

Verify whether the share has been configured with appropriate permissions by trying to access it using a non-administrative account.

1. Select the PC10 VM. Send Ctrl+Alt+Delete and, select Other user. In the User name box, type Sam. In the Password box, type Pa$$w0rd, and press Enter.

2. From the taskbar, select File Explorer windows 10 icon explorer.png

3. In the File Explorer address bar, enter \\10.1.16.1\TOOLS.

4. You should see the contents of the TOOLS share.

    - Sam is not an administrator and should not have access. You need to implement a prevention control so that Sam and other non-administrators cannot access this share.

5. Close File Explorer.

6. Select the DC10 VM. Send Ctrl+Alt+Delete and, if needed, sign in as Structureality\Administrator using Pa$$w0rd as the password.

7. In Server Manager, select File and Storage Services, and then select Shares.

8. Right-click the TOOLS share, then select Properties.

9. Select the Permissions tab, and then click Customize permissions.

Check your work

Confirm that you implemented a preventive control.
Confirm that you tested a preventive control.

### Configure and test detective controls

A detective control records a log each time an event occurs, regardless of whether that activity is benign or malicious. In this exercise, you will first perform an activity that will not be logged. Next, you will configure logging to record that activity, and then you will perform the activity again. Finally, you will review the log to confirm the record of the activity was created.

1. Connect to the PC10 virtual machine, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

    - Jaime is a member of the LocalAdmin group. So, this user account is an administrator on the PC10 system.

2. Open File Explorer, and from the Quick access pane, select LABFILES.

3. Right-click the folder empty then select Delete.

    - The empty folder should no longer be present.

    - The default configuration of Windows Server 2019 is NOT to prompt to confirm deletions. This setting can be changed through the Properties of the Recycle Bin.

4. Right-click Start and select Event Viewer.

5. Maximize the Event Viewer window.

6. Double-click Windows Logs to expand its contents.

7. Select Security from in the Windows Logs expanded contents.

    - Wait a few moments for the log to be loaded and displayed.

8. Select Find… in the right pane.

9. Type empty in the Find what: filed, then select Find Next.

10. After a few moments of searching, a window will appear stating the search term was not found. Select OK.

11. Select Cancel to close the Find window.

12. Select Type here to search from the taskbar, type local, then select Local Security Policy from the results.

13. Double-click Local Policies to expand its contents.

14. Select Audit Policy from the Local Policies expanded contents.

15. Right-click Audit object access in the right pane, then select Properties.

16. Select to mark both the Success and Failure checkboxes, then select OK.

    - While the main switch for auditing object access activities is now on, auditing will not occur on most file objects until an on-object auditing setting is made.

17. Close the Local Security Policy window.

    - The setting change should apply immediately. If the next steps do not result in a record of a folder deletion in the Security log accessed through the Event Viewer, restart PC10 and repeat from here, but you will then need to delete the MARKETING folder.

18. Return to File Explorer.

19. Right-click LABFILES in the left pane, then select Properties.

20. Select the Security tab on the LABFILES Properties window.

21. Select Advanced.

22. Select the Auditing tab on the Advanced Security Settings for LABFILES window.

23. Select Continue since you are an administrator.

24. Select Add.

25. Select Select a principle on the Auditing Entry for LABFILES window.

26. Type everyone in the Enter the object name to select field, then select Check Names.

    - The field should now display Everyone..

27. Select OK.

28. Select Show advanced permissions from the middle area of the Auditing Entry for LABFILES window.

29. Select to mark the Delete subfolders and files and Delete checkboxes.

30. Select OK to save the settings and close the Auditing Entry for LABFILES window.

31. Select to mark the Replace all child object auditing… settings checkbox.

32. Select OK to save the settings and close the Advanced Security Settings for LABFILES window.

33. Select OK to save the settings and close the LABFILES Properties window.

34. Return to File Explorer.

35. Right-click the pcaps folder, then select Delete.

    - The pcaps folder should no longer be present.

36. Minimize File Explorer.

37. Return to the Event Viewer.

38. Select Refresh from the right pane.

39. Select the first entry at the top of the middle pane.

    - This sets the search-from point for the Find function, which only searches from the currently selected entry to earlier entries (i.e., down).

40. Select Find… in the right pane.

41. Type 4660 in the Find what: filed, then select Find Next.

    - 4660 is the Event ID for the event type of object deletion.

42. Select Cancel to close the Find window.

43. An audit record of Event ID: 4660 should be selected. In the bottom pane, you should see the statement "An object was deleted".

    - Oddly, while Event ID 4660 is the record of an object being deleted, it does not contain the actual object's name. For that, you need to find the associated Event ID 4663.

44. The Event ID 4663 for the folder deletion should be about five records above the currently selected one. Select the lowest record of Event ID 4663 above the currently selected Event ID 4660 record.

    - The correct Event ID 4663 record should be about five records above the selected Event ID 4660 record.

45. Once you have selected the Event ID 4663 record, you can view the details in the bottom pane. The General tab has a small scrollable sub-window with details. You should see a line of "Object Name: C\LABFILES\pcaps". This Event ID 4663 record confirms that the object deleted was the C\LABFILES\pcaps folder.

    - You could also select the Details pane to see most of the same information.

46. Sign out of PC10 by selecting the Start menu, then selecting Jaime (which will be a circle at the top of the menu), then select Sign out. If prompted that there are open programs, select Sign out anyway.

You have successfully implemented a detective control to record object access activity.

Check your work

Confirm that you implemented a detective control.
Confirm that you tested a detective control.

### Configure and test directive controls

A directive control provides instruction to direct a user towards more compliant behavior. In this exercise, you will configure a directive control in the form of a login warning banner. Finally, you will test this directive control.

1. Connect to the PC10 virtual machine, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

    - Jaime is a member of the LocalAdmin group. So, this user account is an administrator on the PC10 system.

2. Right-click Start, and select Windows PowerShell (Admin). At the UAC prompt, select Yes.

3. Enter the following code into the Administrator: Windows PowerShell console:

    - Be sure to press Enter on your keyboard after each entry fully appears in the PowerShell console. There will not be any confirmation.

    - $BannerText = "This computer system is the property of Structureality Inc. It is for authorized use only. By using this system, all users acknowledge notice of and agree to comply with the Acceptable Use Policy (AUP). Unauthorized or improper use of this system may result in administrative disciplinary action, civil charges/criminal penalties, and/or other sanctions set forth in the AUP. By continuing to use this system, you indicate your awareness of and consent to these terms and conditions. If you are physically located in the European Union, you may have additional rights per the GDPR. Visit the website gdpr-info.eu for more information."

    - New-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\System" -Name "legalnoticecaption" -Value "Authorized Use Only" -PropertyType "String" -Force | Out-Null

    - New-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\System" -Name "legalnoticetext" -Value $BannerText -PropertyType "String" -Force | Out-Null

    - The text of the warning banner in this exercise amalgamates several banners used by various commercial and educational facilities. Be sure to consult with your own legal counsel before setting a warning banner to ensure it complies with laws and regulations.

4. Sign out of PC10 by selecting the Start menu, then selecting Jaime (which will be a circle at the top of the menu), then select Sign out. If prompted that there are open programs, select Sign out anyway.

5. Connect to the PC10 virtual machine, send Ctrl+Alt+Delete.

6. You should be presented with the login warning banner that was just defined.

7. Read the warning banner, then select OK.

8. Complete the sign-in process as Jaime using Pa$$w0rd as the password.

You have successfully implemented a directive control to inform personnel of the limitations and restrictions of a controlled system.

Check your work

Confirm that you implemented a directive control.
Confirm that you tested a directive control.

### Configure and test corrective controls

A corrective control is intended to detect when something is in a less secure or less desirable state, then attempts to return to the more secure or more desirable state. In some cases, the corrective control can repair minor damage to restore a system to a more secure or desirable state.

In this exercise, you will first use a fault injection tool to trigger the existing correct control of Windows to trigger its native corrective control protection against misbehaving applications. Next, you will create and test a custom corrective control to protect the contents of a text file.

1. Connect to the PC10 virtual machine and, if needed, send Ctrl+Alt+Delete, select OK, then complete the sign-in process as Jaime using Pa$$w0rd as the password.

2. Open File Explorer, and from the Quick access pane, select SYSINTERNALS.

3. Scroll to locate, then double-click notmyfault64 to execute it.

    - There is a CLI (command line interface) version of NotMyFault, which has a c in the file name: notmyfaultc64. If a Command Prompt window flashes open and disappears, you selected the CLI version, not the GUI version of NotMyFault64.

    - Windows Sysinternals is a website that offers technical resources and utilities to manage, diagnose, troubleshoot, and monitor a Microsoft Windows environment. You can experiment with the Sysinternals tools in this lab environment or go directly to sysinternals.com to learn more and download nearly 75 tools onto your system.

4. Select Yes on the User Account Control window.

5. Select the Code overwrite option, then select Crash.

6. The PC10 system should immediately experience a stop error (often called the BSOD (Blue Screen of Death)). The system will perform a partial memory dump (for potential analysis - which will not be done in this lab) and then reboot.

    - You have verified that the Windows corrective control to protect the execution environment from misbehaving applications is active. While you might not prefer in-memory data to be lost, the stability of the Windows execution environment is protected by immediately ceasing all execution. You can be assured that the offending application will not be running once the system reboots. This native Windows protective feature is why you should save early and often when creating new content or media.

7. Connect to the PC10 virtual machine and send Ctrl+Alt+Delete, select OK, then complete the sign-in process as Jaime using Pa$$w0rd as the password.

    - Besides the preventive, detective, directive, and corrective control types covered in this lab's exercises. There are other types of controls, such as deterrent and compensation. A deterrent control persuades perpetrators to go elsewhere - such as a warning sign or an acceptable use policy (AUP). A compensation control is used to compensate for a failed control - such as a backup to compensate for a preventive control failing to stop the deletion of a file.

Next, you will create your own corrective control to simulate the correction functions of the SigVerif utility.

8. Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.

- In this exercise portion, you will create a corrective control to monitor the contents of a file. If the file contents change, the control will restore the file back to its preferred content.

9. Enter "This is important" | Set-Content notes.txt.

    - This command creates a text file containing the phrase "This is important".

10. Enter type notes.txt.

    - This command displays the contents of the notes.txt file.

11. Enter Get-FileHash ./notes.txt -Algorithm SHA256 | Select-Object -ExpandProperty Hash | Set-Content ./hash.txt.

    - This command calculates a hash of the file and stores it in hash.txt for future use.

    - The dot and slash (i.e., ./) in front of the filename indicate the current working directory.

12. Enter type hash.txt.

    - This command displays the contents of the hash.txt file, which is the hash calculated from the notes.txt file.

13. Enter echo blah >> notes.txt.

    - This command injects new content into notes.txt, which changes the file.

    - The use of double greater-than symbols (i.e., >>) performs an append rather than a replace function when capturing output into a file.

14. Enter type notes.txt.

    - You should see different contents of the notes.txt file.

15. Enter if((Get-FileHash ./notes.txt -Algorithm SHA256).Hash -eq (Get-Content ./hash.txt)) {Write-Host "The file is correct."} else {Write-Host "The file has changed. Corrective action should be initiated."}.

    - This command calculates the hash of notes.txt and compares it to the value stored in hash.txt. Since the file has changed, an error message is displayed.

16. Enter "This is important" | Set-Content notes.txt.

    - This command is the corrective action to reset the contents of notes.txt back to the desired content.

17. Enter type notes.txt.

18. Enter if((Get-FileHash ./notes.txt -Algorithm SHA256).Hash -eq (Get-Content ./hash.txt)) {Write-Host "The file is correct."} else {Write-Host "The file has changed. Corrective action should be initiated."}.

    - This command calculates the hash of notes.txt and compares it to the value stored in hash.txt. Since the file has been restored, a confirmation message is displayed.

    - You have performed the corrective control manually. Now configure scripts to automate the process.

19. Enter notepad calchash.ps1.

20. Select Yes on the Notepad window about creating a new file.

21. Type the following into the new document: Get-FileHash ./notes.txt -Algorithm SHA256 | Select-Object -ExpandProperty Hash | Set-Content ./hash.txt.

22. Close Notepad, select Save when prompted.

23. Enter ./calchash.ps1.

    - This command executes the PowerShell script of calchash.ps1, which generates a hash of the notes.txt file and saves it as hash.txt.. The "Set-Content" cmdlet performs a replacement rather than an append function when writing output into a file.

    - The dot and slash (i.e., ./) before the script name are essential for execution.

24. Enter type hash.txt.

    - This command displays the contents of hash.txt

25. Enter notepad check.ps1.

26. Select Yes on the Notepad window about creating a new file.

27. Select the empty area of the Notepad window, then select the  below to paste the script into the VM.

if((Get-FileHash ./notes.txt -Algorithm SHA256).Hash -ne (Get-Content ./hash.txt))

{

  "This is important" | Set-Content ./notes.txt
  Write-Host "The file has changed. Corrective action initiated."   

}

else

{

  Write-Host "The file is correct. No corrective action needed."
 
}

28. Close Notepad, select Save when prompted.

29. Enter ./check.ps1.

    - This command executes the PowerShell script of check.ps1, which calculates the hash of notes.txt and compares it to the value stored in hash.txt. If the file has not changed, a "No corrective action needed" message is displayed. If the file has changed, an "Corrective action initiated" message is displayed.

    - The result should display the "The file is correct. No corrective action needed." message since you previously restored the notes.txt file manually.

30. Enter type notes.txt.

    - You should see the correct contents of the notes.txt file.

31. Enter echo blah >> notes.txt.

    - This command injects new content into notes.txt, which changes the file.

32. Enter type notes.txt.

    - You should see the modified contents of the notes.txt file.

33. Enter ./check.ps1.

    - This should display the "The file has changed. Corrective action initiated." message since the notes.txt file was modified.

34. Enter type notes.txt.

    - You should see the corrected contents of the notes.txt file.

You have successfully implemented a simulation of a corrective control to repair the contents of a file should that file be modified.

        - This corrective action is similar to that performed by the Signature Verification (SigVerif) tool of Windows. SigVerif executes before each booting of Windows to ensure that the necessary files for a secure booting operation are present and meet a specific hash value. If any of those files are corrupted, they are removed and replaced with a valid file. The corrective actions you took manually can be automated to perform similarly. For example, you could schedule a boot task to run the check.ps1 script each time the system reboots. Also, you should run the calchash.ps1 script every time a valid change to notes.txt is performed. However, if you elect to change the contents of notes.txt, the correction action would need to be updated accordingly.

Check your work

Confirm that you implemented a corrective control.
Confirm that you tested a corrective control.
