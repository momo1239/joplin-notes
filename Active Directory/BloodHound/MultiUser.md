# MultiUser Bloodhound
## Neo4j config
Changing Bloodhound's password would not be necessary if I knew DHS' Bloodhound password.
It's done here because Blodhound automatically launches on localhost, logging out clears the password field.
This step resets the neo4j password so other uses can log in.

Uncomment `dbms.security.auth_enabled=false` in /usr/share/neo4j/conf/neo4j.conf
The line `dmbs.default_listen_address=0.0.0.0` should already be uncommented, if not uncomment it
Run `/usr/share/neo4j/bin/cypher-shell -d systemn;ALTER USER neo4j SET PASSWORD 'bloodhound';:exit`

## Allow remote hosts
```
ufw allow from <subnet>
ufw allow 7678 #maybe optional
```
## Start Neo4j/Bloodhound
`systemctl restart neo4j.service` or `/etc/init.d/neo4j restart`
If you want to start Bloodhound too, run `/root/tools/Bloodhound/bloodhound.sh`

