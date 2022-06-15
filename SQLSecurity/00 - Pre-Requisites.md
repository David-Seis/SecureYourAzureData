<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/Lock-2.png">

# Workshop: SQL Server Security Ground to Cloud

#### <i>A Security Course For Data Professionals</i>

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/textbubble.png"><h2>00 Pre-Requisites</h2>

The <a href="https://github.com/David-Seis/SecureYourAzureData" target="_blank">SQL Server Security Ground to Cloud workshop</a> ues the following components. Read through this list, and in the Activities that follow you will see specific steps for each installation. You can also simply read through each of these steps, and observe the activities in the workshop if you cannot install the hands-on poritions.

- **A Microsoft Windows 10 or Higher Workstation**: For this workshop, you will use a Microsoft Windows system as the base workstation, although Apple and Linux operating systems can be used in production.
- **Microsoft Azure**: This workshop uses the Microsoft Azure platform for the cloud database and the Microsoft Defender products. You can also host your workstation there if desired. You can use a free Azure account, an MSDN Account, your own account, or potentially one provided for you, as long as you can create about $100.00 (U.S.) worth of assets.
- **SQL Server Developer Edition**: An installation of the SQL Server Developer Edition, the Database Engine feature.
- **Microsoft Azure SQL DB**: A Microsoft Azure SQL Database (smallest edition) allows for testing and exploration of SQL Server security on that platform.
- **Microsoft Azure Defender Account**: This is the primary tool from Microsoft for securing and reporting on security for your on-premises and in-cloud environments.
- **Python**: A simple set of Create, Read, Update and Delete (CRUD) applications to show traffic to and from the on-premises and in-cloud environments.

> All of the following activities must be completed **prior** to class - there will not be time to perform these operations during the workshop. The complete pre-requisites steps will take from 2-6 hours.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity 1: Set up a Microsoft Azure Account</b></p>

You have multiple options for setting up Microsoft Azure account to complete this workshop. You can use a free account, a Microsoft Developer Network (MSDN) account, a personal or corporate account, or in some cases a pass may be provided by the instructor. (For most classes, the MSDN account is best)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/checkbox.png"><b>Option 1 - Free Account</b></p>

The free account gives you twelve months of time, and a limited amount of resources. Set this up prior to coming to class, and ensure you can access it from the system you will bring to the class.

- <a href="https://azure.microsoft.com/en-us/free/" target="_blank">Open this resource, and click the "Start Free" button you see there.</a>

> You can only use the Free subscription once, and it expires in 12 months.

<p><b>Option 2 - Microsoft Developer Network Account (MSDN) Account</b></p>

