# Local Privesc
Host privilege escalation allows us to elevate privileges from that of a standard user to Administrator. It’s not a necessary step, as we’ll see in later modules how it's possible to obtain privileged credentials and move laterally in the domain without having to "priv esc" first. 

However, elevated privileges can provide a tactical advantage by allowing you to leverage some additional capabilities. For example, dumping credentials with Mimikatz, installing sneaky persistence or manipulating host configuration such as the firewall.

# How to do Local Privesc
- Weak Service Permissions. Change binary path.
- Unquoted Service Paths. Where path to service binary is not wrapped in quotes. Windows tries to read the path to executable and interprets the space as a terminator so it will attempt to execute a binary in that path before the real one. But you need write permissions into them.
- Weak Service Binary Permissions. Weak permission on the service binary itself. Allows you to overwrite the binary with something else.

Always Install Elevate
- This policy allows standard users to install applications that require access to directories and registry keys that they may not usually have permission to change. This is equivalent to granting full administrative rights and even though Microsoft strongly discourages its use, it can still be found.

You can use tools like PowerUp / SharpUp to enumerate host misconfigurations for privilege escalations as well.

On linux
- bash history might have creds
- misconfigured sudo rules
- kernel exploitation (rare)