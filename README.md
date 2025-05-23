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

- Windows Server 2022- Windows 10 (21H2)

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

5. Log in to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t [ip address] (perpetual ping)


   ![image](https://github.com/riquewill1977/configure-ad/assets/139101776/3ac06ad9-9ab8-4acf-adb9-ba4622b29d06)


6. Login to the Domain Controller and enable ICMPv4 on the local windows Firewall.

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

    Select the "Members of" tab and click add.

    ![Screenshot 2024-03-14 130448](https://github.com/riquewill1977/configure-ad/assets/139101776/e7a9ed68-c809-4693-975f-6ca47d708bc1)

    enter the object name "domain" in the box and click check names

    ![Screenshot 2024-03-14 130533](https://github.com/riquewill1977/configure-ad/assets/139101776/4bb96ab7-f9a9-4465-a98e-886181d030e2)

    select "Domain Admins" and click ok.

    ![Screenshot 2024-03-14 130557](https://github.com/riquewill1977/configure-ad/assets/139101776/426b4900-b72e-43ac-9523-6a330b0847aa)

15. Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
16. User jane_admin as your admin account from now on

    ![Screenshot 2024-03-14 140323](https://github.com/riquewill1977/configure-ad/assets/139101776/298c717d-d069-4e91-86c6-d0c16b2c121a)

    verify that you are Jane buy opening command prompt and typing "whoami"

    ![Screenshot 2024-03-14 140454](https://github.com/riquewill1977/configure-ad/assets/139101776/efa3e9f2-3e12-42a8-b09f-7dad388e9ee9)

    <h2>Join Client-1 to your domain (mydomain.com)</h2>

    Here's a visual to help illustrate what we'll be doing. Right now, Client-1 virtual NIC's DNS settings is pointing to the vnet's DNS server. 

    ![Azure Virtual Network](https://github.com/riquewill1977/configure-ad/assets/139101776/d6699807-e966-4849-acef-2b44cd5dea16)

    We want Client-1 virtual NIC to point to DC-1's static IP in order to join DC-1's Domain.

    ![Domain Controller Static](https://github.com/riquewill1977/configure-ad/assets/139101776/5948d973-6725-491e-92c0-2877392e7b13)

    From Client-1 command line, If you enter ipconfig /all. You'll see that the DNS server is not DC-1's private IP.

    ![Screenshot 2024-03-14 150039](https://github.com/riquewill1977/configure-ad/assets/139101776/4b5121da-fa1d-4736-a3b6-a22178259a06)


17. From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address

    You do this by clicking on network settings > Network Interface > DNS Servers

    ![Screenshot 2024-03-14 152455](https://github.com/riquewill1977/configure-ad/assets/139101776/5d499f78-dbad-41ad-9396-b7c4a6682bff)

    ![Screenshot 2024-03-14 152505](https://github.com/riquewill1977/configure-ad/assets/139101776/7e15c091-7565-4365-8db7-8cdd0092838b)

    Then click on DNS Servers and change from "Inherit from virtual network" to "Custom" then enter DC-1's Private IP address and click save.

    ![Screenshot 2024-03-14 152546](https://github.com/riquewill1977/configure-ad/assets/139101776/25f35bda-741c-477f-8716-eeb0a1a1de77)

18. From the Azure Portal, restart Client-1

    ![Screenshot 2024-03-14 154737](https://github.com/riquewill1977/configure-ad/assets/139101776/609cf8d6-6cbb-4ef3-af8c-4b1d5c8b5da4)

19. Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)

    Run a quick ipconfig /all and you'll notice the DNS is now pointing to DC-1's Private IP.

    ![Screenshot 2024-03-14 161401](https://github.com/riquewill1977/configure-ad/assets/139101776/7a5e8b37-9e84-4022-955b-feeb186189c1)

    right-click start > System > Rename this PC (advanced) > Change. Then enter mydomain.com

    ![Screenshot 2024-03-14 162023](https://github.com/riquewill1977/configure-ad/assets/139101776/140e478c-b76e-4a81-93a9-e9da0982faa9)

    Enter Jane_admin credentials.

    ![Screenshot 2024-03-14 162145](https://github.com/riquewill1977/configure-ad/assets/139101776/30615591-30be-4980-ba87-e9f66a0ce514)

    You should now see a "Welcome" message. Click ok and you'll get a prompt to restart.

    ![Screenshot 2024-03-14 162203](https://github.com/riquewill1977/configure-ad/assets/139101776/558305dd-c8ab-4ccc-9966-811336d7f79c)

    ![Screenshot 2024-03-14 162219](https://github.com/riquewill1977/configure-ad/assets/139101776/848a8842-78f6-4cd4-83cf-103539f052ad)

20. Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain

    ![Screenshot 2024-03-14 171434](https://github.com/riquewill1977/configure-ad/assets/139101776/20607ee5-b72d-4f47-a9c4-65092daffa14)

    

    <h2>Setup Remote Desktop for non-administrative users on Client-1</h2>

21. Log into Client-1 as mydomain.com\jane_admin and open system properties
Click “Remote Desktop”

    ![Screenshot 2024-03-14 163829](https://github.com/riquewill1977/configure-ad/assets/139101776/45cf95ef-5f7f-4a5c-bf1f-75385091ec69)

    ![Screenshot 2024-03-14 170257](https://github.com/riquewill1977/configure-ad/assets/139101776/94c19043-f6a9-42d7-9247-3cf4646845f9)


22. Click “Remote Desktop”

    ![Screenshot 2024-03-14 170319](https://github.com/riquewill1977/configure-ad/assets/139101776/24915168-578f-417e-a988-574a376e501f)


23. Allow “domain users” access to remote desktop

    ![Screenshot 2024-03-14 170451](https://github.com/riquewill1977/configure-ad/assets/139101776/c60766bc-98c2-4a97-ba7f-e4b4efd3d8f1)

    ![Screenshot 2024-03-14 170510](https://github.com/riquewill1977/configure-ad/assets/139101776/5aab0d11-15b8-405c-9938-6a2715c704df)

    <h2>Create a bunch of additional users and attempt to log into client-1 with one of the users</h2>

24. Log into DC-1 as jane_admin
25. Open PowerShell_ise as an administrator

    ![Screenshot 2024-03-14 182549](https://github.com/riquewill1977/configure-ad/assets/139101776/a0a290aa-d10e-4496-9c03-761ec126dea0)

26. Create a new File and paste the contents of the <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">script</a> into it

    ![Screenshot 2024-03-14 184231](https://github.com/riquewill1977/configure-ad/assets/139101776/445728b2-ba75-4a23-b4d0-ebf49d5de09f)

27. Run the script and observe the accounts being created

    ![Screenshot 2024-03-14 184521](https://github.com/riquewill1977/configure-ad/assets/139101776/9ba03566-e37e-4895-aba9-bc0063bce39f)

28. When finished, open ADUC and observe the accounts in the appropriate OU (_EMPLOYEES)

    ![Screenshot 2024-03-14 185326](https://github.com/riquewill1977/configure-ad/assets/139101776/166b9c8d-348b-4d6d-850a-b10758874eae)

29. Attempt to log into Client-1 with one of the accounts (take note of the password in the script)

    ![Screenshot 2024-03-14 190112](https://github.com/riquewill1977/configure-ad/assets/139101776/66173bda-a90e-4dcb-8c82-8ceb9a774485)

    ![Screenshot 2024-03-14 190129](https://github.com/riquewill1977/configure-ad/assets/139101776/3fdc2702-c764-4cdd-81e5-18e356aa235e)

    Open command prompt to verify

    ![Screenshot 2024-03-14 190414](https://github.com/riquewill1977/configure-ad/assets/139101776/accf053d-e8d8-4254-8b95-15c7fadc2e6e)

    you will also see your user in C:\users

    ![Screenshot 2024-03-14 190456](https://github.com/riquewill1977/configure-ad/assets/139101776/606e9cc8-ecb5-40f8-8865-24a75d71c057)

    This will conclude our lab. Great job!










 
















    
