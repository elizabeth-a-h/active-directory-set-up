# active-directory-set-up

(((((EDITING IN PROCESS))))))

Active Directory is a Microsoft directory service that runs on a Windows Server and allows administrators to manage resources, assign permissions and control access to network resources within an organization


The purpose of this project is to set up and configure an on-premises Active Directory within Azure VMs.

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/EuKuzGF.png" height="80%" width="80%" alt="Create Domain Controller"/>
</p>
<p>
Step 1: Set up Resources in Azure
  
- Create the Domain Controller VM (Windows Server 2022) named “DC-1”
   - Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
- Set Domain Controller’s NIC Private IP address to be static
- Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
- Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

</p>
<br />
<hr>

<p>
<img src="https://i.imgur.com/CBNCkGM.png" height="80%" width="80%" alt="NIC to Static"/>
</p>
<p>
Step 2. Ensure Connectivity between the client and Domain Controller
  
- Log in to the Domain Controller and enable ICMPv4 in on the local windows Firewall
  - Login to the Domain Controller in the Remote Desktop
  - Open Windows Defender Firewall
  - Select "Advanced Settings" on Left
  - Select "Inbound Rules"
  - Sort by "Protocol"
  - Enable ICMPv4 rules
- Log in to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping) to verify connectivity
</p>
<br />
<hr>

<p>
Step 3. Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created with DC-1
Ensure that both VMs are in the same Vnet 
</p>
<br /> <hr>

<p>
<img src="https://i.imgur.com/CBNCkGM.png" height="80%" width="80%" alt="NIC to Static"/>
</p>
<p>
Step 5. Check back at Client-1 
Ping DC-1's private IP address again to see the ping succeed, ensuring connectivity between the Client and Domain Controller.


</p>
<br />
<hr>

<p>
<img src="https://i.imgur.com/9aAxXeI.png" height="80%" width="80%" alt="NIC to Static"/>
</p>
<p>
Step 6. Install Active Directory

  Login to DC-1 through Remote Desktop
Install Active Directory Domain Services: 
In the Server Manager, Select "Add Roles and Features"
Select Next, Next, Next, 
Select "Active Directory Domain Services"
"Add Features"; "Next"; "Next"; "Next"; "Install"; "Close"


</p>
<br />
<hr>

<p>
<img src="https://i.imgur.com/RGA5XH7.png" height="50%" width="50%" alt="New Forest Set Up"/><img src="https://i.imgur.com/uGcernN.png" height="50%" width="50%" alt="Promote server to Domain Controller"/>
</p>
<p>
Step 7. Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
After AD is installed, click "notification" to Select: "Promote this server to a Domain Controller"
Select: "Add a new forest" 
Choose a Password and make note of this
"Next"; "Next"; "Next"; "Next" and "Install" to complete installation
Allow the server to close, which will disconnect the Remote Desktop. 
</p>
<br />
<hr>

<p>
<img src="https://i.imgur.com/RGA5XH7.png" height="50%" width="50%" alt="New Forest Set Up"/><img src="https://i.imgur.com/uGcernN.png" height="50%" width="50%" alt="Promote server to Domain Controller"/>
</p>
<p>
Step 8. Restart and then log back into DC-1 as user: mydomain.com\labuser

Create users accounts:
Part 3- 
 Create an Admin and Normal User Account in AD
</p>
<br />

<hr>

<p>
<img src="https://i.imgur.com/41VOvtn.png" height="80%" width="80%" alt="Create OU"/>
</p>
<p>
9. In Active Directory Users and Computers (ADUC), create: 

- an Organizational Unit (OU) called “_EMPLOYEES” 

- a new OU named “_ADMINS”

- a new employee named “Jane Doe” (same password) with the username of “jane_admin”
jane_admin
Password1
For practice purposes, select "Password never expires"


</p>
<br />

<hr>

<p>
<img src="https://i.imgur.com/41VOvtn.png" height="80%" width="80%" alt="Create OU"/>
</p>
<p>
10. Add jane_admin to the “Domain Admins” Security Group
Select the _ADMIN Jane Doe and right click to Select Properties; Select "Member Of"; Add Domain Users: "Domain" and select "Check Names" to open name options; Select "Domain Admins"; Select "Ok"; "Ok"; "Apply"; "Ok"


