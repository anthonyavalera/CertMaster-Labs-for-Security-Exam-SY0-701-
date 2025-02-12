# APPLIED LAB: Implementing a Firewall

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.5 Explain the purpose of mitigation techniques used to secure the enterprise.
4.5 Given a scenario, modify enterprise capabilities to enhance security.

### Tools Used

- Command Prompt
- Windows Defender Firewall

## Steps

### Harden a system by setting firewall rules

In this exercise, assume that a system hardening requirement is to prevent ICMP communications between systems by blocking ICMP through firewall rules. In this exercise, you will first discover that a server system (i.e., DC10) still responds to ICMP communications (specifically ping requests) while another system (i.e., PC10) does not respond. Based on this finding, you apply changes to the DC10 firewall to block future ICMP communications. Finally, you will re-test to verify the firewall rules are taking effect.

1. Connect to the PC10 VM. Send Ctrl+Alt+Delete and, if needed, sign in as Jaime using Pa$$w0rd as the password.

    - Jaime is a member of the Domain Admins group. So, this user account is an administrator on the PC10 system.

2. Test ICMP connectivity between PC10 and DC10 by using Command Prompt and the ping DC10 command.

  Expand this hint for guidance.
    a. Select Type here to search from the taskbar, type cmd, then select Command Prompt from the results.

    b. Run the following command to ping: ping DC10

  The output displays four replies, which indicates that the PC10 system can trigger an ICMP response from DC10. This indicates the DC10 system is not in compliance with the system hardening requirement.

3. Connect to the DC10 VM. Send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

4. Minimize or close Server Manager if it appears. It will not be used in this lab.

5. Test ICMP connectivity between DC10 and PC10 by using Command Prompt and the ping PC10 command.

  Expand this hint for guidance.
    a. Select Type here to search from the taskbar, type cmd, then select Command Prompt from the results.

    b. Run the following command to ping: ping PC10

    - The output displays four Request timed out messages. This indicates that communications were unsuccessful. This is because the settings of Windows Defender Firewall on PC10 are blocking inbound ICMP requests. You need to make similar settings on DC10 to block ICMP responses to queries from other systems (such as PC10).

6. Set the inbound rules of File and Printer Sharing (Echo Request – ICMPv4-In) and File and Printer Sharing (Echo Request – ICMPv6-In) to block ICMP Echo Requests from all network profiles by using Windows Defender Firewall with Advanced Security.

  Expand this hint for guidance.
    a. Select Type here to search from the taskbar, type firewall, then select Windows Defender Firewall from the results.

    b. In the Windows Defender Firewall window, select Advanced settings.

    c. In the Windows Defender Firewall with Advanced Security window, select Inbound Rules.

    d. In the list of Inbound Rules, scroll down to locate the rule that begins with File and Printer Sharing (Echo Request – ICMPv4-In).

      - You may need to expand the column to view the full rule name. This is done by using the click-hold-drag-release method on the dividing line between the Name column and the Group column.

    e. Right-click the File and Printer Sharing (Echo Request – ICMPv4-In) rule and then select Properties.

    f. On the File and Printer Sharing (Echo Request – ICMPv4-In) Properties window, select Block the connection, then select OK.

    g. Right-click the File and Printer Sharing (Echo Request – ICMPv6-In) rule and then select Properties.

    h. On the File and Printer Sharing (Echo Request – ICMPv6-In) Pproperties window, select Block the connection, then select OK.

    i. Close Windows Defender Firewall with Advanced Security and the Windows Defender Firewall window.

    - When adjusting firewall rules, in some instances, simply disabling an active rule will prevent unwanted communication. However, when there are numerous rules, there may be several rules granting access. Therefore, removing one rule from granting access leaves the other rules still granting access. Therefore, an explicit denial may be necessary. A denial always overrides any number of allows.

7. Switch to the PC10 VM. Send Ctrl+Alt+Delete and, if needed, sign in as Jaime using Pa$$w0rd as the password.

8. Test ICMP connectivity between PC10 and DC10 by using Command Prompt and the ping DC10 command.

  Expand this hint for guidance.
    a. Select Type here to search from the taskbar, type cmd, then select Command Prompt from the results.

    b. Run the following command to ping: ping DC10

  The output displays four Request timed out messages. This indicates that communications were unsuccessful. This is because the settings of Windows Defender Firewall on DC10 are now blocking inbound ICMP requests.

    - On the Windows Defender Firewall page, there is a Restore defaults option. However, this option will reset the configuration of Windows Defender Firewall to its Microsoft defaults; therefore, you should avoid using it. To manage the configuration profile more sensibly, you can right-click on Windows Defender Firewall with Advanced Security on Local Computer (i.e., the top item in the left pane of the Windows Defender Firewall with Advanced Security window) and then select one of the three options: Import Policy, Export Policy, or Restore Default Policy. You should use the Export Policy option before changing the firewall’s settings. If your setting changes do not work as expected, you can restore the previous configuration by using the Import Policy feature. Use the Restore defaults option as a last resort.

#### Check your work

Confirm that you blocked ICMP communications on DC10 by managing the Windows Defender Firewall with Advanced Security inbound rules.

### Configure Windows Defender Firewall to manage shared folder access

In this exercise, assume that a system hardening requirement is to prevent client systems from hosting file shares. The company configuration guide indicates that this should be managed through firewall rules. You will first create a share from a client system, then access that share from another system. You will then configure firewall rules to block access to the client’s file share, then test the rules' effectiveness.

1. Connect to the PC10 VM. If needed, send Ctrl+Alt+Delete and sign in as Jaime using Pa$$w0rd as the password.

2. Share C:\LABFILES.

  Expand this hint for guidance.
    a. Select Type here to search from the taskbar, enter File Explorer, and then select File Explorer.

      File Explorer should open to the Quick access list by default.

    b. Right-click LABFILES, and then select Properties.

    c. On the LABFILES Properties window, select the Sharing tab, and then select Advanced Sharing.

    d. On the Advanced Sharing window, select the Share this folder checkbox, and then select OK.

    e. On the LABFILES Properties window, select Close.

3. Connect to the DC10 VM. If needed, send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

4. Attempt to access the LABFILES share from PC10.

  Expand this hint for guidance.
    a. Select Type here to search from the taskbar, enter File Explorer, and then select File Explorer.

      File Explorer should open to the Quick access list by default.

    b. In the address field of File Explorer, enter \\PC10.

  The LABFILES share should be displayed.

    a. Double-click the labfiles share.

  This confirms that access to a share from a client system (i.e., PC10) is currently possible. But since that is against company policy, you will configure a firewall rule on PC10 to block access to any shares from that system.

5. Close any window open to the LABFILES share.

6. Connect to the PC10 VM. If needed, send Ctrl+Alt+Delete and sign in as Jaime using Pa$$w0rd as the password.

7. Disable File and Printer Sharing by using Windows Defender Firewall.

  Expand this hint for guidance.
    a. Select Type here to search from the taskbar, enter Firewall, and then select Windows Defender Firewall.

    b. In the Windows Defender Firewall winddow, select Advanced settings.

    c. In the Windows Defender Firewall with Advanced Security window, select Inbound Rules.

    d. In the list of Inbound Rules, scroll down to locate the rule that begins with File and Printer Sharing (SMB-In) and has Private in the Profile column.

    e. Right-click the File and Printer Sharing (SMB-In) | Private rule and then select Properties.

    f. On the File and Printer Sharing (SMB-In) Properties window, select Enabled, select Block the connection, then select OK.

    g. Right-click the File and Printer Sharing (SMB-In) | Domain rule and then select Properties.

    h. On the File and Printer Sharing (SMB-In) Properties window, select Enabled, select Block the connection, then select OK.

    i. Close Windows Defender Firewall with Advanced Security and the Windows Defender Firewall window.

8. Connect to the DC10 VM. If needed, send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

9. Attempt to access the LABFILES share from PC10.

  Expand this hint for guidance.
    a. Select Type here to search from the taskbar, enter File Explorer, and then select File Explorer.

      File Explorer should open to the Quick access list by default.

    b. In the address field of File Explorer, enter \\PC10.

    - It may take a minute or two, but you will receive a Network Error window stating that Windows cannot access \\PC10. This indicates that the attempt to access the Sales share failed. This is because the current settings of the Windows Defender Firewall on PC10 do not allow File and Printer Sharing across network connections.

10. Wait for a Network Error to be displayed. This error confirms that the firewall block of File and Printer Sharing is enforced on the client.

11. Select Cancel to close the Network Error window.

This confirms that access to a share from a client system (i.e., PC10) is now blocked. Through this configuration change, you have hardened the PC10 client system to be more in line with the company's security policy and configuration baseline.

#### Check your work

Confirm that you blocked access to File and Printer Sharing through the Windows Defender Firewall.
