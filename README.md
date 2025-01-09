# ADAPTIVE LAB: Using a Playbook

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

1.4 Explain the importance of using appropriate cryptographic solutions.
2.4 Given a scenario, analyze indicators of malicious activity.
4.3 Explain various activities associated with vulnerability management.
4.4 Explain security alerting and monitoring concepts and tools.
4.8 Explain appropriate incident response activities.
4.9 Given a scenario, use data sources to support an investigation.
5.1 Summarize elements of effective security governance.

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

### SIEM, SOAR, and Playbooks

The organization's SEIM solution has detected a significant increase in CPU consumption on the PC10 workstation. The level of CPU activity has been at or near 100%, which is abnormal for the PC10 system. Under normal circumstances, the SOAR solution would respond to and resolve the abnormal event automatically. However, the SOAR system is currently offline due to a recent reconfiguration and update failure. Therefore, as a security professional, you have been tasked with responding manually to this situation.

Fortunately, a pre-crafted playbook will guide you through the manual response activities. An incident response (IR) consulting group wrote the organization's library of playbooks. The IR consulting group was given broad parameters for crafting the playbooks. This has resulted in playbooks with flexibility and support for a wide range of knowledge, skill, and experience levels for those needing to use them to respond to incidents. You will work through the playbook explicitly designed to deal with high CPU consumption by rogue processes.

A playbook is a checklist of actions to perform to detect and respond to a specific type of incident.

There are several primary steps or phases in this playbook:

Investigate the high CPU usage and determine the rogue process's name.
Terminate the offending process.
Hash the file associated with the rogue process.
Perform an online malware analysis using the hash value of the suspicious file.
Determine the owner of the suspicious file.
Archive the suspicious file into a zip container along with a file of its hash value.
Copy the zip archive of the suspicious file to a quarantine system.
Remove the suspicious file from the affected system(s).
Fill out an incident report and submit it to the SOC for review.
For each of these steps, there are several options to select from. The playbook steps offer CLI (command line interface) solutions, GUI (graphical user interface) choices, or even third-party utility methods. While most of the operations use native tools, some reference use of tools from third-parties. All of the tools referenced in the playbooks have been pre-installed.

The lab is designed so you can choose your own options for each step of the overall playbook procedure. You are welcome to repeat the entire lab and make other choices, or you can work through the various choices of each playbook step before moving forward. However, there may be a need to reset the system or implement a work around to use an alternate playbook step choice. These will be defined for you at the end of each playbook step before the Check your work section.

A playbook is a common example of responsive controls. These are controls that serve to direct corrective actions that need to be enacted after an incident has been confirmed. In a Security Operations Center (SOC), responsive controls might include several very well-defined actions to be taken by a security professional after identifying a specific issue.

In this introductory exercise, you will log into PC10 and initiate the rogue process.

This is a necessary step to simulate the persistent execution of a rogue process.

Connect to the PC10 virtual machine. Send Ctrl+Alt+Delete and sign in as Jaime using Pa$$w0rd as the password.

Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.

Enter C:\LABFILES\Playbook-Lab.ps1.

There may be a brief presentation of an empty Windows PowerShell console while the process starts.

If prompted about allowing an execution exception for the script, type Y, then press Enter.

Close this Windows PowerShell console.

If you fail to close the Windows PowerShell console, the rogue process will be a sub-process of a PowerShell process.

The rogue process, which is the focus of this lab, should now be running.

The rogue process will immediately begin to consume most of the CPU. This will cause the system to be sluggish. You should be able to complete the initial playbook steps (where you will terminate the process) but be patient for the interface to respond to you.

Check your work
Confirm that you signed into PC10.
Confirm that you initiated the rogue process.

### Investigate High CPU usage

Playbook Step #1
The first step of the High-CPU IR Playbook is:


Investigate the high CPU usage and determine the rogue process's name.
In this High-CPU IR Playbook step, you will determine which rogue process is consuming most of the CPU's resources.

Make a selection of the method to use to accomplish this initial task. The method options are:

