# How to deploy

2. Run nginx container

```bash
docker-compose up -d nginx
```

3. Create SSL certificate by letsencrypt

```bash
docker-compose up -d certbot
docker exec -it certbot sh
certbot certonly --webroot --webroot-path=/var/www/certbot -d webshina.xyz

Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): kazu4101719@gmail.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.3-September-21-2022.pdf. You must
agree in order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: y
Account registered.
Requesting a certificate for webshina.xyz and www.webshina.xyz

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/webshina.xyz/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/webshina.xyz/privkey.pem
This certificate expires on 2024-06-07.
These files will be updated when the certificate renews.

NEXT STEPS:
- The certificate will need to be renewed before it expires. Certbot can automatically renew the certificate in the background, but you may need to take steps to enable that functionality. See https://certbot.org/renewal-setup for instructions.
We were unable to subscribe you the EFF mailing list because your e-mail address appears to be invalid. You can try again later by visiting https://act.eff.org.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

This command will create a new folder `./data/certbot/conf/live/webshina.xyz` with SSL certificate files.
And access to "/.well-known/acme-challenge" via nginx.

5. Include other container to nginx.conf

```bash
sudo ln -s ./sites-available/mypage.conf ./sites-enabled/
sudo ln -s ./sites-available/favo.conf ./sites-enabled/
```

6. Rerun nginx container

```bash
docker-compose stop nginx
docker-compose up -d nginx
```

6. Connect other container to "nginx-proxy" docker network

```bash
docker network connect nginx-ssl_nginx-proxy <container_name>
```
