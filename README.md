# Docker-notes
Containerisation

5 couples -> 10 members
12 members
20-25 members joint family
individual house

4members -> small family
mother and father -> 2 kids
a flat in apartment is enough

individuals -> metros
PG or a single room

Individual vs Flat vs PG room
==============================
advantages
==========
completely ours, space is not shared
privacy
own design

disadvantages
============
maintaince on us
	electric
	plumbing
	security agency
costly
time taking

Flat
====
advantages
===
Less maintaince
	Electric
	Plumbing
	Security
Less cost
Time is also less

disadvantages
======
less privacy
shared space
not your design
rules

Individual PG
========
advantages
====
less cost
no maintaince
less time

disadvantages
=====
no privacy
everything is shared

HTML CSS JavaScript -> Frontend
Java or .NET -> Backend

J2EE -> Java Enterprise Edition
Servlets and JSP(Java Server pages) for viewing -> spring also depends on this
a single file -> frontend+backend
roboshop.war + dependencies(libraries) jar files
roboshop.ear (enterprise archeive)
150-200MB

A simple change in anything should again follow the release process

Client side BA -> Our BA
BA, Delivery Manager, Architects, Leads -> meeting
1 month
DEV SIT UAT PROD

9PM -> 6AM
Bridge -> L1 support, L2 support, L3 support(Developers) monitoring, service desk, client, managers

Seam framework(Jboss)
Struts
Spring (Evolution and Revolution)

Weblogic, Webspher, Jboss, Wildfly

Monolithic
=============
Frontend -> Backend(Entire app should be in single programming language)
API
.war
Apache tomcat, JBoss Web Server

Microservices
=============
Catalogue, Cart, Shipping
API
every component in diff programming language
MBs
Containers

Booting time

Virtulisation			Containerisation
Costly					Cheaper
More boot time			less boot time
more size				less size
resource utilisation	resource utilisation is more
is less
More privacy			Less privacy
Less portable			More portable

sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo

sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

a docker group is created
only root user and users inside docker group will have acess to docker cli

sudo usermod -aG docker ec2-user

exit and login again

docker ps -> all running containers

images

AMI = OS + System Packages + App run time + App code + App libraries -> Amazon machine image

Docker image = Bare minimum OS(10Mb) + System Packages + App run time + App code + App libraries -> Amazon machine image
500Mb

Image is static, Server is running version of OS

if you run image you get container(running instance of image)

docker ps -> running containers
docker pull nginx -> pulling the nginx image
docker images -> images in the local docker repo

docker pull + docker create ImageID + docker start ContainerID

docker run = docker pull + create + start


image and container
image = bare min OS + System pacakges + Application Runtime + App code + dependencies
if You run image we get container, container is the running instance is image

docker ps -> all running docker containers
docker ps -a -> all containers
docker images -> images available in the server
docker pull <image-name>:<tag> -> pulls the image from docker hub
docker create imageID -> creates container from image
docker start containerID -> starts the container

docker run = pull + create + start
docker run -d 

growpart /dev/nvme0n1 4
lvextend -L +30G /dev/mapper/RootVG-varVol
xfs_growfs /var
/var/lib/docker -> docker home repo

sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user

docker run -d -p <host-port>:<container-port> nginx

docker run -d -p 80:80 nginx

http://98.92.17.244

docker exec -it container-id bash
docker inspect image/containerID

Dockerfile -> Instructions to build custom images

Dockerfile

FROM
=====
refers to base OS of the image. your Dockerfile first instruction should be FROM

docker build -t <image-name>:<version> . -> . refers to current directory, it means Dockerfile is in current directory

docker push <URL>/<Username>/<image-name>:<version>


from 

docker.io/joindevops/from:version
ramesh/from

docker login

docker tag image-name:version username/image-name:version
docker push username/image-name:version

if you are pulling the image, first it checks whether it is available in local or not. if not available it pulls from central repo

docker rm -f `docker ps -a -q`

RUN
===
RUN instruction is used to configure the image. like installing packages, configurations, creating user, etc.

docker build --no-cache --progress=plain -t run:v1 .

CMD
====
referring the base image
configuring using RUN instruction

CMD ["nginx", "-g", "daemon off;"]

systemctl start nginx

CMD ["systemctl", "start", "nginx"] -> this will not work inside container, because does not have capabilities to contact kernal

instruction inside CMD should run the container for infinite time. command we are giving inside CMD should run in foreground, then we should take it into background

FROM almalinux:9

building the image
running the image

CMD -> instruction used to start the container

RUN -> executes at the time of image building
CMD -> executes at the time of container starting

image can have multiple run instructions
CMD should be only one, if we give multiple CMD last one is considered

LABEL
====
adds the metadata to the image, used for filtering

