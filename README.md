# Assisted Lab: Setting up Remote Access

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

3.2 Given a scenario, apply security principles to secure enterprise infrastructure.
5.6 Given a scenario, implement security awareness practices.

### Tools Used

- Remote Desktop Connection
- Command Prompt
- Terminal Emulator
- PuTTY

## Steps

### Use Microsoft Remote Desktop

In this exercise, you will enable then establish a Remote Desktop connection between two Windows systems.

1. Connect to PC10, send Ctrl+Alt+Delete, and sign in as Jaime using Pa$$w0rd as the password.

2. In the Windows search bar, enter settings: system, and then select System.

    - Do not select System Information.

3. In the System window navigation pane, select Remote settings.

4. In the System Properties window, verify that the Remote tab is selected.

5. On the Remote tab, under the Remote Desktop heading, select Allow remote connections to this computer, and then select Select Users.

    - If the Remote Desktop Connection window appears regarding firewall exceptions, select OK.

6. In the Remote Desktop Users window, select Add.

7. In the Select Users or Groups window, in Enter the object names to select, enter Rene, and then select OK.

8. In the Remote Desktop Users window, verify that Rene is now listed, then select OK.

9. In the System Properties window, select OK.

10. Close all open windows.

11. Sign out of PC10 by selecting the Start menu, then selecting Jaime (which will be a circle at the top of the menu), then select Sign out. If prompted that there are open programs, select Sign out anyway.

12. Switch to DC10, send Ctrl+Alt+Delete, and then sign in as Structureality\Administrator using Pa$$w0rd as the password.

13. Minimize or close Server Manager if it appears. It will not be used in this exercise.

14. On the Windows task bar, open Windows search, enter remote desktop, and then select Remote Desktop Connection.

15. In the Remote Desktop Connection window, enter PC10 in the Computer: field, and then select Connect.

16. On the Windows Security window, select More choices, then select Use a different account.

17. Enter Rene in the newly available User name field, then enter Pa$$w0rd in the Password filed, and then select OK.

    - This initial connection to Remote Desktop may take up to a minute to fully establish a connection. This is because a new user profile and home directory are being created for the Rene account. This is the first time this account has been used to sign into this computer.

    - If you are prompted regarding signing out an account on the target computer, select Yes to confirm that you understand that another user is signed in and that they will be disconnected.

18. Notice that your entire display or screen is filled with the Desktop of the remote system. On the Remote Desktop toolbar (located at the top of the screen), select the Minimize icon (which is a single undescore character (i.e., _)).

    - As the Remote Desktop window is minimized, notice the differences between the local Desktop of DC10 and the remote Desktop of PC10.

19. Return to the Remote Desktop window by selecting it from the Taskbar.

20. In the Windows search bar, enter settings: system, and then select System

    - Do not select System Information.

21. In the System window, in View basic information about your computer, in Computer name, domain, and workgroup settings section, verify that the Computer name is PC10.

    - Through the Remote Desktop connection, you are able to use the PC10 virtual machine as though you had signed into it directly.

22. Close the System window.

23. Select Type here to search from the taskbar, type cmd, then select Command Prompt from the results.

24. Enter ipconfig to display the network configiuration of the PC10 system

25. On the Remote Desktop toolbar, select Close (i.e. the X icon) to disconnect the connection.

26. On the Remote Desktop Connection pop-up window stating Your remote session will be disconnected select OK.

27. You are returned to the desktop of the DC10 virtual machine.

    - Remote desktop allows you to connect to a remote computer as though you had walked over to the computer and signed in locally. Remote Desktop can be a convenient tool for remote management of Windows system.

#### Check your work

Confirm that you configured remote desktop access on PC10 to allow remote connections.

Confirm that you connected to PC10 using Microsoft Remote Desktop.

### Confirm the configuration of SSH on Kali Linux

In this exercise, you will confirm that SSH is already present on a Linux system. You will also determine the authentication method supported, confirm the SSH service is active, and determine the Linux system's IP address.

1. Switch to KALI and sign in as root using Pa$$w0rd as the password.

