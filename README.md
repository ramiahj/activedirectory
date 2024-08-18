<p align="center">
<img src="https://i.imgur.com/OLjGPOY.png" alt="osTicket logo"/>
</p>

<h1>Configuring Active Directory with Azure Virtual Machines</h1>
This project focuses on setting up a basic Active Directory (AD) environment with one domain controller and one client machine, both hosted in Azure. The project also serves as a tutorial for installing AD Domain Services, creating user accounts, joining a client machine to the domain, and allowing remote desktop access for domain users. PowerShell is used to create bulk user accounts for testing login functionality.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines, Resource Groups, Virtual Networks)
- Remote Desktop Protocol (RDP)
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10</b>

<h2>Installation Steps</h2>

<h3>Setup Resources in Azure</h3>

<p>
<img src="https://i.imgur.com/tPuN97m.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First, create a Domain Controller Virtual Machine (VM) in Azure using Windows Server 2022. For this project, we will refer to ours as DC-1. Ensure the Network Interface Controller (NIC) for DC-1 is assigned a static private IP address. Following that, create a Windows 10 client VM (ours will be named Client-1) within the same Resource Group and Virtual Network (VNet) as DC-1. This will ensure network communication between the two VMs.
</p>

<h3>Verify Connectivity</h3>

<p>
<img src="https://i.imgur.com/7rsZBTE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ensure connectivity between Client-1 and DC-1. Log in to Client-1 via Remote Desktop and use the endless ping command (ping -t) to test connectivity with DC-1, pinging it's private IP address. You will see that requests are unsuccessul. What we will need to do is log into DC-1 and enable ICMPv4 in Windows Firewall to allow successful pings.

<p>
<img src="https://i.imgur.com/NC0TIKV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Active Directory Installation</h3>

<p>
<img src="https://i.imgur.com/RfiTKx5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On DC-1, navigate to Server Manager, "Add Roles and Features, and install Active Directory Domain Services and promote DC-1 to a Domain Controller by creating a new forest. In this lab, we will use "mydomain.com". Once complete, restart the server and log back in with the new domain and the credentials you created at the beginning. For example, my login username will be "mydomain.com\labuser".
</p>

<h3>User and Admin Account Creation</h3>

<p>
<img src="https://i.imgur.com/ZLbqg6K.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Access Active Directory Users and Computers (ADUC) and create two Organizational Units (OUs): _EMPLOYEES and _ADMINS. Then, create an admin user account, assigning this user to the “Domain Admins” group. From this point forward, log in as mydomain.com\(your admin username) for administrative tasks. For example, my admin login is "mydomain.com/ramiahj"
</p>
<br />

<p>
<img src="https://i.imgur.com/JuNwezt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</p>

<h3>Join Client-1 to the Domain</h3>

<p>
<img src="https://i.imgur.com/P9n3bdc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Update Client-1’s DNS settings in Azure to point to DC-1’s private IP address. Restart Client-1, log in, and join it to the domain. Afterward, verify that Client-1 appears in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.
<p>
<img src="https://i.imgur.com/PTVaCgB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Configure Remote Desktop for Non-Admin Users</h3>

<p>
<img src="https://i.imgur.com/EitDzD3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On Client-1, log in as Admin and configure Remote Desktop to allow “domain users” access. This enables standard users to log in remotely, streamlining the administrative process. To do this, navigate to "System Properties",  click "Remote Desktop", allow domain users access to Remote Desktop. 
</p>
<br />

<h3>Create Additional Users via PowerShell</h3>

<p>
<img src="https://i.imgur.com/i5bWBzt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Using a PowerShell script created by one of my CourseCareers mentors Josh Madakor, we will generate multiple user accounts in the _EMPLOYEES OU. After running the script, log in to Client-1 with one of the new accounts to verify domain access and user functionality (each account's password is "Password1"). Here is the script: (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
</p>
<br />

<h2>Conclusion</h2>


In this lab, we successfully set up a fully functional Active Directory environment in Microsoft Azure. By creating and configuring a Domain Controller and a client VM, installing Active Directory Domain Services, and managing user accounts, we established domain connectivity and demonstrated how to efficiently manage users and resources in a Windows-based domain. This project showcases essential skills in network administration, user management, and Active Directory configuration, which are critical for managing enterprise IT environments.
