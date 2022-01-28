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


Docker Image 
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

Docker Compose
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
3. networks (version 2 or higher) : a list of networks with (optional) config
