# Assisted Lab: Working with Threat Feeds

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

4.3 Explain various activities associated with vulnerability management.

### Tools Used

- CIS
- AlienVault
- Google Exploit Database

## Steps

### Understanding IoC and threat intelligence sources

Sources of IoC (Indicators of Compromise) and threat intelligence feeds are available from numerous open-source community groups, commercial entities, and government agencies. An IoC source or a threat feed can provide you with actionable intelligence to defend against future attacks or discover evidence of previous compromise (i.e., threat hunting).

However, most of these information sources require registration or a paid subscription. This is because an attacker can use the information they provide to avoid detection, so the providers and vendors want to minimize access to this data by the malicious and limit it to those deemed benign. Therefore, these services do not lend themselves to a lab demonstration.

Instead, in this exercise, you will view the introduction to the real-time cyber threat intelligence indicator feeds from CIS. Then, you will explore an example of an IoC report from AlienVault.

  - The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser.

1. On your local computer, open another tab in your current browser or open a new browser.

    - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

2. In your local browser's address bar, enter https://www.cisecurity.org/ms-isac/services/real-time-indicator-feeds.

    - You can highlight and cut-n-paste this URL from the instructions into the address bar of your local browser.

    - When an instruction reads "enter" it is informing you to type in the bolded and/or "quoted" item, then press Enter on your keyboard.

3. The Real-Time Indicator Feeds page from CIS: Center for Internet Security is displayed. Look over this document to learn about the information provided by CIS for free to US State, Local, Tribal, and Territorial entities (SLTTs) (i.e., US-based government entities).

    - CIS is an excellent source of information. However, their feed may be limited to US SLTT entities (which usually means government).

4. In your local browser's address bar, enter https://otx.alienvault.com/browse/global/pulses

5. Enter mirai in the Search OTX field at the top of the page.

  The results should include any AlienVault Pulse related to the Mirai botnet and its related malware.

    - Pulses are collections of Indicators of Compromise, IPs, urls, and file hashes related to potentially malicious activity, contributed by the AlienVault Labs research team as well as other members of the OTX community.

6. Select any of Pulse results which includes Mirai in the title (such as Mirai Bonet IOCs).

7. Look over the page. Notice the colored bar of the various TYPES OF INDICATORS.

8. Notice that the first 10 of many pages of indicators are displayed. The display of indicators is sorted by ADDED date and time by default.

9. Select the item under the INDICATOR heading.

10. The Analysis Overview page for the selected indicator is displayed.

11. Scroll down the page to view the Analysis tab results.

12. Scroll back up and select the Related Pulses tab.

    - This tab lists other IoCs with elements in common with this selected indicator.

13. Use the back arrow button on the browser to return to the AlienVault Pulse page.

14. In the Search: field directly above the indicator table, enter domain.

15. The results will be of indicators with domain names.

16. Select any indicator to view its Analysis Overview page. Then, return to the AlienVault Pulse page using the Back arrow button on the browser.

17. Perform additional searching using the key terms of "URL", "IPv4", "IPv6", "hostname", and "hash".

    - Not all of these search terms will have indicator results for the selected Pulse.

18. Close the tab in your local browser focused on alienvault.com.

This exercise showed you an example of a US government IoC and threat intelligence feed description page and an older IoC page from AlienVault. There are many other IoC and threat intelligence sources to consider, but most require registration to access. Here are several to consider:

  - Cybersecurity and Infrastructure Security Agency:
  https://www.cisa.gov/cybersecurity
  - NIST Computer Security Resource Center:
  https://csrc.nist.gov/
  - FBI InfraGard:
  https://www.infragard.org/
  - SANS Internet Storm Center:
  https://isc.sans.edu/
  - Virus Total Intelligence:
  https://www.virustotal.com/gui/intelligence-overview
  - Cisco Talos Intelligence:
  https://www.talosintelligence.com/
  - SPAMHAUS:
  https://www.spamhaus.org/
  - Crowdstrike:
  https://www.crowdstrike.com/products/threat-intelligence/
  - AlienVault Open Source Threat Exchange:
  https://otx.alienvault.com/
  - Anomali:
  https://www.anomali.com/products/threatstream
  - Mandiant:
  https://www.mandiant.com/advantage/threat-intelligence
  - Abuse.CH:
  https://abuse.ch/
  - Free and open-source threat intelligence feeds:
  https://threatfeeds.io/
    - If you explore these URLs, open a new tab in your local browser.

    - There is a community-managed list of threat intelligence sources and sites maintained on GitHub under the awesome-threat-intelligence project. You can find this project at https://github.com/hslatman/awesome-threat-intelligence

