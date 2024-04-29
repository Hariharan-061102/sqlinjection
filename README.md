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

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/cda15716-36d0-4b37-b8ca-6c7b7f746a84)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/98b38216-7c59-48b6-abc0-0120dc61772a)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/2b427d8b-7595-4f20-be62-c745cbcaaa2d)

Click on the menu Login/Register and register for an account

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/8619c5bf-7641-43c6-80c1-02ff24267da6)

Click on the link “Please register here”

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/568007b6-c3b6-4821-9d08-21aa4ba04c00)

Click on “Create Account” to display the following page:

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/e4f76553-0c98-4f2e-9e2a-a220b5fd1153)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/2693343f-41a9-41cd-b70c-777c30a02d28)

Click “Login”. The logged in page will show as below:

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/ecf71295-c98f-4084-afc0-2bcd2f2eb786)

### Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/627a2194-95e5-4a60-91fb-be2e6b0e0349)

Click the login button and you will see it enter into the administrator page.

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/c980ddba-cdbb-44b8-8ae8-2049d002a25b)

### Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/175abda1-8254-4dd8-b5ce-b4e7751b397c)



![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/35eef46e-a7cb-4b5f-ac78-6c0ea9f6edc2)


![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/340a5ee0-582e-4b31-948c-a63bcb7c6966)


![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/4e9e5dcc-0011-4b7b-8814-52e53f085d3c)


![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/d2f6ce52-b693-4a7e-9120-6cf21ffddcad)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/8aef0785-1afa-4cfb-b073-7f63b849c67a)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/7a4b4814-7ecc-474c-ab56-2fffdbe072a9)

After adding the order by 6 into the existing url , the following error statement will be obtained:

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/0ad64777-c55d-4dd9-89f7-6c9d5a148918)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/444e9086-7235-45e4-a8e1-cf0a785046a1)

As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/ec7e6ce6-6b69-4c20-b3f6-655c569ddcdd)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/3b9277b9-aa71-41a2-bf44-707bcbf22050)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/eec5ee95-e872-4476-9346-acd549030317)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/2c8b7995-2e30-47f0-afdd-4ed43108de36)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/1ce7a7a7-4ef6-4639-8ce2-34699d452208)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/be331700-fa2d-4451-97e9-32223288c578)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/face1010-756e-4322-b114-3da7a7112ef6)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/4d1e4a58-963a-4e38-a4fe-748ac42ceb50)

### Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Hariharan-061102/sqlinjection/assets/93427270/df042fb4-5637-436f-8208-326230624498)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).


## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
