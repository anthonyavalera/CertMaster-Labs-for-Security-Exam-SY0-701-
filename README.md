# Assisted Lab: Using Group Policy

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

4.5 Given a scenario, modify enterprise capabilities to enhance security.
4.7 Explain the importance of automation and orchestration related to secure operations.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Server Manager
- Command Prompt

## Steps

### Apply a Group Policy to an organizational unit

In this exercise, you will create a new organizational unit (OU) within the structureality.com domain. This OU will be used to contain users and computeres related to the newly established sales division. At this point, there is only one system, one user, and one admin assigned to the sales division. The work you do here can be built upon for future expansion. Once the OU is established, you will create and link a GPO to that OU. Then, you will make a few initial settings within the GPO which are to be applied to the members of the OU.

1. Sign in to DC10, send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

2. Server Manager should open automatically.

    - If Server Manager does not open, select Type here to search from the taskbar, enter server manager, and then select Server Manager.

3. On the Server Manager menu bar, select Tools, and then select Active Directory Users and Computers.

4. In the Active Directory Users and Computers window's navigation pane, right-click ad.structureality.com, select New, and then select Organizational Unit.

5. In the New Object - Organizational Unit window, in Name, enter SalesClients, and select OK.

6. In the Active Directory Users and Computers left pane, select the arrow beside ad.structureality.com to expand its contents, then select Nonadmins.

7. In the right pane, right-click Dani, and then select Move.

8. In the Move window, select SalesClients, and then select OK.

9. In the Active Directory Users and Computers left pane, select Admins.

10. In the right pane, right-click Cam, and then select Move.

11. In the Move window, select SalesClients, and then select OK.

12. In the Active Directory Users and Computers left pane, select Clients.

13. In the right pane, right-click PC10, and then select Move.

14. In the Move window, select SalesClients, and then select OK.

15. In the Active Directory Users and Computers left pane, select SalesClients.

16. Close Active Directory Users and Computers.

17. Return to Server Manager.

18. In the Server Manager window, on the menu bar, select Tools, and then select Group Policy Management.

19. In the Group Policy Management window's left navigation pane, the ad.structureality.com elements should already be expanded.

    - If not, expand Forest: ad.structureality.com | Domains | ad.structureality.com by selecting the > icon beside each element.

20. In the Group Policy Management window's left navigation pane, right-click SalesClients, and then select Create a GPO in this domain, and Link it here.

21. In the New GPO window, in Name, enter SalesPolicy, and then select OK.

22. In the Group Policy Management window's left navigation pane, expand SalesClients, and then verify that SalesPolicy is displayed.

23. Under SalesClients, right-click SalesPolicy, and then select Edit.

    - If you select (i.e., left click) rather than right-click a GPO, such as SalesPolicy, you may see a warning indicating that any edits to a GPO will affect all objects linked to the GPO. If this is displayed, then mark the checkbox Do not show this message again and then select OK.

24. In the Group Policy Management Editor window's left navigation pane, in Computer Configuration, expand Policies | Windows Settings | Security Settings | Account Policies, and then select Account Lockout Policy.

25. In Account Lockout Policy, double-click Account lockout threshold.

26. In the Account lockout threshold Properties window, select the Define this policy setting checkbox, in the invalid login attempts field enter 3, and then select OK.

27. In the Suggested Value Changes window, select OK.

28. In the Group Policy Management Editor window's left navigation pane, collapse Computer Configuration.

29. In the Group Policy Management Editor window's left navigation pane, in User Configuration, expand Preferences | Windows Settings, and then select Folders.

30. In the Folders pane, right-click in the area labeled There are no items to show in this view, select New, and then select Folder.

31. In the New Folder Properties window, in the Action dropdown menu, select Create.

32. In the Path field, enter C:\SalesDocs, and then select OK.

33. Verify that C:\SalesDocs is displayed in the Folders pane in the Path column.

34. Close the Group Policy Management Editor window.

35. Leave Group Policy Management and Server Manager windows open.

#### Check your work

Confirm that you created an OU.

Confirm that you moved Active Directory objects to a new OU.

Confirm that you created a GPO.

Confirm that you linked a GPO to an OU.

Confirm that you configured computer and user settings in a GPO.

### Review applied Group Policies and Group Policy settings

Now that the OU and a linked GPO are defined, you want to test that the GPO is functioning as expected. In this exercise, you will reboot the PC10 client and then confirm whether the settings of the GPO applied correctly.

1. Switch to PC10 and send Ctrl+Alt+Delete. But do not sign in.

2. Select the Power icon in the lower-right corner of the sign in screen, and then select Restart.

    - You must restart the virtual machine to ensure that a secure connection is established with the domain controller because both virtual machines were started at the same time when the challenge lab environment launched.

3. Once PC10 reboots, send Ctrl+Alt+Delete, select Other User, and then sign in as Dani using Pa$$w0rd as the password.

    - It may take up to a minute for the user profile to be created.

