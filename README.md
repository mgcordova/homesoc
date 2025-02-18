<p align="center"><img src="https://www.cybersecurityeducation.org/wp-content/uploads/2023/11/soc-analyst-750x350-1.jpg" alt="osTicket logo"/></p>

<h1>Create a Home SOC and Analyze Real-World Attack Data</h1>
This tutorial outlines how to set up a basic home SOC in Azure from scratch. Create a virtual machine (VM), opening it to the internet as a honeypot, and forwarding logs to a central repository. We then integrate Microsoft Sentinel to analyze real-world attack data.<br/>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure
- Microsoft Ssentinel

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Set up the virtual machine

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

Image - Windowss 10 Pro, version 22H2 -x64 Gen2
Size - Either 2 vCPUs so it's a more responsive experience or 1 vCPUs to save money.
Username - labuser
Password - Cyberlab123!
Disable Boot diagnostics</p>

![image](https://github.com/user-attachments/assets/33ccd8da-d4fc-4726-b668-5398dc1aa6c3)

<h2>Edit the Network Security Group</h2>

<p>Go to the resource group and find the NSG. In this case it's called "CORP-NET-EAST-1-nsg".</p>

![image](https://github.com/user-attachments/assets/7c3b1077-c313-41b4-a719-df739929cdb8)

<p>Delete the 300 Default RDP rule and create an inbound rule that allows all sorts of traffic.</p>

![image](https://github.com/user-attachments/assets/c39d255e-42b9-4aad-9ab0-0ee8fe5f3275)

<p>Go to inbound security rules and set source port and destination port ranges to any.</p>

![image](https://github.com/user-attachments/assets/bfae1b9d-de3d-4e0b-a933-f3c5a91de74e)

<h2>Disable the Internal Windows Firewall</h2>