GUI - using the Windows Task Manager
CLI - using the CLI Command Prompt wmic utility
Third-party - using the Sysinternals GUI tool Process Manager
The rogue process is configured to run for 15 minutes and then terminate automatically. If you do not see an "unknown" process consuming most of the CPU, then re-launch the rogue process. If needed, you can re-launch the rogue process by expanding the following hint:


Expand this hint for guidance.
Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.
Enter C:\LABFILES\Playbook-Lab.ps1.
There may be a brief presentation of an empty Windows PowerShell console while the process starts.
If prompted about allowing an execution exception for the script, type Y, then press Enter.
Close this Windows PowerShell console.

If you fail to close the Windows PowerShell console, the rogue process will be a sub-process of a PowerShell process.
You can review the offered methods using the pull-down list below before making a final selection to work through.


GUI

Security Orchestration, Automation, and Response (SOAR) is a security solution whose purpose is to scan security and threat intelligence data collected from multiple sources within the enterprise and then analyze it using various techniques. A SOAR can also assist with provisioning tasks, such as creating and deleting user accounts, making shares available, or launching VMs from templates. The SOAR will use technologies such as cloud and SDN/SDV APIs, orchestration tools, and cyber threat intelligence (CTI) feeds to integrate the different systems it manages. It will also leverage technologies such as automated malware signature creation and user and entity behavior analytics (UEBA) to detect and identify threats. The automated actions performed by a SOAR are to be documented in runbooks. However, when the SOAR fails to operate properly, security personnel can use a playbook to perform manually the actions that the SOAR would have automated.

...less
Use GUI Task Manager
Select Type here to search from the taskbar, type task, then select Task Manager from the results.

The Task Manager window should be displayed in details view with a menu bar and several tabs.

If the Task Manager is not in the details view, select More Details to switch to the details view.

Select the CPU column to sort the processes by their CPU consumption.

If the CPU percentages at the top of the column are 0%, then select the CPU column header again to reverse the sort order.

If the most CPU-consuming process is Windows PowerShell, you failed to close the Windows PowerShell console you used to launch the rogue process. You must expand the Windows PowerShell process to view its children or sub-processes or close the Windows PowerShell console.

Determine the name of the process that is consuming most of the CPU. Enter the name of this process as displayed in Task Manager into the field below:

High CPU process name: 

Type in the process name exactly as shown, including duplicating the capitalization.

Press Enter on your keyboard after you type in the value or click out of the text box.

Leave the Task Manager open. You may use it in a later playbook step.

You have completed this playbook step using the GUI Task Manager.

If you want to work through a different method for this playbook step, select another method and perform those steps.

However, if the high CPU-consuming process is no longer active, you can re-launch the rogue process by expanding the following hint:


Expand this hint for guidance.
Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.
Enter C:\LABFILES\Playbook-Lab.ps1.
If prompted about allowing and execution exception for the script, type Y, then press Enter.
Close this Windows PowerShell console.
Check your work

Select the Score button to validate this task:

Confirm that you determined the process's name that consumes most of the CPU resource on PC10.

### Terminate the offending process

Playbook Step #2
The next step of the High-CPU IR Playbook is:


Terminate the offending process.
In this High-CPU IR Playbook step, you will terminate the rogue process named <HighCPUName>.

Make a selection of the method to use to accomplish this task. The method options are:

GUI - using the Windows Task Manager
CLI-CP - using taskkill from a Command Prompt
CLI-PS - using Stop-Process from a PowerShell console.
Third-party-CLI - using the Sysinternals CLI tool pskill.
Third-party-GUI - using the Sysinternals GUI tool Process Manager.
The rogue process is configured to run for 15 minutes and then terminate automatically.

If needed, you can re-launch the rogue process by expanding the following hint:


Expand this hint for guidance.
Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.
Enter C:\LABFILES\Playbook-Lab.ps1.
If prompted about allowing and execution exception for the script, type Y, then press Enter.
Close this Windows PowerShell console.
You can review the offered methods using the pull-down list below before making a final selection to work through.


GUI

