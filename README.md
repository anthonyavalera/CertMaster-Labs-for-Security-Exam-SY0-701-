# Assisted Lab: Performing Reconnaissance

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

4.3 Explain various activities associated with vulnerability management.
5.3 Explain the processes associated with third-party risk assessment and management.
5.5 Explain types and purposes of audits and assessments.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Terminal Emulator
- nslookup
- dig

## Steps

### Find and explore a target's website

Often the first step a penetration tester performs is orienting themselves to where they are logically located and then confirming or discovering the target's online presence.

In this exercise, you will verify your Kali system is connected to the simulated internet of the lab environment, determine if the target's website is online, and discover information about the target from their website.

1. Connect to the KALI and sign in as root using Pa$$w0rd as the password.

2. Open a Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar.

3. Determine the current IP address of the eth0 interface by entering:
ip a s eth0

4. Enter the IP address of the Kali VM as revealed by the ip command in the text box below:

    - In this lab, the IP address range of 203.0.113.0/24 is defined as the internet. Technically, the lab environment does not have internet access.

5. You have been informed that the FQDN of the target is www.structureality.com. Capture the output of a ping command to see if that FQDN responds to echo-requests by entering the following command:
ping www.structureality.com -c 4 > target_info.txt

    - This should take about 15 seconds to complete. In Linux, the ping command will perform indefinite echo requests. Using the "-c 4" parameter limits the operation to 4 requests.

    - It is a best practice to always capture the output into a file to create a record of activities during a penetration test.

6. Confirm that the output file exists and contains data by entering the command: ls -l

    - You should see the file target_info.txt with a size of at least 100 bytes. This confirms that the output file was created and collected some information.

7. Display the contents of the output file by entering the command:
cat target_info.txt

    - The display should show that the FDQN of www.structureality.com resolves to 203.0.113.1. You should also see that while 4 packets were transmitted (i.e., echo-requests sent), no packets were received in response.

    - Not getting any results from an echo-request from a ping command may indicate that the target is either protected by a firewall or otherwise configured to ignore echo-requests.

8. Open Firefox by selecting its icon from the Kali Linux toolbar.

9. In the address field of Firefox, enter www.structureality.com

10. View the company's website. Notice that the company name, address, phone number, and email address are presented.

    - In a real-world pentest, you would take note of all information discovered. You might take a screenshot or save all web pages visited during the reconnaissance of a target.

11. Close Firefox, but leave the Terminal window open.

#### Check your work

Confirm that you determined your system's IP address.

Confirm that you attempted to ping the target's FQDN and recorded the results into a file.

Confirm that you visited the target's FQDN using a browser.

Confirm that you discovered details about the target from their website.

### Whois reconnaissance

Another common practice in the penetration testing phase of reconnaissance is determining the ownership of the target's registered domain name using whois. This can provide details about the company name, address, phone, email address, and personnel set as contact points.

1. Connect to the KALI and, if needed, sign in as kali using Pa$$w0rd as the password.

2. Return to the Terminal window.

3. Perform a whois query using the whois database host of 192.0.2.10 against the target's registered domain name and capture the output to a file by entering: whois -h 192.0.2.10 structureality.com > target_whois.txt

    - The host of 192.0.2.10 is the whois database host and server for this lab environment.

4. View the captured output of the whois operation by entering: cat target_whois.txt

5. Read over the output of the whois command. You may need to scroll up to view the entire presentation.

6. Leave the Terminal window open.

#### Check your work

Confirm that you performed a whois query and captured the output to a file.

Confirm that you discovered information about the target from whois output.

### DNS reconnaissance

Another common penetration testing task is to enumerate information from DNS about the target. This can often be used to discover other related FQDNs, name servers, email servers, and more. In this exercise, you will be using the CLI tools of nslookup and dig.

1. Connect to the KALI and, if needed, sign in as kali using Pa$$w0rd as the password.

2. Return to the Terminal window.

3. Open nslookup in interative-mode by entering: nslookup

    - Nslookup is a program to query the Internet Domain Name System (DNS) by submitting requests directly to domain name servers. Nslookup operates in one of two modes: interactive or non-interactive.
      - Interactive mode allows a user to search domain name servers for IP and name information on various hosts or domains. It can also be used in this mode to display a list of hosts in a domain.
      - Non-interactive mode just returns the name or IP information requested for an individual host or domain.

    - Windows also has a native nslookup command, which operates for the most part like the version in Linux or macOS.

4. Check the lookup server being used by nslookup by entering: server

5. The results should indicate 203.0.113.226 is being used as the default server.

    - By default, nslookup uses the same DNS server to perform queries as the host. However, the server sub-command can be used to change the DNS server used to perform lookups.

