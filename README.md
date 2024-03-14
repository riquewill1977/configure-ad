<p align="center">
<img src="https://marketplace.radiantlogic.com/wp-content/uploads/bw_store_facet_images/bw_activedirectory_bw_activedirectory-900x0.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>

This tutorial delineates the procedural framework for integrating on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Configuration Guidelines</h2>

- Setup Resources In Azure
- Ensure Connection between Client and Domain Controller
- Install Active Directory and Admin Creation
- Create X-Amount of Client Users using PowerShell Script

<h2>Setup Resources in Azure</h2>

1. Create the Domain Controller VM (Windows Server 2022) named “DC-1” (Take note of the Resource Group and Vnet that get created at this time)

fill out the highlighted sections

- Resource group
- Virtual machine name
- Region
- Image (choose Windows Server 2022)
- Size
- username
- password
- check both boxes for licensing.

![image](https://github.com/riquewill1977/configure-ad/assets/139101776/1e36be9c-c7bd-4bdb-b425-1db5dda5e38b)

![image](https://github.com/riquewill1977/configure-ad/assets/139101776/087e3b02-1ee7-4b62-a687-d6b1681fcc41)




2. Set Domain Controller’s NIC Private IP address to be static

   go to your newly created virtual machine, then Network Settings > Network Interface/ IP Configuration

   ![image](https://github.com/riquewill1977/configure-ad/assets/139101776/9cebbe64-70af-4cfc-9401-80a77bac0acc)

   This will take you to the IP Configurations section. Click on ipconfig1 and change private IP settings from dynamic to static.

   ![image](https://github.com/riquewill1977/configure-ad/assets/139101776/0736645a-8255-4a43-a0c8-f774c0ebce52)


3. Create the Client VM (Windows 10) named “Client-1”. (Use the same Resource Group and Vnet that was created in Step 1)
4. Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher)

   ![image](https://github.com/riquewill1977/configure-ad/assets/139101776/acba3e4d-cf17-410c-b741-0156cf71d1eb)

   ![image](https://github.com/riquewill1977/configure-ad/assets/139101776/7e314b89-2169-4fc5-bae5-af5493afcae2)

   ![image](https://github.com/riquewill1977/configure-ad/assets/139101776/a5eb549b-cae0-4f0c-873c-832f9b398cb1)

<h2>Ensure Connectivity between the client and Domain Controller</h2>

5. Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t [ip address] (perpetual ping)


   ![image](https://github.com/riquewill1977/configure-ad/assets/139101776/3ac06ad9-9ab8-4acf-adb9-ba4622b29d06)


6. Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall.

   Type wf.msc in the task bar for DC-1. This will bring up Windows Defender Firewall. Select Inbound Rules, then sort by protocol and look for ICMPv4. Enable both "Core Networking Diagnostics.

   ![Screenshot 2024-03-13 184104](https://github.com/riquewill1977/configure-ad/assets/139101776/e180260e-5a74-48dd-be44-34672443d594)

7. Check back at Client-1 to see the ping succeed. Now we're sure there's connectivity between Client-1 and DC-1.

   ![Screenshot 2024-03-13 184913](https://github.com/riquewill1977/configure-ad/assets/139101776/7ba390ee-27fd-4afc-beec-c6aa109048cb)

   <h2>Install Active Directory</h2>

8.  Login to DC-1 and install Active Directory Domain Services

   From the Server Manager Dashboard, click "Add roles and features"

   ![Screenshot 2024-03-13 204629](https://github.com/riquewill1977/configure-ad/assets/139101776/74ba6ad3-4f47-41e2-86ea-f56869dbcf98)

   Select Active Directory Domain Services.  

   ![Screenshot 2024-03-13 204749](https://github.com/riquewill1977/configure-ad/assets/139101776/0492c58b-3bb3-45b9-b9c9-4622d4b7b855)

   Then Click Next on all prompts and close.

   ![Screenshot 2024-03-13 204901](https://github.com/riquewill1977/configure-ad/assets/139101776/c9334b5a-ca19-4e41-ba3a-03f1613f3264)

   Click on the Yellow notification at the top right and click on "Promote this server to a domain controller"

   ![Screenshot 2024-03-13 205346](https://github.com/riquewill1977/configure-ad/assets/139101776/82a30faf-109c-4eff-851c-94b33fc0e9d1)

9. Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is). Enter a password and "Next" through all prompts.

    ![Screenshot 2024-03-13 205436](https://github.com/riquewill1977/configure-ad/assets/139101776/9b2ccd17-f53c-4927-bcea-6a577dcda29a)

10. Restart and then log back into DC-1 as user: mydomain.com\labuser

    ![Screenshot 2024-03-13 211117](https://github.com/riquewill1977/configure-ad/assets/139101776/b87ec160-afec-4cec-9776-38a45a604169)
   
   <h2>Create an Admin and Normal User Account in AD</h2>

11. In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
12. Create a new OU named “_ADMINS”
13. Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”

    ![Screenshot 2024-03-13 211319](https://github.com/riquewill1977/configure-ad/assets/139101776/36235226-1b10-4d3e-8e13-de5585f8be34)

    ![Screenshot 2024-03-13 212529](https://github.com/riquewill1977/configure-ad/assets/139101776/7cde1c9e-4bda-43fb-b22f-2c19940d6878)

    ![Screenshot 2024-03-13 212739](https://github.com/riquewill1977/configure-ad/assets/139101776/af5f5e69-80c7-4bf1-b4e4-d017cfd2f49d)

    ![Screenshot 2024-03-13 221544](https://github.com/riquewill1977/configure-ad/assets/139101776/ed6bc82d-04ea-4fe2-bd9a-b99e81b87f5d)

    ![Screenshot 2024-03-13 221655](https://github.com/riquewill1977/configure-ad/assets/139101776/2a7ea520-2c85-45f3-a5ea-1bff03a7445e)

14. Add jane_admin to the “Domain Admins” Security Group

    ![Screenshot 2024-03-14 130339](https://github.com/riquewill1977/configure-ad/assets/139101776/aa6c733b-43e1-4eb2-9bf1-701f267815e2)

    ![Screenshot 2024-03-14 130448](https://github.com/riquewill1977/configure-ad/assets/139101776/ac0bc79c-e129-47d9-9ccf-a443da9a4f3d)

    ![Screenshot 2024-03-14 130533](https://github.com/riquewill1977/configure-ad/assets/139101776/64538fc8-a574-43ec-8904-81d87fd6fea3)

    ![Screenshot 2024-03-14 130557](https://github.com/riquewill1977/configure-ad/assets/139101776/1fde84c2-054f-4a6d-a966-810717c089d1)

    








