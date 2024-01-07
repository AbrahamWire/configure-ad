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

<p>Log back in as "Yourdomain.com\Youruser</p>

![AD20](https://github.com/AbrahamWire/configure-ad/assets/155400161/98b02724-b51f-4635-84de-76610cbb4697)

<p>Now go to tools(in Server Manager) and click on Active Directory Users and Computers</p>

![AD21](https://github.com/AbrahamWire/configure-ad/assets/155400161/10e38331-844f-4dc7-aa49-6d0db7a0d67f)

<p>Now create a new "Organizational Unit" by right clicking "yourdomain.com" and name it _EMPLOYEES and another one named _ADMINS</p>

![AD22](https://github.com/AbrahamWire/configure-ad/assets/155400161/868b2921-0675-42d5-817d-94af92aea9f5)

<p>Right click _ADMINS and press users</p>

![AD23](https://github.com/AbrahamWire/configure-ad/assets/155400161/ea2516cd-cc65-4dfe-ad30-ca15e15fb607)

<p>Lets name this user Jane Doe. Lets put her user as jane.doe.</p>

![AD24](https://github.com/AbrahamWire/configure-ad/assets/155400161/a38f6de8-52ba-4cf2-93af-49108818630b)

<p>Choose whichever password you want but make sure to take note of it. Make sure to tick the box that prevents the password from expiring.</p>

![AD25](https://github.com/AbrahamWire/configure-ad/assets/155400161/2f42f99d-ace9-4038-9d77-7beeee39ac4d)

<p>Right click on Jane Doe and add this user to a group called Domain Users</p>

![AD26](https://github.com/AbrahamWire/configure-ad/assets/155400161/cc317ee4-88b0-49f3-8f03-91906781e6e0)

<p>Log out of the Domain Controller and log back in as jane doe. You will be using this user for now on.</p>

![AD27](https://github.com/AbrahamWire/configure-ad/assets/155400161/54d27365-6d94-444d-be4a-bd57797d606b)

<p>NOTE: If you are having issues signing into Jane Doe(aka myself). Add Jane Doe to Administrators Security Groups as well.</p>

![AD28](https://github.com/AbrahamWire/configure-ad/assets/155400161/c6e55795-419f-49dd-9854-122164d3bfbd)

<h1>Part 5: Joining Client-1 to DC-1</h1>

<p>In Azure go to Client-1 and go to networking. Press network interface then DNS servers. Tick the customs box and type DC-1 private IP address. Then Restart Client-1</p>

![AD29](https://github.com/AbrahamWire/configure-ad/assets/155400161/fdf34345-f98d-42b9-bccb-b261e09b2e8d)

![AD30](https://github.com/AbrahamWire/configure-ad/assets/155400161/60192e07-f032-4d01-8390-aad36cd15e18)

<p>Sign back into Client-1 as your regular user and go to control panel and type "join a domain" in the search bar</p>

![AD31](https://github.com/AbrahamWire/configure-ad/assets/155400161/4e938b0f-557e-41af-b501-bc9648582323)

<p>Tick the domain box and type your domain name in the box</p>

![AD32](https://github.com/AbrahamWire/configure-ad/assets/155400161/027ec974-6ccb-4096-ae1c-4fbc9b13ae22)

![AD33](https://github.com/AbrahamWire/configure-ad/assets/155400161/10ebccc3-74a4-4932-aedd-eba51742c7e1)

<p>Let the VM Restart. Back in the Domain Controller open Active Directory Users and Computers and verifyif Client-1 shows up</p>

![AD34](https://github.com/AbrahamWire/configure-ad/assets/155400161/39e36db0-b1bc-491b-838e-dc25d47cff89)

<p>Now log into Client-1 as your "wire.com\labuser"</p>
<p>Then go to your system properties and click on advanced then remote and add Domain Users so unauthorized users can log in.</p>

![AD35](https://github.com/AbrahamWire/configure-ad/assets/155400161/30307f22-d338-475a-bebe-af7e2ead3cf3)

<h1>Part 6: Creating Multiple users and changing passwords</h1>

<p>Back in the Domain Controller logged in as Jane of course. Open Powershell ISE as an administrator.</p>

![AD36](https://github.com/AbrahamWire/configure-ad/assets/155400161/ad60499d-3c7b-4fc5-a31f-57f4bcc6d7ea)

<p>Create a new script and copy this code (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) and paste it on to your script. Edit the script so that only 500 accounts are made</p>

![AD37](https://github.com/AbrahamWire/configure-ad/assets/155400161/6e951e71-9c63-461e-8df7-ede7a34c673a)

<p>Now just run the script</p>

<p>Once the script is done go to Active Directory Users and Computers and go to _EMPLOYEES. Choose any random employee and copy the user name.</p>

![AD38](https://github.com/AbrahamWire/configure-ad/assets/155400161/40623ec7-a91c-40ac-a27e-86b68970d80b)

<p>Sign into Client-1 with that user. Take note that the password is "Password1"</p>

![AD39](https://github.com/AbrahamWire/configure-ad/assets/155400161/9ecbbc95-50fa-48e9-bfd2-9b4236f99fe7)

<p>Sign out of the client and back in the Domain Controller chang that same users password by right clicking and type whatever you like</p>

![AD40](https://github.com/AbrahamWire/configure-ad/assets/155400161/9b8a6f57-0a1c-4d2d-9086-a030a3d4ced7)

<p>Sign into the Client again with that same user</p>

<h1>Conclusion</h1>
<p>Thats the end of my Project/Tutorial! I hope you learned something! Continue toimmerse yourself with knowledge and continue to experiment with Active Directory!</p>
