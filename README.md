# Assisted Lab: Implementing Allow Lists and Deny Lists

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

1.1 Compare and contrast various types of security controls.
1.3 Explain the importance of change management processes and the impact to security.
2.5 Explain the purpose of mitigation techniques used to secure the enterprise.

### Tools Used

- Windows PowerShell
- AppLocker

## Steps

### Configure application control policies by using local group policy

Often users have the ability to execute applications or commands that are outside of their assigned work responsibilities. It is in the best security interest of an organization to implement the principle of least privilege by restriction execution abilities to only those applications necessary for a worker's job tasks. AppLocker is a native feature of Windows that can be used to set deny (and allow) restrictions on executables.

1. Sign in to the PC10 virtual machine, send Ctrl+Alt+Delete, then sign in as Jaime using Pa$$w0rd as the password.

2. Select Type here to search from the taskbar, type regedit, then select Registry Editor from the results.

3. Select Yes on the User Account Control window.

4. Once the Registry Editor window is visible. Close it.

    This confirms that you are currently able to open the Registry Editor.

5. Select Type here to search from the taskbar, type powershell, right-click Windows PowerShell from the results, then select Run as administrator.

6. Select Yes on the User Account Control window.

7. Enter the following command to start the Application Identification service:

    - net start appidsvc

8. Select Type here to search from the taskbar, type secpol.msc, then select secpol.msc from the results.

    The Local Security Policy window should be displayed.

9. Maximize the Local Security Policy window and adjust the left pane to right pane divider by using the click-hold-drag-release method to move the divider to about one-third the width of the screen from the left. This will allow you to display the full names of the items in the left pane.

10. In the left pane, select the arrow to expand Application Control Policies.

11. Select the arrow to expand AppLocker then select AppLocker.

12. In the right pane, select Configure rule enforcement in the Configure Rule Enforcement section.

13. On the AppLocker Properties window, mark all 4 (four) of the Configured checkboxes, then verify that Enforce rules is selected in all 4 (four) of the pull-down lists, select Apply, then select OK.

    This operation enables AppLocker rule enforcement. Next, you will create an AppLocker executable rule to deny Regedit execution based on Path.

14. In the left pane, select Executable Rules

15. In the right pane, right-click the empty space and then select Create New Rule….

16. On the Create Executable Rule window, on the Before You Begin page, select Next.

17. On the Permissions page, under Action, select Deny, leave the User or group: value set at Everyone, and then select Next.

18. On the Conditions page, select Path, and then select Next.

19. On the Path page, under Path, type %WINDIR%\regedit.exe, then Create.

20. If an AppLocker pop-up window is displayed regarding creating default rules, select Yes.

    - A Path rule simply denies the application from running based on the name and location of the file. If the file is moved or renamed, it will execute. The Hash rule generates a hash of the file, so even if the file is renamed or moved, it will still not execute. Hash rules use more overhead but are more restrictive.

21. Switch back to the PowerShell window.

22. Force a Group Policy update by entering the following command:

    - gpupdate /force
      
    Wait for the messages indicating that both the Computer Policy and User Policy updates have completed.

23. Enter regedit to test the AppLocker policy enforcement from the PowerShell CLI.

    You should see an error indicating the program failed to run since it is blocked by group policy.

24. Test the AppLocker policy enforcement from the GUI. Select Type here to search from the taskbar, type regedit, then select Registry Editor from the results.

    A notification window should appear stating "This app has been blocked by your system administrator".

25. Select Close to close the notification window.

26. Leave the Local Security Policy and PowerShell windows open.

#### Check your work

Confirm that you created an AppLocker Executable rule based on path.

### Control an application using hashing

In this exercise, you will configure an Applocker restriction to block the execution of Firefox based on its executable's hash.

1. Connect to the PC10 virtual machine, and if needed, send Ctrl+Alt+Delete, then sign in as Jaime using Pa$$w0rd as the password.

2. Return to the Local Security Policy window.

    THe Application Control Policies section in the left pane should still be expanded showing AppLocker and its sub-elements, including Executable Rules.

3. In the left pane, select Executable Rules.

4. In the right pane, right-click and then select Create New Rule.

5. On the Create Executable Rule window, on the Before You Begin page, select Next.

6. On the Permissions page, under Action, select Deny and then select Next.

7. On the Conditions page, select File hash, and then select Next.

8. On the File Hash page, select Browse Files….

9. Select This PC in the left pane, then double-click Local Disk (C:) in the right pane.

10. Double-click Program Files, then double-click Mozilla Firefox.

11. Select firefox and then select Open.

12. You are returned to the File Hash page, select Create.

    - You should now see the firefox.exe | File Hash Deny rule.

    - A Path rule simply denies the application from running based on the name and location of the file. If the file is moved or renamed, it will execute. The Hash rule generates a hash of the file so even if the file is renamed or moved, it will still not execute. Hash rules use more overhead but are more restrictive.

13. Switch back to the PowerShell window.

14. Force a Group Policy update by entering the following command:

    - gpupdate /force
      
    Wait for the messages indicating that both the Computer Policy and User Policy updates have completed.

15. Select Type here to search from the taskbar, type Firefox, then select Firefox from the results

    A notification window should appear stating "This app has been blocked by your system administrator".

16. Select Close to close the notification window.

#### Check your work

Confirm that you created an AppLocker Executable rule based on file hash.
