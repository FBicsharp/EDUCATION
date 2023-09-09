# DOCKER COMPOSE IN PILLS
Must be used for configure all in specific enviroment such as network, containers etc all in single file.
Create a file called docker-compose.yaml

consider the following command for start up  2 container  with some configuration,
```sh
#container 1
docker run -d \
--name mongodb \
-p 27017:27017 \
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=password \
--net mongo-network \
mongo
#container 2
docker run -d \
--name mongodb-express \
-p 8080:8080 \
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=password \
-e MONGO_INITDB_ROOT_SERVER=mongodb \
--net mongo-network \
mongo-express

```
if you have a lot of container to start and configure the works begin hard, so for do that automatically in sample way Docker compose allow us to predefine a script for configuring multipl containere and rules.

for example, if we convert the script above in a docker-compose.yaml the syntax will be :

```sh
#version of docker compose 
version: '3.8'
# list of servicies
services:
  #container 1
  mongodb:
    image: mongo
    ports:
      - "27017:27017"
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: "admin"
      MONGO_INITDB_ROOT_PASSWORD: "password"
  #container 2
  mongodb-express:
    image: mongo-express
    ports:
      - "8080:8081"
    restart: always
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: "admin"
      ME_CONFIG_MONGODB_ADMINPASSWORD: "password"
      ME_CONFIG_MONGODB_SERVER: "mongodb"
```

After defining the compose file , go in the folder of compose file and run this command in terminal

```sh
docker compose -f {file.yaml} up -d
```
Stopping all container started previuslly
```sh
docker compose -f {file.yaml} down -d
```