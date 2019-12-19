# Build
IMAGEID=$(cat /dev/urandom | tr -dc 'a-z0-9' | fold -w 32 | head -n 1) && sudo docker build -t $IMAGEID .

# Run new container
CONTAINER_ID=$(sudo docker run -t -d -p 0.0.0.0:81:80 $IMAGEID)

# Login into running containe. REQUIRES DOCKER >= 1.3
sudo docker exec -it $CONTAINER_ID bash

# !!! DEPRECATED !!! Login into running container. REQUIRES DOCKER < 0.9
FULL_CONTAINER_ID=$(sudo docker ps -a -notrunc -q | grep -F $CONTAINER_ID) && sudo lxc-attach -n $FULL_CONTAINER_ID /bin/bash

# Container stdout
sudo docker logs $CONTAINER_ID

# Inspect container config
sudo docker inspect $CONTAINER_ID

# Stop container
sudo docker stop $CONTAINER_ID

# Stop all containers
sudo docker stop $(sudo docker ps -a -q)

# Remove all docker images
sudo docker rmi $(sudo docker images -a -q)

# Stop and Remove all docker containers
sudo docker stop $(sudo docker ps -a -q) && sudo docker rm $(sudo docker ps -a -q)

# Stop and Remove all docker containers + Remove all docker images
sudo docker stop $(sudo docker ps -a -q) && sudo docker rm $(sudo docker ps -a -q) && sudo docker rmi $(sudo docker images -a -q)

# Show all docker containers memory consumption
for line in `docker ps | awk '{print $1}' | grep -v CONTAINER`; do docker ps | grep $line | awk '{printf $NF" "}' && echo $(( `cat /sys/fs/cgroup/memory/docker/$line*/memory.usage_in_bytes` / 1024 / 1024 ))MB ; done

