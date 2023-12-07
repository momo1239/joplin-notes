## Download/Mod Chrome Profile
```
cd /opt/cobaltstrike_4.0/Malleable-C2-Profiles/normal/;
curl -k https://raw.githubusercontent.com/xx0hcd/Malleable-C2-Profiles/master/normal/chrome_browser.profile -o chrome-browser.profile;
#Set sleep to 10.5 seconds
sed -i '7s/38500/10500/g' chrome-browser.profile;
#Allow staging
sed -i '12s/false/true/g' chrome-browser.profile;
#Enable SSL
sed -i '49,52s/#//g' chrome-browser.profile;
##Add edit to domain Key Store, lines 49-52##
sed -i '50s/domain001.store/####/g' chrome-browser.profile
sed -i '51s/password123/####/g' chrome-browser.profile
```
