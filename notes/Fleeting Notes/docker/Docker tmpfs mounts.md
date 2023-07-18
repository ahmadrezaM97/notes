# Docker tmpfs mounts
Created: 2022-04-11 23:39
Tags: 
____

if you'r running Docker on Linux. you have a third option over [[Docker-Volume]] and [[Docker bind-mount]]: tmpfs

As opposed to volumes and bind mounts, a tmpfs mount is temporary, and only presisten in the host memory
when container stops, the tmpfs is removed.




1.