6. View the address resource records of the FQDN of the target by entering: www.structureality.com

    - You may see an error of ";; communications error to 203.0.113.226#53: timed out" followed by the actual results from the DNS server. This is an artifact of the simulated internet in the lab environment. This error can be ignored.

7. The results should show an IPv4 address of 203.0.113.1. This is the same address discovered through the ping operation in an earlier exercise.

    - Notice the caveat statement above the results of "Non-authoritative answer:". This indicates the results are being returned from a caching DNS server instead of directly from an authoritative server. It is good practice to work directly against an authoritative server. This lab environment is limited to using only IPv4. If IPv6 were available, you would also see any defined IPv6 address associated with this FQDN.

8. Resolve the nameserver discovered from the prior whois exercise of ns.structureality.com into its IP address by entering: ns.structureality.com

9. Change the lookup server for nslookup to the IP address of ns.structureality.com by entering: server <NSrecord>

10. Perform the target's FQDN address query again by entering: www.structureality.com

    Notice the results no longer have the caveat line. Therefore, the results are coming directly from an authoritative DNS server.

11. Change the resource record type to view the authoritative DNS servers related to the registered domain name by entering: set type=ns and press ENTER followed by structureality.com
  
    The results should show the nameserver of ns.structureality.com with an IPv4 address of 203.0.113.225

12. Change the resource record type to view the SMTP email servers related to the registered domain name by entering: set type=mx press ENTER followed by structureality.com

    The results should show the mail exchanger FQDN of mail.structureality.com.

13. Change the resource record type to view the authoritative DNS servers related to the registered domain name by entering: set type=a press ENTER followed by mail.structureality.com

    The results should show that the IPv4 address of mail.structureality.com is 203.0.113.1

14. Change the resource record type to view the canonical names (i.e., CNAME resource record) related to a registered domain name by entering: set type=cname press ENTER followed by website.structureality.com

    The results should show that the FQDN of website.structureality.com resolves to another FQDN of www.structureality.com.

15. Change the resource record type to view the SOA record related to the registered domain name by entering: set type=soa press ENTER followed by structureality.com

16. The results should be similar to the following:

    origin = structureality.com
    mail addr = hostmaster.structureality.com
    serial = 2023070301
    refresh = 3600
    retry = 600
    expire = 86400
    minimum = 600

    - The serial value may be different in your display as any edits to a DNS zone file causes an incrementation of the serial number.

    - The mail addr line in the SOA record is the email address to be used when needing to contact someone about the registered domain. However, it does not look like a standard email address. That is because the @ symbol is not an allowed character in DNS. So, the convention is to replace the @ symbol with a period. Therefore, you must replace the first period with an @ symbol to return it to the proper structure of an email address.

17. Exit interactive-mode nslookup by entering: exit

    - The tool of nslookup is a powerful DNS utility, but it does not lend itself easily to recording its operations and results (at least not in interactive mode). So while you can use nslookup in non-interactive command mode, in this exercise, you will use dig to perform the same queries and capture the output into a file to retain this information for your pentest report.

18. Use the dig utility to extract DNS information from an authoritative DNS server related to the registered domain name of structureality.com and capture the output into a file by entering: dig @203.0.113.225 structureality.com > target_dns.txt

19. View the captured output of the dig operation by entering: cat target_dns.txt

    Notice that this operation captured the SOA record for structureality.com. While not clearly labeled in the dig output, the same values of origin, mail addr, serial, refresh, retry, expire, and minimum are present.

20. Capture the dig output from www.structureality.com into the same output file, but don't lose the existing content by entering: dig @203.0.113.225 www.structureality.com >> target_dns.txt

    - The use of double greater-than symbols (i.e., >>) performs an append rather than a replace function when capturing output into a file.

21. View the captured output of the dig operation by entering: cat target_dns.txt

    Notice the output shows the same A record result seen previously when using nslookup.

22. Capture the dig output from structureality.com for the resource record type of MX into the same output file, but don't lose the existing content by entering: dig @203.0.113.225 structureality.com -t mx >> target_dns.txt

23. Capture the dig output from structureality.com for the resource record type of NS into the same output file, but don't lose the existing content by entering: dig @203.0.113.225 structureality.com -t ns >> target_dns.txt

24. View the captured output of the dig operation by entering: cat target_dns.txt.

    - You could alter this command to cat target_dns.txt | more in order to view one page of the file at a time. Use the SPACEBAR to advance to the next page and q to exit the more utility.

    You will need to scroll to view the entire output. Notice the output shows the same MX and NS record results seen previously when using nslookup.

25. Leave the Terminal window open.

#### Check your work

Confirm that you used nslookup in interactive mode.

Confirm that you retrieved results from an authoritative DNS server.

