# Setup
## Enable HTTPS for GoPhish
### Install Certbot
```
sudo apt update
sudo apt install snapd
sudo snap install core
sudo snap refresh core 
sudo snap install --classic certbot 
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo systemctl stop pca-gophish-composition.service
sudo apt install letsencrypt
```
### Obtain & Configure Cert
```
FQDN=mail.fed-survey.org
sudo certbot certonly --standalone -d $FQDN --agree-tos --register-unsafely-without-email
cd /var/pca/pca-gophish-composition/secrets/gophish 
sudo cp /etc/letsencrypt/live/$FQDN/fullchain.pem phish_fullchain.pem
sudo cp /etc/letsencrypt/live/$FQDN/privkey.pem phish_privkey.pem
sudo chmod +r phish_privkey.pem
```
### Reconfig Listening Port
```
cd /var/pca/pca-gophish-composition/
sudo sed -i 's/published: 80/published: 443/g' docker-compose.yml
cd ./secrets/gophish/
sudo sed -i 's/use_tls": false/use_tls": true/g' config.json
```
### Start GoPhish
```
sudo systemctl start pca-gophish-composition.service
```
## Add custom senders/users to Postfix
```
echo 'user.name' | sudo tee -a /var/pca/pca-gophish-composition/secrets/postfix/users.txt
docker restart $(docker ps -a | grep postfix | cut -d " " -f 1)
```
## Retrieve usernames/passwords
### Postfix
```
cd /var/pca/pca-gophish-composition/;docker-compose logs postfix|grep -A 15 password
```
### Default GoPhish Admin
```
cd /var/pca/pca-gophish-composition/;docker-compose logs gophish|grep password