# Assisted Lab: Performing DNS Filtering

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.4 Given a scenario, analyze indicators of malicious activity.
4.4 Explain security alerting and monitoring concepts and tools.
4.5 Given a scenario, modify enterprise capabilities to enhance security.
4.7 Explain the importance of automation and orchestration related to secure operations.
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

- Event Viewer
- Terminal Emulator

## Steps

### Investigate Strange DNS activity

The security team at your ISP has informed you that there is suspicious activity taking place across the Internet connection. The communications are initiated by a system in the Structureality private network. You elect to start your investigation of the issue on the PC10 client system. This is one of the clients that has had issues in the past due to the user's poor security hygiene. You decide to enable DNS logging to see if it can detect IoCs related to suspicious activity.

1. Connect to the PC10 virtual machine, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

        - Jaime is a member of the Domain Admins group. So, this user account is an administrator on the PC10 system.

2. Select Type here to search from the taskbar, type event, then select Event Viewer from the results.

3. Maximize the Event Viewer window.

4. In the left pane, select the arrow beside Applications and Service logs to expand its contents.

5. Select the arrow beside Microsoft, then select the arrow beside Windows to expand its contents.

6. Scroll down to locate, then select the arrow beside DNS Client Events to expand its contents.

        - You may need to click-hold-drag-release the pane divisions to resize them. You may need or want to readjust the panes throughout this exercise.

7. Right-click Operational, then select Properties.

Sec-Lab22-Event_Viewer-Operational-Properties.jpg

8. Select to mark the checkbox Enable logging, then select OK.

9. In the left pane, select Operational.

        - You should now be viewing the Operational DNS client events log. There may even be a few events in the log already.

    In a real-world investigation, you would allow the log to collect entries for a period of time, then begin reviewing the recorded events to look for suspicious activities. In this exercise, you will be initiating a script that will perform the activities that will be labeled as "suspicious" as you perform threat hunting.

10. Select Type here to search from the taskbar, type powershell, then select Windows PowerShell from the results.

11. Enter .\lab22demo1.ps1.

        - This script will perform several operations to create DNS traffic to be collected by the logging function you enabled. This simulates suspicious activity that you will investigate.

12. Wait for the presentation of the numbers 1 thru 5, with a final display of "Maximum number of attempts reached. Terminating script."

        - This script was written to perform tasks for five (5) iterations and then terminate. A real-world malicious script might continue to function indefinitely.

13. Minimize the Windows PowerShell console.

14. Return to the Event Viewer.

15. In the right pane, select Refresh.

        - The Operational log should now have hundreds of events.

16. In the right pane, select Filter Current Log….

        - Most logs collect a significant number of records, even for a singular event. For example, the DNS queries you are investigating in this exercise may generate 12 or more separate records. Part of this is due to both IPv4 and IPv6 queries being performed, but also the complex operations of modern DNS systems.

17. On the Filter Current Log window, select the <All Event IDs> text field, then type 3010.

        - In this log, the Microsoft assigned Event ID of 3010 is for the initial DNS query.

18. Select OK to apply this filter.

19. Scroll to the top of the filtered log, then select the first Event ID 3010 entry.

20. Look at the bottom pane where the General tab is shown. In the top text box of this tab, you should see a statement starting with "DNS Query set to DNS Server…." followed by the FQDN being queried.

21. Look through the Event ID 3010 entries to see if you can find any FQDNs that might be suspicious.

        - Use the Up and Down arrows on your keyboard to move through the list.

        - You can ignore any FQDN that is a system hostname added to .ad.structureality.com. This is because these are internal DNS names and are not sent to the external internet. Therefore, the ISP would not be complaining about queries or communications between systems internal to the organizational network.

Once you have discovered the problematic FQDN, type it into the DNS Threat text box below:

        - Press enter after you type in the FQDN or click out of the text box.

        - The use of this specific FQDN and country-focused TLD is not intended as disparagement to that country. Every country has malicious sites being hosted within its borders. Often, an FQDN stands out as odd, problematic, or suspicious when it is unique (i.e., has not been resolved by the environment before), has name components where previous malicious events originated, or is of an origin that is outside the typical communication patterns of the organization.

    Now that you have discovered the problematic FQDN, you need to determine its frequency of occurrence in the DNS log.

