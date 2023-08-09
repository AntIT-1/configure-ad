<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

Below is the virtual network that will be setup for this project. 

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/d8473a48-8b81-4b23-a4a5-2f095edb63de)

Now were going to setup a Domain controller. Create a vitrual machine and name it DC-1. Select Windows Server 2022 Datacenter. 

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/be00fce0-de4f-4e7c-b7f6-9618e4c9d8f2)

Next, set the domain controller's IP address to static instead of dynamic. This will keep the DHCP server from changing the IP address of the domain controller. We will setup a client to join this domain and it will use the Domain as the DNS server. 

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/41dcef91-0990-4385-ad62-03eb417eebdf)

The client will now be setup. Create another virtual machine and name it "Client-1". Choose Windows 10 as the operating system with two CPU's. Ensure Connectivity between the client and Domain Controller. Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall.
Check back at Client-1 to see the ping succeed.

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/41880416-91a4-4092-90b3-5c5ed363b8f2)

In the domain controller, go to Windows Defender Firewall-> inbound rules and open ICMPv4 traffic to allow the ping. 

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/36f63e17-d7ec-4dd9-9107-8ad158d9b60b)

No back on client-1 you can see that the ping started working after we enabled the ICMPv4 port. Now connectivity between Client and Domain controller is confirmed.

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/d403e87d-9e30-4435-9f2c-3a181f3f8e4e)

The next step is to install Active Directory.
Login to DC-1 and install Active Directory Domain Services by clicking on "add roles and features". Once AD is install you can
promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
Restart and then log back into DC-1 as user: mydomain.com\labuser

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/0586aba2-2235-4b39-b69f-f5201259d54f)

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/db7d894e-8cbe-4349-8fcb-e5c2878eb1b1)

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/986f8b85-3c26-41fa-bf89-27099914f696)

Create an Admin and Normal User Account in Active Directory. 
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS”
Create a new employee (same password) with the username of “user_admin”
Add user_admin to the “Domain Admins” Security Group
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\user_admin”
User_admin as your admin account from now on.

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/137cc8bb-ee53-47b6-878c-97404c30fc5a)

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/016a767d-8671-464f-b49e-39b2af1ee337)

The new user was added to the domain admins group. Log on to the domain controller as the new user.  

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/dbd1cdea-ca88-4275-989b-444b00bf5f41)

Join Client-1 to your domain
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
From the Azure Portal, restart Client-1
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)
![image](https://github.com/AntIT-1/configure-ad/assets/141161539/eef34b38-2d11-420e-b8d6-dd2b5da30cae)

An ipconfig/ all command confirms that the client-1 is now using the domain's DNS server.

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/1d42f3e1-bb32-476d-b30e-8eef4ded749e)

Adding client to domain.

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/71eebb4b-49cc-4a73-8984-f7754a223bdd)

Now client is on Domain!

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/c0a93915-d514-4303-8dda-ddc2e6a50816)

You can see the Client in Active Directory Users and Computers

![image](https://github.com/AntIT-1/configure-ad/assets/141161539/7629fa3f-be49-40fa-8777-37cc15fa0328)








