Confirm that you enumerated the IP addresses of the target's FQDN, name server (NS), and email server (MX)

Confirm that you viewed the target's SOA record.

Confirm that you used dig to capture DNS information into a file.

### Perform Google Dorking

Google Dorking is the activity of using Google’s advanced search expressions to gain more control and focus on search operations. This is also known as Google hacking and learning Google foo. In this exercise, you will perform a few examples of Google Dorking.

- The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser. The  feature can be used to facilitate the pasting of items into your local browser.

1. On your local computer, open another tab in your current browser or open a new browser.

    - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

2. In your local browser's address bar, enter www.google.com.

3. Using Google search expressions including site and filetype, attempt to discover a link to the robots.txt file hosted at twitter.com using the following search query construction:

    site:twitter.com filetype:txt robots

4. This search query should show results that include www.twitter.com/robots.txt. Select this link.

    You should see the presentation of a text file that has instructions for various search engine spidering bots (identified by the term “User-agent”) related to directories and pages that can (via an “Allow:”) or cannot (via a “Disallow:”) be indexed. Looking through a robots.txt file could reveal interesting locations where sensitive or important data could be stored.

5. In your local browser's address bar, enter www.google.com.

6. Using Google search expressions including site and intitle, attempt to discover pages related to passwords on linkedin.com using the following search query construction:

    intitle:password site:linkedin.com

    This search query should show results from LinkedIn which are related to passwords.

7. Select any link to visit the page. After viewing it, select the Back arrow on your browser to your search results. You can repeat this several times to explore other search results.

8. In your local browser's address bar, enter www.google.com.

9. Using Google search expressions including filetype and the phrase “enable password 7”, attempt to discover results that reveal an insecurity in Cisco devices on the Internet. Use the following search query construction:

    filetype:cfg “enable password 7”

    The results should include a link hosted on www.opus1.com. Select ONLY that link.

10. This is the configuration file (i.e., .cfg file) of a Cisco switch which includes the password 7 hash format of the primary password for this device. Look through the file to locate the line that starts with “enable password 7”. Notice the hash value of 09424F0A170414425D.

11. In your local browser's address bar, enter the following URL:

    https://www.ifm.net.nz/cookbooks/passwordcracker.html

    This is the URL of a site that hosts a Cisco Password-7 Hash cracker.

12. Once the “Cisco Password Cracker” page opens, enter 09424F0A170414425D into the “Type 7 Password:” field.

13. Select Crack Password.

    - Notice how quickly the result is displayed. This password hash format is not secure.

    - The nap_lkdwncisco3550.cfg file hosted as opus1.com is a demonstration file rather than a live or active file relevant to a current device. This file is used for demonstration purposes in many OSINT presentations.

    - The Cisco Password-7 password hash is a deprecated hashing mechanism. Today it can be compromised in less than 1 second, so it should no longer be in use on any system anywhere.

14. In your local browser's address bar, enter www.google.com.

15. In the Google search field, enter blue suede shoes.

    - Notice that there are over 100 million results. By default, Google search breaks up multi-word search words, locates them in any order or distance from each other in results, varies plurality, and more.

16. In the Google search field, now at the top of the search results page, add double quotes around the search terms, or highlight the current terms, then enter ”blue suede shoes”.

    - Notice that there are only a few million results. The use of double quotes causes Google Search to use your search keywords exactly as you typed them without any variations.

17. On the Google search results page, locate and select the Quick Settings icon, which looks like a gear and is located in the top right. From the presented menu, select Advanced search.

18. On the Google Advanced Search page, you are presented with numerous fields that perform different search functions and use various search operators. Read the instructions to the right of each field.

19. In the “all these words:” field, enter pants shirt

20. In the “none of these words:” field, enter purple red

21. In the “numbers ranging from:” field, enter 10 in the first field, then in the second field after the “to” enter 189.

22. Select Advanced Search located below all the search option fields.

    - Notice the results page shows the resulting search expression based on the entered values of: pants shirts “blue suede shoes” -purple -red 10..189

    - There are dozens of Google search operators that can be used to help refine searches. A great repository of them is: https://ahrefs.com/blog/google-advanced-search-operators/. However, websites change and disappear over time. You can find other options by using the search keywords of “search operators”.

    - Another great source of examples of Google dorks is the Google Hacking DataBase (GHDB) hosted at https://www.exploit-db.com/google-hacking-database. This is an ever-growing collection of search expressions that can assist in locating interesting information collected by the Google indexing spider bot.

23. Close your local browser tab focused on Google's website and switch back to the browser tab focused on the Security+ Skillable virtual lab environment.

#### Check your work

Confirm that you used Google search expressions.

Confirm that you performed Google Dorking to find interesting information.

