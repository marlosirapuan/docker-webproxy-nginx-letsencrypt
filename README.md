# Web Proxy using Docker, NGINX and Let's Encrypt

Copy `.env.sample` to `.env` (`/nginx` folder)
```
cp .env.sample .env
```

Change info `.env` and save
```
vim .env
```

Create the network (you only need to do this once)
```
source .env
docker network create $NETWORK
```

Run command below to copy nginx template:
```
curl https://raw.githubusercontent.com/jwilder/nginx-proxy/master/nginx.tmpl > nginx.tmpl
```

Run
```
docker-compose up -d
```
Set up docker-compose and up container each project.  

Credits:  
[https://github.com/evertramos/docker-compose-letsencrypt-nginx-proxy-companion](https://github.com/evertramos/docker-compose-letsencrypt-nginx-proxy-companion)
