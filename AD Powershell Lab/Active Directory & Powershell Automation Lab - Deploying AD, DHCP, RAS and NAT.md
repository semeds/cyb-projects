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

![](attachments/Pasted%20image%2020250720164908.png)

Next, open VirtualBox. Click new and a small window should pop up

For name, Windows Server 2022 is fine. 
For the ISO image, navigate to where you downloaded your image on your laptop.'
For Edition: Select Windows Server 2022 Standard Evaluation (Desktop Experience). That is the option we will be using for this lab as you will be using the GUI of this VM.
The settings should resemble something like this:

![](attachments/Pasted%20image%2020250723031948.png)

![](attachments/Pasted%20image%2020250723032123.png)



For the Network settings, Adapter 1 should be set to NAT and Adapter 2 should be set to Internal Network
![](attachments/Pasted%20image%2020250723031803.png)

![](attachments/Pasted%20image%2020250723031822.png)




## Step 1: Launch Windows Server Instance

I have previously provisioned a functioning instance of Windows Server 2025 using this guide here. This allows you use a Remote Desktop client ()RDP) for this lab.

## Step 2: Server Manager Setup

Now in the search bar at the bottom of the RDP, look up "Server Manager" and click open. It should look like this:
![](attachments/Pasted%20image%2020250720170446.png)

Once opened, you should have a window that looks like this:
![](attachments/Pasted%20image%2020250720171311.png)


If you see a bar loading at the top, it's just preparing the app for you. Once that is done. Click "Add roles and features" and this window should pop up:
![](attachments/Pasted%20image%2020250720171529.png)


Click next until you get to that tab that says "Server Roles". There you will select the following roles:
- Active Directory Domain Services
This window pops up:
![](attachments/Pasted%20image%2020250720172114.png)

Click "Add Features"

Then click Next until you get to the end. Then finally click "Install". The installation may take a while, so just we'll wait until everything is ready.
![](attachments/Pasted%20image%2020250720174630.png)


## Step 3: Configuring Active Directory Domain Services
Once everything is done installing, you will notice a flag 
with a hazard symbol in the top-right corner of the window. 
![](attachments/Pasted%20image%2020250720180305.png)

Click on this flag then click "Promote this server to domain controller" and another window will pop up:
![](attachments/Pasted%20image%2020250720180402.png)

In this window, select "Add new forest" so we can input a new domain name.
For root domain name, it can be whatever you desire, but we will put in "mydomain.com" for the purpose of this lab.
![](attachments/Pasted%20image%2020250720180717.png)

Click Next.

You'll be prompted to create a password so put what you want in. For this lab, we'll put in "Password1":
![](attachments/Pasted%20image%2020250720181020.png)

Click Next. Verify that the NetBIOS name says "MYDOMAIN" if you used mydomain.com in the earlier steps
![](attachments/Pasted%20image%2020250720181528.png)

Then click next until you get to the prerequisites tab. You should see a green checkmark at the top saying all prereqs were passed successfully showing that we set everything up correctly. Here you click install.
![](attachments/Pasted%20image%2020250720181919.png)

Once the install is complete, your RDP/VM will restart. For the RDP, reconnect the same way that you did when you first connected. 

You'll notice on the login page that the username changes from Administrator to MYDOMAIN\Administrator, showing that the addition of the domain was successful
![](attachments/Pasted%20image%2020250720190758.png)


