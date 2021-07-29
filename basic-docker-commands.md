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


#### IMAGES
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
docker build <path to docker file>
```


### VOLUMES
- Volumes are created individually and attached to the container for storing data. Removing the container will now remove the volume. 
And these volumes are not in use and are called dangling volume. You can list the volumes using list command after confirming, you can remove it.
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
```
- Exec / get the terminal of the running container
```
docker exec -it <CONTAINER_ID or CONTAINER_NAME>
```
