LLMNR / NBT NS is essentially the backups for DNS. They are on by default by most windows host. When your host request for a hostname. Ask dns recursively over and over. If it cant find google it will come back and say hey dont have any entry. Your local host will use llmnr or nbt ns to try to find the host name on the local subnet that your host is on. If you are an attacker sitting on the network and see that traffic you can poison that answer and say yes i am that host connect to me. And when that happens the victim will pass an ntlmv1/v2 hash over to my attacking box because its trying to authenticate to me. So traditionally theyll do the responder attack and take the hash offline to crack it. But you can actually take that session that is being authenticated to you and relay it to another host to execute code on that system.

SMB signing needs to be disabled to relay the challenge/response. You can use nmap to locate hosts with smb signing disabled. 

This authentication takes place in 3 steps:
First the client tells the server that it wants to authenticate.
The server then responds with a challenge which is nothing more than a random sequence of characters.
The client encrypts this challenge with its secret, and sends the result back to the server. This is its response.
This process is called challenge/response.
