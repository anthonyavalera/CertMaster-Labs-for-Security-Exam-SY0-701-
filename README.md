# Assisted Lab: Performing Root Cause Analysis

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.2 Explain common threat vectors and attack surfaces.
2.4 Given a scenario, analyze indicators of malicious activity.
4.4 Explain security alerting and monitoring concepts and tools.
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

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

### Investigating a security alert

It is just before 6 PM (or 18:00) on Mar 31, 2023. You have received a security flash message from the company SOC (Security Operations Center), which indicates that several auditing policies on DC10 have been changed. In this exercise, you will initiate an investigation into the cause of this alert.

This lab is based on a simulated exploitation. It was performed and "recorded" into the lab environment on 3/31/2023. You will perform the investigation as if the issue was current/recent rather than in the distant past. In other words, act as if today is 3/31/2023.

Connect to the KALI virtual machine and sign in as root using Pa$$w0rd as the password.

Open Firefox by selecting its icon from the taskbar.

In the Firefox address bar, enter 10.1.16.242.

If an "Warning: Potential Security Risk Ahead" page is displayed when attempting to access 10.1.16.242, select Advanced, scroll down, and then select Accept the Risk and Continue. The reason for this is the wazuh security platform automatically rotates its certificates on a regular basis. Therefore, each time this happens, it will not be recognized by the browser as a known entity.

If the wazuh log in page is not displayed, if you see an error, if you see the message Wazuh dashboard server is not ready yet, then wait a few moments, then refresh the page.

The wazuh platform is deployed in this lab environment on an Ubuntu server VM named wazuh. This system takes a few minutes to become fully active due to the significant number of components that must be loaded and activated by the wazuh platform. Be patient, and after a minute or two, it will be ready for you.

Once the log in fields are presented, type admin in the Username field, type Pa??w0rd in the Password field, and then select Log In.

Due to the password rules of wazuh, the password for the wazuh admin account uses question marks instead of dollar signs in the common lab password.

A presentation of service activation progress may be displayed. This can take up to a minute to complete.

The wazuh home screen should be displayed.

If you leave the wazuh interface idle for too long (typically 10 mins or more), the session will timeout. However, the currently displayed screen will not change, but the session will have ended. When you attempt to select another feature or function from the wazuh interface, you will be prompted to log in again. Use admin and Pa??w0rd to log in if needed.

Select Security events from the Security Information Management section of the wazuh home page.

The wazuh Security events presentation is an amalgamation of the data pulled from all systems where a wazuh agent is installed. In this lab, there is an agent on DC10 and PC10.

Select Show dates from the data select field (it is beside the Refresh button).

Since you are working on a security breach that occurred on 3/31/2023, you need to set wazuh to display the information from that time period.

The date selection field should now display ~ a day ago -> now.

Select ~ a day ago.

A time selection management window is displayed with three tabs: Absolute, Relative, and Now.

Select the Absolute tab.

In the Start date field on the bottom of the Absolute tab, type Mar 31, 2023 @ 00:00:00.000.

Once you type the final zero, the calendar will automatically update to that date and the date selection field should now display Mar 31, 2023 @ 00:00:00.000 -> now.

From the date selection field, select -> now.

Do not select the Now tab!

A time selection management window is displayed with three tabs: Absolute, Relative, and Now.

Select the Absolute tab.

In the End date field on the bottom of the Absolute tab, type Apr 1, 2023 @ 00:00:00.000.

Once you type the final zero, the calendar will automatically update to that date and the date selection field should now display Mar 31, 2023 @ 00:00:00.000 -> Apr 1, 2023 @ 00:00:00.000.

Select Update to apply the new time settings.

The Refresh button changes to the Update button when you type something into the search field.).

Select Refresh to update the Security events page.

You review the security flash message. It states that the notification relates to wazuh security alerts based on Rule ID 60112. Type 60112 into the Search field, then select Update.

How many security alerts for Rule ID 60112 are present for DC10?

42
13
23
17
Scroll down to view the list of Security alerts. Select one after another to review them.

After selecting an alert to expand it, select it again to collapse it.

In several of the Rule ID 60112 alerts, look for the data.win.eventdata.auditPolicyChanges value and the data.win.eventdata.subjectUserName value. Notice that the alerts are indicating PolicyChanges of Success removed and/or Failure removed. This is quite concerning, as this means that any events occurring after these audit changes would not be recorded in the logs.

Disabling auditing is a common anti-forensic tactic performed by malicious entities once they obtain elevated privileges. This allows them to perform additional malicious activities without the risk of a record of those actions being created.

What UserName is associated with the Rule ID 60112 security alerts on DC10?

DC10
administrator
jaime
structureality
When you reach the bottom of the page of results, notice that only 10 rows of results are displayed per page by default. You can increase the number of events shown per page of results by selecting Rows per page: 10, then select 50 rows from the pop-up list of options.

Find and expand the last of the Rule ID 60112 security alerts. Locate the data.win.system.eventRecordID and enter it in the box below:

RecordID for an Audit Change security alert: 

Press Enter on your keyboard after you type in the value or click out of the text box.

Be sure to collapse any alert record after reviewing it. Otherwise, it will stay open even after a new search (if it remains as a search result), which just makes scrolling more complicated.

After considering the information from the numerous security alerts for Rule ID 60112, you realize that the changes were made by an account that has administrator privileges throughout the organization's network. This means that serious violations of company policy could have taken place through that account due to the fact that it has significant privileges on almost every system on the network. You decide to see what else might have been detected by wazuh in relation to that account.

Scroll back to the top of the Security events page.

In the Search field, type jaime, then select Update.

If the results do not update based on the new search term, select the Update/Refresh button a second time.

Scroll down to locate a logon security alert related to jaime. You should see an alert related to Rule ID 92653.

What type of connection is indicated in the security alert for Rule ID 92653 related to jaime?

Local Workstation
Remote Desktop Connection (RDP)
Interactive
Network Connection
This is the logon event that occurred just before the audit policy changes. You find it a bit suspicious that so soon after establishing a connection would the user make the audit policy changes. You also find it odd that the jaime account connected to the DC10 over a remote access connection method, when normally, they perform their management tasks at the keyboard, which is the company's standard practice for managing domain controllers. Additionally, it is even more concerning that the RDP connection was made from a system with IP address 10.1.16.2, which is not the normal workstation used by Jaime. 10.1.16.2 is MS10 which is an older Windows Server. Jaime typically operates from his assigned workstation of PC10 (10.1.24.101).

In a real-world investigation, the security specialist will already have knowledge of the organization's security policies, operational guidelines, and typical practices. This type of institutional knowledge, experience, and information is essential to provide context for the interpretation of the evidence uncovered during an investigation.

Expand the security alert related to Rule ID 92653. Locate the data.win.system.eventRecordID and enter it in the box below:

RecordID for an RDP security alert: 

Press Enter on your keyboard after you type in the value or click out of the text box.

Collapse the security alert. Take note of the time of the alert and enter it in the box below in the format HH:MM:SS (you do not need to enter the microseconds):

Time of RDP security alert: 

Press Enter on your keyboard after you type in the value or click out of the text box.

Leave Firefox open to the wazuh security events page.

After your review of the security alerts related to the audit policy changes, you decide to continue your investigation on the DC10 system.

Check your work
Confirm that you started the investigation into the security issue of changed audit policies.
Confirm that you used the security events and alerts of wazuh to identify details about the violations.

### Investigate the breach on DC10

You will now switch over to the DC10 system to continue your root cause investigation. In this exercise, you will determine what audit policies were changed and inspect the Security log of DC10 for more information.

Connect to the DC10 virtual machine. Send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

Minimize or close Server Manager if it appears. It will not be used in this exercise.

You want to determine the state of the audit policy on DC10.

Select Type here to search from the taskbar, enter cmd, right-click over Command Prompt from the results, and then select Run as administrator.

Select Yes on the User Account Control window.

Maximize the Command Prompt window.

Enter auditpol /get /category:*

Scroll to view the entire list of audit policy status report lines.

What is the status of the audit policies on DC10?

No Auditing
Success
Success and Failure
Failure
Close the Command Prompt window.

Select Type here to search from the taskbar, enter Event and then select Event Viewer.

