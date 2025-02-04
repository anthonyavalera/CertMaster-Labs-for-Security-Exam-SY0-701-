# Assisted Lab: Detecting and Responding to Malware

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.2 Explain common threat vectors and attack surfaces.
2.4 Given a scenario, analyze indicators of malicious activity.
4.1 Given a scenario, apply common security techniques to computing resources.
4.4 Explain security alerting and monitoring concepts and tools.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Joe Sandbox Cloud
- VirusTotal

## Steps

### Explore Joe Sandbox Cloud

As security professional, you encounter suspicious files on a regular basis. You have been tasked with determining if these files are malicious and whether the host systems need to be sanitized. You are aware of an online malware analysis service that may be able to provide detailed insight into these files, but you would like to review the site's features, capabilities, and report options before submitting a sample. In this exercise, you will be evaluating the cloud-based sandbox-focused malware-analysis service of Joe Sandbox Cloud.

- The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser.

1. On your local computer, open another tab in your current browser or open a new browser.

    - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

2. In your local browser's address bar, enter joesandbox.com.

    - You can highlight then cut and paste this URL from the instructions into the address bar of your local browser.

    - If a cookie pop-up appears, select Allow all.

    - Joe Sandbox Cloud is an online version of the Joe Sandbox malware analysis tool that runs in the cloud. Users can upload files and URLs to be analyzed in a secure, cloud-based environment without requiring any local infrastructure. Joe Sandbox Cloud offers various analysis options such as static, dynamic, and hybrid analysis, which helps users to better understand the behavior of malware and develop effective countermeasures.

      Joe Sandbox Cloud has many benefits, including scalability and flexibility. As an online service, it can be accessed from anywhere with an internet connection, making it ideal for distributed teams and remote workers. Additionally, it is highly customizable, allowing users to configure their analysis options and workflows as per their needs.

      Joe Sandbox Cloud provides detailed reporting and analysis, including information on the malware's behavior, such as network traffic, file system changes, registry modifications, and other activities. This information can be used by security professionals and researchers to identify new threats and develop effective defenses against them.

  The Joe Sandbox Cloud Basic website should be displayed showing the "Deep Malware Analysis" page.

3. On the JoeSandbox Cloud Basic pop-up, select the "x" in the top right corner to close it.

4. Look over the "Deep Malware Analysis" page of the Joe Sandbox Cloud service.

    - Notice the following:
      - Choose Analysis Architecture where you can select an OS to detonate a suspect file within.
      - Define Sample Source and Choose Analysis System where you can provide sample code through upload, URL, download & execute, or command line and where you select the OS VMs to use for the analysis.
      - Live Interactions & Results where you can manipulate the sample code and OS environment during the test.
      - Settings where you can provide comments and set the execution time for the analysis.
    - To submit your own samples for analysis, you must register to obtain an account. Registration is free for the use of the basic analysis service. There is a subscription required to use the Pro-level services. You are not required to register to perform this exercise.

5. Select Results at the top of the page.

    This shows the "Analyses Overview" page of all publicly submitted samples. Free malware analysis sample submissions are publicly viewable. Pro-level services can elect to keep sample analysis results private.

6. Look over the various recent result summaries.

    - Look over the numerous columns of information presented in this overview/summary page. There is a legend at the bottom of the screen you can use to interpret the icons in the Info column.

7. In the Search (hash, ID, tag) field at the top of the page, enter Zeus.x86.

    - Zeus.x86, also known as Zbot or WSNPoem, is a type of malware designed to steal sensitive information from infected computers. It is a banking Trojan that primarily targets users of online banking and financial services. Once installed on a victim's computer, Zeus.x86 can capture login credentials, account numbers, credit card details, and other sensitive information that can be used for financial fraud. Zeus.x86 can be spread through various methods, including email attachments, malicious links, and exploit kits. It typically installs itself as a rootkit to avoid detection by security software and can remain hidden on an infected system for an extended period.

  You should see at least a few results related to this malware.

    - If no results are available for Zeus.x86, use ReVIL or RAT.

