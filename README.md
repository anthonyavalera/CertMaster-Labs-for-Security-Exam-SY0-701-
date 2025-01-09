# Assisted Lab: Using Virtualization

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

3.1 Compare and contrast security implications of virtual networking.
3.2 Create and configure a virtual machine.

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

### Install Hyper-V

Structureality Inc, has asked you to evalute virtulization in Windows Server 2019. In this exercise, you will install the Hyper-V feature on the server.

Connect to the MS10 virtual machine. Send Ctrl+Alt+Delete and sign in as Administrator using Pa$$w0rd as the password. If Administrator is not the default user, select Other User on the lower left part of the screen.

Select the Type Text icon to enter the associated text into the virtual machine.

Right-click the Windows Start button and then select Windows PowerShell (Admin).

Select Yes on the User Account Control window.

Running PowerShell as an administrator ensures you have the necessary permissions to install features.

Run Install-WindowsFeature -Name Hyper-V -IncludeManagementTools to install Hyper-V.

Wait for the install to complete which should take less than one minute.

Select the Windows Start button, select the Power icon, and then select Restart.

If prompted to choose a reason, select Other (Unplanned) and then select Continue.

Restarting the virtual machine will allow Hyper-V to fully install on the virtual machine.

Check your work
Confirm that you installed Hyper-V on the server.

### Configure a new virtual machine

Connect to the MS10 virtual machine. Send Ctrl+Alt+Delete and sign in as Administrator using Pa$$w0rd as the password.

Open Server Manager by entering Server Manager in the search field on the taskbar, and then selecting Server Manager.

Select Tools in the Server Manager window, and then select Hyper-V Manager to open Hyper-V Manager.

Select MS10 in the left menu on the Hyper-V Manager window.

Select New in the Actions menu on the right to review the options.

What is the option below virtual machine?

Press Enter on your keyboard after you type in the value or click out of the text box.

Select Virtual Machine from the New menu in the Actions menu.

Select Next on the Before you Begin page of the New Virtual Machine Wizard.

Enter My Lab VM in the Name: field on the Specify Name and Location page, and then select Next.

Select Generation 1 on the Specify Generation page.

Does Generation 2 support 32-bit operating systems?

Press Enter on your keyboard after you type in the value or click out of the text box.

Select Next on the Specify Generation page.

Enter 1024 in the Startup Memory: field on the Assign Memory page, and select Next.

Always allocate resources carefully to avoid over-provisioning.

Select Next on the Configuring Networking page.

Enter 90 for Size: on the Connect Virtual Hard Disk page, and select Next.

Select Install an operating system later on the Installation Options page.

If you wanted to install an operating system using a bootable CD/DVD-ROM with an image file, what file extension does the image need to be?

Press Enter on your keyboard after you type in the value or click out of the text box.

Select Finish on the Installation Options page to finish creating a virtual machine.

Check your work
Confirm that you created a new virtual machine named My Lab VM.

### Install an operating system on the virtual machine

In Hyper-V manager, right-click My Lab VM and then select Settings.

In the Hardware section, select DVD Drive under IDE Controller 1.

In the DVD Drive window, select Physical CD/DVD drive. Confirm Drive 'D:' is selected and then select OK.

Click WINSERVER-2019-1809 to load the Windows Server installation disk.

In Hyper-V Manager, right-click My Lab VM and then select Connect.

In the My Lab VM on MS10 virtual machine window, select Start. The installation of Windows Server 2019 should start.

It is recommended to maximize the My Lab VM on MS10 window to see all the options in the window.

In the Windows Setup window, select Next.

Select Install now.

In the Activate Windows screen, select I don't have a product key.

In the Select the operating system you want to install window, select Windows Server 2019 Standard (Desktop Experience) and then select Next.

In the Applicable notice and license terms window, select the check box labeled I accept the license terms and then select Next.

On the Which type of installation do you want window, select Custom: Install Windows only (advanced).

In the Where do you want to install Windows window, select Next.

It will take about 5 to 10 minutes to install the operating system on the virtual machine. You do not have to wait for the process to complete. You can continue to the next exercise and create the virtual switches and then come back to this point in the lab after that exercise.

In the Customize settings window, enter Pa$$w0rd in both the Password and Reenter password fields and then select Finish.

Check your work
Confirm that you created a new virtual machine with Windows Server 2019 installed.

### Configure virtual networking switches

Switch back to the MS10 virtual machine.

To enable the MS10 Ethernet adapter, select the Resources tab in the instructions window.

Confirm that MS10 is selected and in the dropdown box under Internal select vLan_Servers.

