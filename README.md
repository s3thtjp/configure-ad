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

<h2>High-Level Deployment and Configuration Steps</h2>

- Set up nessary resources within Azure
- Create virtual machines with Windows Server and Windows 10
- Set up Windows Server to "dc-1"
- Set up Windows 10 to "client-1"
- Configure the network adapters to make "dc-1" a Domain Controller
- Configure "client-1" to connect DNS Server "dc-1"
- Test connection between "client-1" to "dc-1" through Windows Powershell
- 

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/P7ix79W.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First we need to create a new resource group within the Azure portal and set the name to "Active-Directory-Lab".
</p>
<br />

<p>
<img src="https://i.imgur.com/Ywrfxjj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we need to create a new virtual network within the Azure portal and set the name to "Active-Directory-Vnet".
</p>
<br />

<p>
<img src="https://i.imgur.com/N3sHrTU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we need to create the virtual machine named "dc-1" with Windows Server 2022 installed as the image. Make sure it is installed to the "Active-Directory-Lab" resource group as well as the "Active-Directory-Vnet" under networking. Fill in "labuser" as the username and fill in any easy to remember password. Next we will review and create the virtual Windows Server.
</p>
<br />

<p>
<img src="https://i.imgur.com/CDGJSEd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we are going to create a second virtual machine named "client-1" with Windows 10 installed as the image. Make sure it is installed to the "Active-Directory-Lab" resource group as well as the "Active-Directory-Vnet" under networking. Fill in "labuser" as the username and fill in any easy to remember password. Next we will review and create the virtual Windows 10 machine.
</p>
<br />

<p>
<img src="https://i.imgur.com/BFonWd4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we have to change the "dc-1" NIC to a static IP. To do this we have to navigate to the virtual machines in Azure then select "dc-1" the click on "Network Settings" the click on the "ipconfig" at the top with the green NIC picture then select "ipconfig1" link. On the right under Private IP address settings next to allocation we then need to select the "Static" bullet and click "Save" at the bottom.
</p>
<br />

<p>
<img src="https://i.imgur.com/cYlZery.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we need to connect to the "dc-1" using Remote Desktop Connection. Copy down the public ip address of "dc-1" then open up the Remote Desktop Connection and paste the ip. Fill in the log in credentials and connect.
</p>
<br />

<p>
<img src="https://i.imgur.com/SQDDZMI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we need to disable Windows Firewall. To do this right click the start button then select run and type "wf.msc" to run the Windows Firewall. Click on the Windows Defender Firewall Properties and turn off in all the tabs then click "Apply" and "OK"
</p>
<br />

<p>
<img src="https://i.imgur.com/EwyhRcl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we have to point the virtual machine "client-1" to look up DNS on "dc-1" by setting the DNS server as the Private IP of "dc-1". To do this click on virtual machines in the Azure portal the select "client-1" then click on "Network Settings" then click on the NIC at the top labeled as "ipconfig1". Select the DNS Servers then select the "Custom" bullet under DNS servers and enter the Private IP address of "dc-1" then click "Save" at the top.
</p>
<br />

<p>
<img src="https://i.imgur.com/eRetWB7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we have to restart "client-1" due to the changes in the NIC. To do this we need to navigate to the virtual machines in Azure. Check the box next to "client-1" then select the "Restart" button at the top and select the "Yes" button.
</p>
<br />

<p>
<img src="https://i.imgur.com/6rZTICA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we have to log into the "client-1" virtual machine using Remote Desktop Connection. To do this grab the Public IP address then open up Remote Desktop Connection and fill in the credentials.
</p>
<br />

<p>
<img src="https://i.imgur.com/SuLZee2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we are going to send a ping to the "dc-1" server to test the connection. To do this we need to open up Windows Powershell by searching "powershell" by the start button and it should show up then select Windows Powershell. When Windows Powershell is open type "ping" then enter "dc-1" private address.
</p>
<br />

<p>
<img src="https://i.imgur.com/Ik2Omqn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we are going to look at the DNS Server information within Windows Powershell. To do this in the same Windows Powershell window type "ipconfig /all" and it'll show all the DNS information in Windows Powershell.
</p>
<br />

<p>
<img src="https://i.imgur.com/oHnLlq9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we will set up the Active Directory Services on "dc-1". To do this open up the Remote Desktop Connection to "dc-1". Click on the start button at the bottom left and select "Server Manager". When opened click on "Add roles and features" at the top. Then click "Next" button three times. Check the box for "Active Directory Domain Services" then select "Add Features" continue pressing "Next" until you can't anymore. Check the box to "Restart the destination server automatically if required" then select "Yes" and "Install"
</p>
<br />

<p>
<img src="https://i.imgur.com/XwcZcUz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we are going to promote "dc-1" to a "Domain Controller". Click on the flag icon and click on the link to promote server to a domain controller. Next click the bullet by "Add a new forest" and type "mydomain.com" then select the "Next" button. Enter a password into the fields and click the "Next" button. Uncheck the box by "Create DNS delegation" then click the "Next" button. Keep clicking the "Next" button until you can't anymore. Then select the "Install" button. After installation is complete the virtual machine will restart.
</p>
<br />

<p>
<img src="https://i.imgur.com/wRTDidR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
When reconnecting through Remote Desktop Connection you have to add "mydomain\labuser" as the username to log back into the "dc-1".
</p>
<br />

<p>
<img src="https://i.imgur.com/QKehJGO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we are going to create a new "Organizational Unit" in active directory called "_EMPLOYEES". To do this click on the start button the select the "Windows Administrative Tools" and select "Active Directory Users and Computers". Once opened right click on "mydomain.com" drag down to "New" and select "Organinizational Unit". In the name field type "_EMPLOYEES" and click the "Ok" button at the bottom.
</p>
<br />

<p>
<img src="https://i.imgur.com/cp6y66d.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next we are going to create a new "Organizational Unit" in active directory called "_ADMINS". To do this click on the start button the select the "Windows Administrative Tools" and select "Active Directory Users and Computers". Once opened right click on "mydomain.com" drag down to "New" and select "Organinizational Unit". In the name field type "_ADMINS" and click the "Ok" button at the bottom.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