8. Select the HTML icon on the far left of the result with a "Time & Date" of 2022-04-21 21:51:18 +02:00.

    A detailed "Linux Analysis Report: Zeus.x86" page will be displayed in a new tab.

    - If a result with this date and time stamp is not available, select the HTML icon of the most recent result.

9. Look over the wealth of information provided by the Joe Sandbox Cloud service regarding this malware.

10. After you have looked over the lengthy report, close the tab displaying the "Linux Analysis Report: Zeus.x86" page.

11. In the Search (hash, ID, tag) field at the top of the page, enter Mirai.

    - Mirai is a type of malware that is used to create botnets of infected Internet of Things (IoT) devices. Mirai was first discovered in 2016 and is considered to be one of the most potent and widespread IoT botnets to date. Mirai is constantly evolving, with new variants and updates being released by its creators to evade detection and maintain control over infected devices. It has been used in a variety of attacks, ranging from financial gain to political motives.

  A page with numerous results related to Mirai will be displayed.

12. Look over the results list for Mirai malware. Notice that the percent of detection by Antivirus varies amongst the tested samples.

13. Select the HTML icon to view the report of any of the Mirai results.
  
    A detailed report of the tested sample related to Mirai will be displayed in a new tab.

14. Look over the wealth of information provided by the Joe Sandbox Cloud service regarding this malware.

15. After you have looked over the lengthy report, close the tab displaying the report.

16. Select Results at the top of the page.

    This clears the search results and returns you to the full list of recently submitted samples.

