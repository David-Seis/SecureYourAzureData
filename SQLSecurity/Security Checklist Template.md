<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/Lock-2.png">

# Workshop: SQL Server Security Ground to Cloud

#### <i>A Security Course For Data Professionals</i>

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/textbubble.png"> <h2>SQL Server Security Checklist Template</h2>

In <a href="https://github.com/David-Seis/SecureYourAzureData" target="_blank">the SQL Server Security Ground to Cloud workshop</a> you'll cover the basics of securing SQL Server installations and databases, from on-premises systems to Microsoft Azure deployments. After completing that workshop, you will know the basics of securing your system along with the other teams in your organization. This checklist should be used as a *guide* to form the basis of your own security posture, after you have completed the workshop and fully understand each section's implementation.

This checklist covers both the Microsoft Azure SQL DB platform as well as physical installations of SQL Server on bare-metal or Virtual Machine Environments. SQL Server Installations are the responsibility of the data professional and their larger security team, and Microsoft Azure SQL DB security is a shared responsibility between Microsoft and your organization. A more <a href="https://docs.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility" target="_blank">complete description of these responsibilities is here.</a>

Some of these items are checked on installation or initial configuration, others are periodic tasks based on your organization's requirements and the general use of your installation. You should augment the steps in this guide to fit your organization and installations. You should review this checklist periodically to ensure compliance and to update the teams in your organization for changes or additions. 

<p style="border-bottom: 1px solid lightgrey;"></p>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Security Checklist by "Defense in Depth" Area</b></p>

The following sections should be answered as "*Complete*", "*Checked*", or "*Implemented*". If any area is not marked with one of these designations, you should complete the tasks to secure the area using the knowledge gained in the SQL Server Security course. You should also use the <a href="https://docs.microsoft.com/en-us/azure/security/fundamentals/zero-trust" target="_blank">"Zero-Trust"</a> model for your deployments. 

<br>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/Compass.png"><b>Physical</b></p>

Physical security involves restricting and <a href="https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack" target="_blank">controlling access to your datacenter and computing assets</a> to only allow authorized access.

*SQL Server Installations*
<br><img src="../graphics/checkbox.png"> The physical server hosting SQL Server is kept in a secure location, with alarm and monitoring systems implemented, which are documented and periodically tested.
<br><img src="../graphics/checkbox.png"> Only authorized personnel have access to the physical server that hosts SQL Server.
<br><img src="../graphics/checkbox.png"> All access to the physical system hosting SQL Server is audited and periodically reviewed.
<br><img src="../graphics/checkbox.png"> Encryption at rest (such as Transparent Data Encryption) has been evaluated and implemented wherever possible.
<br><img src="../graphics/checkbox.png"> All Database Backups are encrypted, or stored on Encrypted media.
<br><img src="../graphics/checkbox.png"> Server Master Keys and Database Master Keys, along with other Certificate mechanisms, are backed up and secured. Ideally in a different location. 

*Microsoft Azure SQL DB environments*
<br><img src="../graphics/checkbox.png"> The following reference has been reviewed and approved by appropriate security auditing teams: <a href="https://docs.microsoft.com/en-us/azure/security/fundamentals/physical-security" target="_blank">https://docs.microsoft.com/en-us/azure/security/fundamentals/physical-security</a>
<br><img src="../graphics/checkbox.png"> All Database Backups are encrypted, or stored on Encrypted media, especially if exported from Microsoft Azure.
 
<br>
<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/Compass.png"><b>Perimeter</b></p>

Perimeter security entails creating defenses at the network level for <a href="https://docs.microsoft.com/en-us/azure/ddos-protection/types-of-attacks" target="_blank">distributed denial of service (DDoS)</a> attacks, and using <a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/network-access-controls-overview?view=azuresql" target="_blank">network access controls</a>, <a href="https://docs.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview" target="_blank">Network Security Groups</a>, and other network segmentation strategies to limit communication between systems, avoiding spoofing, man-in-the-middle attacks, and other network-related issues.

