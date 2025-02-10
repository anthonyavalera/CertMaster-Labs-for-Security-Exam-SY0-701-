# APPLIED LAB: Incident Response: Detection

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.4 Given a scenario, analyze indicators of malicious activity.
3.2 Given a scenario, apply security principles to secure enterprise infrastructure.
4.4 Explain security alerting and monitoring concepts and tools.
4.5 Given a scenario, modify enterprise capabilities to enhance security.
4.8 Explain appropriate incident response activities.
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

- wazuh
- Terminal Emulator
- Event Viewer

## Steps

### Detecting logon events with wazuh

Wazuh is an open-source security platform built on OSSEC, providing a range of features for monitoring, threat detection, and compliance management. Key features include log analysis, file integrity monitoring, vulnerability detection, intrusion detection, configuration assessment, and incident response. Wazuh can be integrated with other security tools like the Elastic Stack and is highly scalable and suitable for on-premises, cloud, or hybrid deployments. This comprehensive solution helps organizations protect their IT infrastructure, detect potential security threats, and maintain compliance with industry standards and regulations. Effectively, wazuh functions as a SIEM, an IDS, and a SOAR solution all in one.

The wazuh platform is deployed in this lab environment on an Ubuntu server VM named wazuh. This system takes a few minutes to become fully active due to the significant number of components that must be loaded and activated by the wazuh platform. Thus, to give the system time to finish loading, you will perform attack preparation steps before accessing wazuh.

  - The name of the primary security tool in use in this lab is wazuh, which only uses lowercase letters. So, other than when this name appears as the first word of a sentence, it will be in lowercase as its' developers intended.

IoC (Indicator of Compromise) detection and analysis is the process of identifying, collecting, and analyzing signs of potential security breaches or malicious activities within an IT infrastructure. Indicators of Compromise are artifacts or pieces of information that suggest an intrusion, malware infection, or other security incidents. These specific details are also known as observables. IoCs help security teams detect threats, investigate incidents, and respond effectively to minimize the impact of potential breaches. While IoC detection and analysis can be accomplished manually, it is often essential to use automation to maintain relentless oversight of an enterprise network environment.

In this exercise, you will be using wazuh to detect IoC (Indicators of Compromise) related to logon events. You will perform two types of login attacks, then view the security alerts caused by those attacks in wazuh.

1. Connect to the KALI virtual machine and sign in as root using Pa$$w0rd as the password.

2. Open a Terminal window and then maximize the Terminal window.

  Expand this hint for guidance.
    a. On the Kali Linux toolbar (located at the top of the screen by default), select the Terminal Emulator. This icon looks like a black computer screen with a cursor.

    b. The window that opens is the Terminal window. It will have a prompt of root@kali.

    c. Select the Maximize button on the Terminal window located to the far-right on the header. The icon is immediately to the left of the close X. It will look like a blank or black circle, until your mouse cursor is over it, then it displays a white square.

3. Create /root/passlist.txt by adding Pa$$w0rd into the 57th line position of the /usr/share/seclists/Passwords/500-worst-passwords.txt file. Then, confirm the addition of this lab password.

  Expand this hint for guidance.
    a. Enter the following:
      sed '57i\Pa$$w0rd' /usr/share/seclists/Passwords/500-worst-passwords.txt > passlist.txt
      The passlist.txt is modified in this command in order to allow for a successful password-guessing attack.

    b. Enter ls -l to confirm the passlist.txt is present in the current directory (which should be /root).

    c. Enter the following:

      grep -n 'Pa$$w0rd' passlist.txt
  The output should be: 57:Pa$$w0rd. This confirms that the lab password was added to this password list file in the proper position.

4. Access the wazuh platform at 10.1.16.242 using a web browser and log in as admin using Pa??w0rd as the password.

    - If an "Warning: Potential Security Risk Ahead" page is displayed when attempting to access 10.1.16.242, select Advanced, scroll down, and then select Accept the Risk and Continue.

  Expand this hint for guidance.
    a. Open Firefox by selecting its icon from the taskbar.

    b. In the Firefox address bar, enter 10.1.16.242.

    c. If the wazuh log in page is not displayed, wait a few moments, then refresh the page.

       If you see the message Wazuh dashboard server is not ready yet, then wait several seconds, then refresh the page.

    d. Once the log in fields are presented, type admin in the Username field, type Pa??w0rd in the Password field, and then select Log In.

    e. A presentation of service activation progress may be displayed. This can take up to a minute to complete.

  The wazuh home screen should be displayed.

    - If you leave the wazuh interface idle for too long (typically 10 mins or more), the session will timeout. However, the currently displayed screen will not change, but the session will have ended. When you attempt to select another feature or function from the wazuh interface, you will be prompted to log in again. Use admin and Pa??w0rd to log in if needed.

