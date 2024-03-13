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


4. Create the Client VM (Windows 10) named “Client-1”. (Use the same Resource Group and Vnet that was created in Step 1)
5. Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher)