22. Scroll back to the top of the filtered log, then select the first Event ID 3010 entry.

        - The selection of the first record is necessary as the Find… feature only searches in reverse chronological order. In other words, it searches from the currently selected event to older events.

23. In the right pane, select Find….

24. On the Find window, select the Find what: text field, then type <DNSThreat>, then select Find Next.

        - If there is not an FQDN in this instruction, then you did not enter it in the text box a few steps prior.

25. The first occurrence of an Event ID 3010 with a query of <DNSThreat> should be selected. Confirm by looking at the text box on the General tab of the bottom pane of the selected event record.

26. On the Find window, select Find Next to move to the next occurence.

        - Notice that at the end of the text statement on the General tab of the selected Event ID 3010 item, there is either a "type 1" or "type 28". Type 1 is an A (i.e., address) record query attempting to discover the IPv4 address associated with the FQDN. Type 28 is an AAAA (i.e., address) record query attempting to discover the IPv6 address associated with the FQDN.

27. Repeat the selection of Find Next until you see the error window with the statement: "Searching from the selected event to the end of the list, there is no event that contains the specified string. To search all events, select the first event in the list and run the search again".

28. Select OK to close the error window.

29. Go back over the entries for <DNSThreat> and paying attention to only the "type 1" queries, look at the time stamp for each of the event records.

        - When a process repeatedly attempts to access an external site, it is known as beaconing. Often beaconing is used to establish or maintain connectivity to a command and control (C&C) location on the internet. A C&C connection may facilitate granting an attacker remote access and remote control over the system from which the beacon originated.

30. While one the entries for <DNSThreat> is selected, in the bottom pane, select the Details tab.

        - This tab contains more specifics compared to the General tab.

31. Select + System near the top of the Details tab's content.

        - This expands information about the system from where the event record originated.

32. Scroll down through the System section to locate Execution | [ProcessID].

        - This is the PID of the process that initiated the query. If the offending process was continually running (unlike the lab22demo1.ps1 script, which only runs for about 10 seconds), then this PID could be used in a tasklist query to discover the process name (as performed in a previous exercise in this lab). Once you identify the offending process, then you can consider your next actions. Options for further action include determining how the offending process came to be on the system and what can be done to mitigate the issue (i.e., terminate its execution and remove it from the system). You should also add the related FQDN to a DNS filter to block future attempts to resolve this domain name.

#### Check your work

Confirm that you enabled DNS logging.

Confirm that you discovered IoC activity of a suspicious process beaconing.

### Automation of DNS filtering

You are subscribed to a threat intelligence feed that provides a list of FQDNs that are known to be associated with malicious activities, such as DDOS, phishing, ransomware, and botnet command and control (C2 or C&C). In this exercise, you will automate the task of creating a DNS block to the problematic FQDN from the threat feed, which alters the local /etc/hosts file via a script.

1. Select the KALI VM and sign in as root using Pa$$w0rd as the password.

2. Open a Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar.

3. The maximized Terminal window should still be open.

4. Enter the following command to view the DNS threat feed:
    curl 127.0.0.1/bad_DNS.feed
        - The lab environment does not have internet access. Therefore, this threat intelligence feed is being simulated for this exercise. This simulated feed contains fictitious FQDNs. If these do exist on the internet, we are making no claims as to whether these sites are or are not related to malicious activity. These FQDNs are being used for demonstration purposes only within this non-internet exercise.

    This command retrieves and displays the contents of the malicious DNS threat feed to the screen. Notice the inclusion of the badsite.ru, which you discovered as the beaconing target of malware on PC10. If only you had started the DNS filtering sooner…

5. You would like to have a script that will automatically retrieve the content of the malicious DNS threat feed and then add the FQDNs to the /etc/hosts file, so any attempt to resolve the FQDN would result in the loopback address instead of its real IP address. Enter vim block-DNS.sh to create a new script file.

6. Type i to enter insert mode. The message -- INSERT -- should be present at the bottom of the screen.

