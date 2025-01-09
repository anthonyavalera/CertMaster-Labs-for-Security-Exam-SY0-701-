# Assisted Lab: Performing Vulnerability Scans

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.2 Explain common threat vectors and attack surfaces.
2.3 Explain various types of vulnerabilities.
4.3 Explain various activities associated with vulnerability management.
4.4 Explain security alerting and monitoring concepts and tools.
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

### Access the GSA vulnerability scanner

In this exercise, you will initiate access to the Greenbone Security Assistant (GSA) vulnerability scanner interface.

The vulnerability scanning tool you will use in this lab was previously known as OpenVAS (Open Vulnerability Assessment Scanner). However, it has changed management ownership and has been renamed numerous times. This scanner is currently managed by Greenbone Networks GmbH (greenbone.net) and is known as the Greenbone Security Assistant. However, the commands use the acronym GVM, which stands for Greenbone Vulnerability Manager (its immediately previous name).

Greenbone Security Assistant (GSA), while pre-installed on Kali, is not ready to run by default. Many updates need to be installed, and a significant number and volume of security information feeds need to be downloaded. These updates have already been performed in the lab environment since this lab environment is a self-contained, private network. Therefore, updating the GVM/GSA tool is unnecessary (and impossible) for this lab.

If you work with GSA/GSM from your own Kali instance, you will need to update the tool before use. The commands gvm-setup and gvm-check-setup should be used. However, it can take 30 mins or more for a full update to complete. Therefore, updating the GVM/GSA tool is unnecessary (and impossible) for this lab.

...less
Connect to the KALI and sign in as root using Pa$$w0rd as the password.

Open a Terminal window, then enter gvm-start to launch Greenbone Security Assistant.

The Greenbone Security Assistant (GSA) may take a few moments to initiate. Once its background services are active, Firefox, the default web browser, will open to https://127.0.0.1:9392.

If an "Warning: Potential Security Risk Ahead" page is displayed when attempting to access 127.0.0.1, select Advanced, scroll down, and then select Accept the Risk and Continue.

Sign into the Greenbone Security Assistant (GSA) by typing admin and Pa$$w0rd into the Username and Password fields, respectively, then select Sign In.

If you allow the GSA/GVM interface to time out (after 15 mins), you will be returned to this initial Sign in to your account screen. You will need to sign in again to continue.

Leave the Firefox browser open to the GSA interface.

Check your work
Confirm that you launched the GSA interface.
Confirm that you logged into the GSA interface.

### Configure a target for a vulnerability scan with credentials

Create a scan target with admin credentials. This target will be used to perform a credentialed vulnerability scan. The target will be the Windows Server 2016 system named MS10, which uses the IP address 10.1.16.2.

Connect to the KALI and, if needed, sign in as root using Pa$$w0rd as the password.

Return to the Firefox browser open to the GSA interface.

From the GSA toolbar, select Configuration, then select Targets.

Select the New Target icon. It is located in the top left area of the page and is a rectangle with a star.

On the New Target window, in the Name field, select the default Unnamed value and enter MS10-with-creds to replace it.

In the Manual field of Hosts, enter 10.1.16.2.

Scroll down to view the SMB field under the Credentials for authentication checks heading.

What types of credentials can be defined for a scan target? (Select all that apply)

SMTP
ESXi
LDAP
SSH
SNMP
SMB
TLS
Select the Create a new credential icon to the right of the SMB field. It looks like a rectangle with a star.

Sec-Lab17-GSA-Create-a-new-credential.jpg

On the Create new SMB credential window, in the Name field, select the default Unnamed value and enter MS10 admin to replace it.

Select Yes for Allow insecure use.

Enter structureality\jaime in the Username field and Pa$$w0rd in the Password field.

Sec-Lab17-GSA-Create-new-SMB-credential.jpg

Credentials can also be defined through the Configuration menu's Credentials option.

Select Save.

You should be returned to the New Target window where the MS10 admin credential should be selected (i.e., displayed) in the SMB field.

Leave all other fields and values at their defaults.

Select Save. Due to the window's position, the Save button may be located off the bottom of the screen.

You may need to click-hold-drag-release the header of the New Target window to move it up to access the Save button.

Select the Score button to validate this task:

You have now created a target that can be scanned with credentials.

A scan target without credentials is created using the same process, but you simply don't select or define credentials.

