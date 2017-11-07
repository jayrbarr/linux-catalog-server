## Linux Catalog Server

# IP Address
```
52.15.59.173
```

# Steps Taken
- Created instance of Amazon Lightsail - OS only, Ubuntu
- On AWS, created public static IP address and attached instance.
- Connected to instance via SSH.
- Updated all packages.
```
sudo apt-get update
sudo apt-get upgrade (y to any prompts)
```
- 3 packages not updated, so install aptitude and re-try.
```
sudo apt-get install aptitude
sudo aptitude update
sudo aptitude safe-upgrade (y to any prompts)
```
- Rebooted server instance (due to "*** System restart required *** message).
- Reconnected to instance via SSH.

- On Amazon Lightsail Networking tab, created two TCP ports at 2200 and 123.
- On Linux server, set ufw configuration.
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow www
sudo ufw allow ntp
sudo ufw allow ssh
sudo ufw allow 2200
sudo ufw enable
```
- Open sshd_config and change defaults.
```
sudo nano /etc/ssh/sshd_config
```
   - Change port from 22 to 2200.
   - PermitRootLogin no
   - PermitEmptyPasswords no (should be default)
   - PasswordAuthentication no (should be default)
```
sudo service sshd restart
sudo ufw status numbered
sudo ufw delete 3 (delete the rule that allows 22 / y at prompt)
sudo ufw status numbered
sudo ufw delete 6 (delete the rule that allows 22 (v6) / y at prompt)
```
   
- On Amazon Lightsail networking tab:
   - Click "Edit rules"
   - Delete port 22
   - Click "Save"
   
- From AWS account page, download private key for default pair (used for this server instance)
- Once downloaded, I saved in convenient location for terminal/bash use, and renamed file to just "lightsail".
- From terminal (AWS "Connect using SSH" button will no longer work):
```
ssh ubuntu@52.15.59.173 -p 2200 =i path/to/private/key/lightsail (yes to prompt about connecting/fingerprint)
```
- Once connected, update timezone:
```
sudo timedatectl set-timezone America/New_York
```
- Also install and configure NTP for time sync:
```
sudo apt-get install ntp (y to prompt)
sudo service ntp restart
```
- Add user: grader
```
sudo adduser grader
Enter new UNIX password: (type password - it won't appear)
Retype new UNIX password: (type the same password again - it also won't appear)
Changing the user information for grader
Enter the new value, or press ENTER for the default
(Complete user information as you wish - y at the final prompt)
```
- Give grader sudo access:
```
sudo nano /etc/sudoers.d/grader
```
- Add this line to new file and save:
```
grader ALL=(ALL) NOPASSWD:ALL
```
- Add authorized key for grader. In terminal on local computer, enter:
```
ssh-keygen
```
Follow prompts to create keypair for grader.
Open public key and copy text to clipboard.
Back on Lightsail server,
```
sudo su - grader
mkdir .ssh
nano .ssh/authorized_keys
```
Paste public key into new file and save.
Change folder and file permissions for public keys.
```
chmod 700 .ssh
chmod 644 .ssh/authorized_keys
```
Install Apache.
```
sudo apt-get install apache2
```
Install mod-wsgi and change Apache config file.
```
sudo apt-get install libapache2-mod-wsgi
sudo nano /etc/apache2/sites-enabled/000-default.conf
```
Add this line right before *</VirtualHpst>* tag:
```
WSGIScriptAlias / /var/www/html/myapp.wsgi
```
