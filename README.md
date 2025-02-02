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
+ Windows 10 (22H2)
# List of Prerequisites
+ Azure Virtual Machine
+ [osTicket Installation files](https://drive.google.com/drive/u/0/folders/1APMfNyfNzcxZC6EzdaNfdZsUwxWYChf6)
# Installation Steps
Within the Azure interface, create a resource group and name it "osTickets". I used the US East region but you can choose whichever one best fits your needs. Just ensure that your VM and other resources are all within the same region. Within that RG, create a Windows 10 VM that has 2-4vCPUs. Name it "VM-osTicket". Create a username and password for the VM. For training purposes you can keep it basic. For the username I chose "labuser". 

<img width="949" alt="SS1" src="https://github.com/user-attachments/assets/6e62744a-5046-4b16-8133-e0d8731065ca" />

Once the VM is deployed, we will connect to it using a Remote Desktop connection. First take note of your VM's public IP address. From the Start menuon your computer, open up Remote Desktop Connection and connect to the VM that was created in Azure. This can also be done within the Azure interface by navigating to the VM then select connect then Native RDP.

<img width="916" alt="SS2" src="https://github.com/user-attachments/assets/304871b1-ddb1-4314-9552-ddf0646ddf9d" />

After connecting, it's time to enable Internet Information Services (IIS). IIS is a web server for Windows that's used to exchange static and dynamic web content with internet users. It can be used to host, deploy, and manage different web applications. To enable IIS: Start Menu > Windows Features > Internet Information Services > World Wide Web Services > Application Development Features > CGI. CGI will be necessary for downloading the PHP Manager which will allow us to run osTicket within our IIS web server.

<img width="675" alt="SS3" src="https://github.com/user-attachments/assets/c9a616ff-60fb-4b02-96d8-92f85eed0aad" />

From within the installation folder, download the PHP manager file (PHPManagerForIIS_V1.5.0.msi). Run the installer and agree to all the terms. Follow the same steps now with the Rewrite Module file (rewrite_amd64_en-US.msi). 

<img width="654" alt="SS4" src="https://github.com/user-attachments/assets/ca8d7f85-e826-4a70-8408-284d738de674" />
<img width="614" alt="SS5" src="https://github.com/user-attachments/assets/a2cda90c-d21d-4296-b70b-215c312d1662" />

Create a directory named "C:\PHP" by opening the File Explorer. Navigate to the C:\ drive and right-click to create a new folder named "PHP". From within the Installation Files folder, download the zip file "php-7.3.8-nts-Win32-VC15-x86.zip". Extract the files to the PHP folder you just created. 

<img width="728" alt="SS6" src="https://github.com/user-attachments/assets/f937a80c-1243-473c-94c6-dd040a95cd62" />

Now, download and install the VC_redist executable file. Then you'll download and install MySQL (mysql-5.5.62-win32.msi). Allow typical setup and it will bring you to a page where you will create a username and password for the database you'll be using to store ticket information from oSTicket. To keep things simple I chose "root" for the username and "Password1" for the password. 

<img width="547" alt="SS7" src="https://github.com/user-attachments/assets/ef982177-35f3-49d2-93c9-5b93808462e4" />

Now it's time to download oSTicket which is "osTicket-v1.15.8.zip" in the installation files folder. Once it's downloaded, extract the contents and copy the "upload" folder into C:\ inetpub\wwwroot. Then rename the copied folder "osTicket". Now open the IIS Manager and restart the server.

<img width="754" alt="SS8" src="https://github.com/user-attachments/assets/39c76ed2-2c8f-4483-949c-21de7ff742a3" />

On the left-hand side underneath Connections, select the VM > Sites > Default Web Site > oSTicket. On the right-hand side click "Browse *:80". You should receive an error. This error is because there are some extensions we need to enable.

<img width="788" alt="SS9" src="https://github.com/user-attachments/assets/d423f5df-e761-4aed-bcb1-98fabad5ccac" />

Now we will enable extensions in IIS. While in IIS > Sites > Default Web Site > oSTicket, double-click PHP Manager. At the bottom click "Enable or Disable an Extension". Right-click and enable the extensions:
+ php_imap.dll
+ php_intl.dll
+ php_opcache.dll

<img width="733" alt="SS10" src="https://github.com/user-attachments/assets/d4d18c6d-43eb-4dd7-add3-4c006a72eb07" />

Refresh the oSTicket site in your web browser and you should now see a green check next to Intl Extension. Now navigate to C:\inetpub\wwwroot\oSTicket\include\ost-sampleconfig.php and rename the file to C:\inetpub\wwwroot\oSTicket\include\ost-config.php.

<img width="700" alt="SS11" src="https://github.com/user-attachments/assets/476b68aa-dbf6-4078-94e6-e8419ba00726" />

Next, assign permissions to ost-config.php by right-clicking the file and open Properties > Security > Advanced > Permissions. Click Disable inheritance > Remove all inherited permissions from this object.

<img width="704" alt="SS12" src="https://github.com/user-attachments/assets/d11382f8-30bd-4dd4-ae00-a03464f97ed9" />

While still in the Permissions settings of ost-config.php, click Add > Principal and type in "Everyone". Check the names then click OK. Allow everyone full control by ensuring all boxes are checked. Apply the changes.

<img width="737" alt="SS13" src="https://github.com/user-attachments/assets/8fbc7f58-bd15-4677-aa5f-bfa634d6f287" />

Now we will continue setting up osTicket from within the web browser using the following parameters:

<img width="819" alt="SS14" src="https://github.com/user-attachments/assets/02afb294-81ed-4097-95d7-b4a3cb896922" />

Before we finish installing osTicket, lets setup our database. First, let's download and install HeidiSQL (HeidiSQL_12.3.0.6589_Setup.exe) from the provided installations folder. HeidiSQL is an open-source admin tool for managing databases. After it finishes installing, open HeidiSQL and create a new session. User: root Password: Password1. Select Open and on the left side right-click Unnamed > Create New > Database. Name it osTicket and click OK.

<img width="766" alt="SS15" src="https://github.com/user-attachments/assets/cba7cd3b-9fde-4d76-982f-7b7f1613b271" />

Let's finish setting up osTicket. In the web browser finish filling out the fields:

<img width="752" alt="SS16" src="https://github.com/user-attachments/assets/394a7047-e064-43f1-b8cc-feb334f0fd44" />

Finish by clicking "Install Now". Congrats! osTicket has been successfully installed. Don't forget to complete some post-installation cleanup. First, delete C:\inetpub\wwwroot\osTicket\setup. Next, navigate to C:\inetpub\wwwroot\osTicket\include. Right-click on ost-config.php and select Security > Advanced > "everyone" > edit to change permissions. Allow everyone to only have read and execute permissions.

<img width="771" alt="SS17" src="https://github.com/user-attachments/assets/ecfa8ba5-bb41-41b9-94b0-722cb261bfd5" />

For information on post-installation configuration, see pt. 2 [here](https://github.com/nahjac/osTicket-Post-Installation-Configuration).
