# secure-linux
Harden Linux Server using ufw, Fail2Ban and configuring ssh



## Install and Configure ufw



Debian based distros

```
sudo apt update ; sudo apt install ufw -y
```


Arch based distros

```
sudo pacman -Syu ufw
```



To check ufw status

```
sudo ufw status
```


add some Rules, here an example

```
sudo ufw limit 22/tcp
sudo ufw allow 443/tcp
sudo ufw default deny incoming
sudo ufw default allow outgoing
```




When finished enable the ufw

```
sudo ufw enable
```




If you want to go further away and deny Ping replys

```
sudo nano /etc/ufw/before.rules
```




```
# ok icmp codes for INPUT
-A ufw-before-input -p icmp --icmp-type echo-request -j DROP
```



## Auto update the system (only Debian)


To let the system install the updates automatically

```
sudo apt install unattended-upgrades
```



## Harden SSH

Here you can modify SSH settings.
```
sudo nano /etc/ssh/sshd_config
```


Normaly I do the following:



* change the defaul ssh port

```
port <Number>
```

Don't forget to reconfigure ufw after changing ssh port



* Only allow IPv4

```
AddressFamily inet
```


* Don't allow root login

```
PermitRootLogin no
```



You can also generate a ssh key which can be a better authentication method than a password.

On the client

```
ssh-keygen -b 4096
```



```
ssh-copy-id <server-username>@<server-ip>
```


Now diable login with password, so that you can login only with the key

```
sudo nano /etc/ssh/sshd_config
```


```
PasswordAuthentication no
```



## Install and configure Fail2Ban


On Debian

```
sudo apt install fail2ban -y
```


On Arch Linux

```
sudo pacman -Syu fail2ban
```


Configuration file examples and defaults are in two main files /etc/fail2ban/fail2ban.conf and /etc/fail2ban/jail.conf



To configure fail2ban

```
sudo nano /etc/fail2ban/jail.local
```


```
[DEFAULT]
ignoreip = 127.0.0.1/8 ::1
bantime = 3600
findtime = 600
maxretry = 5

[sshd]
enabled = true
```


Enable Fail2Ban
```
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```


Noe restart ssh to apply the new configuration

```
sudo systemctl restart sshd.service
```
#gg
