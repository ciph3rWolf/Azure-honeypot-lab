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
- 
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








