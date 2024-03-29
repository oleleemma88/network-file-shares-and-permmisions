# network-file-shares-and-permissions

<p align="center">
<img src="https://jumpcloud.com/wp-content/uploads/2016/07/AD1.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>



<h1>Network File Shares and Permissions</h1>
In this tutorial, we will share out resources over the network by creating file shares to allow, read, write, or deny access to individual users and groups. <br />

<h2>Enviornments and Technologies Used</h2>

- Microsoft Azure Virtual Machines
- Remote Desktop
- Active Directory Users and Computers
- Network Security Group
- Organization Units

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Windows Server 2019 Datacenter (1809)

<h2>High-Level Steps</h2>

- Create sample file share folders with permissions
- Access file shares as a normal users
- Create an "ACCOUNTANTS" Sccurity Group, assign permissions, and test access

<h2>Actions and Observations</h2>

<h3>Create sample file share folders with permissions</h3>

<p>
<img src="https://i.imgur.com/ulODls5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create 2 instances of your remote desktop and log into the domain controller as an admin and your client PC as one of the users.
</p>
<br />

<p>
<img src="https://i.imgur.com/802hsrZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From your domain controller click on the Windows Explorer icon on your taskbar-->This PC-->Click on your C:\ drive to open its contents.
</p>
<br />

<p>
<img src="https://i.imgur.com/1DohoPM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From your domain controller in the C:\ drive create 4 folders: "read-access", "write-access", "no-access", and "accounting".
</p>
<br />

<p>
<img src="https://i.imgur.com/bgCYhn3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Windows C:\ drive right click the folder-->hover to Properties-->Click on the Sharing Tab-->Click on Share from the Sharing tab-->Type Domain Users in the bar above the name and permission level. 

There will be a drop down menu that will allow you to select the permission level for your domain users.  Check "Read" for the read-access folder, "Read/Write" for the write-access folder. Instead of adding domain users in the no-access folder, we will use domain admins instead and provide them with "Read/Write" access.  This will give normal users no access to that folder. 

We will go to the client PC and check folders for access in the following steps.
</p>
<br />

<p>
<img src="https://i.imgur.com/Z8kDOs0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once your permissions are set there, you can send  and email of that shared folder or share the link into another app.  In the individual items section, there will be a file path that you can copy and paste in a windows explorer search box that will take you to that specific folder.
</p>
<br />

<p>
<img src="https://i.imgur.com/PW1fiMv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open each folder and create a text document that we can test for access when we log into the client virtual machine as a domain user.  To create the document, right click on each folder-->hover to New-->hover to text document and click.
</p>
<br />

<p>
<img src="https://i.imgur.com/L2W3COP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once your file is created, type a sample text, go to File-->Save As-->Name your file-->Click Ok.
</p>
<br />

<h3>Attempt to access file shares as a normal user</h3>
<p>
<img src="https://i.imgur.com/E5yyzi2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On Client-1, navigate to the shared by typing Run in the search bar-->type \\DC-1-->The network folder should populate in a new window.
</p>
<br />

<p>
<img src="https://i.imgur.com/qHHhIZ9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open the read-access folder-->Open your test file-->Attempt to edit the text and save the document. A dialog box will show that you have read access only.
</p>
<br />

<p>
<img src="https://i.imgur.com/64GeqeX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open the no-access folder.  Upon clicking you will get a network error stating that you do not have access to this folder.
</p>
<br />

<h3>Create an "ACCOUNTANTS" Security Group, assign permissions, and test access</h3>

<p>
<img src="https://i.imgur.com/o7wJeeF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go to DC-1 in Active Directory and create a organization unit _SECURITY_GROUP  then add the group  "ACCOUNTANTS" to that folder.
</p>
<br />

<p>
<img src="https://i.imgur.com/GYyfU5w.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a security group "ACCOUNTANTS"  in your organizational folder. Right click _SECRUITY_GROUP-->hover New-->hover to Group and click-->Type ACCOUNTANTS in the dialog box for group name-->click on the radio buttons global for group scope and security for group type-->Click Ok
</p>
<br />

<p>
<img src="https://i.imgur.com/Sdsg6NT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On the "accounting" folder that was created earlier in DC-1 virtual machine, we are going to set the following permissions, for the "accounting folder, add the security group "ACCOUNTANTS" from the properties sharing tab.  Give the group permission read/write permissions. Click on share.
</p>
<br />

<p>
<img src="https://i.imgur.com/MSgxjZf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On Client-1 virtual machine as a user, open Windows File Explorer-->Type\\DC-1 in the file location bar-->Click on the "accounting" folder to access the file.  

An error message shows no access.  The current user does not have access to the file folder. In the next step, we will go back to DC-1 virtual machine, and add the user to the security group for access to that folder.
</p>
<br />

<p>
<img src="https://i.imgur.com/3KjDe9g.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/2vTnxyv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On DC-1 virtual machine, add the user to the "ACCOUNTANTS" -->Click on _SECURITY_GROUPS-->Right click "ACCOUNTANTS"-->Click Properties-->Click on Members tab-->Click Add...-->Type the name of the user names in the object box-->Click Find Names-->When the user has been found in your directory, click Ok.
</p>
<br />

<p>
<img src="https://i.imgur.com/OAjipjh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On the Client-1 virtual machine, click on Start-->click on the user-->click on Sign out to log off the virtual machine. 

You will need to log off and on for the changes to take effect.
</p>
<br />

<p>
<img src="https://i.imgur.com/ntWTGef.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log into your Client-1 virtual machine with your user credentials-->Open Windows File Explorer-->Type\\DC-1 in the file directory bar-->Click "accounting"-->Open the test file.  

*If everything was done correctly, the file should open, and you should be able to edit the document and save it in the network folder.
</p>
<br />

