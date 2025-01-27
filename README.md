# Assisted Lab: Using IPSec Tunneling

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

1.4 Explain the importance of using appropriate cryptographic solutions.
2.5 Explain the purpose of mitigation techniques used to secure the enterprise.
3.2 Given a scenario, apply security principles to secure enterprise infrastructure.
3.3 Compare and contrast concepts and strategies to protect data.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Command Prompt
- Wireshark
  
## Steps

### Configure a host to attempt to negotiate an IPSec VPN

In this exercise, you will define an IPSec policy that will attempt to negotiate an IPSec encrypted transport mode VPN link with any and all systems to which it communicates. However, if the negotiation of encryption fails, it will allow fallback to an unsecured connection.

1. Connect to PC10-v2023-06 (10 IPSEC VM10), send Ctrl+Alt+Delete, and sign in as Admin using Pa$$w0rd as the password.

2. If the Networks banner is shown, select No.

    - If you don't make a selection on this page before it disappears, it won't affect the lab.

3. Select Type here to search from the taskbar, enter local security, and then select Local Security Policy.

4. On the Local Security Policy window, in the left pane, select IP Security Policies on Local Computer.

5. In the right pane, right-click in the empty area, and then select Create IP Security Policy….

6. On the IP Security Policy Wizard window, on the Welcome to the IP Security Policy Wizard page select Next.

7. On the IP Security Policy Name page, type Structureality IPSec Policy (attempt) in the Name: field, and then select Next.

8. On the Requests for Secure Communication page, leave the default setting (i.e., unchecked), and then select Next.

9. On the Completing the IP Security Policy Wizard page, verify the Edit properties checkbox IS selected, and then select Finish.

10. On the Structureality IPSec Policy (attempt) Properties window, select Add.

11. On the Security Rule Wizard window, Welcome to the Create IP Security Rule Wizard page, select Next.

12. On the Tunnel Endpoint page, confirm This rule does not specify a tunnel is selected, and then select Next.

13. On the Network Type page, confirm All network connections is selected, and then select Next.

14. On the IP Filter List page, select Add.

15. On the IP Filter List window in the Name: field, type Structureality IP Filter List, and then select Add.

16. On the IP Filter Wizard window, Welcome to the IP Filter Wizard page, select Next.

17. On the IP Filter Description and Mirrored property page, leave the Description: field empty, confirm Mirrored is selected, and then select Next.

18. On the IP Traffic Source page, select Any IP Address, and then select Next.

19. On the IP Traffic Destination page, select Any IP Address, and then select Next.

20. On the IP Protocol Type page, select Any in the Select a protocol type: pull-down list, and then select Next.

21. On the Completing the IP Filter Wizard page, confirm the Edit properties checkbox is NOT selected, and then select Finish.

22. You are returned to the IP Filter List window. The IP Filter you just defined will be displayed in the bottom window. Select OK.

23. You are returned to the Security Rule Wizard, IP Filter List page, and the Structureality IP Filter List is now displayed in the IP filter lists: field. Select Structureality IP Filter List, then select Next.

24. On the Filter Action page, confirm the Use Add Wizard checkbox is selected, then select Add.

25. On the Filter Action Wizard window, Welcome to the IP Security Filter Action Wizard page, select Next.

26. On the Filter Action Name page, in the Name: field, type Encrypt Some of The Things, and then select Next.

27. On the Filter Action General Options page, confirm Negotiate security is selected, and then select Next.

28. On the Communicating with computers that do not support IPSec page, select Allow unsecured communication if a secure connection cannot be established, and then select Next.

    - In most real-world situations, you do not want to allow fallback to unsecured connections in an IPSec policy. Instead, you would want to define an IPSec policy for those IP addresses that you want to enforce and require IPSec encryption so that you can use non-IPSec encrypted communications on other IP addresses (such as when communicating with an internet site).

29. On the IP Traffic Security page, confirm Integrity and encryption is selected, and then select Next.

30. On the Completing the IP Security Filter Action Wizard page, confirm the Edit properties checkbox is NOT selected, and then select Finish.

