# Assisted Lab: Perform System Configuration Gap Analysis

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

- 1.2 Summarize fundamental security concepts.
- 3.2 Given a scenario, apply security principles to secure enterprise infrastructure
- 4.1 Given a scenario, apply common security techniques to computing resources.
- 4.4 Explain security alerting and monitoring concepts and tools.
- 5.1 Summarize elements of effective security governance.
   
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
drag & drop screenshots here or use imgur and reference them using imgsrc

Every screenshot should have some text explaining what the screenshot is about.

Example below.

*Ref 1: Network Diagram*

### Perform gap analysis

Gap analysis is the act of comparing the current configuration of a system with a template, configuration file, baseline, security framework, or settings documentation. This is an essential operation to discover the differences between the intended or expected configuration of a system and its actual operating configuration. In this exercise, you will perform a gap analysis.

1. Connect to the PC10 virtual machine, send Ctrl+Alt+Delete, select Other User, and sign in as .\admin with the password Pa$$w0rd.
   - Select the Type Text icon to enter the associated text into the virtual machine.
   - Since Jaime may be the default account, you must select Other user, then enter .\admin followed by Pa$$w0rd as the password.

2. Determine the build number for the Windows Server 2019 running in thePC10 virtual machine using winver. Select Type here to search from the taskbar, type winver, then select winver from the results.

3. The About Windows window should be displayed. Look at the second line, which reads “Version 1809 (OS Build 17763.4377)”.
- The Windows Server 2019 version number 1809 and the OS Build number 17763 are fixed for this challenge. If you perform these operations against other systems, you will need to match the version and build numbers to the baseline template files.

4. Select OK to close the About Windows window.

5. Select Type here to search from the taskbar, type powershell, right-click Windows PowerShell from the results, then select Run as administrator.
- Do no use Windows PowerShell ISE nor Windows PowerShell (X86).

6. Select Yes on the User Account Control window.

7. Enter copy D:\* c:\LABFILES

   This command copies PolicyAnalyzer.zip and Windows 10 Version 1809 and Windows Server 2019 Security Baseline.zip from the read-only removable media virtual optical disc     (i.e., D:) to C:\LABFILES.
   - If the command is successful, there will be no confirmation.
   - These two files are from the Microsoft Security Compliance Toolkit. The baseline file was selected based on the OS version and build number.
   - The Microsoft Security Compliance Toolkit includes the Policy Analyzer tool as well as numerous security configuration template files. Searching for "Microsoft       
     Security     Compliance Toolkit" will help you locate the download area on the Microsoft website where these items are hosted. They have been provided for you the 
     Student-Resources-     L01.ISO media.

8. Enter cd c:\LABFILES to change into the directory.

9. Enter ls to view the contents of the directory.
- You should see PolicyAnalyzer.zip and Windows 10 Version 1809 and Windows Server 2019 Security Baseline.zip in the list of files.
- "ls" is a Linux command (one of many) that are supported by Windows PowerShell. The "dir" will also display the directory contents.

10. Enter the following commands to extract the contents of the zip files into their own sub-directories:

- Expand-Archive -Path PolicyAnalyzer.zip
Expand-Archive -Path "Windows 10 Version 1809 and Windows Server 2019 Security Baseline.zip"
11. Enter the following command to open the Policy Analyzer application:

C:\LABFILES\PolicyAnalyzer\PolicyAnalyzer_40\PolicyAnalyzer.exe
12. The Policy Analyzer window should now be displayed. If needed, minimize the PowerShell window.

The Policy Analyzer window may appear behind the PowerShell window.

13. Maximize the Policy Analyzer window.

The operation buttons on the right side of the interface may not display fully when the window is not maximized.

14. At the bottom of the Policy Analyzer window, select the Policy Rule sets in field.

If you do not see Policy Rules sets in and Policy Definitions in, adjust your screen to a higher resolution and relaunch the Policy Analyzer.

15. On the Pick the folder containing the Policy Analyzer Policy Rules files window, in left pane select Local Disk (C:), in the right pane double-click LABFILES, double-click Windows 10 Version 1809 and Windows Server 2019 Security Baseline, double-click Documentation, then select Select Folder.

The Policy Analyzer window should now show several policy rule sets.

16. Perform a View/Compare of MSFT-Win10-v1809-RS5-WS2019-FINAL using Policy Analyzer by marking the MSFT-Win10-v1809-RS5-WS2019-FINAL checkbox, then selecting View / Compare.

The Policy Viewer window will be displayed, showing the various policy settings contained in the MSFT-Win10-v1809-RS5-WS2019-FINAL policy rule set.

This feature, View/Compare, shows the settings currently in the baseline security template file.

17. Scroll down the list and look at a few policy setting lines.

18. Scroll to the bottom of the list and locate the LockoutBadCount, which is 9th from the bottom.

What is the baseline value from the security template for the policy setting item of LockoutBadCount?

19. Also near the bottom, locate MinimumPasswordLength, which is 4th from the bottom.

What is the baseline value from the security template for the policy setting item of MinimumPasswordLength?

20. Close the Policy Viewer window.

21. Perform a Compare to Effective State of MSFT-Win10-v1809-RS5-WS2019-FINAL using Policy Analyzer by marking the MSFT-Win10-v1809-RS5-WS2019-FINAL checkbox, then selecting Compare to Effective State.

This feature, Compare to Effective State, performs a gap analysis between the baseline security template file and the current in-use values of the local operating system.

22. If a User Account Control window appears, select Yes.

23. The Policy Viewer window will be displayed, showing a comparison between the various policy settings contained in the MSFT-Win10-v1809-RS5-WS2019-FINAL policy rule set and the current operating system (labeled as “Effective state”).

24. Notice that many items are highlighted in yellow. These are where there are differences between the baseline file and the current effective state of the live operating system environment of PC10.

25. Scroll down to the bottom of the list.

26. Notice the Effective state value of LockoutBadCount is 0, and MinimumPasswordLength is 7.

Is the PC10 system in compliance with the security template based on the gap analysis results?

No
Yes
27. Close the Policy Viewer window.

Since you are using a security template from a third party, it is essential to understand that while the template's settings may be based on general security best practices and recommendations, they are not tuned specifically to your organization's risk profile or business goals. You will need to tailor and scope security configuration templates to your organization's specific needs and requirements.

Check your work
Confirm that you determined the version and build number of a Windows computer.
Confirm that you used the Policy Analyzer from Microsoft to view a security baseline template.
Confirm that you used the Policy Analyzer from Microsoft to perform a gap analysis by comparing a security baseline template to the effective state of a computer.
