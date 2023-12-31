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

    <img width="50%" height = "50%" alt="Create VM-2" src="https://github.com/s-evelyn/network-protocols/assets/53543374/bd1a1407-1b9a-41c5-990b-a00749427f64">

- Navigate to the Networking tab when creating VM-2 and assure that VM-2 is using the same Vnet as VM-1. Then review and create VM-2

    <img width="50%" height = "50%" alt="Vnet same" src="https://github.com/s-evelyn/network-protocols/assets/53543374/f5a049a6-ba09-4cb3-8eea-363ff6be28c2">

- Once both VM-1 and 2 are deployed and running navigate to the resource group that was created.
- Observe that in the overview we can see a list of all the different elements created with the deployment of the two VMs. VM-1 and VM-2 both have a disk, network interface, public IP address, a network security group, a virtual machine, and they both share a virtual network.
  
    <img width="50%" height = "50%" alt="Observe Resource group and what has been created" src="https://github.com/s-evelyn/network-protocols/assets/53543374/f9bef528-1dd7-4a83-9b60-e42cf8b940a6">

2. Connect to VM-1 and Install Wireshark
   
- Connect to VM-1 through remote desktop

    <img width="50%" height = "50%" alt="rmd into VM-1" src="https://github.com/s-evelyn/network-protocols/assets/53543374/67dbbc76-f636-4971-b818-fcc4f3c62fc8">

- Login with you user information.

    <img width="50%" height = "50%" alt="login to vm-1" src="https://github.com/s-evelyn/network-protocols/assets/53543374/83552cad-ae61-4bb7-8109-973ac1d7fa41">

- Once logged in open the web browser and search for the wireshark download and click to navigate to the link. Click on Windows x64 Installer and complete the installation process:

    <img width="50%" height = "50%" alt="Install wireshark" src="https://github.com/s-evelyn/network-protocols/assets/53543374/f5c0cf86-c668-46c0-aaf1-d5cd122eb610">

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h4>Observe ICMP Traffic </h4>

Observe ICMP traffic between hosts on the same network

- Open Wireshark, type in icmp in the search bar on top and click on the blue arrow next to it to filter for icmp traffic  

   <img width="50%" height = "50%" alt="filter icmp" src="https://github.com/s-evelyn/network-protocols/assets/53543374/49934b7f-151b-48cb-a0af-1b7d8f8cd78a">

- Go to the azure portal outside of your VM, and take note of the private IP address of VM-2

   <img width="50%" height = "50%" alt="Retrieve private ip from vm-2" src="https://github.com/s-evelyn/network-protocols/assets/53543374/0a3edce7-fb1d-4e56-92da-cccb35cd84b2">

- Go back to VM-1 and open command prompt. Ping the private IP of VM-2. Our ping request gives us 4 replies from VM-2

   <img width="50%" height = "50%" alt="pring private ip vm2" src="https://github.com/s-evelyn/network-protocols/assets/53543374/ade16638-d9ee-4492-88b5-3f82e786badb">

- Look at the wireshark window and observe the traffic

   <img width="50%" height = "50%" alt="wireshark icmp" src="https://github.com/s-evelyn/network-protocols/assets/53543374/22509a94-328f-40a4-9fd9-ce3c613cee98">

   - Observe that VM-1, which has a private IP address of (10.0.0.4) sends a request to VM-2 private IP address (10.0.0.5), and then VM-2 replies to VM-1. This demonstrates what happens when a ping request is sent. Internet Control Message Protocal (ICMP) takes place, where VM-1 sends a request to VM-2, and VM-2 replies establishing that there is network connectivity between the two hosts.

Observe ICMP traffic between a computer and the internet

- In command prompt ping www.google.com

   <img width="50%" height = "50%" alt="ping google com cmd" src="https://github.com/s-evelyn/network-protocols/assets/53543374/cf05c5b2-453d-48f0-9497-759355cdb712">

- Observe wireshark

     <img width="50%" height = "50%" alt="google icmp wireshark with label" src="https://github.com/s-evelyn/network-protocols/assets/53543374/36956837-04be-4879-87d0-3077105407be">

   - Observe the traffic on wireshark. Notice that the destination IP is a server that hosts the website www.google.com, and that it is a similar request and reply as seen in the previous example.


Observe perpetual ping before and after inbound rule implementation

- In command prompt initiate a perpetual ping to the private IP of VM-2

     <img width="50%" height = "50%" alt="perpetual ping" src="https://github.com/s-evelyn/network-protocols/assets/53543374/b8d510f6-ba05-4324-9974-5d2e85769bf7">

- In azure navigate to Network Security Group -> vm2-nsg -> Inbound security rules, click Add.

  <img width="50%" height = "50%" alt="nsg add denyicmp" src="https://github.com/s-evelyn/network-protocols/assets/53543374/1c2945d1-0668-4123-93c1-d31ebd6895de">

   - For the Source click Any.
   - For the source Port it can stay as an astricks, which means that traffic can come in on any port
   - Service can stay as custom
   - For the Protocal click on ICMP, which will automatically change the destination port ranges to and astericks.
   - Select Deny
   - Keep the Priority that is automatically set
   - Name the inbound rule
   - Click Add

      <img width="50%" height = "50%" alt="Add icmp deny rule" src="https://github.com/s-evelyn/network-protocols/assets/53543374/a46d2363-adc9-4839-bb21-7a9771f4a056">

- Refresh VM-2
- Go back to VM-1 and observe the change on command line

<img width="50%" height = "50%" alt="request time out cmd" src="https://github.com/s-evelyn/network-protocols/assets/53543374/8bfd5591-8067-4605-b116-02edbb8d7dfc">

