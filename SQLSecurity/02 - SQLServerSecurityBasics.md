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

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 1 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="01"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">1.0 Principals</h2>


This topic is broad, and we will hit the high points of each sub-point but there will always be more to invetsigate on your own time. Here is the 50,000 foot view:

Access to SQL is managed through principals, or logins. Logins can be for a user, a computer, an application, a service, a group, <TODO: and more?>. Each principal will have a way for the user to 'log in' (considering passwordless), which then enables the user of the account to Access SQL Server at the most basic level, and then we get to the authorization side. Authorization is What you can do once you are connected.

<h3>Authentication</h3>
<br>

Now to break it down:
When it comes to SQL, there are two modes of authentication: 1) SQL Authentication where the account is managed and stored inside of SQL, or 2) managed externally through Windows (including Active directory, Azure AD and Linux AD Integration). Remember Authentication only pertains to whether or not you can log in to the server. Authorization will come later.


<h4>1) SQL Authentication</h4>
The Master database stores all security principals except in the case of a contained database. For all principals stored in the master database, the username and permissions are tracked and managed. 

Contained Databases are those where the authentication and authroization are stored inside, without ever using the master database.  <TODO: Research and fill out conenciton and use cases.>

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

This activity will take you to your test instance where you will query the principals that are present. You will be able to classify principals based on type, which will be useful in getting a clearer security picture for your environment. 

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

The following code will list all Principals on an Instance, in each Database, and show any Certificates in use on your test system. You will run this code multiple times throughout this course to show the effect of adding, removing, and altering a Principal.

  > It is a common practice to run these scripts and save the output to another database or secure artifact. This way you can provide an audit trail for users on the system. 

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

Run the following code on your test system: 
  <pre>
      SELECT * FROM sys.server_principals 
  </pre>

If you would like to further classify the output run these: 

Classify Server principals by type:
<pre>
    -- Windows Groups
      SELECT * FROM sys.server_principals where type = 'G'

    -- Windows Logins
      SELECT * FROM sys.server_principals where type = 'U'

    -- SQL Logins 
      SELECT * FROM sys.server_principals where type = 'S'

    -- Certificate mapped logins
      SELECT * FROM sys.server_principals where type = 'C'

    -- Server Roles
      SELECT * FROM sys.server_principals where type = 'R'

    -- Asymmetric Key mapped Users
      SELECT * FROM sys.server_principals where type = 'K'

    -- External login from Azure Active Directory
      SELECT * FROM sys.server_principals where type = 'E'

    -- External group from Azure Active Directory/ applications
      SELECT * FROM sys.server_principals where type = 'X'
</pre>

Classify Database Principals (this needs to be run in each database context.)
  <pre>
  SELECT * FROM sys.database_principals order by 'type'
  </pre>

After taking inventory of each principal on your server, do more research into any that you are surprised or unsure about. A solid grasp on **WHO** can log into the server is a solid starting point in determining **WHAT** they should be able to do!


<p style="border-bottom: 1px solid lightgrey;"></p>  


