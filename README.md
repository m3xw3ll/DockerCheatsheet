# Docker Cheat Sheet

---

![](docker_logo.png)

Quick reference guide for useful Docker commands I used in the past.
# Table of Content

- [Basic Defintions](#basic-definitions)
  * [What is Docker?](#what-is-docker)
  * [The Docker platform](#the-docker-platform)
  * [Docker vs. VM`s](#docker-vs-vms)
  * [Docker architecture](#docker-architecture)
    * [Docker daemon](#docker-daemon)
    * [Docker client](#docker-client)
    * [Docker registries](#docker-registries)
    * [Image](#image)
    * [Container](#container)
- [Installation](#installation)
  * [Windows](#windows)
  * [Mac](#mac)
  * [Linux](#linux)
- [Dockerfiles](#dockerfiles)
- [Docker Images and Containers](#docker-images-and-containers)
  * [Images](#images)
  * [Containers](#containers)
- [Docker Network: Connecting Containers](#docker-networks-connecting-containers)
- [Docker Volume](#docker-volume)
- [Docker Compose](#docker-compose)


---
# Basic Definitions

## What is Docker?
Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your
applications from your infrastructure, so you can deliver software quickly. With Docker, you can manage your
infrastructure in the same ways you manage your applications. By taking advantage of Docker´s methodologies for shipping,
testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in
production. 

## The Docker platform
Docker provides the ability to package and run an application in a loosely isolated environment called a container. The 
isolation and security allow you to run many containers simultaneously on a given host. Containers are lightweight and 
contain everything needed to run the application, so you do not need to rely on what is currently installed on the host. 
You can easily share containers while you work, and be sure that everyone you share with gets the same container that 
works in the same way.

Docker provides tooling and a platform to manage the lifecycle of your containers:

* Develop your application and its supporting components using containers.
* The container becomes the unit for distributing and testing your application.
* When you’re ready, deploy your application into your production environment, as a container or an orchestrated service. 
  This works the same whether your production environment is a local data center, a cloud provider, or a hybrid of the two.
  

## Docker vs. VM´s
![](containers-vs-virtual-machines.jpg)

[*Source*](https://www.weave.works/blog/a-practical-guide-to-choosing-between-docker-containers-and-vms)

## Docker architecture
![](architecture.svg)

[*Source*](https://docs.docker.com/get-started/overview/)

### Docker daemon
The Docker daemon (```dockerd```) listens for Docker API requests and manages Docker objects such as images, containers,
networks, and volumnes. A daemon can also communicate with other daemons to manage Docker services. 

### Docker client
The Docker client (```docker```) is the primary way that many Docker users interact with Docker. When you use commands
such as ```docker run```, the client sends these commands to ```dockerd``` which carries them out. The ```docker``` 
command uses the Docker API. The Docker client can communicate with more than one daemon. 

### Docker Desktop
Docker Desktop is an easy-to-install application for your Mac or Windows environment that enables you to build and share
containerized applications and microservices. Docker Desktop includes the Docker daemon, the Docker client, Docker Compose,
Docker Content Trust, Kubernetes and Credential Helper. 

### Docker registries
A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to
look for images on Docker Hub by default. You can even run your own private registry. 
When you use the ```docker pull``` or ```docker run``` commands, the required images are pulled from your configured
registry. When you use the ```docker push``` command, your image is pushed to your configured registry.

### Image
An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another 
image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but 
installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images, or you might only use those created by others and published in a registry. To build 
your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. 
Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, 
only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, 
when compared to other virtualization technologies.

### Container
A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the 
Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new 
image based on its current state.
By default, a container is relatively well isolated from other containers and its host machine. You can control how 
isolated a container’s network, storage, or other underlying subsystems are from other containers or from the host machine.
A container is defined by its image as well as any configuration options you provide to it when you create or start it. 
When a container is removed, any changes to its state that are not stored in persistent storage disappear.

---

# Installation

## Windows
Docker for Windows can be installed [here](https://docs.docker.com/desktop/windows/install/)

## Mac
Docker for Mac can be installed [here](https://docs.docker.com/desktop/mac/install/)

## Linux
Docker for Linux can be installed [here](https://docs.docker.com/engine/install/)

---

# Dockerfiles

[Full documentation Dockerfile](https://docs.docker.com/engine/reference/builder/)

Docker can build images automatically by reading the instructions from a Dockerfile. 
A Dockerfile is a text document that contains all the commands a user could call on the command line to 
assemble an image. Using docker build users can create an automated build that executes several command-line 
instructions in succession.

Please check that the file Dockerfile has no file extension like .txt. Some editors may append this file extension
automatically and this would result in an error in the next step.

*Example*
````
FROM ubuntu:16.04

RUN apt-get update && apt-get install nginx -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
````

**Build the Dockerfile**
```
docker build -t <imagename> <path_to_dockerfile>

Example:
docker build -f myubuntu .
```

*Note*
Maybe you have to run the command with ```sudo```. Please make sure you run the command in the same directory where
your Dockerfile is stored.

**Run the Docker container**
```
docker run -itd --name <containername> <imagename>

Example:
docker run -itd --name myubuntu myubuntu
```

**Exec the container**
```
docker exec -it <containername> bash

Example
docker exec -it myubuntu bash
```

Afterwards you can exit the container with the ```exit``` command

---

# Docker Images and Containers
##Images


[Full documentation Docker image](https://docs.docker.com/engine/reference/commandline/image/)

**Search images on Docker hub**
```
docker search <image>
docker search <image:version>
```

**Search only for official images**
```
docker search --filter is-official=true <image>
```

**Display non-truncated description**
```
docker search --no-trunc <image>
```

**Search only for images with at least 3 stars**
```
docker search --filter stars=3 <image>
```

**Search ony official images with at least 3 stars**
```
docker search --filter is-official=true --filter stars=3 <image>
```

**Format the output**
```
docker search --format "{{.Name}}: {{.StarCount}}" <image>
```

---

**List all local images**
```
docker image ls
docker images
```

**List only specific images**
```
docker images <image>
docker images <image:version>
```

**List images with no truncation**
```
docker images --no-trunc <image>
```
---

**Pull an image**
```
docker image pull <image>
```

**Pull an image with all tags**
```
docker image pull --all-tags <image>
```

**Push an image to registry**
```
docker image tag <image:version> <repo_url:yourtag>
docker image push <image>


Example:
docker image tag rhel-httpd:latest registry-host:5000/myadmin/rhel-httpd:latest
docker image push registry-host:5000/myadmin/rhel-httpd:latest
```

If you do not specify a version the latest will be used. 

*Note:*

To push an image you have to create a free account first. 
You can create your account [here](https://hub.docker.com)

**Login to registry**
```
docker login
```

**Login to a self-hosted registry**
```
docker login <registry_url>
```

**Login to registry and provide a password using STDIN**
```
cat ~/<my_password.txt> | docker login --username <username> --password-stdin
```

**Logout from a registry**
```
docker logout
```

---

**Inspect images**
```
docker image inspect <image>

Example:
docker image inspect ubuntu
docker image inspect ubuntu:latest
```

**Inspect image and write output to file**
```
docker image inspect --format "{{json.Config}}" <image> > <file>

Example:
docker image inspect --format "{{json.Config}}" ubuntu > myfile.txt
```

**See image history**
```
docker image history <image>

Example:
docker image history ubuntu
```

---

**Remove an image**
```
docker image rm <image>

Example:
docker image remove nginx
docker image remove nginx:1-alpine-perl
```

**Remove image with specific image id**
```
docker image rmi <id>
```

*Note*
If you have several images with the same id you have to use:
```
docker image rmi <id> --force
```

---

## Containers
* Running instance of a Docker image
* Provides similar isolation to VM´s but lighter
* Adds writable layers on top of image layers and works on it
* Can talk to other containers like processes in Linux
* Use Copy-on-Write

[Full documentation docker container](https://docs.docker.com/engine/reference/commandline/container/)

**Create a new container**
```
docker container create -it --name <container> <image>

Example:
docker container create -it --name cc_busybox-A busybox:latest
```

*Note* This only creates a container, it is not running yet.

**List containers (shows exited/created/running)**
```
docker container ps -a
```

**Create and run a container**
```
docker container run --itd --name <container> <image>

Example:
docker container run --itd --name mynginx nginix:latest
```

**Start container**
```
docker container start <container>
```

**Stop container**
```
docker container stop <container>
```

**Restart container wiht 5sec buffer**
```
docker container restart --time 5 <container>
```

**Rename a container**
```
docker container rename <oldcontainer> <newcontainer>

Example:
docker container rename cc_busybox-A new_busybox-A
```
---

**Run a container in attached mode**

```
docker container attach <container>
```

Run ```exit``` to exit the container (will be stopped automatically)

**Exec the container and run terminal command**
```
docker exec -it <container> ls -l
```

**Run container and map hostport to containerport**
```
docker container run -itd --name <container> -p <hostport>:<containerport> <imagename>

Example:
docker container run -itd --name mynginx -p 8080:80 nginx:latest
```

**Show port mapping**
```
docker container port <container>
```

---

**Remove a container**
```
docker container rm <container>
docker container rm <name1> <name2> <name3>
```

**Remove a running container**
```
docker container rm <container> --force
```

**Kill a running container**
```
docker container kill --signal=SIGTERM <container>
```

**Remove all stopped containers**
```
docker container prune
```

---

# Docker Networks: Connecting Containers

[Full documentation docker network](https://docs.docker.com/engine/reference/commandline/network/)

* Piece of software that enables networking of containers
* Responsible for invoking a network inside the host or within the cluster
* Native n/w drivers are shipped with Docker Engine
* Remote n/w drivers are created and managed by 3th party vendors or community
* IP Address Management Drivers provide default subnets if not specified by admin

![](docker_network.png)

**Create a network bridge**
```
docker network create --driver bridge <bridge>

Example:
docker network create --driver bridge --subnet=192.168.0.0/16 --ip-range=192.168.5.0/24 mybridge
```

**List all networks**
```
docker network ls
```

**Filter networks by name**
```
docker network ls --filter driver=bridge
```
---

**Connect a container to a network**
```
docker network connect <network> <container>

Example:
docker network connect mynetwork my_ubuntu
```

After that run: `````docker container inspect my_ubuntu````` and check the network section
if the container was successfully connected

**Inspect a network object**
```
docker network inspect <network>

Example:
docker network inspect my_bridge
```

**Inspect a network object with format parameter**
```
docker network inspect --format "{{.ID}}:{{.Name}}" <network>
```
# Docker Volume

[Full documentation docker volume](https://docs.docker.com/storage/)

**Create a new volume**
```
docker volumen create <volume>
```

**Mount a volume into a container**
```
docker run -d --volume <volume>:/<volumename> <container>

Example
docker run -d --volume myvolume:/mytmp myubuntu
```

**List all volumes**
```
docker volume ls
```

**List all volumes which are not mounted to any container**
```
docker volume ls --filter "dangling=True"
```

**Inspect a volume**
```
docker volume inspect <volume>
```

**Remove a volume**
```
docker volume rm <volume>
```
---

# Docker Compose

[Full documentation Docker Compose](https://docs.docker.com/compose/)

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to 
configure your application’s services. Then, with a single command, you create and start all the services 
from your configuration.

Using Compose is basically a three-step process:
- Define your app´s environment with a Dockerfile so it can be reproduced anywhere
- Define the services that make up your app in docker-compose.yml so they can be run together in an isolated environemnt.
- Run ````docker compose up```` and the Docker compose command starts and runs your entire app.

**MySQL & Wordpress example**
```yaml
version: '3.3'

services:
  db:
    platform: linux/amd64
    image: mysql:latest
    container_name: mysql_database

    volumes:
      - db_data:/var/lib/mysql

    restart: always

    environment:
      MYSQL_ROOT_PASSWORD: word@press
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: abc@123

  wordpress:
    depends_on:
      - db

    image: wordpress:latest
    container_name: wd_frontend

    volumes:
      - wordpress_files:/var/www/html

    ports:
      - "8000:80"

    restart: always

    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: abc@123

volumes:
  wordpress_files:
  db_data:
```

**Build, (re)create, start and attach the containers for a service**
```
docker compose up
```

**Check if both containers are running**
```
docker ps -a
```

You can now access wordpress on ````localhost:8000```` in your browser and complete the installation.

**Show docker-compose.yaml file**
```
docker-compose config
```

**Show parts of the docker-compose.yaml, in this case services**
```
docker-compose config --services
```

**List all images used to create containers for services**
```
docker-compose images
```

**Fetch logoutput from the service**
```
docker-compose logs
```

**Show only last 10 log events**
```
docker-compose logs --tails=10
```

**List all running processes inside the containers**
```
docker-compose top
```

**Stop all containers within a service**
```
docker-compose down
```
