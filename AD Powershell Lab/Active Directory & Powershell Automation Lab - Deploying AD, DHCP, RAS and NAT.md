I will followed this lab I found on YouTube: https://www.youtube.com/watch?v=X0IN36CUVsU

There are two ways to complete this lab. Either using an EC2 instance on AWS or creating a virtual machine using a hypervisor such as Oracle VirtualBox.

I used the VirtualBox method for the lab after I got to a part where the EC2 instance ran too slow as it heavily depends on your internet connection.

The EC2 instance method is great if your laptop may not have enough RAM to run the virtual machines at the same time and/or you have a strong internet connection.

# Part 1: Deploying Active Directory
### Step 1: Install Windows Server 2022
First start was downloading Windows Server 2022 as virtual machine to be used in Virtual Box. I don't want it to affect anything on my laptop currently.

The biggest problem I encountered was the tutorial being done on a Mac using VMWare Fusion, which I believe is a paid software. So I realized most of this lab will be configured to be ran on VirtualBox

I found a video where I could do this: https://www.youtube.com/watch?v=AlFQ0tSdgW0

I went to the [download site](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022) and clicked "Download the ISO" option. This option only provides a free trial for 180 days. There will then be a form for you to fill out

![](attachments/Pasted%20image%2020250720155145%201.png)

After filling the form, click on the 64-bit ISO file in the language of your choice and install it somewhere you can easily find it.
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720164908.png]]
<img width="1460" height="926" alt="Pasted image 20250720164908" src="https://github.com/user-attachments/assets/05b728b6-5019-4d70-8459-f606449d7933" />



Next, open VirtualBox. Click new and a small window should pop up

For name, Windows Server 2022 is fine. 
For the ISO image, navigate to where you downloaded your image on your laptop.'
For Edition: Select Windows Server 2022 Standard Evaluation (Desktop Experience). That is the option we will be using for this lab as you will be using the GUI of this VM.
The settings should resemble something like this:
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250723031948.png]]
<img width="938" height="729" alt="Pasted image 20250723031948" src="https://github.com/user-attachments/assets/f4a198c5-868c-4e19-bb0f-453d781bcda0" />
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250723032123.png]]
<img width="943" height="723" alt="Pasted image 20250723032123" src="https://github.com/user-attachments/assets/099c4add-e645-4309-855c-ca6bd6f13219" />


For the Network settings, Adapter 1 should be set to NAT and Adapter 2 should be set to Internal Network
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250723031803.png]]
<img width="962" height="592" alt="Pasted image 20250723031803" src="https://github.com/user-attachments/assets/29707b90-9a73-413f-a171-ecb9cd5767de" />
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250723031822.png]]
<img width="960" height="592" alt="Pasted image 20250723031822" src="https://github.com/user-attachments/assets/324a535b-d5e8-4f02-b3e9-4465ec5b5e49" />



## Step 1: Launch Windows Server Instance

I have previously provisioned a functioning instance of Windows Server 2025 using this guide here. This allows you use a Remote Desktop client ()RDP) for this lab.

## Step 2: Server Manager Setup

Now in the search bar at the bottom of the RDP, look up "Server Manager" and click open. It should look like this:![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720170446.png]]
<img width="962" height="812" alt="Pasted image 20250720170446" src="https://github.com/user-attachments/assets/b26824b3-6df0-416f-aba0-2dbc8db724da" />

Once opened, you should have a window that looks like this:
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720171311.png]]
<img width="1903" height="969" alt="Pasted image 20250720171311" src="https://github.com/user-attachments/assets/96c6a1c5-13ac-428b-845a-39431ea9ef52" />


If you see a bar loading at the top, it's just preparing the app for you. Once that is done. Click "Add roles and features" and this window should pop up:![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720171529.png]]
<img width="978" height="695" alt="Pasted image 20250720171529" src="https://github.com/user-attachments/assets/ce81abe7-73ce-4a0b-96e3-dd42e0975dd5" />