EXPOSE
======
it will not any functionality to the image/container, it adds the information about ports used by container

EXPOSE <port-number> -> it will not open the port, just for information

docker.joindevops.com/joindevops/from
Notes
#!/bin/bash
growpart /dev/nvme0n1 4
lvextend -L +30G /dev/mapper/RootVG-varVol
xfs_growfs /var
dnf -y install dnf-plugins-core
dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
systemctl start docker
systemctl enable docker
usermod -aG docker ec2-user

docker exec -it containerid bash

FROM -> refer the base OS/image
RUN -> configure the image, like installing packages, users, etc.
CMD -> command to start the container
LABEL -> add the tags, key value pairs. basically metadata
EXPOSE -> ports information
COPY -> copy files from workspace to image

docker build -t URL/username/image-name:version .
docker build -t joindevops/from:v1 .

docker tag image-name:version username/image-name:version
docker push

ENV KEY=VALUE

ADD
===
ADD is also like COPY to copy the content inside the image. But ADD has 2 extra capabilities

1. It can directly fetch the content from internet
2. It can directly untar the file inside container/image

CMD ENTRYPOINT
==========
1. CMD instruction can be overriden.
2. ENTRYPOINT can't be overriden, if we try it will go and append that leads to failure
3. CMD can be used to supply the default args to ENTRYPOINT. You can always override the default args at run time

ARG
===
ARG can be the first instruction in an exceptional case i.e to supply the version to the base image.. after FROM instruction we can't access ARG variable

ARG vs ENV
==========
1. ENV is used to supply the key value pairs for the container/runtime
2. ARG is used to supply the values to variables at build time.
3. ARG can't be accessible inside container.

FROM -> refer base OS
RUN -> configure the image, install, create user, folder, etc.
CMD -> command to start the container
ENTRYPOINT -> command to start the container, but can't be overriden. used with CMD, CMD can supply default args. Users always can override default args
LABEL -> adds metadata to the image. used to filter
EXPOSE -> info about ports opened by container
ENV -> environment variables, containers can use
ARG -> build time variables. can be before FROM instruction to supply version to base OS. build variables can't be accessed inside container
COPY/ADD -> copies the files from local to image. ADD can download file from internet. ADD can directly untar the file into the image
USER -> default user to run the container
WORKDIR -> working directory for the container/image
ONBUILD -> force the users to follow ONBUILD instructions while using some image

1. host -> you no need docker interface, directly connect to host
2. bridge -> docker creates another virtual network interface inside server and allocates the IP address to containers

-p 80:80

docker default bridge network can't communicate between containers

1. use bridge network
2. create your own network interface

docker run -d --name rabbitmq --network roboshop -e RABBITMQ_DEFAULT_USER=roboshop -e RABBITMQ_DEFAULT_PASS=roboshop123 rabbitmq:3

1. Build the image
2. How to run the image

for i in mongodb mysql catalogue user cart shipping payment frontend ; do cd $i; docker build -t $i:v1 . ; cd ..; done

docker compose
===============
it is simple yaml used to up or down the services at a time, define dependencies, networks, volumes, etc.
services are dependent on each other. 

docker create network roboshop
docker run -d -p 80:80 --name frontend --network roboshop frontend:v1

by default containers are ephermeral, if you remove container data also will be deleted. to persist the data we need to use volumes
1. un named volumes. if you create directories/volumes manually we need to manage them
2. named volumes. docker can create volumes and manage using docker commands

stateful and stateless
mongodb redis mysql rabbitmq -> data
stateless -> not data, just code.. we are already storing code in git

package.json
server.js
node_modules -> dependencies

Multi stage builds
=================
Having multiple dockerfiles, one dockerfile can be used as builder, we copy the content required from that to the final image. in this process we can remove un-necessary installtions, build cache, etc. we can reduce image size using multi stage builds

1. Use official small images like alpine
2. use mutli stage builds
3. use volumes and custom networks
4. use labels and expose
5. Optimise layers
6. don't hardcode secrets, we will handle this in kubernetes level
7. use dockerignore to ignore un-necessary files related to docker
8. use entrypoint and cmd
9. dont use root user, use normal user

1. Build the image
	* Optimise the image
2. Run the image
	* How are you providing configuration
	
1. Official small images
2. Multi stage builds
3. Non root containers
4. Labels and Expose
5. Use custom networks and volumes
6. Use dockerignore
7. Limit the resources and perform health checks
8. Optimise layering
	* Reduce number of layers, frequently changing instruction should be at last
	* Combine multiple instructions into single instruction, that speeds up the build prcess
9. use entrypoint and cmd

JDK and JRE
JDK -> Java development kit = JRE + DEV tools
JRE -> Java runtime environment