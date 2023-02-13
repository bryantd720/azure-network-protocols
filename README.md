<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
This tutorial observes various network traffic to and from Azure VMs with Wireshark as well as experimenting with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, DHCP, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Testing Steps</h2>

**Part 1(Create Resources)**
- Create a Resource Group
- Create a Windows 10 Virtual Machine (VM)
- While creating the VM, select the previously created Resource Group
- While creating the VM, allow it to create a new Virtual Network (Vnet) and Subnet
- Create a Linux Virtual Machine (Ubuntu)
- While creating the VM, select the previously created Resource Group and Vnet
- Observe Your Virtual Network within Network Watcher via Azure portal

**Part 2(Observe Traffic)**<p><br>ICMP Traffic</br></p>
- Use Remote Desktop to connect to your Windows 10 Virtual Machine
- Within your Windows 10 Virtual Machine, download and install Wireshark
- Open Wireshark and filter for ICMP traffic only
- Retrieve the private IP address of the Ubuntu VM from Azure portal and attempt to ping it from within the Windows 10 VM
- Observe ping requests and replies within WireShark
- From The Windows 10 VM, open command line or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in WireShark
- Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM (Ex: ping ... -t)
- Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic
- Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity
- Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using
- Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity (it should start working)
- Stop the ping activity (Ctrl + C)

SSH Traffic
- Back in Wireshark, filter for SSH traffic only
- From your Windows 10 VM, "SSH into" your Ubuntu VM (via its private IP address)
- Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark
- Exit the SSH connection by typing 'exit' and pressing [Enter]

DHCP Traffic
- Back in Wireshark, filter for DHCP traffic only
- From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)
- Observe the DHCP traffic appearing in WireShark

DNS Traffic
- Back in Wireshark, filter for DNS traffic only (or you can use 'udp.port == 53')
- From your Windows 10 VM, within a command line, use nslookup to see what google.com and yahoo.com's IP addresses are
- Observe the DNS traffic being shown in WireShark

RDP Traffic
- Back in Wireshark, filter for RDP traffic only (or you can use 'tcp.port == 3389')
- Do you observe the immediate non-stop spam of traffic? Why do you think it is non-stop spamming versus only showing traffic when you do an activity?
Answer: Because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefore traffic is always being transmitted


<h2>Example Screenshots</h2>

<p>
<img src="https://i.imgur.com/hV7oWQ9.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Testing ICMP (Internet Control Message Protocol) traffic. VM1 is "pinging" VM2's Private IP Address. The traffic is then displayed in the PowerShell command terminal and in WireShark.
</p>


<p>
<img src="https://i.imgur.com/vazEJaH.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Testing SSH (Secure Shell) traffic. Remotely connecting to VM2 from VM1 using SSH.
</p>
<br />


<p>
<img src="https://i.imgur.com/XG2lkL0.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Testing DHCP (Dynamic Host Configuration Protocol) traffic. Pinging Azure to "forcefully" assign a new ip address to VM1 using DHCP and ipconfig /renew.
</p>
<br />


<p>
<img src="https://i.imgur.com/FemPPs2.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Testing DNS (Domain Name System) traffic. Using nslookup command to request DNS information for www.google.com.
</p>
<br />


<p>
<img src="https://i.imgur.com/ViPMtrN.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Testing RDP (Remote Desktop Protocol) traffic. Using WireShark to display the non-stop traffic of VM1 which is accessed remotely. The more it is used, the more traffic will be logged.
</p>
<br />
