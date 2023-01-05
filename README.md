<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



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
Step 1: Setup Resources in Azure 
- Create two virtual machines
- The first VM will be the Domain Controller 
- Name: DC-1 
- Image: Windows Server 2022
- Remember the Virtual Network (Vnet) that we create. 

<p>
<img src="https://i.imgur.com/T1OukuK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Set DC-1's Virtual Network Interface Card (NIC) Private IP address to be static 
Go to Dc-1's network setting -> select networking -> select the link next to network interface 
IP Configurations-> ipconfig1
Assignment from dynamic to static (this ensures DC-1's IP address will not change)
  
<p>
<img src="https://i.imgur.com/jWrRofV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
<p>
<img src="https://i.imgur.com/Z9XbpQa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/dnVdvux.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
The 2nd VM will be the Client 
  Name: Client-1
  Image: Windows 10 Pro
  Use the same resource group and Vnet as DC-1 
</p>
<br />

<p>
<img src="https://i.imgur.com/kVfgU1g.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/D5K6Bmo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 2: Ensure Connectivity between client and Domain Controller 
Login to Client-1 using Microsoft Remote Desktop 
Search for command prompt and open it 
Ping DC-1's private IP Address (Mine 10.0.0.4)
Ping -t 10.0.0.4
Due to the firewall it will request timeout. To fix this, we need to enable ICMPv4 on DC-1's local Windows firewall.

<p>
<img src="https://i.imgur.com/438mkaE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

</p>
<br />
Login to DC-1 using Microsoft Remote Desktop
Start -> Windows Administrative Tools -> Windows Defender Firewall with Advanced Security-Inbound rules. 
Sort the list by protocols 
Find Core Networking Diagnostics ICMPv4 and enable these two inbound rules

<p>
<img src="https://i.imgur.com/mOvHGLG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/hyHBE9L.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Log back into Client-1 and the Command line will start to ping DC-1 successfully. To end it from Pinging press Control C 
</p>
<br />

<p>
<img src="https://i.imgur.com/P7KBgCn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 3: Install Active Directory 
Log back ito DC-1
Open Server Manager
Select "Add roles and features." Follow the prompts.
At Server Roles, check "Active Directory Domain Services." (Ignore how the picture below that already says "Installed") Then select next Add Features. Select next and finish installing.

<p>
<img src="https://i.imgur.com/JIwGRkY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/BAAly3j.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

At the top right of the Server Manager Dashboard, click on the flag. 
Select promote this server to a domain controller 

<p>
<img src="https://i.imgur.com/eFFV0ik.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Select Add a new forest - Root domain name: mydomain.com 
Select next - Create password 
Select next, follow the prompts and finish up by selecting install. 

<p>
<img src="https://i.imgur.com/PEeaQy6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

DC-1 will automatically restart.
Log back into DC-1 as user: mydomain.com\Azurelabuser

<p>
<img src="https://i.imgur.com/B1AwRDO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 4: Create an Admin and Normal User Account in Active Directory v1.15.8 
On DC-1, open up Server Manager 
Click tools at the top right hand side 
Select Active Diretory Users and Computers. 

<p>
<img src="https://i.imgur.com/684wsHd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Right click mydomain.com -> New -> Select Organizational Unit (We will be creating 2 folders.)
Name one _EMPLOYEES and the other _ADMINS

<p>
<img src="https://i.imgur.com/SCrdZox.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Right click mydomain.com and click refresh to sort the new organizational untis to the top. 
Go to _ADMINS organzational unit -> right click -> New -> User
First/Last name: Jack doe 
User login Jack_admin
Select next and create a password 
uncheck all boxes, select next and then select finish 

<p>
<img src="https://i.imgur.com/a5ZbkIc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/aPbMqQH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Go to _ADMINS organzational unit -> right click Jack doe -> select properties 
Click "memember of" tab -> Select Add -> type in domain admins -> Check Names -> OK -> Apply 
Log out of DC-1 as "Azurelabuser" and log back in as "mydomain.com\jack_admin"

<p>
<img src="https://i.imgur.com/B7ORMAC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/0QjIOjN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 5: Join Client-1 to your domain (mydomain.com)
Go back to the Azure portal. 
GO to Client-1 Virtual Machine 
On the left hand side select Networking -> select the link next to the NIC -> DNS server -> Custom -> Type in DC-1's private IP address -> save 
After it is done updating, select restart and select yes

<p>
<img src="https://i.imgur.com/EhWDCds.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/dwrpc0v.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/zsZPhag.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Log back into Client-1 using Remote Desktop as orginal local admin (Azurelabuser) 
Right click the start menu and select System. 
On the right hand side, select Rename this PC (advanced) -> Change -> Under Memeber of, select domain -> type my domain.com and select OK 
Username: mydomain.com\jane_admin 
Type in password and press OK
Restart the computer 

<p>
<img src="https://i.imgur.com/enexEYu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 6: Setup Remote Desktop for non adminstrative users on Client 1 
Log back into Client-1 
user mydomain.com\jack_admin
Right click the start menu and select system
on the right hand side, select Remote Desktop-> under User Accounts, click on select users that can remotely access this PC -> Select add 
Type: domain users -> Check Names -> OK. Select OK again.

<p>
<img src="https://i.imgur.com/Li4BHQx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
<img src="https://i.imgur.com/DfdDVZN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>

Step 7: Create a bunch of additonal users and attempt to log into client-1 with one of the users
Log back into DC-1 as jack_admin 
Search for Powershell_ise, right click on it and open administrator 
At the top left, select new script and paste the contents of the script into it. You can find the script [here](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).. 

<p>
<img src="https://i.imgur.com/eLgbVan.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>

<p>
<img src="https://i.imgur.com/pmihakd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>

Click the green arrow button near the top middle to run the script 
Once the users have been created, go back to Active Directory Users and Computers -> mydomain.com -> _EMPLOYEES
You will see all the accounts that were created, in here. 
You can now log into Client-1 with one of the accounts that were created. 

<p>
<img src="https://i.imgur.com/lCvIjbT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>

<p>
<img src="https://i.imgur.com/VKtZ3bk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>

<p>
<img src="https://i.imgur.com/gFxArfS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>

Log into Client-1 with one of the users that were created (in our instance "baj.kas", password will be Password1)

<p>
<img src="https://i.imgur.com/cusUgaJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>

<p>
<img src="https://i.imgur.com/fCgiapo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>

Congrats! You have offically implementated on-premises Active Directory and created users within Azure Virtual Machines!