Maximize the Event Viewer.

In the left pane, double-click Windows logs to expand it.

In the expanded list, select Security.

Select the topmost event record, then select Find… from the right pane.

In the Find what: field type <RID-60112-AuditChange>, then select Find Next.

This is the Event Record ID for the first audit policy change event you pulled from the wazuh security alert.

The Find function should have located a matching event record. Select Cancel to close the Find window.

The event record with an Event Record ID of <RID-60112-AuditChange> should be selected.

Notice that this event record has an Event ID of 4719. The selected event record is the last in a series of event records with this same Event ID. This is the same collection of records that triggered the audit policy change security alerts in wazuh.

The Event Record ID is located in the event record, but it is not displayed or viewable by default. To view the location in the event record where the Event Record ID is stored, select the Details tab, then select to expand the + System item, then scroll down to view the EventRecordID value line.

An Event ID is a reference to a type of occurrence that was recorded in an event log. These are standard references established by Microsoft. An Event Record ID is a unique number assigned to each event record as it is added to the log in sequential order.

You take note of the time of the first of the event records related to the audit policy changes. That time is 05:56:05 PM (or 17:56:05).

You want to view the event record of the logon event for the jaime account that occurred just before the audit policy changes. Select Find… from the right pane, then type <RID-92653-RDP> into the Find what: field, then select Find Next.

This is the Event Record ID for the RDP session where the jaime account connected to DC10. You pulled this number from the wazuh security alert.

The Find function should have located a matching event record. Select Cancel to close the Find window.

The event record with an Event Record ID of <RID-92653-RDP> should be selected. Look over the information for this event record on the General tab.

The General tab has a scrollable window of information. Be sure to scroll through this collection of details so you don't overlook something important.

What is the Logon Type for this event record related to the jaime connection over RDP?

3
7
2
10
The Security log records logon events and categorizes them based on the following types:

Logon type	Logon title	Description
2	Interactive	A user logged on to this computer.
3	Network	A user or computer logged on to this computer from the network.
4	Batch	Batch logon type is used by batch servers, where processes may be executing on behalf of a user without their direct intervention.
5	Service	A service was started by the Service Control Manager.
7	Unlock	This workstation was unlocked.
8	NetworkCleartext	A user logged on to this computer from the network. The user's password was passed to the authentication package in its unhashed form. The built-in authentication packages all hash credentials before sending them across the network. The credentials do not traverse the network in plaintext (also called cleartext).
9	NewCredentials	A caller cloned its current token and specified new credentials for outbound connections. The new logon session has the same local identity, but uses different credentials for other network connections.
10	RemoteInteractive	A user logged on to this computer remotely using Terminal Services or Remote Desktop.
11	CachedInteractive	A user logged on to this computer with network credentials that were stored locally on the computer. The domain controller was not contacted to verify the credentials.
This table is from Microsoft at: learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc787567(v=ws.10)

...less
This record confirms what the wazuh security alert indicated specifically, that the RDP connection to DC10 was initiated from 10.1.16.2, which is the MS10 system.

You decide it is time to talk with Jaime directly to inquire about these events and alerts. However, before you contact HR and the physical security team, you look up the work schedule to determine whether Jaime is at work or not. The schedule shows that Jaime was at work today, but that his day likely ended at 6 PM and he may have already left.

You decide to look up badge access uses for Jaime to see which buildings and data center rooms he entered today. The records show that Jaime only entered the building where his office is located, and there are no data center room entries recorded for him for today (3/31/2023). Since MS10 is located in a data center of a different building on the company campus, it is unlikely that Jaime was able to work from MS10. Also, you see that Jaime has already left for the day. You decide to continue to investigate the issue before contacting HR, legal, and physical security.

You need to find more evidence to determine what happened and why. You would like to determine what happened before the RDP connection was established from MS10 to DC10. You continue your investigation on MS10.

Leave the Event Viewer open.

Check your work
Confirm that you confirmed that the anti-forensic tactic of disabling auditing was fully implemented on DC10.
Confirm that you reviewed the Security log on DC10 to confirm the wazuh alerts.

### Expanding the investigation to MS10

