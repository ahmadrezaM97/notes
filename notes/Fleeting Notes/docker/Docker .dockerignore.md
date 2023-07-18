# Docker .dockerignore
Created: 2022-04-12 21:22
Tags: 
____

learn about the docker build context and how to optimize it ( with .dockerignore file)

##### What is docker build context

the docker build command is used to build a new Docker image.

```bash
docker build . -t my-app-image:lastest
```

There is one argument you can pass to the build command that specifies the __build context__.

In most cases you usually pass the current directory 

The Docker is a client=server application.
It consists of 
- Docker Client
	- The docker client command line tool talks with the docker server ans asks it ti di things
	- One of theses is Docker build - BUILDING A NEW DOCKER IMAGE
- Docker server - (A.K.A  The Docker DAEMON)
	- the docker server can run on the 
		- same machine 
		- remote in the cloud

in order to create a new Docker image, the Docker server needs access to the files

[https://codefresh.io/docker-tutorial/not-ignore-dockerignore-2/]()


_____
##### References
1.