7. Type the following into the empty document area of vim:
    #!/bin/bash
    echo '  ' >> /etc/hosts
    echo '#Bad DNS' >> /etc/hosts
    curl -s 127.0.0.1/bad_DNS.feed | while read fqdn; do
        echo "127.0.0.1 $fqdn" >> /etc/hosts
    done
    awk '!x[$0]++' /etc/hosts > /tmp/hosts
    mv /tmp/hosts /etc/hosts
        - This script already includes a deduplication function.

        - The first "echo" statement has two spaces between the single quotes. This is used to separate the current entries of the /etc/hosts file from the ones to be added by the script from the threat feed, and it is different from the existing empty line already present (which is using a single space). This ensures it will be retained when the deduplication function executes.

8. Once finished, press ESC to exit insert mode.

        - Pressing ESC may cause your browser to exit full-screen mode. If that occurs, press ESC a second time to exit VIM's insert mode. Then you can re-enable full-screen mode from the Display lab interface menu.

9. Enter :wq to save and quit VIM.

10. Enter cat block-DNS.sh to view the contents of the script file you have created.

11. Enter chmod +x block-DNS.sh to set the execute permissions on the file.

12. Enter cat /etc/hosts to view the current contents of the /etc/hosts file.

13. Enter ./block-DNS.sh to execute the script.

14. Enter cat /etc/hosts to view the updated contents of the /etc/hosts file.

    Notice the new section of "#Bad DNS" along with entries for the malicious FQDNs linked to the loopback address.

15. Enter ping reallybadsite.com -c 4 to test the DNS blocking implemented by the script.

        - You can test all of the FQDNs from the threat feed if you want.

16. Automate the execution of the script so it runs daily at 3 AM by entering the following:

    echo "0 3 * * * /bin/bash /root/block-DNS.sh" | crontab -

17. Enter crontab -l to view the currently scheduled tasks.

- This DNS script is for proof of concept purposes. This type of automated script only affects the system where the altered /etc/host file resides. While this script could be set to run on all systems, it would be a better option to protect the entire private network by crafting a script to modify the DNS system to block queries attempting to resolve the malicious FQDNs from the feed.

You have automated updates to the /etc/hosts to prevent access to malicious FQDNs as indicated by a threat intelligence feed.

#### Check your work

Confirm that you crafted a script to update the /etc/hosts file to block malicious DNS name resolutions.

Confirm that you tested the DNS resolution of the malicious FQDNs and confirmed the blocking protection.

### DNS reconnaissance with nslookup

When reviewing the contents of logs of user activity, you will often discover domain names that need to be investigated. For this exercise, you will use the nslookup tool to evaluate the domain name of comptia.org. This procedure can often be used to discover other related FQDNs, name servers, email servers, and more.

- The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local system.

- Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

1. Open a terminal window on your local system.

       - If you do not know how to open a terminal window (a.k.a., shell or command prompt) on your local system, please perform an online search using the phrase: "how to open a terminal window on " and then include the name of your operating system. For example, if you are running on Debian Linux, search using "how to open a terminal window on Debian", or if you are working from a MacOS, search using "how to open a terminal window on MacOS".

2. Working from your local system's terminal window, open nslookup in interactive mode by entering: nslookup.

        - Nslookup is a program to query the Internet Domain Name System (DNS) by submitting requests to domain name servers. Nslookup operates in one of two modes: interactive or non-interactive.
            - Interactive mode allows a user to search domain name servers for IP and name information on various hosts or domains. It can also be used in this mode to display a list of hosts in a domain.
            - Non-interactive mode returns the name or IP information requested for an individual host or domain.

    - The nslookup command is present on most OSes. However, if it is not present on your specific system, then you may need to install it or access a different system with nslookup present. Barring those options, you may need to skip this exercise.

3. Check the lookup server used by nslookup by entering: server.

