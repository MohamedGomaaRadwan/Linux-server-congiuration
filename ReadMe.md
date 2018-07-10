# Linux Server Configuration Project
  i will take a baseline installation of a Linux server and prepare it
  to host my web applications.
  i will secure my server from a number of attack vectors,
  install and configure a database server,
  and deploy one of my existing web applications onto it.

# Access
  IP Address: 35.178.64.221

  SSH Port: 2200

  URL: http://ec2-35-178-64-221.eu-west-2.compute.amazonaws.com
	
	login : sudo ssh grader@35.178.64.221 -vvv -p 2200 -i [pass to key file]

# Installed Packages
  1-apache2
  2-postgresql
  3-sqlalchemy
  4-flask
  5-libapache2-mod-wsgi
  6-finger
  7-git

# Resources
https://askubuntu.com/questions/896389/how-do-i-install-and-start-apache2
http://flask-ask.readthedocs.io/en/latest/getting_started.html
https://stackoverflow.com/questions/22353512/how-to-install-sqlalchemy-on-ubuntu

# commands which followed

  -Create a new user 'grader' :
  $ sudo adduser grader

  -permission to perform sudo command:
  $ sudo nano /etc/sudoers.d/grader
  $ grader ALL=(ALL) NOPASSWD:ALL

  -Generate a SSH key
  $ ssh-keygen

  -Place the public key on remote server
  $ mkdir .ssh

  -Login to the server using ssh key
  $ ssh grader@35.178.64.221 -p PORT_NUMBER -i ~/.ssh/authorized_keys

  -Update packages
  $ sudo apt-get update
  $ sudo apt-get upgrade

  -Open the config file:
  $ sudo nano /etc/ssh/sshd_config
  then:
    1-change PermitRootLogin to no
    2-Change to Port 2200

  -Block all incomming requests:
  $ sudo ufw default deny incoming

  -Allow all outgoing requests from the server:
  $ sudo ufw default allow outgoing

  -Allow incoming connections for SSH (port 2200):
  $ sudo ufw allow 2200/tcp

  -Allow incoming connections for HTTP (port 80):
  $ sudo ufw allow www

  -Allow incoming connections for NTP (port 123):
  $ sudo ufw allow ntp

  -Configure the local timezone to UTC
  $ sudo dpkg-reconfigure tzdata
  then:
    -Select UTC

  -Install Apache2:
  $ sudo apt-get install apache2

  -Install mod_wsgi for serving Python apps from Apache:
  $ sudo apt-get install python-setuptools libapache2-mod-wsgi

  -Restart the Apache server for mod_wsgi to load:
  $ sudo service apache2 restart

  -Install Git and clone item_catalog app
  -Install and configure PostgreSQL

  -Install PostgreSQL:
  $ sudo apt-get install postgresql postgresql-contrib

  -Check that no remote connections are allowed (default):
  $ sudo vim /etc/postgresql/9.3/main/pg_hba.conf

  -Open the database setup file:
  $ sudo vim database_setup.py

  -Change from sqlite to postgresql:
  python engine = create_engine('postgresql://catalog:PW-FOR-DB@localhost/catalog')

  -Rename application.py:
  $ mv application.py __init__.py

  -Create linux user for psql:
  $ sudo adduser catalog

  -Change to default user postgres:
  $ sudo su - postgre

  -Connect to the system:
  $ psql

  -Create user with LOGIN role and set a password:
  CREATE USER gomaa WITH PASSWORD 'password';

  -Allow the user to create database tables:
  ALTER USER gomaa CREATEDB;

  -Create database:
  CREATE DATABASE data WITH OWNER gomaa;

  -Connect to the database data:
  \c data

  -Revoke all rights:
  REVOKE ALL ON SCHEMA public FROM public;

  -Grant only access to the gomaa role:
  GRANT ALL ON SCHEMA public TO data;

  -Create postgreSQL database schema:
  $ python database_setup.py

  -Run the application

  -Restart Apache2 server
  $ sudo service apache2 restart

  Go to this link: http://ec2-35-178-64-221.eu-west-2.compute.amazonaws.com
