## Linux Catalog Server

# IP Address
52.15.59.173

# Steps Taken
- Created instance of Amazon Lightsail - OS only, Ubuntu
- Updated all packages.
- Added user 'grader'.
- On Amazon Lightsail Networking tab, created two TCP ports at 2200 and 123.
- On Linux server, deny all incoming, allow all outgoing, allow ssh, allow www, allow ntp, allow 2200/tcp.
- In sshd_config, change port from 22 to 2200.
- On Amazon Lightsail, delete port 22.
- On Linux server, enable ufw. Restart ssh service.
- Downloaded Putty. Configured server and key for login to SSH and port 2200.
