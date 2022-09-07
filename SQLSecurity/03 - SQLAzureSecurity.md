<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/Lock-2.png">

# Workshop: SQL Server Security Ground to Cloud

#### <i>A Security Course For Data Professionals</i>

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/textbubble.png"> <h2>03 - Azure SQL Security</h2>

In <a href="https://github.com/David-Seis/SecureYourAzureData" target="_blank">this workshop</a> you'll cover the basics of securing SQL Server installations and databases, from on-premises systems to Microsoft Azure deployments.

In each module you'll get more references, which you should follow up on to learn more. Also watch for links within the text - click on each one to explore that topic.

(<a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/00%20-%20Pre-Requisites.md" target="_blank">Make sure you check out the <b>Pre-Requisites</b> page before you start</a>. You'll need all of the items loaded there before you can proceed with the workshop.)

This module builds on the <a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/02%20-%20SQLServerSecurityBasics.md" target="_blank">previous module where you learned about the basics of SQL Server Security</a>. This module focuses on the the differences between those security aspects and security in Microsoft Azure SQL DB. 

You'll cover these topics in this module:
<ul>
  <li><a href="#01" target="_blank">01 - Accessing Azure SQL DB</li></a>
  <li><a href="#02" target="_blank">02 - Principals</li></a>
  <li><a href="#03" target="_blank">03 - Securables</li></a>
  <li><a href="#04" target="_blank">04 - Applications</li></a>
  <li><a href="#05" target="_blank">05 - Encryption, Certificates, and Keys</li></a>
  <Li><a href="#06" target="_blank">06 - Auditing</li></a>
</ul>

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 1 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="01"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">3.1 Accessing Azure SQL DB</h2>
In SQL Server installations, you are able to control access to the network addresses and ports using any networking controls and configuration you like - or you can leave the default settings and your system is available on the entire network. In Microsoft Azure SQL DB however, two mechanisms are enforced at all times: <i>Encrypted Connections</i>, and <i>Firewalls</i>. 

<h3> Azure SQL DB Encrypted Connections </h3>
SQL Server installations allow for Encrypted Connections to the Instance. In Azure SQL DB, Encrypted Connections are always enforced, using <a href="https://support.microsoft.com/en-us/topic/kb3135244-tls-1-2-support-for-microsoft-sql-server-e4472ef8-90a9-13c1-e4d8-44aad198cdbe" target="_blank">Transport Layer Security</a> (SSL/TLS v1.2). The Azure SQL DB service will listen for TLS requests, so your applications (including management tools like SQL Server Management Studio or Azure Data Studio) are required to connect with <i>Encryption</i> set. It's also a best practice to <b>not</b> trust the Server Certificate, so that the client verifies the Certificate for TLS at all times.

<h3> Microsoft Azure Firewalls</h3>
Before a client or application can connect to Azure SQL DB to begin the Authentication process, the Azure Firewall must have a rule allowing that address to connect, either to the Server or each Azure SQL DB Database. This can be done in the Microsoft Azure Portal or using AZ commands, or for Azure SQL DB database scopes, in Transact-SQL. <a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/firewall-configure?view=azuresql" target="_blank">More details on that process is here</a>.

<br>
<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Create Firewall Rules to allow connections to Azure SQL DB</b></h4>
<br>

In this Activity you will set the firewall rules to allow connections from your test system to an Azure SQL DB Database. 

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>
<ol type="1">
  <li> Determine the IP Address on your test system, and record it.</li>
  <li> <a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/secure-database-tutorial?view=azuresql" target="_blank">Open this resource</a>, and complete the sections from <a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/secure-database-tutorial?view=azuresql#prerequisites" target="_blank">Prerequisites</a> to <a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/secure-database-tutorial?view=azuresql#create-firewall-rules" target="_blank">Setup Database Firewall Rules</a>.</li>
  <li> Connect to that database with SQL Server Management Studio using an encrypted connection.</li>
</ol>

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 2 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="02"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">3.2 Principals</h2>
You have two primary mechanisms for Principals in Azure SQL DB: <i>SQL Server logins</i>, and <i>Azure Active Directory</i> logins. 
 
<h3>Authentication</h3>
Similar to the mechanism in SQL Server, <i>authentication</i> only pertains to whether or not you can log in to the server and database. <i>Authorization</i>, which is covered later, defines the rights and privileges for databases and database objects once the authentication of a Principal is determined. 

<h4>SQL Authentication</h4>
In the case of SQL Authentication, the <b>master</b> system database (accessed by querying <i>sys.syslogins</i>) stores information for the Instance, setting a value for the Principal. This is linked to a correspoding database <i>sysusers</i> system table entry in each database that the Principal will access. This is a similar arrangement to a SQL Server installation, with the exception that in Azure SQL DB, you are using a logical "Server" rather than an installed Instance.

Using SQL Authentication means that Azure SQL DB stores the password for the Principals. Because passwords are stored in the <b>master</b> database, it is up to the database owner to ensure that a password and account policy is applied to each user.

<h4>Microsoft Azure Active Directory</h4>
As described in the last module, Microsoft Active Directory (AD) is a suite of services, and Active Directory Domain Services (AD DS) is the core Active Directory service used to manage users and resources. <a href="https://docs.microsoft.com/en-us/azure/active-directory/?culture=en-us&country=US" target="_blank">Microsoft Azure Active Directory (AAD) is a Domain Service run in the cloud</a>, and works differently than the on-premises installation of Active Directory. AAD is a more secure way to access resources, and allows a higher level of security with Multi-Factor Authentication (MFA), <a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/authentication-mfa-ssms-overview?view=azuresql" target="_blank">as described in this reference.</a>

You can also connect your local Active Directory to Microsoft Azure Active Directory, allowing administration of your local domain to access resources in the cloud, in a single-sign-on approach. <a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/authentication-aad-configure?view=azuresql&tabs=azure-powershell" target="_blank">You can learn how to integrate Microsoft Azure Active Directory into Azure SQL DB using this resource</a>.

Microsoft Azure Purview is a Data Governance Tool that allows you to catalog all of your data assets regardless of where they reside - on premises, in Azure, or even in other Cloud providers. It has a feature for certain Azure SQL services that allows you to also configure <a href="https://techcommunity.microsoft.com/t5/azure/a-beginner-s-guide-to-role-based-access-control-on-azure/m-p/1354947" target="_blank">Role Based Access Control</a> to database objects. You can <a href="https://cloudblogs.microsoft.com/sqlserver/2022/08/11/microsoft-purview-access-policies-for-sql-server-2022/" target="_blank">learn more about Microsoft Azure Purview here</a>.

<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Create and List Users</b></h4>
<br>
For this Activity, you will run the same scripts as in the previous module, with the exception of not creating the Windows local users. For this course, you will focus on using SQL Authentication, and you will be pointed to resources and demonstrations for using Azure Active Directory. 

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

1. Run the following code on your test SQL Azure DB conencted ot the master database:

<pre>
    CREATE LOGIN [User1] 
    WITH PASSWORD=N'Tes#20. Use12!'
    GO
    CREATE LOGIN [User2] 
    WITH PASSWORD=N'Tes#20. Use22!'
    GO
</pre>

2. Run the following commands while connected to the test database:

<pre>
    CREATE USER [User1] 
    FROM LOGIN [User1]
    GO
    CREATE USER [User2] 
    FROM LOGIN [User2]
    GO
</pre>

<h3>Roles in Microsoft Azure SQL DB</h3>
Similar to SQL Server installations, you can group Principals into <i>Roles</i> in Azure SQL DB. In the case of Azure SQL DB, you do not have direct access to an <i>Instance</i> of SQL Server, but instead rely on a logical construct called a <i>Server</i>. There are several built-in Server Roles, which allow the most common set of permissions. It is a best practice to always put Principals (users) into Roles, and apply permissions at the Role level. This prevents "orphaned" users, and allows easier tracking for the permissions spread. 

You can see the included <a href= "https://docs.microsoft.com/en-us/azure/azure-sql/database/security-server-roles?view=azuresql#fixed-server-level-roles" target="_blank">Server Roles and their permissions at this reference</a>. 

Similar to SQL Server Installations, Azure SQL DB also has Database-level Roles, and you can add more if you want to create custom permissions. You can see the included <a href= "https://docs.microsoft.com/en-us/azure/azure-sql/database/security-server-roles?view=azuresql#fixed-server-level-roles" target="_blank">Database Roles and their permissions, along with the instructions to create more Database Roles at this reference</a>.

Also similar to SQL Server Installations, Azure SQL DB has Application Roles. Application Roles remove the need for user permissions. Using Application Roles, the user runs a client application, which connects to the database as that user. The application switches contexts to the Application Role, runs the commands on behalf of the user, and returns the result.

You can learn more about <a href= "https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/application-roles?view=sql-server-ver16" target="_blank"> Application Roles and how to use them at this reference</a>. 

<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Working with Server and Database Roles</b></h4>
<br>

In this Activity you will Create Database Roles, add Users to those Roles, and then list Roles and Members.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

1. Run the following code on your test SQL Azure Database: 

<pre>
    CREATE ROLE Elevated_permissions;
</pre>

2. Add User1 to the new role

<pre>
    ALTER ROLE [Elevated_permissions] ADD MEMBER [User1]
    GO
</pre>

3. Query all users and role memberships:

<pre>
    --Server Principals
    --
    SELECT Name, type_desc, create_date FROM sys.database_principals
    WHERE type = 'S' AND is_fixed_role = 0 AND authentication_type= 1 AND name NOT IN ('dbo')
    --
    --Database Principals
    --
    SELECT Name, type_desc, create_date FROM sys.database_principals 
    WHERE type = 'R' AND is_fixed_role = 0  AND name NOT IN ('public')
    --
    --Find Role Memberships 
    --
    SELECT DP1.name AS DatabaseRoleName,   
      isnull (DP2.name, 'No members') AS DatabaseUserName   
    FROM sys.database_role_members AS DRM  
    RIGHT OUTER JOIN sys.database_principals AS DP1  
      ON DRM.role_principal_id = DP1.principal_id  
    LEFT OUTER JOIN sys.database_principals AS DP2  
      ON DRM.member_principal_id = DP2.principal_id  
    WHERE DP1.type = 'R'
    ORDER BY DP1.name; 
</pre>


<p style="border-bottom: 1px solid lightgrey;"></p>  

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 3 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="03"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">3.3 Securables</h2>
Just as in a SQL Server Instance, <b>Securables</b> in Azure SQL DB are the objects the database contains. A special container called a <i>Schema</i> allows for a gouping of Securables into a single group. Each Schema is owned by one or more Principals, and using a Schema makes for a simplified security management process. You can <a href="https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/ownership-and-user-schema-separation?view=sql-server-ver16" target="_blank">learn more about Schemas here</a>.

Securables fall into three categories, or <i>scopes</i>, for ease of use, the same as a SQL Server installation:

<h4>Server Scopes</h4>
<ul>
  <li>Availability Groups</li> 
  <li>Endpoints</li>
  <li>Logins</li>
  <li>Server Roles</li>
  <li>Databases</li>
</ul>

<h4>Database Scopes</h4>
<ul>
  <li>Application Roles</li> 
  <li>Assemblyies</li>
  <li>Asymmetric Keys</li>
  <li>Certificates</li>
  <li>Contracts</li>
  <li>Fulltext Catalogs</li> 
  <li>Fulltext stoplists</li>
  <li>Message types</li>
  <li>Remote Service Bindings</li>
  <li>Database Roles</li>
  <li>Routes</li> 
  <li>Search Property Lists</li>
  <li>Services</li>
  <li>Symmetric Keys</li>
  <li>Users</li>
</ul>

<h4> Schema Scopes</h4>
<ul>
  <li>Types</li> 
  <li>XML schema Collections</li>
  <li>Objects</li>
    <ul>
      <li>Aggregates</li>
      <li>Functions</li> 
      <li>Procedures</li>
      <li>Queues</li>
      <li>Synonyms</li>
      <li>Tables</li>
      <li>Views</li> 
      <li>External Tables</li>
    </ul>
</ul>
  
Each securable includes a set of <i>Permissions</i> relevant to its scope and function. Some of these permissions include <i>Rights</i>, such as the ability for one Principal to allow access to an object the first owns. Permissions are "most permissive" - meaning that if a user has three permissions in one Role they are a member of and two other permissions from another Role they are a member of, they will have a total of five permissions. 

The primary commands for object access (Data Control Language, or DCL), just as in a SQL Server installation, are:

<ul>
  <li><b>GRANT</b> - Allows a Principal to perform an action on the object, such as SELECT or DELETE.</li>  
  <li><b>REVOKE</b> - Removes the permission on an object for a Principal, but if the Principal is a member of another Role with access, does not remove that access.</li>  
  <li><b>DENY</b> - Removes the permission on an object for a Principal. Overrides all other permissions, including those inhereted from a Role assignment.</li> 
</ul>

<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Apply Permissions to Azure SQL DB Objects</b></h4>
<br>

In this Activity you will work with Permissions on various database objects. You can alter these scripts after you run them to experiment with how Permissions work.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

1. Add the three objects from the previous Module:

<pre>
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
        , (5, 'Engleton', 'Edbert', '5555 Esquire Rd. E', 'Easton', '555-55-5555', '5555-5555-5555-5555');
        GO
</pre> 

<pre>
    CREATE VIEW Patient_Mailing_Address AS
    SELECT FirstName, LastName, Address, City
    FROM Patient;
    GO
</pre>

<pre>
    CREATE PROCEDURE uspGetCardInformation   
        @Loginid tinyint  
    AS   

        SET NOCOUNT ON;  
        SELECT Loginid, cardnumber  
        FROM Patient  
        WHERE loginId= @loginId   
    GO  
</pre>

2. Connect with User1 or User2 to see what current Permissions allow:

<pre>
    Select LoginId FROM Patient WHERE Loginid = 1;
    GO

    EXEC uspGetCardInformation @Loginid = 2;
    GO

    SELECT * FROM Patient_Mailing_Address;
    GO
</pre>

3. Switch to an admin user and GRANT CONTROL (all permisisons) on all three objects to the Elevated_permissions role:

<pre>
    GRANT CONTROL ON Patient TO Elevated_Permissions;
    GRANT CONTROL ON Patient_Mailing_Address TO Elevated_Permissions;
    GRANT CONTROL ON uspGetCardInformation TO Elevated_Permissions;
</pre>

4. Run the script again on User1:

<pre>
    Select LoginId FROM Patient WHERE Loginid = 1;
    GO

    EXEC uspGetCardInformation @Loginid = 2;
    GO

    SELECT * FROM Patient_Mailing_Address;
    GO
</pre>

5. Using your admin connection, Revoke SELECT for User1 on the Patient_Mailing_Address view:

<pre>
    REVOKE SELECT ON Patient_Mailing_Address TO User1;
</pre>

6. Now return to User1's conenction, run the query again:

<pre>
    SELECT * FROM Patient_Mailing_Address;
    GO
</pre>

7. As you can see, GRANT supercedes REVOKE. Return to admin and DENY SELECT on the view:

<pre>
    DENY SELECT ON Patient_Mailing_Address TO User1;
</pre>

8. Finally, rerun the same query with User1. You will note that the DENY permission overrides the previous GRANT:

<pre>
    SELECT * FROM Patient_Mailing_Address;
    GO
</pre>


> Always use the principle of <i>Least Privilege</i> each time you assign permissions, and cross referencing permissions granted through each role is necessary. The <a href="https://aka.ms/sql-permissions-poster" target="_blank">Permissions Poster produced by Microsoft</a> can help illustrate that heirarchy.

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 4 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="04"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">3.4 Applications</h2>
<p>This section repeats much of the same information as an installed Instance of SQL Server, so you can review the text below, running any sample scripts that you ommitted in the previous Module.</p>

Apart from the <a href="https://docs.microsoft.com/en-us/dotnet/standard/security/secure-coding-guidelines" target="_blank">Secure Coding principles your client applications should follow</a>, there are security mechanisms within Azure SQL DB that you can leverage in your code for enhanced protections.

<h3>Client Libraries and TLS</h3>
Azure SQL DB requires SSL/TLS at all times. There are, however, various version of TLS, and you want to <a href="https://support.microsoft.com/en-us/topic/kb3135244-tls-1-2-support-for-microsoft-sql-server-e4472ef8-90a9-13c1-e4d8-44aad198cdbe" target="_blank">implement the highest version possible</a> when you connect to an Azure SQL DB. You can find the <a href="https://docs.microsoft.com/en-us/sql/connect/sql-connection-libraries?view=sql-server-ver16" target="_blank">latest drivers and connection methods for Azure SQL DB at this reference</a>. Each of these connection libraries has differing security impacts, so it is important to review the latest releases and use the most secure methods of access possible. 

<h3>Protection of Sensitive Data</h3>
You should follow the best practices for <a href="https://social.technet.microsoft.com/wiki/contents/articles/930.sql-server-how-to-design-create-and-maintain-a-database.aspx" target="_blank">data design for your application</a>, so that you store sensitive data in a separate, protected space. You can then move to to protecting the data within the application. You have various options for creating a secure database program. 

<h4>Permissions and Rights, Views and Stored Procedures</h4>
The first step is to implement a least-privilege approach within your permission structure, as described in the previous sections. By granting permissions only to the higher-level objects (such as Views, Functions and Store Procedures) you are able to protect the underlying base Tables from unecessary access by the end user. 

Views allow you to show only the collumns and rows required for least-privilege, and Functions and Stored Procedures allow you to restrcit both columns and rows. 

<h4>Row Level Security</h4>
Azure SQL DB <a href="https://docs.microsoft.com/en-us/sql/relational-databases/security/row-level-security" target="_blank">provides Row-Level Security</a> so that you can restrict access to objects based on the security context of the user - whether that is based on Role membership or even the execution context of the query.

By creating a Function and a Security Policy, you can set up protections for _SELECT_, _UPDATE_ and _DELETE_ operations. 

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Apply Row-Level Security to an Object</b></p>
In this Activity you will explore setting up Row-Level Security on your Azure SQL DB environment. 

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

<ol>
  <li><a href="https://docs.microsoft.com/en-us/sql/relational-databases/security/row-level-security?view=sql-server-ver16#CodeExamples" target="_blank">Navigate to this reference, and follow all the steps you see there for Scenario "A"</a>, using your sample Azure SQL DB environment.</li> 
</ol>

<h4>Dynamic Data Masking</h4>
<a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/dynamic-data-masking-overview?view=azuresql" target="_blank">Dynamic Data Masking</a> allows you to substitute a standard return from a Qeury with obfuscated characters. You can replace all or part of a result string with another character (such as the letter X). That means you could allow your users to see that there is in fact data in a field or part of a field, without showing them the data itself.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Review Dynamic Data Masking</b></p>
In this Activity you will review an example set of scripts that implement Dynamic Data Masking, and shows the data returned. 

> You can implement these scripts if you would like a hands-on experience in your sample workshop database. 

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

<ol>
  <li><a href="https://docs.microsoft.com/en-us/sql/relational-databases/security/dynamic-data-masking?view=sql-server-ver16#creating-a-dynamic-data-mask" target="_blank">Navigate to this reference, and review all the steps you see there</a>, using your sample Azure SQL DB environment.</li> 
</ol>

<h4>SQL Ledger</h4>

SQL Server 2022 and Microsoft Azure SQL DB include a new feature called <a href=" https://docs.microsoft.com/en-us/azure/azure-sql/database/ledger-landing?view=azuresql" target="_blank">Ledger</a> that stores a one-way hash root digest of the data rows in a table. This Ledger can be kept in a secure location so that the hash of the current rows can be compared to the digest, alerting you to any differences, which indicates that the data has been tampered with.

With the Ledger feature, you are able to create updatable tables, or insert-only tables depending on your application's needs.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Review SQL Ledger</b></p>
In this Activity you will review an example of setting up and working with SQL Ledger. 

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

<ol>
  <li><a href="https://docs.microsoft.com/en-us/sql/relational-databases/security/ledger/ledger-how-to-updatable-ledger-tables?view=azuresqldb-current" target="_blank">Navigate to this reference, and review all the steps you see there</a>, using your sample Azure SQL DB environment.</li> 
</ol>

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 5 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="05"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">3.5 Encryption, Certificates, and Keys</h2>
<br>

Microsoft Azure SQL DB supports encryption of data in multiple places: in-transit, at-rest, in-database and more.

For data-in-transit, Transport Layer Security (TLS) is enforced for connections to the server over secure protocols. 

For data encryption, two mechanisms are available. For data-at-rest, Azure SQL Database automatically implements the SQL Server <i>Transparent Data Encryption</i> (TDE) feature. With no code or database changes, your database is encrypted on storage. 

For data encryption, it's important to understand how Azure SQL DB uses Keys to encrypt data.

On creation, an Azure SQL DB "Server" creates a <i>Service Master Key</i> (SMK), which is used to create a <i>Database Master Key</i> (DMK). These Keys are protected by either a Certificate, an Asymmetric Key (such as a password) or a Symmetric Key to lock and unlock the Key. An Extensible Key Management (EKM) module can also be used, holding symmetric or asymmetric keys outside of Azure SQL DB.

You can encrypt data as you insert it using  T-SQL functions, such as <a href="https://docs.microsoft.com/en-us/sql/t-sql/functions/encryptbypassphrase-transact-sql?view=azuresqldb-current" target="_blank">ENCRYPTBYPASSPHRASE</a> and <a href="https://docs.microsoft.com/en-us/sql/t-sql/functions/decryptbypassphrase-transact-sql?view=azuresqldb-current" target="_blank">DECRYPTBYPASSPHRASE</a> calls.

Another method of setting up encryption for your database is using the <i>Always Encrypted</i> feature. Always Encrypted allows applications to encrypt sensitive data wihtout revealing the encryption keys to the Database Engine. This is transparent to the application, so you don't need to write special code to take advantage of this feature.  

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Set up Always Encrypted on your Sample Database</b></p>
In this Activity you will implement Always Encrypted on your sample course database.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

<ol>
  <li><a href="https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-wizard?view=azuresqldb-current" target="_blank">Navigate to this reference, and follow all the steps you see there</a>, using your sample Azure SQL DB environment.</li> 
</ol>

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 6 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="06"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">3.6 Auditing</h2>
<br>
Microsoft Azure SQL DB has an auditing capability similar to a SQL Server installation on-premises, but since you do not have access to the storage where the service is running, you send the output of the audits to an Azure storage account, a Log Analytics workspace, or to Azure Event Hubs. Each of these targets provides different interfaces and features, but they store the same information. 
<br>
  
You can enable a logical <i>Server</i> audit, a <i>Database</i> audit, or both. In general, you should choose one or another, but not both. Enabling a Server audit audits all the databases on that server, and any new ones you create, and sends it to a single target. Enabling a Database audit allows you to send each audit collection to a different target. 
<br>

You can <a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/auditing-overview?view=azuresql" target="_blank">learn more about Azure SQL DB audits here</a>.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Set up a Server Audit on your Azure SQL DB account</b></p>

In this Activity you will implement a Server audit on your sample course database environment.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

<ol>
  <li><a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/auditing-overview?view=azuresql#setup-auditing" target="_blank">Navigate to this reference, and follow all the steps you see there</a>, using your sample Azure SQL DB environment.</li> 
</ol>

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Closing   =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/owl.png"><b>For Further Study</b></p>

<ul>
  <li><a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/security-overview?view=azuresql" target="_blank">Official Microsoft Documentation for this section</a></li>
  <li><a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/security-best-practice?view=azuresql" target="_blank">Microsoft Azure SQL DB Security Best Practices</a></li>
</ul>
  
<br>
<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/geopin.png"><b >Next Steps</b></p>

Next, Continue to <a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/04%20-%20MonitoringAndIncidentResponse.md" target="_blank"><i> 04 - Monitoring and Incident Response</i></a>.