31. You are returned to the Security Rule Wizard, Filter Action page, select Encrypt Some of The Things from the Filter Actions: list, and then select Next.

32. On the Authentication Method page, select Use this string to protect the key exchange (preshared key), type Password!Password! in the string field below that selection, and then select Next.

33. On the Completing the Security Rule Wizard page, confirm the Edit properties checkbox is NOT selected and then select Finish.

34. You are returned to the Structureality IPSec Policy (attempt) Properties window, which is now displaying the Structureality IP Filter List in the IP Security rules: field and its checkbox is selected. Select OK.

35. You are returned to the Local Security Policy window, where the newly created IP Security Policy named Structureality IPSec Policy (attempt) is displayed.

    - This IPSec policy is configured to attempt to negotiate an IPSec encrypted transport mode VPN link with any and all systems to which it communicates. However, if the negotiation of encryption fails, it will allow fallback to an unsecured connection.

36. Right-click Structureality IPSec Policy (attempt), and then select Assign.

37. Right-click Structureality IPSec Policy (attempt), and then select Un-assign.

    - You assigned this policy to allow the verification script to be able to locate it in the Registry. You have un-assigned this policy so that no encryption will initially be in use during the third exercise named Confirm whether the IPSec VPN tunnel encrypts communications.

38. Leave the Local Security Policy window open

The IPSec policy that you have defined on PC10 (i.e., Structureality IPSec Policy (attempt)) is not yet in effect. You will next define a similar policy on PC20, and then you will confirm that you can capture network traffic when it is not encrypted. After enabling these policies, you will confirm that traffic between PC10 and PC20 is no longer sent outside of the encrypted tunnel.

#### Check your work

Confirm that you configured an IPSec policy on a client system to support IPSec-encrypted VPN sessions, but still allow fallback to insecure communications.

### Configure a host to require an IPSec VPN

In this exercise, you will define an IPSec policy that will require the use of IPSec in priority with any and all systems to which it communicates and will NOT allow fallback to plaintext.

1. Connect to PC20, send Ctrl+Alt+Delete, and sign in as Admin using Pa$$w0rd as the password.

2. If the Networks banner is shown, select No.

    - If you don't make a selection on this page before it disappears, it won't affect the lab.

3. Select Type here to search from the taskbar, enter local security, and then select Local Security Policy.

4. In the Local Security Policy window's left pane, select IP Security Policies on Local Computer.

5. In the right pane, right-click in the empty area, and then select Create IP Security Policy….

6. On the IP Security Policy Wizard window, on the Welcome to the IP Security Policy Wizard page select Next.

7. On the IP Security Policy Name page, type Structureality IPSec Policy (required) in the Name: field, and then select Next.

8. On the Requests for Secure Communication page, leave the default setting (i.e., unchecked), and then select Next.

9. On the Completing the IP Security Policy Wizard page, verify the Edit properties checkbox IS selected, and then select Finish.

10. On the Structureality IPSec Policy (required) Properties window, select Add.

11. On the Security Rule Wizard window, Welcome to the Create IP Security Rule Wizard page, select Next.

12. On the Tunnel Endpoint page, confirm This rule does not specify a tunnel is selected, and then select Next.

13. On the Network Type page, confirm All network connections is selected, and then select Next.

14. On the IP Filter List page, select Add.

15. On the IP Filter List window in the Name: field, type Structureality IP Filter List, and then select Add.

16. On the IP Filter Wizard window, Welcome to the IP Filter Wizard page, select Next.

17. On the IP Filter Description and Mirrored property page, leave the Description: field empty, confirm Mirrored is selected, and then select Next.

18. On the IP Traffic Source page, select Any IP Address, and then select Next.

19. On the IP Traffic Destination page, select Any IP Address, and then select Next.

20. On the IP Protocol Type page, select Any in the Select a protocol type: pull-down list, and then select Next.

21. On the Completing the IP Filter Wizard page, confirm the Edit properties checkbox is NOT selected, and then select Finish.

22. You are returned to the IP Filter List window. The IP Filter you just defined will be displayed in the bottom window. Select OK.

