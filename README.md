# APPLIED LAB: Using Storage Encryption

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

1.4 Explain the importance of using appropriate cryptographic solutions.
2.5 Explain the purpose of mitigation techniques used to secure the enterprise.
3.3 Compare and contrast concepts and strategies to protect data.
5.1 Summarize elements of effective security governance.

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
- mmc.exe

## Steps

### Configure an EFS Data Recovery Agent

Storage encryption can be implemented in many ways, including full-disk, partition, file, volume, database, and record encryption. This lab focuses on file encryption. Specifically, you will be using the encryption file system (EFS) which is a feature of the NTFS file format from Windows OSes. EFS, like most file encryption solutions, employs symmetric encryption to provide confidentiality protection for the files and asymmetric encryption to store the symmetric encryption keys.

Before you encrypt files with EFS, it is important to configure an EFS Recovery Agent first. Otherwise, encrypted files will be unrecoverable and inaccessible if the user's password is changed or if their EFS private key becomes corrupted.

1. Connect to the PC10 virtual machine, send Ctrl+Alt+Delete and sign in as Jaime with the password Pa$$w0rd.

    - You are signing in as Jaime since that account is a domain administrator and the default account for the PC10 system. Once you remove PC10 from the domain, you will use the local administrator account named Admin.

2. Select Type here to search from the taskbar, type powershell, then right-click Windows PowerShell from the results, then select Run as administrator.

3. Select Yes on the User Account Control window.

4. Enter the following code into the Administrator: Windows PowerShell console:

    - Remove-Computer -UnjoinDomaincredential Administrator -Restart -Force
    - This operation removes the PC10 virtual machine from the domain and configures it as a stand-alone system. This is necessary for this lab in order to simulate the loss of an EFS private key. The process used in this lab to remove a user's EFS private key does not work on a domain member.

5. On the Windows PowerShell credential request window, type Pa$$w0rd as the password, then select OK

    - After a few seconds, the system will reboot.

6. Connect to the PC10 virtual machine, send Ctrl+Alt+Delete and sign in as Admin with the password Pa$$w0rd.

    - Since the system is no longer a member of the domain, you must use local accounts. The local administrator account is named admin.

7. Manually create an Encryption File System (EFS) Data Recovery Agent (DRA) certificate stored in a new folder named c:\certificates.

  Expand this hint for guidance.
  
    a. Select Type here to search from the taskbar, type cmd, right-click Command Prompt from the results, then select Run as administrator.

    b. Select Yes in the User Account Control window.

    c. To create a folder, run the command:

    - mkdir c:\certificates
    
    d. To change to the new folder, run the command:

    - cd c:\certificates
    
    e. To create and export a certificate to be used for EFS recovery agent activities, run the command:

    - cipher /r:EFSRA
    
    f. At the prompt to provide a password to protect the .PFX file, enter Pa$$w0rd.

    g. Confirm the password Pa$$w0rd.

    h. To view the new certificate files enter the command:

    - dir
    i. Leave the Command Prompt window open.

        - You will not see any characters echoed in the Command Prompt window for the password. After you type the password, press enter.

        - This operation creates and saves the EFS DRA certificate as a file named EFSRA.CER and a separate file named EFSRA.PFX which contains the password-protected private key for the EFS DRA certificate.

        - The original name for the Data Recovery Agent (DRA) was Key Recovery Agent (KRA). However, some misunderstood the concept of the KRA, assuming it would restore access to the lost EFS private key, which the KRA (nor the DRA) is unable to accomplish. The new DRA name clearly indicates this account is only able to restore access to the data.

