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

  - Setup Domain Controller in Azure
  - Setup Client-1 in Azure
  - Install Active Directory
  - Create a Domain Admin user within the domain
  - Join Client-1 to your domain (mydomain.com)
  - Setup Remote Desktop for non-administrative users on Client-1
  - Create additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/lzqysey.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/MuSdGq4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/nSjqfVU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
This process was done on a Virtual Machine I created through Microsoft Azure. To set up the Domain controller I created a resource group named “Active-DirectoryLab” Then, I created a virtual Network called “Active-DirectoryVnet” Finally, I created the Domain controller virtual machine named “DC-1” which I linked to my resource group and virtual network. I added credentials which I will use to enter this virtual machine through remote desktop. 
</p>
<br />

<p>
<img src="https://i.imgur.com/zb2dOnp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/F6oQYIM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/wRyPzJx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/gxzHaJo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/HunHe1y.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Setting Up Client-1: I created a new virtual machine named “Client-1” I made sure to add this virtual machine to resource group “Active-DirectoryLab” and also to virtual network “Active-DirectoryVnet” I added credentials which I will use to enter this virtual machine through remote desktop.  Next, I set Domain Controller’s (DC-1) NIC Private IP address to be static so it can serve as a DNS server to the “Client-1” virtual machine. I also logged into the DC-1 virtual machine through remote desktop and disabled the Windows Firewall (for testing connectivity). I set Client-1’s DNS settings to DC-1’s Private IP address and restarted the Client-1 virtual machine to apply changes. To ensure connectivity between DC-1 and Client-1: I entered the Client-1 virtual machine, opened PowerShell and pinged DC-1’s private IP address. 
</p>
<br />

<p>
<img src="https://i.imgur.com/Cu5Z0EG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/6NXN7nT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/6QIYOGA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Installing Active Directory to DC-1: In Server Manager>Add roles and features>On roles tab check “Active Directory Domain Services”> Add Features>Install. After the installation was complete, I began to configure Active Directory and set up a new forest as mydomain.com. Finally, I restarted my virtual machine and logged back into DC-1 as user: mydomain.com\labuser.
</p>
<br />

<p>
<img src="https://i.imgur.com/Lk9HFvb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/Cto6Aiz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/m53X3uk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>Creating a Domain Admin User within the Domain: In DC-1, I opened Active Directory Users and Computers (ADUC) and created an Organizational Unit (OU) called “_EMPLOYEES” This folder will be used in the future to store all the employee names I will create through PowerShell. Next, I created another organizational unit (OU) named “_ADMINS” In this folder, I added a user named Jane Doe. In order to make her a domain admin, I added her to the “Domain Admins” Security Group. Next, I logged out and closed the connection to DC-1. Finally, I logged back in as “mydomain.com\jane_admin” to make sure she was successfully added as a domain administrator.
</p>
<br />

<p>
<img src="https://i.imgur.com/dyDNWpu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p> 
</p>
<br />

<p>
<img src="https://i.imgur.com/7uXLlCw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/jH7VAa8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Joining Client-1 to (mydomain.com): I logged into to Client-1 as the original local admin (labuser) and joined it to the domain. I did this in Settings> System>About>Rename this PC(Advanced)>Computer Name> Change>Member of Domain. I added “mydomain.com” and applied changes. Next, I added Jane Doe’s credentials to join the domain because she is a domain administrator and has permission to join the domain. The computer restarted to apply all changes. To verify that client-1 was part of the domain, I logged into DC-1 and checked in “Active Directory Users and Computers” In computer folder, I saw Client-1 had successfully joined. 
</p>
<br />

<p>
<img src="https://i.imgur.com/l0Ob3OS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Setting up Remote Desktop for non-administrative users on Client-1: I logged into Client-1 as Jane Doe. I opened Systems> remote desktop>Select users that can remotely access this PC>Add. I added “ domain users” and applied changes. This will now allow me to log into Client-1 as a normal, non-administrative user.
</p>
<br />

<p>
<img src="https://i.imgur.com/toHQqjc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/iMz9kJc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/RBMdyEP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
<img src="https://i.imgur.com/uxR6lgg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/gxzHaJo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