5. View the Security events for only the DC10 system.

  Expand this hint for guidance.
    a. Select Security events from the Security Information Management section of the wazuh home page.
      
      The wazuh Security events presentation is an amalgamation of the data pulled from all systems where a wazuh agent is installed. In this lab, there is an agent on DC10 and PC10.

    b. Select Explore agent near the top of the page.

    c. On the Explore agent pop-up window, select DC10.

    The DC10 (001) agent identifier should be displayed in the location where the Explore agent link was previously.

    - If needed, you can clear the agent focus setting by selecting the push-pin image to the right of the DC10 (001) agent name.

    - The PC10 system is also configured with a wazuh agent. But that agent and VM are not used in this exercise.

6. Scroll down the Security events page to view the currently available information.

   Since the monitored system of DC10 has only been running for a few minutes, there will be only minimal information.

    - Notice that the timer interval is set to Last 24 hours by default. This is sufficient for this lab and all of the wazuh exercises.

    - Most of the information on the Security events page is clickable to view more information or implement filters.

7. Scroll back to the top of the Security events page. Look at the counter of Total, then select Refresh to update the presentation with any new events. You may see the Total counter increment, which indicates new events occurred on the monitored systems that have been evaluated by wazuh.

    - Windows is a very noisy and busy operating system. This is further exacerbated by the DC10 system being a domain controller. There are myriad tasks performed automatically by the OS and Active Directory, which involve launching tasks or services which trigger logon and logoff events. These and other common management events will populate the log files of DC10 and be evaluated by wazuh. You will perform specific actions to simulate suspicious or malicious events and then see what wazuh detected about those events.

8. Leave the browser open to wazuh.

9. From the Terminal window, use hydra to perform a password guessing attack, using the passlist.txt modified previously, via the RDP (Remote Desktop Protocol) service against the administrator account on DC10 (10.1.16.1).

  Expand this hint for guidance.
    a. Return to the Terminal window opened previously

    b. Enter the following to perform a password guessing/stuffing attack against the administrator account on DC10 (10.1.16.1) while attempting to access the RDP service:

      hydra -t 1 -V -f -l administrator -P passlist.txt rdp://10.1.16.1
      This command will perform the password-guessing attack against the target. You should see 57 attempts, with the 57th attempt succeeding. Hydra terminates once a successful password guess occurs.

10. Switch back to the browser displaying wazuh.

11. View the security alert(s) resulting from the password guessing attack. Find the entry of Rule ID 92652 for the successful password discovery.

  Expand this hint for guidance.
    a. Select Refresh at the top of the page to update the display with new information obtained by the wazuh agent on DC10.

      The Total counter should increase by at least 57, and you should see incremented Authentication failure and Authentication success counters.

    b. Type 92652 into the Search field near the top of the page, then select Update.

    - You may have to select Refresh a second time for the results to actually update based on your search term. (Note: the Refresh button changes to the Update button when you type something into the search field.)

    c. Scroll down below the graphs to view the list of Security Alerts.

    The result should be a page with a very low Total count (likely 1 (unless you ran the hydra attack multiple times)), and the Security alert list should only have one or very few rows.

    d. Select the first item you can locate in Security alert list with a Rule ID of 92652.

    This should expand the Security alert to present you with all of the details. Notice that it contains information pulled from the Windows Event Security log and details related to the wazuh rule.

    e. Select the same Security Alert row again to collapse the details.

    - If you cannot locate the event record, select Refresh again from the top of the page. It can take up to a full minute for logged items on the target (i.e., DC10) to be retrieved by the wazuh agent and included in the presentation from the wazuh server. If you still can't find it, try searching the page, type CTRL+F, then type 92652. If this Rule ID is present on the current results page, then it should be automatically highlighted.

    - You can increase the number of events shown per page of results at the bottom of the screen. Select Rows per page: 10, then select 50 rows from the pop-up list of options.

    - The wazuh platform primarily pulls data from DC10's event logs. Thus, based on the level of logging/auditing configured on the source, wazuh may not obtain sufficient information to detect the broadest range of threats. The wazuh platform is not limited to Windows event logs, as it can also pull in application logs, network device logs, logs from other OSes (such as Unix and Linux), and even cloud service logs. The more broadly these various systems, services, and devices collect logs; the more wazuh can obtain and analyze the details of the related events.