Click next until you get to that tab that says "Server Roles". There you will select the following roles:
- Active Directory Domain Services
This window pops up:
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720172114.png]]
<img width="518" height="542" alt="Pasted image 20250720172114" src="https://github.com/user-attachments/assets/a33cde7c-3203-43f8-a623-32d50d37aca7" />

Click "Add Features"

Then click Next until you get to the end. Then finally click "Install". The installation may take a while, so just we'll wait until everything is ready.
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720174630.png]]
<img width="977" height="695" alt="Pasted image 20250720174630" src="https://github.com/user-attachments/assets/69511d4e-84e8-48f1-a8c7-3ad7488b3dbb" />


## Step 3: Configuring Active Directory Domain Services
Once everything is done installing, you will notice a flag 
with a hazard symbol in the top-right corner of the window. 
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720180305.png]]
<img width="1885" height="466" alt="Pasted image 20250720180305" src="https://github.com/user-attachments/assets/7166cbed-6979-421d-86f5-78faa10020b9" />

Click on this flag then click "Promote this server to domain controller" and another window will pop up:
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720180402.png]]
<img width="953" height="694" alt="Pasted image 20250720180402" src="https://github.com/user-attachments/assets/f1a54d48-ca78-4ca7-ab4c-c677f10fe60d" />

In this window, select "Add new forest" so we can input a new domain name.
For root domain name, it can be whatever you desire, but we will put in "mydomain.com" for the purpose of this lab.
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720180717.png]]
<img width="953" height="697" alt="Pasted image 20250720180717" src="https://github.com/user-attachments/assets/00ebddd2-4828-43fb-93a6-a8c9cb91ef88" />

Click Next.

You'll be prompted to create a password so put what you want in. For this lab, we'll put in "Password1":
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720181020.png]]
<img width="953" height="698" alt="Pasted image 20250720181020" src="https://github.com/user-attachments/assets/b20ffbe7-58a2-4a81-84ea-74b85605c6dc" />

Click Next. Verify that the NetBIOS name says "MYDOMAIN" if you used mydomain.com in the earlier steps![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720181528.png]]
<img width="950" height="699" alt="Pasted image 20250720181528" src="https://github.com/user-attachments/assets/9b86ea8f-ea59-4796-8382-5b973a12b0cb" />

Then click next until you get to the prerequisites tab. You should see a green checkmark at the top saying all prereqs were passed successfully showing that we set everything up correctly. Here you click install.
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720181919.png]]
<img width="953" height="700" alt="Pasted image 20250720181919" src="https://github.com/user-attachments/assets/c2dbeab1-8844-478f-bd2f-1d8888bfe20e" />

Once the install is complete, your RDP/VM will restart. For the RDP, reconnect the same way that you did when you first connected. 

You'll notice on the login page that the username changes from Administrator to MYDOMAIN\Administrator, showing that the addition of the domain was successful
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720190758.png]]
<img width="885" height="746" alt="Pasted image 20250720190758" src="https://github.com/user-attachments/assets/9395e7b2-4323-4bed-8742-a08d5e639dfd" />


## Step 4: Creating Users in Active Directory
Navigate to the Start menu and search "Active Directory Users and Computers" (don't need to search the whole thing fr, just "Active Directory" is fine and the "Users and Computers" option will pop up)

Click on that to open a new window and you'll see something similar to the photo below.![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720191513.png]]
<img width="936" height="660" alt="Pasted image 20250720191513" src="https://github.com/user-attachments/assets/5c00c52c-5453-4459-82af-e247264eebaa" />


Right-click on mydomain.com (or whatever your domain name is), navigate to "New", then "[[Organizational Units]]". Click on that and we'll have a new pop up:![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720191646.png]]
<img width="542" height="466" alt="Pasted image 20250720191646" src="https://github.com/user-attachments/assets/498f59e1-8b4e-4157-9053-0f27046a85b0" />


In here, type "_ADMINS"_  and click "OK". The new folder should appear at the bottom![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720192215.png]]
<img width="240" height="241" alt="Pasted image 20250720192215" src="https://github.com/user-attachments/assets/068e14df-944e-4abb-a9c4-84494d88cc03" />


