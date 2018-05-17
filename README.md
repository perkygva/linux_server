## README

### [Project instructions](Project_Details.md)

## Detailed Setup
**Create your server | ubuntu instance - Amazon Lightsail or alternative**

### Server Details
STATIC IP: 18.194.122.102
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
```mkdir /home
ssh-keygen
 ```
Download the private key and save it in .ssh folder
```mv ~/Downloads/<NAME> ~/.ssh
chmod 600 ~/.ssh/udacity_privatekey.pem
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
sudo pip install flask-seasurf
```
4. Install Git and clone repository
```sudo apt-get install git
cd /catalog
sudo git clone https://github.com/perkygva/CatalogApp.git
```

### Run application
Restart Apache:
`sudo service apache2 restart`
Open a browser and put in your public ip-address as url, e.g. 18.184.187.88 - if everything works, the application should come up
Source: A2 Hosting
View the last 20 lines in the error log: $ sudo tail -20 /var/log/apache2/error.log