[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 2 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="02"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.0 Securables</h2>
<br>

Securing your SQL Server is the goal of this course. Principals can be managed to limit who can access the server, Adding permissions to each user to determine WHAT they can do while they are logged in is key to a secure system. A **Securable** in SQL server is virtually everything. Knowing how to limit access to sensitive data and leveraging least privilege when assigning permissions is of the utmost importance when securing a SQL instance. The topic of securables is usually broken down into three categories, or scopes, for ease of use:

<h3>Server Scope</h3>
<ul>
  <li>Availability Group</li> 
  <li>Endpoint</li>
  <li>Logins</li>
  <li>Server Roles</li>
  <li>Database</li>
</ul>

<h3>Database Scope</h3>
<ul>
  <li>Application Roles</li> 
  <li>Assembly</li>
  <li>Asymmetric Keys</li>
  <li>Certificates</li>
  <li>Contract</li>
  <li>Fulltext Catalogs</li> 
  <li>Fulltext stoplist</li>
  <li>Message type</li>
  <li>Remote Service Binding</li>
  <li>Database Roles</li>
  <li>Route</li> 
  <li>Search Property List</li>
  <li>Service</li>
  <li>Symmetric Key</li>
  <li>User</li>
</ul>

<h3> Schema Scope</h3>
<ul>
  <li>Type</li> 
  <li>XML schema Collection</li>
  <li>Objects</li>
    <ul>
      <li>Aggregate</li>
      <li>Function</li> 
      <li>Procedure</li>
      <li>Queue</li>
      <li>Synonym</li>
      <li>Table</li>
      <li>View</li> 
      <li>External Table</li>
    </ul>

</ul>
  
Each securable includes a set of permissions relevant to scope and function. Permissions can be granted, denied, or revoked. Referencing the general [permissions hierarchy](https://docs.microsoft.com/en-us/sql/relational-databases/security/permissions-database-engine?view=sql-server-ver16) as well as reviewing the built in [Server roles](https://docs.microsoft.com/en-us/sql/relational-databases/security/permissions-database-engine?view=sql-server-ver16) and [Database roles](https://docs.microsoft.com/en-us/sql/relational-databases/security/permissions-database-engine?view=sql-server-ver16) is good start to understanding what roles are necessary for most users. Considering least privilege each time you assign a role, and cross referencing permissions granted through each role is necessary. These images can help with that process, as well as the [Permissions Poster produced by Microsoft](https://aka.ms/sql-permissions-poster). 
<br>

<img style="height: 400; box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);" src="https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/media/permissions-of-server-roles.png?view=sql-server-ver16">

> Most environments use these out of the box roles. Unfortunately many users have more permissions than they need because it is simpler to manage.

<img style="height: 400; box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);" src="https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/media/permissions-of-database-roles.png?view=sql-server-ver16">




<br>

<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Query the Roles, Permissions, and Principals in your Test Environment</b></h4>
<br>

Knowing **WHO** is in your environment and **WHAT** they can do is an important step in strengthening your security posture.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

Use the following Scripts to get an overview of the users, permisisons, and role memberships in your environment (this is technically a security *audit* but that is ok, I wont tell if you don't!)

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

1) Get a list of users based on their Server Roles:
<pre>
      SELECT a.name AS Role, b.name AS Principal
      FROM
          master.sys.server_role_members ab
              inner join
                master.sys.server_principals a 
                on a.principal_id = ab.role_principal_id and a.type = 'R'
              inner join
                master.sys.server_principals b 
                on b.principal_id = ab.member_principal_id
</pre> 
2) Get a list of all users based on their database roles:
<pre>
      EXEC sp_MSforeachdb @command1 ='
      SELECT  ''?''
      ,   DP1.name AS DatabaseRoleName
      ,   DP2.name AS DatabaseUserName
      FROM sys.database_role_members AS DRM
      RIGHT OUTER JOIN sys.database_principals AS DP1
          ON DRM.role_principal_id = DP1.principal_id  
      LEFT OUTER JOIN sys.database_principals AS DP2  
          ON DRM.member_principal_id = DP2.principal_id  
      WHERE 
          DP1.type = ''R'' and 
          DP2.name is not Null
      ORDER BY DP1.name;'  
</pre>
3) Cross reference the users and groups with the roles they are and, determine if these levels of permissions are appropriate.
Remembering least privilege. Reducing permissions will make management more tedious in the short term, and it will cause those with higehr permissions to be consulted more so that changes can be made to the server, but this will help strengthen your security posture and protect your business in the long run, especially from accidents amplified by elevated permissions.

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 3 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="03"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">3.0 Applications</h2>
<br>

TODO: Topic Description

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: TODO: Activity Name</b></p>

TODO: Activity Description and tasks

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

TODO: Enter activity description with checkbox

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

TODO: Enter activity steps description with checkbox

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 4 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="04"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">4.0 Encryption, Certificates, and Keys</h2>
<br>

TODO: Topic Description

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: TODO: Activity Name</b></p>

TODO: Activity Description and tasks

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

TODO: Enter activity description with checkbox

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

TODO: Enter activity steps description with checkbox

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 5 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="05"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">5.0 Auditing</h2>
<br>

TODO: Topic Description

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: TODO: Activity Name</b></p>

TODO: Activity Description and tasks

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

TODO: Enter activity description with checkbox

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

TODO: Enter activity steps description with checkbox

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Closing   =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/owl.png"><b>For Further Study</b></p>
<ul>
    <li><a href="https://github.com/sqlstudent144/SQL-Server-Scripts/blob/master/sp_SrvPermissions.sql" target="_blank">An example stored procedure to fetch Server permissions</a></li>
    <li><a href="https://github.com/sqlstudent144/SQL-Server-Scripts/blob/master/sp_DBPermissions.sql" target="_blank">An example stored procedure to fetch Database permissions</a></li>
</ul
>
<br>
<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/geopin.png"><b >Next Steps</b></p>

Next, Continue to <a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/03%20-%20SQLAzureSecurity.md" target="_blank"><i> 03 - Azure SQL DB Security</i></a>.
