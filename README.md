# Assisted Lab: Configuring System Monitoring

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

4.1 Given a scenario, apply common security techniques to computing resources.
4.4 Explain security alerting and monitoring concepts and tools.
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

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

### Configure centralized logging

Centralized logging is an essential service of modern security management. Having logs duplicated to a single location facilitates analysis and backup operations. In this exercise, you will configure centralized logging on Windows systems.

Select the DC10 VM. Send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

The DC10 virtual machine will be used as the collector and the MS10 virtual machine will be used as the logging source.

Minimize or close Server Manager if it appears. It will not be used in this lab.

Select Type here to search from the taskbar, type powershell, right-click Windows PowerShell from the results, then select Run as administrator.

Select Yes on the User Account Control window.

Select the empty area of the Administrator: Windows PowerShell console, then select the   below to paste the PowerShell script into the VM:

Import-Module GroupPolicy

# Get the cc-domain-default GPO object
$gpo = Get-GPO -Name "cc-domain-default"

# Get the Group Policy Preference registry key for WinRM
$winrmRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service"

# Set the value of the "IPv4Filter" registry value to "*"
Set-GPRegistryValue -Name $gpo.DisplayName -Key $winrmRegKey -ValueName "IPv4Filter" -Type String -Value "*"

# Refresh the Group Policy settings on the local computer
gpupdate /force
Press Enter on your keyboard for the last line to be executed. You should see the message "Updating policy…"

This PowerShell script is used to change the default setting of the WinRM listener from empty to *. The default setting acts as a deny-all, while the asterisks act like an accept-all.

Select the Score button to validate this task:

In the Administrator: Windows PowerShell console, enter wecutil qc.

When prompted by This service startup mode will be changed to Delay-Start. Would you like to proceed (Y- yes or N- no)? enter Y.

You should see the result message of Windows Event Collector service was configured successfully.

You have now enabled the Windows Event Collector service on DC10 (the collector).

Close the Administrator: Command Prompt window.

Connect to the MS10 virtual machine. Send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

Jaime is a member of the Domain Admins group. So, this user account is an administrator on the MS10 system.

Restart MS10 by selecting the Start menu, select Power, select Restart, then select Continue to label the restart as Other (Unplanned).

This restart will ensure the VM is properly logged into the domain and the GPO settings defined by the previous script are enforced.

Once restarted, connect to MS10, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

Minimize or close Server Manager if it appears. It will not be used in this lab.

Select Type here to search from the taskbar, type PowerShell, right-click Windows PowerShell from the results, and then select Run as administrator.

Select Yes on the User Account Control window.

In the PowerShell console, enter Set-NetFirewallRule -DisplayGroup "Remote Event Log Management" -Enabled True -Profile Domain.

In the PowerShell console, enter Set-NetFirewallRule -DisplayGroup "Remote Event Monitor" -Enabled True -Profile Domain.

You have now enabled Remote Event Log Management and Remote Event Monitor through the Windows Defender Firewall using the PowerShell cmdlet Set-NetFirewallRule.

Select the Score button to validate this task:

In the PowerShell console, enter winrm quickconfig.

You should see the message: WinRM service is already running on this machine. WinRM is already set up for remote management on this computer.

If you see the message: WinRM is not set up to receive requests on this machine. The following changes must be made: Start the WinRM service. Set the WinRM service type to delayed auto start., then expand the guidance and perform the steps.

Expand this hint for guidance.
Close the PowerShell console.

Right-click the Start menu, select Computer Management.

In the Computer Management window, select Local Users and Groups, which is located under System Tools.

In the middle pane, double-click Groups.

Double-click the Event Log Readers groups.

On the Event Log Readers Properties window, select Add.

On the Select Users, Computers, Service Accounts, or Groups window, select Object Types….

On the Object Types window, select to enable the Computers checkbox, and then select OK.

On the Select Users, Computers, Service Accounts, or Groups window, in the Enter the object names to select field, enter DC10, and then select OK.

The Event log Readers Properties window should now show that structureality\DC10 is a member.

Select OK to close the Event log Readers Properties window.

Close the Computer Management window.

Restart MS10 by selecting the Start menu, select Power, select Restart, then select Continue to label the restart as Other (Unplanned).

Once the MS10 virtual machine restarts, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

Once you see the MS10 desktop appear, leave the MS10 system as is and return to DC10.

Switch back to the DC10 VM. If needed, send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

Now, you must configure an Event Viewer subscription to pull recorded event records from the source (i.e., MS10).

Select Type here to search from the taskbar, enter Event and then select Event Viewer.

Maximize the Event Viewer window.

On the Event Viewer window, select Subscriptions in the left pane.

In the right pane, select Create Subscription….

On the Subscription Properties window, enter Logs from MS10 in the Subscription name field.

Leave the Destination Log field set to the default of Forwarded Events.

Select the Collector Initiated radio button and then select Select Computers.

A collector-initiated setup for centralized logging means that the collector system pulls log information from the source system. It is also possible to set up centralized logging to be source computer initiated. In that configuration, the source computer pushes log updates to the collector system.

On the Computers window, select Add Domain Computers.

On the Select Computer window, enter MS10 in the Enter the object name to select field, and then select OK.

You are returned to the Computers window, which should now display the name MS10.ad.structureality.com, select Test.

You should see a message stating Connectivity test succeeded, select OK to close this message window.

Select OK to close the Computers window.

On the Subscription Properties window, select Select Events….

On the Query Filter window, select Last 24 hours from the Logged: pull-down list.

Select all five of the checkboxes in the Event level: area, which are Critical, Warning, Verbose, Error, and Information.

Select the By log radio button, select the down arrow of the pull-down list, select the Windows Logs checkbox from the pull-down list, and then select somewhere outside the pull-down list to close it. The result of this selection should be a list of the Windows logs in the field showing Application, Security, Setup, System, Forwarded Events.

Leave all other settings at their defaults, and then select OK to close the Query Filter window and return to the Subscription Properties window.

Select OK to close the Subscription Properties window.

You should now see the Logs from MS10 subscription in the list of subscriptions.

Right-click the Logs from MS10 subscription, and then select Runtime Status.

The Subscription Runtime Status window should show that it is Active. Select Close.

In the left pane of Event Viewer, select the arrow beside Windows Logs to expand its contents.

Select Forwarded Events.

If the center pane remains empty after 10 seconds, select Refresh from the right pane.

If the center pane still remains empty, wait a minute or two, and then select Refresh from the right pane.

Once the subscription is listed as Active, it can take a few minutes for events to be pulled from the source computer to the collecting computer. If no events are showing in the Forwarded Events log after five to ten minutes, you can wait longer or move on to the next exercise and return later.

Once events are displayed in the Forwarded Events log, they will be updated regularly through regular pollings (i.e., queries) of the source system by the collector system.

Select any event. View the General tab for information about the selected event.

The Windows Event Viewer Subscription implements the concept of centralized or collected logging. An Event Viewer Subscription can be configured to pull logged event records from any member of the domain. Also, any member of a domain can be configured as the collecting system.

Check your work
Confirm that you enabled the Windows Event Collector service on the source computer.
Confirm that you configured the firewall for remote log access on the source computer.
Confirm that you enabled the Windows Remote Management service on the collector computer
Confirm that you created an Event Viewer Subscription.
