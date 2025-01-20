# Assisted Lab: Managing Password Security

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.4 Given a scenario, analyze indicators of malicious activity.
2.5 Explain the purpose of mitigation techniques used to secure the enterprise.
4.6 Given a scenario, implement and maintain identity and access management.
5.1 Summarize elements of effective security governance.
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

- Terminal Emulator
- Windows PowerShell

## Steps

### Understanding password spraying attacks

Password spraying attacks are used when a password is known, but the account it belongs to is not. An attacker will submit the known password while attempting to sign in to numerous user accounts in the hope of discovering the credential set. This is a form of online or live authentication attack. This approach is also used to avoid triggering account lockout - by only trying one or two passwords per user account before moving on to the next user account.

To get started, you are already aware of a share on MS10 named HR that any user account can access. You also have a list of users from the MS10 system. Let's say you have seen a list of passwords that were discovered in a trash can, but the list did not have any usernames associated with it. You will then attempt to use those passwords against the known accounts in a password spraying attack to mount the HR share.

1. Connect to the KALI virtual machine and sign in as root using Pa$$w0rd as the password.

2. Open a Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar. Then, maximize the Terminal window.

3. Enter mkdir /mnt/HR to create a mount point.

    - A mount point is needed to attach a network share to, assuming you discover valid credentials to access the share.

4. Enter cat users.txt to view a list of users from the MS10 (10.1.16.2) system.

5. Here is the list of discovered passwords that you think might relate to an account on MS10.

    - abc123
    - 123456
    - Pa$$w0rd
    - 
6. Perform your first password submission attempt using the following command:

    - mount //10.1.16.2/HR /mnt/HR -o username=pat
    - This mount command will attempt to map the Windows share named HR from MS10 (10.1.16.2) to the local mount point of /mnt/HR with the user credentials of pat.

7. When prompted for a password, enter the first password from your discovered list: abc123.

  This should result in a mount error. This means that this password is not used by that account.

8. Perform a second password submission attempt using the following command:

    - mount //10.1.16.2/HR /mnt/HR -o username=pat

9. When prompted for a password, enter the first password from your discovered list: 123456.

  This should result in a mount error. This means that this password is not used by that account.

10. Obviously, trying each of the passwords against each of the known user accounts would be very tedious if performed manually. You decide to use an automated tool, but you need to create a file containing the discovered passwords. Enter the following commands:

    - echo abc123 > pass.txt
    - echo 123456 >> pass.txt
    - echo 'Pa$$w0rd' >> pass.txt
    - cat pass.txt

  You should see a final output of the contents of the pass.txt file you just created, showing the three discovered passwords.

    - If the passwords are not spelled correctly, especially the final one, the subsequent password spraying attack will fail.

      Be sure to use single quotes around the Pa$$w0rd password.

    - The use of double greater-than symbols (i.e., >>) performs an append rather than a replace function when capturing output into a file.

11. Enter hydra-wizard in the opened Terminal to use the wizard version of the authentication attack tool Hydra. Provide each of the following values as prompted. Press Enter after typing each value. If no value is listed, press Enter to leave it blank or accept the default.

Prompt	Response
Service	smb
Target	10.1.16.2
Username	users.txt
Password	pass.txt
Test	
Port	
Module options	
Run command	Y

  This wizard will run the password spraying attack by using each password against each of the user accounts against the share on MS10.

  The results will be displayed, showing that the password spraying attack was able to discover that the Pa$$w0rd password is used by two accounts.

12. Enter the following command to mount the share and confirm the credentials are real.

    - mount //10.1.16.2/HR /mnt/HR -o username=jaime
  Enter Pa$$w0rd as the password when prompted. If no error is displayed, the mounting of the share from MS10 was successful.

13. Enter ls /mnt/HR to view the contents of the mounted share.

    - If you want to dismount the share to try other credential combinations, use umount /mnt/HR.

14. Leave the Terminal window open.

    - Password spraying is effectively a simple version of a dictionary attack. But instead of a massive list of potential passwords, a password spraying attack uses a single or a short list of known passwords (or assumed known) for a user within a specific environment. The goal of password spraying is to discover which user account the password is used by. The hydra tool used in this exercise can be used to perform full dictionary attacks as well as brute force attacks.

This password spraying exercise demonstrates the importance of good password management. In a real-world scenario, you should improve user training and implement password complexity requirements. You should encourage users to make longer and more complex passwords. You should remind them not to write down passwords unless absolutely necessary and then shred the paper once it is no longer needed. You can also elect to implement password complexity and strength requirements through security configurations.

