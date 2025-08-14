# Active-Directory-Labs-
This repository demonstrates practical Active Directory skills via labs focused on user/group management, domain configuration, Group Policy, and troubleshooting authentication and access. Each scenario reflects real-world tasks, emphasizing tech proficiency, customer service, and enterprise security protocols.
# Description:
I will be creating a Active Directory Homelab using Oracle VirtualBox as our hypervisor to host our Windows Server. The Windows Server is needed so that we can run Active Directory in this environment. This is accomplished by installing the Windows Server ISO directly to the VirtualBox hypervisor. From there, we have to configure our IP addresses for Domain Controller usage. This then will allow me to create users within Active Directory (setting up credentials, passwords, priviledges, etc). From there, I will demonstrate how to create a DHSP server (integral part in IP management, lease duration, and network effieciency). This will successful mimic real life applications where multiple users need to be managed within Active Directory and their profiles managed to online access, as well as, file access and management. 
# Language & Applications Used: 
Active Directory 

Powershell 

CMD (command-line) 
# Environments Used: 
Oracle VirtualBox 

Microsoft Server 2019

Windows 10 ISO 
# Links 
VirtualBox https://www.virtualbox.org/wiki/Downloads

Windows Server 2019 https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019

Windows 10 (Media Creation Tool) https://www.microsoft.com/en-us/software-download/windows10
#  Process 
(I will be skipping the initial virtualbox install and set up as we will be focusing on setting up the Domain Controller (referred to as DC). This will require us to set up two NICs: 1) Will be fore communicating with the Internet and house our DHCP server. 2) Will be fore communicating internally (be assigned an IP address through the DHCP server of NIC (1). 

We can easily find which NIC on our VM will be used for which by checking our network connections by opening run prompt (WIN + R) and entering ncpa.cpl. This opens our Network Connections. From here, he will be right clicking each connectiona and checking both the status and IPv4 address (see image below) 
<img width="1016" height="876" alt="image" src="https://github.com/user-attachments/assets/44afcad4-05a8-49dd-a31b-5e1e99294d41" />
<img width="1022" height="875" alt="image" src="https://github.com/user-attachments/assets/c18715b2-9f5b-424b-a39c-6a7412d85a04" />

Notice that Ethernet 2 has an APIPA address for its Ipv4 (169.254.x.x). This means that it cannot reach the DHCP server. This makes it easier for us to determine which of these to use for our internal communication with the VM. To make it easier to defferenciate, I will rename them "_INTERNET_" for our DHCP server/connection to the internet and "X_Internet_X" for our internal VM connection.  

Next, we will be configuring the Internal network adapter by assigning it a static IP address, subnet mask, and DNS server address. I can skip the default gateway input due to the Domain Controller being the gateway. In addition the DNS server will be assigned a loop back address, seeing as Active Directory will take care of the DNS allocation. 
<img width="1026" height="871" alt="image" src="https://github.com/user-attachments/assets/23025a19-f727-49b8-aad2-6360d1378c7d" />

Now that I know which NIC is internal and external, I am going to rename the PC for cohesion and ease. This requires a restart. <img width="1013" height="875" alt="image" src="https://github.com/user-attachments/assets/91ca5347-1786-4849-b34f-72fc3a212a90" />

From here we will be installing Active Directory on our newly named PC. This is done by using the roles and features wizard within Server Manager.
<img width="1012" height="877" alt="image" src="https://github.com/user-attachments/assets/2965f1cf-9f1f-43bf-9a30-65d976405841" />
 <img width="1020" height="874" alt="image" src="https://github.com/user-attachments/assets/9f1ad0bb-dc5b-40c9-8ba8-a5eda494bf55" />

Once the install is complete, I will promote the server toa domain controller using the drop down menu in Server Manager
<img width="1014" height="882" alt="image" src="https://github.com/user-attachments/assets/9e033f40-c0ae-42e5-a1bc-93de145ffedb" /> 

This will bring up another configuration wizard. From here, I will follow the wizard and name my domain, add a password and install. 
<img width="1020" height="876" alt="image" src="https://github.com/user-attachments/assets/dccc87ae-bb44-4d01-b3f4-269045987c76" />

<img width="1020" height="876" alt="image" src="https://github.com/user-attachments/assets/6dfb8583-f51d-4273-a5c4-e22d0b02a6b8" />

After a successful install, the VM will restart. From here, I will go to Start > Administrative Tools > Active Directory Users and Computers and right click my new domain (mydomain.com) and then go to new > organizational unit.<img width="1017" height="872" alt="image" src="https://github.com/user-attachments/assets/adde4d19-0a3b-49e6-b7b6-56f9288a04d7" />

From here, I will name it _ADMINS and make sure the "Protect container from accidental deletion" is checked. <img width="1018" height="880" alt="image" src="https://github.com/user-attachments/assets/6194dc60-efab-4955-8be6-99b0e9abe015" />

From here, I will creating a user within Active Directory. This is done by right clicking _Admins and click new > user and fill out the information with my name and then the username. After clicking next, I will be prompted to make and confirm my password for this account. I will make sure that this password never expires (which isn't preferred in the real world, but for ease in this lab- I have) 
<img width="1025" height="877" alt="image" src="https://github.com/user-attachments/assets/d62eff4e-a765-43d6-b666-59a09117b26f" />
<img width="1019" height="862" alt="image" src="https://github.com/user-attachments/assets/4df268ac-2946-4260-ab6b-4a0c72646b3c" />

Now I will make this user a member of the Domain Admins group. This done by right-clicking the user name, going down to Properties, and clicking the "member of" tab. From here, I will click the add button and type in "Domain Admins" and press check names. Then press ok and apply.  
<img width="1014" height="877" alt="image" src="https://github.com/user-attachments/assets/af9ef8cb-498f-4d3d-96c7-d12e24d2348b" />
<img width="419" height="546" alt="image" src="https://github.com/user-attachments/assets/132ed048-308b-4d86-af2e-3dee1f34cc7b" />

Now, we want to make it so that the client can access the internet through the domain controller and virtual network; 
and this is done by using Remote Access Server (RAS) / Network Access Translation (NAT). This must be added to the Domain Controller. In Server Manager, I will go to Add roles and features and then we’re going to check Remote Access and then click next and next again to check the Routing box and add it, then next through until you click install. After the install, I clicked on tools and scrolled down to Routing and Remote Access. 
<img width="622" height="447" alt="image" src="https://github.com/user-attachments/assets/39a8f286-d6f5-4ab0-9ab8-5548da5d424e" />

From here, I right clicked DC (local) > Configure and Enable Routing and Remote Access and then selected NAT.<img width="623" height="446" alt="image" src="https://github.com/user-attachments/assets/eb2bf16d-2d2a-43af-a20f-8363e973c2c3" />

This is where the previously renamed NICs will come in handy, I have to select which will be the public interface to connect to the Internet. I will select the appropriately named NIC, _INTERNET_ and click finish. <img width="624" height="447" alt="image" src="https://github.com/user-attachments/assets/d9c1b5c1-24f8-4232-bb99-04207978f254" />

I now have to configure the DNS Server to run on my Domain Controller. I will go back to Add Roles and Features in the Server Manager. However, this time I will be choosing the DHCP Server option and continue to follow the wizard until the install is complete. <img width="777" height="560" alt="image" src="https://github.com/user-attachments/assets/91aed694-9c3f-4d3c-ba4e-982bb926b084" /> 
<img width="785" height="561" alt="image" src="https://github.com/user-attachments/assets/b21969e0-fcab-47f2-946f-69287dc4b1a9" />

After the install, I will navigate to Tools > DHCP. On the left panel, I'll click the domain and right click IPv4 > New Scope. The DHCP server will be assigning IP addresses for any computer on the network. I will be setting up a range of 172.16.0.100–200. This will allow the DHCP server to assign 100 IP addresses on the network. I can also adjust the amount of time for the lease as well. This allows us to control the length of time a device will be allowed to use the IP address, essentially freeing up IPs for future use with new devices. Following the wizard, I will configure the IP address range, lease duration, router (default gateway), and finally the activation of the scope. 
<img width="512" height="427" alt="image" src="https://github.com/user-attachments/assets/a617cd48-28d4-4458-92ee-da5c01040466" />
<img width="510" height="432" alt="image" src="https://github.com/user-attachments/assets/b322dbec-d503-4e7f-b771-f85a387c2197" />
<img width="509" height="420" alt="image" src="https://github.com/user-attachments/assets/cdaf9185-8f19-49eb-bae1-30956c9243ce" />

After the wizard is complete, I will authorize domain under the DHCP server and then refresh. This will activate our IPv4 and IPv6 under the DHCP Server. 

<img width="584" height="399" alt="image" src="https://github.com/user-attachments/assets/0845eda0-b9df-457d-8d60-1472f174cf3f" />

Now that the DHCP Server is set up, I will be using PowerScript to add users to Active Directory. To be able to accomplish this, I'll be disabling the security protections so that I can freely access the internet and download a the PowerShell script. Its worth noting that in the real world, this would pose as a security risk and would frowned upon. But since this is a lab environment with minimal risks, it is not an issue. To do this I will go back to Server Manager on the DC, select Configure this Local Server and turn off IE Enhanced Security Configuration. 
<img width="1014" height="701" alt="image" src="https://github.com/user-attachments/assets/7fa980aa-dbcc-4278-86a0-8e95fbc8a3e2" />

Now that its turned off, I obtained the zip containing the PowerShell scripts and a few other items (full code and txt file found in my repository). To start, I want to open the names.txt file and put my name at the top of the list; and save. <img width="1008" height="734" alt="image" src="https://github.com/user-attachments/assets/af78a799-c37c-4344-af77-44e180d8ef45" />

Next, from the start menu, I will select  Windows Powershell ISE and then open the script named 1_CREATE_USERS inside of your PowerShell ISE instance. In the bottom half, I will input the following: Set-ExecutionPolicy unrestrcited and click enter. Click Yes to All when prompted. <img width="1007" height="738" alt="image" src="https://github.com/user-attachments/assets/73e1c95b-6b88-458b-9ada-7fc596ba9bff" />

From here, I will direct PowerShell to the directory which has my name.txt file. Do this by entering cd C:\Users\["your login]\Desktop\AD_PS-master. By pressing the green run button, I am prompted on how many times to run the script- I chose run once. After the completion of the script, I can double check that the users were created in Server Manager > Active Directory Users and Computers. <img width="757" height="534" alt="image" src="https://github.com/user-attachments/assets/a82c6b94-1b75-4a54-a06d-c6356b8ef4e4" />

I will now create a new virtual machine that will act as a user in the domain (aka an employee computer). I will name this CLIENT1. After creating the machine, I will adjust the settings in General > Advanced > Drag'n'Drop > Bidirectional. Additionally, I will change the settings under Network > Adapter 1 > and set the Attached to: box to NAT.  Then, Network > Adapter 2 > Select Enable Network Adapter, change the Attached to: to Internal Network and leave the Name to intnet, click Okay and double click your machine to boot it up. 
<img width="1143" height="706" alt="image" src="https://github.com/user-attachments/assets/e7a55c95-acbd-4e65-a547-2d1bf66d5ffd" />
<img width="1146" height="713" alt="image" src="https://github.com/user-attachments/assets/e0497aee-43f8-4048-a17d-ba3ff7352d42" />
<img width="1143" height="712" alt="image" src="https://github.com/user-attachments/assets/739bd133-5802-477d-941d-47331fb2bd88" />

Once everything is set up, I will be renaming the PC (to CLIENT1), as well as, have it join the domain as a user. To do this, we must right-cick Start > System > Rename this PC (advanced).  