Right-click on the _ADMINS_ folder go to "New" then click "User"
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720192159.png]]
<img width="676" height="644" alt="Pasted image 20250720192159" src="https://github.com/user-attachments/assets/b0c3b029-efbf-4432-8866-3ed2fc7bd4c0" />


This window should pop up:![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720192535.png]]
<img width="546" height="467" alt="Pasted image 20250720192535" src="https://github.com/user-attachments/assets/4c869702-dec4-419c-a735-e2523c1d1a21" />


You can fill this in with whatever you want. Then click next.

Here, type in a password, and have the checkboxes filled in the same way as the photo. Take note that we NEVER want to have a password that never expires. This is only checked for lab purposes. We would not have this box in a real-life scenario.
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720192756.png]]
<img width="545" height="470" alt="Pasted image 20250720192756" src="https://github.com/user-attachments/assets/2c43d387-89a6-4014-afc7-711e4b550540" />


After that click next, then click Finish. Your windows should now have the newly created user
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720193156.png]]
<img width="944" height="661" alt="Pasted image 20250720193156" src="https://github.com/user-attachments/assets/a863d4b9-eda2-4938-90fd-6f675bdac8e3" />

## Step 5: Granting Permissions
Right-click on the newly created user and click on "Properties"
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720200846.png]]
<img width="950" height="666" alt="Pasted image 20250720200846" src="https://github.com/user-attachments/assets/cdbc6a76-ac17-42fc-8b4f-3f10c45100e1" />

Click on the "Member Of", then click "Add..."
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720200941.png]]
<img width="512" height="669" alt="Pasted image 20250720200941" src="https://github.com/user-attachments/assets/837134f4-416f-46b0-b16e-b6357aeaeaf0" />

Type in "domain admins" then click "Check Names". It will search for any groups that have the corresponding names. This should correct to the group "Domain Admins". ![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720201312.png]]
<img width="571" height="313" alt="Pasted image 20250720201312" src="https://github.com/user-attachments/assets/e3b6688b-8923-42b9-86d2-1a7162d305d5" />

Your user properties should now look like this
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720201342.png]]
<img width="512" height="378" alt="Pasted image 20250720201342" src="https://github.com/user-attachments/assets/1f533e0c-29ea-447d-9c23-f54456a04831" />

Now to make sure that everything is working right, log out of the Administrator account and log in using the new user credentials we just created. (This is not possible if you are using a RDP client)
# Part 2: Deploying RAS and NAT
Back in Server Manager, click on "Add Roles and Features". In the new window, click next until you get to "Server Selection". Make sure that you've selected the server you just created. Click Next and in the "Roles" menu, look for and check "Remote Access" on. 
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720205549.png]]
<img width="987" height="703" alt="Pasted image 20250720205549" src="https://github.com/user-attachments/assets/c321c63f-1c72-481e-80e4-614a20892ff0" />

Click Next until you get to the "Role Services" tab. Here, check on the "Routing" option. Click "Add Features", then click Next until you get to the "Confirmation" tab, where you can click "Install"
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720205850.png]]
<img width="518" height="517" alt="Pasted image 20250720205850" src="https://github.com/user-attachments/assets/763899f4-1073-41a4-a786-9326c0965902" />

![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250720205754.png]]
<img width="985" height="698" alt="Pasted image 20250720205754" src="https://github.com/user-attachments/assets/23959f0f-7d9a-4498-baaf-7a9f866cc4d0" />

I got to this point and my EC2 instance started slowing down like crazy, and I honestly got fed up with it. The instance was running quite slow, which may have been due to my network connection. I ultimately decided to switch to doing this in a VM which did present its own problem.

I got this error:
"Error determining whether the target server is already a domain controller: The domain controller promotion completed, but the server is not advertising as a domain controller."

In the end, since this is a lab, I ultimately deleted the VM and started fresh. It is definitely something I will look into since it may or may not pop up in companies that use Active Directory to manage organization and department access, users, permissions, etc.