Since the RDP connection to DC10 originated from MS10, you will continue your root cause analysis and investigation from the MS10 system in this exercise.

Connect to the MS10 virtual machine. Send Ctrl+Alt+Delete, select Other user, and then sign in as administrator using Pa$$w0rd as the password.

The default user to log into MS10 will be presented as Jaime. In this situation, this is not evidence of the violating event(s). The Jaime account is the default account for the MS10 system in the lab environment and is displayed as such in every lab for the first connection you make as a student to a lab system.

Minimize or close Server Manager if it appears. It will not be used in this exercise.

Select Type here to search from the taskbar, enter Event and then select Event Viewer.

Maximize the Event Viewer.

In the left pane, double-click Windows logs to expand it.

In the expanded list, select Security.

Select the topmost event record, then select Find… from the right pane.

Type 5:55 into the Find what: field, then select Find Next.

Please ignore any events occurring after 5:55 PM on MS10. They are not relevant to this lab or attack scenario.

Once the first event with a time stamp starting with 5:55 is found, change the search term to jaime in the Find what: field, then select Find Next.

Select Cancel to close the Find window.

The selected event record should be a Logon event with Event ID of 4648. Look over the information on the General page for this event record. Enter the username from the Account Name: value line in the box below: (Only use lowercase letters as it is listed in the event record.)

RDP initiator 

Press Enter on your keyboard after you type in the value or click out of the text box.

Select the Score button to validate this task:

Notice that the time stamp for this event record is nearly the same as that of the RDP connection event record on DC10, which was <time-of-RDP>. This confirms that MS10 was the origin of the RDP session to DC10 and that the jaime account was used to log into DC10 over RDP.

You now have evidence that the user that initiated the RDP session from MS10 to DC10 was not Jaime the administrator, but <RDP-initiator> from HR, who is a standard worker with a limited account. This means that the credentials for the jaime account were somehow obtained by <RDP-initiator>, and then used to connect to DC10 via RDP and disable auditing.

You now want to confirm that the <RDP-initiator> account was logged onto MS10. So, you look through the event log for an entry with an Event ID of 4624 (a logon event) and an Account name: of <RDP-initiator>. You discover one such event record with an Event Record ID of 4176.

While the event record for the RDP initiation is still selected, select Find… from the right pane, change the search term to 4176 in the Find what: field, then select Find Next.

Select Cancel to close the Find window.

The selected event record should indicate that An account was successfully logged on and that account was <RDP-initiator>. Review the other information presented on the General tab.

What is the logon type for the currently selected event record related to Dylan and MS10?

3
2
10
7
You now have evidence that Dylan logged into MS10 directly. You consult the data center entry logs and see that Dylan was able to enter the data center at 5:32 PM. You check the video footage of the data center entrance at the time and see proof of Dylan entering the data center.

Take note of the time stamp for this logon event record of the dylan account accessing MS10 in the box below in the format HH:MM:SS (you do not need to enter the microseconds):

Time of Dylan logging into MS10: 

Press Enter on your keyboard after you type in the value or click out of the text box.

Leave the Event Viewer window open.

You now have evidence that Dylan was the perpetrator of the audit policy changes and that they were responsible for the RDP connection from MS10 to DC10. You have proof of their entry into the data center to access the MS10 system directly. However, you don't understand how Dylan was able to obtain login credentials for the jaime account.

You decide to continue your investigation from Jaime's workstation, which is PC10.

Check your work
Confirm that you discovered evidence showing that the dylan account was used on MS10.
Confirm that you determined that the dylan account was logged on when the RDP session was initiated to DC10 using the jaime account.

### Continuing the investigation from PC10

You are now looking for anything that might reveal how the credentials for the jaime account were obtained by Dylan. You have decided to look at Jaime's PC10 workstation.

Connect to the PC10 virtual machine. Send Ctrl+Alt+Delete and sign in as Jaime using Pa$$w0rd as the password.

As the security professional, you may be authorized to access systems throughout the network in pursuit of evidence related to security breaches. In this lab scenario, we are taking a shortcut to grant you access to PC10 under the jaime account by having you log into the system with the jaime credentials.

After reviewing the event log for security events and checking the malware scanner for records of malicious code discoveries, you don't find anything relevant. You decide to check Jaime's email inbox.

