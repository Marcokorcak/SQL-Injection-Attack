# SQL-Injection-Attack Description 

SQL injection is a code injection technique that exploits the vulnerabilities in the interface between web
applications and database servers. The vulnerability is present when user’s inputs are not correctly checked
within the web applications before being sent to the back-end database servers.
Many web applications take inputs from users, and then use these inputs to construct SQL queries, so
they can get information from the database. Web applications also use SQL queries to store information in
the database. These are common practices in the development of web applications. When SQL queries are
not carefully constructed, SQL injection vulnerabilities can occur. SQL injection is one of the most common
attacks on web applications.
In this project, we have created a web application that is vulnerable to the SQL injection attack. Our
web application includes the common mistakes made by many web developers. Students’ goal is to find
ways to exploit the SQL injection vulnerabilities, demonstrate the damage that can be achieved by the
attack, and master the techniques that can help defend against such type of attacks. This project covers the
following topics:
* SQL statements: SELECT and UPDATE statements
* SQL injection
* Prepared statement 

## Table of contents
* [Lab Setup](#Lab-Setup)
* [Task 1 Get Familiar with SQL Statements](#Task-1-Get-Familiar-with-SQL-Statements)
* [Task 2-1 SQL Injection Attack from webpage](#Task-2-1-SQL-Injection-Attack-from-webpage)
* [Task 2-2 SQL Injection Attack from command line](#Task-2-2-SQL-Injection-Attack-from-command-line)
* [Task 2-3 Append a new SQL statement](#Task-2-3-Append-a-new-SQL-statement)
* [Task 3-1 Modify your own salary](#Task-3-1-Modify-your-own-salary)
* [Task 3-2 Modify other peoples salary](#Task-3-2-Modify-other-peoples-salary)
* [Task 3-3 Modify other peoples password](#Task-3-3-Modify-other-peoples-password)
* [Task 4 Countermeasure - Prepared Statement](#Task-4-Countermeasure-Prepared-Statement)

# Lab Setup
<img src= "https://user-images.githubusercontent.com/77298953/210660510-1002d71c-0d47-459e-941d-3f4ceb9e979c.PNG" width=70% height=70%>

Lab Setup Image

When setting up the environment for this lab, we edited the /etc/hosts file using the nano editor

tool and commented out the lines that will be used in later labs. In order to edit this file successfully

and save the changes made, we used the sudo command. Moreover, we added the urls for the two

websites that will be used throughout this lab along with their IP addresses, configuring the DNS

for the domains. We then built the containers using the “docker-compose build” command and

then started them using the “docker-compose up” command to ensure a smoothly running

environment in the virtual machine.


# Task 1 Get Familiar with SQL Statements
<img src= "https://user-images.githubusercontent.com/77298953/210660629-9a8ed0b2-ef38-49ab-b92f-a04547295cec.PNG" width=70% height=70%>

Image of the data stored in credential table for Alice

</br>


## Task 1 Explanation 

For this task in the lab, we were instructed to familiarize ourselves with SQL

statements and write a query that would print out all the profile information of the employee, Alice.

Therefore, we used SELECT \* FROM credential WHERE Name = ‘Alice’ to only print out a table

of an employee whose name is specified as Alice in the table, credential. In SQL, the asterisk(\*)

character is used to print out all columns for the employee specified, which in this case is Alice.


# Task 2-1 SQL Injection Attack from webpage

<img src= "https://user-images.githubusercontent.com/77298953/210660840-cd8dc6df-fe55-4769-b75f-3421cf67b2bb.PNG" width=70% height=70%>

Image of unsafe\_home.php file contents

<img src= "https://user-images.githubusercontent.com/77298953/210660887-a022c7c6-c12b-4e7f-a483-fa2d3c60728a.PNG" width=70% height=70%>

Image of the input used to gain access to the admin account

<img src= "https://user-images.githubusercontent.com/77298953/210660913-f9979ac2-2577-4f15-a968-be347652e967.PNG" width=70% height=70%>

Results displayed when logging into the admin account


## Task 2-1 Explanation
For this task the objective was to perform a SQL Injection attack on the webpage so

we can see employee information by logging in as an administrator. In order to accomplish this

task, we decided to look at the unsafe\_home.php file in the image\_www directory. Looking at this

file we were able to see the code that was used for getting information from the database. We

added the code “echo $sql;” into the file before the if statement. By adding this bit of code, the

hashed password was displayed at the top of the webpage, causing us to notice that the SQL code

had a vulnerability in it. In order to get access to the administrator account without knowing the

password, the username we entered was admin’# and the password field was left blank. Inputting

this information, we were granted access into the administrator account. This is because when

adding a ‘#’ after the username, the rest of the SQL query becomes commented out. This worked

around the original code because it gave us access without knowing the password for the

administrator account.


# Task 2-2 SQL Injection Attack from command line

<img src= "https://user-images.githubusercontent.com/77298953/210661110-482189b8-cbc1-4f1e-8982-4e87aaf427e0.PNG" width=70% height=70%>

Output of curl command showing a functional SQL injection attack


## Task 2-2 Explanation 
For this task in the lab, we performed an SQL injection attack via the command line

using curl. This command line tool sent out an HTTP GET request to display the data on the admin

user, which contained data on all of the employees. The HTML code displayed all columns of the

credential table, such as the employees’ usernames, eid, salary, birthday, and social security

number. This curl command also made the administrator’s hashed password visible, which was a

variation of letters and numbers. It was important to encode the special characters in the username

or password fields, to ensure that the meaning of the requests was what we intended it to be.

Therefore, %27 was used to represent a single quote and %23 was used to represent the ‘#’

character. These are URL encoded characters, as the normal # and ‘ are not usable in a URL. The

‘#’ character was necessary for the attack to work because it commented out the password field,

enabling us to access the data stored in the administrator without specifying its correct password.



# Task 2-3 Append a new SQL statement

<img src= "https://user-images.githubusercontent.com/77298953/210661273-8eafe2ea-4e13-42cc-96e3-72d526a811fc.PNG" width=70% height=70%>

Error thrown when trying to run two SQL statements

<img src= "https://user-images.githubusercontent.com/77298953/210661336-03ea56b4-00ad-4661-b6c8-343054af705b.PNG" width=70% height=70%>

File contents of unsafe\_home.php


## Task 2-3 Explanation 
For this task the objective was to modify the database using the same vulnerability

exploited in the task above but using two SQL statements. For this attack the second statement

would have to be a UPDATE or DELETE statement that would be separated from the first

statement using a semicolon. When attempting this attack, regardless of the SQL statements

written, we always received an error and noticed this attack could not be carried out successfully.

The web page would always throw an error saying the query could not be run so we decided to

look as to why this would not work. We looked at the unsafe\_home.php file and we noticed that

the php code used an extension called mysqli. This extension uses an API that does not allow

multiple queries to run in the database server, due to the concern of SQL injection attacks. Since

this extension was used, the appending tactic could not be used to successfully modify the

database.


# Task 3-1 Modify your own salary
<img src= "https://user-images.githubusercontent.com/77298953/210661517-a293e01f-53c7-417c-84c7-6e3c5aedd6c2.PNG" width=70% height=70%>

SQL query that was inputted in order to modify salary

<img src= "https://user-images.githubusercontent.com/77298953/210661567-7dbd489d-0975-451b-b5d9-3c0ec54c6920.PNG" width=70% height=70%>

Results when the previous SQL query was ran


## Task 3-1 Explanation
In this task, we exploited the SQL injection vulnerability in the Edit-Profile page

and modified Alice’s salary. We injected an SQL statement into the Nickname field on Alice’s

profile, which increased her salary to $100,000. The query that we inputted into the NickName

field was ‘, salary=100000 WHERE eid=10000;# . Typically, users are not authorized to change

their salaries, but this attack was successful due to us passing it into a field that is allowed to be

edited, the user’s nickname. This allowed us to successfully edit Alice’s salary because when the

nickname field was edited, the SQL query was executed, forcing Alice’s salary to be altered.


# Task 3-2 Modify other peoples salary

<img src= "https://user-images.githubusercontent.com/77298953/210661857-829da7a3-3534-4f94-9c75-584f1a96f697.PNG" width=70% height=70%>

SQL query that was entered in order to change Boby’s salary

<img src= "https://user-images.githubusercontent.com/77298953/210661894-a36e5a18-8603-4826-907a-3e395132cf6f.PNG" width=70% height=70%>

Results of running the SQL commands


## Task 3-2 Explanation
For this task, the objective was to modify other peoples’ salaries, so we specifically

wanted to change Boby’s salary to $1. In order to complete this task, we utilized the same strategy

that was used in the previous one. Inside of the Nickname field on Alice’s account, we entered ,’

salary=1 WHERE Name=’Boby’:#. Once this was inputted, we clicked save which ran the query

with no errors. In order to check if the query ran correctly, we logged into Boby’s account and saw

that his salary was updated to $1.


# Task 3-3 Modify other peoples password
<img src= "https://user-images.githubusercontent.com/77298953/210662177-68b460c7-2749-4577-ae71-d5b632c178fc.PNG" width=70% height=70%>

The hashed password that was generated in the terminal using Sha1

<img src= "https://user-images.githubusercontent.com/77298953/210662207-1f212e64-772a-45a5-8f3a-3112795dbcba.PNG" width=70% height=70%>

<img src= "https://user-images.githubusercontent.com/77298953/210662247-ef8e5989-f0b6-4d3f-b046-4211ae65c789.PNG" width=70% height=70%>

Image of the new hashed password generated stored in the database confirming it was successfully changed

<img src= "https://user-images.githubusercontent.com/77298953/210662287-95b742ad-5565-4c9d-a6db-cf46244c57d4.PNG" width=70% height=70%>

Image of Boby’s profile with the new hashed password


## Task 3-3 Explanation
In this task, we modified Boby’s password by first using the sha1() hash function

to hash a new password. In order to do so, we used the following command to change his

password to aliceisgreat, “echo -n aliceisgreat | sha1sum”. We then copied the new hashed

password and updated Boby’s password through Alice’s edit profile page. We inputted the

following SQL query into Alice’s NickName field in order to successfully change Boby’s

password, Password=‘d65cf3d9dbdde06b391e50785c1c9ecd1458756f’ WHERE Name =

‘Boby’;#. The NickName field is a field that Alice herself can alter, so we injected an SQL query

that will be executed, forcing Boby’s password to be modified. We then printed out Boby’s data

stored in the credential table, to check if his password was successfully changed. Finally, we

logged into Boby’s account without any issues using the new hashed password we generated,

indicating that his password has been compromised and modified in this attack.


# Task 4 Countermeasure - Prepared Statement

<img src= "https://user-images.githubusercontent.com/77298953/210662514-91d10d1a-ac3e-416a-b71d-55480af86250.PNG" width=70% height=70%>

Image of the prepared statement that was added to the unsafe.php file
<img src= "https://user-images.githubusercontent.com/77298953/210662548-887fe0be-1411-4732-85d2-6e99794a0848.PNG" width=70% height=70%>

Output when logging in with a valid account

<img src= "https://user-images.githubusercontent.com/77298953/210662573-3a5cafb9-5eeb-41aa-897a-858be3a4fb29.PNG" width=70% height=70%>

Output when not logging in properly and trying a SQL injection attack after prepared statement
was implemented

<img src= "https://user-images.githubusercontent.com/77298953/210662621-f7148e98-d50d-49a8-b665-68582c74cd60.PNG" width=70% height=70%>

Output when the prepared statement was implemented and logged in properly


## Task 4 Explanation
In this task, we utilized the prepared statement mechanism to understand the view

of the boundaries when running a SQL statement. Prepared statements are used to ensure that the

view of the boundaries is consistent in the server-side code and in the database. For this task we

visi[ted](http://www.seed-server.com/defense)[ ](http://www.seed-server.com/defense)[www.seed-server.com/defense](http://www.seed-server.com/defense)[ ](http://www.seed-server.com/defense)which prompted us with a login page. Prior to making any alterations to the
unsafe.php file, 
all of the information relating to the account was displayed when

logging into a specific user’s account and when performing a SQL injection attack. This is an

example of a vulnerability that we needed to fix to prevent SQL Injection attacks from being

successful. In order to achieve this, we edited the unsafe.php file and added prepared statements.

These prepared statements changed the program to prevent SQL Injection attacks from working

due to data not being printed out on the webpage when an attack was launched. If a user logged in

with correct credentials, then the user data would be displayed, fixing the vulnerability.