After successfully configuring my new VM and setting everything up, I went to "Tools" then "Routing and Remote Access"
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250721192305.png]]
<img width="623" height="438" alt="Pasted image 20250721192305" src="https://github.com/user-attachments/assets/0d3afa3c-d089-471a-801a-04f6b95e06c4" />

Right-click on AD-DC (or whatever you named your Domain Controller)  and click "Configure and Enable Routing and Remote Access"
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250721202145.png]]
<img width="623" height="377" alt="Pasted image 20250721202145" src="https://github.com/user-attachments/assets/25911e2f-d396-495e-9c63-b2c6cce296d4" />

The installation wizard should pop up. Click Next. Click on "Network address translation" then click next
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250721202303.png]]
<img width="499" height="420" alt="Pasted image 20250721202303" src="https://github.com/user-attachments/assets/ac792f97-fcc3-416c-82d6-cc96fd3bdf27" />

You may encounter something like this:
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250721202329.png]]
<img width="474" height="411" alt="Pasted image 20250721202329" src="https://github.com/user-attachments/assets/0eaf8039-b2c2-45e2-af0b-9c69db2949b9" />

Here, we'll just click "Cancel" and close out from the "Routing and Remote Access" window. Go through the steps again starting from clicking on "Tools"

After going through the steps again, the box should be populated like this:
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250721202548.png]]
<img width="474" height="420" alt="Pasted image 20250721202548" src="https://github.com/user-attachments/assets/56410963-bd8a-4a20-8930-1f2fbbf1192b" />

Here we will click on "\_Internet_" then click Next and then Finish.

Once it's done installing, it should look something like this:D
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250721202951.png]] 
<img width="533" height="378" alt="Pasted image 20250721202951" src="https://github.com/user-attachments/assets/6e02725b-2d0b-4903-a0f3-93837592b618" />

AD-DC should be lit up with a green up arrow
and your dashbaord should look something like this
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250721203131.png]]
<img width="1729" height="877" alt="Pasted image 20250721203131" src="https://github.com/user-attachments/assets/c8858c73-0ef7-4a0a-9d23-60110732e7c8" />

# Part 3: Deploying DHCP
Now we start with the DHCP Server.
Click on "Add roles and features" and walk through it the same way we did last time when we added the "Remote Access" role.
Once you get to the Server Roles tab, check the box for "DHCP Server" 
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250721203612.png]]
<img width="678" height="554" alt="Pasted image 20250721203612" src="https://github.com/user-attachments/assets/526bd964-b2f5-488c-b62d-5296d0d9da13" />

Click "Add features". Click Next until you get to the end and then click Install.
Now go to Tools -> DHCP
Click on the arrow next to your DC so you see IPv4 and IPv6. Right-click IPv4 -> New Scope and the New Scope Wizard should pop up
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722000520.png]]
<img width="713" height="543" alt="Pasted image 20250722000520" src="https://github.com/user-attachments/assets/c3909a68-b821-4d42-8a9f-233b1961a056" />


For the Name, put in 172.16.0.100-200. It's just to describe what we're using the scope for. Click Next.

For IP Address Range, it should look like this:
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722001110.png]]
<img width="518" height="428" alt="Pasted image 20250722001110" src="https://github.com/user-attachments/assets/8af9477d-bc26-4634-ab21-085203246486" />

Click Next

We don't want to add any exclusion for the IP address range, so click Next here.
For Lease Duration, we can leave it as 8 days. 
For Configure DHCP Options, select the "Yes, I want to configure these options now" then click Next

For Router (Default Gateway), input 172.16.0.1 as the IP address and click Add. Click Next
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722001640.png]]
<img width="516" height="424" alt="Pasted image 20250722001640" src="https://github.com/user-attachments/assets/95bef525-eb1d-4fa4-b5c0-826b6e23e67a" />

![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722001655.png]]
<img width="516" height="424" alt="Pasted image 20250722001655" src="https://github.com/user-attachments/assets/d8f051e1-7734-4e6f-a31b-34a11eb8bd3c" />