*SQL Server Installations*
<br><img src="../graphics/checkbox.png"> Only required network protocols are enabled using the SQL Server Configuration Manager utility. 
<br><img src="../graphics/checkbox.png"> Default SQL Server ports have been changed to non-standard values.
<br><img src="../graphics/checkbox.png"> The SQL Server Browser Service has been disabled using the SQL Server Configuration Manager utility.
<br><img src="../graphics/checkbox.png"> Where possible, Always Encrypted is enabled to ensure encryption of data on the wire and at-rest.
<br><img src="../graphics/checkbox.png"> Strict connection encryption enabled for SQL Server 2022 and higher applications. 
<br><img src="../graphics/checkbox.png"> TLS 1.2 should be enforced when possible.
<br><img src="../graphics/checkbox.png"> Where applicable, the *Extended Protection for Authentication* feature is configured using channel binding and service binding. 

*Microsoft Azure SQL DB environments*
<br><img src="../graphics/checkbox.png"> The following reference has been reviewed and approved by appropriate security auditing teams: <a href="https://docs.microsoft.com/en-us/azure/security/fundamentals/infrastructure-sql" target="_blank">https://docs.microsoft.com/en-us/azure/security/fundamentals/infrastructure-sql</a>
<br><img src="../graphics/checkbox.png"> Microsoft Azure Server-level IP Firewall configured for appropriate access based on application patterns.
<br><img src="../graphics/checkbox.png"> Microsoft Azure Database-level IP Firewall configured for appropriate access based on application patterns.
<br><img src="../graphics/checkbox.png"> Microsoft Azure Virtual network service endpoints configured for appropriate access based on application patterns.
<br><img src="../graphics/checkbox.png"> Microsoft Azure Virtual network rules configured for appropriate access based on application patterns.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/Compass.png"><b>Compute</b></p>

The Compute security area requires creating a <a href="https://techcommunity.microsoft.com/t5/itops-talk-blog/introduction-to-secured-core-computing/ba-p/2701672" target="_blank">"strong system</a> for controlling access to physical and virtual machines, and implementing strong Cloud controls.

*SQL Server Installations*
<br><img src="../graphics/checkbox.png"> All Windows Operating System Service Packs and Linux updates evaluated and applied after testing to the system(s) hosting SQL Server binaries and files. 
<br><img src="../graphics/checkbox.png"> Firewalls are configured to allow only specific access from authorized systems. 
<br><img src="../graphics/checkbox.png"> A process is in place for auditing and reviewing all Operating System users to check for administrative or elevated access.
<br><img src="../graphics/checkbox.png"> A process is in place for auditing and reviewing all Operating System users to check for unusual access to resources.
<br><img src="../graphics/checkbox.png"> The Server that hosts SQL Server Instances does not also provide file shares, print spools, applications or other functions. 
<br><img src="../graphics/checkbox.png"> SQL Server is not installed on a Domain Controller.
<br><img src="../graphics/checkbox.png"> Binary and file locations for the SQL Server are secured against unauthorized access.
<br><img src="../graphics/checkbox.png"> Only required Operating System components, utilities, and features are installed required for operating the SQL Server installation.
<br><img src="../graphics/checkbox.png"> System lock after timeout is enabled.
<br><img src="../graphics/checkbox.png"> Virus and other malware scans and tools are enabled, run, and results are periodically evaluated. The following reference has been reviewed and approved by appropriate security and auditing teams: <a href="https://support.microsoft.com/en-us/topic/how-to-choose-antivirus-software-to-run-on-computers-that-are-running-sql-server-feda079b-3e24-186b-945a-3051f6f3a95b" target="_blank">https://support.microsoft.com/en-us/topic/how-to-choose-antivirus-software-to-run-on-computers-that-are-running-sql-server-feda079b-3e24-186b-945a-3051f6f3a95b</a>
<br><img src="../graphics/checkbox.png"> Only required SQL Server components, utilities and features are installed required for securely servicing authorized data and programmatic requests.
<br><img src="../graphics/checkbox.png"> SQL Server is at a currently supported version.
<br><img src="../graphics/checkbox.png"> SQL Server latest Service Packs and/or Cumulative Updates are tested, installed and configuration documents updated.
<br><img src="../graphics/checkbox.png"> SQL Server CLR feature evaluated for necessary and proper use, and disabled if not.
<br><img src="../graphics/checkbox.png"> SQL Server Application Roles documented with applicable applications that use them.
<br><img src="../graphics/checkbox.png"> SQL Server Guest user disabled.
<br><img src="../graphics/checkbox.png"> The SQL Vulnerability Assessment Tool has been run and a baseline report has been created, and a schedule is in place for it to be run periodcially.
<br><img src="../graphics/checkbox.png"> The *Default Trace Enabled* Instance option is enabled.
<br><img src="../graphics/checkbox.png"> A Server Audit, Server Audit Specification and Database Audit Specifications have been evaluated and implemented to the level required by the organization. 
<br><img src="../graphics/checkbox.png"> A full monitoring and alerting systems has been implemented on the server, and has a review process and team appointed. 
<br><img src="../graphics/checkbox.png"> Unless required, and after testing, the *SQLWriter* and *SQLBrowser* services are disabled.
<br><img src="../graphics/checkbox.png"> If the CLR feature is required, the *clr strict security* Instance option is enabled.
<br><img src="../graphics/checkbox.png"> The *Maximum number of error log files* Instance option is set to 10 or higher.
<br><img src="../graphics/checkbox.png"> A full configuration audit of the Operating System and SQL Server Instances has been created, and is updated with Delta reports on a periodic basis. These documents are reviewed by both the Security and Data teams.