2. Open a Terminal Emulator window by using the icon menu.

    - The icon looks like a small computer screen showing a black background with a white dollar sign cursor prompt.

3. In the Terminal Emulator window, enter apt list openssh-server command to verify that openssh-Server is installed.

4. Verify that the output of the command displays the following statement in brackets: [installed, automatic], which indicates that the SSH service is already installed.

    - SSH is pre-installed on Kali Linux. However, on other Linux distributions, you may need to install SSH by using the apt install openssh-server command. However, this requires interent access which is not available in the lab environment.

5. Determine whether password authentication is enabled in the /etc/ssh/sshd_config SSH configuration file. Enter the following command:

    - cat /etc/ssh/sshd_config | grep PasswordAuthentication
    - Using cat and grep in this manner should produce an output of #PasswordAuthentication yes. The leading octothorp (i.e., number sign, hash, pound, or hashtag sign) character indicates that this is a commented line, thus this setting is not technically in effect. However, the SSH server accepts password authentication by default if no other form of authentication is enabled. This configuration can be edited to change the setting to no and removing the octothorp, which disables password authentication. An alternate authentication method must then be configured.

6. Confirm that the SSH server is already running on Kali by entering the following command:

    - systemctl status ssh
    - The output of the command should include a line that starts with Active: active (running)…. At the bottom of the status display, you should see a line including "Server listening on 0.0.0.0 port 22." This confirms that the SSH service is operational and ready to receive a connection.

7. Type q to exit the status display.

8. Determine the IPv4 address of the Kali Linux system’s eth0 (i.e., Ethernet Zero [i.e., the first NIC]) interface by entering ip a s eth0.

#### Check your work

Confirm that you confirmed the configuration of the pre-installed SSH service on Kali Linux.

### Establish a SSH connection by using password authentication

In this exercise, you will establish an SSH connection between a Windows system and a Linux system. You will first use a GUI utility, then you will use a CLI utility.

1. Switch to PC10, send Ctrl+Alt+Delete, and then sign in as Jaime using Pa$$w0rd as the password.

2. Double-click PuTTY on the Desktop.

    - PuTTY is a free and open-source terminal emulator, serial console, and network file transfer application. It is primarily used to establish remote connections to other devices, such as servers, routers, switches, and more, using various network protocols, including SSH (Secure Shell), Telnet, and serial connections.

3. On the PuTTY Configuration window, type 10.1.16.66 in the Host Name (or IP address) field.

4. Verify that 22 is present in the Port field. All other settings should remain at their default.

5. Select Open to initiate the SSH connection to the Kali VM.

6. On the PuTTY Security Alert pop-up window, select Accept.

7. In the 10.1.16.66 - PuTTY window, at the login as prompt, enter root.

8. In the 10.1.16.66 - PuTTY window, at the root@10.1.16.66's password: prompt, enter Pa$$w0rd.

    - You should be presented with the default SSH welcome message.

9. Enter hostname to view the name of the system

    - The output should be "kali".

10. Enter mkdir remote to create a folder on Kali in the current working directory.

11. Enter exit to terminate the SSH session.

    - Notice that the PuTTY window closes.

12. Select Type here to search from the taskbar, type cmd, then select Command Prompt from the results.

13. In the Command Prompt window, enter ssh root@10.1.16.66.

14. If prompted to continue connecting, enter yes.

15. At the root@10.1.16.66's password: prompt, enter Pa$$w0rd.

    - You should be presented with the default SSH welcome message.

16. Disable the automatic connection message by entering the following command: touch ~/.hushlogin.

17. Enter exit to terminate the SSH session.

18. In the Command Prompt window, enter ssh root@10.1.16.66.

19. At the root@10.1.16.66's password: prompt, enter Pa$$w0rd.

    - The initial connection welcome message was suppressed by the existence of the empty ~/.hushlogin file and not displayed.

20. Enter exit to terminate the SSH session.

#### Check your work

Confirm that you initiated an SSH connection from Windows to Kali Linux by using password authentication using PuTTY (a GUI utility).

Confirm that you initiated an SSH connection from Windows to Kali Linux by using password authentication using CLI SSH.