Incident response playbooks are an invaluable tool for organizations to quickly and efficiently respond to security incidents. With an incident response playbook, organizations can define the steps they need to take to respond to a security incident, such as the specific roles, processes, and procedures that security staff must follow. Incident response playbooks can also guide communication with stakeholders and the public, as well as how to gather evidence and determine the incident's root cause. Oftentimes, the playbook is just that—a physical book a security professional uses in response to an incident. Using a physical book ensures its availability during a wide-scale incident. In a highly secure environment, it also ensures attackers do not digitally exfiltrate the IR capabilities.

...less
Use GUI Task Manager
Return to the Task Manager, which may have been left open from a previous playbook activity.

If the Task Manager is not open, select Type here to search from the taskbar, type task, then select Task Manager from the results.

Right-click the <HighCPUName> process, then select End Task.

The <HighCPUName> process should no longer be visible in the list of processes in Task Manager.

If there are multiple instances of the <HighCPUName> process, then you will need to kill each one.

Close Task Manager.

You have completed this playbook step using the GUI Task Manager.

If you want to work through a different method for this playbook step, select another method and perform those steps.

However, since you have terminated the CPU-consuming rogue process, you must re-launch it by expanding the following hint:


Expand this hint for guidance.
Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.
Enter C:\LABFILES\Playbook-Lab.ps1.
If prompted about allowing and execution exception for the script, type Y, then press Enter.
Close this Windows PowerShell console.
Check your work

Select the Score button to validate this task:

Confirm that you terminated the rogue process, consuming most of the CPU resource.

### Hash the suspicious file

Playbook Step #3
The next step of the High-CPU IR Playbook is:


Hash the file associated with the rogue process.
In this High-CPU IR Playbook step, you will locate the file associated with the rogue process named <HighCPUName> and calculate a SHA256 hash of the suspicious file.

Make a selection of the method to use to accomplish these tasks. The method options are:

CLI-CP - using CLI Command Prompt tool certutil
CLI-PS - using CLI PowerShell cmdlet Get-FileHash
Third-party - using the Sysinternals CLI tool sigcheck
You can review the offered methods using the pull-down list below before making a final selection to work through.


CLI-CP

The most effective incident response playbooks are tailored to an organization's specific security needs and provide detailed guidance on responding to various security incidents. For example, a playbook may contain detailed instructions on responding to a ransomware attack or a data breach. Additionally, the playbook should include guidance on taking the necessary steps to contain the incident, such as isolating affected systems and measures to ensure the incident is fully resolved.

...less
Use CLI Command Prompt tool certutil
Return to the Command Prompt window, which may have been left open from a previous playbook activity.

If the Command Prompt window is not open, select Type here to search from the taskbar, enter cmd, right-click Command Prompt from the results, then select Run as administrator. Then, select Yes on the User Account Control window.

Enter cd c:\ && dir /s <HighCPUName>.exe.

This command changes the working or current directory to the root of drive C:\, then searches all sub-folders for the file <HighCPUName>.exe.

There should be two results indicating that <HighCPUName>.exe is located in both c:\Users and c:\Program Files\JAM Software\HeavyLoad\. For this lab, assume the only result is in c:\Users.

This command may take up to 1 minute to complete as it is searching the entire drive for the file. You can terminate it using CTRL+C.

The c:\Program Files\JAM Software\HeavyLoad\<HighCPUName>.exe was copied into c:\Users\ for use in this playbook lab. The original file location needs to be left unchanged. This will allow you to reset the lab in playbook step 8, where you will be removing the c:\Users\<HighCPUName>.exe from the system.

Enter cd c:\Users.

This command changes the working directory to c:\Users.

Enter certutil -hashfile "c:\Users\<HighCPUName>.exe" SHA256 > c:\Users\<HighCPUName>-Hash.txt.

This command will calculate the SHA256 hash of the suspicious executable and save it in an output file.

Enter type <HighCPUName>-Hash.txt

This command displays the contents of the output file, which is the SHA256 hash of the suspicious executable.

Leave the Command Prompt window open. You may use it in a later playbook step.

You have completed this playbook step using the CLI Command Prompt tool certutil.

