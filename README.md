<img width="720" height="356" alt="image" src="https://github.com/user-attachments/assets/440abf1a-4720-49a2-a3c8-c54b7eaccadf" />



#Deployment of a Honey pot in the Azure cloud. 

###Overview
This project was set up in such a way it would be enticing to an adversary when scanning the public internet. This was set up entireily in Azure's cloud. 

## Tools used and implemented
- Azure Cloud
- Microsoft Sentinel (SIEM Solution available in Azure)
- Windows Event Viewer
- Kusto Query Language (KQL)
- JSON Attack Map
- Geolocation Data CSV


# Part 1. Setup Azure Subscription

Create Free Azure Subscription: https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account. 

After your subscription is created, you can login at:
https://portal.azure.com. 
- From here go to the Azure dashboard, and create a resource group: (This can be thought of as a folder/container where all the components that make up this lab will go)
  <img width="976" height="152" alt="image" src="https://github.com/user-attachments/assets/820f0887-7726-4312-a0e5-0dae3f742b7e" />
  <img width="574" height="857" alt="Pasted image 20251201154422" src="https://github.com/user-attachments/assets/a8036cd9-3ff9-4e9d-a156-77aadee2c60f" />


# Part 2 Create the Virtual Network. 
- This is fairly simple, simply, from the dashboard, click on Virtual networks, then click create, follow the prompts, review and create.
  
  <img width="778" height="155" alt="image" src="https://github.com/user-attachments/assets/037261d2-f3ff-4e8a-9517-28ed49befb2f" />
  <img width="870" height="332" alt="image" src="https://github.com/user-attachments/assets/6bfc9b2a-eef3-4baf-b790-e0ba5dc87b7f" />
  <img width="764" height="731" alt="Pasted image 20251201154521" src="https://github.com/user-attachments/assets/b2c58e41-380f-4540-af6d-9a757cf24c7e" />
  <img width="768" height="558" alt="Pasted image 20251201154537" src="https://github.com/user-attachments/assets/233a8265-790e-4ea7-ace3-ff2c4a620859" />


# Part 3 Set up the honey pot VM (Windows 11 pro VM) Go to: https://portal.azure.com and search for virtual machines or use the search bar located at the top and search virtual machines. 

<img width="519" height="170" alt="image" src="https://github.com/user-attachments/assets/64e428f9-fb65-41a5-8086-46ac82dc364f" />

Then click create

<img width="729" height="377" alt="image" src="https://github.com/user-attachments/assets/9b5489d6-18f9-4b1c-b6b9-b976e8d5f4e2" />

From here, we will fill out and select the details of our VM. It's important to ensure that all components are located in the same region. For example, if the Subscription, the virtual network, and other components are in US EAST1, then the VM and everything else should be in that same region. 


<img width="741" height="848" alt="Pasted image 20251201212427" src="https://github.com/user-attachments/assets/db952252-f647-4514-9d1b-6864d5e309be" />

<img width="1420" height="226" alt="Pasted image 20251201213020" src="https://github.com/user-attachments/assets/81812a1e-ac3b-47a9-a97c-6a056034446c" />


# Step 4 - Configure Azure's Network Security Group to accept any inbound traffic. 
The NSG (Network Security Group) can be thought of as a firewall in Azure's cloud. 



Log into your virtual machine and turn off the windows firewall (start -> wf.msc -> properties -> all off)


Part 3. Logging into the VM and inspecting logs

Fail 3 logins as “employee” (or some other username)

Login to your virtual machine

Open up Event Viewer and inspect the security logs

See the 3 failed logins as “employee”, event ID 4625

Next, we are going to create a central log repository called a LAW



Part 4. Log Forwarding and KQL

Create Log Analytics Workspace

Create a Sentinel Instance and connect it to Log Analytics

(observe architecture)

Configure the “Windows Security Events via AMA” connector

Create the DCR within sentinel, watch for extension creation

Query for logs within the LAW


We can now query the Log analytics workspace as well as the SIEM, sentinel directly, which we will do soon

Note: Querying logs in here is a really important skill that you MUST have if you want to work in security operations. Depending on where you work, you need to know SQL, KQL, or SPL, but these are all basically the same thing. If you know one, you can easily learn the others. Microsoft and Sentinel uses KQL, which you can learn in the Cyber Range https://skool.com/cyber-range, or from here https://kc7cyber.com/ (free)

Tip: The Cyber Range is basically a full production environment with hundreds of users and Virtual Machines in it, which are all producing a ton of logs. It’s a really good place to practice just sifting through logs and seeing what you can see.

Observe some of your VM logs:

SecurityEvent
| where EventId == 4625

(observe architecture)

Part 5. Log Enrichment and Finding Location Data

Observe the SecurityEvent logs in the Log Analytics Workspace; there is no location data, only IP address, which we can use to derive the location data.

We are going to import a spreadsheet (as a “Sentinel Watchlist”) which contains geographic information for each block of IP addresses.

Download: geoip-summarized.csv https://raw.githubusercontent.com/joshmadakor1/lognpacific-public/refs/heads/main/misc/geoip-summarized.csv 

Within Sentinel, create the watchlist:

Name/Alias: geoip
Source type: Local File
Number of lines before row: 0
Search Key: network

Allow the watchlist to fully import, there should be a total of roughly 54,000 rows.

In real life, this location data would come from a live source or it would be updated automatically on the back end by your service provider.

(observe architecture)

Observe the logs now have geographic information, so you can see where the attacks are coming from

let GeoIPDB_FULL = _GetWatchlist("geoip");
let WindowsEvents = SecurityEvent
    | where IpAddress == <attacker IP address>
    | where EventID == 4625
    | order by TimeGenerated desc
    | evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network);
WindowsEvents


(observe architecture)

Part 6. Attack Map Creation

Within Sentine, create a new Workbook

Delete the prepopulated elements and add a “Query” element

Go to the advanced editor tab, and paste the JSON