4. The results should indicate the IP address used as the default DNS lookup server. Enter the IP address into the Default DNS lookup server box below:

        - Press Enter on your keyboard after you type in the value or click out of the text box.

        - Windows also has a native nslookup command that operates for the most part like the version in Linux or macOS.

        - By default, nslookup uses the same DNS server to perform queries as the host. However, the server sub-command can change the DNS server used to perform lookups.

        - If the following nslookup commands are not providing results, you may need to change your DNS lookup address. Your current default DNS server lookup address <DDNSLS> may be limiting or restricting your queries. You can attempt to bypass this restriction by using a different and external DNS server, such as that hosted by Google at 8.8.8.8, Cloudflare at 1.1.1.1, or Level 3 Communications at 4.2.2.1. Use the following command to change your DNS lookup server address: server 8.8.8.8.

5. View the address resource records of the FQDN by entering: www.comptia.org.

6. The results should show one or more addresses assigned to the FQDN of www.comptia.org in an Address (A) record.

        - Since comptia.org is hosted within the Cloudflare service, the IP addresses assigned to the domain name may change over time. Therefore, we do not include those addresses in this exercise since they will likely differ.

        - This lab environment is limited to using only IPv4. If IPv6 were available, you would also see any defined IPv6 address associated with this FQDN. You can also set the lookup type to A for IPv4 addresses or AAAA for IPv6 addresses or ANY for any/all addresses.

7. Notice the caveat statement above the results of "Non-authoritative answer:". This indicates the results are returned from a caching DNS server instead of directly from an authoritative server. It is good practice to work directly against an authoritative server.

8. Enter: set type=SOA

        - This command sets the lookup to the SOA (Start of Authority) resource record type, which contains the primary authoritative DNS server's IP address.

        - It is also possible to view NS records to see all authoritative DNS servers for a domain name. The NS list will include the primary and secondary authoritative DNS servers for a domain name, but they will not be labeled as such. Any addresses from the NS list would be effective for obtaining authoritative results.

9. Enter: comptia.org

10. The results should be similar to the following:

    origin = armando.ns.cloudflare.com
    mail addr = dns.cloudflare.com
    serial = 2299472123
    refresh = 1000
    retry = 2400
    expire = 604800
    minimum = 3600
        - The serial value may be different in your display as any edits to a DNS zone file causes an incrementation of the serial number.

        - The refresh, retry, expire, and minimum values are shown as numbers of seconds. Divide those numbers by 60 to determine the intervals in minutes, then divide again by 60 to determine the intervals in hours, then divide by 24 to determine the intervals in days. For example, 604800 seconds / 60 = 10,080 minutes; 10,080 minutes / 60 = 168 hours; and 168 hours / 24 = 7 days.

        - The value named "origin" identifies the primary authoritative DNS server for the queried domain name (i.e., comptia.org). The primary authoritative DNS server hosts the only readable and write-able copy of the zone file for the domain. The zone file contains the various resource records for the domain.

        - The mail addr line in the SOA record is the email address to be used when needing to contact someone about the registered domain. However, it does not currently look like a standard email address. That is because the @ symbol is not an allowed character in DNS. So, the convention is to replace the @ symbol with a period. Therefore, you must replace the first period with an @ symbol to return it to the proper structure of an email address.

11. Enter: set type=a.

12. Enter: armando.ns.cloudflare.com

        - One or more IP addresses should result from this A query.

13. Enter the IP address of the authoritative DNS server into the DNS IP address box below:

        - Press Enter on your keyboard after you type in the value or click out of the text box.

15. Change the lookup server for nslookup to the IP address of the authoritative DNS server for comptia.org (hosted by Cloudflare) by entering: server <DNSIP>

        - If this command does not show an IP address, you did not enter an IP address in the DNS IP address field in the previous step.

15. Perform the original FQDN address query again by entering: www.comptia.org.

16. Notice the results no longer have the caveat line. Therefore, the results are coming directly from an authoritative DNS server. This usually means they are more trustworthy and accurate since they are from an authoritative source.