Double-click Mozilla Thunderbird from the Desktop.

After a few moments, the Mozilla Thunderbird email client should open.

While at first you are impressed by Jaime's adherence to Inbox Zero, you are curious about the single message remaining in their inbox. Select Grab you free juice!.

You immediately suspect that this is a phishing scam email since the subject line is not using correct grammar. As you read over the message, you are now convinced that this is a scam email. Even though the source email address is one you know to be legitimate, it could easily be spoofed to give the scam message a sense of validity.

Position your cursor over the System Update link but don't click on it.

Notice the URL that appears in the bottom status bar. It contains an IP address and a file named proxyset.bat. Enter the IP address in the text box below:

Scam IP address: 

Press Enter on your keyboard after you type in the value or click out of the text box.

Leave the Thunderbird window open.

Select Type here to search from the taskbar, enter cmd, then select Command Prompt from the results.

Enter ping <Scam-IP>.

Wait for the ping operation to complete and for the C\Users\jaime> prompt to be displayed. Notice that the results of this command show that the IP address used in the scam email is no longer present on the network.

Technically, a ping check for a system is not a completely reliable means of knowing that a system is not present or does not exist. A firewall on the target can discard any ping echo requests, thus, the results look the same as when the target is not present. A more effective technique is to perform a full port scan over TCP and a full enumeration scan over UDP. However, this would take considerable time and isn't relevant to the continuation of this lab's root cause analysis.

...less
You want to determine if the file that the URL from the scam email is present on the PC10 system. Enter cd c:\ && dir /s proxyset.bat.

After a few seconds, you should see the result indicating that the proxyset.bat file is present on the system.

What is the absolute folder reference for where the proxyset.bat file is located? (type in the path exactly as shown by the dir command result, including capitalization)

You have confirmed that Jaime received a spam email message, which included a link to download a file. Jaime must have fallen for the scam message and downloaded the file.

Everyone is vulnerable to social engineering attacks. Even administrators can be fooled by a cleverly crafted pretext message. Don't blame the victim for falling for the scam. Blame the crafters of the attack for being malicious and using social engineering tricks to fool their targets. Social engineering remains one of the primary means by which adversaries gain access to a secure organization's network. Attackers will use any and every opportunity to exploit an existing weakness, or they will use techniques to create a vulnerability. Social engineering is often used to trick a member of an organization into giving away information or granting logical or physical access to a secured infrastructure. We all need to be more aware of the potential to be targeted by social engineering attacks and be more skeptical of any and all communications.

...less
View the contents of the downloaded file by entering type c:\Users\jaime\Downloads\proxyset.bat

Based on the contents of this file, you see that it changes the proxy settings for Firefox. You want to see if Jaime executed this file.

Select Type here to search from the taskbar, enter firefox, then select Firefox from the results.

From the Firefox browser window, select the Open application menu from the toolbar (a.k.a. the hamburger menu), then select Settings.

Scroll down to the bottom of the Settings General page. Under Network Settings select Settings….

Based on what you see on the Connection Settings you have verified that Jaime did fall for the scam email message, downloaded the batch script, and executed that downloaded script.

What is the setting selected in the Connection Settings area of Firefox?

Use system proxy settings
Auto-detect proxy settings for this network
Automatic proxy configuration URL
Manual proxy configuration
No proxy
Select Cancel to close the Connection Settings window of Firefox.

Leave Firefox open.

Switch back to Thunderbird by selecting its icon from the taskbar. Its icon is a blue phoenix around an envelope.

Look over the scam email again. There is an encouragement to first run the System Update file, but the second inducement is to visit a URL for the Juice Shop located in the building.

You recognize the URL of juiceshop.com as being valid. This seems odd as part of a scam message. You wonder why there would be a link to a valid site in a scam message.

Switch back to Firefox by selecting its icon from the taskbar. Its icon is the orange fox curled around a globe.

In the Firefox address bar, enter juiceshop.com.

After 30 seconds or so, you will see an error of The connection has timed out. You then remember that Firefox is configured to use a proxy, which was set by the script from the scam email. Since the attempt to access this known valid URL failed, the proxy settings in use by Firefox are not currently working as expected.