#### Check your work

Confirm that you looked over the CIS feed information.

Confirm that you explored an IoC page from AlienVault.

### Explore The Exploit Database

The Exploit Database is a CVE-compliant archive of public exploits and corresponding vulnerable software developed for use by penetration testers and vulnerability researchers. The Exploit Database is maintained by Offensive Security, an information security training company that provides various information security certifications and high-end penetration testing services.

The Exploit Database is a non-profit project offered by Offensive Security as a public service. They aim to serve the most comprehensive collection of exploits gathered through direct submissions, mailing lists, and other public sources and then present them in a freely-available and easy-to-navigate database.

The Exploit Database is a repository for exploits and proofs-of-concept rather than advisories, making it a valuable resource for those who need actionable data right away.

  - The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser.

1. On your local computer, open another tab in your current browser or open a new browser.

    - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

2. In your local browser's address bar, enter https://www.exploit-db.com/.

    - You can highlight and cut-n-paste this URL from the instructions into the address bar of your local browser.

3. The Exploit Database main page should display listing posted exploit information in reverse chronological order (i.e., most recent at the top of the list).

4. Look over the list of exploits. Notice how many of the postings are very recent.

    - The default Exploits page provides the following information:
      - The *Date* column is sorted in reverse chronological posting order.
      - The *D* or *Download Exploit* column enables you to download the exploit file.
      - The *A* or *Vulnerable Application* column may include a link to download the vulnerable software (if licensing allows)
      - The *V* or *Verified* column indicates whether the site owners have verified the exploit works (with a checkmark) or have not been able to verify (with an X). Note: items that are proven not to work are removed entirely.
      - The *Title* column is a brief description of the target software and the type of exploit
      - The *Type* column indicates if the exploit is DOS, Local, Remote, or a WebApp.
      - The *Platform* column indicates the affected OS or host, such as Windows, Linux, Hardware, PHP, Python, or Multiple
      - The *Author* column indicates the entity creating or at least publishing the information.

    - The most valuable feature of Exploit Database is unique compared to other exploit information sites and services - namely, the ability to download the source code of listed exploits.

5. Select the Filters button.

    - The filter fields are displayed: Type, Platform, Author, Port, and Tag.

6. Select each of the filter fields' pull-down lists to view the options. You can make selections from the pull-down lists to immediately filter the results in the main list.

7. On the left side of the page, there is a compressed side menu. Move your mouse over the side menu to expand it.

8. Select GHDB from the side menu.

9. The Google Hacking Database page is displayed.

    - The GHDB is a collection of search expressions that can be used to find vulnerabilities in websites via Google.

10. Select the Filters button.

11. Select the Category pull-down list to view all the groups/types of Google hacks (a.k.a., Google dorks). Then, select Files Containing Passwords from that list.

12. From the results, select any password-focused Google hacks to try.

    - Since the collection of Google hacks is always changing, we can't indicate a specific one to try. So, pick the first one that seems interesting to you.

13. A details page for the selected Google hack will be displayed. There are usually very few additional details on this page.

14. Select the Google hack code beside the Google Search: indicator.

15. This should open a new browser tab to google.com with the selected Google hack as a search term.

    - Look over the results. But do not visit any of the sites listed in the results at this time.
    - While visiting a site discovered through a Google search is often valid, there is always the possibility that the discovered site is malicious or hosting malicious content. Always perform Google hacking/dorking from a hardened system, such as one hosted in a VM which can be reset and restored to a known secure state in the event of malware exposure.

16. Close the local browser tab focused on the google.com search results to return to the tab focused on exploit-db.com.

17. There are other options to explore in the side menu.

    - Other features of Exploit Database to explore from the side menu include:
      - Security Papers - a collection of non-commercial papers on security and exploitation issues
      - Shellcodes - ready-to-run payload exploit scripts which may be used in combination with a delivery/intrusion exploit
      - SearchSploit: The Manual - a guide to using the Linux tool to search a downloaded cache of Exploit Database

18. Close the tab in your local browser focused on exploit-db.com.

#### Check your work

Confirm that you explored The Exploit Database
