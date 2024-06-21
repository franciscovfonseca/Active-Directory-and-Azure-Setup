<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Building the Foundation: Preliminary Setup for Active Directory and Network Traffic Analysis between Azure VMs</h1>


This is the first project in our comprehensive series of tutorials on **Azure** and **Active Directory** implementation.<br>
This initial project serves as the foundational setup for the subsequent parts of this tutorial series.

The primary objective is to establish a basic lab environment in **Azure** to simulate the setting where **Active Directory** is typically used within an enterprise environment.

By completing this project, we'll create the essential infrastructure needed to explore **Active Directory** functionalities in an *Azure-Based Network*, preparing us for more advanced concepts in future projects.
</p>
<br>

<h2>Overview </h2>

We will configure and interconnect *2 Virtual Machines*, each assuming distinct roles:

- The first virtual machine will be designated as the **Domain Controller**.<br>
- The second virtual machine will be configured as the **Client**.<br>
<br>
<br>

<h2>Key Objectives</h2>
<h3>Virtual Machine Setup</h3>

1.  Configure the **Domain Controller** virtual machine
2.  Establish the **Client** virtual machine

<h3>Remote Connectivity</h3>

  ⇨ Enable a connection using ***Remote Desktop Connection***.

<h3> Traffic Inspection</h3>

  ⇨ Undertake a basic inspection of the network traffic between the **Domain Controller** and **Client** virtual machines.

<br>



<h2>Environments and Technologies Used</h2>

🔹 Microsoft Azure (Virtual Machines/Compute)<br>
🔹 Remote Desktop<br>
🔹 Active Directory Domain Services<br>
🔹 PowerShell<br>
<br>
<br>

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)
<br>
<br>

<h2>Configuration Steps</h2>

<h3>1️⃣ Create the Domain Controller</h3>

- Create a virtual machine on Azure.
- Name it DC-01 
- Select Windows Server 2022: Azure Edition - x64 Gen2 as the image


<p>
<img width="776" alt="VM image" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/f072cd7b-b547-4006-9ddb-ae6ba39c497e">
</p>
<p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<strong> NOTE: Make sure to select at least 2 vcpus and 16 GiB memory and take note of the vnet that the VM has created.</strong>
</p>
<br />

<p>
<img width="736" alt="DC-vm" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/323e78b9-4e86-46e3-b021-6ac529ccb600">
</p>
<p>
</p>
<br />
<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<h3>&#9313; Set the Domain Controller's Private IP to static </h3>

-  Once the VM has been deployed, proceed to the VM overview page and select "Networking" on the left side. 
<img width="692" alt="networking" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/a35e1aad-57e1-4c1c-9e4e-aefa4fcf31ea">
<br>
<br>
<br>

-  Select Network Interface Card -> IP configurations -> ipconfig1 and set Private IP address allocation to static.

<br>

<p>
<img width="518" alt="static" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/8629a747-9809-4329-859f-2d38896ec484">
</p>

<br />
<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<h3>&#9314; Create the client VM </h3>

- Once again create a new VM and we'll name it Client-01. We'll select Windows 10 as the image and make sure to select at least 2 vcpus and 16 GiB memory.
<img width="717" alt="VM 2 name " src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/a3005b2a-cc0a-49f6-8b9e-c6a594c2aba9">

<br>
<br>
<br>

<p><strong> NOTE: Make sure to select the same resource group and vnet from the DC-01 VM </strong></p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<img width="731" alt="VM2 vnet" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/d591d9b0-ce68-4e74-a466-cdd34886c74b">\

<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

- Now finalize everything and wait for its deployment.

<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<h3>&#9315; Ensure connectivity between Domain Controller and Client  </h3>

<p>To ensure connectivity between the two VM's, we will ping the domain controller from the client.</p>

- First login to the Client-01 using it's public ip address and remote desktop

<img width="993" alt="client 1 public ip" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/c12a5300-fd26-4ae7-b15b-b4fee053bece">


<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<img width="297" alt="remote desktop first login" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/97467245-a6b9-4922-a668-71fdf6f77989">

<br>
<br>
<br>
<br>
<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p><strong> Find DC-01's private ip address in the Azure Portal and copy it. Proceed to Client-01 and open the terminal and type "ping -t (DC-01 private ip address)" </strong></p>


<img width="668" alt="perpetual ping" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/d83a1cbf-2619-4382-bc3c-7025ed246119">


<br>
<br>

<p> <strong> Now notice how the request timed out, this is because ICMP v4 traffic is blocked by default on DC-01's firewall. So we will have to enable inbound ICMP traffic to allow for Client-01's ping.</strong> </p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p><strong> Login to DC-01 using remote desktop and open windows defender firewall and select advanced settings. Sort by protocol and find both ICMP echo requests and enable both these rules by right clicking and selecting enable rule.</strong></p>

<br>

<img width="668" alt="firewall" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/818f7ebe-2ec3-4bd5-b59a-2bc55c24a567">

<br>

<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p><strong> Now once the traffic has been enabled, you can check back with Client-01 and notice that the ping is now successful.</strong> </p>

<img width="334" alt="ping 2" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/ede5ce46-2d9d-49c6-82d7-5afc9796294b">

<h2> Final Thoughts </h2>

<p> We've completed the foundational setup for our Azure and Active Directory project series. By configuring two virtual machines, we've laid the groundwork for implementing the subsequent set of projects. In this project, we focused on establishing a Domain Controller and a Client machine, enabling remote access, and briefly examining network traffic between them. Moving forward, this foundation will help implement more advanced configurations and practical scenarios in Azure and Active Directory. </p>









