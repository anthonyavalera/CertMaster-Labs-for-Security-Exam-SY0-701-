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

    - A playbook is a checklist of actions to perform to detect and respond to a specific type of incident.

There are several primary steps or phases in this playbook:

  1. Investigate the high CPU usage and determine the rogue process's name.
  2. Terminate the offending process.
  3. Hash the file associated with the rogue process.
  4. Perform an online malware analysis using the hash value of the suspicious file.
  5. Determine the owner of the suspicious file.
  6. Archive the suspicious file into a zip container along with a file of its hash value.
  7. Copy the zip archive of the suspicious file to a quarantine system.
  8. Remove the suspicious file from the affected system(s).
  9. Fill out an incident report and submit it to the SOC for review.

For each of these steps, there are several options to select from. The playbook steps offer CLI (command line interface) solutions, GUI (graphical user interface) choices, or even third-party utility methods. While most of the operations use native tools, some reference use of tools from third-parties. All of the tools referenced in the playbooks have been pre-installed.

- The lab is designed so you can choose your own options for each step of the overall playbook procedure. You are welcome to repeat the entire lab and make other choices, or you can work through the various choices of each playbook step before moving forward. However, there may be a need to reset the system or implement a work around to use an alternate playbook step choice. These will be defined for you at the end of each playbook step before the Check your work section.

- A playbook is a common example of responsive controls. These are controls that serve to direct corrective actions that need to be enacted after an incident has been confirmed. In a Security Operations Center (SOC), responsive controls might include several very well-defined actions to be taken by a security professional after identifying a specific issue.

In this introductory exercise, you will log into PC10 and initiate the rogue process.

- This is a necessary step to simulate the persistent execution of a rogue process.

1. Connect to the PC10 virtual machine. Send Ctrl+Alt+Delete and sign in as Jaime using Pa$$w0rd as the password.

2. Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.

3. Enter C:\LABFILES\Playbook-Lab.ps1.

  There may be a brief presentation of an empty Windows PowerShell console while the process starts.

4. If prompted about allowing an execution exception for the script, type Y, then press Enter.

5. Close this Windows PowerShell console.

    - If you fail to close the Windows PowerShell console, the rogue process will be a sub-process of a PowerShell process.

The rogue process, which is the focus of this lab, should now be running.

- The rogue process will immediately begin to consume most of the CPU. This will cause the system to be sluggish. You should be able to complete the initial playbook steps (where you will terminate the process) but be patient for the interface to respond to you.

#### Check your work

Confirm that you signed into PC10.

Confirm that you initiated the rogue process.

### Investigate High CPU usage

#### Playbook Step #1

The first step of the High-CPU IR Playbook is:

  1. Investigate the high CPU usage and determine the rogue process's name.

In this High-CPU IR Playbook step, you will determine which rogue process is consuming most of the CPU's resources.

Make a selection of the method to use to accomplish this initial task. The method options are:

    - GUI - using the Windows Task Manager
    - CLI - using the CLI Command Prompt wmic utility
    - Third-party - using the Sysinternals GUI tool Process Manager
- The rogue process is configured to run for 15 minutes and then terminate automatically. If you do not see an "unknown" process consuming most of the CPU, then re-launch the rogue process. If needed, you can re-launch the rogue process by expanding the following hint:
  Expand this hint for guidance.
    1. Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.
    2. Enter C:\LABFILES\Playbook-Lab.ps1.
There may be a brief presentation of an empty Windows PowerShell console while the process starts.
    3. If prompted about allowing an execution exception for the script, type Y, then press Enter.
    4. Close this Windows PowerShell console.

      If you fail to close the Windows PowerShell console, the rogue process will be a sub-process of a PowerShell process.
  
You can review the offered methods using the pull-down list below before making a final selection to work through.

