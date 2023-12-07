# What is Host Persistence
A method of regaining or maintainng access to a compromised machine without having to exploit the initial compromise step all over again. Workstation are voltatile because user logout or reboot.

Installing persistence usually involves config changes or dropping a payload to disk which carry high risk of detection but useful and essential in long term engagements.

Common persistence methods in userland:
HKCU / HKLM Registry Autoruns
Scheduled Tasks
Startup Folder

# How to Do Host Persistence
Scheduled Tasks

The Windows Task Scheduler allows us to create "tasks" that execute on a pre-determined trigger. That trigger could be a time of day, on user-logon, when the computer goes idle, when the computer is locked, or a combination thereof.

Let's create a scheduled task that will execute a PowerShell payload once every hour. To save ourselves from having to deal with lots of quotations in the IEX cradle, we can encode it to base64 and execute it using the -EncodedCommand parameter in PowerShell (often appreciated to -enc).

Use SharPersist or command line schtasks /create /sc minute /mo 1 /tn "eviltask" /tr C:\tools\shell.cmd /ru "SYSTEM"
****
- Applications, files and shortcuts within a user's startup folder are launched automatically when they first log in. It's commonly used to bootstrap the user's home environment (set wallpapers, shortcut's etc).

- AutoRun values in HKCU and HKLM allow applications to start on boot. You commonly see these to start native and 3rd party applications such as software updaters, download assistants, driver utilities and so on.

