<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/Lock-2.png">

# Workshop: SQL Server Security Ground to Cloud

#### <i>A Security Course For Data Professionals</i>

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/textbubble.png"> <h2>00 Pre-Requisites</h2>

The "SQL Server Security Ground to Cloud" workshop is taught using the following components, which you will install and configure in the sections that follow in this module.

For this workshop, you will use Microsoft Windows as the base workstation, altough Apple and Linux operating systems can be used in production. You can <a href="https://developer.microsoft.com/en-us/windows/downloads/virtual-machines" target="_blank">download a Windows 10 Workstation Image for VirtualBox, Hyper-V, VMWare, or Parallels for free here</a>.

The other requirements are:

- **Microsoft Azure**: This workshop uses the Microsoft Azure platform to host Virtual Machines for the servers and workstations. You can use a free Azure account, an MSDN Account, your own account, or potentially one provided for you, as long as you can create about $100.00 (U.S.) worth of assets.
- **SQL Server Developer Edition**: Installation of SQL Server Developer Edition is free and this system can also host the application code and other utilities if desired. 
- **Microsoft Azure SQL DB**: A Microsoft Azure SQL Database (smallest edition) allows for testing and exploration of SQL Server security on that platform.
- **Microsoft Azure Defender Account**: This is the primary tool from Microsoft for securing and reporting on security for your on-premises and in-cloud environments. 
- **NodeJS Application**: A simple Create, Read, Update and Delete (CRUD) application to show traffic to and from the on-premises and in-cloud environments. 


*Note that all following activities must be completed prior to class - there will not be time to perform these operations during the workshop.*

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity 1: Set up a Microsoft Azure Account</b></p>

You have multiple options for setting up Microsoft Azure account to complete this workshop. You can use a free account, a Microsoft Developer Network (MSDN) account, a personal or corporate account, or in some cases a pass may be provided by the instructor. (Note: for most classes, the MSDN account is best)

> *Unless you are explicitly told you will be provided an account by the instructor in the invitation to this workshop, you must have your Microsoft Azure account and Virutal Machine Workstation set up before you arrive at class. Instructions are below.*

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/checkbox.png"><b>Option 1 - Free Account</b></p>

The free account gives you twelve months of time, and a limited amount of resources. Set this up prior to coming to class, and ensure you can access it from the system you will bring to the class.

- [Open this resource, and click the "Start Free" button you see there](https://azure.microsoft.com/en-us/free/)

> *NOTE: You can only use the Free subscription once, and it expires in 12 months. Set up your account and create the Workstation per the instructions below, but **ensure that you turn off the VM in the Portal to ensure that you do no exceed the cost limits on this account**. You will turn it off and on in the classroom per the instructor's directions.*

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/checkbox.png"><b>Option 2 - Microsoft Developer Network Account (MSDN) Account</b></p>

The best way to take this workshop is to use your [Microsoft Developer Network (MSDN) benefits if you have a subscription](https://marketplace.visualstudio.com/subscriptions).

- [Open this resource and click the "Activate your monthly Azure credit" button](https://azure.microsoft.com/en-us/pricing/member-offers/credit-for-visual-studio-subscribers/)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/checkbox.png"><b>Option 3 - Use Your Own Account</b></p>

You can also use your own account or one provided to you by your organization, but you must be able to create a resource group and create, start, and manage a Workstation. 

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/checkbox.png"><b>Option 4 - Use an account provided by your instructor</b></p>

Your workshop invitation may have instructed you that they will provide a Microsoft Azure account for you to use. If so, you will receive instructions that it will be provided.

> *Unless you received explicit instructions in your workshop invitations, you much create either a free, MSDN or Personal account. You must have an account **prior** to the workshop.*

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity 2: Create a Workstation and Install SQL Server Developer Edition withh all features and defaults</b></p>
<br>

In this activity you will install SQL Server Developer Edition either on a Microsoft Azure Virtual Machine, or on a system you own that you can make changes to. Your system should have at least 8GB RAM and 150GB space available. 

> NOTE: All installations should be done on a system you have complete control over. These systems should not be part of your production or testing environments.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/checkbox.png">(Optional) Create a Microsoft Azure Virtual Machine<p>

- Open this reference, and follow the instructions you see there. Select a Virtual Machine that has at least 8GB of RAM and 150GB drive space. You can select Windows 10 or higher, or any version of Windows Server desired: [https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal)
    
- Log in to that system and use Windows Update to bring it up to date. [https://support.microsoft.com/en-us/windows/update-windows-3c5ae7fc-9fb6-9af1-1984-b5e0412c556a#WindowsVersion=Windows_10](https://support.microsoft.com/en-us/windows/update-windows-3c5ae7fc-9fb6-9af1-1984-b5e0412c556a#WindowsVersion=Windows_10)
    
- After you update the system, download SQL Server 2022 Evaluation from here, and install the Developer Edition with all options: [https://www.microsoft.com/en-us/evalcenter/download-sql-server-2022](https://www.microsoft.com/en-us/evalcenter/download-sql-server-2022) You will also need to install SQL Server Management Studio by following the process shown on this reference: [https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16) 
    
 *You may install SQL Server 2019 Developer Edition if desired, but the exercises for SQL Server 2022 will not work on that version*
    
- Next, you'll install the Sample Application we will use for the this course. Open this reference and follow the instructions you see there. You do not need to run the applicaiton at this time. [https://www.microsoft.com/en-us/sql-server/developer-get-started/node/windows/](https://www.microsoft.com/en-us/sql-server/developer-get-started/node/windows/)

<br>
 
> NOTE: When you are done with the installation and with each time period of lab exercises, shut down the Virtual Machine from the Microsoft Azure Portal by selecting "Stop" in the Virutal Machine Panel. Simply shutting down the Virtual Machine using the Power Off feature in the operation system does not release the assets and you are charged until the machine is Stopped in the Portal. [https://build5nines.com/properly-shutdown-azure-vm-to-save-money/](https://build5nines.com/properly-shutdown-azure-vm-to-save-money/) 
    
<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity 3: Create a Microsoft Azure SQL Database</b></p>
<br>

In this Activity you will create a Serverless Microsoft Azure SQL DB for use in this course. 
- Open this reference, and follow all the steps you see there: [https://docs.microsoft.com/en-us/azure/azure-sql/database/single-database-create-quickstart?view=azuresql&tabs=azure-portal](https://docs.microsoft.com/en-us/azure/azure-sql/database/single-database-create-quickstart?view=azuresql&tabs=azure-portal)

<br>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity 4: Create a Microsoft Azure Defender for SQL Setup</b></p>
<br>

The instructions that follow explain how to create and enable a Microsoft Defender for Azure SQL and SQL Server installations. 

- Open this reference, and follow the instructions you see there for your Windows and Microsoft Azure SQL Server Databases: [https://docs.microsoft.com/en-us/azure/defender-for-cloud/defender-for-sql-usage](https://docs.microsoft.com/en-us/azure/defender-for-cloud/defender-for-sql-usage)

<br>

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/owl.png"><b>For Further Study</b></p>
<ul>
    <li><a href="[url](https://docs.microsoft.com/en-us/azure/defender-for-cloud/)" target="_blank">Official Documentation for Microsoft Defender for Cloud</a></li>
</ul>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/geopin.png"><b >Next Steps</b></p>

Next, Continue to <a href="[url](https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/01%20-%20SecurityLandscape.md)" target="_blank"><i> 01 - The Database Security Landscape</i></a>.