Confirm that you experimented with Google Advanced Search.

### Perform OSINT using Netcraft

There are a myriad of online research tools that can be used to perform OSINT gathering about online sites and services. One of these is Netcraft. A recent marketing statement from their site is: "Combining detection, threat intelligence and robust disruption & takedown, Netcraft’s automated digital risk protection platform keeps your organization and customers safe from phishing, scams, fraud and cyber attacks.". In this exercise, you will use Netcraft's "What’s that site running?" service to discover information about a website.

- The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser. The  feature can facilitate the pasting of items into your local browser.

1. On your local computer, open another tab in your current browser or open a new browser.

    - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

2. In your local browser's address bar, enter www.netcraft.com.

    - If a cookie notification appears, select Accept.

3. Scroll down to the bottom of the page and locate the “What’s that site running?” search field.

    - The “What’s that site running?” search field is located about 1/3 of a page up from the bottom of the main page at Netcraft's website.

4. In the “What’s that site running?” search field, type http://comptia.org, then select Analyze.

   A page titled “Site report for http://comptia.org” should be displayed.

    - The information collected and presented by Netcraft is a combination of public data pulled in real-time from various sources and historical data collected by Netcraft’s own polling, probing, and indexing operations.

  Scroll through the page and view the listed contents. It will include details grouped into numerous sections.

6. In the Network section, on the “Domain” line in the second column, select the link comptia.org.

  This should display a page titled “Hostnames matching *.comptia.org.

    - Notice that there are dozens of sub-domains defined and managed by Comptia.

7. Close your local browser tab focused on Netcraft's website and switch back to the browser tab focused on the Security+ Skillable virtual lab environment.

#### Check your work

Confirm that you performed research on URLs at Netcraft.

### Perform a background search on a person

A key element of OSINT is gathering details about people associated with a target organization. There are many sites that can be used for this purpose. In this exercise, you will use the people search engine of PeekYou and the government database indexing service of SearchSystems.

The PeekYou site is designed to help locate information about individual people.

- The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser. The  feature can facilitate the pasting of items into your local browser.

1. On your local computer, open another tab in your current browser or open a new browser.

    - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

2. In your local browser's address bar, enter www.peekyou.com.

3. In the search fields, provide information about Elvis Presley. In the First Name field, enter Elvis. In the Last Name field, enter Presley.

4. In the Location field, select the pull-down list, locate and select Tennessee.

5. Select the Search button, which looks like a white magnifying glass over a green background.

    - Notice the results have several people listed. Select the result of *Elvis A. Presley.

6. Scroll through the details page about this person. Notice the types of information that has been gathered about this person and the links to external sites for additional information.

    - PeekYou is just one of dozens of people search engines. You can experiment with other people’s names to see what you can discover. Realize you could also search yourself, and you could be surprised by what information has been collected about you. On many of these sites, you can request to have your information removed, but this option is often hidden and may not obtain fast results.

7. In your local browser's address bar, enter www.searchsystems.net to access SearchSystems to view the 70,000+ public databases maintained by your taxes which contain information about individuals.

    - The sites indexed on SearchSystems.net are public record databases. As a US citizen, you have the right to access and view these records. Some linked sites will allow for information access with a basic search. Some of the linked sites will require that you create an account and confirm your email address before allowing you to search for information. A few of these sites are restricted to confirmed and approved regional residency (such as a state, county, or city), which may require that you provide proof of residence before you can access their records.

    - Notice the top or initial portion of the site is an advertisement. If you click on a link and are taken to a new/different URL other than searchsystems.net when you were not expecting to leave this site, you likely clicked on an advertisement.

8. In the “Search Free Public Records” box, notice the options of In a State, By Type of Record, In a County, By City, By Zipcode, Nationwide, and In Other Countries.

9. Leave the default selection of “In a State”, then select the pull-down list *Select a State, then select the name of your state.

    - If you are not in the United States, then select any state.

10. This should display a page listing the Public Record databases from the selected state. Scroll down the list to view the range and type of databases that can be accessed.

11. Select Home from the page's top menu or select your browser's Back arrow to return to the previous/home page.

12. Select the Nationwide radio button.

13. Select the pull-down list Nationwide, then select Most Wanted.

  This should display a page of Most Wanted links, including a link to the FBI Wanted Search Center.

    - You are welcome to explore the various databases you discover on SearchSystems. Each site you visit will likely be hosted and operated by a different entity, so the sites' quality and ease of use can vary greatly.

14. Close your local browser tab focused on SearchSystem's website and switch back to the browser tab focused on the Security+ Skillable virtual lab environment.

#### Check your work

Confirm that you performed personal information searches.

Confirm that you explored the public record database options through SearchSystems.