#### Check your work

Confirm that you attempted manual password spraying.

Confirm that you perform automated password spraying.

### Perform a dictionary password crack

Password cracking demonstrates just how important long and complex passwords are to a robust identity and access management (IAM) system. Even with the best password hashing algorithms in use, poor passwords can still be discovered by an adversary without much difficulty.

In this exercise, you will be using John the Ripper (JtR or john) to perform a dictionary attack against password hashes taken from MS10. This form of password crack is an offline attack as it does not involve a live/active/online authentication service. Instead, it is a direct attack against password hashes.

    - A live or online attack is limited in the speed at which attempts can be made. It can take a fraction of a second to several seconds for each failed login attempt against a live authentication service. An offline attack is limited by the computational capability of the local processor. Often, 10 billion or more password crack attempts can be made per second. Also, an online attack can be stopped with account lockout (such as when only three (3) attempts are allowed), while an offline attack is not limited by account lockout because the authentication service is not involved in the attack.

1. Connect to the KALI virtual machine and, if needed, sign in as root using Pa$$w0rd as the password.

2. The Terminal window should still be open.

3. Enter cat ms10-hashes.txt to display the collected user account details and password hashes from MS10.

        - Notice the account with a RID of 500 (the default administrator account of Windows) has a username of admin. Also, the account with a RID of 501 is the default Guest account which is usually disabled and has a blank password by default.

        - This file contains the user account details and password hashes extracted from MS10.

4. Display a list of available dictionary password files by entering the following:

        - ls -lSr /usr/share/seclists/Passwords
    This command presents the long list of directory contents sorted by smallest to largest.

        - The parameters used in this ls command are -l for long list format, -S for sort by size, largest first, and -r, which reverses the sorting order (i.e., smallest first).

        - There are numerous password lists included with Kali. You will use the xato-net-10-million-passwords.txt file as it is primarily English-focused and is one of the largest options.

        - A dictionary-based password crack is only able to discover passwords when the list contains an exact match to a user’s password. If the user employed a different case or a different letter, then a dictionary-list-based password cracking approach would not be successful.

5. Enter the following to initiate a dictionary attack against the hash file:

        - john --format=NT --wordlist=/usr/share/seclists/Passwords/xato-net-10-million-passwords.txt ms10-hashes.txt
    This command initiates John the Ripper to perform a dictionary-based password crack. The parameters used are:

        - "--format" sets the hash algorithm to be compromised. Here NT stands for NTLM.
        - "--wordlist" sets the password list to use.
    This attack will only take a few seconds - even with a 10 million password dictionary file.

        - A dictionary attack is a form of offline password cracking as it uses stolen password hashes and does not interact with a live authentication system. A dictionary attack performs a hash of each password in the source list file and then compares the resulting hash to the target hash(es) pulled from the hash file. If a matching hash is discovered (a.k.a. hash collision), then a password has been found. If not, the next password in the list is used. Most systems can perform billions of password hashes per second. Therefore even with large dictionary lists, the attack concludes quickly.

        - Microsoft's NTLM (Windows New Technology LAN Manager) is a security protocol suite that ensures the security, integrity, and confidentiality of users' activity by authenticating their identity. It serves as a single sign-on (SSO) tool that uses a challenge-response protocol to verify the user's identity without the need to enter a password. Despite the existence of known weaknesses, NTLM is still commonly used, even on new systems, to guarantee compatibility with legacy clients and servers.

    Notice how many of the passwords of the targeted accounts were cracked using the dictionary technique. However, there are still a few more accounts whose passwords are yet to be compromised.

        - While you could attempt to crack the other outstanding passwords using other dictionary files. Unfortunately, none of the other dictionary files in this lab environment have any additional passwords for the target user accounts from this hash file from MS10.

6. Enter the following to export the compromised passwords along with the information from the hash source file into a separate file:

        - john --show --format=NT ms10-hashes.txt > dict-cracked.txt
    With the selected dictionary file, John the Ripper should have cracked 12 passwords using the dictionary password cracking method.

        - You still need to include the --format parameter to export the NTLM cracked password results.

7. Enter the following to view the file.

        - less dict-cracked.txt 
        - Notice that JtR has placed the cracked password between the username and the RID value in this presentation.
        - When using the less file viewing utility, press the spacebar to view the next page. You can return to a previous page using b or scroll one line up or down utilizing the arrow keys. When finished looking over the results, type q to exit the less viewer.