*Microsoft Azure SQL DB environments*
<br><img src="../graphics/checkbox.png"> Database backup and other off-DB file storage reviewed for access only by authorised personnel.
<br><img src="../graphics/checkbox.png"> Microsoft Azure Defender for SQL is implemented and Threat Detection is enabled, with a group alias for incident reporting.


<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/Compass.png"><b>Identitiy and Authorization</b></p>
<br>
Identity and Authorization security defines the appropriate *Principals* and checking that they are who they claim using <a href="https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-mfa-howitworks" target="_blank">multifactor authentication</a>[]() and other conditional access systems for infrastructure, code, and change tracking systems.

*SQL Server Installations*
<br><img src="../graphics/checkbox.png"> The *sa* SQL Server account has been disabled or renamed, after testing.
<br><img src="../graphics/checkbox.png"> *Integrated Authentication* is implemented wherever possible. Active Directory or Azure Active Directory is used wherever possible for that integration.
<br><img src="../graphics/checkbox.png"> If SQL Server Authentication is used, the Password and Account settings have been strengthened with Password History, length, complexity, age, and lockout settings to the most restrictive possible.
<br><img src="../graphics/checkbox.png"> The *SQL Server Configuration Manager* utility is used for all Service Account changes.
<br><img src="../graphics/checkbox.png"> All SQL Server Services evaluated and disabled where not required for servicing authorised data requests.
<br><img src="../graphics/checkbox.png"> All SQL Server Services use specific, low-privilege accounts for each service operation, and are periodically reivewed for activity.
<br><img src="../graphics/checkbox.png"> A strong audit and review process is in place for evaluating user access to data and programmatic objects, such as using SQL Server Audit and other utilities.
<br><img src="../graphics/checkbox.png"> A specific audit and review rule is in place for evaluating elevated Principal accounts (such as Instance or Database Administrator effective permissions), and to ensure no applications use an elevated account.
<br><img src="../graphics/checkbox.png"> A specific audit and review rule is in place for evaluating Failed Login attempts.
<br><img src="../graphics/checkbox.png"> The *public* group has no execute access to unnecessary stored procedures, such as extended stored procedures. 
<br><img src="../graphics/checkbox.png"> The *xp_cmdshell* is disabled.
<br><img src="../graphics/checkbox.png"> DBA accounts have been removed from the *sysadmin* role, and **CONTROL SERVER** has been granted to DBA accounts.
<br><img src="../graphics/checkbox.png"> There is a strict, documented and reviewed process for Operating System, Instance, and Database accounts that are no longer active, and access to data-bearing assets are revoked as soon as the user leaves the position or organization. 
<br><img src="../graphics/checkbox.png"> Unless required by auditing or Replication, the *Scan For Startup Procs* Instance option is disabled.
<br><img src="../graphics/checkbox.png"> Unless required, the *Database Mail XPs* Instance option is disabled.
<br><img src="../graphics/checkbox.png"> Unless required, the *Cross DB Ownership Chaining* Instance option is disabled. 
<br><img src="../graphics/checkbox.png"> Unless required, the *Remote Access* Instance option is disabled. 
<br><img src="../graphics/checkbox.png"> Unless required, the *Remote Admin Connections* Instance option is disabled.
<br><img src="../graphics/checkbox.png"> Unless required, the *Trustworthy* Database Property is disabled. Note: The *msdb* System Database requires this configuration be enabled.
<br><img src="../graphics/checkbox.png"> The SQL Agent Proxies have been audited to establish least privilege.