If you want to work through a different method for this playbook step, select another method and perform those steps.

Keep in mind that using the same output filename will be overwriting the file's contents each time you use a different hashing method or technique.

Check your work

Select the Score button to validate this task:

Confirm that you captured the SHA256 hash of the suspicious file.

### Online Malware Scan

Playbook Step #4
The next step of the High-CPU IR Playbook is:


Perform an online malware analysis using the hash value of the suspicious file.
In this High-CPU IR Playbook step, you will perform a malware evaluation of the hash of the suspicious file using an online malware scanning service.

Make a selection of the method to use to accomplish this task. The method options are:

Hybrid Analysis
MetaDefender
VirusTotal
You can review the offered methods using the pull-down list below before making a final selection to work through.


VirusTotal

When a playbook utilizes a high degree of automation from a SOAR system, it can be referred to as a runbook, though the terms are also widely used interchangeably. A runbook aims to automate as many playbook stages as possible while incorporating clearly defined interaction points for human analysis. These interaction points should present contextual information and guidance needed for an analyst to make a quick, informed decision about the best way to proceed with incident mitigation. For example, a runbook may use integrations for cloud-based email platforms and antimalware solutions. The runbook may take email attachments from user emails and submit them to an online malware detection engine. Suppose such a service identifies the file as malicious. In that case, the SOAR can provide a new custom detection signature to the antimalware software to locate and block any other instances of the malware.

...less
Use VirusTotal
The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser.

On your local computer, open another tab in your current browser or open a new browser.

Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

In your local browser's address bar, enter https://www.virustotal.com/.

Select the URL, and it will be copied into your local clipboard. From there, you can paste this URL into the address bar of your local browser.

The VirusTotal website should be displayed.

Select the Search tab.

Type the following hash into the URL, IP address, domain, or file hash field, then press Enter on your keyboard.

705204f80158bfbf9216a19bc2cb25a392ff028ce028745ac16f543b6290ec81

Select the  to the left of the hash, and it will be copied into your local clipboard. From there, you can paste it into the field of the website.

The results of the online malware search will be displayed.

Close the tab in your local browser focused on VirusTotal.

You have completed this playbook step using the online malware analysis service of VirusTotal.

Since the malware hash scan results are indeterminate, you do not know if the suspicious file is benign or is a new unknown malicious file. Therefore, you will continue forward with the next playbook step.

To see actual results from this site, repeat the search using the hash of 82407eaf6437d6956f63e85b28c0ec6ca58d298a. This is the hash of ca_setup.exe, the installer for the password cracking and network attack tool Cain. Cain is considered by many to be a hacker tool and potentially malicious.

If you want to work through a different method for this playbook step, select another method and perform those steps.

Check your work

The online malware hash analysis provided what result?

The file associated with the hash is a rootkit.
The file associated with the hash is ransomware.
The file associated with the hash is a keystroke logger.
The file associated with the hash is unknown or is not known to be malware.
Confirm that you determined whether the suspicious file was known to be malicious.

### Determine the owner of the suspicious file

Playbook Step #5
The next step of the High-CPU IR Playbook is:


Determine the owner of the suspicious file.
In this High-CPU IR Playbook step, you will determine the owner of the suspicious file.

Make a selection of the method to use to accomplish this task. The method options are:

GUI - using File Explorer
CLI-CP - using CLI Command Prompt dir command
CLI-PS - using PowerShell cmdlet Get-ACL
You can review the offered methods using the pull-down list below before making a final selection to work through.


GUI

An incident response workflow is a classic example of a SOAR task defined within a playbook. A playbook contains a checklist of actions to respond to a specific event. A playbook should be made highly specific by including the query strings and signatures to detect a particular type of incident. A playbook may account for compliance concerns, such as breach notification requirements, including when and to whom notification must be made.

Use GUI File Explorer
Return to File Explorer, which may have been left open from a previous playbook activity.

Expand this hint if File Explorer is not open.
Select Type here to search from the taskbar, type file, then select File Explorer from the results.

The File Explorer window should be displayed.

In the left pane, expand This PC, and then select Local Disk (C:).

