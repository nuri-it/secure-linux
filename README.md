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

## Auto update the system
To let the system install the updates automatically
```
sudo apt install unattended-upgrades
```

## Harden SSH
```
sudo nano /etc/ssh/sshd_config
```

Here you can modify SSH settings.

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

#gg
