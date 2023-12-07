# What is Internal Network Recon
Perform internal network scanning to determine level of network segregation, network architecture, and identify vulnerable services.

Collect things like 
- file shares
- network seg
- domain controllers 
- proxy settings

# How to Do Internal Network Recon
Passive Reconnaissance: To understand what is happening in a network, sniffing and scanning is performed. Network sniffing refers to using network interface on a system to monitor or Capture information sent over a wired or wireless connection by placing network interface into promiscuous mode to passively access data in transit over the network. Once you compromised a machine create a listener on the victim machine to perform further activities.

If a sniffer is installed on the machine (like tcpdump or Wireshark’s tshark tool), run it to look for network traffic to identify other possible target machines, as well as cleartext protocols containing sensitive or useful information.

Traceroute - map out the route that data flows through the network in route to a target destination

Pinging and Port Scanning - Port scanning for vulnerable services and checking for alive hosts, hosts with smb signing disabled.

DNS queries

File Shares - SMB, NFS, Samba, SharePoint

Find Remote Sessions and Processes - c:\> netstat -na | find “EST” | find “:445”

c:\> netstat -na | find “EST” | find “:3389”

Routing Table
arp -a # arp cache discover all ipv4 connected device. identify device by nic vendor.