The contents of the root of drive C: should be displayed in the right pane.

In the right pane, double-click Users.

The contents of c:\Users should be displayed in the right pane.

Right-click <HighCPUName>, then select Properties.

By default, File Explorer does not display the file extensions for known file types. Therefore, instead of seeing HeavyLoad.exe, you only see HeavyLoad.

Select the Security tab on the HeavyLoad Properties window.

Select Advanced on the Security tab.

Look over the information on the Advanced Security Settings for HeavyLoad window.

Determine the file's owner and enter the name as spelled and capitalized into the field below.

<HighCPUName> file owner: 

Do not include the information within parentheses.

Press Enter on your keyboard after you type in the value or click out of the text box.

Select OK to close the Advanced Security Settings for HeavyLoad window.

Select OK to close the HeavyLoad Properties window.

Leave the File Explorer window open. You may use it in a later playbook step.

You have completed this playbook step using File Explorer.

If you want to work through a different method for this playbook step, select another method and perform those steps.

Check your work

Select the Score button to validate this task:

Confirm that you have determined the owner of the suspicious file.

### Archive the Suspicious File

Playbook Step #6
The next step of the High-CPU IR Playbook is:


Archive the suspicious file into a zip container along with a file and its hash value.
In this High-CPU IR Playbook step, you will create an archive containing the suspicious file and the hash output file.

Make a selection of the method to use to accomplish this task. The method options are:

GUI - using File Explorer
CLI-CP - using CLI Command Prompt tool tar
CLI-PS - using PowerShell cmdlet Compress-Archive
You can review the offered methods using the pull-down list below before making a final selection to work through.


GUI

When creating an incident response playbook, organizations should ensure they have the right level of detail and that all necessary stakeholders are involved, including security teams, IT staff, legal teams, and other personnel who may be involved in responding to the incident. Organizations should also ensure that the incident response playbook is updated as new threats and technologies emerge.

Use GUI File Explorer
Return to File Explorer, which may have been left open from a previous playbook activity.

Expand this hint if File Explorer is not open.
Select Type here to search from the taskbar, type file, then select File Explorer from the results.

The File Explorer window should be displayed.

In the left pane, select Local Disk (C:).

The contents of the root of drive C: should be displayed in the right pane.

In the right pane, double-click Users.

The contents of c:\Users should be displayed in the right pane.

Select <HighCPUName>.

By default, File Explorer does not display the file extensions for known file types. Therefore, instead of seeing HeavyLoad.exe, you only see HeavyLoad.

Hold down Shift on your keyboard, then select <HighCPUName>-Hash.

Right-click over <HighCPUName> while the two files are selected/highlighted, then select Send to >, then select Compressed (zipped) folder.

If you right-click over the <HighCPUName>-Hash file, then the archive file will have a filename of <HighCPUName>-Hash.zip instead of <HighCPUName>.zip.

Select Yes on the Compressed (zipped) Folders window, which asks to create the archive file on the desktop.

File Explorer does not have permission as a process to create a new file in the c:\Users folder. It also cannot be launched as the administrator.

In the left pane, select Desktop.

The contents of the Desktop should be displayed in the right pane. You should see <HighCPUName>. It should have an icon of a yellow file folder with a zipper and be labeled as Type of Compressed (zipped) Folder.

Use the click-hold-drag-release method to move <HighCPUName> to Local Disk (C:).

Select Continue on the Destination Folder Access Denied window.

The <HighCPUName> file should no longer be displayed in the Desktop folder.

In the left pane, select Local Disk (C:).

You should see <HighCPUName> in the right pane.

Leave File Explorer open. You may use it in a later playbook step.

You have completed this playbook step using File Explorer.

If you want to work through a different method for this playbook step, select another method and perform those steps.

However, you may need to delete the <HighCPUName>.zip file you have already created. Otherwise, the archive creation process may fail or prompt for overwriting permission. These issues can be avoided by deleting the existing file: from a Command Prompt or Windows PowerShell console, enter del c:\<HighCPUName>.zip to delete the file.

Check your work

Select the Score button to validate this task:

