<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/Lock-2.png">

# Workshop: SQL Server Security Ground to Cloud

#### <i>A Security Course For Data Professionals</i>

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/textbubble.png"> <h2>02 - SQL Server Security Basics</h2>

In <a href="https://github.com/David-Seis/SecureYourAzureData" target="_blank">this workshop</a> you'll cover the basics of securing SQL Server installations and databases, from on-premises systems to Microsoft Azure deployments.

In each module you'll get more references, which you should follow up on to learn more. Also watch for links within the text - click on each one to explore that topic.

(<a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/00%20-%20Pre-Requisites.md" target="_blank">Make sure you check out the <b>Pre-Requisites</b> page before you start</a>. You'll need all of the items loaded there before you can proceed with the workshop.)

This module builds on the previous module where you learned about securing and auditing the platform. This module focuses on the data professional's responsibilities in regard to the operation of the applications, and introduces the basics of SQL Server security controls, from the login access to the row-level and other security mechanisms they can use. 

You'll cover these topics in this module:
<ul>
  <li><a href="#01" target="_blank">01 - Principals</li></a>
  <li><a href="#02" target="_blank">02 - Securables</li></a>
  <li><a href="#03" target="_blank">03 - Applications</li></a>
  <li><a href="#04" target="_blank">04 - Encryption, Certificates, and Keys</li></a>
  <Li><a href="#04" target="_blank">05 - Auditing</li></a>
</ul>

<p style="border-bottom: 1px solid lightgrey;"></p>


[//]: <> (================================= Section 1 )


<h2 id="01"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.1 Principals</h2>
<br>
Access to SQL is managed through principals, or logins.  Each principal will have a way for the user to 'log in' considering passwordless, which then enables the user of the account to Access SQL Server at the most basic level, and then we get to the authorization side. Authorization is What you can do once you are connected.

Authentication
<br>

SQL has two modes of authentication: 1 SQL Authentication where the account is managed and stored inside of SQL, or 2 managed externally through Windows which includes Active Directory, Azure AD and Linux AD Integration. 


1 SQL Authentication

The Master database stores all security principals except in the case of a contained database. For all principals stored in the master database, the username and permissions are tracked and managed.

Contained Databases are those where the authentication and authroization are stored inside, without ever using the master database.  <TODO: Research and fill out connection and use cases.>

One key difference between these two kinds of authentication is that SQL stores the password for any non-Windows managed principals. Because passwords are stored in SQL, it is up to the DBA to ensure that a password and account policy is created and applied to each user.


[//]: <> (2 Windows Authentication AD, AAD, Linux AD integration)</h4>

It is typically easier and safer to manage accounts via Active Directory. There are some organizations that do not work in a domain, and there are some who use applications that require a certain API to reach SQL. Otherwise, if you have a domain and your app can easily reach SQL, active directory is the safer, and typically easier to manage choice.
The ease comes in reducing the number of accounts needed to access the server wihtout AD integration a user needs one account to log into their computer, and then another to access SQL Server, simplifying account lifecycles and policy enforcment, and the ease of using groups to manage access and permissions.

A login for a windows group can be created, and later when we get into authorization, permissions and roles can be granted to the group rather than the individual, which makes provisioning accounts and access a more streamlined process.

With relatively little work, Azure can be synced with an on-prem Active directory which opens up a lot of possibilities. This is especially true because there is now the option to extend Active directory to Linux servers. Which enables the SQL installations there to make use of Windows authentication!
<br>

<br>

<h3>Authorization</h3>

Once you are logged into a SQL server Authorization is the next contender. If you were to log in to a SQL server where you had been given an account and no server roles, what would you see?


So the next step for each user or group is to look through the prebuilt server and database roles. Roles are a collection of permissions based on general use cases for a SQL Server user and there a general breakdown in the table below:

<TODO: create table of Server roles, database roles.>

If you want to create your own roles, you can dig into the permissions poster to have the exact levers pulled to get the access profile your organization needs. We recommend against changing server roles, but you can copy the permissions of a prebuilt role and modify from there you can get a custom role tailored to your specific needs.

[Link to the SQL Docs article that has the permisisons poster for download](https://docs.microsoft.com/en-us/sql/relational-databases/security/permissions-database-engine?view=sql-server-ver16)

Combining roles with Windows Authentication groups helps efficiently and quickly provision permisisons to large groups of people, and it makes for less work on the DBA. However, be aware that if you ar enot managing who is going into and out of roles it can become a security risk. Regular account, role, and group audits should be done in your organization. 



<TODO Determine the placement and use of the topics below:> SQL Server controlled security accounts Active Directory Integration Azure Active Directory Certificates and other Authentication Methods Roles and Role-Based Access Control Kubernetes as a windows auth process.


<h4>Principals "Worst" Practices</h4>
Having one user account for a group of individuals to use. This can limit the auditing and tracking capabilites of SQL server, so if a malicious actor does something heinous under a shared account, it is less likely you will be able to find out who did it, more about this in <a href="#04" target="_blank">Auditing</a>.</li>
<br>

<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Create and list principals in an instance</b></h4>
<br>
 >Description<

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

1. Create the Test Database we will use for this module:
<pre>
    CREATE DATABASE SQLSecurityTest;
    GO
</pre>

2. Create a set of users on your Windows test environment in an elevated command prompt (linux will be different):
  <pre>
    net user User1 "Tes#20. Use12!" /add
    net user User2 "Tes#20. Use22!" /add
  </pre>

3. Create a set of users in your SQL environment in a new query window, you will need to replace -Placeholder- with your device name. Remember, these are test users and are not designed to be secure.
  <pre>
    USE [master]
    GO
    CREATE LOGIN [-Placeholder-\User1] FROM WINDOWS WITH DEFAULT_DATABASE=[SQLSecurityTest]
    GO

    USE [SQLSecurityTest]
    GO
    CREATE USER [-Placeholder-\User1] FOR LOGIN [-Placeholder-\User1]
    GO

    USE [master]
    GO
    CREATE LOGIN [-Placeholder-\User2] FROM WINDOWS WITH DEFAULT_DATABASE=[SQLSecurityTest]
    GO

    USE [SQLSecurityTest]
    GO
    CREATE USER [-Placeholder-\User2] FOR LOGIN [-Placeholder-\User2]
    GO

    USE [master]
    GO
    CREATE LOGIN [User3] WITH PASSWORD=N'Tes#20. Use32!', DEFAULT_DATABASE=[SQLSecurityTest], CHECK_EXPIRATION=OFF, CHECK_POLICY=OFF
    GO

    USE [SQLSecurityTest]
    GO
    CREATE USER [User3] FOR LOGIN [User3]
    GO

    USE [master]
    GO
    CREATE LOGIN [User4] WITH PASSWORD=N'Tes#20. Use42!', DEFAULT_DATABASE=[SQLSecurityTest], CHECK_EXPIRATION=OFF, CHECK_POLICY=OFF
    GO
    
    USE [SQLSecurityTest]
    GO
    CREATE USER [User4] FOR LOGIN [User4]
    GO
  </pre>

<h2 id="02"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.1.1 Roles</h2>
<br>
<TODO: compelte intro> Server and databases roles are key in managing the everyday security of your SQL server. 

<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Create and assign roles.</b></h4>
<br>
<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

1. Create 1 server role and 2 database roles:
<pre>
    -- Create a server role that will eventually own a table in the database
    USE master;
    CREATE SERVER ROLE SQLSecurityTest_Table_Owner;

    -- Create a database roel that will eventually have rights to execute a stored procedure in the database
    USE [SQLSecurityTest];
    CREATE ROLE Stored_procedure_user_role;

    --Create a database role that will have permisison to query a view in the database
    USE [SQLSecurityTest];
    CREATE ROLE View_user_role;
</pre>

2. Assign users to roles:
<pre>
    --Add user 1 to the Server role
    ALTER SERVER ROLE [SQLSecurityTest_Table_Owner] ADD MEMBER [A1\User1]
    GO


    --Add user 2 and 3 to the stored procedure user role
    USE [SQLSecurityTest]
    GO
    ALTER ROLE [Stored_procedure_user_role] ADD MEMBER [A1\User2]
    GO

    USE [SQLSecurityTest]
    GO
    ALTER ROLE [Stored_procedure_user_role] ADD MEMBER [User3]
    GO


    --Add user 1 and 4 to the View user role
    USE [SQLSecurityTest]
    GO
    ALTER ROLE [View_user_role] ADD MEMBER [A1\User1]
    GO

    USE [SQLSecurityTest]
    GO
    ALTER ROLE [View_user_role] ADD MEMBER [User4]
    GO
</pre>

3. Run this script to identify all user created roles on the system:
<pre>
    --Server Principals
    SELECT * FROM sys.server_principals
    WHERE type = 'R' AND is_fixed_role = 0 AND name NOT IN ('public')

    --Database Principals
    SELECT * FROM sys.database_principals 
    WHERE type = 'R' AND is_fixed_role = 0  AND name NOT IN ('public')
</pre>
4. Run this script to list all users by whether they are SQL or Windows users, excluding any built-in users.
<pre>
    -- Windows Logins
      SELECT * FROM sys.server_principals WHERE type IN ('U','G', 'E', 'X') AND name NOT LIKE '%NT%'

    -- SQL Logins 
      SELECT * FROM sys.server_principals WHERE type = 'S' AND name NOT LIKE '%#%' AND name NOT LIKE 'sa'
</pre>
5.  Open an Administrator Powershell command window and navigate to the test app directory and open the document
  <pre>
    cd \SampleDBApp
    notepad SimpleConnection.py
  </pre>
6. Add the contents below the previous entry:
  <pre>
    # ---------------------------- #
    # Declare connection variables #
    # ---------------------------- #
    string = 'DRIVER={ODBC Driver 17 for SQL Server};Server=(local);DATABASE=SQLSecurityTest;'
    user3 = 'User3'
    pw3 = 'Tes#20. Use32!'
    user4 = 'User4'
    pw4 = 'Tes#20. Use42!'

    # -------------------------------------------------- #
    # Declare the queries you want to test for each user #
    # -------------------------------------------------- #

    query1 = "SELECT 'Connecting user name = ' + suser_name()"
    query2 = "SELECT 'Select Server Name = ' + @@SERVERNAME"

    # ---------------------------------------- #
    # Declare connection strings for each user # 
    # ---------------------------------------- #

    conn1 = pyodbc.connect(string+'Trusted_Connection=yes;')
    conn2 = pyodbc.connect(string+'UID='+ user3 +';PWD=' + pw3)
    conn3 = pyodbc.connect(string+'UID='+ user4 +';PWD=' + pw4)
    cursor1 = conn1.cursor()
    cursor2 = conn2.cursor()
    cursor3 = conn3.cursor()

    # ------------------------------------ #
    # This will run as the powershell user #
    # ------------------------------------ #

    cursor1.execute(query1)
    row = cursor1.fetchone() 
    while row: 
        print(row[0])
        row = cursor1.fetchone()
    cursor1.execute(query2)
    row = cursor1.fetchone() 
    while row: 
        print(row[0])
        row = cursor1.fetchone()

    # ----------------------------- #
    # Execute the Queries for User3 #
    # ----------------------------- #

    cursor2.execute(query1)
    row = cursor2.fetchone() 
    while row: 
        print(row[0])
        row = cursor2.fetchone()
    cursor2.execute(query2)
    row = cursor2.fetchone() 
    while row: 
        print(row[0])
        row = cursor2.fetchone()

    # ----------------------------- #
    # Execute the queries for User4 #
    # ----------------------------- #

    cursor3.execute(query1)
    row = cursor3.fetchone() 
    while row: 
        print(row[0])
        row = cursor3.fetchone()
    cursor3.execute(query2)
    row = cursor3.fetchone() 
    while row: 
        print(row[0])
        row = cursor3.fetchone()

  </pre>
7. Run the python app to see the connection results. 
<pre>
    python SimpleConnection.py
</pre>
> Note, windows users will only be able to connect via a trusted connection. Please run powershell as user1 and user2 to see their connection results.

<p style="border-bottom: 1px solid lightgrey;"></p>  


[//]: <> (================================= Section 2 )


<h2 id="02"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.2 Securables</h2>
<br>
Securing your SQL Server is the goal of this course. Principals can be managed to limit who can access the server, Adding permissions to each user to determine WHAT they can do while they are logged in is key to a secure system. A **Securable** in SQL server is virtually everything. Knowing how to limit access to sensitive data and leveraging least privilege when assigning permissions is of the utmost importance when securing a SQL instance. The topic of securables is usually broken down into three categories, or scopes, for ease of use.


Each securable includes a set of permissions relevant to scope and function. Permissions can be granted, denied, or revoked. Referencing the general permissions hierarchy as well as reviewing the built in Server roles and Database roles is good start to understanding what roles are necessary for most users. Considering least privilege each time you assign a role, and cross referencing permissions granted through each role is necessary. These images can help with that process, as well as the Permissions Poster produced by Microsoft.


Most environments use the out of the box roles. Unfortunately many users have more permissions than they need because it is simpler to manage.
<br>

<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Test access through roles</b></h4>

Knowing **WHO** is in your environment and **WHAT** they can do is an important step in strengthening your security posture.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>



1.  Add this table to the Sample 'SQLSecurityTest' database we created earlier
<pre>
    USE [SQLSecurityTest]
    GO
    CREATE TABLE Patient (
    LoginID tinyint
    ,   LastName varchar(255)
    ,   FirstName varchar(255)
    ,   Address varchar(255)
    ,   City varchar(255)
    ,   SSN nvarchar(11)
    ,   CardNumber nvarchar(19)
    );

    INSERT INTO Patient (loginid, lastname, firstname, address, city, ssn, cardnumber)
    VALUES	
      (1,'Arbiter', 'Agatha', '111 Apple Ave.', 'Atlanta', '111-11-1111', '1111-1111-1111-1111')
      , (2, 'Bob', 'Billy', '222 Bayshore Blvd.', 'Boice', '222-22-2222', '2222-2222-2222-2222')
      , (3, 'Choice', 'Charley', '333 Castaway Ct.', 'Chesterfield', '333-33-3333', '3333-3333-3333-3333')
      , (4, 'Dangerfield', 'David', '4444 Denvue Drive', 'Denver', '444-44-4444', '4444-4444-4444-4444')
      , (5, 'Engleton', 'Edbert', '5555 Esquire Rd. E', 'Easton', '555-55-5555', '5555-5555-5555-5555')
</pre>
2. Create view, and Stores procedure in the sample DB
<pre>
    CREATE VIEW Patient_Mailing_Address AS
    SELECT FirstName, LastName, CONCAT(Address + ', ' + City)
    FROM Patient
</pre>
3. Create a stored procedure in the database:
<pre>
    USE SQLSecurityTest;  
    GO  
    CREATE PROCEDURE uspGetCardInformation   
        @Loginid tinyint  
    AS   

        SET NOCOUNT ON;  
        SELECT Loginid, cardnumber  
        FROM Patient  
        WHERE loginId= @loginId   
    GO  
</pre>
4. Grant User1 full control on the table:
<Pre>
USE [SQLSecurityTest]
GO
GRANT CONTROL ON [dbo].[Patient] TO [A1\User1]
GO
</pre>
5. Grant rights to execute the stored procedure to the 'Stored_procdure_user_role' role.
<Pre>
USE [SQLSecurityTest]
GO
GRANT EXECUTE ON [dbo].[uspGetCardInformation] TO [Stored_procedure_user_role]
GO
</pre>
6. Grant rights to use the view to the 'View_user_role' role.
<Pre>
USE [SQLSecurityTest]
GO
GRANT SELECT ON [dbo].[Patient_Mailing_Address] TO [View_user_role]
GO
</pre>
7. Integrate pyhton APP


<p style="border-bottom: 1px solid lightgrey;"></p>


[//]: <> (================================= Section 3 )


<h2 id="03"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.3 Applications</h2>
<br>

Application security is as important as SQL security. Most applications are set up to have direct access to SQL, and in some cases with sysadmin rights. Secure coding practices as well as managing access and permisisons to the application account are the focus of this section.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Parameterized Queries and Error Handling</b></p>
<br>
<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>
See the effect of a SQL injection string on a non-parameterized query, and then see the effect once it is parameterized. Also see how error handling helps prevent an attacker from learning too much about your environment.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

1. Run this Query for a client to see their account information as a general user
<pre>
    SELECT * 
    FROM Patient 
    WHERE loginid = '1' 
    -- user input = 1
</pre>
2. Run the same query with an injection string
<pre>
    SELECT * 
    FROM Patient
    WHERE loginid = '' or 1=1 --'  
    -- user input = ' or 1=1 --  
</pre>
3.  Run the query below showing how parameterizing the query helps prevent injection strings allowed by an application from negatively affecting SQL server:
<pre>
    DECLARE @Loginid tinyint

    SET @Loginid = '' or 1=1-- '
    --user input = ' or 1=1-- 
    SELECT * 
    FROM Patient
    WHERE loginid = @Loginid --parameterized input

    
    SET @Loginid = 3 
    --user input = 3
    SELECT * 
    FROM Patient
    WHERE loginid = @Loginid --parameterized input

</pre>
4.  Notice that an error could be returned to a malicious actor which they could use to learn mroe about your environment. Adding error handling helps prevent verbose errors from giving more information for an attacker to use.
<pre>
    DECLARE @Loginid tinyint
      BEGIN TRY
        SET @Loginid = '' or 1=1--' 
        --user input = ' or 1=1--
            SELECT * 
            FROM Patient
            WHERE loginid = @Loginid --parameterized input
      END TRY
      BEGIN CATCH
        Print 'Please use only your user ID'
      END CATCH

    DECLARE @Loginid tinyint
      BEGIN TRY
        SET @Loginid = '3'
        --user input = 3
            SELECT * 
            FROM Patient
            WHERE loginid = @Loginid --parameterized input
      END TRY
      BEGIN CATCH
        Print 'Please use only your user ID'
      END CATCH

</pre>


<p style="border-bottom: 1px solid lightgrey;"></p>


[//]: <> (================================= Section 4 )



<h2 id="04"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.4 Encryption, Certificates, and Keys</h2>
<br>

Encryption, certificates, and keys are tools for securing the physical layer of SQL server. <todo: add more words>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Enabling TDE on a database</b></p>


<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

Setting up Transparent Data Encryption is a positive tool for the physical security  of data. This activity will touch the setting up of keys, certificates, and finally the encryption of a database. 
<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>


1. Create & Backup master key (first create the C:\EncryptedDrive folder, you will also need to grant permissions to SQL)
<pre>
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Fl@sh G0rd0n!';
    GO

    OPEN MASTER KEY DECRYPTION BY PASSWORD = 'Fl@sh G0rd0n!';
    GO
    BACKUP MASTER KEY TO FILE = 'C:\EncryptedDrive\masterkey.mk' 
        ENCRYPTION BY PASSWORD = 'S@vior Of.The Un1v3r$3!';
    GO
</pre>
2.  Create, Verify, and Backup a certificate - you will need to adjust the backup location of the certificate for your system.
<pre>
    CREATE CERTIFICATE TDE_Cert WITH SUBJECT = 'TDE Certificate';
    GO

    BACKUP CERTIFICATE TDE_Cert TO FILE = 'C:\EncryptedDrive\TDE_Cert.cer'
        WITH PRIVATE KEY (
            FILE = 'C:\EncryptedDrive\TDE_Cert.pvk',
            ENCRYPTION BY PASSWORD = 'I$ @.Mirac1e!');
    GO
</pre>
3. Verify the creation/presence of the TDE_Cert and the Database Master key.
<pre>
    SELECT * FROM sys.certificates where [name] = 'TDE_Cert'
    GO

    Select name, algorithm_desc, create_date from sys.symmetric_keys
</pre>

4.  Encrypt our test Database
<pre>
    USE SQLSecurityTest
    GO

    /*Create Encryption key*/
    CREATE DATABASE ENCRYPTION KEY
      WITH ALGORITHM = AES_256
      ENCRYPTION BY SERVER CERTIFICATE TDE_Cert;
    GO

    /* Encrypt database */
    ALTER DATABASE SQLSecurityTest SET ENCRYPTION ON;
    GO

    /* Verify Encryption */
    SELECT 
      DB_NAME(database_id) AS DatabaseName
    , Encryption_State AS EncryptionState
    , key_algorithm AS Algorithm
    , key_length AS KeyLength
    FROM sys.dm_database_encryption_keys
    GO

    SELECT 
    NAME AS DatabaseName
    , IS_ENCRYPTED AS IsEncrypted 
    FROM sys.databases where name ='SQLSecurityTest'
    GO
</pre>



<p style="border-bottom: 1px solid lightgrey;"></p>


[//]: <> (================================= Section 5 )



<h2 id="05"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.5 Auditing</h2>
<br>

TODO: Topic Descriptions

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Create a Server Audit</b></p>

In this Activity you will set up a server and database audit on your test system. You will then perform an action that would be included in the audit scope, and then you will read the audit log afterwards.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

1. Step 1: Create a SQL Server Audit for the Patient table data
<pre>
    USE [master]
    GO

    CREATE SERVER AUDIT [Patient_Data_Audit]
    TO FILE 
    (	FILEPATH = N'C:\EncryptedDrive'
      ,MAXSIZE = 1024 MB
      ,MAX_ROLLOVER_FILES = 10
      ,RESERVE_DISK_SPACE = OFF
    ) WITH (QUEUE_DELAY = 1000, ON_FAILURE = CONTINUE)

    GO

    ALTER SERVER AUDIT [Patient_Data_Audit]  
    WITH (STATE = ON) ;  
    GO  
</pre>

2. Add an Audit Specification that catches actions on the table
<pre>
    USE [SQLSecurityTest]
    CREATE DATABASE AUDIT SPECIFICATION Audit_Data_Select_On_Patient_Table
    FOR SERVER AUDIT [Patient_Data_Audit]
    ADD ( SELECT, INSERT, UPDATE, DELETE  
        ON SQLSecurityTest.dbo.Patient BY Public )  
    WITH (STATE = ON) ;    
    GO  
</pre>

3. Perform actions that are included in the audit actions: (python examples in the notebook)
<pre>
    USE [SQLSecurityTest]
    INSERT INTO Patient (LoginID,FirstName,LastName,Address,City,SSN,CardNumber)
    VALUES ('6','Fred','Fernandez', '66 Freedom Fwy.', 'Fremont', '666-66-6666', '6666-6666-6666-6666')
    GO

    USE [SQLSecurityTest]
    SELECT * FROM Patient
    WHERE LoginID > 3
    GO

    USE [SQLSecurityTest]
    UPDATE Patient
    SET City = 'Fresno' 
    WHERE City = 'Fremont'
    GO

    USE [SQLSecurityTest]
    SELECT * FROM Patient
    WHERE LoginID > 3
    GO

    USE [SQLSecurityTest]
    DELETE FROM Patient
    WHERE FirstName = 'Fred'
    GO

    USE [SQLSecurityTest]
    SELECT * FROM Patient
    GO
</pre>
4. Read the aduit log events:
<pre>
    SELECT event_time, server_instance_name, server_principal_name, database_name, object_name, [statement] FROM sys.fn_get_audit_file ('C:\EncryptedDrive\Pa*',default,default);  
    GO  
</pre>


<p style="border-bottom: 1px solid lightgrey;"></p>


[//]: <> (================================= Closing )
)
<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/owl.png"><b>For Further Study</b></p>
<ul>
    <TODO: add sql permission poster and builtin roles links>
    <li><a href="https://github.com/sqlstudent144/SQL-Server-Scripts/blob/master/sp_SrvPermissions.sql" target="_blank">An example stored procedure to fetch Server permissions</a></li>
    <li><a href="https://github.com/sqlstudent144/SQL-Server-Scripts/blob/master/sp_DBPermissions.sql" target="_blank">An example stored procedure to fetch Database permissions</a></li>
</ul
>
<br>
<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/geopin.png"><b >Next Steps</b></p>

Next, Continue to <a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/03%20-%20SQLAzureSecurity.md" target="_blank"><i> 03 - Azure SQL DB Security</i></a>.