Using a credentialed scan may often provide a more thorough perspective on the weaknesses of a target. Therefore, performing three vulnerability scan variations may be worthwhile, including an uncredentialed scan, a credentialed scan with a standard user account's credentials, and a credentialed scan with an administrator account's credentials. Such scans provide you with the perspective of discoverable attack points of an anonymous connection, a standard user, and an administrator.

...less
Leave the Firefox browser open to the GSA interface.

Check your work
Confirm that you defined a scan target with credentials.

### Configure and initiate a credentialed scan task

Create and run a credentialed vulnerability scan using the pre-defined credentialed scan target.

Connect to the KALI and, if needed, sign in as root using Pa$$w0rd as the password.

Return to the Firefox browser open to the GSA interface.

On the GSA menu bar, select Scans, then select Tasks.

Sec-Lab17-GSA-Scans-menu.jpg

On the Tasks page, select the New Task icon. It is located in the top left area of the page and is a rectangle with a star. Then, from its pop-up menu, select New Task.

Sec-Lab17-GSA-New-Task.jpg

In the Name field, enter MS10 Scan with creds

In the Scan Targets field, use the pull-down list to select MS10-with-creds.

Leave all other fields and values at their defaults.

Sec-Lab17-GSA-New-Task-MS10-Scan-with-creds.jpg

Select Save. Due to the window's position, the Save button may be located off the bottom of the screen.

The new task named MS10 Scan with creds should now be displayed at the bottom of the Tasks display.

Select the Start icon to run the MS10 Scan with creds. The Start icon is a right-pointing arrowhead located to the right on the same line as the MS10 Scan with creds task name.

Sec-Lab17-GSA-Start-scan.jpg

The Status indication for MS10 Scan with creds will first display Requested, then Queued, eventually showing a percentage progress bar as the scan runs.

This scan takes over 5 hours to complete fully. However, you will view the in-progress scan report in a later exercise.

You have defined and started a credentialed vulnerability scan

Configuring and running an uncredentialed scan task follows the same process. Only you would need to select a scan target without credentials in the Scan Targets field. You are only running a single scan in this exercise to allow all system resources of the target to be used for that singular operation. This will allow for results to be obtained faster than if two scans were running simultaneously. You will not have enough time to allow the scan to run to completion.

Wait for the scan's Status indicator to change to 0 %.

Leave the Firefox browser open to the GSA interface.

Check your work
Confirm that you initiated an uncredentialed vulnerability scan.

### Review a GSA Vulnerability Scan Report

Although the credentialed scan initiated in an earlier exercise has not been completed, you are able to view the results of active scans currently in progress. The Report and Results displays of scans automatically update as new information is obtained by the scanning process.

Connect to the KALI and, if needed, sign in as root using Pa$$w0rd as the password.

Return to the Firefox browser open to the GSA interface.

On the GSA menu bar, select Scans, then select Reports.

In the GSA vulnerability scanner, a Report is the result of a single scan event.

The Reports page should be displayed. Scroll down to see the available reports. They will be listed by date/time order.

If the scan results summary on the Reports page does not yet show any results. Wait a few more minutes until the Status percentage is at least 2 %.

It should take less than 5 mins for some results to be available in the scan report.

Select the currently running scan's date and time in the Date column for the MS10 Scan with creds scan task.

The Report page for the selected scan will be displayed.

On the Information tab of the Report, notice a summary of the scan task.

Select the Results tab.

If no results are displayed, select Remove Filter from the top of the page (the icon is an X).

Notice the discovered vulnerabilities are sorted by default in severity order (highest to lowest). You should see columns including Vulnerability, Severity, QoD (Quality of Detection), Host IP, Host Name, and Location (i.e., port number or type).

Scroll down to look over the names of the detected vulnerabilities.

The longer you wait, the more vulnerabilities will be discovered and presented on this tab. However, as long as there is a single vulnerability, you can continue with the exercise.

Select one of the names of a discovered vulnerability to expand its details. Click on the small magnifying glass icon on the left hand side to reveal the full details. Then, scroll down to read the displayed details.

7zi1fhhd.jpg

As you scroll through a vulnerability’s details, notice the various information sections.

When you expand a vulnerability's details summary, which of the following sections of information are displayed?

References
Detection Method
Impact
Solution
Affected Software/OS
Insight
Detection Result
Exploit Source
Once you have looked over the summary of information for a vulnerability, select the same vulnerability name again to hide/collapse the details.

Review the details for a few vulnerabilities (if more than one is available).

