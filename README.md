# Assisted Lab: Exploiting and Detecting SQLi

## Objective

This activity is designed to test your understanding of and ability to apply content examples in the following CompTIA Security+ objectives:

2.3 Explain various types of vulnerabilities.
4.9 Given a scenario, use data sources to support an investigation.

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

### Perform SQL Injection

SQLi (Structured Query Language injection) is an attack where DBMS commands are injected into a website to manipulate the backend database. This is a variation of injection attacks that takes advantage of a website’s weakness in allowing submitted code and commands to execute. This is often accomplished through the use of metacharacters (i.e., symbols with programmatic power) which are not being properly filtered or escaped (i.e., reverted back to basic symbols without programmatic power).

In this exercise, you will be acting like an attacker. First, you will probe a target website for vulnerabilities, then perform reconnaissance to learn configuration details, then finally perform data extraction from the website's database.

This exercise uses the DVWA as the target of several database exploitations. However, you must first log into DVWA to access the various challenges. The DVWA has four difficulty levels (Low, Medium, High, and Impossible) and is set to the Low level by default.

 - DVWA or Damn Vulnerable Web Application is a safe and legal security playground that security professionals can use to improve their skills and learn tools and techniques related to web attacks and exploitations. DVWA is designed to be installed into a private (i.e., non-Internet) lab environment for internal use. Do NOT install DVWA on a production or an Internet-accessible system.

1. Connect to the KALI and sign in as root using Pa$$w0rd as the password.

2. Open Firefox, then in the address field of Firefox, enter dvwa.structureality.com.

  - If an error of Unable to connect is displayed, wait 30 seconds, then refresh the page. The LAMP VM may not have fully booted before you attempted to access the website.

  - If you see the DVWA login page, type admin and password into the Username and Password fields, respectively, then select Login.

3. The Welcome to Dann Vulnerable Web Application! page should be displayed.

  - If you scroll to the bottom of any DVWA page, you will see a footer that indicates several values, including the security level. To change the security level, select DVWA Security from the left-side navigation menu bar, make a selection from the pull-down list, then select Submit. This lab assumes the default security level of Low.

4. In the left-side navigation menu bar, select SQL Injection.

 The Vulnerability: SQL Injection page should be displayed.

5. Type 1 into the User ID: field, then select Submit.

6. The results should confirm that the User ID of 1 is the admin. Notice that the URL has changed to include parameters. It should look like the following:

  dvwa.structureality.com/vulnerabilities/sqli/?id=1&Submit=Submit#
  - The URL displayed after a form field submission often reveals details about the variables used in the server-side script. In a real-world situation, you would use this information to predict how the code is crafted and work towards discovering SQL statements that will enable you to perform arbitrary commands against the database or its underlying OS. In this DVWA simulation, you could select the View Source button at the bottom of the page to see the actual code in use on the server. Knowing the server script code is immensely helpful in discovering vulnerabilities to take advantage of.

7. Type 7 into the User ID: field, then select Submit.

8. There should be no result -- not even an error message stating that the User ID doesn't exist in the database.

  - You could experiment and try the numbers 2-6 to see if you can discover other existing user accounts and determine how many user accounts are present on the target system.

