<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Building the Foundation: Preliminary Setup for Active Directory and Network Traffic Analysis between Azure VMs</h1>
<br>


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

<p align="center">
<img src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/0122675a-ebfe-4f14-bf1e-669444f94bdd" height="50%" width="50%" alt="9"/><br />
</p>
<br />
<br>

<h2>Key Objectives</h2>

<h3>🟢 Virtual Machine Setup</h3>

1.  Configure the **Domain Controller** virtual machine
2.  Establish the **Client** virtual machine

<h3>🟢 Remote Connectivity</h3>

- Enable a connection using ***Remote Desktop Connection***.

<h3>🟢 Traffic Inspection</h3>

- Undertake a basic inspection of the network traffic between the **Domain Controller** and **Client** virtual machines.

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
<br>

First, using Azure, create a **Resource Group**.
  
Then Create a **Virtual Machine** in Azure and name it ***DC-01***.


<br>

<img src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/b745bca0-4be2-4832-82f5-6b4da96c5ae2" height="70%" width="70%" alt="9"/><br />
<br />


Now for the Image select ***Windows Server 2022: Azure Edition - x64 Gen2***.

Make sure to select at least ***2 vcpus and 16 GiB memory***.

<img src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/cfc532e5-4d1f-4cb7-8e92-1f3d723df005" height="70%" width="70%" alt="9"/><br />
<br />

Give the admin log in credentials that can be remembered or just write them down in notepad.

<img src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/e1b7f8a2-220c-4ae0-8363-20101afcbd04" height="55%" width="55%" alt="9"/><br />
<br />


Then, click *Next* until reaching the **Networking** tab.<br>
<br>

⚠️ Take note of the Virtual Network created: This will be important when creating the Client VM.<br>

<img src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/f3714e89-9637-42f7-a1b0-70e40601771f" height="70%" width="70%" alt="9"/><br />
<br>

Check the box under *Licensing* then ***Review and Create*** the VM.

<br>

<h2></h2>

<h3>2️⃣ Set the Domain Controller's Private IP to Static </h3>
<br>

Once the VM has been deployed, it's time to set the Domain Controller's NIC Private IP to **Static**

Go to the Domain Controller and click on the **Networking** tab.

After that, click on the *Network Interface*.

<img src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/c0ede8b5-dfc4-4e34-b35e-7431d1005fe5" height="60%" width="60%" alt="9"/><br />
<br />

Now, go the **IP configurations** tab and click on the IP configuration. 

<img src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/b30d15f5-1a2e-49ee-a045-83947fa74219" height="60%" width="60%" alt="9"/><br />
<br />

Now, change the *Allocation* from **Dynamic** to **Static**.

Then click ***Save***.

<img src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/1b381264-d1df-45ce-9abb-16dcb6afaea9" height="60%" width="60%" alt="9"/><br />
<br>
<br>

<h2></h2>

<h3>3️⃣ Create the Client VM </h3>
<br>

Once again we're creating a new VM and we'll name it ***Client-01***.

Same thing as the first one, except the image should be using **Windows 10**, and make sure to select at least ***2 vcpus and 16 GiB memory***.

<img src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/fc47df4c-2b66-49d2-9dde-17e945cb6bbd" height="70%" width="70%" alt="9"/><br />
<br />

Click *Next* until reaching the **Networking tab**.<br>

⚠️ Make sure the **Resource Group** and the **Virtual Network** are the same as the one for the *Domain Controller*.<br>
<br>

Finally ***Review and Create***.
<br>
<br>
<br>


<h2></h2>

<h3>4️⃣ Ensure Connectivity between Domain Controller and Client  </h3>
<br>

To ensure Connectivity between the two VM's 🡪 we will *Ping* the **Domain Controller** (DC-01) from the **Client** (Client-01).

First login to the *Client-01* using its ***Public IP Address*** through ***Remote Desktop Connection***.
<br>

<img width="993" alt="client 1 public ip" src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/2b2ce75c-213d-4d17-9901-67fe51dfc4cf">


<br>
<br>


<img width="297" alt="remote desktop first login" src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/198600dc-7f29-4eee-a674-0a410c1f9c20">

<br>
<br>
<br>


Find the **Domain Controller's Private IP Address** in the *Azure Portal* and copy it.

Then, using **Command Prompt**, ping the Domain Controller with its **Private IP Address**.

Type in "*ping -t (**DC-01 Private IP Address**)*" to perpetually ping.<br>


```commandline
ping -t 10.0.0.4
```
<br>

For now it will time out.

<br>

<img width="668" alt="perpetual ping" src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/973dd24d-92da-4058-bc06-69e427600a18">


<br>
<br>
<br>


The request timed out because **ICMPv4** traffic is blocked by default on DC-01's Firewall.

So we will have to **Enable Inbound ICMP Traffic** to allow for Client-01's Ping.

Login to *DC-01* using **Remote Desktop** and open ***Windows Defender Firewall with Advanced Security***.

<img src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/02c1ac48-101e-43e2-bb66-eef4f3ded35d" height="85%" width="85%" alt="9"/><br />
<br>

Click on **Inbound Rules** and Sort by **Protocol**.

Look for the rules with ***Core Networking Diagnostics - ICMP Echo Request(ICMPv4-In)***.<br>

There will be two of them *(Both at the bottom of the image below)*

<img src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/d71441e0-9cf6-4213-b65d-a6ef67262256" height="80%" width="80%" alt="9"/><br />
<br>

Right-click and **Enable** both rules.<br>

<img src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/75ec444d-a6d5-4d68-9f6c-ddac469279ed" height="55%" width="55%" alt="9"/><br />
<br>

Now go back to **Client-01** VM and check on the *Command Prompt*.

✅ Once the traffic has been enabled, it should be properly **Pinging the Domain Controller**.

<br>


<img width="334" alt="ping 2" src="https://github.com/franciscovfonseca/Active-Directory-and-Azure-Setup/assets/172988970/550ea46e-c7cb-4fa7-85eb-5db7b62a4104">
<br>
<br>
<br>

<h2> Final Thoughts </h2>

We've completed the foundational setup for our **Azure and Active Directory** project series.

By configuring two Virtual Machines, we've laid the groundwork for implementing the subsequent set of projects.

In this project, we focused on establishing a ***Domain Controller*** and a ***Client Machine***, enabling **Remote Access**, and briefly examining **Network Traffic** between them.

Moving forward, this foundation will help implement more advanced configurations and practical scenarios in **Azure and Active Directory**.

<br>
<br>
<br>
<br>









