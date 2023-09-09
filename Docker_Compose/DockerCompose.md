# DOCKER COMPOSE IN PILLS
Create a file called docker-compose.yaml
configure the containere and the network

```sh
version: "3"
services:
    websites:
        image: nginx
        ports:
            - "8080:80"
        restart: always

```



```sh
docker compose up -d
```


```sh
docker compose down
```