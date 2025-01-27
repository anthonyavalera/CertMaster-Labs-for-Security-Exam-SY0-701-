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

- Windows PowerShell

## Steps

### Confirm Docker installation and host VM settings

1. Connect to the MS10 virtual machine, select Ctrl+Alt+Delete, and then select Other User

2. Sign in as Administrator with the password Pa$$w0rd.

3. To verify that Docker has been installed and is running, open Windows search by selecting the magnifying glass on the taskbar and then type Services. In the search results, select Services.

4. Scroll down the list of services and look for Docker Engine. The service should be listed, and the Status should be Running.

5. Close the Services windows.

    - Docker has been pre-installed on the virtual machine to save time and due to internet restrictions.

6. Select Windows search by selecting the magnifying glass on the taskbar and then type PowerShell. In the search results, right-click Windows PowerShell and choose Run as administrator. Click Yes on the User Account Control window.

7. Run ipconfig to list the IP information of the MS10 virtual machine.

8. Enter the IPv4 Address of the Ethernet adapter in the box below. It should start with 10.

9. In PowerShell, run Winver. A window should open with operating system information.

10. The operating system should be Server 2019 and the version should be 1809.

11. Select OK to close About Windows.

12. In PowerShell, enter hostname to confirm the name of the virtual machine.

13. Leave PowerShell open.

#### Check your work

Confirm Docker is installed on the virtual machine.

### Managing Docker Containers with PowerShell

1. To list all available images, in PowerShell, run docker images.

    - You should see the mcr.microsoft.com/windows/nanoserver image listed. The image was saved on the virtual machine to save time and due to internet restrictions in the lab.

2. Create a new container using the pre-downloaded nanaserver image by running docker create --name MyFirstContainer mcr.microsoft.com/windows/nanoserver:1809.

    - This container is now created, but it is not running.

    - We add :1809 to the image name to specify the tag shown in the docker images results, which is our local image copy. Omitting this defaults to the latest version and requires an Internet check and possible download.

3. Enter Docker ps -a to see a list of the current containers. Your newly created container should be listed with a status of Created.

4. Enter Docker start MyFirstContainer to start the new container.

5. Enter Docker ps -a again. The status should show Exited.

    - This container is now created, but we do not have any process, applications or interactive sessions so the container exited.

6. Create another new container by running docker run -it --name TestContainer mcr.microsoft.com/windows/nanoserver:1809.

    - This container is created and running. This allows us to interact with the container.

7. You are now presented with a command prompt from within the container. Run ver to view the detailed system information.

8. Run ipconfig to view the computer's IP configuration.

9. Enter the IPv4 address of the vEthernet (Ethernet) adapter in the box below.

10. Enter the Default Gateway address of the vEthernet (Ethernet) adapter in the box below.

11. Enter hostname to confirm the name of the virtual machine.

    - This container has its own operating system, computer name, and IP addresses.

12. Enter netstat -aon to view the listening ports within the container.

13. Enter Ping <DefaultG> to ping the default gateway. It should be successful.

14. Enter Ping <MSip> to attempt to ping the host virtual machine.

    - The ping should succeed. The default for new containers is to connect to a NAT network that allows communications between containers and between containers and the host. The host's vEthernet adapter connects to the NAT network.

15. Enter exit to exit the container's command shell

16. Leave the container prompt open.

#### Check your work

Confirm you have started a container.

Confirm you have run commands within a container.

### View the users and groups in the container

1. To list all containers and their statuses, run docker ps -a. Both containers should have a status of Exited.

    - This is a good way to keep track of your containers and their states.

2. Start the container by running docker start TestContainer.

    - Always make sure your container is isolated from the production network until you've completed your security checks. This network is isolated.

3. To list all containers and their statuses again, run docker ps -a. TestContainer should now have a status of Up and show the time it has been running.

4. To enter the container session, run docker exec -it TestContainer cmd.

    - Entering the container session with administrator privileges allows you to perform privileged actions like creating users and changing passwords.

5. Enter echo %username% to view your username within the container.

6. List the container's local users with the net user command

7. List the Administrator account details with the net user Administrator command

8. List the container's local groups with the net localgroup command

9. Run exit to return to MS10.

10. To enter the container session using the containers Administrator account, run docker exec -it --user ContainerAdministrator TestContainer cmd.

11. Enter echo %username% to view your username within the container. It should now be ContainerAdministrator.

12. To create a new user, run net user testuser pa$$word123 /ADD.

13. To list the details of the new user, run Net user testuser. Notice the Users are listed for Local Group Memberships.

14. To add the user to the Power Users local group, run net localgroup "Power Users" testuser /add.

15. To list the details of the new user, run Net user testuser. Notice that Local Group Memberships have been updated to include Power Users.

16. To change the Administrator's password, run net user Administrator pa$$word123 /passwordchg:yes.

    - Make sure to set a strong passwords for the container to enhance security. Containers have their own users, groups, operating system, networking and security settings. It is important to make sure all of your security settings are configured properly prior to deploying the container to a production network.

17. Leave the container prompt open.

#### Check your work

Confirm you have viewed the default user and groups in a container.

Confirm you have created a new user in the container.

### Create and manage data in a container

1. To view the folder structure of the container, run Dir to see the folders.

2. To create a new folder within the container, run md MyFolder.

3. run Dir again and you should see the new folder.

4. Change to the folder by running cd MyFolder. The prompt should change to the new folder.

5. To create a new text file with some text, run echo "This is some text for my test file." > test.txt.

6. Run Dir again and you should see the new file.

7. Run type test.txt to view the content of the text file.

8. To exit the container session, run exit.

    - Exiting the session returns you to the host machine's interface.

9. Verify you are once again on the MS10 hostmachine, run Dir. You should see MS10's folders.

10. To re-enter the container session, run docker exec -it TestContainer cmd.

11. To view the folder structure of the container, run Dir.

    - By default, when you stop a Docker container, the container's file system is preserved. This means that any changes that you made to the container's file system will still be there when you restart the container. However, if you explicitly remove a container using the docker rm command, the container's file system will be deleted.

12. To exit the container session, run exit.

13. Stop the container by running docker stop TestContainer.

    - Always stop containers properly to avoid data corruption and potential security risks.

You've successfully navigated the world of Windows Server Containers using PowerShell. You've learned how to install the Containers feature, create, and manage containers, and even dive into a container session. These skills are essential for modern IT infrastructure and will serve you well in enhancing the security and efficiency of your organization.

#### Check your work

Confirm that you created a new folder in a container.

Confirm that you created a new file in a container.