Switch back to the Command Prompt by selecting it from the taskbar.

Review the script, which should still be displayed in the Command Prompt. Notice that the proxy settings made by the script will direct all communications from Firefox to 10.1.16.2. You recognize that IP address. That is the IP address of MS10. Thus, if Jaime did click on the Juice Shop link and reached the actual website, then there would have been a proxy function operating on MS10 at the time Jaime fell for the scam message. But, if such a proxy function was used to support Jaime's visit to the Juice Shop URL, it is not operating now.

Leave all windows open.

Based on the additional evidence gathered from PC10, you think that Jaime was the victim of a scam email. That scam email convinced Jaime to download and run a script. That script then changed the Firefox browser proxy settings to use 10.1.16.2 (MS10) as a proxy. You want to determine if Jaime clicked on the link to visit the Juice Shop URL. So, your next steps will take place on ROUTER-BORDER.

Check your work
Confirm that you discovered a scam email message sent to Jaime.
Confirm that you investigated the batch script from the scam email and verified it was executed, which caused proxy setting changes to Firefox.

### Continuing the investigation on ROUTER-BORDER

In an attempt to confirm whether Jaime clicked on the link to the Juice Shop website, you will continue your root cause investigation on the company's network firewall system, ROUTER-BORDER. This system is located between the private network of Structureality and the internet.

The lab environment does not have real internet access. Instead, the simulated internet for the lab environment where the host of the Juice Shop website is located.

Switch to the KALI virtual machine and, if needed, sign in as root using Pa$$w0rd as the password.

You are returning to Kali, your cybersecurity workstation, to use a web browser to access the GUI management interface of the ROUTER-BORDER system. While you could connect directly to that system, you would be limited to the CLI, and accessing the log details is significantly more cumbersome using that method.

Open a Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar (located at the top of the screen by default). This icon looks like a black computer screen with a cursor.

In the Terminal window, enter ping juiceshop.com -c 1.

This command performs a single ping against juiceshop.com. This results in a presentation of the resolved IP address associated with that FQDN. Enter the IP address resolved from juiceshop.com into the box below:

Juice Shop IP address: 

Press Enter on your keyboard after you type in the value or click out of the text box.

Select the Score button to validate this task:

Switch to the Firefox browser and open a new tab.

On the new tab, enter 10.1.128.253 into the address bar.

If you see an error message of The connection was reset instead of the OPNSense interface (or the "Warning: Potential Security Risk Ahead" page), then expand the workaround guidance below and perform those steps before continuing.

Expand this wordaround guidance in the event of The connection was reset error.
If an "Warning: Potential Security Risk Ahead" page is displayed when attempting to access 10.1.128.253, select Advanced, scroll down, and then select Accept the Risk and Continue.

This message appears because the certificates used by OPNSense automatically rotated on a regular basis. Therefore, each time this happens, it will not be recognized by the browser as a known entity.

On the OPNsens login page, enter root in the Username: field and Pa$$w0rd in the Password field.

In the left pane, select Firewall, then select Log Files in the expanded options under Firewall, then select Live View in the expanded options under Log Files.

At the top of the Firewall: Log Files: Live View page, there is a filtering rule configuration toolbar. Select the left field currently displaying action, then select dst from the pull-down list of options.

The dst filter option is for the destination IP address.

Leave the operator as contains.

Enter <JBIP> in the right field, replacing the current value of pass.

Select + to implement the filter.

If you only see a single result, and that result is the icmp communication you performed just moments ago with the ping command against juiceshop.com, then the default search history depth is too shallow. Just above the results list, to the right, is a pull-down selector that is currently showing a value of 25. Select 25, then select 1000 (or if needed, 5000 or even 10000).

Create another filter. Select src, then set 10.1.24.101 (the IP address of PC10) as the value, then select + to implement the filter.

The src filter option is for the source IP address.

There should only be one result (if any) a single ICMP event. The lack of other communications indicates that no other direct connections were made from PC10 (10.1.24.101) to juiceshop.com (<JBIP>) occurred.

Next, check to see if a connection to juiceshop.com from MS10 (10.1.16.2) occurred.

