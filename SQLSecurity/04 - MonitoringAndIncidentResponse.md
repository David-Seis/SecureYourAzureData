<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/Lock-2.png">

# Workshop: SQL Server Security Ground to Cloud

#### <i>A Security Course For Data Professionals</i>

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/textbubble.png"> <h2>04 - Monitoring and Incident Response</h2>

In <a href="https://github.com/David-Seis/SecureYourAzureData" target="_blank">this workshop</a> you'll cover the basics of securing SQL Server installations and databases, from on-premises systems to Microsoft Azure deployments.

In each module you'll get more references, which you should follow up on to learn more. Also watch for links within the text - click on each one to explore that topic.

(<a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/00%20-%20Pre-Requisites.md" target="_blank">Make sure you check out the <b>Pre-Requisites</b> page before you start</a>. You'll need all of the items loaded there before you can proceed with the workshop.)

This module builds on the previous modules that explained the general security posture of a SQL Server and Microsoft Azure SQL Database environment, and securing your systems against attack. However, security is an ongoing effort. Reviewing your Audits helps you to detect any malicious attempts on your systems in the past - but it is essential that you also have a system to constantly monitor your environment to detect attacks as they happen. 

Security  breaches are always possible even with the best defenses, so it is important to have a comprehensive, logical, verified incident response plan in place at all times for you system. Those additional elements are the focus of this module. 

You'll cover these topics in this module:
<ul>
  <li><a href="#01" target="_blank">01 - Leveraging your Audits</li></a>
  <li><a href="#01" target="_blank">02 - Working with Microsoft Defender for SQL</li></a>
  <li><a href="#02" target="_blank">03 - Implementing an Incident Response Plan</li></a>
</ul>

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 1 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="01"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">01 - Leveraging your Audits</h2>
<br>
Earlier in this workshop you created a SQL Server and Azure SQL DB Audit. In the case of SQL Server installations, the primary components of an Audit are the <i>Server Audit</i>, the <i>Server Audit Specification</i>, and the <i>Database Audit Specification</i>. You learned that the Server Audit information is stored in the <i></i>master</i> system database, which sets the file rollover, the queue delay and the error handling for the Audit process.

The Audit target can be set to several locations. If you use a <i>File</i> target, you should ensure that this is an off-server, secure location. Using the Windows Event logs is another option, and you should bias towards the Security Event Log since it has higher protections than the other Event Log types.

An Audit is a historic reference for what has occurred on your system based on the data you chose to collect when you created your Audit Specifications. This history requires periodic review to ensure you are tracking the latest activity on the environment.
<br>
<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Review Your Audits</b></h4>
<br>
In this Activity you will Locate your Server Audits and review the information you see there. 

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>
<ol type="1">
  <li> Locate your Server Audit. This can be done using the <a href="https://docs.microsoft.com/en-us/sql/relational-databases/security/auditing/view-a-sql-server-audit-log?view=sql-server-ver16#SSMSProcedure">SQL Server Management Studio tool</a>, or by using The <a href="https://docs.microsoft.com/en-us/sql/relational-databases/system-catalog-views/sys-server-audits-transact-sql?view=sql-server-ver16">sys.server_audits Transact-SQL</a> statement for instalations of SQL Server. 
  <li>If you set the target to the Event logs, use the Event Log tool in Windows to review the log. If you set the Audit target toa file, use the <a href="https://docs.microsoft.com/en-us/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql?view=sql-server-ver16">sys.fn_get_audit_file Transact-SQL</a> or a text editor to review the contents.</li>
  <li> For your Azure SQL DB Audits, use the <a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/auditing-overview?view=azuresql#subheading-3">Microsoft Azure Portal to locate and review your Audits</a>.</li>
</ol>

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 2 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="01"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">02 - Working with Microsoft Defender for SQL</h2>
<br>
Microsoft Azure Defender for Cloud <a href="https://docs.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction">is the primary security tool from Microsoft for not only Azure-based assets, but also on-premises systems and even other Cloud provider's environments</a>.  It provides what the industry calls a "Cloud Security Posture Management" (CSPM) and "Cloud Workload Protection Platform" (CWPP) system. The primary aspects are assessing your systems, securing your systems, and reacting to attacks on your systems.

In larger organizations, it is common to have a security team that implements a comprehensive organization security posture. In smaller organizations, that function may fall to each member of the IT team. If your organization has a security team, then you should work with them to extend the systems they have to covering your Data estate.

<a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/azure-defender-for-sql?view=azuresql">Microsoft Defender for SQL is a separate plan within Microsoft Defender for Cloud</a>. Microsoft it asseses potential database vulnerabilities, detects anomalous activities, and has a full reporting system that also surfaces these reports up to the larger Microsoft Defender for Cloud system. It's two main components are performing a Vulnerability Assessment, and enabling Advanced Threat Protection to react to threats, in some cases, pre-emptively.

To enable Microsoft Defender for SQL, you can use the Microsoft Azure Portal, the Azure CLI, or PowerShell. In a previous activity, you enabled this feature for your Azure based system. To enable Microsoft Defender for SQL Servers on local systems, you install an Agent Extension on the system, provision the Log Analytics agent on your SQL Server's host, and then enable the optional plan in Defender for Cloud's environment settings page. From there, you can review the logs and alerts it creates. 
<br>
<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Enabling Microsoft Defender for SQL servers on local or cloud SQL Server Installations</b></h4>
<br>
In this Activity you will review the steps to install and configure the Agent and Azure environment for a SQL Server Installation. You will start by reviewing the process, and if time permits, install and run the agent on your test system.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>
<ol type="1">
  <li> <a href="https://docs.microsoft.com/en-us/azure/defender-for-cloud/defender-for-sql-usage#set-up-advanced-data-security-for-sql-machines">Navigate to this reference</a>, and review the information <a href="https://www.youtube.com/watch?v=V7RdB6RSVpc">you see there</a>.</li>
  <li> If time permits, implement the steps on your classroom system, using the Azure account you have been using for this workshop.</li>
</ol>

<p style="border-bottom: 1px solid lightgrey;"></p>

[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Section 3 =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)

<h2 id="01"><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/pencil2.png">03 - Implementing an Incident Response Plan</h2>
<br>
An Incident Response Plan is a pro-active set of documentation that defines the steps your organization will take based on the monitoring and alerting you have set up throughout this course. Some of these actions are pre-emptory, such as "we will do X when the log shows any attempt to access sensitive resources" and some are reactionary, such as "we will do Y when we detect a breach of our systems". In any case, this should be well-documented, reviewed, and communicated not just within the Security or Information Technology teams,  but all throughout the business.

You can find many examples of Security Incident Response Plans, some that are specific to your industry. Keep in mind that if you are in an industry that is covered by a government or other regulatory body, you should consult the specifications for those regulations and incorporate them into your response plan to ensure compliance.  
<br>
<h4><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity: Incident Response Plan</b></h4>
<br>
In this Activity you will review the steps to create or evaluate a complete Incident Response Plan. If you have a security team, you should consult them to add the Data estate coverage you control, and if your organization does not have a defined Security team, it is advisable to bring in outside assistance to help your organization create a security posture. They will often use an Incident Response Plan to create the posture.

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/checkmark.png"><b>Steps</b></p>
<ol type="1">
  <li> Navigate <a href = "https://docs.microsoft.com/en-us/security/compass/incident-response-overview">to this reference</a>, and review the documents you see there. Bookmark this location to develop the proper level of Incident Response for your organization in cooperation with your SecOps team. 
</ol>

<p style="border-bottom: 1px solid lightgrey;"></p>


[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= Closing   =========================================================)
[//]: <> (================================= ========= =========================================================)
[//]: <> (================================= ========= =========================================================)
<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/owl.png"><b>For Further Study</b></p>
<ul>
    <li><a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/Security%20Audit%20Notebook.ipynb" target="_blank">You can find a Jupyter Notebook on this course with the Activity code from this course here.</a> You can use this resource as the basis for further exploring your test environment, and this Notebook can be used as the basis for your production envirnment, after testing and validation with your team. You can <a href="https://docs.microsoft.com/en-us/sql/azure-data-studio/notebooks/notebooks-guidance?view=sql-server-ver16">learn how to use a Jupyter Notebook with Azure Data Studio here</a>.</li>
    <li><a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/Security%20Checklist%20Template.md" target="_blank">You can find a sample Security Checklist here</a> which you can use as the basis for creating a comprehensive check on your environments, after testing and validitation with your team.</li>
    <li>You can review the <a href="https://www.microsoft.com/security/blog/2021/05/20/simuland-understand-adversary-tradecraft-and-improve-detection-strategies/">complete Simuland environment and resources</a> to understand the complexity of a security footprint</li>.
</ul>

Congratulations! You have completed this workshop. You now have the tools, assets, and processes you need to extrapolate this information into other applications.