12. Notice that the wazuh Security events page presents a range of interesting information for each listed event, including Technique(s), Tactic(s), Description, and Level.

    - Column details:

    Techniques(s) - This column provides a reference code as a click-link to more information about the potential techniques used in the attack or detected activity. This can include details about attack vectors, tactics, and any known attack patterns or signatures. This column aims to offer insights into the nature of the event and the attacker's methodology or intent. Wazuh uses the MITRE ATT&CK framework to categorize and describe the techniques used in detected events. This framework is a globally accessible knowledge base of adversary tactics and techniques based on real-world observations. By leveraging the MITRE ATT&CK framework, Wazuh can provide more contextual information about the threats and help administrators understand the attacker's objectives, tactics, and techniques, leading to more effective incident response and threat mitigation.

    Tactics(s) - A higher-level category that groups related techniques, representing the attacker's overall objectives or goals. This column aims to offer insights into the overall objectives or goals of the attacker, giving context to the techniques used in the event. By providing this context, wazuh enables more effective incident response, threat management, and mitigation strategies.

    Description - This information is defined by the wazuh rule, which matches the event from the source's logs. It is a description of or a prediction of the type of event that occurred.

    Level - The wazuh security event Level, also known as the alert level, is a numerical value assigned to each security event or alert generated by the wazuh platform. The alert level is designed to indicate the severity or importance of the event, helping administrators prioritize their responses and focus on the most critical issues.

    Wazuh uses a scale from 0 to 16 for its alert levels, with 0 being the least severe and 16 being the most severe. The alert levels are typically categorized as follows:

      - Informational (0-3): These alerts indicate routine events or general information about the system or application and usually do not require immediate action.
      - Low severity (4-7): These alerts indicate minor security issues, non-critical system events, or policy violations that should be investigated but may not require immediate action.
      - Medium severity (8-11): These alerts indicate more significant security issues, potential breaches, or critical system events that should be addressed promptly.
      - High severity (12-15): These alerts indicate severe security issues, active breaches, or critical system failures that require immediate attention and action.
      - Emergency (16): These alerts represent the most severe and urgent security events, indicating an active or imminent threat to the system or infrastructure.
    Alert levels can be customized based on an organization's specific requirements, allowing administrators to fine-tune the priority and response to different types of security events. Wazuh's flexible alert management system helps organizations effectively manage their security monitoring and incident response efforts.

13. View the Technique information related to Rule ID 92652.

  Expand this hint for guidance.
    a. Select T1550.002 from the first Security Alerts row of an entry with Rule ID 92652.

      A Details page about the Pass the Hash technique is displayed.

    b. Review the information about this attack technique?

What is the MITRE ATT&CK technique associated with the event with Rule ID 92652? (Enter exactly as presented under the Name heading on the Details page)

  The technique associated with this event is inaccurate. While it is true that a pass the hash attack (PtH) could have been the cause of the event recorded into the Windows security log, you know that is not the attack you performed. You ran a password-guessing attack using a dictionary list, which is not the same attack concept as PtH. A PtH attack requires the theft of an access token from a valid client, which is then used from a different system to fool the authentication service.

    - The Technique(s), Tactic(s), Description, and Level columns of the wazuh Security Alerts are not always accurate. You would need to look at the raw data from the logs to confirm what actually took place. You can create your own rules to process log entries differently than the default rules. This lab uses only the default wazuh rule set.

    - The wazuh interface can be challenging to navigate. You mostly can only move forward through content, as using the back toolbar button does not do anything. If you navigate away from a search result list or interface page, you often need to return to the wazuh home page and re-navigate to the desired page or location. This also means your search will be discarded, although filters are usually more resilient.

14. Find the entries of Rule ID 60122 for the failed password discovery attempt records.

  Expand this hint for guidance.
    a. Select the wazuh homepage, and then select Security Events in the Security information management section to return to the top of the wazuh Security events page.

    b. Change the search value from 92652 to 60122, then select Update.

  There should be numerous results of Logon failure - Unkown user or bad password.

    - A Wazuh rule is a set of conditions and criteria used to identify and classify security events, generate alerts, and trigger actions in response to specific patterns or activities. Wazuh rules are written in XML format and are essential to the platform's intrusion detection, log analysis, and compliance monitoring capabilities. The main components of a Wazuh rule include:
      - Rule ID: A unique identifier for the rule, which is used to reference the rule in logs, alerts, and other rules.
      - Description: A brief summary of the rule's purpose, explaining what it is designed to detect or monitor.
      - Level: The severity or importance of the event detected by the rule, represented as an alert level on a scale of 0 to 16.
      - Groups: One or more group names that categorize the rule, making it easier to manage and filter related rules.
      - Frequency: The number of events matching the rule's conditions that must occur within a specified time window before an alert is generated (used in combination with timeframe).
      - Timeframe: The time window (in seconds) within which a specified number of events matching the rule's conditions must occur to generate an alert (used in combination with frequency).
      - Match: The pattern or expression that the rule looks for in the log data, typically defined using regular expressions or other pattern-matching techniques.
      - Decoders: The decoder(s) associated with the rule, which are responsible for extracting relevant information from log data and normalizing it for further analysis.
      - Options: Additional settings or modifiers that affect the rule's behavior, such as noalert (which prevents alerts from being generated) or ignore (which tells Wazuh to disregard certain events).
      - Mitre ATT&CK ID: The unique identifier(s) for the MITRE ATT&CK technique(s) or tactic(s) associated with the rule, providing context for the detected event and helping security teams understand the attacker's methodology and intent.
  These components define the conditions under which a rule is triggered and the actions taken when an event matches the rule. Wazuh's flexible rule system allows organizations to create custom rules tailored to their specific needs, enhancing their security monitoring, threat detection, and compliance management capabilities.