8. Enter the following to display the accounts that have not yet been compromised.

        - john --show=left --format=NT ms10-hashes.txt
        - There may be a discrepancy between the two --show operations in regards to the number of passwords cracked. Two accounts have blank passwords. This is counted as two successes by the --show operation but counted as only one success by the --show=left operation.

9. Leave the Terminal window open.

        - Password guessing is a form of online or live password attack. It is a live or online attack as it requires working against the actual authentication system of the target. In a password guessing attack, you can make up passwords yourself, use a dictionary list, or use a brute force approach. Account lockout is used to prevent continuous password guessing by disabling accounts after a limited number of failed login attempts.

This dictionary-based password cracking exercise demonstrates the importance of good password management. In a real-world scenario, you should improve user training and implement password complexity requirements. You need to encourage users to make longer and more complex passwords. You can also elect to implement password complexity and strength requirements through security configurations.

#### Check your work

Confirm that you performed dictionary password cracking.

### Perform a brute force password crack

In this exercise, you will be using John the Ripper (JtR) to perform a brute force attack against the password hashes taken from MS10. This form of password crack is also an offline attack as it does not involve a live/active/online authentication service. Instead, it is a direct attack against password hashes.

1. Connect to the KALI virtual machine and, if needed, sign in as root using Pa$$w0rd as the password.

2. The Terminal window should still be open.

3. Enter the following to clear the history of cracked passwords (i.e., the ones you cracked using a dictionary approach in the previous exercise). This enables you to see the operation of a brute force attack more clearly by having all target hashes available.

        - rm ~/.john/john.pot
   
5. Enter the following to initiate a brute force attack (known as incremental by John the Ripper) against the hashes from MS10.

        - john --format=NT --incremental ms10-hashes.txt
    You will be prompted to "press almost any other key for status." Press SPACEBAR to see a status update on the progress and the time elapsed.

        - The process of brute force attack is based on attempting all possible patterns of characters based on length. John the Ripper's default configuration will use all standard ASCII's 95 printable characters (i.e., positions 32 - 127 in the 7-bit ASCII table), including lowercase, uppercase, numbers, and symbols. It will also attempt potential passwords up to 13 characters in length. However, the more complex and/or longer a password, the more time is required to compromise it with this method. Brute force has the potential to discover all possible passwords, but only if given sufficient computing capability and time.

While the default mode of JtR is ASCII, this is a bit of a legacy holdover as most systems use UTF-8 as their standard character mode. UTF-8 is backward compatible with ASCII since the first 128 characters of UTF-8 are the entire ASCII 7-bit table.

As of JtR version 1.9.0, pre-defined incremental modes are "ASCII" (all 95 printable ASCII characters), "LM_ASCII" (for use on LM hashes), "Alnum" (all 62 alphanumeric characters), "Alpha" (all 52 letters), "LowerNum" (lowercase letters plus digits, for 36 total), "UpperNum" (uppercase letters plus digits, for 36 total), "LowerSpace" (lowercase letters plus space, for 27 total), "Lower" (lowercase letters), "Upper" (uppercase letters), and "Digits" (digits only). You can define custom incremental (i.e., brute force) modes.

5. You should see some quick successes for accounts with simple and short passwords. Then, several more passwords will be cracked within a minute or so.

6. Allow the attack to run for up to 5 minutes or until the renee account's password is cracked. Once sufficient time has passed type q to exit John the Ripper.

        - The longer you wait while the brute force attack continues, the longer and more complex passwords will be cracked by John the Ripper.

7. Enter the following to display the compromised passwords along with the information from the hash source file.

        - john --show --format=NT ms10-hashes.txt
        - Notice you still need to include the --format parameter.

    If you allowed John the Ripper to run for a full 5 minutes, it should have compromised 10 passwords using the brute force password cracking method.

8. Enter the following to save the brute-force-cracked passwords to a text file.

        - john --show --format=NT ms10-hashes.txt > brute-cracked.txt

9. Enter the following to view the file.

        - less brute-cracked.txt 
        - Notice that JtR has placed the cracked password between the username and the RID value in this presentation.
        - When using the less file viewing utility, press the spacebar to view the next page. You can return to a previous page using b or scroll one line up or down utilizing the arrow keys. When finished looking over the results, type q to exit the less viewer.

10. Enter the following to display the accounts that have not yet been compromised.

        - john --show=left --format=NT ms10-hashes.txt
    