For Domain Name and DNS Servers, it should be all setup now, but cross check that everything is fine and looks something similar to this:![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722002112.png]]
<img width="516" height="427" alt="Pasted image 20250722002112" src="https://github.com/user-attachments/assets/0ed3e386-d22e-4473-b746-866749679823" />

Click Next on WINS Server. 
For Activate Scope, make sure "Yes, I want to activate this scope now" is checked. Click Next -> Finish

Back in the DHCP Server window, right click on ad-dc.mydomain.com then click authorize. Right click again and click Refresh. Now both IPv4 and IPv6 should be lit up with a green checkmark.
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722002446.png]]
<img width="583" height="405" alt="Pasted image 20250722002446" src="https://github.com/user-attachments/assets/1105a526-33ed-4f30-abd8-94ca72d3ccb5" />

![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722002505.png]]
<img width="585" height="405" alt="Pasted image 20250722002505" src="https://github.com/user-attachments/assets/5f5777e2-a5f4-469b-b40b-90b7358238a4" />

# Part 4: Powershell Automation
Now we will use a Powershell script to create and populate the domain with users

To use the internet freely we can configure the server to allow us browse whatever we want. This should NOT be done in a production environment

So in the Server Manager Dashboard, Configure this local server -> IE Enhanced Security Configuration. Click "On" this option, and check both things off. Clcik OK
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722003203.png]]
<img width="419" height="449" alt="Pasted image 20250722003203" src="https://github.com/user-attachments/assets/e9d05a2f-5a78-46f3-823a-0a163405cfc4" />

This is just so we can download the script and text file containing the names we will be using for the powershell automation.

This is [Powershell Script and Names text file](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) that we used: https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1
Download the zip file and extract the folder. Save it somewhere you can easily access.
Verify the files are there
Go to Start -> Windows Powershell -> Right-click Windows Powershell ISE -> More -> Run as administrator
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722004912.png]]
<img width="771" height="680" alt="Pasted image 20250722004912" src="https://github.com/user-attachments/assets/8459b143-e3f5-4dd1-97d4-2db1b8752d83" />

In Powershell ISE, go to the top-left corner and look for the open folder icon and click it![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722005015.png]]
<img width="455" height="164" alt="Pasted image 20250722005015" src="https://github.com/user-attachments/assets/892d0ce4-98cc-475b-947e-6a2c42111dba" />

Navigate to where you saved the folder named AD_PS-master and open the powershell file named "1_CREATE_USERS"

![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722005210.png]]
<img width="834" height="574" alt="Pasted image 20250722005210" src="https://github.com/user-attachments/assets/64238aa2-f2b6-4451-943a-0a0d73509ca8" />

We won't be able to run the script just yet, so in the terminal (the blue box with PS C:\Windows\system32 in the photo), type in "Set-ExecutionPolicy Unrestricted" We're using Unrestricted because its a lab. This will let us run any scripts in this VM.

A dialog box will pop up, click "Yes to All"
[How Powershell script works](https://youtu.be/MHsI8hJmggI?si=jcB-OmCZkxfJoLVi&t=2129) for explanation on how the script works (Add comments to it in future)

Now we will want to cd into the folder containing our script and text file
`cd c:\users\a-soemede\Desktop\AD_PS-master` (Powershell is a bit different)

Click the play button at the top and you should see the script run in the terminal
To verify that the users are populating in AD, go to Start -> Windows Administrative Tools -> Active Directory Users and Computers

Click on your domain and you should a new folder called `_USERS` with the newly created users from the script we just ran

# Part 5: Client Virtual Machine Set-up
Set up the virtual machine as normal using a Windows 10 64-bit ISO file
Make sure for the network settings, you configure it to "Internal" instead of "NAT"
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722011450.png]]
<img width="956" height="595" alt="Pasted image 20250722011450" src="https://github.com/user-attachments/assets/aa2dfa62-640e-4d33-ae0a-d89a74bb821f" />