- Note that the request is timing out.
- Observe Wireshark traffic
  
  <img width="50%" height = "50%" alt="request time out wireshark" src="https://github.com/s-evelyn/network-protocols/assets/53543374/53716a80-d2b0-4891-aac1-b4ab181d4d9d">

     - Note that before there was a request and then a response, once the inbound rule was applied a request is still sent from VM-1 to VM-2 but there is no reply from VM-2. On the request line towards the end it reads "No response found" As the ping is perpetual, we see that this continues. The inbound rule that was created is working.

- Navigate back to the network security group inbound rules for VM-2.
- Delete the DenyICMP rule

   <img width="50%" height = "50%" alt="delete icmp rule" src="https://github.com/s-evelyn/network-protocols/assets/53543374/fbb2ad7c-8d19-476d-a22c-1af5c9a56497">

- Go back to wireshark to observe the changes

     <img width="50%" height = "50%" alt="wireshark after rule icmp" src="https://github.com/s-evelyn/network-protocols/assets/53543374/d3d157f1-7d5a-44a7-9801-09468d1f4c98">

     - Observe that now that the rule has been eliminated, VM-2 is now responding to the ping request.

- Go back to command prompt and click ctrl+C, to stop the perpetual ping. Type cls, to clear previous output.

- Open wireshark and clear the output by clicking on the green shark fin at the top, and continue without saving. 
  
  <img width="50%" height = "50%" alt="clear wireshark" src="https://github.com/s-evelyn/network-protocols/assets/53543374/e327833c-4806-47be-b555-ebf9c2aa0cce">

 _________________________________________________________________________________________________________________________

<h4>Observe SSH Traffic </h4>

- In Wireshark filter by ssh

  <img width="50%" height = "50%" alt="Filter for ssh in wireshark" src="https://github.com/s-evelyn/network-protocols/assets/53543374/b4b84d7d-5c09-4d99-a518-58caafdf4e4b">

- Open Command Prompt and SSH into VM-2 by typing : ssh username@privateIPofVM-2, and then press enter. You should then be prompted to continue connecting. Type yes, and then click enter. Type in your passsword. Note that the area where you enter your password will stay blank, even as you enter the password. Once the password has been entered click enter.
  
<img width="50%" height = "50%" alt="ssh into vm2" src="https://github.com/s-evelyn/network-protocols/assets/53543374/98b9d935-f699-4567-9e3e-68d6e38ddce3">

- Navigate back to wireshark and observe the ssh traffic

  <img width="50%" height = "50%" alt="wireshark ssh into vm2" src="https://github.com/s-evelyn/network-protocols/assets/53543374/fc47b78e-d43e-48b1-a925-148aa9359c4a">

    - Notice that now that we have ssh into VM-2 anything that is typed into the commandline shows up as traffic in wireshark. Also that the source port is always port 22. Try typing a few commands in the command line to observe further.
       -  Type pwd to print the current working directory
       -  Create a directory and call is Documents. Type mkdir Documents
       -  Type ls to observe the directory that was created
           
            <img width="50%" height = "50%" alt="ssh in cmd say commands" src="https://github.com/s-evelyn/network-protocols/assets/53543374/342f1eab-bdba-4689-a0be-e3464a3be771">

   - Observe Wireshark after the commands have been executed

      <img width="50%" height = "50%" alt="wire shark ssh after" src="https://github.com/s-evelyn/network-protocols/assets/53543374/9c49d7d1-8ac7-4b00-b459-bea7598ffecc">

     - Observe that there is traffic back and forth between the two VMs.

- Exit the ssh connection by typing exit.

     <img width="50%" height = "50%" alt="exit out of ssh vm2" src="https://github.com/s-evelyn/network-protocols/assets/53543374/27333732-a62a-4da3-b271-67c3a5da43f5">

 
  --------------------------------------------------------------------------------------------------------------------------

  <h4>Observe DHCP Traffic</h4>

- Clear wireshark
- Filter for DHCP only
- In command line issue a new IP address for VM-1 by typing in ipconfig/renew

  <img width="50%" height = "50%" alt="dhcp cmd" src="https://github.com/s-evelyn/network-protocols/assets/53543374/ed6d5c10-bd64-4093-b5a9-30c07957bee7">

- Observe DHCP traffic in Wireshark

     <img width="50%" height = "50%" alt="dhcp wireshark" src="https://github.com/s-evelyn/network-protocols/assets/53543374/9cc347bd-61b3-4b5a-949f-77a3859ee1d8">

  - Notice that the ports used are port 67 and port 68 as expected.


_______________________________________________________________________________________________________________________________________________________________________________

<h4>Observe DNS Traffic</h4>

- Filter Wireshark for DNS traffic only
- In command line use nslookup for google.com
  
  <img width="50%" height = "50%" alt="dns cmd" src="https://github.com/s-evelyn/network-protocols/assets/53543374/dbee0752-e78f-4410-962d-89f0a4927161">

- Observe DNS traffic in Wireshark

  <img width="50%" height = "50%" alt="dns wireshark" src="https://github.com/s-evelyn/network-protocols/assets/53543374/58704cb6-9bb7-4268-a0a0-e9cab106ae86">

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h4>Observe RDP Traffic</h4>

- Filter by RDP traffic (rdp.port==3389)
- Observe RDP traffic

     <img width="50%" height = "50%" alt="rdp wireshark" src="https://github.com/s-evelyn/network-protocols/assets/53543374/da682bad-9cf9-4a08-9668-00962b314c03">

  - Notice that the traffic is ongoing. This is because as we are active RDP session from the computer and the VM that was created there is information constantly passing between the two. Any movement that is made intiates traffic between the VM and the local computer. If you notice to 10.0.0.4 is the IP address of the VM and the other IP address that traffic is being passed to should be the IP address of the physical computer being used.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Congratulations you have successfully observed network traffic on a VM!**
  

  

       
