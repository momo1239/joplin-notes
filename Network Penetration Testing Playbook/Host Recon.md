# What is Host Recon
Before we carry out any post-exploitation steps, it's prudent to take stock of the current situation. Every action that we perform carries some risk of detection. The level of that risk depends on our capabilities and the capabilities of any defenders. We can enumerate the host for indicators of how well it's being protected and monitored. This can include Antivirus (AV) or Endpoint Detection & Response (EDR) software, Windows audit policies, PowerShell Logging, Event Forwarding and more.

# How to do Host Recon
Seatbelt is a .NET application written in C# that has various "host safety-checks". The information it gathers includes general OS info, installed antivirus, AppLocker, audit policies, local users and groups, logon sessions, UAC, Windows Firewall and more.

Security configs like Applocker policy, LAPS, OSinfo, powershell logging, etc.

- Gather network information - netstat -ano or net group domain admins /domain
- Process List: tasklist /v or ps -ef
- System Host Info: sysinfo, uname, wmic qfe get Caption, Description, HotFixID, InstalledOn | Get-WmiObject

# Linux System Info
id - current user
w - logged on user
who -a - user information
last -a - user logged on
getent passwd - list of users

# Windows System Info
ver - os version
sc query state=all - show services

Enumerate for running processes, network connections, mapped drives, endpoint agents, credentials.