## Linux Server Project - README

### [Project instructions](Project_Details.md)  
### [CatalogApp instructions](README_catalogapp.md)

## Detailed Setup
Software Summary
- **Amazon Lightsail Ubuntu**
- Python 3.6
- Git and Git Bash
- Postgresql
- Apache2
- Flask

### Server Details
STATIC IP: 18.196.119.141  
SSH PORT: 2200

### Update and Upgrade packages
`sudo apt-get update      
sudo apt-get upgrade`

### Create new user `grader` and give `sudo` rights
`sudo adduser grader`    #password is udacity    
`sudo usermod -aG sudo grader`  
Setup SSH for "grader"   
(*source: [Digital  Ocean](https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server)*)
```cd .ssh
ssh-keygen -f ~/.ssh/udacity_key.rsa
 ```

### Change the SSH port from 22 to 2200 and configure SSH access to now allow root login
Configure settings on lightsail page (Manage > Network to allow for port 2200)  
`sudo nano /etc/ssh/sshd_config` to change port to 2200  
Change `PermitRootLogin` to  no 

### Login as `grader`
`ssh -i ~/.ssh/<udacity_key.rsa> grader@18.196.119.141 -p 2200`

### Configure Uncomplicated Firewall (UFW)
```sudo ufw default deny incoming  
sudo uw default allow outgoing
sudo ufw allow 2200/tcp  
sudo ufw allow www  
sudo ufw allow ntp/123  
sudo ufw enable  
sudo ufw status  
```

### Prepare to deploy the Project
Thanks to Udacity Forums and [Digital Ocean - Deploying Flask on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
1. Install Apache:
`sudo apt-get install apache2`
2. Install the libapache2-mod-wsgi package:
`sudo apt-get install libapache2-mod-wsgi`
3. Install the package requirements for Python
```sudo apt-get install python-psycopg2 python-flask
sudo apt-get install python-sqlalchemy python-pip
sudo pip install oauth2client
sudo pip install requests
sudo pip install httplib2
```
4. Install Git and clone repository
```sudo apt-get install git
cd /var/www
mkdir CatalogApp; cd CatalogApp
sudo git clone https://github.com/perkygva/CatalogApp.git CatalogApp
sudo pip install -r requirements.txt
```

5. Configure Apache and virtual VirtualHost
Setup and run virtual environment
```cd /var/www/CatalogApp/CatalogApp  
sudo pip install virtualenv  
sudo virtualenv venv  
source venv/bin/activate  
sudo a2ensite CatalogApp  
sudo service apache2 restart
```

6. Create CatalogApp/conf `sudo nano /etc/apache2/sites-available/CatalogApp.conf`
```
<VirtualHost *:80>
    ServerName 18.196.119.141
    ServerAlias udacity
    ServerAdmin allan.perk@gmail.com
    WSGIScriptAlias / /var/www/CatalogApp/catalog.wsgi
    <Directory /var/www/CatalogApp/CatalogApp/>
        Order allow,deny
        Allow from all
    </Directory>
    Alias /static /var/www/CatalogApp/CatalogApp/static
    <Directory /var/www/CatalogApp/CatalogApp/static/>
        Order allow,deny
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
7. Create `catalogapp.wsgi`
```
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/CatalogApp/")

from CatalogApp import app as application
application.secret_key = 'JTIX4yDFMM0YDsdSO9iZ6_kp'
```
8. Overwrite (hardcode) the CLIENT_ID and CLIENT_SECRET in the `__init__.py` file due to json error.

### Run application
Open a browser and put in your public ip-address as url, e.g. 18.196.119.141 - if everything works, the application should come up! (**Tested and functional on my end!**)

### List of References
- Digital Ocean tutorials (*SSH Configuration; Deploying Flask*)
- StackOverflow
- StackExchange
- Udacity Forums
