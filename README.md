## README

[Project details](Project_Details.md)


##### SERVER
STATIC IP: 18.194.122.102
SSH PORT: 2200

# Create new user `grader` and give `sudo` rights
`sudo adduser grader #password is udacity
sudo usermod -aG sudo grader`


# Update and Upgrade packages
`sudo apt-get update
sudo apt-get upgrade`

# Setup SSH for "grader"
`mkdir /home`
ssh-keygen
# Download the private key and save it in .ssh folder
`mv ~/Downloads/<NAME> ~/.ssh
chmod 600 ~/.ssh/udacity_privatekey.pem`


# Change the SSH port from 22 to 2200 and configure SSH access
Configure settings on lightsail page (Manage > Network to allow for port 2200)
`sudo nano /etc/ssh/sshd_config` to change port to 2200

# Configure Uncomplicated Firewall (UFW)
`sudo ufw default deny incoming`
`sudo uw default allow outgoing`
`sudo ufw allow 2200/tcp`
`sudo ufw allow www`
`sudo ufw allow ntp`
`sudo ufw enable`
`sudo ufw status`

ip-172-26-13-226 (127.0.0.1)
