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
<dl>
  <dt><a href="#01" target="_blank"><dt>01 - Principals</dt></a>
  <dt><a href="#02" target="_blank"><dt>02 - Securables</dt></a>
  <dt><a href="#03" target="_blank"><dt>03 - Applications</dt></a>
  <dt><a href="#04" target="_blank"><dt>04 - Encryption, Certificates, and Keys</dt></a>
  <dt><a href="#04" target="_blank"><dt>05 - Auditing</dt></a>
  
</dl>

<p style="border-bottom: 1px solid lightgrey;"></p>

<h2 id="01"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">1.0 Principals</h2>


This topic is broad, and we will hit the high points of each sub-point but there will always be more to invetsigate on your own time. Here is the 50,000 foot view:

Access to SQL is managed through principals, or logins. Logins can be for a user, a computer, an application, a service, a group, <TODO: and more?>. Each principal will have a way for the user to 'log in' (considering passwordless), which then enables the user of the account to Access SQL Server at the most basic level, and then we get to the authorization side. Authorization is What you can do once you are connected.

<h3>Authentication</h3>
<br>

Now to break it down:
When it comes to SQL, there are two modes of authentication: 1) SQL Authentication where the account is managed and stored inside of SQL, or 2) managed externally through Windows (including Active directory, Azure AD and Linux AD Integration). Remember Authentication only pertains to whether or not you can log in to the server. Authorization will come later.


1) SQL Authentication
The Master database stores all security principals except in the case of a contained database. For all principals stored in the master database, the username and permissions are tracked and managed. 

Contained Databases are those where the authentication and authroization are stored inside, without ever using the master database.  <TODO: Research and fill out conenciton and use cases.>

One key difference between these two kinds of authentication is that SQL stores the password for any non-Windows managed principals. Because passwords are stored in SQL, it is up to the DBA to ensure that a password and account policy is created and applied to each user.


2) Windows Authentication (AD, AAD, Linux AD integration)

It is typically easier and safer to manage accounts via Active Directory. There are some organizations that do not work in a domain, and there are some who use applications that require a certain API to reach SQL. Otherwise, if you have a domain and your app can easily reach SQL, active directory is the safer, and typically easier to manage choice. The ease comes in reducing the number of accounts needed to access the server (wihtout AD integration a user needs one account to log into their computer, and then another to access SQL Server), simplifying account lifecycles and policy enforcment, and the ease of using groups to manage access and permissions.

A login for a windows group can be created, and later when we get into authorization, permissions and roles can be granted to the group rather than the individual, which makes provisioning accounts and access a more streamlined process.

With relatively little work, Azure can be synced with an on-prem Active directory which opens up a lot of possibilities. This is especially true because there is now the option to extend Active directory to Linux servers. Which enables the SQL installations there to make use of Windows authentication!

<h4>Other kinds of Authentication</h4>
- Application roles
  <TODO: research and explain this>
- Users mapped to certificates
  <TODO: research and explain this>
- Users Mapped to Asymmetric keys
  <TODO: Research and explain this>

<h3>Authorization</h3>
<br>

Once you are logged into a SQL server Authorization is the next contender. If you were to log in to a SQL server where you had been given an account and no server roles, what would you see?

<TODO: description of what they would see>

Role- security
So the next step for each user or group is to look through the prebuilt server and database roles. Roles are a collection of permissions based on general use cases for a SQL Server user and there a general breakdown in the table below:

<TODO: create table of Server roles, database roles.>

IF you want to create your own roles, you can dig into the permissions poster to have the exact levers pulled to get the access profile your organization needs. We recommend against changing server roles, but you can copy the permissions of a prebuilt role and modify from there you can get a custom role tailored to your specific needs.

<TODO: Link the role permissions poster>

Combining roles with Windows Authentication groups helps efficiently and quickly provision permisisons to large groups of people, and it makes for less work on the DBA. However, be aware that if you ar enot managing who is going into and out of roles it can become a security risk. Regular account, role, and group audits should be done in your organization. 




SQL Server controlled security accounts

Active Directory Integration 
  (Linux) 

Azure Active Directory

Certificates and other Authentication Methods

Roles and Role-Based Access Control

Schemas 

Principals "Worst" Practices
- Having one user account for a group of individuals to use. This can limit the auditing and tracking capabilites of SQL server, so if a malicious actor does somehting heinous under a shared account, it is less likely you will be bale to find out who did it.


<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: List Principals in an Instance</b></p>

TODO: Activity Description and tasks - Something about listing Instance, DB and Contained Principals, along with Certificates. 

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

The following code will list all Principals on an Instance, in each Database, and show any Certificates in use on your test system. You will run this code multiple times throughout this course to show the effect of adding, removing, and altering a Principal.
  
  > It is a common practice to run these scripts and save the output to another database or secure artifact. This way you can provide an audit trail for users on the system. 


Code to query all principals in the master database:
  <pre>
  SELECT * FROM sys.server_principals 
  </pre>

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

Classify Database Principals (this needs to be run in each database ocntext.)
  <pre>
  SELECT * FROM sys.database_principals order by 'type'
  </pre>

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

Run the following code on your test system: 
  
<pre>
/* Code Goes here with Comment */

SELECT @@VERSION;
GO 

</pre>

<p style="border-bottom: 1px solid lightgrey;"></p>  

  
<h2 id="02"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.0 Securables</h2>

Securing your SQL Server is the goal of this course, where principals can be managed to limit what they can access, knowing what and how to limit their access to the different *securables* in your environment is key. A securable is anything isn SQL server, a database, a view, a column, and indiviudal cell. SQL server's granular controls can be used to increase the security of the data you manage.

Object Hierachy
  
TODO: Topic Description

<br>

<img style="height: 400; box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);" src="https://docs.microsoft.com/en-us/sql/big-data-cluster/media/concept-security/cluster_endpoints.png">

<br>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: TODO: Activity Name</b></p>

TODO: Activity Description and tasks

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

TODO: Enter activity description with checkbox

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

TODO: Enter activity steps description with checkbox

<p style="border-bottom: 1px solid lightgrey;"></p>

<h2><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">1.2 TODO: Topic Name</h2>

TODO: Topic Description

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: TODO: Activity Name</b></p>

TODO: Activity Description and tasks

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

TODO: Enter activity description with checkbox

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

TODO: Enter activity steps description with checkbox

<p style="border-bottom: 1px solid lightgrey;"></p>

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/owl.png"><b>For Further Study</b></p>
<ul>
    <li><a href="url" target="_blank">TODO: Enter courses, books, posts, whatever the student needs to extend their study</a></li>
</ul>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/geopin.png"><b >Next Steps</b></p>

Next, Continue to <a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/03%20-%20SQLAzureSecurity.md" target="_blank"><i> 03 - Azure SQL DB Security</i></a>.
