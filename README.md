## Full Stack Web Developer Nano Degree ##
## _Linux Server Configuration_ Project  ##

### Accessing the Project ####
IP Address :18.216.105.17.xip.io

#### Steps to complete the project ####

### Step 1: Amazon Lightsail Server Set Up ###
1.Create a new AWS account (https://lightsail.aws.amazon.com/ls/webapp)

2.Create instance and set the platform to _OS ONLY_ and blueprint to _Ubuntu_

3.Add the SSH Keys: 123, 2200.

### Step 2: Server Configuration ###
1.Move the downloaded public key file to .ssh folder at the root of Finder `.pem`.

2.To make the public key secure and usable, run ` chmod 600 ~/.ssh/YourAWSKey.pem ` in terminal


3.log to Amazon Lightsail Server using the public key ` ssh -i ~/.ssh/AWSKey.pem ubuntu@[ip] `
4.Switch to user root `sudo su-`.
5.Add grader user `sudo adduser grader`
6.Create new file in _sudoers directory_ and add ALL=(ALL:ALL) ALL
7.update packages and install finger package
- sudo apt-get update
- sudo apt-get upgrade
- sudo apt-get install finger


8.Generate Key ssh-keygen -f ~/.ssh/key.pem
9.read and copy the public key cat ~/.ssh/udacity_key.rsa.pub

10.Go to the terminal window where you are logged into Amazon Lightsail as the root user, move to grader's folder by cd /home/grader.

11.Create a .ssh directory: mkdir .ssh
12.Create a file and store the public key `touch .ssh/authorized_keys`
13.Edit and paste the public key `nano .ssh/authorized_keys`
14.Change the permission: sudo chmod 700 /home/grader/.ssh and $ sudo chmod 644 /home/grader/.ssh/authorized_keys

15.Change the owner from to grader: sudo chown -R grader:grader /home/grader/.ssh and Restart the ssh service: `sudo service ssh restart`
16.Disconnect from Amazon Lightsail server `~.`

17.Log to the server as grader: `ssh -i ~/.ssh/key.rsa grader@[ip]`

18.Enforce the key-based authentication `sudo nano /etc/ssh/sshd_config` and change _PasswordAuthentication_ to no
19.Restart ssh again:  `sudo service ssh restart`
20.Run `sudo nano /etc/ssh/ssdh_config` and change the port 22 to 2200
21.Disconnect $ ~. and log back through port 2200: ` ssh -i ~/.ssh/udacity_key.rsa -p 2200 grader@[ip]`
22.Disable ssh login for root user, ` sudo nano /etc/ssh/sshd_config` and change _PermitRootLogin_ to no
23.Restart ssh `sudo service ssh restart`.
24.configure UFW to satisfy the requirements as following
- sudo ufw allow 2200/tcp
- sudo ufw allow 80/tcp
- sudo ufw allow 123/udp
- sudo ufw enable

### Deploy the application ###
1.ssh to Amazon terminal with grader
2.Install needed packages
- sudo apt-get install apache2
- sudo apt-get install libapache2-mod-wsgi python-dev
- sudo apt-get install git
3.Enable mod_wsgi by `sudo a2enmod wsgi` and start the web server by `sudo service apache2 start` or `sudo service apache2 restart`
4.Set up the folder structure
- cd /var/www
- sudo mkdir catalog
- sudo chown -R grader:grader catalog
- cd catalog
5.Clone the project from Github
- ` git clone [your link] catalog `

6.Create a .wsgi file: `sudo nano catalog.wsgi` and add the following into this file

`
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/")
from catalog import app as application
application.secret_key = 'supersecretkey'
`

7.Rename the application.py to __init__.py
8.Install and start the virtual machine
- `sudo pip install virtualenv`
- `sudo virtualenv venv`
- `source venv/bin/activate`
- `sudo chmod -R 777 venv`

9.Install the Flask and other packages needed
- `sudo apt-get install python-pip`
- `sudo pip install Flask`
-  `sudo pip install httplib2 oauth2client sqlalchemy psycopg2 sqlaclemy_utils requests`

10.Change the _client_secrets.json_ line to `/var/www/catalog/catalog/client_secrets.json`

11.Change the host to your Amazon Lightsail public IP address and port to 80 


### Refernces ###
I really appreciate those students who added the steps of completing this project:

- https://github.com/chuanqin3/udacity-linux-configuration
