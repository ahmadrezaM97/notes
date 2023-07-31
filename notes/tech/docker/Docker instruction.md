# Docker instruction
Created: 2022-04-13 22:10
Tags: 
____
Docker -t/--tag -> Name and optionally a tag in the 'name:tag' format


##### CMD vs ENTRYPOINT vs RUN
1. Dockerfile should specify at least on of CMD or ENTRYPOINT commands.
2. ENTRYPOINT should be defined when using the container as an executable
3. CMD should be used as a way of defining __default arguments for an ENTRYPOINT___ command or for execuring an ad-hoc command in a container
4. CMD will be overridden when running the containter with alternative arguments.

![[docker-cmd-vs-entrypoint.png]]
##### ENTRYPOINT

The exec form

> ENTRYPOINT [ "executable", "param1", "param2" ]

The shell form

> ENTRYPOINT command param1 param2


_____
##### References
1. https://docs.docker.com/engine/reference/builder/#entrypoint

