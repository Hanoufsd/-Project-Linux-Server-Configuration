# Linux Server Configuration

-   URL is : http://ec2-18-197-39-159.eu-central-1.compute.amazonaws.com/
-   IP address: http://18.197.39.159
-   SSH port- 2200
-   Login : ssh -i ~/.ssh/grader_key  grader@18.197.39.159 -p 2200

# Installed Packages

> finger git apache2 ntp libapache2-mod-wsgi python-setuptools
> sqlalchemy flask postgresql oauth2 Glances

 **1. Generate public Key for udacity (grader_key)**
    - cd  ~/.ssh
    - ssh-keygen -t rsa
    - cat ~/.ssh/grader_key.pub
    - sudo chmod 600 ~/.ssh/grader_key
    - sudo chmod 755 ~/.ssh

**2. Add user grader**
    - sudo adduser grader  , password; 12345
    - sudo nano /etc/sudoers.d/grader and  Add *grader ALL=(ALL:ALL) ALL*
    - installing finger; apt-get install finger  , finger grader

**3. SSH configuration file**
    - nano /etc/ssh/sshd_config
       - Change Port to Port 2200.
       - Change PermitRootLogin no
       - Change PasswordAuthentication no
       - add UseDNS no and AllowUsers grader. 
       -  sudo service ssh restart

**4.  Update all installed packages, and set the cron to have this process automatically done **
    - sudo apt-get update
    - sudo apt-get upgrade
    - sudo apt-get install unattended-upgrades
    - sudo dpkg-reconfigure --priority=low unattended-upgrades
   
**5.  Configure local timezone to UTC**
    - sudo dpkg-reconfigure tzdata and select  UTC
    - sudo apt-get install ntp

**6.Configure Firewall (UFW) to (SSH 2200, HTTP 80, and NTP 123)**
    - sudo ufw allow 2200/tcp
    - sudo ufw allow 80/tcp
    - sudo ufw allow 123/udp
    - sudo ufw enable
    - sudo ufw default deny incoming
    - sudo ufw default allow outgoing

**7. Install Apache, mod_wsgi and Git** 
    - sudo apt-get install apache2
    - sudo apt-get install libapache2-mod-wsgi python-dev
    - sudo a2enmod wsgi
    - sudo service apache2 start
    - sudo apt-get install git
    - cd /var/www
    - sudo mkdir catalog
    -  cd /var/www/catalog
    - sudo mkdir catalog
    - git clone 
    - Rename app  to  __init__.py
    - Update the  path of  client_secrets.json
    - sudo apt-get install python-pip
    - sudo pip install virtualenv 
    - sudo virtualenv venv 
    - source venv/bin/activate 
    - sudo apt-get install python-psycopg2`
    - create the .wsgi app

**8.  Create and configure the .wsgi File**
    - sudo nano /var/www/catalog/catalog.wsgi
>   import sys
>     import logging
>     logging.basicConfig(stream=sys.stderr)
>     sys.path.insert(0, "/var/www/catalog/")
>     from catalog import app as application
>     application.secret_key = 'secret'
    
**9. install PostgreSQL**
    - sudo apt-get install postgresql postgresql-contrib
    - sudo su - postgres  psql
    - Create user catalog with password 'catalog'
    - Create database catalog with owner catalog
    - Update create_engine('postgresql://catalog:catalog@localhost/catalog')

**10. Restart Apache to launch the program**
    - sudo service apache2 restart


# Useful resources
 1. [link1](https://libraries.io/github/golgtwins/Udacity-P7-Linux-Server-Configuration)
 2. [link2](https://github.com/bencam/linux-server-configuration)