8. Add the EFS DRA certificate to the Local Security Policy.

    Expand this hint for guidance.
        a. Select Type here to search from the taskbar, type mmc.exe, then select mmc.exe Run command from the results.

        b. Select Yes in the User Account Control window.

        c. From the Console1 window, select File, then select Add/Remove Snap-in….

        d. On the Add or Remove Snap-ins window, select Group Policy Object Editor, then select Add >.

        e. On the Select Group Policy Object window, leave the default name of "Local Computer", and select Finish.

        f. Select OK to close the Add or Remove Snap-ins window.

        g. The Local Computer Policy should now be displayed in the Console1 window. Select Local Computer Policy in the left pane.

        h. In the left pane, under Computer Configuration, double-click to expand Windows Settings | Security Settings | Public Key Policies, then select Encrypting File System.

        - You may need to click-hold-drag-release the pane divisions to resize them.

        i. Right-click Encrypting File System in the left pane, and then select Add Data Recovery Agent….

        j. On the Add Recovery Agent Wizard, select Next.

        k. On the Select Recovery Agent page, select Browse Folders.

        l. Select the arrow to expand This PC, then select Local Disk (C:), double-click the certificates folder, select the EFSRA file, then select Open. This imports the public key file named EFSRA.CER.

        m. On the Add Recovery Agent window, select Yes.

        n. On the Add Recovery Agent Wizard window, select Next, then select Finish.

        o. You should see the Admin certificate listed in the right pane.

9. Close the Console1 window. Select No when prompted to save settings.

        - Stand-alone Non-Domain joined Windows systems do not have a pre-defined Data Recovery Agent (DRA). A domain-member Windows system may or may not have a DRA defined. The DRA can recover EFS encrypted files in the event the original owner and encryptor of the files loses access or is removed from the computer or organization. In order for this recovery process to function, a DRA must be defined prior to the encryption of files.

10. Create a new local user account named Pat with password of Password1.

    Expand this hint for guidance.
        a. Return to the Administrator: Command Prompt.

        b. Create the new account by entering the following:

        - net user pat Password1 /add
    
        - You are assigning a unique password to the pat account, which is not the lab's standard password. You will also change this in a later exercise.

Check your work

Confirm that you created an EFS DRA certificate.
Confirm that you added the EFS DRA certificate to the Local Security Policy.
Confirm you created a new local user account.

### Encrypt files with EFS

EFS is a native feature of NTFS. EFS allows for individual files (or entire folders) to be encrypted without needing to encrypt an entire storage device or volume. EFS is easy to use, both from a GUI utility as well as from the CLI.

1. Sign out of PC10 by selecting the Start menu, then selecting Admin (which will be a circle at the top of the menu), then select Sign out.

    If prompted that there are open programs, select Sign out anyway.

2. Connect to the PC10 virtual machine, send Ctrl+Alt+Delete and then sign in as Pat with the password Password1.

        - It may take a minute for the user profile to be created.

3. Create a text file named Jan-Security.txt in a new folder named C:\SecReports.

    Expand this hint for guidance.
        a. Select Type here to search from the taskbar, type File Explorer into the Windows search bar, and then, on the menu, select File Explorer.

        b. Select This PC, then double-click Local Disk (C:).

        c. In the blank space of the folder view pane (i.e., the right pane), right-click, select New, then select Folder.

        d. Enter SecReports for the folder name.

        e. Double-click SecReports to open the folder.

        f. Right-click in the empty folder area, select New, then select Text Document.

        g. Enter the name Jan-Security.

        - File Explorer does not display known file extensions by default. So, while you only provided the main filename of Jan-Security, the actual filename with the extension is Jan-Security.txt.

4. Create two additional text files named Feb-Security and Mar-Security in the folder named C:\SecReports.

5. Open each of these new text files and enter This a security report. and save each file.

    Expand this hint for guidance.
        a. Double-click Jan-Security.

        b. In the Notepad window, enter This a security report..

        c. Close Notepad.

        d. Select Save to save the changes.

        e. Repeat these steps for Feb-Security and Mar-Security.

