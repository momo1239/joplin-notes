# Lateral Movement
Moving laterally between hosts in a domain is important for accessing sensitive informatin and material and obtianing new creds.

# How to do Lateral Movement
- psexec, works by uploading a service binary to target system then creating and starting a windows service to execute that binary.
- WinRM ps remoting. 
- DCOM/WMI/RPC
- RDP

Sysinternals PSEXEC
- 
Allows users to execute commands over SMB using named pipes.
Connects to ADMIN$ share and uploads a psexecsvc.exe. SC manager is used to start the service binary creates named pipes and use said pipe for i/o operations.

Cobalt Strike (CS) goes about this slightly differently. It first creates a PowerShell script that will base64 encode an embedded payload which runs from memory and is compressed into a one-liner, connects to the ADMIN$ or C$ share & runs the PowerShell command, as shown below.

By default, PsExec will spawn the rundll32.exe process to run from. It’s not dropping a DLL to disk or anything, so from a blue-team perspective, if rundll32.exe is running without arguments, it’s VERY suspicious.



Impacket PSEXEC
-
Same as sysinternals but uploads a service binary with an arbitrary name. Might get flagged. 

Metasploit PSEXEC
-
Similar as sysinternals but when sc manager starts the service, it will run a new rundll32 process and allocates memory inside the process and copies shellcode into it.

Creates a randomly-named service executable with an embedded payload
Connects to the hidden ADMIN$share on the remote system via SMB
Drops malicious service executable onto the share
Utilizes the SCM to start a randomly-named service
Service loads the malicious code into memory and executes it
Metasploit payload handler receives payload and establishes session
Module cleans up after itself, stopping the service and deleting the executable

Smbclient
-
Normally used to test connectivity to windows shares. Transfer files, upload files and have backup functionality. You can use with hashes or password to access shares.

WMI cobalt
-
Windows Management Instrumentation (WMI) is built into Windows to allow remote access to Windows components, via the WMI service. Communicating by using Remote Procedure Calls (RPCs) over port 135 for remote access (and an ephemeral port later), it allows system admins to perform automated administrative tasks remotely, e.g. starting a service or executing a command remotely. 
CS leverages WMI to execute a powershell payload on target. 

RDP

Fileless PSEXEC