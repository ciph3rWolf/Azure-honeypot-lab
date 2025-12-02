<img width="720" height="356" alt="image" src="https://github.com/user-attachments/assets/440abf1a-4720-49a2-a3c8-c54b7eaccadf" />



#Deployment of a Honey pot in the Azure cloud. 




1. Go to portal.azure.com 
2. Once subscription is set up
	1. Set up Resource Group
	2. Vnet - Virtual Network (This is my network infrastructure)
	3. VM
	4. NSG - Network Security group. 


-------- 
1. Create resource group
	1. ![[Pasted image 20251201154422.png]]
2. Create VNET
	1. ![[Pasted image 20251201154521.png]]
	2. ![[Pasted image 20251201154537.png]]
3. Create VM 
	1. socuser:Donapatas2025 
	2. ![[Pasted image 20251201212427.png]]
	3. ![[Pasted image 20251201213020.png]]
4. Set up inbound rules on security group to allow any type of traffic to come in through Azure's NSG
	1. ![[Pasted image 20251201213553.png]]
5. Disable Windows FW on the VM 
	1. ![[Pasted image 20251201221240.png]]
	2. Confirmed successful ping responses from local machine
		1. ![[Pasted image 20251201221404.png]]
6. Set up Log Analytics workspace.
	1. This is what will help us collect logs being sent from the virtual machine that was left intentionally exposed. 
	2. ![[Pasted image 20251201222435.png]]
	3. ![[Pasted image 20251201223341.png]]
	4. Set up Sentinel (SIEM Solution to view and analyze logs)
		1. Configured Sentinel and the Windows security event agent to forward logs from the VM to the SIEM
		2. ![[Pasted image 20251201224540.png]]
		3. Create a data collection rule
			1. ![[Pasted image 20251201225128.png]]
			2. ![[Pasted image 20251201225238.png]]
			3. Confirmed log ingestion working as intended
				1. ![[Pasted image 20251201230514.png]]
		4. Set up a watchlist in Microsoft Sentinel utilizing a csv file. This will allow us to pin an IP to location in a map.
			1. ![[Pasted image 20251201232928.png]]
			2. ![[Pasted image 20251202140653.png]]
