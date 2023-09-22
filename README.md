<p align="center">
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

<img width="1115" alt="Screenshot 2023-09-22 123643" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/5e805a91-09ec-43bf-b92a-c3a5d79adb74">

**_Wireshark displays protocol and request/reply data from each IP address._**

<br/>

**ICMP Google Ping Test**
- Ping "www.google.com"
- Notice the requests and replies to and from VM1 and Google
- Observe packet contents inside the Internet Control Message Protocol toggle

<img width="631" alt="Screenshot 2023-09-22 125031" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/b73dc44a-a3b1-4b39-abfd-e803a7694524">

**_In Powershell, we'll ping Google._**

****

**ICMP Perpetual Ping Test**
- Send a perpetual ping from VM1 to VM2
- Notice the nonstop traffic between both VMs

<img width="631" alt="Screenshot 2023-09-22 125705" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/103eb27d-1d4c-4f6d-9639-86884d3f7119">

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

<img width="786" alt="Screenshot 2023-09-22 124013" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/c8a268e1-a4a4-4374-8a40-44525688e039">

**_Network Security Groups essentially contain firewall settings for VMs in Azure._**

<br/>

<img width="427" alt="Screenshot 2023-09-22 124217" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/bedc0cc3-31b8-4600-8baa-5e461d628736">

**_We'll add an inbound security rule to disallow ICMP at priority level 200._**

<br/>

<img width="1102" alt="Screenshot 2023-09-22 124427" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/a2f675a6-585e-4974-9172-d4392e1af65f">

**_We'll notice the inbound rule is added then refresh._**

<br/>

<img width="1279" alt="Screenshot 2023-09-22 132304" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/ccf3d642-ba4b-4d7e-ab92-2c1890ab7136">

**_ICMP traffic comes to a stop._**

<br/>

<img width="430" alt="Screenshot 2023-09-22 124217" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/34d4be2b-7555-4c9c-bfe0-f8e8160eff99">

**_Instead of denying we can click allow ICMP traffic again and refresh._**

<br/>

<img width="634" alt="Screenshot 2023-09-22 131954" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/ba5b777c-9612-4332-81e3-ee1243d10be6">

**_Powershell shows the replies start back up again._**

<br/>

<img width="631" alt="Screenshot 2023-09-22 131742" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/c0da2430-c5d9-4b16-803d-f716aa735dae">

**_We can stop the perpetual ping with control or command + c._**

<br/>

**SSH Traffic Exploration**
- Filter traffic for SSH
- Log into VM2 using SSH in Powershell inside VM1
- Observe raw traffic between VM1 and VM2 over SSH
- Filter for "tcp.port==22"
- Notice the same information displayed because SSH traffic occurs on Port 22

<img width="535" alt="Screenshot 2023-09-22 131553" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/3a0d8724-432a-4aa7-9061-9b7315af3b89">

**_While inside VM1, we'll use SSH to log into VM2 using the Powershell command line._**

<br/>

<img width="517" alt="Screenshot 2023-09-22 131500" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/f7b8a36c-9784-4804-a0c1-abcb80716801">

**_Powershell displays SSH has successfully connected to VM2._**

<br/>


<img width="1247" alt="Screenshot 2023-09-22 131328" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/761dcf9f-b3fc-47b0-a223-18ba22e05ad6">

**_Linux commands create raw traffic between VM1 and VM2 over SSH._**

<br/>

<img width="1254" alt="Screenshot 2023-09-22 131210" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/1b47d0c3-28c9-4b28-bd96-7d12b10b117f">

**_Filtering by "tcp.port==22" displays the same data as filtering by "ssh"._**

<br/>

**DHCP Traffic Exploration**
- Filter traffic for DHCP
- Use command "ipconfig /renew"
- Observe the command request a new IP address lease from DHCP

<img width="1237" alt="Screenshot 2023-09-22 131055" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/7175f4c3-aad6-4287-8cef-0b8bc93c23f3">

**_We'll filter for DHCP and use command "ipconfig /renew" it will cause VM1 to reconnect._**

****

<br/>

<img width="893" alt="Screenshot 2023-09-22 130818" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/2c3b7af1-81f7-4b51-bf1b-33759d3edb6c">

**_These packets indicate the DHCP IP address renewal process ocurred._**

<br/>

**DNS Traffic Exploration**
- Filter traffic for DNS
- Use nslookup for "www.google.com"
- Notice the requests and responses to Google
- Use nslookup for "www.disney.com"
- Notice the requests and responses to Disney


<img width="604" alt="Screenshot 2023-09-22 130706" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/1c35dcec-10cf-48bc-87b1-23c03b4414b4">

**_We'll use nslookup for Google and Disney's websites._**

<br/>

<img width="1280" alt="Screenshot 2023-09-22 130431" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/09000c39-5d4f-4777-a752-7ad17c86f0c0">

**_The "dns" filter displays raw traffic between VM1 and Google, then between VM1 and Disney._**

<br/>

<img width="1278" alt="Screenshot 2023-09-22 130337" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/cea4b87a-dccf-4c5a-b54d-b86f4e66725d">

**_Filtering by "udp.port==53" displays the same data as filtering by "dns"._**

<br/>

**RDP Traffic Exploration**
- Filter traffic for "tcp.port==3389" (RDP's protocol)
- Observe raw traffic is nonstop while actively using VM1

<img width="944" alt="Screenshot 2023-09-22 130057" src="https://github.com/DJcaston76/azure-network-protocols/assets/145403292/0334bac0-681c-4041-99bb-4b0fa4ab8c7c">

**_Remote desktop uses Port 3389 by default._**
