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


<h2 id="01"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">1.0 Principals</h2>
<br>

Access to SQL is managed through principals, or logins.  Each principal will have a way for the user to 'log in' (considering passwordless), which then enables the user of the account to Access SQL Server at the most basic level, and then we get to the authorization side. Authorization is What you can do once you are connected.

<h3>Authentication</h3>
<br>

SQL has two modes of authentication: 1) SQL Authentication where the account is managed and stored inside of SQL, or 2) managed externally through Windows which includes Active Directory, Azure AD and Linux AD Integration. 


<h4>1) SQL Authentication</h4>
The Master database stores all security principals except in the case of a contained database. For all principals stored in the master database, the username and permissions are tracked and managed. 

Contained Databases are those where the authentication and authroization are stored inside, without ever using the master database.  <TODO: Research and fill out connection and use cases.>

One key difference between these two kinds of authentication is that SQL stores the password for any non-Windows managed principals. Because passwords are stored in SQL, it is up to the DBA to ensure that a password and account policy is created and applied to each user.


<h4>2) Windows Authentication (AD, AAD, Linux AD integration)</h4>

It is typically easier and safer to manage accounts via Active Directory. There are some organizations that do not work in a domain, and there are some who use applications that require a certain API to reach SQL. Otherwise, if you have a domain and your app can easily reach SQL, active directory is the safer, and typically easier to manage choice. The ease comes in reducing the number of accounts needed to access the server (wihtout AD integration a user needs one account to log into their computer, and then another to access SQL Server), simplifying account lifecycles and policy enforcment, and the ease of using groups to manage access and permissions.

A login for a windows group can be created, and later when we get into authorization, permissions and roles can be granted to the group rather than the individual, which makes provisioning accounts and access a more streamlined process.

With relatively little work, Azure can be synced with an on-prem Active directory which opens up a lot of possibilities. This is especially true because there is now the option to extend Active directory to Linux servers. Which enables the SQL installations there to make use of Windows authentication!
<br>

<h5>Less common forms of Authentication</h5>
<ul>
  <li>Application roles</li> <TODO: research and explain this>
  <li>Users mapped to certificates</li>  <TODO: research and explain this>
  <li>Users Mapped to Asymmetric keys</li> <TODO: Research and explain this>

</ul>

<br>

<h3>Authorization</h3>

Once you are logged into a SQL server Authorization is the next contender. If you were to log in to a SQL server where you had been given an account and no server roles, what would you see?

<TODO: description of what they would see>

So the next step for each user or group is to look through the prebuilt server and database roles. Roles are a collection of permissions based on general use cases for a SQL Server user and there a general breakdown in the table below:

<TODO: create table of Server roles, database roles.>

If you want to create your own roles, you can dig into the permissions poster to have the exact levers pulled to get the access profile your organization needs. We recommend against changing server roles, but you can copy the permissions of a prebuilt role and modify from there you can get a custom role tailored to your specific needs.