23. You are returned to the Security Rule Wizard, IP Filter List page, and the Structureality IP Filter List is now displayed in the IP filter lists: field. Select Structureality IP Filter List, then select Next.

24. On the Filter Action page, confirm the Use Add Wizard checkbox is selected, then select Add.

25. On the Filter Action Wizard window, Welcome to the IP Security Filter Action Wizard page, select Next.

26. On the Filter Action Name page, in the Name: field, type Encrypt All The Things, and then select Next.

27. On the Filter Action General Options page, confirm Negotiate security is selected, and then select Next.

28. On the Communicating with computers that do not support IPSec page, confirm Do not allow unsecured communications is selected, and then select Next.

    - This IPSec policy is configured to require negotiation of an IPSec encrypted transport mode VPN link with any and all systems to which it communicates. Therefore, it will not fall back to plaintext if the VPN negotiation fails.

29. On the IP Traffic Security page, confirm Integrity and encryption is selected, and then select Next.

30. On the Completing the IP Security Filter Action Wizard page, confirm the Edit properties checkbox is NOT selected, and then select Finish.

31. You are returned to the Security Rule Wizard, Filter Action page, select Encrypt All The Things from the Filter Actions: list, and then select Next.

32. On the Authentication Method page, select Use this string to protect the key exchange (preshared key), type Password!Password! in the string field below that selection, and then select Next.

33. On the Completing the Security Rule Wizard page, confirm the Edit properties checkbox is NOT selected and then select Finish.

34. You are returned to the Structureality IPSec Policy (required) Properties window, which is now displaying the Structureality IP Filter List in the IP Security rules: field and its checkbox is selected. Select OK.

35. You are returned to the Local Security Policy window, where the newly created IP Security Policy named Structureality IPSec Policy (required) is displayed.

    - An IPSec Policy must be defined on each system that needs to support or require IPSec encrypted connections. There are three primary security methods for an IPSec policy: permit, block, and negotiate security. The permit option will negotiate encryption if another system asks for it. The block option prevents IPSec negotiations. The negotiate security option will always request encryption. In addition, there are sub-options, including Accept unsecured communication, but always respond using IPsec and Allow fallback to unsecured communications if a secure connection can not be established. Therefore, it is important to fully understand your communication encryption needs and intentions, map out the IPSec configuration you intend to deploy on your network, and then only after thorough planning should you attempt to implement the designed and planned IPSec configuration.

36. Right-click Structureality IPSec Policy (required), and then select Assign.

37. Right-click Structureality IPSec Policy (required), and then select Un-assign.

    - You assigned this policy to allow the verification script to be able to locate it in the Registry. You have un-assigned this policy so that no encryption will initially be in use during the third exercise named Confirm whether the IPSec VPN tunnel encrypts communications.

38. Leave the Local Security Policy window open.

The IPSec policy that you have defined on PC20 (i.e., Structureality IPSec Policy (required)) is not yet in effect. You will next confirm that you can capture network traffic when it is not encrypted. Then, after enabling these policies, you will confirm that traffic between PC10 and PC20 is no longer sent outside of the encrypted tunnel.

#### Check your work

Confirm that you configured an IPSec policy on a server system to require IPSec-encrypted VPN sessions.

### Confirm whether the IPSec VPN tunnel encrypts communications

In this exercise, you will capture network traffic before the IPSec policies are assigned (i.e., made effective). This will allow the capture of plaintext communications between PC10 and PC20. Then, once you have assigned the IPSec policies, you will discover that while you can still capture the traffic between PC10 and PC20, that traffic will be encrypted.

1. Connect to PC10-v2023-06 (10 IPSEC VM10), and if needed, send Ctrl+Alt+Delete and sign in as Admin using Pa$$w0rd as the password.

2. Select Type here to search from the taskbar, type wireshark, then select Wireshark from the results.

3. Maximize the Wireshark window.

4. Notice all of the available interfaces listed in Wireshark that could be the focus of a network traffic capture.

    - Wait a few seconds and see activity on the EKG-like line presented for each listed interface. If you don’t already know which interface to select, you can use activity levels to help make that decision.

