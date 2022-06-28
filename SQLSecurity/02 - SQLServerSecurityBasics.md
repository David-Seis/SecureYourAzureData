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
  <dt><a href="#03" target="_blank"><dt>03 - Rights and Permissions</dt></a>
  <dt><a href="#04" target="_blank"><dt>04 - Programmatic Security</dt></a>
</dl>

<p style="border-bottom: 1px solid lightgrey;"></p>

<h2 id="01"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">1.0 Principals</h2>

Logins and Users
  Relationship
  
However, there is one exception to this rule: Contained Databases. <TODO: Research and fill out>

SQL Server controlled security accounts

Active Directory Integration 
  (Linux) 

Azure Active Directory

Certificates and other Authentication Methods

Roles and Role-Based Access Control

Schemas 

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: List Principals in an Instance</b></p>

TODO: Activity Description and tasks - Something about listing Instance, DB and Contained Principals, along with Certificates. 

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Description</b></p>

The following code will list all Principals on an Instance, in each Database, and show any Certificates in use on your test system. You will run this code multiple times throughout this course to show the effect of adding, removing, and altering a Principal.
  
  > It is a common practice to run these scripts and save the output to another database or secure artifact. This way you can provide an audit trail for users on the system. 

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>

Run the following code on your test system: 
  
<pre>
/* Code Goes here with Comment */

SELECT @@VERSION;
GO 

</pre>

<p style="border-bottom: 1px solid lightgrey;"></p>  

  
<h2 id="02"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">2.0 Securables</h2>

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
