<p align="center">
<img src="https://marketplace.radiantlogic.com/wp-content/uploads/bw_store_facet_images/bw_activedirectory_bw_activedirectory-900x0.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>

Welcome back! This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources In Azure
- Ensure Connection between Client and Domain Controller
- Install Active Directory and Admin Creation
- Create X-Amount of Client Users using PowerShell Script

<h2>Setup Resources in Azure</h2>

- Create the Domain Controller VM (Windows Server 2022) named “DC-1”
a) Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
- Set Domain Controller’s NIC Private IP address to be static
- Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
- Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher
