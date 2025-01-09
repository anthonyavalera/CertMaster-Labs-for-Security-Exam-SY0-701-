# Assisted Lab: Configuring Examples of Security Control Types

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

1.1 Compare and contrast various types of security controls.

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

A preventive control attempts to stop an unwanted activity from taking place. In this exercise, you will first perform an undesirable activity. Next, you will implement a preventive control to block that activity. And finally, you will attempt the unwanted activity again to test the preventive control.

A TOOLS folder has been configured on the DC10 server. This folder is designed to hold utilities and data files that must only be accessible to domain and local administrators only. Users without an administrative rule should be prevented from viewing the share.

Verify whether the share has been configured with appropriate permissions by trying to access it using a non-administrative account.

1. Select the PC10 VM. Send Ctrl+Alt+Delete and, select Other user. In the User name box, type Sam. In the Password box, type Pa$$w0rd, and press Enter.

2. From the taskbar, select File Explorer windows 10 icon explorer.png

3. In the File Explorer address bar, enter \\10.1.16.1\TOOLS.

4. You should see the contents of the TOOLS share.

Sam is not an administrator and should not have access. You need to implement a prevention control so that Sam and other non-administrators cannot access this share.

5. Close File Explorer.

6. Select the DC10 VM. Send Ctrl+Alt+Delete and, if needed, sign in as Structureality\Administrator using Pa$$w0rd as the password.

7. In Server Manager, select File and Storage Services, and then select Shares.

8. Right-click the TOOLS share, then select Properties.

9. Select the Permissions tab, and then click Customize permissions.

Which account or group object on the access control list should NOT have been assigned permissions on the share?

Domain Admins
CREATOR OWNER
Users
LocalAdmin
Check your work
Confirm that you implemented a preventive control.
Confirm that you tested a preventive control.
