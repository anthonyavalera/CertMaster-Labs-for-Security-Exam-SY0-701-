# Assisted Lab: Using Hashing and Salting

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

1.4 Explain the importance of using appropriate cryptographic solutions.
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

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

### Using hashing to confirm a file download

When downloading files, it is important to confirm that the file on the local drive has retained its integrity compared to the file being offered on the download site. This is often accomplished using hashing. In this exercise, you will work on a downloaded file and perform a hash check of the file.

  - The Security+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser.

1. On your local computer, open another tab in your current browser or open a new browser.

    - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

2. In your local browser's address bar, enter http://dftt.sourceforge.net/.

    - You can highlight and cut-n-paste this URL from the instructions into the address bar of your local browser.

    - The site dftt.sourceforge.net is the Digital Forensics Tool Testing image repository. This site contains 14 forensic images which can be used to test forensic analysis tools. These images are also useful in practicing and developing skills in using forensic tools and techniques before working on actual crime scene collected evidence. This is just one of many similar repositories of forensic image testing files.

3. Select EXT3FS Keyword Search Test #1.

    - The forensic test file needed for this lab has been made available to your lab environment via the ISO media of Student-Resources-L06.ISO. You DO NOT need to download the forensic test file to your local system.

4. Look over the contents of this page. You may need to return to this page later in this exercise.

5. Leave the local browser tab open that is focused on dftt.sourceforge.net.

6. Switch back to the browser tab focused on the Security+ Skillable virtual lab environment.

7. Connect to the KALI virtual machine and sign in as root using Pa$$w0rd as the password.

8. An icon of a DVD labeled as "Student-Resources-L06.ISO" should be on the Kali desktop, but it will be greyed out. Right-click on this DVD icon and select Mount Volume.

    - If the DVD icon is not on the Desktop: Select the Resources tab from the lab interface's Instructions area. On the Resources tab, select the DVD Drive pull-down list and select Student-Resources-L06.ISO. Then, select the Instructions tab to return to the lab steps.

9. Open a Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar.

    The Terminal window should already be elevated to use root privileges.

10. Maximize the Terminal window.

11. Enter the following command to view the contents of the DVD Drive:

    ls /media/cdrom0/
    
