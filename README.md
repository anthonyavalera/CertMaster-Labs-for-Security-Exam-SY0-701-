# Assisted Lab: Finding Open Service Ports

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.2 Explain common threat vectors and attack surfaces.
2.3 Given a scenario, analyze the results of a reconnaissance exercise.

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

### Discovering outward facing open service ports

You are intially working from an external subnet (an internet simulation) in relation to the Structureality private network. You are performing a discovery and enumeration scan using nmap against the primary firewall of Structureality. Nmap is an open-source network scanner and security auditing tool.

- More information can be found at https://nmap.org/. The Security+ Skillable lab environment does not have direct internet access. Therefore, you will need to visit this URL using your local browser.

1. Connect to the KALI and sign in as root using Pa$$w0rd as the password.

- You are purposely signing in as root for this lab to access all of nmap's capabilities and avoid permissions issues. If you are working from a non-root account, you must use sudo su to switch user context and elevate your Terminal window to root.

2. Open a Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar (located at the top of the screen by default). This icon looks like a black computer screen with a cursor.

- The Terminal window should already be elevated to use root privileges.

- Maximize the Terminal window. This optimizes the space for output and maximizes the amount of information that can be displayed at once. If needed, you can adjust the text size from the Edit menu of the Terminal window.

3. Enter the following command to perform a port scan of the top 100 common ports of the company's border router Internet facing interface at 203.0.113.1, using the SYN scan method, while identifing the services on open ports and the OS, disable host discovery, and saving the results into a file.

- nmap 203.0.113.1 -F -sS -sV -O -Pn -oN border-scan.nmap
- For all nmap parameters, capitalization is important.

- This should take less than 30 seconds to complete. The output will be displayed to the screen as well as captured in a file.

- You can view the standard syntax of nmap using nmap without any parameters. You can view the manual page for nmap using man nmap. You can also view the complete Nmap Reference Guide online at nmap.org/docs.html. The Security+ Skillable lab environment does not have direct internet access. Therefore, you will need to visit this URL using your local browser.

- This nmap command will perform several operations against the target:

◦ The "-F" parameter sets the scan to only test the top 100 popular ports.

◦ The "-sS" parameter sets the scan type to SYN scan. This is also the default scan type. The SYN scan is the most reliable scan option as it simulates the initial communication attempt from a valid client, while not completing the establishment of a full session. Therefore, the SYN scan has the best chance of determining the open state of TCP ports.

◦ The "-sV" parameter performs a version scan, which attempts to elicit the identity of services on open ports.

◦ The "-O" parameter attempts to identify the operating system.

◦ The "-Pn" parameter disables host discovery and assumes all IPs are actively in use.

◦ The "-oN" parameter saves the output of nmap to the specified filename (in addition to the screen display of the same).

4. Enter the following command to display just the open port results:

- grep open border-scan.nmap
What port(s) are discovered as being open on this target?

53
80
21
3389
22
443
25

- Open service ports represent a threat vector to an organization. Especially ports that are discoverable from the internet. Open ports for services like email (i.e., port 25/tcp for SMTP) and web (i.e., port 80/tcp HTTP) can be targeted for attacks. If those services have vulnerabilies, an adversary may be able to compromise the system and gain remote control. You also discovered that port 22/tcp for SSH is open. This supports remote control/management/administration. But, is that necessary and warranted by the organization from the internet? If not, it should be closed. Generally, anything internet exposed needs to be hardened against any potential attack potential.

5. Enter the following command to display just the OS detection results:

- grep OS border-scan.nmap

What OS was detected on the target?

Linux
Windows
UNIX
FreeBSD
MacOS

- Being able to determine the OS of a target may allow an adversary to select a more effective exploit based on OS type and version. When possible, minimizing OS information made accessible to external entities would reduce this threat.

6. Leave the elevated Terminal window open for the next exercise.

Check your work
Confirm that you performed a network discovery and enumeration scan against a target.
Confirm that you evaluated the threat vectors and attack surface revealed by the scan.

### Discover threat vectors from a guest network

In addition to the threats from the internet, you should also be concerned about threat sources closer to home. A guest network may be a nice benefit to offer visitors, but if not properly configured, it could expose company resources to attack.

Change the network location of your Kali workstation to perform an anlysis of the attack surface from the guest network.

1. Select the Resources tab in the lab environment control pane.

2. In the area for Kali, select the pull-down list under eth0, then select vGUEST.

