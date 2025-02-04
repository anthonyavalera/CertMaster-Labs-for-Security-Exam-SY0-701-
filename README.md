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

- PowerShell
- Computer Management
- Event Viewer

## Steps

### Configure centralized logging

Centralized logging is an essential service of modern security management. Having logs duplicated to a single location facilitates analysis and backup operations. In this exercise, you will configure centralized logging on Windows systems.

1. Select the DC10 VM. Send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

    - The DC10 virtual machine will be used as the collector and the MS10 virtual machine will be used as the logging source.

2. Minimize or close Server Manager if it appears. It will not be used in this lab.

3. Select Type here to search from the taskbar, type powershell, right-click Windows PowerShell from the results, then select Run as administrator.

4. Select Yes on the User Account Control window.

5. Select the empty area of the Administrator: Windows PowerShell console, then select the   below to paste the PowerShell script into the VM:

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

    - This PowerShell script is used to change the default setting of the WinRM listener from empty to *. The default setting acts as a deny-all, while the asterisks act like an accept-all.

6. In the Administrator: Windows PowerShell console, enter wecutil qc.

7. When prompted by This service startup mode will be changed to Delay-Start. Would you like to proceed (Y- yes or N- no)? enter Y.

8. You should see the result message of Windows Event Collector service was configured successfully.

    - You have now enabled the Windows Event Collector service on DC10 (the collector).

9. Close the Administrator: Command Prompt window.

10. Connect to the MS10 virtual machine. Send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

    - Jaime is a member of the Domain Admins group. So, this user account is an administrator on the MS10 system.

11. Restart MS10 by selecting the Start menu, select Power, select Restart, then select Continue to label the restart as Other (Unplanned).

    - This restart will ensure the VM is properly logged into the domain and the GPO settings defined by the previous script are enforced.

12. Once restarted, connect to MS10, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

13. Minimize or close Server Manager if it appears. It will not be used in this lab.

14. Select Type here to search from the taskbar, type PowerShell, right-click Windows PowerShell from the results, and then select Run as administrator.

15. Select Yes on the User Account Control window.

16. In the PowerShell console, enter Set-NetFirewallRule -DisplayGroup "Remote Event Log Management" -Enabled True -Profile Domain.

17. In the PowerShell console, enter Set-NetFirewallRule -DisplayGroup "Remote Event Monitor" -Enabled True -Profile Domain.

    - You have now enabled Remote Event Log Management and Remote Event Monitor through the Windows Defender Firewall using the PowerShell cmdlet Set-NetFirewallRule.

18. In the PowerShell console, enter winrm quickconfig.

    - You should see the message: WinRM service is already running on this machine. WinRM is already set up for remote management on this computer.

    - If you see the message: WinRM is not set up to receive requests on this machine. The following changes must be made: Start the WinRM service. Set the WinRM service type to delayed auto start., then expand the guidance and perform the steps.
      Expand this hint for guidance.
        a. At the prompt Make these changes [y/n] enter y.
        b. A message should be displayed indicating that the WinRM service type was changed successfully and that it is started. However, there may be a need to create a firewall exception. If prompted to make this change, enter y.
        c. You should see a message stating WinRM has been updated for remote management. WinRM firewall exception enabled.
19. Close the PowerShell console.

20. Right-click the Start menu, select Computer Management.

21. In the Computer Management window, select Local Users and Groups, which is located under System Tools.

22. In the middle pane, double-click Groups.

23. Double-click the Event Log Readers groups.

24. On the Event Log Readers Properties window, select Add.

25. On the Select Users, Computers, Service Accounts, or Groups window, select Object Types….

26. On the Object Types window, select to enable the Computers checkbox, and then select OK.

27. On the Select Users, Computers, Service Accounts, or Groups window, in the Enter the object names to select field, enter DC10, and then select OK.

28. The Event log Readers Properties window should now show that structureality\DC10 is a member.

29. Select OK to close the Event log Readers Properties window.

30. Close the Computer Management window.

31. Restart MS10 by selecting the Start menu, select Power, select Restart, then select Continue to label the restart as Other (Unplanned).

32. Once the MS10 virtual machine restarts, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

33. Once you see the MS10 desktop appear, leave the MS10 system as is and return to DC10.

34. Switch back to the DC10 VM. If needed, send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

  Now, you must configure an Event Viewer subscription to pull recorded event records from the source (i.e., MS10).

35. Select Type here to search from the taskbar, enter Event and then select Event Viewer.

36. Maximize the Event Viewer window.

37. On the Event Viewer window, select Subscriptions in the left pane.

38. In the right pane, select Create Subscription….

39. On the Subscription Properties window, enter Logs from MS10 in the Subscription name field.

40. Leave the Destination Log field set to the default of Forwarded Events.

41. Select the Collector Initiated radio button and then select Select Computers.

    - A collector-initiated setup for centralized logging means that the collector system pulls log information from the source system. It is also possible to set up centralized logging to be source computer initiated. In that configuration, the source computer pushes log updates to the collector system.

42. On the Computers window, select Add Domain Computers.

43. On the Select Computer window, enter MS10 in the Enter the object name to select field, and then select OK.

44. You are returned to the Computers window, which should now display the name MS10.ad.structureality.com, select Test.

45. You should see a message stating Connectivity test succeeded, select OK to close this message window.

46. Select OK to close the Computers window.

47. On the Subscription Properties window, select Select Events….

48. On the Query Filter window, select Last 24 hours from the Logged: pull-down list.

49. Select all five of the checkboxes in the Event level: area, which are Critical, Warning, Verbose, Error, and Information.

50. Select the By log radio button, select the down arrow of the pull-down list, select the Windows Logs checkbox from the pull-down list, and then select somewhere outside the pull-down list to close it. The result of this selection should be a list of the Windows logs in the field showing Application, Security, Setup, System, Forwarded Events.

51. Leave all other settings at their defaults, and then select OK to close the Query Filter window and return to the Subscription Properties window.

52. Select OK to close the Subscription Properties window.

53. You should now see the Logs from MS10 subscription in the list of subscriptions.

54. Right-click the Logs from MS10 subscription, and then select Runtime Status.

55. The Subscription Runtime Status window should show that it is Active. Select Close.

56. In the left pane of Event Viewer, select the arrow beside Windows Logs to expand its contents.

57. Select Forwarded Events.

58. If the center pane remains empty after 10 seconds, select Refresh from the right pane.

59. If the center pane still remains empty, wait a minute or two, and then select Refresh from the right pane.

    - Once the subscription is listed as Active, it can take a few minutes for events to be pulled from the source computer to the collecting computer. If no events are showing in the Forwarded Events log after five to ten minutes, you can wait longer or move on to the next exercise and return later.

60. Once events are displayed in the Forwarded Events log, they will be updated regularly through regular pollings (i.e., queries) of the source system by the collector system.

61. Select any event. View the General tab for information about the selected event.

    - The Windows Event Viewer Subscription implements the concept of centralized or collected logging. An Event Viewer Subscription can be configured to pull logged event records from any member of the domain. Also, any member of a domain can be configured as the collecting system.

#### Check your work

Confirm that you enabled the Windows Event Collector service on the source computer.

Confirm that you configured the firewall for remote log access on the source computer.

Confirm that you enabled the Windows Remote Management service on the collector computer

Confirm that you created an Event Viewer Subscription.