15. Delete the 60122 value from the wazuh Search field, then select Update.

16. From the Terminal window, attempt to mount the C$ share using the Jaime account and Pa$$w0rd as the password.

  Expand this hint for guidance.
    a. Return to the Terminal window.

    b. Enter mkdir /mnt/dc10-c to create a mount point.

    c. Enter the following and provide Pa$$w0rd as the password when prompted.

      mount -o username=jaime //10.1.16.1/c$ /mnt/dc10-c
  This mount attempt will fail.

17. Attempt to mount the C$ share using the administrator account and Pa$$w0rd as the password.

  Expand this hint for guidance.
    a. Return to the Terminal window.

    b. Enter the following and provide Pa$$w0rd as the password when prompted.

      mount -o username=administrator //10.1.16.1/c$ /mnt/dc10-c
  This mount attempt will succeed.

18. Switch back to the web browser focused on wazuh.

19. Locate the Security events caused by the mount attempts.

    - The Rule ID for a logon failure is 60122. The Rule ID for a logon success is 60106.

  Expand this hint for guidance.
    a. Select Refresh to update the wazuh Security events page with the latest data.

    b. Enter 60122 in the Search field, then select Update.

    c. Scroll down to view the alert record(s).

    d. Select an alert record to expand it. After reviewing the details, select it again to collapse it.

    e. Scroll back up to the top of the page.

    f. Enter 60106 in the Search field, then select Update.

    g. Scroll down to view the alert record(s).

    h. Select an alert record to expand it. After reviewing the details, select it again to collapse it.

20. Delete the 60122 value from the wazuh Search field, then select Update.

21. Leave all windows open.

You have seen wazuh security alerts triggered by matching IoCs to questionable logon activity. The activity of Incident Response Detection is the recording of events into logs, the automated analysis of those logs, and the automated notification of significant incidents to security professionals (if configured). Without detection, i.e., becoming aware of a violating occurrence, it is not possible to initiate Incident Response.

#### Check your work

Confirm that you accessed the wazuh interface.

Confirm that you performed dictionary-based password guessing using hydra.

Confirm that you attempted mounting a Windows administrative share.

Confirm that you viewed security alerts through wazuh related to IoCs of suspicious logon events.

### Detecting anti-forensics with wazuh

Anti-forensics are activities performed by intruders in an attempt to mask, hide, or destroy evidence of their malicious actions on a system. In this exercise, you will delete log files and view the related security alerts of these IoCs in wazuh.

1. Connect to the DC10 virtual machine. Send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

2. Clear the contents of the Security log.

  Expand this hint for guidance.
    a. Select Type here to search from the taskbar, enter Event and then select Event Viewer.

    b. In the left pane, double-click Windows logs to expand it.

    c. In the expanded list, select Security.

    d. In the right pane, select Clear log….

    e. On the Event Viewer pop-up confirmation window, select Clear.

    f. Close the *Event Viewer

3. Connect to the KALI virtual machine and, if needed, sign in as root using Pa$$w0rd as the password.

4. Return to the web browser displaying the wazuh interface.

    - If you leave the wazuh interface idle for too long (typically 10 mins or more), the session will timeout. However, the currently displayed screen will not change, but the session will have ended. When you attempt to select another feature or function from the wazuh interface, you will be prompted to log in again. Use admin and Pa??w0rd to log in if needed.

5. Locate the wazuh security alert related to the security log deletion (i.e., 63103).

  Expand this hint for guidance.
    a. Enter 63103 in the Search field, then select Update.

    b. Scroll down to view the alert record(s).

    c. Select an alert record to expand it. After reviewing the details, select it again to collapse it.

You have reviewed the detection of the IoCs of clearing logs of a monitored system through wazuh.

#### Check your work

Confirm that you cleared Windows event logs and viewed the related wazuh security alerts.
