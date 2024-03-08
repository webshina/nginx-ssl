# How to deploy

1. Create SSL certificate by letsencrypt

```bash
docker-compose run --rm certbot certonly --webroot --webroot-path=/var/www/certbot -d webshina.xyz -d www.webshina.xyz
```

2. Run docker image

```bash
docker-compose up -d
```
