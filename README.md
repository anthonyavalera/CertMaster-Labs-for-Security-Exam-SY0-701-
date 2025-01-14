# APPLIED LAB: Hardening

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.5 Explain the purpose of mitigation techniques used to secure the enterprise.
3.2 Given a scenario, apply security principles to secure enterprise infrastructure.
4.1 Given a scenario, apply common security techniques to computing resources.

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

### Managing device drivers to remove unneeded devices

While evaluating a system to improve security, you may discover that there are unwanted device drivers present. Knowing how to remove those devices and/or device drivers is an important part of system hardening. In this exercise, you will scan for hardware changes, update a driver, disable and re-enable a device, remove a device driver, and install a device driver.

  - The activities in this exercise demonstrate the functions of device and driver management. You would need to apply these processes properly and effectively in your own environment. For example, when removing a device causing poor system performance or introducing an exploitable vulnerability.

  - The concept of system hardening is focused on two operations: remove and update. One task of hardening is to remove anything that is not needed for a business activity. The other hardening task is to update and secure anything that is needed for a business activity. However, the reality is that you can't typically perform these two functions in the simple order of remove and then update with the result of actually having a properly hardened system. Sometimes you must update first, then remove, then repeat updating, then repeat removing. It can take several iterations of updating and removal to achieve the targeted hardened system result. Thus, hardening is often a more complex process than it might first seem.

1. Select the MS10 VM. Send Ctrl+Alt+Delete and, if needed, sign in as Jaime using Pa$$w0rd as the password.

    - Jaime is a member of the Domain Admins group. So, this user account is an administrator on the MS10 system.

2. Minimize or close Server Manager if it appears. It will not be used in this lab.

3. Open Device Manager and scan for hardware changes.

  Expand this hint for guidance.
    a. Select Type here to search from the taskbar, type device manager, then select Device Manager from the results.

    b. Right-click over any device category, then select Scan for hardware changes from the pop-up menu.

    c. A window may appear, indicating a scan is taking place. If it does appear, once completed, any newly discovered hardware will have its drivers automatically installed.

4. Update the device driver for the Microsoft Virtual DVD-ROM under DVD/CD-ROM drives.

  Expand this hint for guidance.
    a. The Device Manager should still be open.

    b. Select the arrow to expand DVD/CD-ROM drives.

    c. Right-click Microsoft Virtual DVD-ROM, then select Update Driver Software….

    d. The Update Drivers window is displayed.

    e. Select Search automatically for updated driver software.

    f. The utility will then search locally for new drivers.

    - If this system had Internet access, the search for new device drivers would also include the Windows Update server.

    g. You should see a result that states The best driver software for your device is already installed.

    h. Select Close.

5. Disable then enable the Microsoft Virtual DVD-ROM.

  Expand this hint for guidance.
    a. The Device Manager should still be open.

    b. If needed, select the arrow to expand DVD/CD-ROM drives.

    c. Right-click Microsoft Virtual DVD-ROM, then select Disable.

    d. On the Microsoft Virtual DVD-ROM pop-up window, select Yes to disable the device.

    e. The device is now disabled. The icon for the device will now have a small circle with an arrow pointing down.

    f. Right-click the disabled device of Microsoft Virtual DVD-ROM, then select Enable

    g. The Device Manager display will refresh and now display the DVD-ROM drive as enabled again.

    - Disabling and enabling a device is often used as a troubleshooting step to see if this process will cause a device to begin functioning correctly.

