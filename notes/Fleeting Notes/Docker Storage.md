# Docker Storage
Created: 2022-04-11 23:44
Tags: 
____

![[types-of-mounts-volume.png]]

Docker has two options for containers to store files on the host machine

- Volume
	- volumes are stored in a part of the host filesystem which is managed by Docker
		- /var/lib/docker/volumes
	- preferred way to persist data
- Bind mount
	- my be stroed anywhete on the host system.


tmpfs
	are store in the system's memeory only

[[Docker-Volume]]
[[Docker bind-mount]]
[[Docker tmpfs mounts]]
_____
##### Refrences
1.

