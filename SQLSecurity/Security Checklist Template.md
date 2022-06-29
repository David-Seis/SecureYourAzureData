<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/Lock-2.png">

# Workshop: SQL Server Security Ground to Cloud

#### <i>A Security Course For Data Professionals</i>

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/textbubble.png"> <h2>SQL Server Security Checklist Template</h2>

In <a href="https://github.com/David-Seis/SecureYourAzureData" target="_blank">the SQL Server Security Gtround to Cloud workshop</a> you'll cover the basics of securing SQL Server installations and databases, from on-premises systems to Microsoft Azure deployments. After completing that workshop, you will know the basics of securing your system along with the other teams in your organization. This checklist should be used as a guide to form the basis of your own security posture, after you have completed this workshop and fully understand each section's implementation.

This checklist covers both the Microsoft Azure SQL DB platform as well as physical installations of SQL Server on bare-metal or Virtual Machine Environments. SQL Server Installations are the responsibility of the data professional and their larger security team, and Microsoft Azure SQL DB security is a shared responsibility between Microsoft and your organization. A more [complete description of these responsibilities are here](https://docs.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility).

<p style="border-bottom: 1px solid lightgrey;"></p>

https://docs.microsoft.com/en-us/azure/security/fundamentals/zero-trust 
Security review scheduled

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Security Checklist by "Defense in Depth" Area</b></p>

The following sections should be answered as "*Complete*", "*Checked*", or "*Implemented*". If any area is not marked with one of these designations, you should complete the tasks to secure the area using the knowledge gained in the SQL Server Security course.

<br>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/Compass.png"><b>Physical</b></p>

Physical security involves restricting and [controlling access to your datacenter and computing assets](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack) to only allow authorized access.

*SQL Server Installations*
<br><img src="../graphics/checkbox.png"> The physical server hosting SQL Server is kept in a secure location, with alarm and monitoring systems, which are periodically tested.
<br><img src="../graphics/checkbox.png"> Only authorized personnel have access to the physical server that hosts SQL Server.
<br><img src="../graphics/checkbox.png"> All access to the physical system hosting SQL Server is audited and periodically reviewed.
<br><img src="../graphics/checkbox.png"> Encryption at rest (such as Transparent Data Encryption) evaluated and implemented wherever possible.
<br><img src="../graphics/checkbox.png"> All Database Backups are encrypted, or stored on Encrypted media.
<br><img src="../graphics/checkbox.png"> Server Master Keys and Database Master Keys, along with other Certificate mechanisms, are backed up and secured.


*Microsoft Azure SQL DB environments*
<br><img src="../graphics/checkbox.png"> The following reference has been reviewed and approved by appropriate security auditing teams: [https://docs.microsoft.com/en-us/azure/security/fundamentals/physical-security](https://docs.microsoft.com/en-us/azure/security/fundamentals/physical-security)
<br><img src="../graphics/checkbox.png"> All Database Backups are encrypted, or stored on Encrypted media, especially if exported from Microsoft Azure.
 
<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/Compass.png"><b>Perimeter</b></p>

Perimeter security entails creating defenses at the network level for [distributed denial of service (DDoS)](https://docs.microsoft.com/en-us/azure/ddos-protection/types-of-attacks) attacks, and using [network access controls](https://docs.microsoft.com/en-us/azure/azure-sql/database/network-access-controls-overview?view=azuresql), [Network Security Groups](https://docs.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview), and other network segmentation strategies to limit communication between systems, avoiding spoofing, man-in-the-middle attacks, and other network-related issues.

*SQL Server Installations*
<br><img src="../graphics/checkbox.png"> Only required network protocols are enabled using the SQL Server Configuration Manager utility. 
<br><img src="../graphics/checkbox.png"> Default SQL Server ports changed to non-standard value.
<br><img src="../graphics/checkbox.png"> Disable the SQL Server Browser Service using the SQL Server Configuration Manager utility.

*Microsoft Azure SQL DB environments*
<br><img src="../graphics/checkbox.png"> The following reference has been reviewed and approved by appropriate security auditing teams: [https://docs.microsoft.com/en-us/azure/security/fundamentals/infrastructure-sql](https://docs.microsoft.com/en-us/azure/security/fundamentals/infrastructure-sql)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/Compass.png"><b>Compute</b></p>

The Compute security area requires creating a [strong system](https://techcommunity.microsoft.com/t5/itops-talk-blog/introduction-to-secured-core-computing/ba-p/2701672) for controlling access to physical and virtual machines, and implementing strong Cloud controls.

*SQL Server Installations*
<br><img src="../graphics/checkbox.png"> All Windows Operating System Service Packs and Linux updates evaluated and applied after testing to the system(s) hosting SQL Server binaries and files. 
<br><img src="../graphics/checkbox.png"> Firewalls configured to allow only specific access from authorized systems. 
<br><img src="../graphics/checkbox.png"> A process is in place for auditing and reviewing all Operating System users to check for administrative or elevated access.
<br><img src="../graphics/checkbox.png"> A process is in place for auditing and reviewing all Operating System users to check for unusual access to resources.
<br><img src="../graphics/checkbox.png"> Binary and file locations for the SQL Server are secured against unauthorized access.
<br><img src="../graphics/checkbox.png"> Only required Operating System components, utilities, and features are installed required for operating the SQL Server installation.
<br><img src="../graphics/checkbox.png"> System lock after timeout enabled.
<br><img src="../graphics/checkbox.png"> Virus and other malware scans and tools enabled, run, and periodically evaluated.
<br><img src="../graphics/checkbox.png"> Only required SQL Server components, utilities and features are installed required for securely servicing authorized data and programmatic requests.
<br><img src="../graphics/checkbox.png"> SQL Server is at currently supported version.
<br><img src="../graphics/checkbox.png"> SQL Server latest Service Packs and/or Cumulative Updates tested, installed and configuration documents updated.
<br><img src="../graphics/checkbox.png"> SQL Server CLR feature evaluated for necessary and proper use, disabled if not.
<br><img src="../graphics/checkbox.png"> SQL Server Application Roles documented with applicable applications that use them.
<br><img src="../graphics/checkbox.png"> SQL Server Guest user disabled.

*Microsoft Azure SQL DB environments*
<br><img src="../graphics/checkbox.png"> Thing

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/Compass.png"><b>Identitiy and Authorization</b></p>

Identity and Authorization security defines the appropriate *Principals* and checking that they are who they claim using [multifactor authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-mfa-howitworks) and other conditional access systems for infrastructure, code, and change tracking systems.

*SQL Server Installations*
<br><img src="../graphics/checkbox.png"> Rename or disable the *sa* SQL Server account after testing.
<br><img src="../graphics/checkbox.png"> Use *Integrated Authentication* wherever possible. Active Directory or Azure Active Directory used wherever possible.
<br><img src="../graphics/checkbox.png"> The *SQL Server Configuration Manager* utility is used for all Service Account changes.
<br><img src="../graphics/checkbox.png"> All SQL Server Services evaluated and disabled where not required for servicing authorised data requests.
<br><img src="../graphics/checkbox.png"> All SQL Server Services use specific, low-privilege accounts for each service operation, and are periodically reivewed for activity.
<br><img src="../graphics/checkbox.png"> A strong audit and review process is in place for evaluating user access to data and programmatic objects, such as using SQL Server Audit and other utilities.
<br><img src="../graphics/checkbox.png"> A specific audit and review rule is in place for evaluating elevated Principal accounts (such as Instance or Database Administrator effective permissions), and to ensure no applications use an elevated account.
<br><img src="../graphics/checkbox.png"> A specific audit and review rule is in place for evaluating Failed Login attempts.
<br><img src="../graphics/checkbox.png"> The *public* group has no execute access to unnecessary stored procedures, such as extended stored procedures. 
<br><img src="../graphics/checkbox.png"> The *xp_cmdshell* is disabled.


*Microsoft Azure SQL DB environments*
<br><img src="../graphics/checkbox.png"> Thing
 
<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/Compass.png"><b>Application</b></p>

The Application security area involves implementing [Secure Code](https://docs.microsoft.com/en-us/dotnet/standard/security/secure-coding-guidelines) practices and policies to prevent security vulnerabilities.

*SQL Server Installations*
<br><img src="../graphics/checkbox.png"> Thing 

*Microsoft Azure SQL DB environments*
<br><img src="../graphics/checkbox.png"> Thing
 

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/Compass.png"><b>Data</b></p>

The Data Security Area involves ensuring that business and customer data is encrypted and protected against unwanted access at rest, in=-transit, in-memory and in-code processes. This is the focus of this course.

*SQL Server Installations*
<br><img src="../graphics/checkbox.png"> Only system and user databases are installed and configured.

*Microsoft Azure SQL DB environments*
<br><img src="../graphics/checkbox.png"> Thing

<a href="url" target="_blank">url</a>

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/owl.png"><b>For Further Study</b></p>
<ul>
    <li><a href="url" target="_blank">Official Documentation for this section</a></li>
</ul>