17. Change the resource record type to view the authoritative DNS servers related to the registered domain name by entering: set type=ns followed by comptia.org.

    The results should show the nameservers for comptia.org of armando.ns.cloudflare.net and jade.ns.cloudflare.net.

        - If the name servers listed for comptia.org are not as expected, they are likely still to be a nameserver within Cloudflare. Thus, while the initial hostname may vary over time, the FQDNs of the nameservers should still end with .ns.cloudflare.com.

        - You may have noticed that for some queries, you use the FQDN of www.comptia.org, while other queries only use the registered domain name and TLD (i.e., comptia.org). The convention is that for most DNS queries, the search is based on just the registered domain name and TLD, while for A/AAAA queries use an FQDN.

18. Change the resource record type to view the SMTP email servers related to the registered domain name by entering: set type=mx followed by comptia.org.

    The results should show the mail exchanger (i.e., SMTP server) FQDN for comptia.org.

19. Change the resource record type to view the CNAME (Canonical Name) (i.e., alias) records related to an FQDN domain name by entering: set type=cname followed by store.comptia.org.

    The results should show a canonical name for store.comptia.org. For example, the result viewed at the time of this lab's creation was store-comptia-org.kibocloud.com.

20. Exit interactive-mode nslookup by entering: exit.

        - Another interesting resource record is PTR (Pointer). However, comptia.org does not have this type of resource record defined (or we could not locate one). If you want to view a PTR record (and use nslookup in non-interactive mode), enter nslookup google.com, then enter nslookup -type=ptr [IP] but replace the [IP] with one of the IP address results from the first query of google.com.

21. Leave the local Terminal window open.

The tool of nsloolup is a powerful DNS utility, but it does not lend itself easily to recording its operations and results (at least not in interactive mode). So while you can use nslookup in non-interactive command mode, in the next exercise, you will use dig to perform the same queries and capture the output into a file to retain this information for your security report.

#### Check your work

Confirm that you used nslookup in interactive mode.

Confirm that you retrieved results from an authoritative DNS server.

Confirm that you enumerated the IP addresses of an FQDN, name server (NS), and email server (MX)

Confirm that you viewed the domain name's SOA record.

### DNS reconnaissance with dig

When reviewing the contents of logs of user activity, you will often discover domain names that need to be investigated. For this exercise, you will use the dig tool to evaluate the domain name of comptia.org. This procedure can often be used to discover other related FQDNs, name servers, email servers, and more.

    - The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local system.

    - The dig tool is typically present on most Linux installations. However, it can be installed on most other OSes where it is not already present. You will need to search to find instructions on installing dig if it is not already present on your local machine. If you are unable to install dig, then please look over this exercise anyway. You should notice that it performs similar operations to that of nslookup. Here is a guide to install DIG on Windows: https://www.configserverfirewall.com/windows-10/dig-command-windows/.

    - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

1. Return to the local terminal window you left open from the previous exercise.

2. Use the dig utility to determine the primary authoritative DNS server related to the registered domain name of comptia.org by entering:

    dig -t SOA comptia.org
    - By default, dig queries for A and AAAA records, but if those are not found, it may attempt to pull the SOA. With the -t parameter, the resource record type to query can be explicitly selected.

3. View the output of the dig operation.

    Notice that this operation displayed the SOA record for comptia.org. The SOA values of origin, mail addr, serial, refresh, retry, expire, and minimum are present, but all in one line without labels.

4. To display the A record for the primary authoritative DNS server, enter:

    - dig -t A armando.ns.cloudflare.com
    View the results to locate an IP address for armando.ns.cloudflare.com. Enter the IP address of the DNS server in the text box below:


    - Press Enter on your keyboard after you type in the value or click out of the text box.

5. Use dig to display the authoritative output of the A records for www.comptia.org by entering:

    dig @<DNSIPDIG> -t A www.comptia.org
6. Use dig to display the authoritative output of the MX records for comptia.org by entering:

    dig @<DNSIPDIG> -t MX comptia.org
7. Use dig to display the authoritative output of the NS records for comptia.org by entering:

    dig @<DNSIPDIG> -t NS comptia.org
8. Use dig to display the authoritative output of the CNAME records for store.comptia.org by entering:

dig @<DNSIPDIG> -t CNAME store.comptia.org
9. Close your local terminal window.

#### Check your work

Confirm that you used dig to queriy for DNS information.
