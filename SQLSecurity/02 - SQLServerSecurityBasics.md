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
<TODO: compelte intro> Server and databases roels are key in managing the everyday security of your SQL server. 

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
3. Create a stored procedure
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
2. TOD0: gran user1 all rights to table
<Pre>
</pre>
3. TODO: Grant 1st DB role rights to the SP
<Pre>
</pre>
4. TODO: grant 2nd db roel rights to view
<Pre>
</pre>
5. TODO: Learn to integrate the python app
    5.1 use the python app individual user connection strings to select fromt he table, view, and execute Sp



<p style="border-bottom: 1px solid lightgrey;"></p>


[//]: <> (================================= Section 3 )


<h2 id="03"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.3 Applications</h2>
<br>

[//]: <> (Application security is as important as SQL security. Most applications are set up to have direct access to SQL, and in some cases with sysadmin rights. Secure coding practices as well as managing access and permisisons to the application account are the focus of this section.)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Parameterized Queries and Error Handling</b></p>
<br>
<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>
See the effect of a SQL injection string on a non-parameterized query, and then see the effect once it is parameterized. Also see how error handling helps prevent an attacker from learning too much about your environment.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

1. Make sure the testign database and tabel are present from previous activities in this module.

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
    DECLARE @Loginid tinyint
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



<h2 id="04"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.4 Encryption, Certificates, and Keys</h2>
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
2.  Create, Verify, and Backup a certificate - you will need to adjust the backup location of the certificate for your system.
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



<h2 id="05"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.5 Auditing</h2>
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