5. Locate and double-click the Ethernet interface to initiate the collection of network frames on that interface and open the primary Wireshark three-pane display.

6. Select Type here to search from the taskbar, type cmd, then select Command Prompt from the results.

7. From the Command Prompt, enter the following command to ping PC20: ping 10.1.24.102

    - Wait for the command to complete.

8. Enter the following command to ping the default gateway: ping 10.1.24.254

    - There should be four replies from 10.1.24.102 and four replies from 10.1.24.254

9. Select Type here to search from the taskbar, type edge, then select Microsoft Edge from the results.

10. Since this is the first time Edge has been launched on this system, select Start without your data (even though the button is greyed out). Then, select Confirm and start browsing.

    - If you are prompted about setting Edge to be the default browser, select Not Now.

11. In the Microsoft Edge address bar, enter dvwa.structureality.com.

12. Connect to PC20, and if needed, send Ctrl+Alt+Delete and sign in as Admin using Pa$$w0rd as the password.

13. Select Type here to search from the taskbar, type cmd, then select Command Prompt from the results.

14. From the Command Prompt, enter the following command to ping PC10: ping 10.1.24.101

    - Wait for the command to complete.

15. Enter the following command to ping the default gateway: ping 10.1.24.254

    - There should be four replies from 10.1.24.101 and four replies from 10.1.24.254

16. Select Type here to search from the taskbar, type edge, then select Microsoft Edge from the results.

17. Since this is the first time Edge has been launched on this system, select Start without your data (even though the button is greyed out). Then, select Confirm and start browsing.

    - If you are prompted about setting Edge to be the default browser, select Not Now.

18. In the Microsoft Edge address bar, enter dvwa.structureality.com.

19. Switch back to PC10-v2023-06 (10 IPSEC VM10), and if needed, send Ctrl+Alt+Delete and sign in as Admin using Pa$$w0rd as the password.

20. Return to Wireshark by selecting its icon from the taskbar.

21. Select the Stop capturing packets icon on the Wireshark toolbar. This icon looks like a red square.

22. In the Apply a display filter field, enter http.

    - You can also select the Apply display filter button on the far right end of the field, which looks like an arrow.

23. Look over the resulting captured packets.

    - You should see the initial http request from 10.1.24.101 (i.e., PC10) to the website hosted at 172.16.0.201 and the response (a 200 OK message). These captured packets are not encrypted.

    - However, the same website visit from PC20 was not captured. This is because the communications from PC20 (to anywhere other than PC10) are not set to the PC10 interface by the network's routing and switching design.

24. Delete the current display filter by selecting the Clear display filter button located at the far right of the display filter field. The button will be a dark grey X over a light gray square until your mouse cursor hovers over it, and then it will turn red.

25. In the Apply a display filter field, enter icmp and ip.addr==10.1.24.101.

    - Ignore the Destination unreachable ICMP packets from 10.1.24.254. Those are unrelated to your intentional ping traffic for this lab. They are based on attempts of Windows to contact Microsoft internet services, which fail because there is no path to the internet for the lab systems.

    - This filter displays only the ICMP ping traffic to or from 10.1.24.101 (i.e., PC10). You should see groups of 4 (four) sets of Echo (ping) request/Echo (ping) reply communications between 10.1.24.101 and 10.1.24.102, then between 10.1.24.101 and 10.1.24.254, and then between 10.1.24.102 and 10.1.24.101.

26. Delete the current display filter by selecting the Clear display filter button located at the far right of the display filter field. The button will be a dark grey X over a light gray square until your mouse cursor hovers over it, and then it will turn red.

27. Start a new Wireshark packet capture by selecting the Start capturing packets button on the toolbar.

28. On the Unsaved packets… pop-up window, select Continue without Saving.

29. Switch to the Local Security Policy window by selecting its icon from the taskbar.

30. Right-click Structureality IPSec Policy (attempt), and then select Assign.

    - At this point, PC10 is configured to attempt to negotiate an IPSec encrypted session for each communication, but will allow fallback to unsecure communications.

31. Switch to PC20, and if needed, send Ctrl+Alt+Delete and sign in as Admin using Pa$$w0rd as the password.