Select src~10.1.24.101 from under the filter configuration toolbar to remove it.

Create another filter. Select src, then set 10.1.16.2 (the IP address of MS10) as the value, then select + to implement the filter.

There should be several results confirming that a connection to juiceshop.com (203.0.113.228) from MS10 (10.1.16.2) did occur. This is, therefore, some evidence that Jaime may have clicked on the Juice Shop link from the scam email. Since his Firefox browser was altered to use 10.1.16.2 as a proxy, his web communications would have been routed through MS10.

Select the information icon on the right side of one of the filter results to open the details for the communication.

Review the details of the communication between 10.1.16.2 and 203.0.113.228. Notice the dstport value. Enter the destination port number in the text box below:

Destination port number: 

Press Enter on your keyboard after you type in the value or click out of the text box.

Select the Score button to validate this task:

The dstport value for one of the logged events between 10.1.16.2 and 203.0.113.228 indicates what about the transaction?

It was a plaintext communication.
It was an encrypted session.
It was an email transaction.
It was an FTP session.
Notice the timestamp of the event. It likely has a value of or is similar to 2023-04-01T00:54:25.

This may seem odd at first. However, the firewall uses UTC time, while the other systems and their log files use local time. The DC10, MS10, and PC10 systems use the Pacific US timezone. To convert UTC to Pacific, you must subtract 7 hours. So, the firewall's time stamp, when adjusted for the local time zone, is 2023-03-31T17:54:25 (i.e., 5:54 PM today (assuming as we are that today is 3/32/2023)). Therefore, this event fits in the timeline of the events discovered so far:

Dylan logs into MS10
Dylan (assumed) sent a spoofed scam email to Jaime
Jaime downloaded and ran the malicious script from the scam email, which changed their proxy settings.
Jaime visited the Juice Shop website by way of an unauthorized proxy service on MS10.
Dylan logs into DC10 via RDP using jaime's credentials. Dylan disabled auditing on DC10.
Close the Detailed rule info window.

Leave all windows open.

It seems like you have almost figured out the exploitation timeline and the TTPs (tactics, techniques, and procedures) of the attack. However, you still have not determined how Dylan obtained the credentials for the jaime account. Since the communication from PC10 to the Juice Shop website was redirected through MS10 and that connection was in plain text, you have an idea of how Dylan may have accomplished credential theft. Your investigation takes you back to MS10.

Check your work
Confirm that you reviewed the firewall logs to determine that a visit to the Juice Shop website occurred from/through MS10.
Confirm that you discovered that the visit to the Juice Shop website was in plaintext.
Confirm that you also deduced from the firewall logs that the communication was of a time to be consistent with the attack timeline.

### Concluding the investigation on MS10

Based on the evidence collected, Jaime visited the Juice Shop website through MS10, which was serving as a proxy. That web connection was in plain text.

Connect to the MS10 virtual machine. If needed, send Ctrl+Alt+Delete, and sign in as administrator using Pa$$w0rd as the password.

You want to determine if the proxyset.bat file was created and remains on MS10.

Select Type here to search from the taskbar, enter cmd, right-click Command Prompt from the results, then select Run as administrator.

Select Yes on the User Account Control window.

Enter cd c:\ && dir /s proxy*.

There should be several results that match the "proxy*" string. However, none of them are proxyset.bat.

You then notice that one of the match results, proxy.ps1, is in the c:\HR folder. Enter type c:\HR\proxy.ps1 to view the contents of this file.

The .ps1 file extension on this script indicates what?

This is a Bash shell script.
This is a PowerShell script.
This is a batch script.
This is a python script.
This is interesting because Dylan is a member of the HR department, and this script would allow the MS10 system to serve as a proxy.

Upon further reflection of the gathered evidence, you recall that the scam email encouraged Jaime to log into the Juice Shop website using his credentials. He may have used his company credentials. And since the web session to the Juice Shop website was in plain text, Dylan could have used a network sniffer to intercept the HTTP communications.

Several servers in the Structureality network have Wireshark installed. This may have been the reason Dylan used MS10 in the first place. He could take advantage of existing software without having to attempt to install a sniffer himself.

Enter dir /s *.pcapng

