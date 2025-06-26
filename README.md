<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Ubuntu Server 20.04

<h2>Actions and Observations (7 Steps)</h2>

<p>
</p>
<p>
Welcome to my tutorial on Network Security Groups (NSGs) and Inspecting Network Protocols.

1.) First, create two VMs on Azure. (Linux VM and Windows 10 VM)

    - Note: Both VMs will have 2 vcpus.
    - Note: Both VMs must be on the same VNET.
  
  - Once created, connect to the Windows VM and download Wireshark.
    - Link: https://www.wireshark.org/download.html
      
  - Upon installation, open Wireshark and filter for ICMP Traffic only.
    - ICMP is a network layer protocol that relays messages concerning network connection issues.
      - Ping uses this protocol, ping tests connectivity between hosts.
        
    - After filtering Wireshark so only ICMP packets are captured, ping the private IP address of the Linux machine to see the packets on Wireshark.

<p>
</p>
<p>

<img src="https://i.imgur.com/IIUShxp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<br />
  
<p>
</p>
<p>
2.) We can inspect each individual packet and see the actual data that is being sent in each ping. the picture below demonstrates just that. 
  
<p>
</p>
<p>
 
<img src="https://i.imgur.com/GLxSIG3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<br />
  
<p>
</p>
<p>
3.) In the next portion of the lab we will perpetually ping the Linux machine with the command ping -t. This will continually ping the machine until we decide to stop it, while the Windows machine is pinging the Linux machine we will go to the Linux machine and block inbound ICMP traffic on it's firewall. Once we do that we will stop recieving echo replys from the Linux machine. We will block ICMP by creating a new Network Security Group on the Linux machine that will be set to block ICMP. We can allow the traffic by allowing ICMP on the Linux Network Security Groups page on Azure. 
  
<p>
</p>
<p>
 
<img src="https://i.imgur.com/5vXO75R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/Asl80tN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<br />
  
<p>
</p>
<p>
4.) Next we will use our Windows machine to SSH to the Linux machine. SSH has no GUI it just gives the user access to the machines CLI. We will set the wireshark filter to capture SSH packets only. When we ssh into the Linux machine with the command prompt "ssh labuser@10.0.0.5" we can see that wireshark starts to immediately capture SSH packets.
  
<p>
</p>
<p>
 
<img src="https://i.imgur.com/zteR41r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<br />
  
<p>
</p>
<p>
5.) Now we will use wireshark to filter for DHCP. DHCP is the Dynamic Host Configuration Protocol this works on ports 67/68. It is used to assign IP addresses to machines. We will request a new ip address with the command "ipconfig /renew". Once we enter the command wireshark will capture DHCP traffic.
  
<p>
</p>
<p>
 
<img src="https://i.imgur.com/vU8fpQf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<br />
  
<p>
</p>
<p>
6.) Time to filter DNS traffic. We will set wireshark to filter DNS traffic. We will initiate DNS traffic by typing in the command "nslookup www.google.com" this command essentially asks our DNS server what is google's IP address.
  
<p>
</p>
<p>
  
<img src="https://i.imgur.com/VMcwmsO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<br />
  
<p>
</p>
<p>
7.) Lastly we will filter for RDP traffic. When we enter tcp.port==3389 traffic is spammed non stop because we are using Remote Desktop Protocol to connect to our Virtual Machine. 
  
<p>
</p>
<p>
 
<img src="https://i.imgur.com/VxXGv6X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