- Security Orchestration, Automation, and Response (SOAR) is a security solution whose purpose is to scan security and threat intelligence data collected from multiple sources within the enterprise and then analyze it using various techniques. A SOAR can also assist with provisioning tasks, such as creating and deleting user accounts, making shares available, or launching VMs from templates. The SOAR will use technologies such as cloud and SDN/SDV APIs, orchestration tools, and cyber threat intelligence (CTI) feeds to integrate the different systems it manages. It will also leverage technologies such as automated malware signature creation and user and entity behavior analytics (UEBA) to detect and identify threats. The automated actions performed by a SOAR are to be documented in runbooks. However, when the SOAR fails to operate properly, security personnel can use a playbook to perform manually the actions that the SOAR would have automated.
- If you want to work through a different method for this playbook step, select another method and perform those steps.

  However, if the high CPU-consuming process is no longer active, you can re-launch the rogue process by expanding the following hint:
    Expand this hint for guidance.
      1. Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.
      2. Enter C:\LABFILES\Playbook-Lab.ps1.
      3. If prompted about allowing and execution exception for the script, type Y, then press Enter.
      4. Close this Windows PowerShell console.

#### Check your work

Confirm that you determined the process's name that consumes most of the CPU resource on PC10.

### Terminate the offending process

#### Playbook Step #2

The next step of the High-CPU IR Playbook is:

    2. Terminate the offending process.
    
In this High-CPU IR Playbook step, you will terminate the rogue process named <HighCPUName>.

Make a selection of the method to use to accomplish this task. The method options are:

    - GUI - using the Windows Task Manager
    - CLI-CP - using taskkill from a Command Prompt
    - CLI-PS - using Stop-Process from a PowerShell console.
    - Third-party-CLI - using the Sysinternals CLI tool pskill.
    - Third-party-GUI - using the Sysinternals GUI tool Process Manager.

- The rogue process is configured to run for 15 minutes and then terminate automatically.

If needed, you can re-launch the rogue process by expanding the following hint:
    Expand this hint for guidance.
        1. Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.
        2. Enter C:\LABFILES\Playbook-Lab.ps1.
        3. If prompted about allowing and execution exception for the script, type Y, then press Enter.
        4. Close this Windows PowerShell console.

You can review the offered methods using the pull-down list below before making a final selection to work through.

- Incident response playbooks are an invaluable tool for organizations to quickly and efficiently respond to security incidents. With an incident response playbook, organizations can define the steps they need to take to respond to a security incident, such as the specific roles, processes, and procedures that security staff must follow. Incident response playbooks can also guide communication with stakeholders and the public, as well as how to gather evidence and determine the incident's root cause. Oftentimes, the playbook is just that—a physical book a security professional uses in response to an incident. Using a physical book ensures its availability during a wide-scale incident. In a highly secure environment, it also ensures attackers do not digitally exfiltrate the IR capabilities.

- If you want to work through a different method for this playbook step, select another method and perform those steps.

  However, since you have terminated the CPU-consuming rogue process, you must re-launch it by expanding the following hint:
    Expand this hint for guidance.
        1. Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.
        2. Enter C:\LABFILES\Playbook-Lab.ps1.
        3. If prompted about allowing and execution exception for the script, type Y, then press Enter.
        4. Close this Windows PowerShell console.
  
#### Check your work

Confirm that you terminated the rogue process, consuming most of the CPU resource.

### Hash the suspicious file

#### Playbook Step #3

The next step of the High-CPU IR Playbook is:

    3. Hash the file associated with the rogue process.
    
In this High-CPU IR Playbook step, you will locate the file associated with the rogue process named <HighCPUName> and calculate a SHA256 hash of the suspicious file.

Make a selection of the method to use to accomplish these tasks. The method options are:

    - CLI-CP - using CLI Command Prompt tool certutil
    - CLI-PS - using CLI PowerShell cmdlet Get-FileHash
    - Third-party - using the Sysinternals CLI tool sigcheck
    
You can review the offered methods using the pull-down list below before making a final selection to work through.

- The most effective incident response playbooks are tailored to an organization's specific security needs and provide detailed guidance on responding to various security incidents. For example, a playbook may contain detailed instructions on responding to a ransomware attack or a data breach. Additionally, the playbook should include guidance on taking the necessary steps to contain the incident, such as isolating affected systems and measures to ensure the incident is fully resolved.

- If you want to work through a different method for this playbook step, select another method and perform those steps.

