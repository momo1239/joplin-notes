Get user domain
````
net domain
````
Domain user enumeration
````
run net accounts /domain
````
Local Enumeration
````
whoami /groups
run net localgroup Administrators
````
Identify DC
````
net domain #should return domain name
run nslookup type=all _ldap._tcp.dc._msdcs.<domain name> #should return DC hostname and IP
````
