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

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