The best way to take this workshop is to use your [Microsoft Developer Network (MSDN) benefits if you have a subscription](https://marketplace.visualstudio.com/subscriptions).

- <a href="https://azure.microsoft.com/en-us/pricing/member-offers/credit-for-visual-studio-subscribers/" target="_blank">Open this resource and click the "Activate your monthly Azure credit" button.</a>

<p><b>Option 3 - Use Your Own Account</b></p>

You can also use your own account or one provided to you by your organization, but you must be able to create a resource group and create, start, and manage a Workstation. 

<p><b>Option 4 - Use an account provided by your instructor</b></p>

Your workshop invitation may have instructed you that they will provide a Microsoft Azure account for you to use. If so, you will receive instructions that it will be provided.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity 2: Create a Workstation and Install SQL Server Developer Edition and the Sample Application</b></p>
<br>

You will use a Windows 10 or higher workstation for this course. After you complete that installation, you will install SQL Server 2019 on this workstation, along with the SQL Server Management Studio tool, and a sample application.  You can use a local Virtual Machine, or a physical workstation in a test configuration that can be reformatted when necessary.

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/checkbox.png"><b>Create your own Workstation</b></p> 

> This course will cover some of the security features of SQL Server 2022, in a demonstration fashion. If you would like to install SQL Server 2022 instead of SQL Server 2019, <a href="https://info.microsoft.com/ww-landing-sql-server-2022.html?culture=en-us&country=US" target="_blank">check this reference.</a>

You can create your workstation for this course using **one** of the following methods:

- Local Workstation - You can use a physical workstation for this course, assuming you can install software on that system and you have complete control of the administration account. **This needs to be a system you could format and start over multiple times, so do not use your work or production workstation for this course.** This system should have a minimum of 8GB of RAM and 150GB drive space free.
- Local Virtual Machine - You can <a href="https://developer.microsoft.com/en-us/windows/downloads/virtual-machines" target="_blank">download a Windows 10 Workstation Image for VirtualBox, Hyper-V, VMWare, or Parallels for free here</a>, or you can use your own installation media.</a> This system should have a minimum of 8GB of RAM and 150GB drive space free. 
- <a href="https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal" target="_blank">Microsoft Azure Virtual Machine.</a> Select a system that has at least 2 processors, a minimum of 8GB of RAM and 150GB drive space free. Ensure you create all assets for this course in a single Resource Group, so that you can delete all these assets when you complete the workshop. **Do not use your organization's production subscription for this workshop.**
- Another Cloud Provider's Virtual Machine Environment. See the provider's documentation for this process. This system should have a minimum of 8GB of RAM and 150GB drive space free. **Do not use your organization's production subscription for this workshop.**

> If you use a Microsoft Azure Virtual Machine,  **ensure that you "Stop" the VM in the Portal to ensure that you do no exceed the cost limits on this account. Simply shutting down the Virtual Machine will continue to cost you.** <a href="https://build5nines.com/properly-shutdown-azure-vm-to-save-money/" target="_blank">You can read more about properly stopping a Microsoft Azure Virtual Machine here.</a>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/checkbox.png"> Apply Operating System Updates</b></p>

Next, ensure all of your updates are current. You can use <a href="https://support.microsoft.com/en-us/windows/update-windows-3c5ae7fc-9fb6-9af1-1984-b5e0412c556a#WindowsVersion=Windows_10" target="_blank">the Windows Update</a> graphical tool <a href="https://www.parallels.com/blogs/ras/powershell-windows-update/" target="_blank">or PowerShell.</a>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/checkbox.png"> Install SQL Server Database Engine</b></p>

In this activity, you will install SQL Server, selecting at least the "Database Engine" Feature. If you select more than the Database Engine components, <a href="https://docs.microsoft.com/en-us/sql/database-engine/install-windows/install-sql-server?view=sql-server-ver16" target="_blank">you will have more configuration options you will need to consider</a>. For this course, we will focus on the Database Engine feature, so that is all you need to install. You can <a href="https://docs.microsoft.com/en-us/sql/database-engine/install-windows/add-features-to-an-instance-of-sql-server-setup?view=sql-server-ver16" target="_blank">add more features later by using the Setup Center</a> on your system.

> For the next two steps, <a href="https://www.youtube.com/watch?v=KZtHbq_Ar_Y" target="_blank">you can see a walkthrough video of this process here.</a>

- <a href="https://www.microsoft.com/en-us/sql-server/sql-server-downloads?rtc=1" target="_blank">Navigate to this page, and select the "Developer" Link you see below the main options.</a>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/checkbox.png"> Install SQL Server Management Studio</b></p>

- <a href="https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16" target="_blank">Navigate to this page, and install SQL Server Management Studio.</a>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/checkbox.png"> Install the NodeJS Sample Application</b></p>

You do not need to be a developer to take this course, but having an application to use for SQL Server makes it more "real world" and what you will face in production. The steps below are comprehensive and simple to implement. This course uses a <a href="https://nodejs.org/en/" target="_blank">NodeJS sample application</a>.

- <a href="https://docs.chocolatey.org/en-us/choco/setup#install-with-powershell.exe" target="_blank">Navigate to this page and install the "Chocolatey " application using PowerShell that you will use for other installations on your test system.</a>

- Now open an elevated permissions (run as Administrator) PowerShell command-line and create a directory for the course:

<pre>
cd \
mkdir c:\SampleDBApp
cd \SampleDBApp
</pre>

- In that same window, install Python using Chocolatey:
<pre>
choco install -y python
</pre>

- In that same window, install the package manager for Python, and use that to install the connection code for SQL Server:
<pre>
python -m pip install -U pip
python -m pip install pyodbc
</pre>

- In that same window, Start the Notepad program to create the first iteration of the sample application:
<pre>
notepad SimpleConnection.py
</pre>
Answer "Y" to create a new file, and paste the following text in the file, then save and close it. You will alter this file later.

<pre>
import pyodbc 
cnxn = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server};Server=(local);Database=master;Trusted_Connection=yes;Application Name=DBConnectionAppFromPython;')
cursor = cnxn.cursor()

# Connect and return version information
cursor.execute("SELECT @@version;") 
row = cursor.fetchone() 
while row: 
    print(row[0])
    row = cursor.fetchone()
</pre>

*You can test this application with the following command if desired:*
<pre>
python SimpleConnection.py
</pre>

<br>
 
> If you are using an Azure Virtual Machine, when you are done with the installation and with each time period of lab exercises, shut down the Virtual Machine from the Microsoft Azure Portal by selecting "Stop" in the Virutal Machine Panel. Simply shutting down the Virtual Machine using the Power Off feature in the operation system does not release the assets and you are charged until the machine is Stopped in the Portal. [https://build5nines.com/properly-shutdown-azure-vm-to-save-money/](https://build5nines.com/properly-shutdown-azure-vm-to-save-money/) 
    
<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity 3: Create a Microsoft Azure SQL Database</b></p>
<br>

The instructions that follow use the Microsoft Azure Account you created earlier. If you created a Windows Virtual Machine in Microsoft Azure, use the same Resource Group for these steps.

- <a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/single-database-create-quickstart?view=azuresql&tabs=azure-portal" target="_blank">Navigate to this resource, and follow the steps you see there.</a>

<br>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/point1.png"><b>Activity 4: Create a Microsoft Azure Defender for SQL Setup</b></p>
<br>

The instructions that follow use the Microsoft Azure Account you created earlier. If you created a Windows Virtual Machine in Microsoft Azure, use the same Resource Group for these steps.

- <a href="https://docs.microsoft.com/en-us/azure/azure-sql/database/azure-defender-for-sql?view=azuresql" target="_blank">Navigate to this resource, and follow the steps you see there.</a>

<br>

<p><img style="margin: 0px 15px 15px 0px;" src="../graphics/owl.png"><b>For Further Study</b></p>
<ul>
    <li><a href="https://docs.microsoft.com/en-us/sql/database-engine/install-windows/install-sql-server?view=sql-server-ver16" target="_blank">Official Documentation for this section</a></li>
</ul>

You now have a testing and classroom environment for this course. You will add more to this environment as you progress through the modules. 

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="../graphics/geopin.png"><b >Next Steps</b></p>

Next, Continue to <a href="[url](https://github.com/David-Seis/SecureYourAzureData/blob/Buck/SQLSecurity/01%20-%20SecurityLandscape.md)" target="_blank"><i> 01 - The Database Security Landscape</i></a>.
