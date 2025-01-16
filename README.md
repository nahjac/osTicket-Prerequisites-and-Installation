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
+ Windows 10 (Build 19044)
# List of Prerequisites
+ Azure Virtual Machine
+ osTicket Installation files
# Installation Steps
Within the Azure interface, create a resource group and name it "osTickets". I used the US East region but you can choose whichever one best fits your needs. Just ensure that your VM and other resources are all within the same region. Within that RG, create a Windows 10 VM that has 2-4vCPUs. Name it "VM-osTicket". 
*pic of vm parameters*
Create a username and password for the VM. For training purposes you can keep it basic. For the username I chose "labuser". Once the VM is deployed, we will connect to it using a Remote Desktop connection. From the Start menu, open up Remote Desktop Connection and connect to the VM that was created in Azure.
*pic of remote desktop connection with vm selected*
After connecting, it's time to enable Internet Information Services (IIS). IIS is a web server for Windows that's used to exchange static and dynamic web content with internet users. It can be used to host, deploy, and manage different web applications. To enable IIS: Start Menu > Windows Features > Internet Information Services > World Wide Web Services > Application Development Features > CGI. CGI will be necessary for downloading the PHP Manager which will allow us to run osTicket within our IIS web server.
*pic of windows features*
From within the installation folder, download the PHP manager file (PHPManagerForIIS_V1.5.0.msi). Run the installer and agree to all the terms. Follow the same steps now with the Rewrite Module file (rewrite_amd64_en-US.msi). 
*pic of both installers*
Create a directory named "C:\PHP" by opening the File Explorer. Navigate to the C:\ drive and right-click to create a new folder named "PHP". From within the Installation Files folder, download the zip file "php-7.3.8-nts-Win32-VC15-x86.zip". Extract the files to the PHP folder you just created. 
*pic of files extracted in php folder*
Now, download and install the VC_redist executable file. Then you'll download and install MySQL (mysql-5.5.62-win32.msi). Allow typical setup and it will bring you to a page where you will create a username and password for the database you'll be using to store ticket information from oSTicket. To keep things simple I chose "root" for the username and "Password1" for the password. 
*pic of creating username and pw*
Now it's time to download oSTicket which is "osTicket-v1.15.8.zip" in the installation files folder. Once it's downloaded, extract the contents and copy the "upload" folder into C:\ inetpub\wwwroot. Then rename the copied folder "oSTicket". Now open the IIS Manager and restart the server.
*pic of where to start and stop*
On the left-hand side underneath Connections, select the VM > Sites > Default Web Site > oSTicket. On the right-hand side click "Browse *:80".
*pic of this on the right side*
Now we will enable extensions in IIS. While in IIS > Sites > Default Web Site > oSTicket, double-click PHP Manager. At the bottom click "Enable or Disable an Extension". Right-click and enable the extensions:
+ php_imap.dll
+ php_intl.dll
+ php_opcache.dll
*pic of extensions enabled*
Refresh the oSTicket site in your web browser and you should now see a green check next to Intl Extension. Now navigate to C:\inetpub\wwwroot\oSTicket\include\ost-sampleconfig.php and rename the file to C:\inetpub\wwwroot\oSTicket\include\ost-config.php.
*pic of changing name*
Next, assign permissions to ost-config.php by right-clicking the file and open Properties > Security > Advanced > Permissions. Click Disable inheritance > Remove all inherited permissions from this object.
*pic of removing inheritance*
While still in the Permissions settings of ost-config.php, click Add > Principal and type in "Everyone". Check the names then click OK. Allow everyone full control by ensuring all boxes are checked. Apply the changes.
*pic*
Now we will continue setting up osTicket from within the web browser using the following parameters.
*pic of correct parameters (name: helpdesk email: helpdesk@osticket.com first: mary last: jane email: admin@osticket.com username: user_admin password: Password1*
Now let's download and install HeidiSQL (HeidiSQL_12.3.0.6589_Setup.exe). HeidiSQL is an open-source admin tool for managing databases. We'll be using it to manage MORE. User: root Password: Password1. Select Open and on the left side right-click Unnamed > Create New > Database. Name it osTicket and click OK.
*pic of renaming it*
Let's finish setting up osTicket. In the web browser finish filling out the fields:
*pic of correct parameters (MYSQL Database: osTicket (that you just created in heidi) MYSQL username: root MySQL pw: Password1)
Finish by clicking "Install Now".
Congrats! osTicket has been successfully installed.
Don't forget to complete some post-installation cleanup. First, delete C:\inetpub\wwwroot\osTicket\setup. Next, navigate to C:\inetpub\wwwroot\osTicket\include. Right-click on ost-config.php and select Security > Advanced > "everyone" > edit to change permissions. Allow everyone to only have read and execute permissions.
*pic of read and execute permissions*
For information on post-installation configuration, see pt. 2 [here](
