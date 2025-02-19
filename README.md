<p align="center"><img src="https://www.cybersecurityeducation.org/wp-content/uploads/2023/11/soc-analyst-750x350-1.jpg" alt="osTicket logo"/></p>

<h1>Create a Home SOC and Analyze Real-World Attack Data</h1>
This tutorial outlines how to set up a basic home SOC in Azure from scratch. Create a virtual machine (VM), opening it to the internet as a honeypot, and forwarding logs to a central repository. We then integrate Microsoft Sentinel to analyze real-world attack data.<br/>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure
- Event Viewer
- Remote Desktop Connection
- Microsoft Sentinel
- Log Analytics Workspace

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Set up the virtual machine
- Resource Group, Virtual Network, and Virtual Machine creation
- Edit Network Security Group
- Disable the internal Windows Firewall
- Ping the VM
- View the Raw Logs on the VM
- Create a Log Repository
- Create a Microsoft Sentinel for the Log Repository
- Using a Data Collection rule to forward the logs to the Log Repository
- Analyze and Filter the Log Repository
- Create a Watchlist with Geolocation Data for the SIEM
- Create an Attack Map

<h2>Create a Resource Group</h2>

<p>The first step is create a new resource group, near whatever server location is closest to you, and create.</p>

![image](https://github.com/user-attachments/assets/155d1550-4506-4e56-a518-f3d433051d2c)

<h2>Create a Virtual Network</h2>

<p>Create a virutal network, make sure it is in the same region as your resource group and put it in the resource group you just created. Leave all settings to default and create the virtual network.</p>

![image](https://github.com/user-attachments/assets/7ec347b9-0d03-4ac1-a793-ef90452c5396)

![image](https://github.com/user-attachments/assets/49ee6612-097a-405b-bbf5-7c31fb933dff)

<h2>Create a Virtual Machine</h2>

<p>Create the virtual machine, make sure it is in the VNet you just made, same region, and resource group. Name it something random, avoid the name "Honeypot" so people won't avoid attacking your VM. 

The virtual machine should have the following

- Image - Windowss 10 Pro, version 22H2 -x64 Gen2
- Size - Either 2 vCPUs so it's a more responsive experience or 1 vCPUs to save money.
- Username - labuser
- Password - Cyberlab123!
- In networking enable "Delete public IP and NIC when VM is deleted"
- In monitoring disable Boot diagnostics</p>

![image](https://github.com/user-attachments/assets/33ccd8da-d4fc-4726-b668-5398dc1aa6c3)

<h2>Edit the Network Security Group</h2>

<p>Go to the resource group and find the NSG. In this case it's called "CORP-NET-EAST-1-nsg".</p>

![image](https://github.com/user-attachments/assets/7c3b1077-c313-41b4-a719-df739929cdb8)

<p>Delete the 300 Default RDP rule and create an inbound rule that allows all sorts of traffic.</p>

![image](https://github.com/user-attachments/assets/c39d255e-42b9-4aad-9ab0-0ee8fe5f3275)

<p>Go to inbound security rules, create a new rule and set source port and destination port ranges to any.</p>

![image](https://github.com/user-attachments/assets/bfae1b9d-de3d-4e0b-a933-f3c5a91de74e)

<h2>Disable the Internal Windows Firewall in the VM</h2>

<p>Type "wf.msc" in the Windows search bar on the bottom left.</p>

![image](https://github.com/user-attachments/assets/18702295-6181-4a0b-9bc2-fc82c067dc92)

<p>Click Windows Defender Firewall Properties</p>

![image](https://github.com/user-attachments/assets/6d95a849-612c-4595-9978-c6c1b7781798)

<p>Set Firewall state to off for Domain Profile, Private Profile, and Public Profile</p>

![image](https://github.com/user-attachments/assets/902bb728-7891-428b-92f4-cd0e7bc13966)

<h2>Ping the Virtual Machine from your local computer</h2>

<p>We ping the VM to make sure it is accesseible from the public internet to make sure anyone in the world can access it.</p>

![image](https://github.com/user-attachments/assets/44dbf6da-2e24-4469-94b9-2cbcdd8642e4)

<h2>Viewing Raw Logs on the Virtual Machine</h2>

<p>Fail a log on attempt three times then log into the virtual machine with the right credentials.</p>

![image](https://github.com/user-attachments/assets/10270343-4e54-436d-9ccd-24be6148b891)

<p>Search up Event Viewer on the bottom left through the Windows search bar.</p>

![image](https://github.com/user-attachments/assets/c7173400-d765-45b0-a3ad-8dc3cba8bf2a)

<p>Select Windows Logs and then go to Security which shows your failed log in attempts.

Event ID 4625 means an account failed log in.</p>

![image](https://github.com/user-attachments/assets/8b5b6801-ed1e-40c1-8fd1-38a9123f44f5)

<h2>Creating Log Repository</h2>

<p>On Azure search up Log Analytics Workspace then create a Log Analytics Workspace

- Put it in the same resource group you created earlier
- Fill out the name 
- Make sure it is the same region as your resosurce group
- Then create the Log Analytics Workspace</p>

![image](https://github.com/user-attachments/assets/f4ed9f90-27f6-4b1d-8495-bcc734011655)

<h2>Create a Microsoft Sentinel</h2>

<p>Search up Microsoft Sentinel and create one. Click the Log Analytics Workspace you just created and add the Sentinel.</p>

![image](https://github.com/user-attachments/assets/ab3ccf0c-cfc3-4e37-b272-73ff971cef3f)

<h2>Connecting the VM to Log Analytics Workspace</h2>

<p>Select the Sentinel you just created, go to Content Hub under Content Management, and search up "Security Event".

Install Windows Security Events</p>

![image](https://github.com/user-attachments/assets/ca3779bb-14e0-49c3-9241-da355bc58043)

<p>Once it has finished installing you can click Manage on the bottom right.

Select "Windows Security Event via AMA" then Open Connector Page</p>

![image](https://github.com/user-attachments/assets/a2b258a1-c9b9-49d1-a297-5d72cb1f5d17)

<p>Create a Data Collection rule which forwards logs to the Log Analytics Workspace.</p>

![image](https://github.com/user-attachments/assets/838bb306-0cec-43ad-8544-db23434f878c)

<p>Make sure to select the virtual machine you are using for this tutorial under the Resources tab.</p>

![image](https://github.com/user-attachments/assets/4d9eab34-0429-4936-bbbe-e996ed8344a0)

<p>Go to Log Analytics Workspace select the one you created earlier and click Logs.

Search up SecurityEvent then click Run.</p>

![image](https://github.com/user-attachments/assets/56ff34f4-6861-4896-b3b7-757d5390f0c2)

<h2>Analyze and Filter the Law Analytics Workspace.</h2>

<p>Wait about 20-40 minutes for events to occur on your virtual machine
  
Select a random account of your choosing and type the following line to filter for that account specifically 

"| where Account == ACCOUNTNAME"

Remember since \ is a special character, add an extra slash</p>

![image](https://github.com/user-attachments/assets/e8782a82-bfbc-4873-b50a-1e1a66cdc7dc)

<p>You can add more specific filters by typing the following

"| project TimeGenerated, Account, Computer, EventID, Activity, IpAddress"</p>

![image](https://github.com/user-attachments/assets/a7882792-72ff-47c7-85db-8270b4638b69)

<p>You can search for specific EventID's such as failed log ons

"| where EventID == 4625"</p>

![image](https://github.com/user-attachments/assets/3bb247d8-747b-4b30-a1eb-c71871cd27cc)

<h2>Uploading the Geolocation Data to the SIEM</h2>

<p>Download the spreadsheet (https://drive.google.com/file/d/13EfjM_4BohrmaxqXZLB5VUBIz2sv9Siz/view)

Go to Sentinel, click the Sentinel you created, then click Watchlist and create a new Watchlist</p>

![image](https://github.com/user-attachments/assets/a283d15a-c0f9-4854-b433-cc4afaf0e9fb)

<p>Within the Watchlist add the following 

- Name - geoip
- Alias - geoip
- Source Type - Local File then upload the file you downloaded earlier
- Searchkey - "network"

Create the wizard, it will take a while to upload (10-20 minutes)</p>

![image](https://github.com/user-attachments/assets/9af54aa0-5db5-4161-ba61-daf88eaefdfb)

<h2>Create the Attack Map</h2>

<p>Search up Sentinel, click the instance you made for the lab, under Threat Management click Workbooks, and create a new Workbook</p>

![image](https://github.com/user-attachments/assets/8982c7a0-ed12-4cd3-a0aa-d4cc96edaec5)

<p>Click edit and remove the elements that the workbook was prepopulated with.</p>

![image](https://github.com/user-attachments/assets/b5bba263-9b39-49c0-bdf8-00ecd450e674)

<p>Click the following link (https://drive.google.com/file/d/1ErlVEK5cQjpGyOcu4T02xYy7F31dWuir/view) and copy all of the content</p>

![image](https://github.com/user-attachments/assets/fe6a0fe3-6798-41c7-9b45-9f6988254e32)

<p>Go to the workbook, add a query, click advanced editor</p>

![image](https://github.com/user-attachments/assets/24ddee25-08db-48b6-b837-aff8d62c48f4)

<p>Erase all the contents and replace with the content you copied from the link and save your changes</p>

![image](https://github.com/user-attachments/assets/c76aed4f-ac29-4b75-8e95-f50fe8df2037)

![image](https://github.com/user-attachments/assets/c765147f-7387-412d-b41a-05fb12dbfa78)

<p>Click Save, add it to the resource group and rename the Workbook</p>

![image](https://github.com/user-attachments/assets/4239c397-fb57-47bf-8a72-d2f9998b083e)

<p>The attack map will show the locations of the attackers attempting to log on to your VM. The longer you keep the VM online, the more populated the map will be.</p>

<hr>

<p>Congratulations you have successfully set up a home SOC on a virtual machine and analyzed the attacks through this tutorial.

Now that we're finished, make sure to CLEAN UP YOUR AZURE ENVIRONMENT to avoid any unnecessary charges.

Remember to close your Remote Desktop connection, delete the Resource Group(s) you created at the start of the tutorial, and verify that the Resource Group has been successfully deleted.</p>
