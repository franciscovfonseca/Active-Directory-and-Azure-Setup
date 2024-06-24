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
<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/515cdb6d-7aa1-4806-b4a5-77ae9893bedb" height="50%" width="50%" alt="9"/><br />
</p>
<br />
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
<br>

First, through the Azure Portal, create a **Resource Group**.
  
Then create a **Virtual Machine** and name it ***DC-01***.


<br>

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/456a1394-bd3d-4ce8-945a-c1e23e4d897d" height="70%" width="70%" alt="9"/><br />
<br />


Now for the Image select ***Windows Server 2022: Azure Edition - x64 Gen2***.

Make sure to select at least ***2 vcpus and 16 GiB memory***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/8e18ab36-9543-4327-9f55-892fc6f599b1" height="70%" width="70%" alt="9"/><br />
<br />

Give the admin log in credentials that can be remembered or just write them down in notepad.

Then, click *Next* until reaching the **Networking** tab.<br>

⚠️ Take note of the Virtual Network created: This will be important when creating the Client VM.<br>

Check the box under Licensing then ***Review and Create*** the VM.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/a5dbd18a-2b7a-433e-b101-07302f503a49" height="70%" width="70%" alt="9"/><br />
<br>
<br>

<h2></h2>

<h3>2️⃣ Set the Domain Controller's Private IP to Static </h3>
<br>

Once the VM has been deployed, it's time to set the Domain Controller's NIC Private IP to **Static**

Go to the Domain Controller and click on the **Networking** tab.

After that, click on the *Network Interface*.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/e702cc27-b53c-42af-b002-3a7ca8322992" height="60%" width="60%" alt="9"/><br />
<br />

Now, go the **IP configurations** tab and click on the IP configuration. 

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/2d248eb4-13d7-42de-9945-1aa600fdae1d" height="60%" width="60%" alt="9"/><br />
<br />

Now, change the *Allocation* from **Dynamic** to **Static**.

Then click ***Save***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/8d3b0db5-68cc-4626-a552-f62aac7574a7" height="60%" width="60%" alt="9"/><br />
<br>
<br>

<h2></h2>

<h3>3️⃣ Create the Client VM </h3>
<br>

Once again we're creating a new VM and we'll name it ***Client-01***.

Same thing as the first one, except the image should be using **Windows 10**, and make sure to select at least ***2 vcpus and 16 GiB memory***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/d9d106da-540a-487f-8cfa-97bf3955945d" height="70%" width="70%" alt="9"/><br />
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

<img width="993" alt="client 1 public ip" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/c12a5300-fd26-4ae7-b15b-b4fee053bece">


<br>
<br>


<img width="297" alt="remote desktop first login" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/97467245-a6b9-4922-a668-71fdf6f77989">

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

<img width="668" alt="perpetual ping" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/d83a1cbf-2619-4382-bc3c-7025ed246119">


<br>
<br>
<br>


The request timed out because **ICMPv4** traffic is blocked by default on DC-01's Firewall.

So we will have to **Enable Inbound ICMP Traffic** to allow for Client-01's Ping.

Login to *DC-01* using **Remote Desktop** and open ***Windows Defender Firewall with Advanced Security***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/d42ded65-aee3-4e87-bc61-a7b2b3050e50" height="85%" width="85%" alt="9"/><br />
<br>

Click on **Inbound Rules** and Sort by **Protocol**.

Look for the rules with ***Core Networking Diagnostics - ICMP Echo Request(ICMPv4-In)***.<br>

There will be two of them *(Both at the bottom of the image below)*

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/67f09539-fc53-451f-82dc-cc0cd9a4d77d" height="80%" width="80%" alt="9"/><br />
<br>

Right-click and **Enable** both rules.<br>

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/83a47889-6fc4-40ec-aa6d-082ea4620238" height="55%" width="55%" alt="9"/><br />
<br>

Now go back to **Client-01** VM and check on the *Command Prompt*.

✅ Once the traffic has been enabled, it should be properly **Pinging the Domain Controller**.

<br>


<img width="334" alt="ping 2" src="https://github.com/kirkgacias/ad-and-azuresetup/assets/158519921/ede5ce46-2d9d-49c6-82d7-5afc9796294b">
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









