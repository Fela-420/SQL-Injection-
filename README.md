# SQL-Injection-
<img width="700" height="370" alt="image" src="https://github.com/user-attachments/assets/e0084df7-a002-4559-b9cf-2b5353acbf12" />

It’s been a while since I dove into the world of Hack The Box (HTB), and as I logged back in to continue my learning journey, I had a bit of a moment — realized I completely forgot which email I used to register my account. I know, I know… a classic mistake! As a SOC Analyst working in cybersecurity, you’d think I’d have my digital footprint under control, but here we are.


Anyway, after a bit of a reset, I’m back and excited to share my experience working through one of the fundamental challenges that’s a must-learn for any cybersecurity professional: SQL Injection.


SQL Injection (SQLi) is one of the most common attack vectors out there, and it’s a crucial skill to master when you’re diving deep into penetration testing or even just strengthening your ability to identify vulnerabilities in your environment. In this writeup, I’ll walk you through the steps I took to solve the SQL Injection challenge on HTB, discussing the concepts behind it, the tools and techniques I used, and — of course — some lessons I learned along the way. Let’s get started!

Challenge 1: Intro to MySQL
In this challenge, the task was to connect to a MySQL database and find the name of the first database.

Target(s): 94.237.63.109:52639
Authenticate to 94.237.63.109:52639 with user “root” and password “password”

Connect to the database using the MySQL client from the command line.

Use the show databases; command to list databases in the DBMS.
What is the name of the first database?
(Note: IP has been generated.)

Connect to the MySQL Database
To connect to the target MySQL database, I used the MySQL client command-line tool. The command is:

mysql -h 94.237.63.109 -P 52639 -u root -p
-h 94.237.63.109: Specifies the IP address of the target MySQL server.
-P 52639: Defines the port number where the MySQL server is listening.
-u root: The username for authentication, in this case, root.
-p: Prompts for the password, which is password.
Once connected, I was able to access the MySQL prompt.

List the Databases
After successfully logging in, the next step was to list the databases available in the MySQL server. The command for this is:

SHOW DATABASES;
This command returns all the databases on the server. The first database listed is the answer to the challenge.

Identify the First Database
The first database that appeared in the list was employees, which is the correct answer for this challenge.
<img width="667" height="402" alt="image" src="https://github.com/user-attachments/assets/bfb89841-c2b6-4969-8851-225ee649fca8" />


Result:

The name of the first database is employees.

This step is crucial in understanding how to interact with MySQL servers, and it provides the necessary groundwork for exploring SQL injection vulnerabilities in later challenges.

Challenge 2: INSERT Statement
In this challenge, the task is to find the department number for the ‘Development’ department in the database. This requires accessing the correct table, searching for the department, and extracting the relevant data.

Target(s): 94.237.63.109:52639

Get ViraSecurity’s stories in your inbox
Join Medium for free to get updates from this writer.

Enter your email
Subscribe
Authenticate to 94.237.63.109:52639 with user “root” and password “password”

the same as before, but query for the department:
SHOW TABLES;SELECT * FROM departments
This sequence revealed the department number for ‘Development,’ which was d005.

Result:

The department number for the ‘Development’ department is d005.
![Uploading image.png…]()

Zoom image will be displayed
Zoom image will be displayed
Zoom image will be displayed
Challenge 3: Query Results
Target(s): 94.237.63.109:52639
Objective: Retrieve the last name of the employee whose first name starts with “Bar” AND who was hired on 1990–01–01.
Credentials:

Username: root
Password: password
Just the same as before ….BUT! Query Execution!
To retrieve the last name of the employee whose first name starts with “Bar” and who was hired on 1990–01–01, I used the following SQL query

SELECT last_name * FROM employees *8 WHERE first_name LIKE 'Bar%' AND hire_date = '1990-01-01';

Query Breakdown:

SELECT last_name: Retrieves only the last name column.
FROM employees: Specifies the table to query from.
WHERE first_name LIKE ‘Bar%’: Filters employees whose first name starts with “Bar”.
AND hire_date = ‘1990–01–01’: Ensures the employee’s hire date matches January 1, 1990.
Result:

The last name of the employee meeting the conditions is Mitchem.


Challenge 4: SQL Operators
Target(s): 94.237.63.109:53202
Objective: Determine the number of records in the titles table where the employee number is greater than 10,000 OR their title does NOT contain 'engineer'.
Credentials:

Username: root
Password: password
The initial step was to connect to the MySQL server using the given target and credentials (because I am stoopid):
mysql -h 94.237.63.109 -P 53202 -u root -p
However, in my haste to solve the challenge, I overlooked establishing the connection properly and ultimately ran out of the time limit for this task. Despite this, I was able to construct the appropriate query for the challenge.


Now, for the awaited Query Execution
Had I successfully connected to the server, the query to retrieve the number of records meeting the conditions would have be:

MariaDB [(none)]> SELECT COUNT(*) FROM titles WHERE emp_no > 10000 OR title NOT LIKE '%engineer%';  
+----------+  
| COUNT(*) |  
+----------+  
|      654 |  
+----------+  
1 row in set (0.066 sec)
Result :

The number of records in the titles table where the employee number is greater than 10,000 OR the title does not contain 'engineer' is 654.

