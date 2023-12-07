# What is External Recon
Two pain portions of recon - organizational and technical.

Organizational recon is focused on collecting information about the organization. This could be the people who work there from names jobs and skills, the org structure, site locations and business relationships.

Technical recon, you're looking for systems like public facing websites, mail servers, remote access solutions, and any vendors or products in use particularly defensive ones like web proxies, email gateways, firewalls, antivirus, etc.

Passive collection relies on third party sources like google, linkedin, shodan - without touching parts of the target network.

Active is touching those components which could be as simple as visiting the target's website, or port scanning their ip ranges. 

Subdomains can also provide insight to other publicly available services, which could include webmail remote access solutions such as Citrix or a VPN. Tools such as dnscan come with lists of popular subdomains.

# How to External Recon

- Port Scanning via nmap for common ports
- Web screenshots via eyewitness or aquatone
- Search Engines like Shodan and Censys to find info about public facing servers, open services, and other details.
- - Manually parsing SSL certificates to find cloud servers. Tools like ssLscrape to strip hostname from certs. 
- Subdomain Enum - to identify ip you can look up company from public sources like American Registry for Internet Numbers. We can look up IP address space to owners in certain regions. You can look up any hostname or FQDN to find the owner of that domain through public sources. But you cannot find subdomains. Subdomain info is stored on target DNS server versus registered on a central public registration system. 

Subdomain can indicate type of server (mail, dev, vpn, internal, test)
Some servers do not respond by IP because they may be on shared infrastructure and only respond by FQDN. This is common on cloud infrastructure.

Subdomain can also provide information about where the target is hosting their servers. This is done by finding all subdomains, performing reverse lookups, and finding where the IPs are hosted. A company could be using multiple cloud providers and data centers.

You can bruteforce subdomains using tools or using OSINT from search engines like in tools such as sublist3r.

## Common Cloud Issues
- Amazon S3 Missing Buckets
- S3 Bucket Permission
- Being able to list and write files to public aws buckets
- lack of logging.

Enumerating S3 Buckets. Tools like Slurp or Bucket Finder usually takes keywords or list and apply multiple permutation and try to identify buckets. 

Using hunter.io or a similar site to find emails and email formats for a company. You could use tools like theharvester to gather email addresses. or SimplyEmail to find email formats. 

Once you have list of emails you can try to do password spraying with it if theres an public facing email portal found during port scanning. Or you could use the emails to do spear phishing. 

You can also use past breaches and check if any emails or username you found have been compromised in any public breaches. 