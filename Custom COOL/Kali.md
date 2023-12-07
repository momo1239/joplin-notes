# Installs
## Sublime Text
```
sudo wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt-get install apt-transport-https
sudo echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install sublime-text
```
## Xclip
```
sudo apt install xclip
```
# Env mods
## Impacket
```
echo "PATH=/usr/share/doc/python3-impacket/examples:$PATH" >> ~/.bashrc
for i in $(ls /usr/share/doc/python3-impacket/examples/);do sudo sed -i 's/\#\!\/usr\/bin\/env python/\#\!\/usr\/bin\/env python3/g' /usr/share/doc/python3-impacket/examples/$i;done
```
## Aliases
```
echo "alias cme='crackmapexec'" >> ~/.bashrc
```
# Notes
## Nessus