Keep in mind that using the same output filename will be overwriting the file's contents each time you use a different hashing method or technique.

#### Check your work

Confirm that you captured the SHA256 hash of the suspicious file.

### Online Malware Scan

#### Playbook Step #4

The next step of the High-CPU IR Playbook is:

    4. Perform an online malware analysis using the hash value of the suspicious file.
    
In this High-CPU IR Playbook step, you will perform a malware evaluation of the hash of the suspicious file using an online malware scanning service.

Make a selection of the method to use to accomplish this task. The method options are:

    - Hybrid Analysis
    - MetaDefender
    - VirusTotal
    
You can review the offered methods using the pull-down list below before making a final selection to work through.

- When a playbook utilizes a high degree of automation from a SOAR system, it can be referred to as a runbook, though the terms are also widely used interchangeably. A runbook aims to automate as many playbook stages as possible while incorporating clearly defined interaction points for human analysis. These interaction points should present contextual information and guidance needed for an analyst to make a quick, informed decision about the best way to proceed with incident mitigation. For example, a runbook may use integrations for cloud-based email platforms and antimalware solutions. The runbook may take email attachments from user emails and submit them to an online malware detection engine. Suppose such a service identifies the file as malicious. In that case, the SOAR can provide a new custom detection signature to the antimalware software to locate and block any other instances of the malware.

Since the malware hash scan results are indeterminate, you do not know if the suspicious file is benign or is a new unknown malicious file. Therefore, you will continue forward with the next playbook step.

- To see actual results from this site, repeat the search using the hash of 82407eaf6437d6956f63e85b28c0ec6ca58d298a. This is the hash of ca_setup.exe, the installer for the password cracking and network attack tool Cain. Cain is considered by many to be a hacker tool and potentially malicious.

- If you want to work through a different method for this playbook step, select another method and perform those steps.

#### Check your work

Confirm that you determined whether the suspicious file was known to be malicious.

### Determine the owner of the suspicious file

#### Playbook Step #5

The next step of the High-CPU IR Playbook is:

    5. Determine the owner of the suspicious file.
    
In this High-CPU IR Playbook step, you will determine the owner of the suspicious file.

Make a selection of the method to use to accomplish this task. The method options are:

    - GUI - using File Explorer
    - CLI-CP - using CLI Command Prompt dir command
    - CLI-PS - using PowerShell cmdlet Get-ACL
    
You can review the offered methods using the pull-down list below before making a final selection to work through.

- An incident response workflow is a classic example of a SOAR task defined within a playbook. A playbook contains a checklist of actions to respond to a specific event. A playbook should be made highly specific by including the query strings and signatures to detect a particular type of incident. A playbook may account for compliance concerns, such as breach notification requirements, including when and to whom notification must be made.

- If you want to work through a different method for this playbook step, select another method and perform those steps.

#### Check your work

Confirm that you have determined the owner of the suspicious file.

### Archive the Suspicious File

#### Playbook Step #6

The next step of the High-CPU IR Playbook is:

    6. Archive the suspicious file into a zip container along with a file and its hash value.
    
In this High-CPU IR Playbook step, you will create an archive containing the suspicious file and the hash output file.

Make a selection of the method to use to accomplish this task. The method options are:

    - GUI - using File Explorer
    - CLI-CP - using CLI Command Prompt tool tar
    - CLI-PS - using PowerShell cmdlet Compress-Archive
    
You can review the offered methods using the pull-down list below before making a final selection to work through.

- When creating an incident response playbook, organizations should ensure they have the right level of detail and that all necessary stakeholders are involved, including security teams, IT staff, legal teams, and other personnel who may be involved in responding to the incident. Organizations should also ensure that the incident response playbook is updated as new threats and technologies emerge.

- If you want to work through a different method for this playbook step, select another method and perform those steps.

However, you may need to delete the <HighCPUName>.zip file you have already created. Otherwise, the archive creation process may fail or prompt for overwriting permission. These issues can be avoided by deleting the existing file: from a Command Prompt or Windows PowerShell console, enter del c:\<HighCPUName>.zip to delete the file.

#### Check your work