[Link to the SQL Docs article that has the permisisons poster for download](https://docs.microsoft.com/en-us/sql/relational-databases/security/permissions-database-engine?view=sql-server-ver16)

Combining roles with Windows Authentication groups helps efficiently and quickly provision permisisons to large groups of people, and it makes for less work on the DBA. However, be aware that if you ar enot managing who is going into and out of roles it can become a security risk. Regular account, role, and group audits should be done in your organization. 



<TODO Determine the placement and use of the topics below:>
  SQL Server controlled security accounts
  Active Directory Integration 
  Azure Active Directory
  Certificates and other Authentication Methods
  Roles and Role-Based Access Control
  Kubernetes as a windows auth process.


<h4>Principals "Worst" Practices</h4>
<ol>
  <li>Having one user account for a group of individuals to use. This can limit the auditing and tracking capabilites of SQL server, so if a malicious actor does something heinous under a shared account, it is less likely you will be able to find out who did it, more about this in <a href="#04" target="_blank">Auditing</a>.</li>

</ol>

<br>

<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: List Principals in an Instance</b></h4>
<br>

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

1. Create the Test Database we will use for this module:
<pre>
    CREATE DATABASE SQLSecurityTest;
    GO
</pre>

2. Create a set of users on your Windows test environment in an elevated command prompt (linux will be different):
  <pre>
    net user User1 "Test20. User1!22" /add
    net user User2 "Test20. User2!22" /add
  </pre>

3. Create a set of users in your SQL environment in a new query window, you will need to replace -Placeholder- with your device name.
  <pre>
    USE [master]
    GO
    CREATE LOGIN [-Placeholder-\User1] FROM WINDOWS WITH DEFAULT_DATABASE=[SQLSecurityTest]
    GO
    use [master];
    GO
    USE [SQLSecurityTest]
    GO
    CREATE USER [-Placeholder-\User1] FOR LOGIN [-Placeholder-\User1]
    GO
    USE [master]
    GO
    CREATE LOGIN [-Placeholder-\User2] FROM WINDOWS WITH DEFAULT_DATABASE=[SQLSecurityTest]
    GO
    use [master];
    GO
    USE [SQLSecurityTest]
    GO
    CREATE USER [-Placeholder-\User2] FOR LOGIN [-Placeholder-\User2]
    GO
    USE [master]
    GO
    CREATE LOGIN [User3] WITH PASSWORD=N'Test20. User3!22', DEFAULT_DATABASE=[SQLSecurityTest], CHECK_EXPIRATION=OFF, CHECK_POLICY=OFF
    GO
    use [master];
    GO
    USE [SQLSecurityTest]
    GO
    CREATE USER [User3] FOR LOGIN [User3]
    GO
    USE [master]
    GO
    CREATE LOGIN [User4] WITH PASSWORD=N'Test20. User4!22', DEFAULT_DATABASE=[SQLSecurityTest], CHECK_EXPIRATION=OFF, CHECK_POLICY=OFF
    GO
    use [master];
    GO
    USE [SQLSecurityTest]
    GO
    CREATE USER [User4] FOR LOGIN [User4]
    GO
  </pre>

TODO: add section 1.1 for roles:


1.1 activity:
1. TODO: Create 1 Server roel and 2 DB roles
2. TODO: assign users to roles:
3. Run this script to identify all user created roles on the system:
<pre>
		SELECT * FROM sys.server_principals WHERE type = 'R' AND is_fixed_role = 0 AND name NOT IN ('public')
		SELECT * FROM sys.database_principals WHERE type = 'R' AND is_fixed_role = 0  AND name NOT IN ('public')
</pre>
4. run this script to list all users by whetehr they are SQL or Windows users, excluding any build-in users.
<pre>
    -- Windows Logins
      SELECT * FROM sys.server_principals where type in ('U','G', 'E', 'X') and name not like '%NT%'

    -- SQL Logins 
      SELECT * FROM sys.server_principals where type = 'S' and name not like '%#%' and name not like 'sa'
</pre>

TODO: learn python app use and integrate it into activity

<p style="border-bottom: 1px solid lightgrey;"></p>  


[//]: <> (================================= Section 2 )


<h2 id="02"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.0 Securables</h2>
<br>

Securing your SQL Server is the goal of this course. Principals can be managed to limit who can access the server, Adding permissions to each user to determine WHAT they can do while they are logged in is key to a secure system. A **Securable** in SQL server is virtually everything. Knowing how to limit access to sensitive data and leveraging least privilege when assigning permissions is of the utmost importance when securing a SQL instance. The topic of securables is usually broken down into three categories, or scopes, for ease of use.

  
Each securable includes a set of permissions relevant to scope and function. Permissions can be granted, denied, or revoked. Referencing the general [permissions hierarchy](https://docs.microsoft.com/en-us/sql/relational-databases/security/permissions-database-engine?view=sql-server-ver16) as well as reviewing the built in [Server roles](https://docs.microsoft.com/en-us/sql/relational-databases/security/permissions-database-engine?view=sql-server-ver16) and [Database roles](https://docs.microsoft.com/en-us/sql/relational-databases/security/permissions-database-engine?view=sql-server-ver16) is good start to understanding what roles are necessary for most users. Considering least privilege each time you assign a role, and cross referencing permissions granted through each role is necessary. These images can help with that process, as well as the [Permissions Poster produced by Microsoft](https://aka.ms/sql-permissions-poster). 


Most environments use the out of the box roles. Unfortunately many users have more permissions than they need because it is simpler to manage.
<br>

<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Query the Roles, Permissions, and Principals in your Test Environment</b></h4>

Knowing **WHO** is in your environment and **WHAT** they can do is an important step in strengthening your security posture.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>


1. TODO: create table, view, and Store procedure in the sample DB
2. TOD0: gran user1 all rights to table
3. TODO: Grant 1st DB role rights to the SP
4. TODO: grant 2nd db roel rights to view
5. TODO: Learn to integrate the python app
    5.1 use the python app individual user conenction strings to select fromt he table, view, and execute Sp



<p style="border-bottom: 1px solid lightgrey;"></p>


[//]: <> (================================= Section 3 )


<h2 id="03"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">3.0 Applications</h2>
<br>

Application security is as important as SQL security. Most applications are set up to have direct access to SQL, and in some cases with sysadmin rights. Secure coding practices as well as managing access and permisisons to the application account are the focus of this section.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Parameterized Queries and Error Handling</b></p>
<br>
<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>
See the effect of a SQL injection string on a non-parameterized query, and then see the effect once it is parameterized. Also see how error handling helps prevent an attacker from learning too much about your environment.

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

2. Run this Query for a client to see their account information as a general user
<pre>
    SELECT * 
    FROM Patient 
    WHERE loginid = '1' -- user input = '1'
</pre>
3. Run the same query with an injection string
<pre>
    SELECT * 
    FROM Patient
    WHERE loginid = '' or 1=1 --'  -- user input = ' or 1=1 --
</pre>
4.  Parameterize the query and run it again
<pre>
    DECLARE @Loginid tinyint
    SET @Loginid = '' or 1=1-- --user input

    SELECT * 
    FROM Patient
    WHERE loginid = @Loginid --parameterized input

    
    SET @Loginid = ‘3’ -- --user input
    SELECT * 
    FROM Patient
    WHERE loginid = @Loginid --parameterized input

</pre>
5.  Notice the error information, add handling
<pre>
    declare @Loginid tinyint
      BEGIN TRY
        SET @Loginid = ''' or 1=1--' --user input
      END TRY
      BEGIN CATCH
        Print 'Please use only your user ID'
      END CATCH

    SELECT * 
    FROM Patient
    WHERE loginid = @Loginid --parameterized input
</pre>
6. TODO see if python app can be used here.

<p style="border-bottom: 1px solid lightgrey;"></p>




[//]: <> (================================= Section 4 )



<h2 id="04"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">4.0 Encryption, Certificates, and Keys</h2>
<br>

Encryption, certificates, and keys are tools for securing the physical layer of SQL server. <todo: add more words>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Enabling TDE on a database</b></p>


<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

Setting up Transparent Data Encryption is a positive tool for the physical security  of data. This activity will touch the setting up of keys, certificates, and finally the encryption of a database. 
<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

1. Create & Backup master key
<pre>
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Fl@sh G0rd0n!';
    GO

    OPEN MASTER KEY DECRYPTION BY PASSWORD = 'Fl@sh G0rd0n!';
    GO
    BACKUP MASTER KEY TO FILE = 'C:\EncryptedDrive\masterkey.mk' 
        ENCRYPTION BY PASSWORD = 'Fl@sh G0rd0n!';
    GO

</pre>
2.  Create, Verify, and Backup a certificate
<pre>
    CREATE CERTIFICATE TDE_Cert WITH SUBJECT = 'TDE Certificate';
    GO

    SELECT * FROM sys.certificates where [name] = 'TDE_Cert'
    GO

    BACKUP CERTIFICATE TDE_Cert TO FILE = 'C:\EncryptedDrive\rev.cer'
      WITH PRIVATE KEY (
            FILE = 'C:\EncryptedDrive\TDE.pvk',
            ENCRYPTION BY PASSWORD = 'Fl@sh G0rd0n!');
GO
</pre>

3.  Encrypt our test Database
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



<h2 id="05"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">5.0 Auditing</h2>
<br>

TODO: Topic Descriptions

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Create a Server Audit</b></p>

In this Activity you will set up a server and database audit on your test system. You will then perform an action that would be included in the audit scope, and then you will read the audit log afterwards.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

1. Connect to your test instance, and ensure the test database from previous activities is present.
2. [Open this resource](https://docs.microsoft.com/en-us/sql/relational-databases/security/auditing/create-a-server-audit-and-database-audit-specification?view=sql-server-ver16), and complete a Server and database audit using the graphical user interface as well as with T-SQL. rememebr to target the database audit at the test database from previous activities.
3. Perform actions that are included in the audit (select or insert)
4. Read the aduit log events, using [this resource](https://docs.microsoft.com/en-us/sql/relational-databases/security/auditing/view-a-sql-server-audit-log?view=sql-server-ver16) to find them if needed.

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
