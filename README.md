# docker
Set of commands and tutorials for using docker

DOCKER

Docker Hub : repo for widely used docker images

Docker Commands
run : runs a container of an image if exists locally else pulls the image from hub.
	         e.g docker run nginx
ps : lists all the running containers with basic info like id, time etc
ps -a : lists all running and not running containers
stop “name or id of container” : stops a running container
rm “name or id” : removes a container from space
images : lists the downloaded images currently on the system with their sizes
rmi “image name” : removes an image (ensure that this image is not running in a container
pull “image” : downloads an image from hub
Note : a docker container stops as soon as it completes the intended task .
exec “image” commands : executes a set of commands in a running container
run -d “image” : this runs the container in background mode
attach “container id” : this again attaches you to background container 
run -it centos bash : runs the centos container, logs you in using -it flag and opens bash
-i : interactive mode
-t : terminal mode
-p External_port : internal_port : -p flag specifies which external port the internal port will be mapped to. 
-v External_directory : internal_directory : maps docker volume to external location
inspect “name or ID” : returns data about a container in a JSON format
logs “id or name” : to see the output of container in background

Docker Image 
We can build docker image using Dockerfile
Dockerfile is in an instruction argument format with instructions on left and arguments on right e.g RUN (instruction)  Ubuntu (argument)
Docker caches each layer and when rebuilt,  it only rebuild from the updated layer

E.g for Dockerfile

FROM ubuntu
RUN apt-get update && apt-get -y install python3 python3-pip
RUN pip install flask flask-mysql
COPY app.py /opt/source-code/app.py
ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run --host=0.0.0.0






To push your local image to docker hub
login  - `sudo docker login` : Enter username and password
Build the image by running ‘your_username/image_name’ as name of your image
Run : `docker push image_name`