9. Type in a single quote character (i.e., ') into the User ID: field, then select Submit.

  - This is a common test to determine if a website is filtering metacharacters. Most SQLi attacks use the apostrophe or single quote.

10. This should return an error message stating:

 "You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near ''''' at line 1"

 Because of this error, you now know that the website is not filtering metacharacters and is, therefore, vulnerable to SQLi. You also know that the DBMS is MySQL.

  - The end of that error message statement will then show a series of five single quotes. This error message attempts to show you the syntax error in quotes. However, since the error is the occurrence of an odd number of quotes (i.e., you are causing the User ID string assignment to be three quotes instead of a number within quotes), the problem is the presence of three quotes (i.e., ''') which is then itself presented inside of single quotes.

  - Discovering information from error messages is known as error-based SQLi. Whether you happen across error messages or inject statements that cause specific error messages to be displayed purposely is an effective means of gathering information to further your efforts in SQLi attacks.

11. Select the back button on the Firefox toolbar to return to the Vulnerability: SQL Injection page.

12. Test to see if you can exploit the server script through Boolean logic. Type the following into the User ID: field, and then select Submit.
  1' or '1'='1
  - This is a typical initial SQLi method that uses logic to trick the target into revealing more information than its programmers intended. The concept is, you are setting up an OR expression between a variable condition (i.e., whether a User ID exists) and a tautology (i.e., a statement of truth (e.g., 1=1)). The results of this statement will always be true regardless of whether the variable condition is true. This results in a lack of context for the remainder of the script, which instead of returning a single entry as intended, the script may return all entries.

  - If you make a mistake in typing any of the SQLi statements and receive an error instead of the expected result, then use the back arrow on the Firefox toolbar to return to the Vulnerability: SQL Injection page and try again. Any mistakes you make will be reflected in the DVWA website access log on the LAMP host. You will see all of your submitted or injected values during the investigate SQLi exercise.

13. The result of this submission should be a presentation of all five user accounts on this website.

14. A common next step is to determine the column query limit. You must know the column query limit to abuse it. This will require trial and error to determine. Enter the following into the User ID: field, then select Submit.

  ' ORDER BY 1#
  - Most SQL injection statements start with a single quote (i.e., '). This initial single quote is used to terminate the string (i.e., whatever data is normally typed into the input field) assignment to a variable in the script on the web server. This means whatever follows that initial single quote will be perceived by the web server as code instead of string input. Be sure you are typing in the leading single quote, then a space, then the "ORDER BY…" statement.

  - The final octothorp (i.e., #) is used here as the end-of-line comment function for this MySQL target (you discovered the identity of this DBMS from the error message earlier). There are variations of SQL syntax between some DBMSes. For example, some DBMSes use double-dash (i.e., --) for this purpose.

  - In many instances, you are limited as to the amount of data (i.e., number of columns) you can retrieve through a SQLi based on what the script is already programmed to do. So, you need to determine the number of columns retrieved from the table. While you may be able to guess this based on the results seen when you provided valid input, it may be the case that more values are being retrieved by the script than what is being displayed on the resulting web page.

15. This should have no results. So, increment the number and try again.

16. Enter the following into the User ID: field, then select Submit.

  ' ORDER BY 2#
  - The SQL expressions used in SQLi do not need to be entered in all capital letters, but it is a common practice to do so anyway. This helps to differentiate the SQL expressions from the various object names or other command logic used in the longer and more complex SQLi statements. The SQL expressions commonly used in SQLi include ORDER BY, UNION, SELECT, UPDATE, INSERT, DELETE, and DROP. However, only some of these expressions will be used in this exercise.

17. This should have no results. So, increment the number and try again.

18. Enter the following into the User ID: field, then select Submit.

  ' ORDER BY 3#

19. This should result in an error message of "Unknown column '3' in 'order clause'". This indicates that the table has two (2) columns.

 With knowledge of the column limitation, you can now attempt to pull other data from the DBMS. To craft more advanced SQL injection queries, you need to know the name of the database and the name of the tables. Since the target's DBMS is a MySQL variant, you can use knowledge of standard MySQL elements to access more data.

  - Rather than assume you know everything about MySQL already, you are provided with several facts about default MySQL installations and the use of SQL expressions in the following steps.

20. Select the back button to return to the Vulnerability: SQL Injection page.

21. Confirm the DMBS version by typing the following command into the User ID: field, then select Submit:

    ' UNION SELECT @@version, NULL# 
  - Some website configurations may block the display of error messages, especially those that would reveal DBMS configuration details. So knowing how to elicit the DBMS version is often helpful.

  - The UNION SQL expression combines the operation of two or more SELECT expression statements. It is often used in SQLi to add an injected set of instructions to whatever the targeted system's script would have executed in normal conditions.

"NULL" is used as a placeholder in the second value position of the query.

22. The results should show the DBMS version as "10.5.19-MariaDB-0+deb11u2" in the "First name:" field and the "Surname:" field should be empty.

23. Type the following command into the User ID: field, then select Submit:

  ' UNION SELECT table_schema, table_name FROM information_schema.tables#
  - This SQLi expression is attempting to request two columns of data (i.e., table_schema, table_name) from the default DMBS database information container of MySQL (i.e., information_schema.tables). The table_schema value will be the name of the database (displayed on the First name: lines), and the table_name will be the name of a table within the database (displayed on the Surname lines).

24. The results should be a long list of database and table names. Since you know you are working against a DVWA website, the database of the same name is most likely being used. Look for the "dvwa" name in the "First name:" field and the names of the tables it contains in the "Surname:" field.

  - Keep in mind that the server script controls the presentation and organization of the retrieved database, while your injected command only affects what data is retrieved. So, the server-determined data layout and labeling are still being used to present the data you pulled from the table.

  - You might want to use the browser's search function (via CTRL+F) to find the entries with dvwa in the "First name:" field.

25. You now need to discover the column names of the tables. However, the fastest way to do that results in all columns from all tables being dumped at once. Type the following command into the User ID: field, then select Submit:

  ' UNION SELECT table_name, column_name FROM information_schema.columns#
  - This SQLi expression is attempting to request two columns of data (i.e., table_name, column_name) from the default DMBS table information container of MySQL (i.e., information_schema.columns). The table_name value will be the table's name (displayed on the First name: lines), and the column_name will be the name of a column within the table (displayed on the Surname lines).

26. The results should be a long list of table and column names. You elect to focus on the users table. Look for the "users" name in the "First name:" field and the contained column names in the "Surname:" field. Unfortunately, the order presentation of the columns is not always consistent, nor are all the column names from the same table necessarily grouped together.

27. Press CTRL+F on your keyboard to open the find function toolbar of Firefox. The find toolbar should appear at the bottom of the Firefox window.

28. In the empty search term field of the Firefox find toolbar, enter name: users, and then select to mark the Highlight All checkbox.

 There should be ten (10) results of column names from the users table. However, you only care about seven (7) of them (see Note). Select the up and down arrows on the Firefox Find toolbar to move between the results to view them all.

  - There are some results you can ignore, as they are not actual columns of the users table but are related to the operations and communications with the users table. These results will all be capitalized. They are: USERS, CURRENT_CONNECTIONS, and TOTAL_CONNECTIONS. (You could also see MAX_SESSION_CONTROLLED_MEMORY and MAX_SESSION_TOTAL_MEMORY,. but these are not useful in this context either). The actual users table column names will be listed in lowercase letters only (in this situation).

29. Close the Firefox find toolbar by selecting the X at the far-right end of the toolbar. Then, scroll to the top of the page (or press CTRL+HOME on your keyboard).

30. With knowledge of the available columns in a table, you can now attempt to retrieve the data from those columns. So, you decide to pull user names and passwords from the users table. Type the following command into the User ID: field, then select Submit:

  ' UNION SELECT user, password FROM users#
31. The results should be a presentation of all user accounts' usernames in the "First name:" field and the corresponding password hashes in the "Surname:" field.

  - At this point, you could export the enumerated password hashes to a file. Then use a password cracker to attempt to discover the passwords.

 As you can see, SQLi attacks can become quite complex and tedious very easily. And the elegance of the output is dependent upon the injected command. You can retrieve data in a raw dump that is hard to understand or you can format the output to your preferences.

 At this point, you have performed some basic SQLi attacks against the DVWA. In the next exercise, you will investigate the website's logs for evidence and IoCs of SQLi.

#### Check your work

Confirm that you tested the target website to determine that it was vulnerable to SQLi.

Confirm that you used Boolean logic to trick the database into revealing all users.

Confirm that you determined the DBMA version.

Confirm that you elicited the name of the DBMA and the names of tables.

Confirm that you extracted the names of columns from a table.

Confirm that you received all information from all columns of a table.

Confirm that you altered the presentation of retrieved data with more complex SQLi statements.

### Investigate SQLi

You have received a report that several users claim that they think their accounts' passwords have been compromised. There is also a report of a data dump on a hacker discussion forum containing several users' personal information. You are tasked with investigating the issue. You suspect that the website was the target of a SQLi attack. In this exercise, you will investigate the log of the company's website to see if you can find evidence or IoCs of SQLi.

1. Connect to the LAMP virtual machine and sign in as lamp using Pa$$w0rd as the password.

2. Elevate to use root privileges by entering: sudo su and then entering Pa$$w0rd as the password.

3. Enter cd /var/log/apache2 to change into the apache2 log directory.

4. Enter ls -l to view the log filenames, sizes, and timestamps.

5. Enter less access.log to view the website's access.log. Look over the log for anything interesting.

  - When using the less file viewing utility, press the spacebar to view the next page. You can return to a previous page using b or scroll one line up or down utilizing the arrow keys.

  - The first line of the access.log file is: /dev/null

  - You need to 'ignore' the directory path element of "/vulnerabilities/sqli/" as this is the obvious name of the HTML document on the DVWA (Damn Vulnerable Web Application) that is designed to demonstrate SQLi. In a real-world situation, you will not see the term "SQLi" in the logs. SQLi attacks are usually more subtle than that.

  - The Apache web server access log has two default log formats. The Common Log Format includes the following seven default fields:
   1. IP address of the client
   2. The identity of the client, but typically presented as only a hyphen (i.e., - )
   3. User ID of requesting user, but will be a hyphen when there is no established user context
   4. Date and time of the request (in square brackets)
   5. The HTTP request type (i.e., GET, POST, etc.) and the resource being requested
   6. The HTTP response status code
   7. The size of the object returned to the client
 The Combined Log Format includes the following two additional fields:

   8. The HTTP referrer (i.e., the address from which the request for the resource originated.)
   9. The User Agent of the client, which identifies information about the browser that the client is using to access the resource.
 It is also possible to customize the fields of the Apache logs.

 In this exercise, Apache is configured to use the Combined Log Format. Note: The User ID is a hyphen in the access.log for this exercise because when using the DVWA as the target, while you must log in as admin to access the vulnerable applications of the demo service, you are not using a user account or active login on most of the demonstration sub-pages.

6. Starting from the top of the access.log file (i.e., the oldest entry in the log), look down through the entries to find the one with the following as its HTTP request:

 "GET /vulnerabilities/sqli/?id=1&Submit=Submit# HTTP/1.1"
 This was your first submission to the SQLi page of just the number '1'. On its own, this is a record that could be benign or an element of reconnaissance.

7. Look at the referrer value for this log record.

  - The HTTP referrer indicates the URL of the page which was displayed in the browser of the user, which is the context from which the next URL is requested (i.e., the HTTP request). This record's HTTP request was submitted to the web server from the 'home' page of the SQLi site.

8. Look further down, maybe only a single record, to find the submission of the number '7'. The HTTP request should be:

 "GET /vulnerabilities/sqli/?id=7&Submit=Submit# HTTP/1.1"
 This was your next submission to the SQLi page of just the number '7'. On its own, this is also a record that could be benign or an element of reconnaissance. However, it begins to show a pattern of probing that could be considered SQLi pre-attack reconnaissance.

  - Notice that this record's referrer is the prior page which was the result of submitting the number '1'. For each remaining log record you look at, you should see this progression (i.e., the current HTTP request's log record will have the prior page as the referrer). While it is possible to backtrack to an initial page before each SQLi submission, it is not that common. Because typically, the attacker needs information from the results of a SQLi query to craft the next SQLi command.

9. The next log record should include the submission of a single quote. However, the log will not retain metacharacters. Instead, they will be converted to percent-encoded values. It is also possible that the SQLi statements will include pre-encoded percent encodings of metacharacters to avoid filters.

  - You may need to consult a reference table to determine the characters being obfuscated by the log. Some recognition of percent encoding will be necessary in order to interpret website log entries. Here is a partial reference table of commonly used percent encodings related to SQLi:

Encoding	Value
%20	(space)
%21	!
%22	"
%23	#
%27	'
%28	(
%29	)
%2b	+
%2c	,
Encoding	Value
%2f	/
%3a	:
%3c	<
%3d	=
%3e	>
%3f	?
%40	@
%5C	\
 	 
  - When dealing with hex values, such as those used in percent encoding, the case of the hex letter is irrelevant. They can be lowercase or uppercase without issue. Thus, %3c and %3C are the same when they are resolved into the < character.

10. The next log record should contain the SQLi code of the following:

 1' or '1'='1 
 This input will be encoded in the access log as:

 1%27+or+%271%27%3D%271 
 The record will have a full HTTP request of:

 "GET /vulnerabilities/sqli/?id=1%27+or+%271%27%3D%271&Submit=Submit#  HTTP/1.1"
 This is the first clear evidence of IoC of a SQLi statement. This is the injection of a logical operation that is intended to confuse the server-side script. In this instance, this injection string was submitted to a page to retrieve user information related to a User ID. As you recall, the result of this injection was a dump of all of the user accounts on the system.

11. Next in the log, locate a record with the HTTP request of the following:

 "GET /vulnerabilities/sqli/?id=%27+ORDER+BY+1%23&Submit=Submit#  HTTP/1.1"
 This is the first of three queries to determine the column query limit. This is another IoC observable that a SQLi attack is occurring.

12. Next in the log, locate a record with the HTTP request of the following:

 "GET /vulnerabilities/sqli/?id=%27+UNION+SELECT+@40%40version%2C+NULL%23&Submit=Submit#  HTTP/1.1"
 This is a SQLi statement used to determine the version of the DBMS running behind the web server. This is absolutely evidence of reconnaissance before initiating further SQLi attacks.

13. Next in the log, locate a record with the HTTP request of the following:

 "GET /vulnerabilities/sqli/?id=%27+UNION+SELECT+table_schema%2C+table_name+FROM+information_schema.tables%23&Submit=Submit#  HTTP/1.1"
 This is a SQLi statement used to extract all of the table names from the default DMBS database information container of MySQL (i.e., information_schema.tables). This is how an attacker learns the names of all of the tables hosted by a website's DBMS.

14. Next in the log, locate a record with the HTTP request of the following:

 "GET /vulnerabilities/sqli/?id=%27+UNION+SELECT+table_name%2C+column_name+FROM+information_schema.columns%23&Submit=Submit#  HTTP/1.1"
 This is a SQLi statement used to extract all of the column names from all of the tables from the default DMBS database information container of MySQL (i.e., information_schema.tables). This is how an attacker learns the names of all of the columns within each table hosted by a website's DBMS.

15. Next in the log, locate a record with the HTTP request of the following:

 "GET /vulnerabilities/sqli/?id=%27+UNION+SELECT+user%2C+password+FROM+users%23&Submit=Submit#  HTTP/1.1"
 This is a SQLi statement used to extract the columns of user and password from the users table. This is how an attacker is able to exfiltrate the password hashes of users. Once the attacker obtains the password hashes, they can initiate password cracking and potentially discover user passwords. In this exercise's scenario, this could be the means by which the attackers were able to take control of users' accounts.

  - The primary IoC for SQLi is the use of SQL expressions. If you see any of the following terms in an HTTP request, then there is a high likelihood that SQLi is taking place. The following is a partial table of common SQL commands used in SQLi statements:

SQL Expression	Description
ORDER BY	Sort data in ascending or descending order.
UNION	Combine the results of two or more SELECT statements.
SELECT	Retrieve certain records from one or more tables.
UPDATE	Modify records.
INSERT	Create a record.
DELETE	Delete a record.
DROP	Delete an entire table, a view of a table, or other objects in the database.

16. When finished looking over the access.log, type q to exit the less viewer.

With the evidence you have discovered from the access.log, you have clearly discovered IoCs of SQLi.

#### Check your work

Confirm that you analyzed the Apache access.log file for evidence and IoCs related to SQLi

Confirm that you discovered evidence of SQLi related to vulnerability testing.

Confirm that you discovered evidence of SQLi related to user account enumeration.

Confirm that you discovered evidence of SQLi related to determining the column query limit.

Confirm that you discovered evidence of SQLi related to disclosing the DBMS version information.

Confirm that you discovered evidence of SQLi related to extracting table names.

Confirm that you discovered evidence of SQLi related to extracting column names.

Confirm that you discovered evidence of SQLi related to exfiltrating user names and password hashes.
