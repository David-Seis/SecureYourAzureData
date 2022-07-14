<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/Lock-2.png">

# Workshop: SQL Server Security Ground to Cloud

#### <i>A Security Course For Data Professionals</i>

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/textbubble.png"> <h2>03 - Azure SQL Security</h2>

In <a href="https://github.com/David-Seis/SecureYourAzureData" target="_blank">this workshop</a> you'll cover the basics of securing SQL Server installations and databases, from on-premises systems to Microsoft Azure deployments.

In each module you'll get more references, which you should follow up on to learn more. Also watch for links within the text - click on each one to explore that topic.

(<a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/00%20-%20Pre-Requisites.md" target="_blank">Make sure you check out the <b>Pre-Requisites</b> page before you start</a>. You'll need all of the items loaded there before you can proceed with the workshop.)

This module builds on the previous module where you learned about the basics of SQL Server Security. This module focuses on the the differences between those security aspects and security in Microsoft Azure SQL DB. 

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
You have two primary mechanisms for Principals in Azure SQL DB: SQL Server logins, and Azure Active Directory logins. 

<h3>Authentication</h3>
<br>
Similar to the mechanism in SQL Server, *authentication* only pertains to whether or not you can log in to the server. *Authorization*, which is covered later, defines the rights and privileges once the authentication of a Principal is determined. 


<h4>1) SQL Authentication</h4>
In the case of SQL Authentication, the **master** system database stores information for database security Principals where the username and permissions are tracked and managed. 

Using SQL Authentication means that Azure SQL DB stores the password for the Principals. Because passwords are stored in the master database, it is up to the database owner to ensure that a password and account policy is applied to each user.

<h4>2) Microsoft Azure Active Directory</h4>
As described in the last module, Microsoft Active Directory (AD) is a suite of services, and Active Directory Domain Services (AD DS) is the core Active Directory service used to manage users and resources. Microsoft Azure Active Directory (AAD) is a Domain Service run in the cloud. This is a more secure way to access resources, and allows a higher level of security with Multi-Factor Authentication (MFA), [as described in this reference.](https://docs.microsoft.com/en-us/azure/azure-sql/database/authentication-mfa-ssms-overview?view=azuresql)

You can also connect your local Active Directory to Microsoft Azure Active Directory, allowing administration of your local domain to access resources in the cloud, in a single-sign-on approach.  

[You can learn how to integrate Microsoft Azure Active Directory into Azure SQL DB using this resource.](https://docs.microsoft.com/en-us/azure/azure-sql/database/authentication-aad-configure?view=azuresql&tabs=azure-powershell) .

<br>

<h3>Roles in Microsoft Azure SQL DB</h3>

Thing

<br>

<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Thing</b></h4>
<br>

Thing

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

Thing

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

Run the following code on your test SQL Azure DB: 
<pre>
      Thing
</pre>


<p style="border-bottom: 1px solid lightgrey;"></p>  


[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 2 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="02"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.0 Securables</h2>
<br>
Just as in a SQL Server Instance, a **Securables** in Azure SQL DB are the objects the database contains. Securables fall into three categories, or scopes, for ease of use:

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

Thing

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

Thing

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

Thing

<pre>
  Thing
</pre> 

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
  <li><a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/security-overview?view=azuresql" target="_blank">Official Microsoft Documentation for this section</a></li>
  <li><a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/security-best-practice?view=azuresql" target="_blank">Microsoft Azure SQL DB Security Best Practices</a></li>
</ul>
  
<br>
<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/geopin.png"><b >Next Steps</b></p>

Next, Continue to <a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/04%20-%20MonitoringAndIncidentResponse.md" target="_blank"><i> 04 - Monitoring and Incident Response</i></a>.