*Microsoft Azure SQL DB environments*
<br><img src="../graphics/checkbox.png"> The highest level of authentication (Azure Active Directory and/or Role-Based Acccess Control) is implemented wherever possible.
<br><img src="../graphics/checkbox.png"> If Kerberos (Active Directory Integration) is used, accounts are regularly reviewed for proper controls and RBAC acccess.
<br><img src="../graphics/checkbox.png"> If SQL Server authentication is used, accounts are regularly reviewed for proper controls, strength, rotation, and object acccess.
 
<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/Compass.png"><b>Application</b></p>

The Application security area involves implementing <a href="https://docs.microsoft.com/en-us/dotnet/standard/security/secure-coding-guidelines" target="_blank">Secure Code</a> practices and policies to prevent security vulnerabilities.

*SQL Server Installations*
<br><img src="../graphics/checkbox.png"> Applications do not use a hard-coded password in the application for database access.
<br><img src="../graphics/checkbox.png"> Applications are developed on a Development Instance, tested on a Testing Instance, and Developers do not have general rights to access Production Instances.
<br><img src="../graphics/checkbox.png"> Development databases do not contain sensitive production data.
<br><img src="../graphics/checkbox.png"> Unless required, the *CLR Enabled* Instance option is disabled.
<br><img src="../graphics/checkbox.png"> Unless required, the *OlE Automation Procedures* Instance option is disabled 
<br><img src="../graphics/checkbox.png"> Unless required, the *Ad Hoc Distributed Queries* Instance option is disabled.
<br><img src="../graphics/checkbox.png"> Each application's input for database access is sanitized to prevent SQL Injection and other attacks.

*Microsoft Azure SQL DB environments*
<br><img src="../graphics/checkbox.png"> Azure SQL Database Audit is run and reviewed periodically.
<br><img src="../graphics/checkbox.png"> Azure SQL Database Vulnerability Report is run and reviewed periodically.
<br><img src="../graphics/checkbox.png"> Azure logs are monitored and reviewed for each Azure SQL DB environment.
 
<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/Compass.png"><b>Data</b></p>

The Data Security Area involves ensuring that business and customer data is encrypted and protected against unwanted access at rest, in-transit, in-memory and in-code processes.

*SQL Server Installations*
<br><img src="../graphics/checkbox.png"> Only system and user databases are installed and configured. All demonstration or sample databases have been removed.
<br><img src="../graphics/checkbox.png"> All databases have been evaluated for encryption requirements, and mechanisms such as Transparent Database Encryption, Always Encrypted, and other technologies are implemented where appropriate.
<br><img src="../graphics/checkbox.png"> The SQL Data Discovery and Classification feature has been evaluated and used to identify and classify sensitive data.
<br><img src="../graphics/checkbox.png"> If Always Encrypted is not enabled, Dynamic Data Masking is configured for applicable columns.
<br><img src="../graphics/checkbox.png"> Row Level Encryption has been evaluated and implemeneted for all sensitive data.
<br><img src="../graphics/checkbox.png"> Any specifically sensitive databases are identified, documented in the configuration documents, and reviewed with the Security and Data teams. If any database is subject to higher restrictions, such as government or organization regulations, the applicable teams are also included in this review.
<br><img src="../graphics/checkbox.png"> Least-privilege role-based security has been implemented for all data access.
<br><img src="../graphics/checkbox.png"> SQL Server Audit features such as Change Tracking, Change Data Capture, and SQL Server Audit have been evaluated and implemented per-database as required.

*Microsoft Azure SQL DB environments*
<br><img src="../graphics/checkbox.png"> Row-Level Security  evaluated and implemented for sensitive data elements.
<br><img src="../graphics/checkbox.png"> Data Masking evaluated and implemented for sensitive data elements.


<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/owl.png"><b>For Further Study</b></p>
<ul>
    <li><a href="https://docs.microsoft.com/en-us/sql/relational-databases/security/sql-server-security-best-practices?view=sql-server-ver16" target="_blank">Official Documentation for this section</a></li>
</ul>
