# Domain Privesc

Kerberoasting - Allows any domain user to request kerberos tickets from TGS that are encrypted with ntlm hash of a domain user account that is used as a service account and crack them off line. 

1. Attacker authenticates to a domain and gets a TGT from dc used for ticket requesting. 
2. Attacker uses their TGT to issue a service ticket request TGS-REQ for a particular SPN of the form sname/host. 
3. If the attacker's TGT is valid, the DC extract information from TGT and puts it into a service ticket. Then the DC looks up which account has the requested spn registered in its  spn field. The service ticket is encrypted with the hash of the account with the requested spn. The ticket is sent back to attacker in a service ticket reply TGS-REP.
4. Attacker extracts the encrypted service ticket from the TGS-REP. The attacker can take offline to crack.

## Kerberos Workflow
When user joins network, a secret key isd generated using the password the user created. Shared between user system and the KDS

When user enters user and password for auth, his system generates a secret key using the password entered.

1. User logs on with user and password
2. Password converts to ntlm hash, a timestamp is encrypted with the hash and sent to kdc as authenticator in auth ticket request AS-REQ.
3. The kdc checks user info and creates TGT
4. the tgt is encrypted signed and delivered to the user. Only krbtgt kerberos service can open and read tgt data.
5. The user presents the tgt to dc when requesting a TGS ticket. The DC opens the tgt and validates the checksum. If DC can open the ticket and the checksum checks out tgt is valid. the data in tgt is copied to create the tgs ticket.
6. The tgs is encrypted using the target service account ntlm password hash and sent to user.
7. User connects to server hosting the service and presents tgs. service opens tgs ticket using it ntlm password hash.

Unconstrained Delegation
Delegation allows a user or service to act on behalf of another user to another service.

If unconstrained delegation is configured on a computer, any time an account connects to that computer their TGT is stored in memory so it can be used for imperosnation later.

If we can compromise a machine with unconstrained delegation, we can extract any TGTs from its memory and use them to impersonate the user against other services in the domain.

You can also try to force a priv user to authenticate to a compromised host that has unconstrained delegation and dump the ticket to load into current session or perform a pass the ticket attack and become that priv user.

Using PrinterBug or PetitPotam. 
You can also use this with AD Certificate Services.

Conditions:
- ADCS is configured to allow ntlm auth
- ntlm auth is not protected by smb signing
- adcs is running cert authority web enrollemnt or enrollment web service.

1. Get foothold in AD with a misconfigured ADCS instance
2. setup ntlm relay listerner on a machine you control so incoming auths are relayed to the misconfigured adcs
3. force the target dc to authenticate (petitpotam or printspooler) to the box running your ntlm relay
4. target dc attempts to auth to your ntlm relay
5. ntlm relay receive the dc machine account auth and relays it to the adcs
6. adcs provides a certificate for the target dc computer account
7. use the target dc computer account certificate to request its kerberos tgt
8. use target dc account tgt to perform dcsync and pull the ntlm hash of krbtgt
9. use krbtgt to create golden ticket to impersonate any domain user including domain admin

## Stealing auth tokens
Run mimikatz or another tool to dump local creds. Use local admin creds to auth to other workstation with admin rights and dump creds on those until you find a domain admin cred. With creds found you can also use crackmapexec to spray creds or hashes on a subnet to see if you have admin access to another workstation and dumping creds there.

## ADCS misconfigured certificate template
ADCS certificate templates are provided by micrtosoft as a starting point for distributing certificates. They are designed to be duplicated and configured for specific needs.

You can use a tool like Certify to find any vulnerable templates. The misconfiguration allows any domain user to request a certificate for any domain user including a domain admin and use it to authenticate to the domain. 

Using the certificate you can request for a TGT. 