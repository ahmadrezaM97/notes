# DockerBestPractice
Created: 2022-04-13 23:26
Tags: 
____

1. Exclude with .dockerignore
2. Use multi-stage builds
	1. order instructions from the less frequently changed to the more frequently change
	2. -> 
		1. Install tools you need to build your application
		2. Install or update library dependencies
		3. Generate your application
	3. DO NOT install unnecessary packages
	4. Decouple applications
		1. Each container should have only one concern
	5. Minimize the number of layers
		1. -> Only the instructions __RUN__, __COPY__ and __ADD__ create layers
			1. Other instructions create temporary intermediate images and don't increate the size of your build
	6. Sort multi-line arguments
	7. ADD vs COPY
		1. generally speaking, Copy is preferred 
			1. is more transparent that ADD -> COPY only supports the basic
		2. If you have multiple files and those changes individually, Copy them separately 
		
_____
##### References
1.[Docker Documents](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