6. Using File Explorer, encrypt Jan-Security with the Encryption File System (EFS).

    Expand this hint for guidance.
        a. Right-click the text file Jan-Security.txt in the C:\SecReports folder then select Properties.

        b. On the General tab, select Advanced.

        c. Select the checkbox named Encrypt contents to secure data.

        d. Select OK to close the Advanced Attributes window.

        e. Select OK to close the Jan-Security.txt Properties window.

        f. An Encryption Warning window will appear, select Encrypt the file only, do not mark the checkbox, and then select OK.

        - The Encryption File System (EFS) is configured by default to encrypt folders and the contents of those folders. When being used to encrypt individual files, you will be prompted to confirm or elect to encrypt the entire folder to which the current file is a member.

        - You will receive a message about backing up your EFS key. You can ignore the message for this exercise. It will disappear after a few seconds or when you select anything else on the desktop. However, it is a good idea to back up your EFS keys. Because if they are lost or corrupted, you will be unable to access your encrypted files ever again (unless you have a DRA defined who can restore your files to you (as will be performed in a later exercise in this lab)).

        - It may take 10 to 15 seconds for EFS to encrypt the file. This is only because this is the first time EFS is being used on this drive, and system configurations must take place. Subsequent encryptions will take less time.

7. Check the status of the files in C:\SecReports using the cipher command and then encrypt Feb-Security.txt and Mar-Security.txt using the CLI.

    Expand this hint for guidance.
        a. Select Type here to search from the taskbar, type cmd, then select Command Prompt from the results.

        b. To change to the C:\SecReports folder, run the command:

        - cd c:\SecReports
   
        c. To view the encryption status of the objects in the current working folder, run the command:

        - cipher
   
        - The cipher command without parameters will display the contents of the current working directory along with an indication of the encryption status of each item. The “E” indicates the item is encrypted, while the “U” indicates the item is unencrypted.

        d. To encrypt several files at once from the Command Prompt (instead of the GUI), run the command:

        - cipher /e *-Security.txt
        - This command and parameters encrypts all files matching the *-Security wildcard in the current directory, which includes Feb-Security.txt and Mar-Security.txt.

8. Check the status of the files in C:\SecReports using the cipher command and then decrypt Mar-Security.txt using the CLI. Then view the status of the files again to confirm the result.

    Expand this hint for guidance.
        a. To view the encryption status of the objects in the current working folder, run the command:

        - cipher
   
        - Notice all 3 text files are now marked as encrypted.

        b. To decrypt a file, run the command:

        - cipher /d Mar-Security.txt
        - This command and parameters decrypts the Mar-Security.txt file.

        c. To view the encryption status of the objects in the current working folder, run the command:

        - cipher
        - Notice that Jan-Security.txt and Feb-Security.txt text files are now marked as encrypted (i.e., E), and Mar-Security.txt text file is marked as unencrypted (i.e., U).

9. Close the Command Prompt.

10. Configure File Explorer to display encrypted files in alternate colors.

    Expand this hint for guidance.
        a. File Explorer should be open.

        b. From the File Explorer menu, select View, then select Options.

        c. Select View tab.

        d. Scroll to locate and then select Show encrypted or compressed NTFS files in color.

        e. Select OK to close the Folder Options window.

        - Notice that the encrypted files in c:\SecReports are now color-coded to indicate if they are encrypted (green) or unencrypted (black).

11. Sign-out Pat from PC10

12. On the PC10 virtual machine, send Ctrl+Alt+Delete and sign in as Admin with the password Pa$$w0rd.

13. Open File Explorer and view the contents of the c:\SecReports folder.

        - Notice that Jan-Security.txt and Feb-Security.txt files have a locked yellow padlock icon over their file icon. This indicates this is an encrypted file.

14. Attempt to open Jan-Security.txt by double-clicking is filename. A notepad warning is displayed, indicating you do not have permission to open this file. Select OK to close the warning. Then close Notepad.

        - If a How do you want to open this file? window appears, select Notepad, mark the checkbox Always use this app to open .txt files, then select OK.

15. Attempt to open Feb-Security.txt by double-clicking is filename. A notepad warning is displayed, indicating you do not have permission to open this file. Select OK to close the warning. Then close Notepad.

