<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Resources
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>

<h4>Create Resources</h4>

1. Create Virtual Machines
   
- In Azure create a Resource group under your azure subscription, and provide it with a name and pick a location.

    <img width="50%" height = "50%" alt="Create RG" src="https://github.com/s-evelyn/network-protocols/assets/53543374/cffe2d42-3982-42c6-8e59-94950d83a574">
 
- In Azure create a virtual machine(VM) that uses a Windows 10 imaging and 2 virtual CPUs, make sure to select the resource group that was just created. Then select review and create. 

    <img width="50%" height = "50%" alt="Create VM-1" src="https://github.com/s-evelyn/network-protocols/assets/53543374/09ce52d4-8be7-40b2-905b-f7a7ba6698b5">

- When the VM-1 has been deployed take note of the virtual network (Vnet) that is uses, as it will be needed when creating the second VM.
- Create another VM in Azure that uses Ubuntu Server 20.04, and uses 2 vCPUs. Select the same resource group that was created. For the Authentication type select Password, and then fill in the username and password.

    <img width="471" alt="Create VM-2" src="https://github.com/s-evelyn/network-protocols/assets/53543374/bd1a1407-1b9a-41c5-990b-a00749427f64">

- Navigate to the Networking tab when creating VM-2 and assure that VM-2 is using the same Vnet as VM-1. Then review and create VM-2

    <img width="471" alt="Vnet same" src="https://github.com/s-evelyn/network-protocols/assets/53543374/f5a049a6-ba09-4cb3-8eea-363ff6be28c2">

- Once both VM-1 and 2 are deployed and running navigate to the resource group that was created.
- Observe that in the overview we can see a list of all the different elements created with the deployment of the two VMs. VM-1 and VM-2 both have a disk, network interface, public IP address, a network security group, a virtual machine, and they both share a virtual network.
  
    <img width="952" alt="Observe Resource group and what has been created" src="https://github.com/s-evelyn/network-protocols/assets/53543374/f9bef528-1dd7-4a83-9b60-e42cf8b940a6">

2. Connect to VM-1 and Install Wireshark
   
- Connect to VM-1 through remote desktop

    <img width="301" alt="rmd into VM-1" src="https://github.com/s-evelyn/network-protocols/assets/53543374/67dbbc76-f636-4971-b818-fcc4f3c62fc8">

- Login with you user information.

    <img width="341" alt="login to vm-1" src="https://github.com/s-evelyn/network-protocols/assets/53543374/83552cad-ae61-4bb7-8109-973ac1d7fa41">

- Once logged in open the web browser and search for the wireshark download and click to navigate to the link. Click on Windows x64 Installer and complete the installation process:

    <img width="960" alt="Install wireshark" src="https://github.com/s-evelyn/network-protocols/assets/53543374/f5c0cf86-c668-46c0-aaf1-d5cd122eb610">

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h4>Observe ICMP Traffic </h4>