4. Select Type here to search from the taskbar, enter cmd, and then select Command Prompt.

5. Run the following command to display the current policies: gpresult /r. Scroll to view the full results.

    - In the results, under USER SETTINGS, under the Applied Group Policy Objects subheading, notice that the SalesPolicy and uu-domain-default are the two policies that are enforcing user settings.

    Notice that there is no COMPUTER SETTINGS section displayed in the results.

    The results indicate that there are user settings from various GPOs applied to this user when they sign in to this computer. The fact that there is no section for computer settings tells you that this user does not have the rights to view the computer settings. You must have administrative rights on the computer to see those settings.

6. Sign out of PC10 by selecting the Start button, select Dani, and then select Sign out.

7. Sign in to PC10 by sending Ctrl+Alt+Delete, select Other user, and then using Cam as the username and using Pa$$w0rd as the password.

    - It may take up to a minute for the user profile to be created.

8. Minimize or close Server Manager if it appears.

9. Select Type here to search from the taskbar, enter cmd, right-click Command Prompt from the results, then select Run as administrator.

    - An elevated Command Prompt is needed to use the administrative privileges of the Cam account. Otherwise, the Command Prompt defaults to a limited privileges state.

10. Select Yes on the User Account Control window to allow changes.

11. Run the following command to display the current policies: gpresult /r. Scroll to view the full results.

    - This time there should be both COMPUTER SETTINGS and USER SETTINGS sections displayed in the results, with various GPOs listed as being applied to this system.

12. Display a Resultant Set of Policy (RSOP) report by using the rsop command.

    - A window will appear as the tool gathers the information. After a few moments, the Resultant Set of Policy window will be displayed.

13. In the Resultant Set of Policy window's left navigation pane, in Computer Configuration, expand Windows Settings | Security Settings | Account Policies and then select Account Lockout Policy.

14. In Account Lockout Policy, in the Source GPO column, verify that SalesPolicy is displayed for all three policies.

    - The Allow Administrator account lockout was not defined in the SalesPolicy GPO, so it will not have a value in the Source GPO column.

15. Close the Resultant Set of Policy window. Select No on the Save console settings to rsop pop-up window.

16. Select Type here to search from the taskbar, enter File Explorer, and then select File Explorer.

17. In the left pane of File Explorer, select This PC.

18. In the right pane of File Explorer, double-click Local Disk (C:).

19. The folder SalesDocs should be present.

20. Close all windows.

#### Check your work

Confirm that you displayed an RSOP report by using the gpresult command.

Confirm that you displayed an RSOP report by using the rsop command.

Confirm that you verified the effect of a GPO application (i.e., that C:\SalesDocs was created).

### Create logon and logoff scripts

After confirming that the GPO settings are being applied to the OU members appropriately, you need to establish scripts that will run at logon and logoff. Management has not yet fully decided what will be executed by these scripts. So, you will simply create all the script components so that once they are set within the GPO, they will display a simple message confirming their opoeration when activated.

1. Switch to DC10, and then if needed, send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

2. Select Type here to search from the taskbar, enter File Explorer, and then select File Explorer.

3. In the left pane of File Explorer, select This PC.

4. In the right pane of File Explorer, double-click Local Disk (C:).

5. In Local Disk (C:), right-click in the empty space below the list of folders, select New, and then select Folder.

6. Enter scripts to name the folder.

7. Right-click the new scripts folder, and then select Properties.

8. In the scripts Properties window, select the Sharing tab.

9. On the Sharing tab, select Advanced Sharing.

10. In the Advanced Sharing window, select the Share this folder checkbox, and then select Permissions.

11. In the Permissions for scripts window, on the Share Permissions tab, in the Group or user names: area, ensure that Everyone is selected, and then in the Permissions for Everyone area, on the line of Full Control, select the Allow checkbox.

12. In the Permissions for scripts window, select OK.

13. In the Advanced Sharing window, select OK.

14. In the scripts Properties window, select Close.

15. In File Explorer, double-click the scripts folder to open it.

16. In the right pane of File Explorer, right-click the blank area, select New, and then select Text Document.

17. Enter logon to name the document logon.txt.

18. In the right pane of File Explorer, right-click the blank area, select New, and then select Text Document.

19. Enter logoff to name the document logoff.txt.

    - Using this method of file creation, both logon and logoff have .txt extensions. However, the default settings of File Explorer hide the extensions of known file types. However, you should see the label of Text Document in the Type column which confirms that these files have a .txt extension.

20. Double-click logon to open it in Notepad.

21. Enter Hello, this is a logon script. into Notepad.

22. Close Notepad and select Save on the pop-up window.

23. Double-click logoff to open it in Notepad.

24. Enter Goodbye, this is a logoff script. into Notepad.

25. Close Notepad and select Save on the pop-up window.

