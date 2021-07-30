# Basic Docker Commands

### CONTAINERS
- The "docker run" command looks for an image locally first, if not found, it pulls from docker hub
```
docker run <IMAGE> (e.g. "postgres:9.6")# = docker pull <IMAGE> & docker start  

# in detached mode
docker run -d redis

# in a special port
docker run -p6000:6379 redis

# giving a name, e.g. "redis-older" to redis:4.0
docker run -d -p6001:6379 --name redis-older redis:4.0
```
- List all Docker containers using the command:
```
docker ps  # running containers
docker ps -a  / docker container ls -a   # all containers
docker ps -aq / docker container ls -aq # just ids
```
- To start a specific container
```
docker start <CONTAINER_ID>
```
- To stop a specific container
```
docker stop <CONTAINER_ID>
```
- To stop all containers, enter:
```
docker stop $(docker ps -a -q)
```
- To remove all stopped containers:
```
docker rm $(docker ps –aq)
```
- Remove all exited containers
```
docker ps -a -f status=exited
docker rm $(docker ps -a -f status=exited -q)
```
- To wipe Docker clean and start from scratch, enter the command:
```
docker container stop $(docker container ls –aq) && docker system prune –af ––volumes
```

### IMAGES
-  listing all the images on your system:
```
docker images 
docker images -a # intermediate images
# or
docker image ls  
docker rmi image_id
```
- Remove images
```
docker rmi image_id1 image_id2
```
-  Remove dangling images
```
docker rmi $(docker images -f "dangling=true" -q)
```
- There is also a prune command available in docker to remove dangling images ( images, which are not used by any containers )
```
docker image prune
```
- This removes all (–a) images created over the last 24 hours. 
```
docker image prune –a ––filter “until=24h”
```
- Remove All Unused Docker Objects. The prune command automatically removes all resources that aren’t associated with a container.
```
docker system prune
```
- Build an image from a specified docker file
```
docker build -t <MY-APP:1.0> <path to docker file>
```

### VOLUMES
- 3 Volume types
<img src="https://github.com/HackTechGO/Docker/blob/master/assets/hosted-volume.png">
 ```
# 1) host volumes
docker run -v /home/mount/data:/var/lib/mysql/data
```

<img src="https://github.com/HackTechGO/Docker/blob/master/assets/anonymous-volume.png">
 ```
# 2) anonymous volumes
docker run -v /var/lib/mysql/data
```

<img src="https://github.com/HackTechGO/Docker/blob/master/assets/named-volume.png">
 ```
 # 3) named volumes - Production
docker run -v name:/var/lib/mysql/data
```

- Volumes are created individually and attached to the container for storing data. Removing the container will remove the volume. 
And volumes are not in use and are called dangling volume.
```
docker volume ls
docker volume ls -f dangling=true # List dangling volumes

docker volume rm volume_name volume_name
docker volume rm $(docker volume ls -f dangling=true -q) # Remove dangling volumes
```

### DEBUGGING
- Logs
```
docker logs <CONTAINER_ID or CONTAINER_NAME>

# only tail
docker logs <CONTAINER_ID or CONTAINER_NAME> | tail

# streaming the logs, so you do not do "docker logs" all the time
docker logs <CONTAINER_ID or CONTAINER_NAME> -f
```
- Exec / get the terminal of the running container
```
docker exec -it <CONTAINER_ID or CONTAINER_NAME> /bin/bash
# or if bash not installed
docker exec -it <CONTAINER_ID or CONTAINER_NAME> /bin/sh
```

### NETWORK
- Listing networks
```
docker network ls
```
- Creating network
```
docker network create <NETWORK_NAME>

# runing an image in a specific network, e.g. mongo and mongo-express
# mongodb
docker run -d \
  -p 27017:27017 \
  --network mongo-network \
  --name mongodb \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  mongo
 # mongo-express
 docker run -it --rm \
    --network mongo-network \
    --name mongo-express \
    -p 8081:8081 \
    -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
    -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
    -e ME_CONFIG_MONGODB_SERVER="mongodb" \
    mongo-express
```

### DOCKER COMPOSE
- Up
```
docker-compose -f FILE.yaml up
```
- Down
```
docker-compose -f FILE.yaml down
```
- Yaml example, mongodb and mongo-express
```
version: '3'
services:
  # my-app:
  # image: ${docker-registry}/my-app:1.0
  # ports:
  # - 3000:3000
  mongodb:
    image: mongo
    container_name: "mongo"
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
      - mongo-data:/data/db
  mongo-express:
    image: mongo-express
    container_name: mongo-express
    ports:
      - 8081:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
      - ME_CONFIG_MONGODB_SERVER=mongodb
      - ME_CONFIG_OPTIONS_EDITORTHEME=ambiance
    depends_on:
       - mongodb
volumes:
  mongo-data:
    driver: local
```