17. Notice along the top of the page are malware terms on red buttons with a fire icon. These are malware concepts that have a recent high occurrence rate. Select one of these threat name buttons to investigate a concept further.

    - The amount of detail and wealth of information about malware provided by the Joe Sandbox Cloud service is astounding. If you are aware of any recent malware that has appeared in the news, try searching for reports on them. If you can't think of any, consider searching the internet for a report on "top malware of 2023" (or another year) to find examples to search on. Here is a list of some example malware names rampant in 2022:
      - SessionManager2
      - Gh0st
      - CoinMiner
      - Agent Tesla
      - NanoCore
      - Ursnif
      - LingyunNet
      - Snugy
      - Tinba
  This list is from the CIS report "Top 10 Malware December 2022" (https://www.cisecurity.org/insights/blog/top-10-malware-december-2022).

  Searching for "CIS top 10 malware" can help you discover newer reports by year or quarter.

Using a malware sandbox analysis service like Joe Sandbox Cloud can provide you with detailed information about potentially malicious files you find in your environment. It is important to remember that anything uploaded to Joe Sandbox Cloud will be publicly viewable unless you subscribe to one of their Pro levels of service. As long as you don't upload a confidential company file, then using this service can be quite informative.

#### Check your work

Confirm that you explored Joe Sandbox Cloud

Confirm that you reviewed reports of several malware samples analyzed by Joe Sandbox Cloud

### File analysis with VirusTotal

If you discover a file that you suspect may be malicious, you can have it analyzed by the online service at VirusTotal. This site will scan the file using over 60 security products and provide you with a report of the findings. In this exercise, you will work with Anti-Malware test files, then use them at VirusTotal.

- The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser.

1. On your local computer, open another tab in your current browser or open a new browser.

    - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

2. In your local browser's address bar, enter eicar.org.

    - You can select the URL to copy it into your local clipboard, then paste it into the address bar of your local browser.

    - If a cookie pop-up appears, select Allow all.

    - European Institute for Computer Antivirus Research (EICAR) was founded in 1991 as an organization aiming to further antivirus research and improve the development of antivirus software. Recently EICAR has furthered its scope to include the research of malicious software (malware) other than computer viruses and extended work on other information security topics like content security, Wireless LAN security, RFID, and information security awareness. EICAR also organizes international security conferences most years, as well as a number of working groups or 'task forces'.

    - The EICAR organization has defined a string to be used to test anti-malware products. This non-functioning, non-executing string of characters is present in most anti-malware scanning products. This likely includes your local browser. The concept is similar to a dummy grenade used by military personnel during training. The next click you make on the EICAR website is to take you to a page where this test code is presented as text and where there are files containing the string can be downloaded. Your browser or other local security tools are likely to display warnings about malicious code. There is absolutely nothing malicious about the EICAR test virus/code. But if the warning concerns you, you do not need to actually visit the page, nor do you need to download any of the EICAR files. All needed elements for this exercise are provided in the instructions later.

3. Select the DOWNLOAD ANTI MALWARE TESTFILE graphic on the home page.

    - Clicking this link DOES NOT download anything to your local machine.

4. Scroll down to view the download links located in the middle of the page. You should see the ability to download four files: eicar.com, eicar.com.txt, eicar_com.zip, and eicarcom2.zip.

    - If your browser or system alerts or warns you regarding this URL, you can access the site anyway or skip this step.

    - These files and this URL/website DO NOT contain any malware. They simply contain a string that has been included in anti-malware scanning products' detection databases which serve as a safe means to test such security products’ ability to detect malware. You can download these files safely to your local machine. However, your local security products might automatically block the download or quarantine/delete the file(s) once downloaded. Fortunately, you DO NOT need to download these files to your local system for this exercise.

5. Leave your local browser's tab open to this EICAR DOWNLOAD ANTI MALWARE TESTFILE page.

6. Switch back to the browser tab focused on the Security+ Skillable virtual lab environment.

7. Connect to the KALI virtual machine and sign in as root using Pa$$w0rd as the password.

8. An icon of a DVD labeled as "Student-Resources-L27.ISO" is on the Kali desktop, but it will be greyed out. Right-click on this DVD icon and select Mount Volume.

    - If the DVD Drive is not present on the Desktop: Select the Resources tab from the lab interface's Instructions area. On the Resources tab, select the DVD Drive pull-down list and select Student-Resources-L27.ISO. Then, select the Instructions tab to return to the lab steps.

9. Open a Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar.

10. Maximize the Terminal window.

11. Enter the following command to view the contents of the DVD Drive:

      ls /media/cdrom0/
12. In the Terminal window, enter the following:

      cp /media/cdrom0/eicar* /root/Downloads/
13. Enter cd /root/Downloads, then ls -l.

      You should see both eicarcom2.zip and eicar.com.txt.

14. Type cat eicar.com.txt to view the contents of the file. It should be:

      X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
    - The EICAR Anti-Virus Test File or EICAR string was developed by EICAR and Computer Antivirus Research Organization (CARO) to test the response of computer antivirus (AV) programs. Instead of using real malware, which could cause real damage, this test file allows the testing of anti-virus software without having to use a real computer virus. Anti-virus programmers set the EICAR string as a verified virus, similar to other identified signatures. A compliant virus scanner, when detecting the file, will respond in more or less the same manner as if it found a harmful virus.

15. Enter the following commands to extract the contents of the EICAR zip and view the contents:

    unzip eicarcom2.zip
    unzip eicar_com.zip
    cat eicar.com
    The contents of eicar.com should be:

    X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
    - The use of the double-zipped file is an attempt to fool anti-malware scanners by obfuscating the EICAR string.

16. Return your local browser's tab open to the EICAR DOWNLOAD ANTI MALWARE TESTFILE page.

17. Right-click the TXT: eicar.com download link and select Copy link address.

    - If you are unable to visit the EICAR site, then use the following URL: https://secure.eicar.org/eicar.com.txt. Select the URL to copy it into your local clipboard.

    - The term or command in your browser may vary. The purpose of this operation is to copy the URL leading to the eicar.com.txt* into your local system's clipboard. So, use your browser's right-click pop-up menu command to accomplish this if the term is different.

18. Open a new tab in your local browser. Leave the tab focused on eicar.org open.

19. In the address bar of the new tab of your local browser, enter virustotal.com.

    - VirusTotal is an online malware scanning service that allows users to upload files and URLs to be scanned for malware by multiple antivirus engines. The results are compiled into a report that provides detailed information on any detected malware or suspicious behavior. VirusTotal also provides analysis tools, such as behavior analysis and file metadata extraction, to help security professionals better understand the submitted files and URLs. VirusTotal is free for individual users and organizations that submit a limited number of files daily. VirusTotal offers a premium service for higher volume use that provides additional capabilities, such as API access and custom analytics.

20. Select URL to select the URL tab of the home page of VirusTotal.

    - If you had a suspicious file on your local system, you could elect to upload it using the File tab of VirusTotal. However, this could reveal confidential material to others. Later in this exercise, you will use the hash submission test method to avoid this potential data leak concern.

21. Select the *Search or scan a URL field, then paste (using CTRL-V or equivalent) the eicar.org URL from your clipboard, then press Enter on your keyboard.

    - VirusTotal creates a hash of the target to determine if the item has already been scanned. VirusTotal maintains a database of all submitted items. This enables them to reconstruct a historical timeline if malware is later discovered and identified. VirusTotal can look back into its archive to see when someone uploaded a file that was infected by the now-known malware. Thus, it is important to remember to only upload non-sensitive files to VirusTotal. Fortunately, you can still use VirusTotal with just a hash of a file (you will perform that task in this exercise).

22. Look over the results of the scan on the DETECTION tab.

    - Notice that while some security products detected the EICAR test string, some do not.

23. Select the DETAILS tab, then look over the provided information.

    - Submitting a URL and uploading a file do not always produce the same results. If you want, you could download the eicar.com.txt file to your local system, then upload it to VirusTotal so you can compare the differences in the scan results between these means of item submission. However, it is not a required element of this exercise to download files to your local system. You can also submit the hash of eicar.com.txt to VirusTotal as an alternative. This will be suggested later in this exercise, where the needed hash is provided.

24. Select each of the other tabs available and look over the provided information.

    - When performing an URL search, you will only see the tabs of DETECTION, DETAILS, and COMMUNITY. If you upload a file, you will also see the tabs of RELATIONS and BEHAVIOR.

25. Select Reanalyze (which is a circled arrow) near the top-right of the page.

    - This function has VirusTotal perform the analysis again. This may be worth doing if you notice the last scanned date (listed as Last Analysis date on the Details tab) is more than a few days ago.

26. Switch back to the browser tab focused on the Security+ Skillable virtual lab environment.

27. In the Terminal window, enter sha256sum eicarcom2.zip to generate a hash of that file. The result should be:

      e1105070ba828007508566e28a2b8d4c65d192e9eaf3b7868382b7cae747b397
28. Double-click the SHA256 hash in the previous instructions step, then right-click and select Copy.

    - There is no ability to copy out of the VM, and the lab environment does not have internet access, so these steps are being used as a workaround to get the SHA256 hash of eicarcom2.zip into the clipboard of your local system, without having to download the file to your local system and perform the hash there.

29. Return your local browser's tab open to the VirusTotal page.

30. Select Return to Front Page (which looks like a blue bookmark or flag) at the top-left corner of the page.

31. You should be back at the initial page of VirusTotal.

32. Select the SEARCH tab.

33. Select the URL, IP address, domain, or file hash text field, right-click, select Paste.

34. The SHA256 hash of the eicarcom2.zip file should now be present on the SEARCH tab. Press Enter on your keyboard.

    - One drawback to using a hash with VirusTotal is that if your unique local file is infected by malware, a hash of that now infected file will not be in the VirusTotal historical database. The hash method of analysis at VirusTotal is only beneficial if you are able to obtain the hash of the malware file (pr infection vector) itself. Otherwise, the host file with the malware embedded would need to be uploaded to VirusTotal. But this could expose confidential information contained in the host file to VirusTotal (owned by Chronicle Security, owned by Google, whose parent is Alphabet) and potentially the public if your host file contains a novel malware sample.

35. The results of the hash database search will be displayed.

    - Notice that VirusTotal was able to identify the file as eicarcom2.zip.

36. Look over the analysis report. Be sure to look at the information on each of the tabs.

    - You could submit the SHA256 hash of the eicar.com.txt to VirusTotal in lieu of uploading that file. Its hash value is:
    - 275a021bbfb6489e54d471899f7db9d1663fc695ec2fe2a2c4538aabf651fd0f
#### Check your work

Confirm that you reviewed EICAR malware test files.

Confirm that you submitted a URL to VirusTotal for analysis.

Confirm that you submitted a hash to VirusTotal for analysis.
