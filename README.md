# Assisted Lab: Use Cases of Automation and Scripting

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

4.3 Explain various activities associated with vulnerability management.
4.7 Explain the importance of automation and orchestration related to secure operations.
5.4 Summarize elements of effective security compliance.

### Tools Used

- Terminal Emulator

## Steps

### Automation of IP range firewall block rules from a threat feed

You are subscribed to a threat intelligence feed that provides a list of IP addresses known to be used for malicious activity. In this exercise, you will automate the process of retrieving the IP addresses from this threat feed and create firewall rules based on them by creating your own bash script.

Select the KALI VM and sign in as root using Pa$$w0rd as the password.

Open a Terminal window by selecting the Terminal Emulator from the Kali Linux toolbar.

Maximize the Terminal window.

You are subscribed to a threat intelligence feed that provides a list of IP addresses that have been identified as being used by malicious actors. Enter the following command to view this threat feed:

curl 127.0.0.1/evil_IP.feed
The lab environment does not have internet access. Therefore, this threat intelligence feed is being simulated for this exercise. This simulated feed contains IP address ranges that were known to be the source of malicious actors in early 2023. These IP address ranges are from the FireHOL service at iplists.firehol.org.

This command retrieves and displays the contents of the IP threat feed to the screen.

Enter the following to add a new iptables rule to block communications from the first IP address range from the feed:

iptables -A INPUT -s 5.134.128.0/19 -j DROP
Enter iptables -S to view the current iptables rules.

You realize that manually creating rules for each IP address range from a threat feed will be quite tedious and a task you may need to perform daily. So, you decide to automate the process.

Enter cat ip_block.sh to view a script.

This script will retrieve the IP address threat feed, extract the IP address and CIDR notations, then, for each IP address range, it will add a drop rule to iptables.

If you also wanted to add rules to iptables to drop outbound packets, which of the following lines could be added to the script?

iptables -A FORWARD -s $IP -j DROP
iptables -A OUTPUT-s $IP -j DROP
iptables -A EGRESS -s $IP -j DROP
iptables -A EXTERNAL -s $IP -j DROP
Enter chmod +x ip_block.sh to set the execute permissions on the file.

Enter ./ip_block.sh to execute the script.

You should see several lines of output indicating that IP address range drop rules have been added to iptables.

Enter iptables -S to view the current iptables rules.

You should notice that there is at least one (1) duplicate rule. This will be addressed later in this exercise.

You now have a script that will update the iptables filter rules with IP address ranges from a threat intelligence feed. However, to complete the configuration of security automation, you need to schedule this script to run regularly.

Automate the execution of the script so it runs daily at 1 AM by entering the following:

echo "0 1 * * * /bin/bash /root/ip_block.sh" | crontab -
This command inserts the scheduled task details into the cron table without having to open the file to edit it in a more manual fashion.

The scheduled task configuration in the quotations of this command has the following elements:

0 : The minute when the job should run.
1 : The hour when the job should run (1 AM).
* : The day of the month when the job should run (every day).
* : The month when the job should run (every month).
* : The day of the week when the job should run (every day of the week).
/bin/bash : specifies that the job should be run using the Bash shell
/root/ip_block.sh : the path to the script that should be executed.
Select the Score button to validate this task:

Enter crontab -l to view the scheduled tasks.

The automation of your IP blocking from a threat feed is nearly complete, but it needs testing and refinement. While you could wait until 1 AM tomorrow for the feed to update, you will simulate this with a pre-written script.

Enter ./update_feed.sh to update the threat feed with additional IP addresses.

This command is used to simulate the occurrence of the threat intelligence provider adding new IP ranges to the feed.

Enter ./ip_block.sh to pull the updated threat feed and add iptables block rules.

This command is used to simulate the auto-triggering of the scheduled task.

Now that a day has passed (wink wink), check the iptables rules list by entering iptables -S.