Select the Hosts tab to view the information presented about the targeted hosts. Notice the range of details provided, including IP Address, Hostname, OS, Ports, Apps, High, Medium, Low, False Positive, and Severity.

Select and view the contents of each of the rest of the tabs of the report, including Ports, Applications, Operating Systems, CVEs, Closed CVEs, TLS Certificates, Error Message, and User Tags.

The other tabs may not have any information until much further in the progress of the scan. You can continue on and return here before exiting the lab to view added details to these tabs.

The GSA reports display both CVEs and Closed CVEs. The Closed CVEs are issues discovered by the GSA vulnerability scanner but which returned a “not vulnerable” exit code when probed. This should mean that the target is not exploitable by the specific vulnerability. The CVEs are issues that are still likely a problem on a particular target and need to be addressed. As system administrators address security problems through patches and security mechanisms, subsequent re-scans will move handled items from the CVEs list to the Closed CVEs list.

...less
GSA supports exporting reports into several report formats, including Anonymous XML, CSV Results, ITG, PDF, TXT, and XML. However, many of these format types require specific tools or utilities to view or use. In addition, some of the formats are not intended for direct viewing but may be used to import into databases or other security utilities.

Leave Firefox accessing GSA open.

Check your work
Confirm that you viewed the report of a GSA vulnerability scan.

### Explore CVE, NVD, and CVSS

Explore the reference details of a discovered vulnerability.

Select Scans from the GSA menu bar, then select Results.

Reports are the details from an individual scan. Results are the collection of findings from all scans.

Notice the Vendorfix icon for this vulnerability (located under the Solution type icon (which looks like a puzzle piece)). This indicates a patch or update is available from the product vendor to resolve this issue.

Sec-Lab17-GSA-Reports-Vendorfix.jpg

Select the Severity column of the results to sort by this column. You may need to select it twice to sort the most severe items to the top. The triangle beside the column name should be upside down.

Select the top most severe vulnerability's name to expand the Summary information available for that specific vulnerability. Scroll down to view the details.

The longer you allow the scan to run, the more likely the scan will detect the highest severity items present on the MS10 system. However, you can complete this exercise with a vulnerability of any severity level (including 0.0).

Look over and read some of the information provided by the vulnerability’s details presentation. You should see sections titled: Summary, Detection Result, Insight, Detection Method, Affected Software/OS, Solution, and References.

In the References section is a list of URLs to various websites. For some vulnerabilities, there will be a CVE reference.

Notice that the presentation includes numerous links to resources for even more information. Most of these links will pull information that is cached by GSA locally. Some links may lead to external sites which are not accessible from this lab.

If you decide to view details from another item in the results, there is no direct navigation method to return to the previous list of vulnerabilities. Instead, you will need to select Scans from the GSA menu bar, select Results, then select the vulnerability again to return to its details presentation.

Since information about vulnerabilities is cached by GSA, there is the possibility that new information is available since that last update was performed. This is why it is important to always update a vulnerability scanner prior to using it to scan targets.

On your local computer, open another tab in your current browser or open a new browser.

The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser.

Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

In your local browser's address bar, enter cve.mitre.org.

You can highlight and cut-n-paste this URL from the instructions into the address bar of your local browser.

The MITRE CVE database website page should be displayed.

Select Search CVE List.

In the search field, enter CVE-2022-32168.

Sec-Lab17-CVE-search.2.jpg

This is a CVE for one of the more severe vulnerabilities to be discovered on the MS10 system. If your scan was given sufficient time to complete, this would have been listed as a result.

The CVE search results should show a single result.

Sec-Lab17-CVE-search-result.jpg

Select CVE-2022-32168 from the search results.

The details of this CVE record hosted by Mitre are displayed. Scroll to look over the entire record. View the sections of CVE-ID, Description, References, Assigning CNA, and Data Record Created.

CVE stands for Common Vulnerabilities and Exposures. It is an element of the NIST program Security Content Automation Protocol (SCAP) to indicate and reference security issues in a standardized means and method. CVE references numbers are formatted: CVE-YYYY-##### where YYYY is the year the record is initially defined, and the ##### is a number to differentiate the issue from all other CVEs recorded in the same year. CVE references were first defined by the Mitre Corporation, which hosts the United States' National Cybersecurity FFRDC (Federally Funded Research And Development Center). The Mitre CVE database is at cve.mitre.org or cve.org. An additional CVE database hosted directly by NIST is the National Vulnerability Database (NVD) (at nvd.nist.gov). The NVD also provides a CVSS (Common Vulnerability Scoring System) rating for each exploit. Many security organizations have adopted CVE reference numbers to facilitate access to and use of security information.