32. Switch to the Local Security Policy window by selecting its icon from the taskbar.

33. Right-click Structureality IPSec Policy (required), and then select Assign.

    - PC20 is now configured to require the use of IPSec in priority but will NOT allow fallback to plaintext.

34. Switch to the Command Prompt window by selecting its icon from the taskbar.

35. From the Command Prompt, enter the following command to ping PC10: ping 10.1.24.101

    - There should be four replies from 10.1.24.101.

    - You are not pinging 10.1.24.254 nor visiting the website again, as those would not be captured by Wireshark on PC10 anyway.

36. Switch back to PC10-v2023-06 (10 IPSEC VM10), and if needed, send Ctrl+Alt+Delete and sign in as Admin using Pa$$w0rd as the password.

37. Switch to the Command Prompt window by selecting its icon from the taskbar.

38. From the Command Prompt, enter the following command to ping PC20: ping 10.1.24.102

    - Wait for the command to complete.

39. Enter the following command to ping the default gateway: ping 10.1.24.254

    - There should be four replies from 10.1.24.102 and four replies from 10.1.24.254

40. Switch to the Microsoft Edge browser window by selecting its icon from the taskbar.

41. Type CTRL+R to re-load the website of dvwa.structureality.com.

42. Select Instructions from the left-side button menu of the DVWA website.

43. Return to Wireshark by selecting its icon from the taskbar.

44. Select the Stop capturing packets icon on the Wireshark toolbar. This icon looks like a red square.

45. In the Apply a display filter field, enter icmp and icmp.type!=3.

    - This display filter hides everything that is not ICMP traffic and also hides ICMP traffic that has a type value of 3 (which stands for destination unreachable). This is used to suppress the numerous error messages from 10.1.24.254 that are not related to your intentional traffic.

46. Notice that the only ICMP traffic now visible is between 10.1.24.101 (i.e., PC10) and 10.1.24.254 (the default gateway).

    - All other ICMP traffic, specifically that generated by the ping from PC10 to PC20 and from PC20 to PC10, is not visible. You witness the successful operations of these commands, so you know the traffic crossed the network. This traffic was enclosed in an IPSec tunnel. The contents of an IPSec tunnel cannot be evaluated by Wireshark due to the encryption occurring at the network layer (i.e, OSI layer 3).

47. Delete the current display filter by selecting the Clear display filter button located at the far right of the display filter field. The button will be a dark grey X over a light gray square until your mouse cursor hovers over it, and then it will turn red.

48. In the Apply a display filter field, enter http.

    - Notice you can still see the website interaction in plaintext. This is because it is not being encapsulated in an IPSec tunnel.

49. Delete the current display filter by selecting the Clear display filter button located at the far right of the display filter field. The button will be a dark grey X over a light gray square until your mouse cursor hovers over it, and then it will turn red.

50. In the Apply a display filter field, enter isakmp and (ip.addr==10.1.24.101 and ip.addr==10.1.24.102).

    - This display filter limits the results to those packets that are ISAKMP and include both PC10 and PC20 IP addresses.

    - The displayed packets of ISAKMP (Internet Security Association Key Management Protocol) all relate to the encryption negotiation of IPSec. The discovery of ISAKMP communications on the network confirms that IPSec negotiations are taking place.

51. Delete the current display filter by selecting the Clear display filter button located at the far right of the display filter field. The button will be a dark grey X over a light gray square until your mouse cursor hovers over it, and then it will turn red.

52. In the Apply a display filter field, enter esp and (ip.addr==10.1.24.101 and ip.addr==10.1.24.102).

    - This display filter limits the results to those packets that are ESP (Encapsulating Security Payload) and includes both PC10 and PC20 IP addresses.

    - ESP is the primary encryption container for IPSec tunnels. The presence of ESP packets confirms IPSec tunnels were established and encrypted communications are taking place.

#### Check your work

Confirm that you attempted to view traffic related to ICMP and HTTP with Wireshark before and after IPSec policies were assigned.

Confirm that you verified an IPSec encrypted VPN is being established between 10.1.24.101 (PC10) and 10.1.24.102 (PC20).
