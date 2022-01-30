# docker
Set of commands and tutorials for using docker

DOCKER

Docker Hub : repo for widely used docker images

Docker Commands
1. run : runs a container of an image if exists locally else pulls the image from hub.
         e.g docker run nginx
2. ps : lists all the running containers with basic info like id, time etc
3. ps -a : lists all running and not running containers
4. stop “name or id of container” : stops a running container
5. rm “name or id” : removes a container from space
6. images : lists the downloaded images currently on the system with their sizes
7. rmi “image name” : removes an image (ensure that this image is not running in a container
8. pull “image” : downloads an image from hub
Note : a docker container stops as soon as it completes the intended task .
9. exec “image” commands : executes a set of commands in a running container
10. run -d “image” : this runs the container in background mode
11. attach “container id” : this again attaches you to background container 
12. run -it centos bash : runs the centos container, logs you in using -it flag and opens bash
13. -i : interactive mode
14. -t : terminal mode
15. -p External_port : internal_port : -p flag specifies which external port the internal port will be mapped to. 
16. -v External_directory : internal_directory : maps docker volume to external location
17. inspect “name or ID” : returns data about a container in a JSON format
18. logs “id or name” : to see the output of container in background


# Docker Image 
We can build docker image using Dockerfile
Dockerfile is in an instruction argument format with instructions on left and arguments on right e.g RUN (instruction)  Ubuntu (argument)
Docker caches each layer and when rebuilt,  it only rebuild from the updated layer

E.g for Dockerfile

``` - FROM ubuntu
- RUN apt-get update && apt-get -y install python3 python3-pip
- RUN pip install flask flask-mysql
- COPY app.py /opt/source-code/app.py
- ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run --host=0.0.0.0 
```






To push your local image to docker hub
login  - `sudo docker login` : Enter username and password
Build the image by running ‘your_username/image_name’ as name of your image
Run : `docker push image_name`

# Docker Compose
Create a YAML for defining all the config.
There are 3 versions 1, 2 and 3 .  

We will take example for version 2

1. version : only in version 2 and 3, specifies the docker-compose version.
2. services : this contains all the main config about images . It is an dictionary of different services like postgre, redis etc.
   * image : name of the image that exists on local or docker hub 
   * build : if an image is to be built, we define build instead of image and this contains the location of the dockerfile for the config of the image
   * ports : a list of port mappings for the given image. External_port : Internal_port
   * links (NOT USED IN VERSION 2 and 3): a list of dependencies to other images
   * depends_on (only with version 2 or higher) : list of dependent images.
   * network (version 2 or higher) : a list of networks for the image
3. networks (version 2 or higher) : a list of networks with (optional) config.

# Docker Architecture 
* –cpus : this command restricts cpu usage for a container , e.g. –cpus=0.5 means restricts till 50% of cpu usage
* –memory : this restricts memory for an isolated containers. E.g –memory=100m means restricts till 100 mbs of memory.
* Namespaces = Since each container is isolated, it can have any PID e.g 1. but we can have a number of isolated containers so giving each container to act as root (start PIDs from 1) will be difficult. Therefore docker uses Namespaces for isolated containers to distinguish them from other containers, inside a container, it act as root .
* docker exec “container_id” ps -eaf : list all the processes inside a container

# Docker File System
When we download  docker on our system, it creates a directory in `/var/lib/docker`.

```
/var/lib/docker
aufs
volumes
image
containers
```

# Docker Layered Architecture
When we build from a Dockerfile, each line is stored as a different layer
E.g FROM Ubuntu, it stores the ubuntu image in one layer, apt-get updates in another layer etc. So each layer only contains the changes from the previous layer. Whenever we modify any layer, docker uses the cache of previous layers. 
Even if we build a separate image that has common layers like same Ubuntu image or packages, docker still uses the cache of the same layers and only downloads the newer content.

# Docker Image layer And Container layer
* Docker’s image layer is a READ only layer, once an image is built, no changes can be done to it
* Docker’s container layer is one which contains all container related things and is a READ WRITE layer. 
If we want to change anything from an image, docker first copies the image to container layer and then runs our changes .
Docker won’t store anything to image layer until we build the image i.e. if a container is removed, the copied image will also be removed

# Docker Volumes
* If we want to persist the data for a container i.e. keep the data even if the container is removed, we can map its storage to an outside folder
* Volume mounting : in this , when we create a volume, it is created inside the docker’s volume folder
* `docker run -v VOLUME_NAME : CONTAINER’S DIRECTORY  IMAGENAME`
The above command  creates a volume folder inside `var/lib/docker/volume` and stores the container’s data
Bind mounting : in this , a container’s data is stored in a system’s location of user’s choice and not in `var/lib/docker/volume` 
* `docker run -v LOCATION : CONTAINER’S DIRECTORY IMAGENAME`
* `docker run –mount type=bind, source=container’s location , target=on machine directory IMAGENAME `


# Docker Networking
There are 3 networks in docker : bridge, none, host.
1. bridge networks : this is the default network to which a container is associated with if nothing is specified. 
* All containers initially are connected internally in the bridge network
* Mostly it starts with 172.17
* To make these container available to outside world, we map them externally
* To interact with other containers, we can specify the container name. E.g to connect to mysql , instead of specifying address , we can type the container;s name. E.g mysql.connector(mysql) where mysql is container’s name.

2. host network : if host is specified, the container will be in the host network but this will make the host’s port busy where the container is situated. E.g if we specify 5000 as port and network as host, the machine’s 5000 will run the container and we cannot run anything on port 5000.
* to run this `docker run Ubuntu –network=host`
3. none network : in this the container runs isolated and cannot interact with other containers or outside world
* to run `docker run Ubuntu –network=none` 

# Orchestration
A set of tools to automate host deployments depending on load and balance it and even create newer hosts when one fails.


1. Docker Swarm : this is provided by docker but doesn’t have many advanced features. In this , one host is made the ‘manager’ and other are worker nodes
* `docker swarm init` : this initializes the host as manager and generates a token.
* `docker swarm join –token <token>` : this is run on all the hosts to join with the manager.
* `docker run –replicas=3 mysql` : this command is run on the manager and it creates 3 replicas on 3 different workers for the mysql
* We can use the docker flags like -p for port, -v for volume etc with all the commands

2. Kubernetes : This is the most popular orchestration tool and even use docker or some other alternatives to deploy containers.
* In kubernetes, you have nodes, there should be multiple nodes as if one fails, other works.
* One node is a master node and other are worker nodes.
* It provides functions like autoscaling when load is higher, rollback softwares etc.