You should see a longer presentation of filter rules than before, including several for the IP address ranges that were recently added to the threat feed. However, you notice that there are several duplicate rules for the same IP address. While the filtering may still function with duplicate rules, this will become an overly large and cumbersome list quickly without addressing the duplicate rule problem.

You need to add an additional command to the scheduled task script to address the duplicate rule issue. Enter vim ip_block.sh to edit the script.

Use the arrow keys on your keyboard to move the cursor to the final line of the existing script.

Type i to enter insert mode. The message -- INSERT -- should be present at the bottom of the screen.

If your cursor is not on an empty line already (because there is no empty line after the "done" statement), move the cursor to the end of the line, then press Enter on your keyboard.

Press Enter on your keyboard again to ensure there is a blank line after the "done" statement and a 2nd empty line where you will type the next statement.

Type the following into the 2nd empty line you have created:

iptables-save | awk '!seen[$0]++' | iptables-restore
The result should look like the following (although you may have left out the comments):

#!/bin/bash

# Retrieve IP addresses with CIDR notation from an HTML file using regex
IPS=($(curl -s 127.0.0.1/evil_IP.feed | grep -oE "([0-9]{1,3}\.){3}[0-9]{1,3}(/[0-9]{1,2})?"))

# Add an IP block to iptables for each IP address
for IP in "${IPS[@]}"
do
  iptables -A INPUT -s $IP -j DROP
  echo "IP address range drop rule for $IP added to iptables"
done

iptables-save | awk '!seen[$0]++' | iptables-restore
This additional command statement will export the iptables rule set through the awk command, which will filter out duplicates (by suppressing already-seen lines). Then it will re-import the de-duplicated rule set back into iptables.

Once finished, press ESC to exit insert mode.

Pressing ESC may cause your browser to exit full-screen mode. If that occurs, press ESC a second time to exit VIM's insert mode. Then you can re-enable full-screen mode from the Display lab interface menu.

Enter :wq to save and quit VIM.

Select the Score button to validate this task:

Enter cat ip_block.sh to view the contents of the script file you have edited.

Enter ./ip_block.sh to execute the script you just updated.

This execution of the same IP blocking script will retrieve the threat feed again and add the IP range block rules to iptables which will result in even more duplicates. However, the final line of the script will then remove the duplicates.

Enter iptables -S to view the rules list.

Notice that there are no longer duplicate rules.

This is a simple example of threat feed automation. This example does not take into account IP address ranges that fall off the malicious lists and are, therefore, potentially usable again. While you may need to manually create automation for threat feed processing, you may also find that some products already support threat feed processing and integration. Additionally, some threat feed sources will provide you with example code to automate the integration of their feeds datasets into your security environment, which will take into account a wide range of conditions, such as removing expired rules or out-of-date protections.

...less
Leave the Terminal window open.

You have automated firewall block rules based on a threat intelligence feed related to malicious IP address ranges.

Check your work
Confirm that you created a script to create firewall rules from a threat feed.
Confirm that you automated the IP blocking script.
Confirm that you adjusted the script to address rule duplicates.

### Automation of malware removal based on threat feed

You are subscribed to a threat intelligence feed that provides a list of malicious files and their corresponding hashes. This feed claims to offer access to IoC (Indicator of Compromise) observables regarding zero-day malware discoveries before typical malware scanners integrate the dataset into their detection databases. In this exercise, you will automate the task of detecting and removing malware based on information from the threat feed by altering a provided script.

Select the KALI VM and, if needed, sign in as root using Pa$$w0rd as the password.

The maximized Terminal window should still be open.

Enter the following command to view the malware threat feed:

curl 127.0.0.1/mal_hash.feed
The lab environment does not have internet access. Therefore, this threat intelligence feed is being simulated for this exercise. This simulated feed contains hashes and filenames, which may be present on company systems. The contents of this feed are not related to actual malware. Instead, they are the hashes of two PUPs (potentially unwanted programs): nc.exe and klogger.exe, as well as an incorrect hash for vncviewer.exe and backdoor.exe.

This command retrieves and displays the contents of the malware threat feed to the screen.

