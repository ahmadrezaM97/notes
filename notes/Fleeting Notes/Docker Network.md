# Docker Network
Created: 2022-04-12 20:57
Tags: 
____

##### Expose ans -p

we have three options
1. Neither specify EXPOSE nor -p
	2. the service will only accessible from inside the container itself.
2. Only specify EXPOSE
	1. the service will NOT accessible OUT side the docker
	2. the service accessible inside DOCKER CONTAINERS
	3. It's suitable for inter-container communication
4. Specify EXPOSE and -p
	1. is accessible from anywhere, even outside Docker
2. 


_____
##### References
1.[https://docs.docker.com/engine/reference/builder/#expose](docker document)
2. [https://stackoverflow.com/questions/22111060/what-is-the-difference-between-expose-and-publish-in-docker]()


