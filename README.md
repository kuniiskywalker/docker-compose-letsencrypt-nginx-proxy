
## 1. Create a new Docker network.
```
docker network create --driver bridge reverse-proxy
```

## 2. Run proxy container
```
docker-compose up -d
```

## 3. Add your docker-compose.yml or options of exec docker run

### case of docker-compose

```docker-compose.yml
version: '2'
# service
services:

  web:
    image: nginx
    container_name: site-a 
    networks:
      - reverse-proxy
      - back
    environment:
      VIRTUAL_HOST: example.com
      LETSENCRYPT_HOST: example.com 
      LETSENCRYPT_EMAIL: webmaster@example.com
    restart: always

networks:
  reverse-proxy:
    external:
      name: reverse-proxy
  back:
    driver: bridge
```

### case of exec docker run
```
docker run -d \
    --name site-a \
    --net reverse-proxy \
    -e 'LETSENCRYPT_EMAIL=webmaster@example.com' \
    -e 'LETSENCRYPT_HOST=example.com' \
    -e 'VIRTUAL_HOST=example.com' nginx
```
