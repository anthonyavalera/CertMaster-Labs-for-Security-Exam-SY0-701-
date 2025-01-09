# Assisted Lab: Implementing Allow Lists and Deny Lists

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

1.1 Compare and contrast various types of security controls.
1.3 Explain the importance of change management processes and the impact to security.
2.5 Explain the purpose of mitigation techniques used to secure the enterprise.

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

### Configure application control policies by using local group policy

Often users have the ability to execute applications or commands that are outside of their assigned work responsibilities. It is in the best security interest of an organization to implement the principle of least privilege by restriction execution abilities to only those applications necessary for a worker's job tasks. AppLocker is a native feature of Windows that can be used to set deny (and allow) restrictions on executables.

Sign in to the PC10 virtual machine, send Ctrl+Alt+Delete, then sign in as Jaime using Pa$$w0rd as the password.

Select Type here to search from the taskbar, type regedit, then select Registry Editor from the results.

Select Yes on the User Account Control window.

Once the Registry Editor window is visible. Close it.

This confirms that you are currently able to open the Registry Editor.

Select Type here to search from the taskbar, type powershell, right-click Windows PowerShell from the results, then select Run as administrator.

Select Yes on the User Account Control window.

Enter the following command to start the Application Identification service:

net start appidsvc
Select Type here to search from the taskbar, type secpol.msc, then select secpol.msc from the results.

The Local Security Policy window should be displayed.

Maximize the Local Security Policy window and adjust the left pane to right pane divider by using the click-hold-drag-release method to move the divider to about one-third the width of the screen from the left. This will allow you to display the full names of the items in the left pane.

In the left pane, select the arrow to expand Application Control Policies.

Select the arrow to expand AppLocker then select AppLocker.

In the right pane, select Configure rule enforcement in the Configure Rule Enforcement section.

On the AppLocker Properties window, mark all 4 (four) of the Configured checkboxes, then verify that Enforce rules is selected in all 4 (four) of the pull-down lists, select Apply, then select OK.

This operation enables AppLocker rule enforcement. Next, you will create an AppLocker executable rule to deny Regedit execution based on Path.

In the left pane, select Executable Rules

In the right pane, right-click the empty space and then select Create New Rule….

On the Create Executable Rule window, on the Before You Begin page, select Next.

On the Permissions page, under Action, select Deny, leave the User or group: value set at Everyone, and then select Next.

On the Conditions page, select Path, and then select Next.

What are the Applocker primary conditions for enforcing or triggering a rule? (Select all that apply)

Path
File hash
File size
Date and time
Publisher
On the Path page, under Path, type %WINDIR%\regedit.exe, then Create.

If an AppLocker pop-up window is displayed regarding creating default rules, select Yes.

A Path rule simply denies the application from running based on the name and location of the file. If the file is moved or renamed, it will execute. The Hash rule generates a hash of the file, so even if the file is renamed or moved, it will still not execute. Hash rules use more overhead but are more restrictive.

Switch back to the PowerShell window.

Force a Group Policy update by entering the following command:

gpupdate /force
Wait for the messages indicating that both the Computer Policy and User Policy updates have completed.

Enter regedit to test the AppLocker policy enforcement from the PowerShell CLI.

You should see an error indicating the program failed to run since it is blocked by group policy.

Test the AppLocker policy enforcement from the GUI. Select Type here to search from the taskbar, type regedit, then select Registry Editor from the results.

A notification window should appear stating "This app has been blocked by your system administrator".

Select Close to close the notification window.

Leave the Local Security Policy and PowerShell windows open.

Check your work
Confirm that you created an AppLocker Executable rule based on path.

### Control an application using hashing

In this exercise, you will configure an Applocker restriction to block the execution of Firefox based on its executable's hash.

Connect to the PC10 virtual machine, and if needed, send Ctrl+Alt+Delete, then sign in as Jaime using Pa$$w0rd as the password.

Return to the Local Security Policy window.

THe Application Control Policies section in the left pane should still be expanded showing AppLocker and its sub-elements, including Executable Rules.

In the left pane, select Executable Rules.

In the right pane, right-click and then select Create New Rule.

On the Create Executable Rule window, on the Before You Begin page, select Next.

On the Permissions page, under Action, select Deny and then select Next.

On the Conditions page, select File hash, and then select Next.

On the File Hash page, select Browse Files….

Select This PC in the left pane, then double-click Local Disk (C:) in the right pane.

Double-click Program Files, then double-click Mozilla Firefox.

Select firefox and then select Open.

You are returned to the File Hash page, select Create.

You should now see the firefox.exe | File Hash Deny rule.

A Path rule simply denies the application from running based on the name and location of the file. If the file is moved or renamed, it will execute. The Hash rule generates a hash of the file so even if the file is renamed or moved, it will still not execute. Hash rules use more overhead but are more restrictive.

Which of the following statements are true in regards to AppLocker rules? (Select all that apply).

A path rule denies execution if the file is renamed.
A hash rule denies execution if the file is renamed.
A path rule denies execution if the file is moved to another directory.
A hash rule denies execution if the file is moved to another directory.
Switch back to the PowerShell window.

Force a Group Policy update by entering the following command:

gpupdate /force
Wait for the messages indicating that both the Computer Policy and User Policy updates have completed.

Select Type here to search from the taskbar, type Firefox, then select Firefox from the results

A notification window should appear stating "This app has been blocked by your system administrator".

Select Close to close the notification window.

Check your work
Confirm that you created an AppLocker Executable rule based on file hash.