11. Enter the following to run a brute force attack limited to a maximum password length of 6 characters.

        - john --format=NT --incremental --max-length=6 ms10-hashes.txt
    
12. Let the attack run for a few moments, then press SPACEBAR.

        - Notice the status update includes a percentage of completion and an ETA for the attack to be completed. The ETA for this operation is many hours.

13. Type q to terminate John the Ripper.

14. Enter the following to run a brute force attack limited to a maximum password length of 7 characters.

        - john --format=NT --incremental --max-length=7 ms10-hashes.txt
15. Press SPACEBAR.

        - Notice the status update includes a percentage of completion and an ETA for the attack to be completed. The ETA for this operation is a date rather than a countdown timer. The ETA will be weeks in the future. If you set max-length to 8, John no longer calculates the ETA. However, it would be several years since each additional character length expands the range of password options by a factor of the domain of character options (with the default ASCII mode, there are 95).

16. Type q to terminate John the Ripper.

This brute force based password cracking exercise demonstrates the importance of good password management. In a real-world scenario, you should improve user training and implement password complexity requirements. You need to encourage users to make longer and more complex passwords. You can also elect to implement password complexity and strength requirements through security configurations.

#### Check your work

Confirm that you performed brute force password cracking.

### Set password policy for a domain

Based on your findings from your password spraying, dictionary, and brute force attacks, you realize that the organization's domain-wide password policy needs to be improved. You have also reviewed the NIST Special Publication 800-63B: Digital Identity Guidelines: Authentication and Lifecycle Management: (https://pages.nist.gov/800-63-3/sp800-63b.html). You have decided to make a few changes to the password and account lockout policy for the Structureality domain.

1. Connect to the DC10 virtual machine. Send Ctrl+Alt+Delete and sign in as Structureality\Administrator using Pa$$w0rd as the password.

2. Minimize or close Server Manager if it appears. It will not be used in this lab.

3. Select Type here to search from the taskbar, type powershell, right-click Windows PowerShell from the results, then select Run as administrator.

4. Select Yes on the User Account Control window.

    The PowerShell console should be displayed.

5. Enter the following command to load the PowerShell module needed to interact with Active Directory:

        - Import-Module ActiveDirectory
   
6. Enter the following command to display the current password policy for the domain:

        - Get-ADDefaultDomainPasswordPolicy

7. Based on the current settings, you want to make the following changes:

| Key	| Value |
| LockoutObservationWindow	| 00:15:00 |
| LockoutDuration |	00:15:00 |
| LockoutThreshold	| 3 |
| MaxPasswordAge	| 365 |
| MinPasswordAge	| 3 |
| MinPasswordLength	| 12| 

8. Enter the following commands to implement these password policy changes:

        - Set-ADDefaultDomainPasswordPolicy -Identity structureality -LockoutObservationWindow 00:15:00
        - Set-ADDefaultDomainPasswordPolicy -Identity structureality -LockoutDuration 00:15:00
        - Set-ADDefaultDomainPasswordPolicy -Identity structureality -LockoutThreshold 3
        - Set-ADDefaultDomainPasswordPolicy -Identity structureality -MaxPasswordAge 365.00:00:00
        - Set-ADDefaultDomainPasswordPolicy -Identity structureality -MinPasswordAge 3.00:00:00
        - Set-ADDefaultDomainPasswordPolicy -Identity structureality -MinPasswordLength 12
        - The time definitions are using the format of D:H:M:S.F where: D = Days (0 to 10675199); H = Hours (0 to 23); M = Minutes (0 to 59); S = Seconds (0 to 59); and F = Fractions of a second (0 to 9999999).

        - You can make these same changes through the GUI interface of Group Policy Management.

9. Enter the following command to display the results of your changes to the domain password policy:

        - Get-ADDefaultDomainPasswordPolicy
   
    You should see the new values for several of the parameters of the password policy.

        - All existing account passwords are allowed to remain in their current non-compliant state. However, once the user changes their passwords, the new complexity requirements, such as length, will be enforced. You could choose to force all accounts to change their passwords upon their next sign in using the commands Get-ADUser -Filter * | Set-ADUser -PasswordNeverExpires $False and Get-ADUser -Filter * | Set-ADUser -ChangePasswordAtLogon $True. The first of these commands removes the password never expires state, and the second sets the password change required at logon state.

#### Check your work

Confirm that you viewed the current domain password policy.

Confirm that you set new values for the domain password policy.