3. Select the Instructions tab in the lab environment control pane.

4. The Terminal window should still be open. Enter dhclient -r && dhclient.

- This command will release the previously assigned IP address from the internet subnet and obtain a new IP address in the server subnet.

5. Enter ip a s eth0.

- This command displays the IP configuration information for just the eth0 interface.

Select the Score button to validate this task.

6. Enter the following command to perform a port scan against the guest network's gateway device at 192.168.16.254 of the top 100 common ports, using the SYN scan method, while identifing the services on open ports and the OS, and saving the results into a file.

- nmap 192.168.16.254 -F -sS -sV -O -oN guest-scan.nmap
- This may take up to 2 minutes to complete. The output will be displayed to the screen as well as captured in a file.

7. Enter the following command to display just the open port results:

- grep open guest-scan.nmap

What is the name of the service found on several open ports? Enter the exact name in the text box below:


Press Enter on your keyboard after you type in the value or click out of the text box.

Select the Score button to validate this task.

- The service detected on ports 80, 443, and 8000 is the firewall. These ports can be used to access the firewall's management interface. It is not a secure deployment if a guest network member can access the management interface of the company firewall. These ports should be closed on the guest network. Keep in mind, that some ports should remain open to support valid communications, such as DNS (53), web (80/443), and email (25/465/587 (SMTP), 110/995 (POP3), 143/993 (IMAP)).

8. Enter the following command to display just the OS detection results:

- grep OS guest-scan.nmap
What is the OS discovered on the target?

Windows
Unix
MacOS
FreeBSD
Linux

- Being able to determine the OS of a target may allow an adversary to select a more effective exploit based on OS type and version. When possible, minimizing OS information made accessible to external entities would reduce this threat.

9. Leave the elevated Terminal window open for the next exercise.

Check your work
Confirm that you performed a network discovery and enumeration scan against a target.
Confirm that you evaluated the threat vectors and attack surface revealed by the scan.

### Discover the attack surface of the internal network

Guest network members are usually temporary visitors. But, what about the long term risk of internal entities - both computers and users? Scanning internal network systems for open service ports can reveal aspects of the internal attack surface. In this exercise, you will position your Kali workstation in the Client subnet, but scan a system in the Server subnet.

1. Select the Resources tab in the lab environment control pane.

2. In the area for Kali, select the pull-down list under eth0, then select vLAN_CLIENTS.

3. Select the Instructions tab in the lab environment control pane.

4. The Terminal window should still be open. Enter dhclient -r && dhclient.

- This command will release the previously assigned IP address from the internet subnet and obtain a new IP address in the server subnet.

5. Enter ip a s eth0.

- This command displays the IP configuration information for just the eth0 interface.

Select the Score button to validate this task.

6. Enter the following command to perform a port scan against the legacy server in the Server network of the top 100 common ports, using the SYN scan method, while identifing the services on open ports and the OS, and saving the results into a file.

- nmap 10.1.16.2 -F -sS -sV -O -oN server-scan.nmap
- This may take up to 2 minutes to complete. The output will be displayed to the screen as well as captured in a file.

7. Enter the following command to display just the open port results:

- grep open server-scan.nmap

What services are discovered to be accessible over open ports on this target?

FTP
MSRPC
IMAP
HTTP
RDP
Microsoft-DS
NTP
MySQL
SMTP
Mountd (i.e., NFS)

- The number of open service ports on this server is significant. While many of these services may be present for a valid reason, that needs to be verified. Any necessary service should be configured to use encrypted communications, even internally. Also, notice that all of these service ports are discoverable as open (and service versions elicited) because there is no firewall seperating the Client and Server networks. This is evidence of a lack of effective network segmentation. It needs to be recognized that internal systems repesent a real threat vector. An insider can cause just as much harm as an external intruder.

8. Enter the following command to display just the OS detection results:

- grep OS server-scan.nmap
- The OS of this server is Windows Server 2016. This OS has reached its EOL (End of Life) which occured on Jan 11, 2022. This means it is no longer considered an actively developed and supported OS. However, it may continue to receive update for security issue only through Jan 12, 2027. At that date it will be an EOSL (End of Service Life) system. This system should be slated for replacement before it reaches the EOSL date.

Check your work
Confirm that you performed a network discovery and enumeration scan against a target.
Confirm that you evaluated the threat vectors and attack surface revealed by the scan.