26. Select Type here to search from the taskbar, enter cmd, and then select Command Prompt

27. Enter echo \\DC10\scripts\logon.txt > c:\scripts\scriptlogon.cmd.

28. Enter echo \\DC10\scripts\logoff.txt > c:\scripts\scriptlogoff.cmd.

    - These two commands create files containing the share path names to the scipts you have created. This method of file creation was used to ensure that only the .cmd file extesion would be used on the file.

    - When defining logon and logoff scripts, you should ensure that you define all references to point to network shared objects as a relative reference, rather than local objects as an absolute reference. This is because the GPO may be applied to a user when logged in to a variety of different computers.

29. Enter dir c:\scripts.

    - You should see four documents displayed in the scripts directory, two with .txt extensions and two with .cmd extensions.

30. Close the Command Prompt window.

31. Return to File Explorer.

32. Double-click scriptlogon.

    - Notepad should open displaying the text from the Logon file.

33. Close Notepad.

Double-click scriptlogoff.

Notepad should open displaying the text from the Logoff file.

Close Notepad.

    - Each .cmd file should open in a Notepad window and display the contents of their respective .txt file. If so, close the Notepad windows. If not, review the filenames and contents of the .cmd files to confirm that they are crafted correctly.

    - A command window will be displayed behind each Notepad window. The command window will disappear when you close the corresponding Notepad window.

34. Close File Explorer.

#### Check your work

Confirm that you created files with contents named logon.txt and logoff.txt.

Confirm that you created files with contents named scriptlogon.cmd and scriptlogoff.cmd.

Confirm that you manually tested the logon and logoff scripts.

### Implement scripts by using Group Policy

Now that you have defined and manually tested the logon and logoff scripts, you will define them in the GPO attached to the OU. Then, you will test the scripts against the PC10 client and the sales user.

1. Connect to DC10, and then if needed, send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

2. Return to the Group Policy Management Console.

3. In the Group Policy Management windows left navigation pane, right-click SalesPolicy, and then select Edit.

    - If you select (i.e., left click) rather than right-click a GPO, such as SalesPolicy, you may see a warning indicating that any edits to a GPO will affect all objects linked to the GPO. If this is displayed, then mark the checkbox Do not show this message again and then select OK.

    - If the domain elements are collapsed, expand Forest: ad.structureality.com | Domains | ad.structureality.com | SalesClients.

4. In User Configuration, expand Policies | Windows Settings, and then select Scripts (Logon/Logoff).

    - If the Scripts element is Scripts (Startup/Shutdown) instead of Scripts (Logon/Logoff), then you have expanded the Computer Configuration section rather than the User Configuration section.

5. In the right pane under Scripts (Logon/Logoff), double-click Logon.

6. In the Logon Properties window, select Add.

7. In the Script Name field, enter \\DC10\scripts\scriptlogon.cmd, and then select OK.

8. In the Logon Properties window, select OK.

9. In right pane under Scripts (Logon/Logoff), double-click Logoff.

10. In the Logoff Properties window, select Add.

11. In the Script Name field, enter \\DC10\scripts\scriptlogoff.cmd, and then select OK.

12. In the Logoff Properties window, select OK.

    - When defining logon and logoff scripts, you should ensure that you define all references to point to network shared objects as a relative reference, rather than local objects as an absolute reference. This is because the GPO may be applied to a user when logged in to a variety of different computers.

13. Under User Configuration | Policies, expand Administrative Templates | System, and then select Scripts.

14. In the right-pane, double-click Display instructions in logoff scripts as they run.

    - There is also a setting named Display instructions in logon scripts as they run. Be sure to select the correct one.

15. Select Enable, then select OK.

16. Leave all windows open.

17. Switch to PC10, if needed send Ctrl+Alt+Delete, select the Power icon in the lower-right corner of the sign in screen, select Restart, then select Restart Anyway.

    - You must restart the virtual machine to ensure that the altered SalesPolicy GPO is properly applied to PC10.

18. Once PC10 reboots, send Ctrl+Alt+Delete, select Other User, and then sign in as Dani using Pa$$w0rd as the password.

    - As the Desktop is displayed, a Notepad window will be open displaying the contents of the logon.txt file, which is Hello, this is a logon script.

19. Close the Notepad window.

20. Sign out as Dani to test the logoff script by selecting the Start button, select Dani, and then select Sign out.

    - Notice that the sign off process will be interrupted as the logoff script runs. A Command Prompt window will open, followed by a Notepad window that displays the contents of the logoff file.

21. Close Notepad.

    - This will allow the sign off process to complete, returning you to the time and date screen saver.

#### Check your work

Confirm that you defined a logon script policy for an OU.

Confirm that you defined a logoff script policy for an OU.

Confirm that you configured a GPO to allow a logoff script to display instructions as they run.

Confirm that you tested the logon script on an OU member client.

Confirm that you tested the logoff script on an OU member client.
