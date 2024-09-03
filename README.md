# SQL Injections Lab

## Objective

I designed this lab to demonstrate a grasp of the SQL Injections methodology, including simulating controlled cyberattacks to identify vulnerabilities and assess security posture. By understanding how SQL injections work, the lab allows me to demonstrate the ability to extract credentials from a MySQL database using SQL queries on a supermarket website.

### Skills Learned

- SQL Injections
- Database management
- Database security best pratices

### Tools Used

- SQL
- Browser developer tools
- phpMyAdmin / MariaDB
- Xampp
- Visual Studio Code


## Xampp installation and setup

1.	For the hosting of the vulnerable site it is used Xampp
   
![image](https://github.com/user-attachments/assets/65994bb5-b71a-44be-b3c0-1c526828164c)

*Ref 1: Xampp app running Apache and mySQL server*

2. Database is created using sql. file which already contains default information which we will be using to pull out data.
3. On phpMyAdmin we create an user account called "Admin" where password is 12345 (this is done on purpose) and host is "%" it means it will have access to any host.
4. We grant all privileges to the user
   
![image](https://github.com/user-attachments/assets/6c3ae373-f025-4db8-9119-50d612475e62)

*Ref 2: Admin user has all priviliges to the database*

5. At this point we have an admin user and working database stored in phpMyAdmin.

   

## Website execution

This site is a page of a supermarket, where customers can search for products and find out their price.

1.	Supermarket.php is added to the directory where Xampp is installed, xampp/htdocs/supermercado.php
2.	The file is executed on the local server (localhost)
3.	Once running the website will look as the picture below

![image](https://github.com/user-attachments/assets/3b09110a-a180-4718-b1b9-b29d7cad5f89)

*Ref 3: Supermarket landingpage*


## SQL Injections

Web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. This can allow an attacker to view data that they are not normally able to retrieve. This might include data that belongs to other users, or any other data that the application can access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.



![image](https://github.com/user-attachments/assets/4c3158b0-3b79-4365-a3e8-8f6ca2c55ec8)

*Ref 4: SQL query M%' UNION (SELECT 1,2,3 FROM DUAL);--*

The next picture shows sensitive information from the database for example:

- Table servers: Sensitive information about database servers could be extracted, such as IP address, login credentials (username and password), and operating system. This information would allow an attacker to directly access the server or attempt brute-force attacks to gain unauthorized access.
- Table columns_priv: This table stores details about access privileges to specific columns within tables. An attacker could exploit this information to view sensitive data that they should not have access to or even modify it without authorization.
- Table roles_mapping: By accessing this table, an attacker could obtain information about the roles assigned to users within the database. With this knowledge, they could impersonate a user with higher privileges to perform unauthorized actions.
- Table user: This table contains detailed information about database users, including their usernames, passwords, and permissions. An attacker could use this information to carry out brute-force or dictionary attacks in an attempt to gain unauthorized access to the database.

![image](https://github.com/user-attachments/assets/002ee872-02c3-4509-b641-7f3845b4bfce)

*Ref 5: SQL query M%' UNION (SELECT TABLE_NAME, TABLE_SCHEMA, 3 FROM INFORMATION_SCHEMA.TABLES);--*

Following picture shows sensitive information which we will take advantage from in our next query such as:
- ID
- Name
- Email
- Password
- Role

![image](https://github.com/user-attachments/assets/305e87ce-a3bb-4642-ab98-85997ba12c88)

*Ref 6: SQL query M%' UNION (SELECT COLUMN_NAME, 2, 3 FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME='usuarios');--* 

The next pictures shows how crededials are leaked, we can see some users have a secure password

- TESLA BONILLA SOLIS #FFs2342dcg5hse2
- VERONICA FONSECA RAMIREZ CVCE@dC34$4QCas2@

![image](https://github.com/user-attachments/assets/6cd7af2b-d011-4e20-9f32-6d7efa60740a)

*Ref 7: SQL query M%' UNION (SELECT NOMBRE, EMAIL, CONTRASENA FROM usuarios);--*

## Findings

- Q1 Is the password and username combination used by the database administrator account secure? Are there ways to enhance the security of this account?
  No, the current password and username for the database administrator account are highly vulnerable. Using "admin" as a username is a common practice that makes it an easy target for attackers. Moreover, the chosen password is weak and could be easily cracked using common hacking techniques.
  
- Q2 What is the purpose of the DUAL table in a database?
  It's a testing or sandbox table where functions or actions can be executed without modifying production data and preventing damage to the database. If someone wants to extract data, they need to know if the UNION operator can be used in the text field.
  
- Q3 Is there any user with an administrative role that has a weak password? Who?
  Admin, uses 12345.

- Q4 Using the browser developer tools, is there any anomally or potential security risk?
  It is, we can see public developer notes, admin credetials leak and we also see that the website is not using SSL.
  ![image](https://github.com/user-attachments/assets/dda22efb-d8a7-4e5d-bcb7-ad3938c84412)

- Q5 How can it be improved the security of the website and database to prevent the leakage of information such as credentials?
  Remove the notes from the HTML file that indicate admin credentials. In the PHP code section where a select is performed, there is a vulnerability that allows extracting more data than necessary; instead, prepared statements or parameterized queries should be used instead of directly concatenating user input values into the SQL string. Prepared statements allow 
  separating the data from the SQL query, making it impossible for an attacker to inject malicious SQL code.



## Countermeasures to the attack

- Use strong passwords and enforce password complexity policies.
- Sanitize user input before incorporating it into SQL queries (prepared statements).
- Grant users only the minimum permissions necessary for their tasks (principle of least privilege).
- Remove sensitive information from developer notes or code comments.
- Implement SSL/TLS encryption for secure communication between browser and server.
- Regularly conduct security audits to identify and address vulnerabilities.

## Results

This lab effectively showcased the dangers of SQL injection attacks and the importance of robust security measures to protect websites and databases. By understanding these risks and implementing proper safeguards, organizations can significantly reduce the chances of data breaches and maintain the integrity of their systems.



