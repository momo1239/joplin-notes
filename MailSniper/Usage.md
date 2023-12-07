MailSniper for Password Spraying

# General Information
A nice little tool for searching and enumerating Microsoft Exchange. The guys at Black Hills have done a much better [write up](https://www.blackhillsinfosec.com/introducing-mailsniper-a-tool-for-searching-every-users-email-for-sensitive-data/) than I would do, so I would suggest checking it out.

There is also a [Field Manual](http://www.dafthack.com/files/MailSniper-Field-Manual.pdf) available for some basic commands.

The tool has some great features for searching internally, but this is a quick write up on using the tool for password sparying for the first week of the engagement.

# Environment Setup 
The tool can be pulled down from the [GitHub](https://github.com/dafthack/MailSniper) page. Nothing super fancy here, just a `powershell` cmdlet that you need to import into your Windows Attack machine. 

# Attack Example
## Step 0 - Find an External OWA Instance
In order for this to work, we will need to find an external facing OWA instance and take note of the associated FQDN for our attack. 

For this demo, we will assume the web server is running at *mail.domain.com*.

## Step 1 - Get the Internal Domain Name
There are a few different ways to do this, but `MailSniper` provides a module called `Invoke-DomainHarvestOWA` that automates the process. It can be ran with:

```PS
Invoke-DomainHarvestOWA -ExchHostname mail.domain.com
```
For the sake of this write up, we will assume the internal domain is *internaldomain*.

## Step 2 - Enumerate Users
With the domain in hand, we can now create a list of potentially valid usernames. This is where OSINT comes in handy to figure out how the user accounts are named, and to gather a potential list of user first and last names. This list will be saved locally as *possible_users.txt*.

We can now use a timing attack to determine valid usernames via the `Invoke-UsernameHarvestOWA` module with the syntax:

```PS
Invoke-UsernameHarvestOWA -ExchHostname mail.domain.com -UserList C:\path\to\possible_users.txt -Domain internaldomain -Threads 1 -Outfile C:\path\to\valid_users.txt
```

Note here that I used the `-Domain` flag to append the discovered domain as my *possible_users.txt* file only has usernames. If you want, you can create a *possible_users.txt* file with the domain appended, e.g. DOMAIN\Username and then this flag would not be necessary.

This attack will first generate known bad users (these have randomly generated names like 'PqVjHW' that shouldn't exist) and use this to baseline the response time. Assuming nothing goes wrong here, we should start to see messages of 'Potentially Valid!' for various users. These will be written to the text file that we created with the `-Outfile` flag in our command.

## Step 3 - Password Spray
Obviously use common sense here to not mass lock out every user. I try to one spray at the start of the day, and one spray in the evening. As with the other commands, this has some simple syntax, but this time with the `Invoke-PasswordSprawOWA` module.

```PS
Invoke-PasswordSprawOWA -ExchHostname mail.domain.com -UserList C:\path\to\valid_users.txt -Password Password1! -Threads 15 -OutFile C:\valid_creds.txt
```

Modify the above command to rotate the `-Password` field for each spray. You will probably have the best luck with variations of the organization name, month/year, season/year, and 'password'. If you get lucky, make sure you check for single factor websites to gain a foothold into the internal network, or look into some other features of `MailSniper` to enumerate open mailboxes, or search for keywords in the compromised users' inbox.