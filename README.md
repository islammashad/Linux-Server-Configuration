# Linux-Server-Configuration
Part of full stack web developer nano degree

- This is the Last Project of My Nano Degree The project Requires Linux Server Configuration and deploying Item catalog Resturant  project and the following steps shows my work for configuring Linux server and deploying project including add software installed and summary to the configurations 

# Part 1 : Linux Security 

## Initial Information 
- IP address : http://52.29.79.147/ 
- Website URL :http://ec2-52-29-79-147.eu-central-1.compute.amazonaws.com/ 

##  user grader 
- Add user name grader : `sudo adduser grader`
- make this file at sidoers.d : `sudo nano /etc/sudoers.d/grader`
- Now giving it sudo by adding `grader ALL=(ALL:ALL) ALL`

## ssh port 
- 2200

## Key For User Grader when log in 
- 01019015320 

## Disable remote login for root 
- open that configration by this command `sudo nano /etc/ssh/sshd_config`
- Find this line `PermitRootLogin prohibit-password StrictModes yes` then change 
   to `PermitRootLogin prohibit-password StrictModes no `
- to make changes `service ssh restart`

## Firewall Configuration 
- for ssh port 2200  `sudo ufw allow 2200/tcp`
- ssh `sudo ufw allow ssh`
- for port 80`sudo ufw allow www`
- for port 123 `sudo ufw allow ntp` 
- enble ufw `sudo ufw enable`
 status  `sudo ufw status`

## Key Based Authentication (ssh key Login )
- first on my local machine  I type `ssh-keygen` require address I give it address and generate two files 
- Back to my server and run the following commands 
- Making Directory to hold keys  `mkdir .ssh`
- create file for authorized keys `touch .ssh/authorized_keys`
- Backing to my local and copy content of  `cat .ssh/linuxCourse.pub`
- on server ` nano .ssh/authorized_keys` and paste the content and save
- change `chmod 700 .ssh`
-	and this `chmod 644 .ssh/authorized_keys`
- Now you can login after changing the port  `ssh -i (MY_PATH_TO_FILE) grader@52.29.79.147 -p 2200 `

## Application Up-to-date
- run this  `sudo apt-get update`
- and this  `sudo apt-get upgrade`

## Non Default Port so I changed  it by 
- Type  `sudo nano /etc/ssh/sshd_config`
- change port `Port 22` to `Port 2200`
- Test changes By login `ssh grader@52.29.79.147 -p 2200`

# Part 2 : Deploying the Project 

## Software Installation

## Apache Installation and Configuration 
- Apche `sudo apt-get install apache2`
- mod_wsgi `sudo apt-get install libapache2-mod-wsgi python-dev`
- For enable mod_wsgi  `sudo a2enmod wsgi`
- begin our apache  `sudo service apache2 start`
## Postgresql Installation and Configuration 
- python libraries `sudo apt-get install libpq-dev python-dev`
- postgresql `sudo apt-get install postgresql postgresql-contrib`
- logign`sudo su - postgres`
- `psql`
- create user `CREATE USER catalog WITH PASSWORD 'password';`
- `ALTER USER catalog CREATEDB;`
- database `CREATE DATABASE catalog WITH OWNER catalog;`
- `\c catalog`
- `REVOKE ALL ON SCHEMA public FROM public;`
- `GRANT ALL ON SCHEMA public TO catalog;`
- `\q`
- `exit`
## Install another software  and Virtual Environment 
- Git `sudo apt-get install git`
- pip `sudo apt-get install python-pip`
- flask `pip install Flask` 
- virtual environment `sudo pip install virtualenv`
- start new one `sudo virtualenv venv`
- environment Activation `source venv/bin/activate`

## Getting Project from Github and making some changes 
- go to this folder `cd /var/www`
- Making New Directory for project  `sudo mkdir catalog`
- clone my project from github ` https://github.com/islammashad/Item-catalog-resturant.git catalog` 
- I made file `catalog.wsgi` and configure it with the path and secret key you can see by ` cat /var/www/catalog/catalog.wsgi`
- I made the ` __init__.py` that have the content of `project.py` 
- Also Updating my `client secret` path in `__init__.py `
- making some configuration on my `virtual host` for my IP and URL 
- Type `sudo nano /etc/apache2/sites-available/catalog.conf`
- after restarting `apache` and checK IP it works 

## Notes 
- when testing my IP the Google sign In doesn't work so I went `http://console.developers.google.com/` 
- add my IP and URL and It's work 
- with this project I uploaded pair of files used to make `key based authentication `