After starting the VM, it'll walk you through an initial setup. When it say Activate Windows, select "I don't have a product key". For the version of Windows, choose Windows 10 Pro.
For the type of installation we want, select "Custom" then click next![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722011828.png]]
<img width="638" height="476" alt="Pasted image 20250722011828" src="https://github.com/user-attachments/assets/0c65c5ed-3a9b-4b8c-83fa-9a6d924578a8" />

Now we just wait for Windows to install

Once Windows is done installing, we want to rename the PC and possibly join our domain at the same time. For this go to Settings -> System -> About. Scroll down till you see "Rename this PC (Advanced)" and click that ![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722020744.png]]
<img width="799" height="632" alt="Pasted image 20250722020744" src="https://github.com/user-attachments/assets/ae4de30b-9594-4233-8aa0-92f118645046" />

Click "Change" in the new window. For computer name, put in "CLIENT".
Check Domain and type in your domain name
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722020951.png]]
<img width="740" height="467" alt="Pasted image 20250722020951" src="https://github.com/user-attachments/assets/7e3dcccd-9b1a-42bd-84bf-784ba8be1f92" />

If this pops up:![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722021328.png]]
<img width="759" height="478" alt="Pasted image 20250722021328" src="https://github.com/user-attachments/assets/3914feef-717f-48c4-a482-92132b3bb3a9" />

Run ipconfig in the terminal/command prompt to check if we have a default gateway.

If something like this shows:![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722021410.png]]
<img width="557" height="277" alt="Pasted image 20250722021410" src="https://github.com/user-attachments/assets/b88df81b-cd9c-495e-a936-43b2b5e2f4ab" />

Go back to the domain controller VM and open DHCP (Tools -> DHCP) 
Right click on Server Options under IPv4, and select "Configure". Make sure that "003 Router" is checked, and add the IP address 172.16.0.1 so the window looks like this
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722021630.png]]
<img width="399" height="456" alt="Pasted image 20250722021630" src="https://github.com/user-attachments/assets/6fb2b243-7cfe-4ba0-996a-c3779e767a4e" />

Right-click on your domain name -> All Tasks -> Restart. Wait for that to run, and confirm we see 003 Router in Server Options
Go back to the CLIENT VM 
Now in the CLIENT VM, run `ipconfig /renew` and you should see that your VM has a default gateway now. Now we can rename the PC and join the domain 

This dialog box should pop up![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722024654.png]]
<img width="463" height="304" alt="Pasted image 20250722024654" src="https://github.com/user-attachments/assets/cd6c1887-f832-4fe6-8392-f862a3929a18" />

You can try the regular user you created. That should work, if not use your admin account.
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722024824.png]]
<img width="769" height="581" alt="Pasted image 20250722024824" src="https://github.com/user-attachments/assets/6fe903bb-f243-4fc5-81ca-c13ce25ac793" />

Go ahead and restart the machine![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722024837.png]]
<img width="775" height="572" alt="Pasted image 20250722024837" src="https://github.com/user-attachments/assets/2be68819-6ee5-44f0-b60d-1adc7152071a" />

After installation and setup, you should be able to use anyone of the users we created with the script to log in to the client VM
![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722025100.png]]
<img width="943" height="761" alt="Pasted image 20250722025100" src="https://github.com/user-attachments/assets/eb35399b-76e7-4748-a899-df12f415d575" />

Now head back to your DC machine and it should populate with the client machine once the client machine is done setting up.
To check, Start -> Windows Administrative Tools ->Active Directory Users and Computers -> Computers
BOOM! There we go![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722025423.png]]
<img width="750" height="522" alt="Pasted image 20250722025423" src="https://github.com/user-attachments/assets/862fa7c0-4454-481f-9ecc-4706b9efaafa" />

For further confirmation, we can go to the client machine and check if the user we used to log in is successfully connected to the domain![[Project Writeups/AD Powershell Lab/attachments/Pasted image 20250722025759.png]]
<img width="491" height="198" alt="Pasted image 20250722025759" src="https://github.com/user-attachments/assets/12bd3635-deb9-4206-90e0-5e59767d1826" />