MS10_Properties.PNG

Select the Instructions tab and continue with the next step.

In Hyper-V manager, select Virtual Switch Manager in the Actions menu on the right of the Hyper-V Manager window to create a virtual switch.

What is the first option for virtual switch type?

Press Enter on your keyboard after you type in the value or click out of the text box.

Select Private and then select Create Virtual Switch on the Virtual Switch Manager for MS10 window.

Virtual Machines connected to a private switch do not have network access to the host machine. They may only communicate to other virtual machines connected to the same private switch. This is a great way to isolate a machine to perform security checks before placing it on the production network.

Enter Private Lab Switch in the Name: field, and then select OK.

To create an External switch, select Virtual Switch Manager in the Actions menu on the right of the Hyper-V Manager window to create a virtual switch.

Select External and then select Create Virtual Switch on the Virtual Switch Manager for MS10 window.

Enter External Lab Switch in the Name: field.

In the Connection type section, verify External network is selected and that Microsoft Hyper-V Network Adapter is selected drop down box.

Notice an external switch has to be associated to a network adapter on the host. This provides external network access outside of the virtual machine and host.

Select OK and in the Apply Network Changes window, select Yes.

Check your work
Confirm that you created a new private switch.
Confirm that you created a new external switch.

### Connect virtual machines with virtual networking

In order to continue from this point, the operating system installation has to be complete. If you skipped ahead while the operating system was installing, please return to the Install an operating system on the virtual machine exercise, switch to the My Lab VM, and complete the installation before continuing with the lab.

On the MS10 virtual machine, right-click the Windows Start button and then select Windows PowerShell (Admin).

If prompted, select Yes.

Enter Ipconfig in the Powershell window and press enter.

Find the IPv4 address for the Ethernet adapter vEthernet (External Lab Switch) and enter it into the box below. It should start with 10.1.

MS10 IPaddress 

Press Enter on your keyboard after you type in the value or click out of the text box.

On the MS10 virtual machine, in Hyper-V, right-click My Lab VM in the Virtual Machines column and select Settings to connect your virtual machine to the new virtual switch.

Select Network Adapter in the Hardware menu on the left column of the Settings for My Lab VM on MS10 window.

Select Private Lab Switch from the Virtual switch dropdown list.

Select OK.

Switch to the My Lab VM nested virtual machine.

To press Ctrl-Alt-Delete in the My Lab VM, select Action on the menu and then select Ctrl-Alt-Delete.

If the My Lab VM is full screen, you need to restore the window by selecting Restore Down in order to see the menu.

Restore.png

Sign in as Administrator using Pa$$w0rd as the password.

In the My Lab VM, right-click the Windows Start button and then select Network Connections.

In the Change you network settings section, select Change adapter options.

Right-click the Ethernet adapter and then select Properties.

Double-click Internet Protocol Version 4 (TCP/IPv4).

Select the Use the following IP addess radio button and enter:

IP address: 10.1.16.225
Subnet mask: 255.255.255.0
Default gateway: 10.1.16.254
Preferred DNS: 10.1.16.1
Select OK twice and then close all open windows on the My Lab VM virtual machine.

In My Lab VM, right-click the Windows Start button and then select Windows PowerShell (Admin).

In PowerShell, enter Ping -4 <IPAddress> which is the IP address of the MS10 virtual machine.

The ping should fail. The private switch only allows network communication to devices connected to that private switch. You cannot even communicate with the host machine.

Leave PowerShell open and switch to the MS10 virtual machine.

In Hyper-V, right-click My Lab VM in the Virtual Machines column and select Settings to connect your virtual machine to the new virtual switch.

Select Network Adapter in the Hardware menu on the left column of the Settings for My Lab VM on MS10 window.

Select External Lab Switch from the Virtual switch dropdown list.

Select OK.

Switch to the My Lab VM nested virtual machine. If a Network pop-up window appears, select Yes.

In PowerShell, enter Ping -4 <IPAddress>. The ping should now be successful.

Switch types:

Hyper-V External switch is linked to a physical card of the Hyper-V host and allows access to the whole network.
Internal switch isolates the virtual machines but allows network switching between the Hyper-V host and the virtual machines.
Private switch completely isolates the network and only allows network access to other devices connected to the private switch.
...less
Close all windows.
Check your work
Confirm that you installed the Hyper-V feature using PowerShell.
Confirm that you opened Hyper-V Manager and navigated through its interface.
Confirm that you created a new virtual machine using the wizard in Hyper-V Manager.
Confirm that you created and connected the virtual machine to a private and external switch.
