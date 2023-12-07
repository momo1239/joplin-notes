# Domain Dominance
The term "domain dominance" is used to describe a state where an attacker has reached a high level of privilege in a domain (such as Domain or Enterprise Admins) and collected credential material or placed backdoors that 1) allows them to maintain that level of access almost indefinitely; and 2) makes it practically impossible that the domain or forest can ever be considered "clean" beyond reasonable doubt.

# How to do Domain Dominance
DCSync is a technique which replicates the MS-DRSR protocol to replicate AD information, including password hashes. Normally this is only performed by domain controllers. There are specific DACLs relating to dcsyc called replicating directory changes which is only granted to domain admins and domain controllers.

As a domain admin you can modify these and grant this right to other principals including user groups or computers.

A silver ticket is a forged TGS, signed using the secret key of a machine account. You can forge a TGS for any user to any service on that machine, which is good until comp account password changes.

A golden ticket is a forged TGT, signed by the domain's krbtgt account. A silver ticket can be used to impersonate any user it is limited to that signle service or any service on a single machine. A golden ticket can impersonate any user, to any service, to any machine on the domain. 

Like a golden ticket, a diamond ticket is a TGT which can be used to access any service as any user.  A golden ticket is forged completely offline, encrypted with the krbtgt hash of that domain, and then passed into a logon session for use.  Because domain controllers don't track TGTs it (or they) have legitimately issued, they will happily accept TGTs that are encrypted with its own krbtgt hash.

As previously mentioned, there are two common techniques to detect the use of golden tickets:

Look for TGS-REQs that have no corresponding AS-REQ.
Look for TGTs that have silly values, such as Mimikatz's default 10-year lifetime.


A diamond ticket is made by modifying the fields of a legitimate TGT that was issued by a DC.  This is achieved by requesting a TGT, decrypting it with the domain's krbtgt hash, modifying the desired fields of the ticket, then re-encrypting it.  This overcomes the two aforementioned shortcomings of a golden ticket because:

TGS-REQs will have a preceding AS-REQ.
The TGT was issued by a DC which means it will have all the correct details from the domain's Kerberos policy.  Even though these can be accurately forged in a golden ticket, it's more complex and open to mistakes.