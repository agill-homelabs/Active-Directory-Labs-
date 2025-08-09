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
