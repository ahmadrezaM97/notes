# Start VPS
Created: 2022-06-03 19:50
Tags: 
____

```bash

apt update

### **Create a new user**
adduser ahmadreza
usermod -aG sudo ahmadreza

## Setting Up a Basic Firewall

ufw app list
ufw allow OpenSSH
ufw enable
ufw status


### **Change the default SSH port**
ssh-copy-id -i ~/.ssh/id_ed25519.pub  ahmadreza@91.198.77.48

vim /etc/ssh/ssh_config

ChallengeResponseAuthentication no
PasswordAuthentication no
UsePAM no
PermitRootLogin no
PermitRootLogin prohibit-password
port 1997


sudo systemctl reload ssh


  
```

[IMPORTANT](https://www.cyberciti.biz/faq/how-to-disable-ssh-password-login-on-linux/)
[telegram mtrpoxy](https://hub.docker.com/r/telegrammessenger/proxy/)

_____


##### References
1.[digitalocean](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04)

