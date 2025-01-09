# Assisted Lab: Using Containers

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

3.1 Compare and contrast security implications of different architecture models.
3.2 Given a scenario, apply security principles to secure enterprise infrastructure

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

### Confirm Docker installation and host VM settings

Connect to the MS10 virtual machine, select Ctrl+Alt+Delete, and then select Other User

Sign in as Administrator with the password Pa$$w0rd.

To verify that Docker has been installed and is running, open Windows search by selecting the magnifying glass on the taskbar and then type Services. In the search results, select Services.

Scroll down the list of services and look for Docker Engine. The service should be listed, and the Status should be Running.

Close the Services windows.

Docker has been pre-installed on the virtual machine to save time and due to internet restrictions.

Select Windows search by selecting the magnifying glass on the taskbar and then type PowerShell. In the search results, right-click Windows PowerShell and choose Run as administrator. Click Yes on the User Account Control window.

Run ipconfig to list the IP information of the MS10 virtual machine.

Enter the IPv4 Address of the Ethernet adapter in the box below. It should start with 10.


In PowerShell, run Winver. A window should open with operating system information.

The operating system should be Server 2019 and the version should be 1809.

Select OK to close About Windows.

In PowerShell, enter hostname to confirm the name of the virtual machine.

What is the hostname displayed?

Leave PowerShell open.

Check your work
Confirm Docker is installed on the virtual machine.

### Managing Docker Containers with PowerShell

To list all available images, in PowerShell, run docker images.

You should see the mcr.microsoft.com/windows/nanoserver image listed. The image was saved on the virtual machine to save time and due to internet restrictions in the lab.

Create a new container using the pre-downloaded nanaserver image by running docker create --name MyFirstContainer mcr.microsoft.com/windows/nanoserver:1809.

This container is now created, but it is not running.

We add :1809 to the image name to specify the tag shown in the docker images results, which is our local image copy. Omitting this defaults to the latest version and requires an Internet check and possible download.

Enter Docker ps -a to see a list of the current containers. Your newly created container should be listed with a status of Created.

Enter Docker start MyFirstContainer to start the new container.

Enter Docker ps -a again. The status should show Exited.

This container is now created, but we do not have any process, applications or interactive sessions so the container exited.

Create another new container by running docker run -it --name TestContainer mcr.microsoft.com/windows/nanoserver:1809.

This container is created and running. This allows us to interact with the container.

You are now presented with a command prompt from within the container. Run ver to view the detailed system information.

What are the last 4 digits of the OS version?

Run ipconfig to view the computer's IP configuration.

Enter the IPv4 address of the vEthernet (Ethernet) adapter in the box below.


Enter the Default Gateway address of the vEthernet (Ethernet) adapter in the box below.


Enter hostname to confirm the name of the virtual machine.

This container has its own operating system, computer name, and IP addresses.

Enter netstat -aon to view the listening ports within the container.

Is TCP port 5985 in a LISTENING state?

Enter Ping <DefaultG> to ping the default gateway. It should be successful.

Enter Ping <MSip> to attempt to ping the host virtual machine.

The ping should succeed. The default for new containers is to connect to a NAT network that allows communications between containers and between containers and the host. The host's vEthernet adapter connects to the NAT network.

Enter exit to exit the container's command shell

Leave the container prompt open.

Check your work
Confirm you have started a container.
Confirm you have run commands within a container.

### View the users and groups in the container

To list all containers and their statuses, run docker ps -a. Both containers should have a status of Exited.

This is a good way to keep track of your containers and their states.

Start the container by running docker start TestContainer.

Always make sure your container is isolated from the production network until you've completed your security checks. This network is isolated.

To list all containers and their statuses again, run docker ps -a. TestContainer should now have a status of Up and show the time it has been running.

To enter the container session, run docker exec -it TestContainer cmd.

Entering the container session with administrator privileges allows you to perform privileged actions like creating users and changing passwords.

Enter echo %username% to view your username within the container.

What is the username for your container login?

List the container's local users with the net user command

List the Administrator account details with the net user Administrator command

Is the local administrator account on the container enabled/active?

List the container's local groups with the net localgroup command

Is there a Docker Administrators group listed?

Run exit to return to MS10.

To enter the container session using the containers Administrator account, run docker exec -it --user ContainerAdministrator TestContainer cmd.

Enter echo %username% to view your username within the container. It should now be ContainerAdministrator.

To create a new user, run net user testuser pa$$word123 /ADD.

To list the details of the new user, run Net user testuser. Notice the Users are listed for Local Group Memberships.

To add the user to the Power Users local group, run net localgroup "Power Users" testuser /add.

To list the details of the new user, run Net user testuser. Notice that Local Group Memberships have been updated to include Power Users.

To change the Administrator's password, run net user Administrator pa$$word123 /passwordchg:yes.

Make sure to set a strong passwords for the container to enhance security. Containers have their own users, groups, operating system, networking and security settings. It is important to make sure all of your security settings are configured properly prior to deploying the container to a production network.

Leave the container prompt open.

Check your work
Confirm you have viewed the default user and groups in a container.
Confirm you have created a new user in the container.

### Create and manage data in a container

To view the folder structure of the container, run Dir to see the folders.

To create a new folder within the container, run md MyFolder.

run Dir again and you should see the new folder.

Change to the folder by running cd MyFolder. The prompt should change to the new folder.

To create a new text file with some text, run echo "This is some text for my test file." > test.txt.

Run Dir again and you should see the new file.

Run type test.txt to view the content of the text file.

To exit the container session, run exit.

Exiting the session returns you to the host machine's interface.

Verify you are once again on the MS10 hostmachine, run Dir. You should see MS10's folders.

To re-enter the container session, run docker exec -it TestContainer cmd.

To view the folder structure of the container, run Dir.

Does MyFolder still exist after you exited the container? (Yes or No)

By default, when you stop a Docker container, the container's file system is preserved. This means that any changes that you made to the container's file system will still be there when you restart the container. However, if you explicitly remove a container using the docker rm command, the container's file system will be deleted.

To exit the container session, run exit.

Stop the container by running docker stop TestContainer.

Always stop containers properly to avoid data corruption and potential security risks.

You've successfully navigated the world of Windows Server Containers using PowerShell. You've learned how to install the Containers feature, create, and manage containers, and even dive into a container session. These skills are essential for modern IT infrastructure and will serve you well in enhancing the security and efficiency of your organization.

Check your work
Confirm that you created a new folder in a container.
Confirm that you created a new file in a container.
