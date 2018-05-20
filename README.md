## README

### [Project instructions](Project_Details.md)

## Detailed Setup
**Create your server | ubuntu instance - Amazon Lightsail or alternative**

### Server Details
STATIC IP: 18.196.119.141
SSH PORT: 2200

### Update and Upgrade packages
`sudo apt-get update
sudo apt-get upgrade`


### Change the SSH port from 22 to 2200 and configure SSH access
Configure settings on lightsail page (Manage > Network to allow for port 2200)
`sudo nano /etc/ssh/sshd_config` to change port to 2200

### Configure Uncomplicated Firewall (UFW)
```sudo ufw default deny incoming  
sudo uw default allow outgoing
sudo ufw allow 2200/tcp  
sudo ufw allow www  
sudo ufw allow ntp  
sudo ufw enable  
sudo ufw status  
```

### Create new user `grader` and give `sudo` rights
`sudo adduser grader #password is udacity
sudo usermod -aG sudo grader`
Setup SSH for "grader" (*source: [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server)*)
```cd .ssh
ssh-keygen -f ~/.ssh/udacity_key.rsa
 ```


### Prepare to deploy the Project
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
sudo git clone https://github.com/perkygva/CatalogApp.git
sudo pip install -r requirements.txt
```
5. Install Virutal Environment
Create CatalogApp/conf `sudo nano /etc/apache2/sites-available/CatalogApp.conf`
```
<VirtualHost *:80>
    ServerName 18.196.119.141
    ServerAlias udacity
    ServerAdmin admin@18.196.119.141
    WSGIScriptAlias / /var/www/CatalogApp/CatalogApp/catalog.wsgi
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
Create `catalogapp.wsgi`
```
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/CatalogApp/CatalogApp/")

from catalog import app as application
```

### Run application
Restart Apache:
`sudo service apache2 restart`
Open a browser and put in your public ip-address as url, e.g. 18.196.119.141 - if everything works, the application should come up
