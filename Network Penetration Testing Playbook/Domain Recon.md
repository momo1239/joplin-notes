# Domain Recon
This section will review (at a relatively high level) some of the information you can enumerate from the current domain as a standard domain user. We'll cover many of these areas (domain trusts, Kerberos abuses, GPO abuses, etc) in much more detail when we get to those specific sections. For now, we'll see some of the different tooling that can be used to query the domain, and how we can obtain targeted information.

# How to do Domain Recon
PowerView is a script that uses powershell and wmi queries to retrieve information about the domain.

- Discover Local Admins
- Identify systems users are logged into
- Domain info

Bloodhound is an app that uses graph theory to display relationship between AD components for finding attack paths. Can use to see what your privs are what groups you are a member of to see where you can remote into.

Nested Groups - John is memebr of Group A, Group A is member of Grroup B, Group B is memeber of Group C, and Group C is part of Domain Admins Group. Not ez to see, but with bloodhound ez.

Manual AD Querying with Dsquery and Ldapsearch

dsquery.dll is on most windows system by default.
Ldapsearch on unix systems.

Query for object types like
- computer
- contact
- group
- ou
- site
- server
- user