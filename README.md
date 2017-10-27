## Linux Catalog Server

# IP Address
```
52.15.59.173
```

# Steps Taken
- Use or generate keypair associated with Github account. See [Girhub Help](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) for more info.
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
- Created instance of Amazon Lightsail - OS only, Ubuntu
   - Added script for Dokku installation for instance.
   - Uploaded public key for generated keypair and attached to instance creation.
- On AWS, created public static IP address and attached to instance.
- Connected to instance via SSH.
- Updated all packages.
```
sudo apt-get update
```
- Rebooted server instance and reconnected via SSH (due to "*** System restart required *** message).


- On Amazon Lightsail Networking tab, created two TCP ports at 2200 and 123.
- On Linux server, deny all incoming, allow all outgoing, allow ssh, allow www, allow ntp, allow 2200/tcp.
- In sshd_config, change port from 22 to 2200.
- On Amazon Lightsail, delete port 22.
- On Linux server, enable ufw. Restart ssh service.

# Dokku Installation Script (on server instance creation at Amazon Lightsail)
```
wget https://raw.githubusercontent.com/dokku/dokku/v0.10.5/bootstrap.sh;
sudo DOKKU_TAG=v0.10.5 bash bootstrap.sh
```