Confirm that you created an archive of the suspicious file and its hash file.

### Copy the archive to a quarantine system

#### Playbook Step #7

The next step of the High-CPU IR Playbook is:

    7. Copy the zip archive of the suspicious file to a quarantine system.
    
In this High-CPU IR Playbook step, you will move the archive of the suspicious file to a quarantine system. In this exercise, the quarantine system will be the Kali VM.

Make a selection of the method to use to accomplish this task. The method options are:

    - NC - using Netcat and PowerShell
    - WinSCP - using WinSCP
    - SAMBA - Using SAMBA
    
You can review the offered methods using the pull-down list below before making a final selection to work through.

- If you want to work through a different method for this playbook step, select another method and perform those steps.

However, you will need to execute the following command on Kali from the Terminal window: rm -rf /root/quarantine to delete the /root/quarantine directory and the HeavyLoad.zip file contained within that directory.

#### Check your work

Confirm that you have copied the archived suspicious file to the quarantine system.

### Remove the suspicious file from the victim

#### Playbook Step #8

The next step of the High-CPU IR Playbook is:

    8. Remove the suspicious file from the affected system(s).
    
In this High-CPU IR Playbook step, you will remove the suspicious file and related files from the victim system.

Make a selection of the method to use to accomplish this task. The method options are:

    - CLI-CP1 - using CLI Command Prompt del command
    - CLI-CP2 - using the Sysinternals CLI tool SDelete
    - GUI - using File Explorer and the Recycle Bin
    
You can review the offered methods using the pull-down list below before making a final selection to work through.

- If you want to work through a different method for this playbook step, select another method and perform those steps.

However, the files were deleted that are necessary to repeat this playbook step. To recreate them, expand the following hint:
    Expand this hint for guidance.
        1. Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.
        2. Enter copy "c:\Program Files\JAM Software\HeavyLoad\HeavyLoad.exe" c:\Users\.
        3. Enter echo 705204f80158bfbf9216a19bc2cb25a392ff028ce028745ac16f543b6290ec81 > c:\Users\HeavyLoad-Hash.txt.
        4. Enter Compress-Archive -Path "C:\Users\HeavyLoad.exe", "C:\Users\HeavyLoad-Hash.txt" -DestinationPath "C:\HeavyLoad.zip".
        5. Close this Windows PowerShell console.

            NOTE: This will not quite recreate the hash file exactly as the specific steps used in Playbook Step #3, but it is close enough for repeating Playbook Step #8, where you are just going to delete them again.

#### Check your work

Confirm that you deleted the suspicious file, its hash file, and the archive file from PC10.

### Craft a Report about the Response

#### Playbook Step #9

The next step of the High-CPU IR Playbook is:

    9. Fill out an incident report and submit it to the SOC for review.
    
In this High-CPU IR Playbook step, you will be instructed about crafting a report of the operations taken to resolve this security incident.

Now that you have completed the playbook's primary steps, you need to craft and file a report about the security incident response. Your report should include a summary of the High-CPU IR Playbook steps, along with the methods used and results obtained.

The High-CPU IR Playbook steps are:

    1. Investigate the high CPU usage and determine the rogue process's name.
    2. Terminate the offending process.
    3. Hash the file associated with the rogue process.
    4. Perform an online malware analysis using the hash value of the suspicious file.
    5. Determine the owner of the suspicious file.
    6. Archive the suspicious file into a zip container along with a file and its hash value.
    7. Copy the zip archive of the suspicious file to a quarantine system.
    8. Remove the suspicious file from the affected system(s).
    9. Fill out an incident report and submit it to the SOC for review.
    
This type of report is often known as an AAR (After Action Report). It can also be referred to as a Lessons Learned or Post-Mortem report. The goal or purpose of this report is to document the activities performed, note any discrepancies or problems encountered, and glean information about where the process, playbook, toolset, or environment may need to be changed or improved.

Once the report is crafted, it should be submitted to your CISO for review.

- Since this is only a simulated environment, there is no need for you to actually craft a report to complete this lab.

#### Check your work

Confirm that you understand the concept of crafting an AAR.