You would like to have a script that will automatically retrieve the content of the malware threat feed, extract the hash and filename values, then scan the system for matches and remove those files that match both a filename and the corresponding hash. You discover that the malware threat feed organization has provided an example bash script. Enter less remove-malware.sh to view the provided script.

The script will be displayed through the less viewer. Notice that the script will retrieve the threat feed URL, then extract the hash and filename for each entry of malicious code. Then it will scan the system for the file by filename, and if discovered, it will compare the hash value of the file to that from the feed. Finally, the script will delete files that match both the filename and the hash but otherwise will take no action on files that only match the filename.

When using the less file viewing utility, press the spacebar to view the next page. You can return to a previous page using b or scroll one line up or down utilizing the arrow keys.

When finished looking over the script, type q to exit the less viewer.

What hashing algorithm is being used by the script?

SHA512
MD5
SHA-1
SHA256
While reviewing the script, you realize that it will scan the entire system drive, which would take considerable time. That won't be a problem when you configure the script to run automatically overnight, but you want to test the script now without wasting too much time. Enter vim remove-malware.sh to edit the script.

Enter :set number to turn on line numbers in the vim editor.

Use the arrow keys on your keyboard to move the cursor to line #32, which should be the following:

  done <<< "$(find / -name "$filename" -type f -print 2>/dev/null)"
Type i to enter insert mode. The message -- INSERT -- should be present at the bottom of the screen.

Use the arrow keys to move the cursor to the space just after the "find /" statement, then type usr/share so that the line reads as follows:

  done <<< "$(find /usr/share -name "$filename" -type f -print 2>/dev/null)"
Once finished, press ESC to exit insert mode.

Pressing ESC may cause your browser to exit full-screen mode. If that occurs, press ESC a second time to exit VIM's insert mode. Then you can re-enable full-screen mode from the Display lab interface menu.

Enter :wq to save and quit VIM.

Select the Score button to validate this task:

Enter ls -l to view the directory in long list format.

Notice that the remove-malware.sh script is not executable.

Enter chmod +x remove-malware.sh to set the script to be executable.

Before you actually run the malware removal script, you remember that you think you have seen a few of the files from the threat feed on the system before. Enter ls -l /usr/share/windows-resources/binaries/.

This displays the contents of a directory where you should see numerous files, including three that are listed on the malware threat feed.

Enter ./remove-malware.sh to test the script against the subfolders under /usr/share for malicious files.

The script should present numerous output statements as it performs the scan for malware. Be patient; it may take up to a minute for the scan to complete and results to be displayed on the screen.

What "malicious" files were found and removed by the script based on the malware threat feed? (Select all that apply)

/usr/share/windows-resources/binaries/nc.exe
/usr/share/windows-resources/binaries/vncviewer.exe
/usr/share/windows-resources/binaries/klogger.exe
/usr/share/sqlninja/apps/nc.exe
/usr/share/seclists/Web-Shells/FuzzDB.nc.exe
Enter ls -l /usr/share/windows-resources/binaries/ to see if any of the files were removed from the directory of concern.

Notice that nc.exe and klogger.exe are no longer present.

Now that you have confirmed the functionality of the script, you could edit the script to return it to scanning all directories on the system.

This is not a required task for this exercise.

Automate the execution of the script, so it runs daily at 2 AM by entering the following:

echo "0 2 * * * /bin/bash /root/remove-malware.sh" | crontab -
Select the Score button to validate this task:

Enter crontab -l to view the currently scheduled tasks.

Leave the Terminal window open.

This is only an example script for this exercise. This script is rather limited in its functionality. For example, this script does not check for files that would match the hash value but have a different filename. This approach was not included as it would take considerably more time to accomplish without severely restricting the scanned directories.

You have automated malicious file removal based on a threat intelligence feed.

Check your work
Confirm that you viewed a simulated malware threat feed.
Confirm that you viewed and edited a script to use IoC observables from the threat feed to remove malware.
Confirm that you executed the malware removal script, confirmed its' operation, and scheduled it to run daily.
