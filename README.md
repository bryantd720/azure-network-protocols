<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
This tutorial observes various network traffic to and from Azure Virtual Machines with WireShark, as well as experimenting with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Various Command-Line Tools (PowerShell)
- Various Network Protocols (ICMP, SSH, DHCP, DNS, RDP)
- WireShark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Testing Steps</h2>

**Part 1 (Create Resources)**
- Within Azure portal, create a Resource Group
- Create a Windows 10 Virtual Machine (VM)
- While creating the VM, select the previously created Resource Group
- While creating the VM, allow it to create a new Virtual Network (Vnet) and Subnet
- Create a Linux Virtual Machine (Ubuntu)
- While creating the VM, select the previously created Resource Group and Vnet
- Observe Your Virtual Network within Network Watcher (Check the Topology)

**Part 2 (Download and Install WireShark on Windows 10 VM)**
- Use a Remote Desktop Client to connect to your Windows 10 VM (via its public IP address)
- Within your Windows 10 VM, download and install WireShark

**Part 3 (Observe Traffic)**

ICMP Traffic
- Open WireShark and filter for ICMP traffic only
- Retrieve the private IP address of the Ubuntu VM from Azure portal and attempt to ping it from within the Windows 10 VM (use command line)
- Observe ping requests and replies within WireShark
- From The Windows 10 VM, open command line or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in WireShark
- Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM (Ex: ping 8.8.8.8 -t)
- Open the Network Security Group your Ubuntu VM is using (via Azure portal) and disable incoming (inbound) ICMP traffic
- Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line ping activity
- Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using
- Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line ping activity (it should be working again)
- Stop the ping activity (Ctrl + C)

SSH Traffic
- Back in WireShark, filter for SSH traffic only (try using 'tcp.port == 22')
- From your Windows 10 VM, "SSH" into your Ubuntu VM (via its private IP address)
- Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark
- Exit the SSH connection by typing 'exit' and pressing [Enter]

DHCP Traffic
- Back in WireShark, filter for DHCP traffic only (try using 'udp.port == 67' or 'udp.port == 68')
- From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)
- Observe the DHCP traffic appearing in WireShark

DNS Traffic
- Back in WireShark, filter for DNS traffic only (try using 'tcp.port == 53' or 'udp.port == 53')
- From your Windows 10 VM, within a command line, use nslookup to see what google.com and yahoo.com's IP addresses are
- Observe the DNS traffic being shown in WireShark

RDP Traffic
- Back in WireShark, filter for RDP traffic only (try using 'tcp.port == 3389')
- Can you observe the immediate non-stop spam of traffic? Why do you think it is constantly spamming versus only showing traffic when you do an activity?
Answer: The RDP (protocol) is constantly showing you a live stream from one computer to another, therefore traffic is always being transmitted


<h2>Example Screenshots</h2>


<p>
<img src="https://i.imgur.com/rwRiCIC.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Azure creating the first (Windows 10) of two VMs needed. The second will use a Linux (Ubuntu) VM.   
</p>


<p>
<img src="https://i.imgur.com/EnEaXhD.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Checking the Topology in Network Watcher. Both VMs have already been created and use the same Resource Group and Vnet. This will allow VM1 to "ping" VM2 within the Remote Desktop Client. VM2 can also communicate with VM1, which will generate network traffic.
</p>


<p>
<img src="https://i.imgur.com/6XJ5ABX.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Within the Windows 10 VM, downloading and installing WireShark software. After a quick Google search of "download wireshark", choose the first option available.
</p>


<p>
<img src="https://i.imgur.com/Eok69NH.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Testing ICMP (Internet Control Message Protocol) traffic. VM1 is "pinging" VM2's private IP Address. The traffic is then displayed in WireShark. Also, using the project guidelines, we can stop or start ICMP traffic from the Azure portal. Those parameters can be configured in the Ubuntu VM's Network Security Groups tab.
</p>


<p>
<img src="https://i.imgur.com/tB8st2S.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Testing SSH (Secure Shell) traffic. Remotely connecting to VM2 from VM1 using SSH. Once connected, any commands entered into VM2 will be recorded and displayed as traffic in WireShark.
</p>
<br />


<p>
<img src="https://i.imgur.com/r7KRdpd.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Testing DHCP (Dynamic Host Configuration Protocol) traffic. Pinging Azure to "forcefully" assign a new IP Address to VM1 using DHCP and ipconfig /renew. That can be done as many times as the user wants.
</p>
<br />


<p>
<img src="https://i.imgur.com/PdzpFxo.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Testing DNS (Domain Name System) traffic. Using nslookup command to request DNS information for www.google.com. Within the command line (or PowerShell) we can invoke two ipconfig subcommands: /display or /flush. Everytime nslookup is used, the information returned will remain in the cache until it is cleared. Using any of the commands mentioned will generate traffic that can be viewed in WireShark. 
</p>
<br />


<p>
<img src="https://i.imgur.com/HTljCTC.png" height="80%" width="80%" alt="Disk Sanitization Steps" />
</P>
<p>
Testing RDP (Remote Desktop Protocol) traffic. Using WireShark to display the non-stop traffic of VM1 which is accessed remotely. As long as the Remote Desktop client is active and in use, it will constantly generate traffic. All traffic will cease when any active VMs are deleted from the client, or deleted within the Azure portal.
</p>
<br />