</p>
<br />

<hr>

<p>
<img src="https://i.imgur.com/41VOvtn.png" height="80%" width="80%" alt="Create OU"/>
</p>
<p>
11. Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin” 


</p>
<br />

<hr>

<p>
<img src="https://i.imgur.com/41VOvtn.png" height="80%" width="80%" alt="Create OU"/>
</p>
<p>
12. User jane_admin as your admin account from now on


Join Client-1 to your domain (mydomain.com)

</p>
<br />
<hr>
<p>
<img src="https://i.imgur.com/dsp5Yqr.png" height="80%" width="80%" alt="Create OU"/>
</p>
<p>
13. From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
Locate DC's Private IP address in the VM DC's Overview

Open the VM Client-1, select "Networking"
Select the "Network Interface" link
Select "DNS Servers" in the Left Column

</p>
<br />
<hr>

<p>
14. From the Azure Portal, VM Client-1- click restart
</p>
<br />
<hr>
<p>
<img src="https://i.imgur.com/Xr2Q8ds.png" height="80%" width="80%" alt="Create OU"/>
</p>
<p>
15. Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart) 
Right Click Start menu 
Select "System"
Select "Rename this PC (advanced)"
Select "Change"
In "Domain" box type: mydomain.com
Select "OK"
In Computer Name/Domain Changes box: 
"mydomain.com\jane_admin" and password
Select "OK" and restart when prompted
</p>
<br />
<hr>
<p>
<img src="https://i.imgur.com/DxaRwX3.png" height="80%" width="80%" alt="Create OU"/>
</p>
<p>
16. Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers
(ADUC) inside the “Computers” container on the root of the domain 
</p>
<br />
<hr>
<p>
<img src="https://i.imgur.com/DxaRwX3.png" height="80%" width="80%" alt="Create OU"/>
</p>
<p>
18. Create a new OU named “_CLIENTS” and drag Client-1 into there

Setup Remote Desktop for non-administrative users on Client-1
</p>
<br />
<hr>
<p>
<img src="https://i.imgur.com/7GRW6Tf.png" height="80%" width="80%" alt="Create OU"/>
</p>
<p>
19.Log into Client-1 as mydomain.com\jane_admin and open system properties
Click “Remote Desktop”
Allow “domain users” access to remote desktop
You can now log into Client-1 as a normal, non-administrative user now
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)
</p>
<br />
<hr>
<p>
<img src="https://i.imgur.com/YjWHBf6.png" height="80%" width="80%" alt="Create OU"/>
</p>
<p>
20.Create random additional users
Within DC-1; 
Open PowerShell ISE by right clicking to "Run as Administrator" 
Open new file and paste the contents of this script file into it to randomly create new users with "Password1" as their passwords for testing purposes.
(Using script file: (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1
Open Active Directory and _EMPLOYEES 
To see the list of random users being added here. 

</p>
<br />
<hr>

<p>
<img src="" height="80%" width="80%" alt="Create OU"/>
</p>
<p>
21.Test by choosing random name and accounts from _EMPLOYEES 

Choose random name, take note of account info

Log off of Client-1; 
Log in, using new account name to test access.

Log into DC-1: 
_EMPLOYEES
Choose Name- Right Click to find properties
Select Account
Check box to Unlock Account
Or Right Click Name to 
Find Drop Down Menu to "Reset Password"


</p>
<br />
<hr>
<p>
<img src="" height="80%" width="80%" alt="Create OU"/>
</p>
<p>
22.and attempt to log into client-1 with one of the users 27.Login to DC-1 as jane_admin
Open PowerShell_ise as an administrator
Create a new File and paste the contents of the script into it
(https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) 30.Run the script and observe the accounts being created
When finished, open ADUC and observe the accounts in the appropriate OU
attempt to log into Client-1 with one of the accounts (take note of the password in the script)

</p>
<br />
<hr>