6. Uninstall, then reinstall the Microsoft Virtual DVD-ROM.

  Expand this hint for guidance.
    a. The Device Manager should still be open.

    b. If needed, select the arrow to expand DVD/CD-ROM drives.

    c. Right-click Microsoft Virtual DVD-ROM, then select Uninstall.

    d. On the Confirm Device Uninstall window, select OK to remove the device.

    e. Notice that both the Microsoft Virtual DVD-ROM device and the DVD/CD-ROM drives device categories are no longer displayed in Device Manager

    f. Right-click over any device category, then select Scan for hardware changes from the pop-up menu.

    g. A window may appear, indicating a scan is taking place. If it does appear, once completed, any newly discovered hardware will have its drivers automatically installed.

    h. Notice that the Microsoft Virtual DVD-ROM device and the DVD/CD-ROM drives device categories are present again in the Device Manager. Those devices were re-discovered and reinstalled automatically.

    i. Close the Device Manager.

    - Most devices will be automatically installed by Windows when they are connected to the computer. However, knowing how to rescan for hardware, update drivers, disable/enable devices, and even uninstall devices can often be useful functions in repair and troubleshooting scenarios. If a device driver is present that is not associated (i.e., one maliciously planted) with an actual device, it will not auto-reinstall once it is uninstalled.

#### Check your work

Confirm that you scanned for new hardware through Device Manager.

Confirm that you updated device drivers through Device Manager.

Confirm that you disabled and enabled hardware through Device Manager.

Confirm that you uninstalled and reinstalled devices through Device Manager.

### Removing unneeded applications and services

System hardening involves removing anything that isn't necessary for a business activity. This may include native or default applications, services, and protocols as well as those installed by a system administrator. In this exercise, you will remove an application (i.e., CPUID) that is no longer being used. You will also remove the insecure FTP service.

1. Connect to the MS10 VM. Send Ctrl+Alt+Delete and if necessary, sign in as jaime using Pa$$w0rd as the password.

    - Jaime is a member of the Domain Admins group. So, this user account is an administrator on the MS10 system.

2. Minimize Server Manager if it appears.

3. Use Programs and Features to uninstall the CPUID software.

  Expand this hint for guidance.
    a. Select Type here to search from the taskbar, enter programs, and then select Programs and Features from the results.

    b. Select CPUID CPU-Z 2.06 from the list of programs.

    c. Select Uninstall from the header menu. This option appears once you have selected a program.

    d. On the CPUID CPU-Z Uninstall window, select Yes.

    e. A progress window will be displayed during the uninstall process.

    f. Once completed, on the CPUID CPU-Z Uninstall window, select OK.

    g. Close the Programs and Features window.

4. Use Remove Roles and Features to remove the FTP service.

  Expand this hint for guidance.
    a. Open or switch to Server Manager.

    - If Server Manager is not open, select Type here to search from the taskbar, enter server manager, and then select Server Manager from the results.

    b. Select Manage from the top menu bar, then select Remove Roles and Features.

    - The Turn Windows features on or off option in the Programs and Features utility on Windows Server opens the Add Roles and Features Wizard. The Add Roles and Features Wizard is only able to add features, not remove them.

    c. On the Remove Roles and Features Wizard window Before you begin page, select Next.

    d. On the Select destination server page, leave the existing selection, select Next.

    e. On the Remove server roles page, select the arrow beside Web Server (IIS) to expand its contents.

    - You may need to scroll down to see this open in the Roles list.

    f. Select to clear the FTP Server checkbox, then select Next

    g. On the Remove features page, leave the existing selections, select Next.

    h. On the Confirm removal selections page, you should see "Web Server (IIS); FTP Server; FTP Service".

    i. Select to mark the Restart the destination server automatically if required checkbox, and then on the Remove Roles and Features Wizard pop-up window warning you about the automatic restart, and select Yes.

    j. Select Remove.

    k. The Removal progress page will show the status of the process.

    l. Once the removal is complete, the system will automatically reboot.

    - You have now removed the insecure FTP service from the system.

#### Check your work

Confirm that you removed an unwanted application.

Confirm that you removed an insecure service.

### Manipulate hosts file name resolution

While dynamic DNS is a key to modern networks, there are situations where manipulating local hosts file-based name resolution is necessary. In some instances, system hardening will include forcing a specific resolution or blocking a resolution to prevent compromise or vulnerability exploitation. In this exercise, you will manipulate the hosts file to force the resolution of a FQDN to a specific IP address, to prevent the resolution of a specific FQDN, and to remove a false FQDN to IP mapping.

