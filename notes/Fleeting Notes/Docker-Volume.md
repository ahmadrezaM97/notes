# Docker-Volume
Created: 2022-04-11 23:30
Tags: 
____
![[types-of-mounts-volume.png]]

Docker is the preferred mechanism for persisting data generated by and used by Docker containers.


#### docker-volume vs mount

volumes are completely managed by Docker.
Bind-mounts are dependent on the directory structure and OS of host machine


You can create a volume explicitly using the 

> docker volume create

You can remove unused volumes using

>docker volume prune

when you mount a volume, it may be named or anonymous


> volume remove when you explicitly remove them.

#### Good use cases for volumes
1. Sharing data among multiple running containers
	1. multiple container can mount the same volume simultaneously 
2. When the Docker host is not guaranteed yo have a given directory or file structure.
	1. Volumes help you to decouple container from docker host file configs.
3.  When you want to store your container's data on a remove host or a cloud provider, rather than locally
4. When you need backup
	1. /var/lib/docker/volumes/<volume-name>
5. better performance for Docker Desktop
6.  When you application requires fully native file system behavior on Docker Desktop

#### Volumes commands

> docker volume create my-vol
> docker volume ls
> docker volume inspect my-vol
> docker volume rm my-vol


#### Start a container with a volume 

``` bash
docker run -d --name devtest --mount source=myvol2,target=/app ngnix:lastest

```

* -d / --detach ->  run container in background and print container ID*
* --name  -> assign a name to the container *


#### use a volume with docker-compose

``` yaml
version: "3.9"
services:
	app:
		image: node:lts
	volumes:
		- myapp:/home/node/app

volumes:
	myapp:
``‍`


docker-compose will reuse the volume

[docker](https://docs.docker.com/storage/)