16. Attempt to open Mar-Security.txt by double-clicking is filename. You are able to open the file as it is not encrypted, and the default inherited permissions granted the Admin user at least read access. Close Notepad.

        - EFS can be used as a means by which individual users can limit access to files and folders without the need to configure access control settings on those files and folders. Any user that does not possess the correct EFS key will be unable to access the EFS encrypted files. While EFS does hide the context of files from other non-authorized users, it does not hide the existence of or the name of the file.

Check your work

Confirm that you switched users.
Confirm that you created text files.
Confirm that you added content to text files.
Confirm that you encrypted files with EFS through GUI and CLI means.
Confirm that you set File Explorer options.
Confirm that you tested the access control aspect of EFS.

### Break a user account’s ability to access their own EFS encrypted files

When an administrator changes the password of a local user account, the EFS private key of that user account is discarded. Therefore, an administrator cannot use a password change as a means to access the encrypted files of a user. However, this does mean the user loses their ability to access the EFS encrypted files as well.

1. Connect to the PC10 virtual machine, and if needed, send Ctrl+Alt+Delete, and then sign in as Admin with the password Pa$$w0rd.

2. Change the password of Pat to Password123 using the Command Prompt.

    Expand this hint for guidance.
        a. Select Type here to search from the taskbar, type cmd, right-click Command Prompt from the results, then select Run as administrator.

        b. Select Yes in the User Account Control window.

        c. To change the password of the Pat run the command:

        - net user Pat Password123
        - There will be no confirmation.

   - You are logged out of the Pat account on PC10 which ensures that the password change will also replace the EFS key assigned to the account.

3. Sign out Admin from PC10.

4. On the PC10 virtual machine, send Ctrl+Alt+Delete, and sign in as Pat with the password Password1.

        - Notice the error message stating that the password is incorrect.

5. Select OK, then sign in to the Pat account using the new password of Password123..

6. Open File Explorer and view the contents of the C:\SecReports folder.

7. Attempt to open Jan-Security by double-clicking its filename. A notepad warning is displayed, indicating you do not have permission to open this file. Select OK to close the warning. Then close Notepad.

8. Attempt to open Feb-Security by double-clicking on Feb-Security. A notepad warning is displayed, indicating you do not have permission to open this file. Select OK to close the warning. Then close Notepad.

9. Attempt to open Mar-Security by double-clicking on Mar-Security. You are able to open the file as it is not encrypted and, therefore, not affected by the account’s loss of its EFS key. Close Notepad.

10. Sign out Pat from PC10.

Check your work

Confirm that you changed a user password via Command Prompt.
Confirm that you verified that the Pat user account had lost access to their encrypted files.

### Use a Data Recovery Agent (DRA) to recover access to EFS-encrypted files

If a user has lost access to their EFS private key, they are no longer able to open their encrypted files. If there was a defined DRA when the files were encrypted, the DRA may be able to restore access to the files to the user.

1. On the PC10 virtual machine, send Ctrl+Alt+Delete, and sign in as Admin using Pa$$w0rd as the password.

2. Attempt to decrypt Jan-Security.txt using the cipher command. Then use the cipher command to display information about the encrypted file.

    Expand this hint for guidance.
        a. Select Type here to search from the taskbar, type cmd, right-click Command Prompt from the results, then select Run as administrator.

        b. Select Yes in the User Account Control window.

        c. To change to the C:\SecReports folder, run the command:

        - cd c:\\SecReports
   
        d. Attempt to decrypt Jan-Security.txt by running the command:

        - cipher /d Jan-Security.txt
   
        e. Notice the results are that an error occurred, and no files were decrypted.

        f. Run the command:

        - cipher /c Jan-Security.txt
   
        g. Review the information about the file.

        - Notice the display indicates that Admin is the designated Recovery Certification entity. However, the reason the attempt to decrypt the file failed is that the private key of the ECA DRA has not been imported. Thus, without that private key, the DRA function cannot take place.

