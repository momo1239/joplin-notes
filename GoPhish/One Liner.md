User define $FQDN

```
sudo su; apt update; apt install snapd; snap install core; snap refresh core; snap install --classic certbot; ln -s /snap/bin/certbot /usr/bin/certbot; systemctl stop pca-gophish-composition.service; certbot certonly --standalone -d $FQDN --agree-tos --register-unsafely-without-email; cd /var/pca/pca-gophish-composition/secrets/gophish; cp /etc/letsencrypt/live/$FQDN/fullchain.pem phish_fullchain.pem; cp /etc/letsencrypt/live/$FQDN/privkey.pem phish_privkey.pem; chmod +r phish_privkey.pem; cd /var/pca/pca-gophish-composition/;
sed -i 's/published: 80/published: 443/g' docker-compose.yml; sed -i 's/use_tls": false/use_tls": true/g' config.json; echo 'user.name' | sudo tee -a /var/pca/pca-gophish-composition/secrets/postfix/users.txt; docker restart $(docker ps -a | grep postfix | cut -d " " -f 1); cd /var/pca/pca-gophish-composition/;
#Need a to see what password query output looks like and make array
docker-compose logs postfix|grep -A 9 password;
#Need a to see what password query output looks like and make array
cd /var/pca/pca-gophish-composition/;docker-compose logs gophish|grep password
```