![Screenshot 2023-09-22 123340](https://github.com/DJcaston76/azure-network-protocols/assets/145403292/0e1a23d9-b294-44e2-9ace-9626d4491b5f)<p align="center">
<img width="657" alt="68747470733a2f2f692e696d6775722e636f6d2f55613775646f532e706e67" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/33610db6-64f5-47ce-adc3-3f9d4f375200">
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this guide, we'll experiment with different network protocols, a firewall, and traffic between two virtual machines running in Azure. We will also use a powerful protocol analyzer called Wireshark to inspect raw traffic running between the two virtual machines.

<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure Virtual Machines
- Wireshark (Protocol Analyzer)
- Network Protocols: ICMP, DHCP, DNS, RDP
- Network Security Groups in Azure
- Command Line Tools
- Remote Desktop

<h2>Operating Systems Used </h2>

- Windows 10, version 22H2
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>

**VM and Wireshark Setup**

<p>First, we'll make sure to set up our two virtual machines (VMs) properly. "VM1" will run Windows 10, and "VM2" will run an Ubuntu Server. In Azure, we'll make sure to use the same VNET (virtual network). To inspect traffic, we'll download and install Wireshark from https://www.wireshark.org/download.html and set it to capture ethernet traffic.</p>

<img width="1094" alt="Screenshot 2023-09-22 122645" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/b9f7fd0c-c1ff-4d13-bd43-40827ae4ef57">

**_VM1 and VM2 will both run VNET, "VM1-vnet", in Azure._**

<br/>

<img width="731" alt="Screenshot 2023-09-22 122837" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/1885a4ed-44d4-46af-b83c-360db197d139">

**_Wireshark capture is set to Ethernet._**

<br/>

**ICMP Exploration**
- Filter for ICMP
- Notice there's nothing at first
- Ping VM2 with VM1 to its private IP address
- Notice the pings from VM1 (10.0.0.4) to VM2 (10.0.0.5)

<img width="625" alt="Screenshot 2023-09-22 123015" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/863fa1e7-a96f-4773-aa17-e9d3b68e7280">

**_ICMP has no raw traffic, at first._**

<br/>

<img width="633" alt="Screenshot 2023-09-22 123340" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/55f67691-bb02-43ed-b44f-5bce7b018f53">

**_Powershell lists out 4 replies from VM2 after the ping._**

<br/>

<img width="1115" alt="Screen Shot 2023-07-23 at 10 36 07 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/3b74d41f-93e4-4610-913e-9dfa07715de6">

**_Wireshark displays protocol and request/reply data from each IP address._**

<br/>

**ICMP Google Ping Test**
- Ping "www.google.com"
- Notice the requests and replies to and from VM1 and Google
- Observe packet contents inside the Internet Control Message Protocol toggle

<img width="631" alt="Screen Shot 2023-07-23 at 10 37 33 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/21c4e34a-830c-4a43-adca-c879e4aafc19">

**_In Powershell, we'll ping Google._**

<br/>

<img width="1280" alt="Screen Shot 2023-07-23 at 10 38 30 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/c1faccda-0a74-4d7e-8f03-f55b6665d795">

**_VM1's private IP address pings Google's public IP address._**

<br/>

<img width="1266" alt="Screen Shot 2023-07-23 at 10 39 51 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/4943fd72-6b4a-48a1-9416-bc23bf3ffa05">

**_The data packet is a random message being sent._**

<br/>

**ICMP Perpetual Ping Test**
- Send a perpetual ping from VM1 to VM2
- Notice the nonstop traffic between both VMs

<img width="631" alt="Screen Shot 2023-07-23 at 10 41 58 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/06ec3ed5-f5fd-4f2d-a1cd-551540bc866d">

**_Powershell shows continuous replies from VM2._**

<img width="1259" alt="Screen Shot 2023-07-23 at 10 43 09 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/3953a703-bb2c-4ae3-8ae5-635935b1667c">

**_Wireshark shows continuous requests and replies are sent between VM1 and VM2._**

<br/>

**Firewall Exploration in Network Security Groups**
- Navigate to Inbound Security Rules inside VM2's Network Security Group (NSG) 
- Block ICMP traffic with priority level "200" to precede all other security rules
- Observe how ICMP traffic stops
- Re-allow ICMP traffic to come through via the Inbound Security Rules
- Observe how ICMP traffic goes through again

<img width="786" alt="Screen Shot 2023-07-23 at 10 46 53 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/90980707-f62b-4c3c-a862-4024ff4f26ac">

**_Network Security Groups essentially contain firewall settings for VMs in Azure._**

<br/>

<img width="427" alt="Screen Shot 2023-07-23 at 10 49 41 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/9fd8e990-c8d2-47e6-8382-820379f1881f">

**_We'll add an inbound security rule to disallow ICMP at priority level 200._**

<br/>

<img width="1102" alt="Screen Shot 2023-07-23 at 10 55 38 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/3b0fa22f-d62e-4b7e-bf2b-aa7257181954">

**_We'll notice the inbound rule is added then refresh._**

<br/>

<img width="1279" alt="Screen Shot 2023-07-23 at 10 51 22 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/bb3bb05f-a5df-4015-afe5-e2b85912891e">

**_ICMP traffic comes to a stop._**

<br/>

<img width="1195" alt="Screen Shot 2023-07-23 at 10 51 52 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/fab1b5c5-d079-402c-9c71-fcd719b66be7">

**_A closer look inside Wireshark to see the change from the perpetual ping to no ICMP traffic._**

<br/>

<img width="430" alt="Screen Shot 2023-07-23 at 10 55 57 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/055def5f-43e1-44dd-87ca-cb5ca89a37a5">

**_We'll allow ICMP traffic again and refresh._**

<br/>

<img width="634" alt="Screen Shot 2023-07-23 at 10 57 02 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/f4c2c025-3b35-4efd-8e98-730f211a6e66">

**_Powershell shows the replies start back up again._**

<br/>

<img width="1195" alt="Screen Shot 2023-07-23 at 10 57 31 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/b7e7fecf-14b4-4326-aeb7-cb0b1cd37138">

**_A closer look inside Wireshark to see ICMP traffic continue._**

<br/>

<img width="631" alt="Screen Shot 2023-07-23 at 10 58 01 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/276155ec-e06d-49f0-8f49-8014b3f40310">

**_We'll stop the perpetual ping with control or command + c._**

<br/>

**SSH Traffic Exploration**
- Filter traffic for SSH
- Log into VM2 using SSH in Powershell inside VM1
- Observe raw traffic between VM1 and VM2 over SSH
- Filter for "tcp.port==22"
- Notice the same information displayed because SSH traffic occurs on Port 22

<img width="535" alt="Screen Shot 2023-07-23 at 11 03 15 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/167dd4c0-d78d-442b-bbfe-91a3c4f831a0">

**_While inside VM1, we'll use SSH to log into VM2 using the Powershell command line._**

<br/>

<img width="517" alt="Screen Shot 2023-07-23 at 11 04 08 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/7ac9d4f2-e3af-4c1d-b821-03e7711b7bbc">

**_Powershell displays SSH has successfully connected to VM2._**

<br/>


<img width="1247" alt="Screen Shot 2023-07-23 at 11 09 32 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/7adccc54-993c-4cc5-a667-92006c77b9de">

**_Linux commands create raw traffic between VM1 and VM2 over SSH._**

<br/>

<img width="1254" alt="Screen Shot 2023-07-23 at 11 11 47 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/5ca63c39-cf89-4507-9c82-6c5e1240566c">

**_Filtering by "tcp.port==22" displays the same data as filtering by "ssh"._**

<br/>

**DHCP Traffic Exploration**
- Filter traffic for DHCP
- Use command "ipconfig /renew"
- Observe the command request a new IP address lease from DHCP

<img width="1237" alt="Screen Shot 2023-07-23 at 5 15 18 PM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/5bf811e6-92e9-4e28-b1d1-f06d334aa242">

**_We'll filter for DHCP and use command "ipconfig /renew"._**

<br/>

<img width="344" alt="Screen Shot 2023-07-23 at 5 15 22 PM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/b15bd3fe-6a0f-4edc-9e22-946251dd85a8">

**_The request will cause VM1 to reconnect._**

<br/>

<img width="893" alt="Screen Shot 2023-07-23 at 5 15 52 PM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/5e27279a-79cb-4ebc-b9e8-9c1548b673f9">

**_These packets indicate the DHCP IP address renewal process ocurred._**

<br/>

**DNS Traffic Exploration**
- Filter traffic for DNS
- Use nslookup for "www.google.com"
- Notice the requests and responses to Google
- Use nslookup for "www.disney.com"
- Notice the requests and responses to Disney


<img width="604" alt="Screen Shot 2023-07-23 at 11 24 20 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/868b8762-ac13-447b-b53a-dd9eabc56120">

**_We'll use nslookup for Google and Disney's websites._**

<br/>

<img width="1280" alt="Screen Shot 2023-07-23 at 11 22 00 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/d39edcd2-dadc-42c4-9f1b-b4ac636cb25f">

**_The "dns" filter displays raw traffic between VM1 and Google, then between VM1 and Disney._**

<br/>

<img width="1278" alt="Screen Shot 2023-07-23 at 11 24 32 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/7db12dc7-5a55-4bd0-94a3-9afcf0bd83dc">

**_Filtering by "udp.port==53" displays the same data as filtering by "dns"._**

<br/>

**RDP Traffic Exploration**
- Filter traffic for "tcp.port==3389" (RDP's protocol)
- Observe raw traffic is nonstop while actively using VM1

<img width="944" alt="Screen Shot 2023-07-23 at 11 28 02 AM" src="https://github.com/yeahglo/azure-network-protocols/assets/91516100/f4017d82-2e99-4a0b-bc21-9eece31af69b">

**_Remote desktop uses Port 3389 by default._**
