# How to deploy

1. Run nginx container

```bash
docker-compose up -d nginx
```

2. Create SSL certificate by letsencrypt

```bash
docker-compose run --rm certbot certonly --webroot --webroot-path=/var/www/certbot -d webshina.xyz -d www.webshina.xyz
```

This command will create a new folder `./data/certbot/conf/live/webshina.xyz` with SSL certificate files.
And access to "/.well-known/acme-challenge" via nginx.

3. Connect other container to "nginx-proxy" docker network

```bash
docker network connect nginx-ssl_nginx-proxy <container_name>
```