This command should produce a result showing that a file named juiceshop.pcapng is present in the c:\Users\dylan\Documents folder.

Select Type here to search from the taskbar, enter wireshark, select Wireshark from the results.

Maximize the Wireshark window.

Select File from the menu bar of Wireshark, then select Open.

On the Wireshark - Open Capture File window, select This PC from the left side.

Scroll down, then double-click Local Disk (C:).

Scroll down, then double-click Users.

Double-click dylan.

Select Continue on the dylan window which claims you don't have permission to access this folder.

You are logged in as the administrator. This warning message is to inform you that you are entering into a user's home folder.

On the User Account Control window, select Yes to allow this app to make changes.

Double-click Documents.

Select juiceshop, then select Open.

The Windows GUI is set to hide file extensions of known file types by default. This is the file named juiceshop.pcapng.

In the Display filter line where it is currently showing "Apply a display filter … <Ctrl-/>", enter http.request.method == "POST".

This operation should implement a display filter so as to only show results for HTTP communications that contain POST methods. There should be two results.

The Wireshark display filters are case-sensitive, so this must be entered as shown.

There are several HTTP communication methods, including GET and POST. GET is typically used when requesting a URL from a web server. POST is typically used to send information, such as login credentials or form field values, to the web server for processing.

Select the second result which has "/rest/user/login" in the Info column.

This packet is the transaction of user credentials from the user's browser to the web server.

In the bottom pane of Wireshark, known as the Packet Bytes pane, scroll down to view the end of the payload of the selected packet. Pay attention to the right column, which is the ASCII conversion of the raw data (presented in hex) from the middle column of the bottom pane.

What is the username/email address contained in this HTTP POST message? (type it into the box below exactly as presented without the quotation marks)

What is the password contained in this HTTP POST message? (type it into the box below exactly as presented without the quotation marks)

Press Enter on your keyboard after you type in the value or click out of the text box.

Select the Score button to validate this task:

With the discovery of credentials in this network traffic capture, you have the final item of evidence which explains how Dylan was able to log into DC10 as jaime.

The activities you have performed in this lab across these numerous exercises are an example of root cause analysis. An investigation of a security breach is often a bit like a mystery that needs to be solved. You have to start with the final piece of evidence, typically the alert that a security violation has occurred, and work your way back to learn the details about the breach, such as what happened and who the perpetrator was. Often, an investigation will take considerable effort to differentiate benign details from actual evidence of the violation. You may find yourself backtracking numerous times after following clues to a dead end or needing to retrace your steps to review prior evidence in light of new information.

Once you have concluded a root cause analysis investigation, you typically need to write up a report to provide to the CISO. This report should detail the evidence discovered and your conclusions. It is also typical to include recommendations on response strategies to the security violation to mitigate future similar incidents.

For this incident, there are many possible recommendations for security improvements that would have stopped this overall attack from succeeding. Some mitigation strategies include:


Improved security awareness training for all personnel in regard to detecting and resisting social engineering attacks.
Blocking scripts from running unless they are pre-approved by an allow-listing execution filter.
Block changes to network configurations, such as proxies, to user browsers or client systems in general.
Do not allow non-administrators to enter a data center.
Do not allow non-administrators to log directly into a server (i.e., interactive logins)
Do not allow administrators to use RDP to connect to servers.
Encourage users to use a credential manager to minimize the occurrence of submitting credentials to a site or service that are associated with a different site or service.
Fortunately, reasonable levels of logging were enabled on most systems (at least before the attack), and an automated security assessment platform (i.e., wazuh) was present and active. This was essential to be alerted about the violating activity as well as providing a majority of the evidence related to the activity of the perpetrator.

There are at least two additional unanswered questions in regard to this security incident. 1) Why did Dylan violate security in this manner? and 2) What did Dylan do after he disabled auditing on DC10? In many real-world investigations, you don't ever really learn the answer to the "Why" question. But, you may have to spend considerable effect to track down non-log file evidence to answer the latter question.

However, this is the conclusion of this lab and this simulated security violation event.

Check your work
Confirm that you discovered the proxy script used on MS10.
Confirm that you discovered the network traffic capture, which contained Jaime's credentials.
