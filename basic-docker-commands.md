# Basic Docker Commands

### CONTAINERS
- The docker run command first creates a writeable container layer over the specified image, and then starts
```
docker run vs docker start
```

- List all Docker containers using the command:
```
docker ps  # running containers
docker ps -a  / docker container ls -a   # all containers
docker ps -aq / docker container ls -aq # just ids
```

- To stop a specific container
```
docker stop [container_id]
```

- To stop all containers, enter:
```
docker stop $(docker ps –aq)
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
