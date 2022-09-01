<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/Lock-2.png">

# Workshop: SQL Server Security Ground to Cloud

#### <i>A Security Course For Data Professionals</i>

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/textbubble.png"> <h2>About this Workshop</h2>

Welcome to this workshop on [*SQL Server Security Ground to Cloud*](https://github.com/David-Seis/SecureYourAzureData). In this workshop, you'll learn about the computing security landscape, and the Data Professional's responsibilities within it. You'll also learn the latest security frameworks and paradigms, SQL Server security operations both on-premises and in-cloud, and the steps you should secure your organization's data.  

The focus of this workshop is to enable the Data Professional to secure and protect their data estate.

You'll start by creating a workshop enviroment using your own laptop or Virtual machine, then work through a module covering general security principles and where the data estates security measures and controls fit within that environment at your organization. From there, you will work through a hands-on module covering the security basics of on-premises and Virtual-Machine based installations of SQL Server. The next module covers the similarities and differences between an installation of SQL Server and the Azure SQL Database Environment. The final module covers monitoring your data environment and creating an Incident Response Plan for your organization, all with a focus on how to extrapolate what you have learned to create other solutions for your organization.

> This course does not currently focus on securing a Linux, Docker Container, or Kubernetes installation of SQL Server. This workshop also does not cover a highly-secure regulatory environment (such as C2 compliance), although most all of the concepts in this workshop are useful as a starting point to securely operate Microsoft SQL Server on those platforms and in those environments.

This [github README.MD file](https://lab.github.com/githubtraining/introduction-to-github) explains how the workshop is laid out, what you will learn, and the technologies you will use in this solution. To download this Lab to your local computer, click the **Clone or Download** button you see at the top right side of this page. [More about that process is here](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository). 

> It's best to right-click any links you see and select "Open in new Tab" for easier navigation.

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/checkmark.png"> <h3>Learning Objectives</h3>

In this workshop you'll learn:
<br>

- The general computing security landscape and the data estate's responsibilities in that environment
- The basics of security for on-premises and in-Virtual Machine SQL Server installations
- The basics of Microsoft Azure SQL Database security
- How to monitor for and react to security incidents in your organization

In addition, you will recieve a [baseline Security Checklist to use as a starting point for a Defense-In-Depth check](https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/Security%20Checklist%20Template.md) of your on-premises and in-cloud environments.  

The goal of this workshop is to train technical professionals in the basics of SQL Server security both on-premises and in-cloud. 

The concepts and skills taught in this workshop form the starting points for:

 - Technical professionals tasked with securing a data estate 
 - Data professionals tasked with complete or partial responsibility for data security
 - Data Security team members who are not familiar with SQL Server security controls and auditing mechanisms

<p style="border-bottom: 1px solid lightgrey;"></p>
<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/building1.png"> <h2>Business Applications of this Workshop</h2>

Businesses require a high level of security on their most fundamental asset: organizational data. Data breaches are costly, disruptive, and can both financially and structurally negatively impact the organization. Having trained professionals that understand the controls and mechanisms to secure that data is fundamental to the business' survival.

Some industries require an even higher level of security, and are subject to regulatory and government compliance standards, such as healthcare, military, banking and government services. The professionals tasked with securing these environments need the highest level of training to ensure this compliance.

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/listcheck.png"> <h2>Technologies used in this Workshop</h2>

This workshop uses the following technologies - although you are not limited to these, they form the basis of the workshop. At the end of the workshop you will learn how to extrapolate these components into other solutions. You will cover these at an overview level, with references to much deeper training provided.

 <table style="tr:nth-child(even) {background-color: #f2f2f2;}; text-align: left; display: table; border-collapse: collapse; border-spacing: 2px; border-color: gray;">

  <tr><th style="background-color: #1b20a1; color: white;">Technology</th> <th style="background-color: #1b20a1; color: white;">Description</th></tr>
    <tr><td>Microsoft Windows Operating System</td><td>This workshop uses the Microsoft Windows operating system. You will navigate, install, and configure software, settings, and user components during the workshop.</td></tr>
    <tr><td>Microsoft Azure Cloud Environment</td><td>In this workshop you will create an Azure SQL Database environment and enable and use the Microsoft Defender for SQL products.</td></tr>
    <tr><td>SQL Server on-premises and in-VM installations</td><td>SQL Server is installed, configured, and audited on a student-supplied environment such as a laptop or Virtual Machine.</td></tr>
    <tr><td>General Security Tools and Processes</td><td>Computing hardware, networking, and configurations as they partain to security are used throughout the workshop.</td></tr>
</table>

> Although SQL Server is supported on Linux, Containers, and Orchestration Platforms such as Kubernetes, those platforms are not currently covered in this workshop. 

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/owl.png"> <h2>Before Taking this Workshop</h2>

You'll need a local system that you are able to install software on, completely format, and start over again. The workshop demonstrations use Microsoft Windows as an operating system and all examples use Windows for the workshop. Optionally, you can use a local or Microsoft Azure Virtual Machine (VM) to install the software on and use for the workshop. A free Virtual Machine image is provided for you using Hyper-V, VirtualBox, or Parallels in the pre-requisites.

You should have a Microsoft Azure account with the ability to create assets. (A free subscription is available, and described in the Pre-Requisite section)

This workshop expects that you understand the basics of the technologies you will use. If you are new to these, here are a few references you can complete prior to class:

-  [Microsoft Windows](https://docs.microsoft.com/en-us/learn/browse/?expanded=windows&products=windows)
-  [Microsoft Azure](https://docs.microsoft.com/en-us/learn/paths/az-900-describe-cloud-concepts/)
-  [SQL Server](https://docs.microsoft.com/en-us/sql/sql-server/tutorials-for-sql-server-2016?view=sql-server-ver16)
-  [Computing Security](https://www.microsoft.com/en-us/security/content-library/Home/Index?culture=en-US)

<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/bulletlist.png"> <h3>Setup</h3>

<a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/00%20-%20Pre-Requisites.md" target="_blank">A full pre-requisites document is located here</a>. These instructions should be completed before the workshop starts, since you will not have time to cover these in class. 

> Remember to turn off any Virtual Machines from the Azure Portal when not taking the class so that you do incur charges (shutting down the machine in the VM itself is not sufficient)

<p style="border-bottom: 1px solid lightgrey;"></p>

<img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/bookpencil.png"> <h2>Workshop Modules</h2>

This is a modular workshop, and in each section, you'll learn concepts, technologies and processes to help you complete the solution.

<table style="tr:nth-child(even) {background-color: #f2f2f2;}; text-align: left; display: table; border-collapse: collapse; border-spacing: 5px; border-color: gray;">
  <tr><td style="background-color: AliceBlue; color: black;"><b>Module</b></td><td style="background-color: AliceBlue; color: black;"><b>Topics</b></td></tr>

  <tr><td><a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/00%20-%20Pre-Requisites.md" target="_blank">00 - Pre Requisites </a></td><td> Covers the materials and tools you need, as well as the knowledge you need prior to taking this course.</td></tr>
  <tr><td style="background-color: AliceBlue; color: black;">  <a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/01%20-%20SecurityLandscape.md" target="_blank">01 - The Database Security Landscape</a> </td>
      <td td style="background-color: AliceBlue; color: black;"> Explains the general Information Technology areas, and also frameworks for IT security. Covers the Database portion of those security areas.</td></tr>
  <tr><td><a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/02%20-%20SQLServerSecurityBasics.md" target="_blank">02 - SQL Server Security</a></td>
      <td> Details the primary components and tools for on-premises/Virtual Machine SQL Server security environments, from Principals and Securables to Data Control Language (DDL) statements.</td></tr>
  <tr><td style="background-color: AliceBlue; color: black;"><a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/03%20-%20SQLAzureSecurity.md" target="_blank">03 - Microsoft Azure SQL Security </a> </td>
      <td td style="background-color: AliceBlue; color: black;"> Explains the similarities and differences between the data structures, statements, tools and processes to sure your Azure SQL databases in the Microsoft Azure Platform. </td></tr>  <tr>
  <td><a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/04%20-%20MonitoringAndIncidentResponse.md" target="_blank">04 - Monitoring and Mitigations</a></td>
      <td> Covers the tools and processes you can use both on-premises and in-cloud to detect and secure your data estate. Also explains some of the common threats and mitigations for those threats to databases. </td></tr>
</table>

<p style="border-bottom: 1px solid lightgrey;"></p>

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="https://raw.githubusercontent.com/microsoft/sqlworkshops/master/graphics/geopin.png"><b>Next Steps</b></p>

Next, Continue to <a href="https://github.com/David-Seis/SecureYourAzureData/blob/main/SQLSecurity/00%20-%20Pre-Requisites.md" target="_blank"><i> Pre-Requisites</i></a>

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

# Legal Notices

### License
Buck Woody and David Seis grant you a license to the content in this repository under the [Creative Commons Attribution 4.0 International Public License](https://creativecommons.org/licenses/by/4.0/legalcode), see [the LICENSE file](https://github.com/David-Seis/SecureYourAzureData/blob/main/LICENSES/LICENSE), and grant you a license to any code in the repository under [the MIT License](https://opensource.org/licenses/MIT), see the [LICENSE-CODE file](https://github.com/David-Seis/SecureYourAzureData/blob/main/LICENSES/LICENSE-CODE).

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at http://go.microsoft.com/fwlink/?LinkID=254653.

Privacy information can be found at https://privacy.microsoft.com/en-us/

Microsoft and any contributors reserve all other rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.