Confirm that you created an archive of the suspicious file and its hash file.

### Copy the archive to a quarantine system

Playbook Step #7
The next step of the High-CPU IR Playbook is:


Copy the zip archive of the suspicious file to a quarantine system.
In this High-CPU IR Playbook step, you will move the archive of the suspicious file to a quarantine system. In this exercise, the quarantine system will be the Kali VM.

Make a selection of the method to use to accomplish this task. The method options are:

NC - using Netcat and PowerShell
WinSCP - using WinSCP
SAMBA - Using SAMBA
You can review the offered methods using the pull-down list below before making a final selection to work through.


NC

Use Netcat and PowerShell
Select the KALI VM and sign in as root using Pa$$w0rd as the password.

Open a Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar

Enter mkdir quarantine to create a directory.

Enter cd quarantine to change into the new folder.

Enter nc -l -p 1234 > <HighCPUName>.zip.

This command uses Netcat to open a listening port to receive a connection from PC10. Once the connection is established, the data transferred from PC10 will be saved into a file named <HighCPUName>.zip on Kali.

Switch back to the PC10 virtual machine. If needed, send Ctrl+Alt+Delete and sign in as Jaime using Pa$$w0rd as the password.

Return to the Windows PowerShell console, which may have been left open from a previous playbook activity.

If the Windows PowerShell console is not open, select Type here to search from the taskbar, type powershell, right-click Windows PowerShell from the results, select Run as administrator, then, select Yes on the User Account Control window.

The Windows PowerShell console should be displayed.

Enter the following code into the Windows PowerShell console:

$filePath = "C:\HeavyLoad.zip"; $destination = "10.1.16.66"; $port = 1234; $tcpConnection = New-Object System.Net.Sockets.TcpClient($destination, $port); $netStream = $tcpConnection.GetStream(); $buffer = [System.IO.File]::ReadAllBytes($filePath); $netStream.Write($buffer, 0, $buffer.Length); $netStream.Close(); $tcpConnection.Close(); $?;

This code will send the contents of the C:\HeavyLoad.zip file to the listening service on Kali.

The final line of the code "&?;" will be left at the Windows PowerShell prompt. Press Enter to submit this last line of code. It will present a result of True.

This code uses PowerShell functions to duplicate the data transfer capability of Netcat. This mechanism can be used from a Windows PowerShell console without needing Netcat present on the local system.

Switch back to the KALI VM and, if needed, sign in as root using Pa$$w0rd as the password.

Notice the Netcat command has completed, and you are returned to the # prompt.

Enter ls -l to view the long list of the current directory.

You should see the HeavyLoad.zip file is now present on the Kali system.

Enter unzip -t HeavyLoad.zip to test and verify the file transferred properly.

The result should indicate "No errors detected in compressed data of HeavyLoad.zip."

Switch back to the PC10 virtual machine. If needed, send Ctrl+Alt+Delete and sign in as Jaime using Pa$$w0rd as the password.

This VM switch back is needed to keep each playbook step transition consistent.

Leave the Windows PowerShell console open. You may use it in a later playbook step.

You have completed this playbook step using PowerShell functions and Netcat.

If you want to work through a different method for this playbook step, select another method and perform those steps.

However, you will need to execute the following command on Kali from the Terminal window: rm -rf /root/quarantine to delete the /root/quarantine directory and the HeavyLoad.zip file contained within that directory.

Check your work

Select the Score button to validate this task:

Confirm that you have copied the archived suspicious file to the quarantine system.

### Remove the suspicious file from the victim

Playbook Step #8
The next step of the High-CPU IR Playbook is:


Remove the suspicious file from the affected system(s).
In this High-CPU IR Playbook step, you will remove the suspicious file and related files from the victim system.

Make a selection of the method to use to accomplish this task. The method options are:

CLI-CP1 - using CLI Command Prompt del command
CLI-CP2 - using the Sysinternals CLI tool SDelete
GUI - using File Explorer and the Recycle Bin
You can review the offered methods using the pull-down list below before making a final selection to work through.


GUI