1. Connect to the KALI and sign in as root using Pa$$w0rd as the password.

2. Open a Terminal window and then maximize the Terminal window.

3. Use wget to visit juiceshop.local.

  Expand this hint for guidance.
    a. Run the following command to use wget:
      wget juiceshop.local
    b. Notice the IPv4 address that the juiceshop.local domain name resolves into. For example, it could be 203.0.113.228.  
    c. You should see a result that includes a final statement of index.html saved.

4. View the contents of the current /etc/hosts file.

  Expand this hint for guidance.
    a. Run the following command:
      cat /etc/hosts
    - Notice that there is already an entry in the hosts file for juiceshop.local. This is present due to the configuration of the lab environment, but it is not an uncommon occurrence to define static FQDN to IP relationships in private networks.

5. Introduce a name resolution error by editing the /etc/hosts file using nano to modify the entry to 203.0.113.249 juiceshop.local.

  Expand this hint for guidance.
    a. Run the following command to open the hosts file in nano:
      nano /etc/hosts
    b. Use the arrow keys on your keyboard to move the cursor to the juiceshop.local line.
    c. Edit the entry to read as follows:
      203.0.113.249 juiceshop.local
      - There only needs to be a single space between the IP address and the domain name or hostname. Several spaces or a tab can be used in order to align the names in a column. But alignment is not necessary for functionality.

    d. Type CTRL+X on your keyboard to exit Nano.

    e. Enter y to save the modified buffer.

    f. Press Enter on your keyboard to accept the existing filename.

6. Use wget to visit juiceshop.local.

  Expand this hint for guidance.
    a. Run the following command to use wget:
      wget juiceshop.local
    b. Notice the IPv4 address that the juiceshop.local domain name resolves into. This time it will be 203.0.113.249.
    c. The wget tool will wait for a connection that will never be established. If wget does not self-terminate after several seconds, press CTRL+C on your keyboard to terminate the command.

    - This result demonstrates that a false entry in a local hosts file will prevent access to a real site because the content of the hosts file always takes precedence over a DNS query. This is a means to block access to a FQDN. Simply define a false resolution. One common option is to map the FQDN to the localhost address (i.e., 127.0.0.1).

7. Alter the /etc/hosts file in nano to adjust the false entry for juiceshop.local to 203.0.113.228 juiceshop.local.

  Expand this hint for guidance.
    a. Run the following command to open the hosts file in nano:
      nano /etc/hosts
    b. Use the arrow keys on your keyboard to move the cursor to the juiceshop.local line.
    c. Edit the entry to read as follows:
      - 203.0.113.228 juiceshop.local
    d. Type CTRL+X on your keyboard to exit Nano.
    e. Enter y to save the modified buffer.
    f. Press Enter on your keyboard to accept the existing filename.

8. Use wget to visit juiceshop.local.

  Expand this hint for guidance.
    a. Run the following command to use wget:
      wget juiceshop.local
    b. Notice the IPv4 address that the juiceshop.local domain name resolves into. This time it will be 203.0.113.228.
    c. You should see a result that includes a final statement of index.html.1 saved.

    - The revision of the hosts file to include the line of 203.0.113.228 juiceshop.local enables the resolution of the domain name juiceshop.local to an IP address where there is an operational web server. The index.html page from this site was downloaded by wget. This demonstrates that a DNS entry, whether defined in a local hosts file or anywhere else in DNS, can have a resolution to a functional but still incorrect IP address. The .1 at the end of the index.html filename indicates that there already exists a file with that original name, so subsequent files saved by wget are assigned an incrementing number in order to prevent the overwriting of already retrieved content.

    - You could repeat this lab but instead of using wget you could open juiceshop.local in Firefox after each edit of /etc/hosts.

9. Leave the Terminal window open.

#### Check your work

Confirm that you inserted a false entry into a hosts file.

Confirm that you inserted a false entry that resolves to a functional IP address into a hosts file.

Confirm that you removed a false entry from a hosts file.

Confirm that you evaluated the name resolution before and after each of these hosts file alterations.
