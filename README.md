![image](https://github.com/user-attachments/assets/9898bdf6-5ad1-4f64-8d30-c8627afc20e9)
# osTicket Prerequisites and Installation
This tutorial will outline the prerequisites and installation of the open-source help desk ticketing system osTicket.
# What is osTicket?
osTicket is an open-source ticketing system used by businesses for managing customer inquiries and support requests.
# Environments and Technologies Used
+ Microsoft Azure
+ Remote Desktop
+ Internet Information Services (IIS)
+ Heidi SQL
# Operating Systems Used
+ Windows 10 (21H2)
# List of Prerequisites
+ Azure Virtual Machine
+ osTicket Installation files
# Installation Steps
Within the Azure interface, create a resource group and name it "osTickets". I used the US East region but you can choose whichever one best fits your needs. Just ensure that your VM and other resources are all within the same region. Within that RG, create a Windows 10 VM and has a size of 4vCPUs. Name it "VM-osTicket". 
*pic of vm parameters*
Create a username and password for the VM. For training purposes you can keep it basic. For the username I chose "labuser". Once the VM is deployed, we will connect to it using a Remote Desktop connection. Navigate to the VM and make note of its public IP address. For me, the IP address is "MORE". 