Use GUI w/ Recycle Bin
Return to File Explorer, which may have been left open from a previous playbook activity.

Expand this hint if File Explorer is not open.
Select Type here to search from the taskbar, type file, then select File Explorer from the results.

The File Explorer window should be displayed.

In the left pane, select Local Disk (C:).

The contents of the root of drive C: should be displayed in the right pane.

Right-click HeavyLoad, then select Delete from the fly-open menu.

Remember, File Explorer does not display the file extensions for known file types by default. Therefore, instead of seeing HeavyLoad.exe, you only see HeavyLoad.

The HeavyLoad archive file should no longer be visible.

Double-click Users to enter that directory.

Right-click HeavyLoad, then select Delete from the fly-open menu.

The HeavyLoad executable file should no longer be visible.

Right-click HeavyLoad-Hash, then select Delete from the fly-open menu.

The HeavyLoad-Hash text file should no longer be visible.

Close File Explorer.

Double-click Recycle bin on the Desktop.

The File Explorer will open focused on the Recycle Bin directory.

You should see the HeavyLoad files you deleted.

Select Recycle Bin Tools on the File Explorer toolbar, then select Empty Recycle Bin.

Select Yes on the Delete Multiple Items query window.

Notice the files are no longer displayed in the Recycle Bin directory. The files have been effectively deleted.

Using File Explorer to delete files will send them to the Recycle Bin. From the Recycle Bin, they can be restored. Once the Recycle Bin is emptied, the files will be deleted, but it is not a secure destruction of the files' contents. An undelete operation (using a third-party tool) can recover access to deleted files whose storage areas are not yet overwritten.

You have completed this playbook step using File Explorer and the Recycle Bin.

If you want to work through a different method for this playbook step, select another method and perform those steps.

However, the files were deleted that are necessary to repeat this playbook step. To recreate them, expand the following hint:


Expand this hint for guidance.
Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.
Enter copy "c:\Program Files\JAM Software\HeavyLoad\HeavyLoad.exe" c:\Users\.
Enter echo 705204f80158bfbf9216a19bc2cb25a392ff028ce028745ac16f543b6290ec81 > c:\Users\HeavyLoad-Hash.txt.
Enter Compress-Archive -Path "C:\Users\HeavyLoad.exe", "C:\Users\HeavyLoad-Hash.txt" -DestinationPath "C:\HeavyLoad.zip".
Close this Windows PowerShell console.

NOTE: This will not quite recreate the hash file exactly as the specific steps used in Playbook Step #3, but it is close enough for repeating Playbook Step #8, where you are just going to delete them again.
Check your work

Select the Score button to validate this task:

Confirm that you deleted the suspicious file, its hash file, and the archive file from PC10.

### Craft a Report about the Response

Playbook Step #9
The next step of the High-CPU IR Playbook is:


Fill out an incident report and submit it to the SOC for review.
In this High-CPU IR Playbook step, you will be instructed about crafting a report of the operations taken to resolve this security incident.

Now that you have completed the playbook's primary steps, you need to craft and file a report about the security incident response. Your report should include a summary of the High-CPU IR Playbook steps, along with the methods used and results obtained.

The High-CPU IR Playbook steps are:

Investigate the high CPU usage and determine the rogue process's name.
Terminate the offending process.
Hash the file associated with the rogue process.
Perform an online malware analysis using the hash value of the suspicious file.
Determine the owner of the suspicious file.
Archive the suspicious file into a zip container along with a file and its hash value.
Copy the zip archive of the suspicious file to a quarantine system.
Remove the suspicious file from the affected system(s).
Fill out an incident report and submit it to the SOC for review.
This type of report is often known as an AAR (After Action Report). It can also be referred to as a Lessons Learned or Post-Mortem report. The goal or purpose of this report is to document the activities performed, note any discrepancies or problems encountered, and glean information about where the process, playbook, toolset, or environment may need to be changed or improved.

Once the report is crafted, it should be submitted to your CISO for review.

Since this is only a simulated environment, there is no need for you to actually craft a report to complete this lab.

Check your work
Confirm that you understand the concept of crafting an AAR.