## Step 4: Creating Users in Active Directory
Navigate to the Start menu and search "Active Directory Users and Computers" (don't need to search the whole thing fr, just "Active Directory" is fine and the "Users and Computers" option will pop up)

Click on that to open a new window and you'll see something similar to the photo below.
![](attachments/Pasted%20image%2020250720191513.png)


Right-click on mydomain.com (or whatever your domain name is), navigate to "New", then "[[Organizational Units]]". Click on that and we'll have a new pop up:
![](attachments/Pasted%20image%2020250720191646.png)


In here, type "_ADMINS"_  and click "OK". The new folder should appear at the bottom
![](attachments/Pasted%20image%2020250720192215.png)


Right-click on the _ADMINS_ folder go to "New" then click "User"
![](attachments/Pasted%20image%2020250720192159.png)



This window should pop up:
![](attachments/Pasted%20image%2020250720192535.png)


You can fill this in with whatever you want. Then click next.

Here, type in a password, and have the checkboxes filled in the same way as the photo. Take note that we NEVER want to have a password that never expires. This is only checked for lab purposes. We would not have this box in a real-life scenario.
![](attachments/Pasted%20image%2020250720192756.png)


After that click next, then click Finish. Your windows should now have the newly created user
![](attachments/Pasted%20image%2020250720193156.png)

## Step 5: Granting Permissions
Right-click on the newly created user and click on "Properties"
![](attachments/Pasted%20image%2020250720200846.png)

Click on the "Member Of", then click "Add..."
![](attachments/Pasted%20image%2020250720200941.png)

Type in "domain admins" then click "Check Names". It will search for any groups that have the corresponding names. This should correct to the group "Domain Admins". 
![](attachments/Pasted%20image%2020250720201312.png)

Your user properties should now look like this
![](attachments/Pasted%20image%2020250720201342.png)

Now to make sure that everything is working right, log out of the Administrator account and log in using the new user credentials we just created. (This is not possible if you are using a RDP client)
# Part 2: Deploying RAS and NAT
Back in Server Manager, click on "Add Roles and Features". In the new window, click next until you get to "Server Selection". Make sure that you've selected the server you just created. Click Next and in the "Roles" menu, look for and check "Remote Access" on. 
![](attachments/Pasted%20image%2020250720205549.png)

Click Next until you get to the "Role Services" tab. Here, check on the "Routing" option. Click "Add Features", then click Next until you get to the "Confirmation" tab, where you can click "Install"
![](attachments/Pasted%20image%2020250720205850.png)


![](attachments/Pasted%20image%2020250720205754.png)

I got to this point and my EC2 instance started slowing down like crazy, and I honestly got fed up with it. The instance was running quite slow, which may have been due to my network connection. I ultimately decided to switch to doing this in a VM which did present its own problem.

I got this error:
"Error determining whether the target server is already a domain controller: The domain controller promotion completed, but the server is not advertising as a domain controller."

In the end, since this is a lab, I ultimately deleted the VM and started fresh. It is definitely something I will look into since it may or may not pop up in companies that use Active Directory to manage organization and department access, users, permissions, etc.

After successfully configuring my new VM and setting everything up, I went to "Tools" then "Routing and Remote Access"

![](attachments/Pasted%20image%2020250721192305.png)

Right-click on AD-DC (or whatever you named your Domain Controller)  and click "Configure and Enable Routing and Remote Access"
![](attachments/Pasted%20image%2020250721202145.png)

The installation wizard should pop up. Click Next. Click on "Network address translation" then click next
![](attachments/Pasted%20image%2020250721202303.png)

You may encounter something like this:
![](attachments/Pasted%20image%2020250721202329.png)

Here, we'll just click "Cancel" and close out from the "Routing and Remote Access" window. Go through the steps again starting from clicking on "Tools"

After going through the steps again, the box should be populated like this:
![](attachments/Pasted%20image%2020250721202548.png)

Here we will click on "\_Internet_" then click Next and then Finish.

Once it's done installing, it should look something like this:
![](attachments/Pasted%20image%2020250721202951.png)

AD-DC should be lit up with a green up arrow
and your dashbaord should look something like this
![](attachments/Pasted%20image%2020250721203131.png)

# Part 3: Deploying DHCP
Now we start with the DHCP Server.
Click on "Add roles and features" and walk through it the same way we did last time when we added the "Remote Access" role.
Once you get to the Server Roles tab, check the box for "DHCP Server" 
![](attachments/Pasted%20image%2020250721203612.png)

Click "Add features". Click Next until you get to the end and then click Install.
Now go to Tools -> DHCP
Click on the arrow next to your DC so you see IPv4 and IPv6. Right-click IPv4 -> New Scope and the New Scope Wizard should pop up
![](attachments/Pasted%20image%2020250722000520.png)


For the Name, put in 172.16.0.100-200. It's just to describe what we're using the scope for. Click Next.

For IP Address Range, it should look like this:
![](attachments/Pasted%20image%2020250722001110.png)

Click Next

We don't want to add any exclusion for the IP address range, so click Next here.
For Lease Duration, we can leave it as 8 days. 
For Configure DHCP Options, select the "Yes, I want to configure these options now" then click Next

For Router (Default Gateway), input 172.16.0.1 as the IP address and click Add. Click Next
![](attachments/Pasted%20image%2020250722001640.png)

![](attachments/Pasted%20image%2020250722001655.png)

For Domain Name and DNS Servers, it should be all setup now, but cross check that everything is fine and looks something similar to this:
![](attachments/Pasted%20image%2020250722002112.png)

Click Next on WINS Server. 
For Activate Scope, make sure "Yes, I want to activate this scope now" is checked. Click Next -> Finish

Back in the DHCP Server window, right click on ad-dc.mydomain.com then click authorize. Right click again and click Refresh. Now both IPv4 and IPv6 should be lit up with a green checkmark.

![](attachments/Pasted%20image%2020250722002446.png)


![](attachments/Pasted%20image%2020250722002505.png)

# Part 4: Powershell Automation
Now we will use a Powershell script to create and populate the domain with users

To use the internet freely we can configure the server to allow us browse whatever we want. This should NOT be done in a production environment

So in the Server Manager Dashboard, Configure this local server -> IE Enhanced Security Configuration. Click "On" this option, and check both things off. Click OK
![](attachments/Pasted%20image%2020250722003203.png)

This is just so we can download the script and text file containing the names we will be using for the powershell automation.

This is [Powershell Script and Names text file](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) that we used: https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1
Download the zip file and extract the folder. Save it somewhere you can easily access.
Verify the files are there
Go to Start -> Windows Powershell -> Right-click Windows Powershell ISE -> More -> Run as administrator
![](attachments/Pasted%20image%2020250722004912.png)

In Powershell ISE, go to the top-left corner and look for the open folder icon and click it
![](attachments/Pasted%20image%2020250722005015.png)

Navigate to where you saved the folder named AD_PS-master and open the powershell file named "1_CREATE_USERS"
![](attachments/Pasted%20image%2020250722005210.png)

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
![](attachments/Pasted%20image%2020250722011450.png)

After starting the VM, it'll walk you through an initial setup. When it say Activate Windows, select "I don't have a product key". For the version of Windows, choose Windows 10 Pro.
For the type of installation we want, select "Custom" then click next
![](attachments/Pasted%20image%2020250722011828.png)


Now we just wait for Windows to install

Once Windows is done installing, we want to rename the PC and possibly join our domain at the same time. For this go to Settings -> System -> About. Scroll down till you see "Rename this PC (Advanced)" and click that 
![](attachments/Pasted%20image%2020250722020744.png)

Click "Change" in the new window. For computer name, put in "CLIENT".
Check Domain and type in your domain name
![](attachments/Pasted%20image%2020250722020951.png)


If this pops up:![](attachments/Pasted%20image%2020250722021328.png)


Run ipconfig in the terminal/command prompt to check if we have a default gateway.

If something like this shows:![](attachments/Pasted%20image%2020250722021410.png)


Go back to the domain controller VM and open DHCP (Tools -> DHCP) 
Right click on Server Options under IPv4, and select "Configure". Make sure that "003 Router" is checked, and add the IP address 172.16.0.1 so the window looks like this
![](attachments/Pasted%20image%2020250722021630.png)


Right-click on your domain name -> All Tasks -> Restart. Wait for that to run, and confirm we see 003 Router in Server Options
Go back to the CLIENT VM 
Now in the CLIENT VM, run `ipconfig /renew` and you should see that your VM has a default gateway now. Now we can rename the PC and join the domain 

This dialog box should pop up
![](attachments/Pasted%20image%2020250722024654.png)


You can try the regular user you created. That should work, if not use your admin account.
![](attachments/Pasted%20image%2020250722024824.png)


Go ahead and restart the machine![](attachments/Pasted%20image%2020250722024837.png)


After installation and setup, you should be able to use anyone of the users we created with the script to log in to the client VM
![](attachments/Pasted%20image%2020250722025100.png)


Now head back to your DC machine and it should populate with the client machine once the client machine is done setting up.
To check, Start -> Windows Administrative Tools ->Active Directory Users and Computers -> Computers
BOOM! There we go![](attachments/Pasted%20image%2020250722025423.png)


For further confirmation, we can go to the client machine and check if the user we used to log in is successfully connected to the domain![](attachments/Pasted%20image%2020250722025759.png)