...less
Hover over the about tab at the top of the screen and select Related Efforts

Click the link for U.S. National Vulnerability Database(VND)

A new tab should open to the page at nvd.nist.gov.

Select Search along the left-hand side of the screen.

Select the option for Vulnerabilities - CVE.

In the search box enter CVE-2022-32168. Then click the result for more details.

Scroll to look over the CVE record hosted by NIST. View the sections of Description, Severity, References to Advisories, Solutions, and Tools, Weakness Enumeration, Known Affected Software Configurations, and Change History

Scroll to view the Severity section. Notice that this CVE entry has a Base Score of 7.8 HIGH. Select 7.8 HIGH.

The Common Vulnerability SCore System Calculator page is opened, with the details for CVE-2022-32168.

Scroll down to view the score graphs. The far-left graph shows three scores: Base, Impact, and Exploitability. The Temporal and Environmental score graphs are often empty but are available for you to adjust to take into account the availability of patches or the security management processes of your environment. The far-right graph is the Overall score (which is 7.8 for the current CVE)

Sec-Lab17-CVE-CVSS-graphs.jpg

CVSS Temporal Metrics are metrics that change over the lifetime of a vulnerability. These metrics measure the current exploitability of the vulnerability, as well as the availability of remediating controls, such as a patch. The subcomponents of Temporal Metrics are: Exploit Code Maturity, Remediation Level, and Report Confidence. If the software vendor has created a widely available patch, the temporal score for that vulnerability will be lower. On the other hand, if there are known and widely available exploits for a vulnerability, the temporal score will be higher. As the availability of patches and exploit code changes, the underlying attributes of the Temporal Metric will change, changing the temporal score and the overall CVSS score.

CVSS Environmental Metrics are modifiers to the Base or static metric group. These account for the aspects of an enterprise that might increase or decrease a vulnerability’s net severity. Environmental metrics are comprised of Modified Base Metrics and Security Requirements.

...less
Published CVSS scores are typically comprised of Base Metrics only. But such a score only the question, "Can this do damage?". If you need to answer, "Can this do damage to my company?" you will need to ensure that you’re also accounting for Temporal and Environmental factors. This is key to successfully operationalizing CVSS scores by refocusing them in your security management program context.

Scroll further to view the Base Score Metrics selections section. If possible, keep the score graphs viewable at the same time.

Switching Firefox into full-screen mode may help keep both sections on the screen simultaneously. Press F11 on your keyboard to switch in and out of full-screen mode.

Sec-Lab17-CVE-CVSS-Base-Score-Metrics.jpg

Notice the CVSS 3.1 Vector located between the graphs and the metric selections. The CVSS Vector is an acronym summary of the overall values of the Base Score Metrics area. You can see that each of the eight metrics has an acronym, and each of those metrics selection options also has an acronym. Watch how the vector changes based on your selections as you perform the next steps.

Experiment with the selections of the Base Score Metrics by clicking on one of the alternate options for each of the eight metrics. Make one change at a time, view the revised calculation of the Overall score, then revert the modified selection to its original value.

Review the screenshot of the original selections of the Base Score Metrics to return the settings to this default. Otherwise, you will have to use the back page function of Firefox to return to the CVE details page, then re-select the CVSS score to return to the calculator page with the original values.

What single metric change raises the CVSS score of CVE-2022-32168 from 7.8 to 8.8?

Set Attack Vector to Network
Set Scope to Changed
Set User Interaction to None
Set Attack Complexity to High
Experiment to discover what Base Score Metrics you need to select to push the Overall score to 10.0.

The Overall CVSS score is based on a scale from 0.0 to 10.0. Here are the divisions of the severity rating scale:


Rating	CVSS Score
Critical	9.0 - 10.0
High	7.0 - 8.9
Medium	4.0 - 6.9
Low	0.1 - 3.9
None	0.0
...less
Expand this hint for guidance on achieving a CVSS score of 10.
To achieve a CVSS score of 10.0, the metric selections must be: AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H

Close the tab(s) in your local browser focused on mitre.org and nist.gov.

Check your work
Confirm that you viewed a CVE record
Confirm that you viewed an NVD record
Confirm that you viewed a CVSS score and used the CVSS calculator