12. Enter the following command to copy the forensic test image files to the /root/Downloads directory:

    cp /media/cdrom0/* /root/Downloads/
    
13. Enter cd /root/Downloads to change into the directory.

14. Enter ls -l to view the contents and verify that the 4-kwsrch-ext3.zip file is present and is the size of 3954200 bytes.

15. Enter unzip 4-kwsrch-ext3.zip to extract the contents of the zip archive into its default subdirectories.

16. Enter ls -l 4-kwsrch-ext3 to view the contents of the new subdirectories.

    There should be four files. One of them is ext3-img-kw-1.dd, which should be 5242880 bytes in size.

17. Return to your local browser's tab focused on dftt.sourceforge.net where the EXT3FS Keyword Search #1 page is displayed.

18. Locate the MD5 hash in the paragraph below the Downloads heading.

    - Most binary numbers are converted to hex when presented on the screen. This is convenient as it uses 1/4 the number of characters to represent the same value. This is because four digits in binary can be used to represent the decimal values of 0 when all the bits are zero through 15 when all the bits are ones. These are the same ranges of values for a single hex character (i.e., 0-9 & A-F (10-15)).

With that information, you can count up the number of hex characters in a hash, then multiply by four (4) to determine the hash length. Then, by knowing the hash length, you can make a reasonable guess as to the hashing algorithm used. Here is a brief chart of common hashing algorithms:

Algorithm	Bit length	Hex length
MD5	128	32
SHA-1	160	40
SHA-224	224	56
SHA-256	256	64
SHA-384	384	96
SHA-512	512	128

19. Switch back to the browser tab focused on the Security+ Skillable virtual lab environment.

20. In the Terminal window, enter ls -l to view the contents of the extracted files.

21. Enter: cat 4-kwsrch-ext3-hash.txt.

  This file contains the hash of the ext3-img-kw-1.dd drive image file. It was copied directly from dftt.sourceforge.net. This should be the same hash value you viewed on the website for the drive image file.

22. Enter the following:

  md5sum 4-kwsrch-ext3/ext3-img-kw-1.dd >> 4-kwsrch-ext3-hash.txt
  
  This command hashes the file and adds that hash to the expected hash file.

23. Enter: cat 4-kwsrch-ext3-hash.txt.

  The first hash was copied directly from dftt.sourceforge.net for the ext3-img-kw-1.dd file. The second hash was added by hashing the file obtained from the DVD/ISO resource. Compare the two hashes to verify that they match. Since the two hashes match exactly, you have verified the file's integrity. The file offered by the website is the file present on the Kali system.

     - The use of MD5 hash has been deprecated. This means it is no longer the preferred or recommend hashing algorithm to use to verify integrity. It is often better to use SHA-256, SHA-512, or even SHA-1. However, many sites and services still use MD5 in spite of this. So, you have to check the integrity of the files you download using whatever hashing algorithm the site uses. The process performed in this exercise would be the same. You need to switch to the hashing utility for the specific hashing algorithm needed, such as sha1sum, sha256sum, or sha512sum.

    - Older hashing algorithms are not necessarily broken or compromised, but they are more prone to collisions. A collision is when two different datasets produce the same hash value (when hashed by the same hashing algorithm). This is a known aspect of hashing due to the fact that hash algorithms accept nearly infinite inputs to produce a hash of a fixed length. Generally, algorithms that produce a shorter hash value output are more prone to collisions than those with longer hash value outputs.
    
24. Leave the Terminal window open.

You have now used hash matching to confirm the integrity of a download file. This same process can be used to verify the integrity of any file from any source as long as you know the correct/expected hash and the hashing algorithm used to calculate that hash.

Check your work

Confirm that you obtained and extracted a forensic challenge image file.
Confirm that you performed a hack check on a file.

### Use the OPSWAT MetaDefender to evaluate files

You have discovered a file that you are concerned about on a client workstation. The user claims not to know anything about the file. You think that it could be a hacker tool and possibly malicious, but you want to confirm that before initiating a system wipe and rebuild. You decide to perform an initial evaluation of the file via hash identification. In this exercise, you will perform suspicious file analysis through an online evaluation service.

1. Connect to the KALI virtual machine and sign in as root using Pa$$w0rd as the password.

2. Open a Terminal window and then maximize the Terminal window.

3. Enter cd /usr/share/windows-resources/binaries to change into the folder where the suspicious file is located.

4. Enter ls -l to view a long listing of the contents of the directory.

  You should see a listing of several files. The file of concern is nc.exe.

5. Create a SHA1 hash value from the file by entering: sha1sum nc.exe.

  The hash result should be: 57F0839433234285CC9DF96198A6CA58248A4707

6. Leave the Terminal window open.

    - Typically, you would copy this value to your clipboard to then paste it into the hash lookup service website. However, since the lab environment does not have internet access, you will use a workaround in this exercise.

    - The CySA+ Skillable lab environment does not have direct internet access. Therefore, you must perform some tasks using your local browser.

7. On your local computer, open another tab in your current browser or open a new browser.

    - Be sure to leave the current local browser tab open, which is focused on the virtual lab environment. This will allow you to return to these instructions and perform additional steps.

8. In your local browser's address bar,
    - enter metadefender.opswat.com

    - Select the double paper icon to copy the URL into your clipboard or highlight then copy and paste this URL from the instructions into the address bar of your local browser.

  The "OPSWAT MetaDefender Cloud" webpage should be displayed.

9. Type or copy-n-paste the following hash into the Trust no field, then select Process.

    - 57F0839433234285CC9DF96198A6CA58248A4707
  The resuts page should indicate that this hash is related to the nc.exe file. Also notice that some, but not all, scanning engines detected something concerning about the file.

10. Select Get full report.

  This should display a page of several scanning engines and which of them detected a threat versus those that did not. Based on this information, you could make a decision on whether this suspicious file is malicious, a potentially unwanted program (PUP), or benign.

    - Just because an anlysis engine lists a file as a threat does not actually mean it is malicous code. It is possible that the file is an administrative utility or even a hacker tool which could be used for malicious purposes, but it not inhereantly malicious. For example, a network sniffer is a useful tool, but not something you would want your typical user to have on their system. The same is true for keystroke loggers and password crackers. Not all anlysis engines include PUPs in their detection databases.

11. Perform additional searches for other files. You can use file names or hashes from your own system, or you can return to the Kali VM and obtain file names and hashes from there. For example, you could search for information on the klogger.exe file using the SHA1 hash of:
    - 196BF6F43F85F97CC2851C840DA8E451256995CB.

Check your work

Confirm that you searched a malware research service using a file’s SHA1 hash value.

### Explore salting

Salting is a means to improve the strength of hashing as a defense against brute force password cracking attacks. The salting process is performed automatically by some OSes, such as Linux. In this exercise, you will view the existence of salts for existing user accounts. Next, you will create password hashes both with and without salting. Finally, you will crack the non-salted password quickly, and then you will see how salted password cracking takes significantly more time.

1. Connect to the KALI virtual machine and, if needed, sign in as root using Pa$$w0rd as the password.

2. A Terminal window should already be open.

3. Enter cd ~.

  This command returns you to the root user's home directory (i.e., /root).

4. Enter grep '\$' /etc/shadow.

  This command displays the entry lines for only those accounts which have a stored password hash.

    - Modern Linux systems salt password hashes by default. The most common default hashing scheme of Linux is yescrypt. This is confirmed in the output by the "y " between the first two dollar signs. The "j9T " value between the second and third dollar signs are parameters used during the hashing process. The value between the third and fourth dollar signs is the salt used when producing the hash. Then, the value after the fourth dollar sign (until the colon) is the salted password hash.

5. Since hashes produced by the yescrypt algorithm are extremely difficult to crack, generate your own password hashes to crack. Enter:

     - openssl passwd -salt "" pass1 > hash.txt
  This command will generate an MD5 hash of the password pass1 without using salting and saves it in the hash.txt file.


6. Enter cat hash.txt to view the contents of the file.

  Notice that there is no value between the second and third dollar signs. This indicates the password hash is not salted.

    - The openssl tool generates MD5 hashes by default. This is encoded in the hash output by the 1 value between the first two dollar signs. There are other password hash options available by using parameters. For example -5 uses SHA-256 and -6 uses SHA-512. Each different hashing algorithm may alter the hash presentation. For example, MD5 hashes don't have parameters; thus its output only contains the algorithm, salt, and hash.

7. Use John the Ripper to perform a brute force password crack against the unsalted password hash by entering:

    - john -incremental hash.txt
  John should crack the unsalted password hash in less than 10 seconds.

8. Generate a salted password hash by entering:

    - openssl passwd -salt SALT pass1 > salted-hash.txt
  A proper salt value is a random string. Some hashing algorithms support salts of up to 512 bytes. Here you are using a pre-selected salt of SALT to be very obvious when viewing the hashing output.

9. Enter cat salted-hash.txt to view the contents of the file.

  Notice that the salt value of SALT is present between the second and third dollar signs.

10. Use John the Ripper to perform a brute force password crack against the salted password hash by entering:

    - john -incremental salted-hash.txt
  John should crack the salted password hash in less than 10 seconds. The full Linux hash statement is present in the salted-hash.txt file. Therefore, John is provided the salt value, which allows it to quickly crack the password.

11. Remove the salt value from the file by entering:

    - cat salted-hash.txt | sed "s/SALT//g" > salt-secret-hash.txt

12. Use John the Ripper to perform a brute force password crack against the salted password hash when the salt value is not known by entering:

    - john -incremental salt-secret-hash.txt
  John will take up to 814,506,250 seconds (~25.8 years) to crack the salted password when it does not have knowledge of the salt value. This is because the hash is effectively produced from 9 characters instead of just the 5 of the original password.

    - The default character set used by John the Ripper is the standard US-ASCII table of 95 characters (i.e., uppercase, lowercase, numbers, and symbols present on a standard US keyboard). Thus, with four salt characters (which could be any of the 95 character options) and the original five-character password (i.e., pass1), which took 10 seconds (or less) to crack, the salted hash of that password (with a 4 character salt) would take 10 seconds x 95 x 95 x 95 x 95 = 814,506,250 seconds.

13. Wait about 10 seconds, then press the spacebar on your keyboard to get a status report from John. Wait 10 more seconds, then press the spacebar again. You can repeat this several times.

  Notice the far-right status element. This is the range of passwords being attempted at the instant you pressed the spacebar to present the status message.

    - The status output includes: successful guesses (#g), time elapsed, successful guesses per second (#g/s), candidate passwords tested per second (#p/s), "crypts" (password hash or cipher computations) per second (#c/s), combinations of candidate password and target hash per second (#C/s), and current candidate passwords.

14. Type CTRL+C to terminate John.

Check your work

Confirm that you viewed Linux accounts salted password hashes.
Confirm that you created salted and unsalted password hashes.
Confirm that you cracked an unsalted password hash.