3. Install the EFSRA.PFX private key.

        - The default configuration of Windows hides file extensions. Double-clicking EFSRA.CER will open a Certificate window displaying the details of the certificate rather than the expected Certificate Import Wizard. If this occurs, close the Certificate window and double-click the other file (which, therefore, would be EFSRA.PFX).

    Expand this hint for guidance.
        a. Open File Explorer.

        b. Select This PC, then double-click Local Disk (C:), and double-click the certificates folder.

        c. Double-click the EFSRA file, which is labeled as being of Type Personal Information Exchange (note: this label may be truncated to “Personal Informati…”) and which has an icon of a document extending out of an envelope with a yellow key overlaid. This opens the Certificate Import Wizard.

        d. Leave the default selection of Current User, and select Next.

        e. Leave the default file name of C:\certificates\EFSRA.PFX, and select Next.

        f. Enter Pa$$w0rd in the Password: field, leave all other settings at their defaults, then select Next.

        g. Leave the default selection of Automatically select the certificate store based on the type of certificate, select Next, then select Finish.

        h. On the Certificate Import Wizard notification that The import was successful, select OK.

4. Return to the Command Prompt, and attempt to decrypt Jan-Security.txt using the cipher command.

        - Notice this time the operation is successful and will display a message stating: "1 file(s) [or directorie(s)] within 1 directorie(s) were decrypted."

5. Use the cipher command to view the encryption status of the current folder’s files.

6. Notice that Jan-Security.txt is now listed as unencrypted (i.e., marked with a “U”). This means as a DRA, you have recovered access to an encrypted file after the original user lost their decryption capabilities. Remember that it was originally encrypted by Pat, but you are currently logged in as Admin. But as the EFS DRA with your private key installed, you are able to decrypt files from other users.

7. View the details about encryption of the Feb-Security.txt file through File Explorer, then use the GUI method to decrypt the file.

    Expand this hint for guidance.
        a. Return to File Explorer. If it is not open, select Type here to search from the taskbar, type File Explorer into the Windows search bar, and then, on the menu, select File Explorer.

        b. Select This PC, then double-click Local Disk (C:).

        c. Double-click SecReports to open the folder.

        d. Right-click Feb-Security.txt and select Properties.

        e. Select Advanced, then select Details.

        f. The User Access to Feb-Security window displays the users who have access to the file, in this case, only Pat, as well as the Recovery certificates, in this case, that of Admin. Select OK to close this window.

        g. De-select the checkbox named Encrypt contents to secure data and then select OK, then select OK again.

        h. You may be prompted by an Access Denied window, select Continue.

        - This Access Denied window is an element of User Account Control (UAC). Even though you are logged in with administrative privileges, the system is configured by default to display the UAC warning. By selecting Continue, you are verifying that you understand the activity being attempted will be using administrative privileges.

        - The File Explorer window focused on C:\SecReports should show that Feb-Security.txt is no longer encrypted.

8. Sign out Admin from PC10.

9. On the PC10 virtual machine, send Ctrl+Alt+Delete, and sign in as Pat with the password Password123.

10. Open File Explorer and view the C:\SecReports folder.

        - Notice all files in this folder are no longer encrypted.

11. Open and view the contents of each text file, closing Notepad after.

        - This confirms that the EFS DRA was able to recover the data files on behalf of the user that lost access to their own EFS private key due to an admin changing their password.

        - An Encryption File System (EFS) Data Recovery Agent (DRA) is able to decrypt files in order to return them to an accessible state. This capability is useful in situations where the user’s EFS private key is damaged, lost, or discarded (such as when an administrator changes their password). The ability of an EFS DRA to decrypt files can also be useful in recovering files encrypted by a user account that has been deleted or by a user who refuses to cooperate with management or investigators.

Check your work

Confirm that you installed the PIX key in order to function as the data recovery agent (DRA).
Confirm that you decrypted a file as a DRA from a Command Prompt and through the GUI.
Confirm that you viewed a file's encryption status.
