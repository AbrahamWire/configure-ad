<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>
<h1>Azure-based deployment of an on-premises Active Directory system</h1>

<p>This tutorial outlines the implementation of Active Directory within Azure Virtual Machines. As well as how to mess with certain accounts and change passwords.</p>

<h1>Environments and Technologies used</h1>

  - Azure
  - Powershell
  - Active Directory Domain Services
  - Remote Desktop
  - Windows 10
  - Windows Server

<h1>Part 1: Create a Domain Controller VM as well as a normal windows 10 VM in Azure</h1>
<p>Create a new Resource Group(Name it whatever you want)</p>

![AD1](https://github.com/AbrahamWire/configure-ad/assets/155400161/ba35d4ee-a327-46d9-8598-fe358b5a8e2b)

<p>Name the VM "DC-1" and choose "Windows Server 2022" as the Image. Also for the size choose the 2 cpus for the best speed.</p>

![AD2](https://github.com/AbrahamWire/configure-ad/assets/155400161/de9b52a5-b476-4f74-91d7-95640c6cd57e)

<p>Pick a password and make sure to remember it</p>

![AD3](https://github.com/AbrahamWire/configure-ad/assets/155400161/abca12e0-1f90-440c-a943-7ed9acbac8a0)

<p>Create the VM and wait until its fully deployed</p>

![AD4](https://github.com/AbrahamWire/configure-ad/assets/155400161/fa36a2b5-8fbf-4578-a65d-aaa3f132b0dd)

<p>Now create another VM with the Windows 10 image and name it Client-1 (Choose the same Resource Group as the DC)</p>

![AD5](https://github.com/AbrahamWire/configure-ad/assets/155400161/072c6b74-ae00-4c40-b48c-15aacc0ba3ca)

<p>For the size choose 2 cpus and create your username/password(Don't forget to make a note)</p>

![AD6](https://github.com/AbrahamWire/configure-ad/assets/155400161/2b7eff92-af39-4ed8-b5db-a10d79294116)

<p>On the Networking tab maneuver your way to the Virtual Network and choose DC-1-vnet</p>
<p>The Vnet was made when you created DC-1. If it is not showing continue to wait a few more minutes and refresh the page. Once you hsve it click create</p>

![AD7](https://github.com/AbrahamWire/configure-ad/assets/155400161/8e63d407-7bf9-412f-8a48-9f6ff090b322)

<h1>Part 2: Check if the client can connect to the Domain Controller</h1>
<p>Before anything go to the Domain Controller VM and click "Networking". Then press the network interface.</p>

![AD8](https://github.com/AbrahamWire/configure-ad/assets/155400161/29ebaa49-da8a-4941-8b14-e3c4630b51cd)

<p>Go to the "IP configurations" and click on "ipconfig". Make the ip address static.</p>

![AD9](https://github.com/AbrahamWire/configure-ad/assets/155400161/87e175b3-f253-4f80-b2d1-93fd657ae3e0)

<p>Login into Client-1 through Remote Desktop</p>
<p>Get the ip address from the VM Client menu and login with the user/password that we created</p>

![AD10](https://github.com/AbrahamWire/configure-ad/assets/155400161/56037308-500f-4953-8511-eb18b471ab13)

<p>Once connected open command prompt and start a non-stop ping to the Domain Controller's private IP address which you can get from "DC-1" VM menu</p>

![AD11](https://github.com/AbrahamWire/configure-ad/assets/155400161/685fd4f2-a821-448a-a180-a20c05e83bc7)

![AD12](https://github.com/AbrahamWire/configure-ad/assets/155400161/e69418a5-8d27-4c0d-8458-1695303dde94)

<p>Now login to the Domain Controller and enable ICMPv4. Once you open the firewall go to inbound rules and sort by protocol and enable these rules.</p>

![AD13](https://github.com/AbrahamWire/configure-ad/assets/155400161/4cbdb05e-84b0-47b6-b9de-382b5809f911)

![AD14](https://github.com/AbrahamWire/configure-ad/assets/155400161/e7ae02a7-044d-4bfb-a1b1-31a940e2b8ee)

<p>Go back to Client-1 and observe the ping</p>

![AD15](https://github.com/AbrahamWire/configure-ad/assets/155400161/71910c0a-72a6-41ef-b61f-5a15f4072ab6)

<p>Hit Control+C to stop the ping</p>

<h1>Part 3: Installing Active Directory Domain Services in Domain Controller</h1>

<p>In the Domain Controller server open Server Manager. Click "Add roles and features" and download Active Directory and Domain Services</p>

![AD16](https://github.com/AbrahamWire/configure-ad/assets/155400161/11043436-1fbc-4a35-ab6d-edfc76bff977)

<h1>Part 4: Creating a domain name as well as an admin account</h1>

<p>Promote DC-1 to a Domain controller</p>

![AD17](https://github.com/AbrahamWire/configure-ad/assets/155400161/11331a55-999f-41ed-b076-1c66051c67ee)

<p>Add the domain to a new forest. Name it whatever you want. Then Create a Password(Don't forget to take note of the password)</p>

![AD18](https://github.com/AbrahamWire/configure-ad/assets/155400161/d9bebf37-48b5-4b0a-8d42-446ad7750888)

![AD19](https://github.com/AbrahamWire/configure-ad/assets/155400161/74ec2c67-5244-48fe-b3bc-25139893dfbc)

<p>The server will restart so do not worry</p>
