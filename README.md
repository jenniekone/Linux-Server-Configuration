Udacity - Linux Server Configuration Project
- Udacity FullStack Web Developer - Nanodegree.


. Technical Information About the Project
Server IP Address: 172-26-13-147
SSH server access port: 2200
SSH login username: grader
Application URL: http://3.16.67.23.xip.io


. Get started on Lightsail
We're recommending Amazon Lightsail for this project. If you prefer, you can use any other service that gives you a publicly accessible Ubuntu Linux server. But Lightsail works pretty well and it's what we've tested.

There are a few things you need to do when you create your server instance.

1. Log in!
First, log in to Lightsail. If you don't already have an Amazon Web Services account, you'll be prompted to create one.

Amazon Web Services login page
Amazon Web Services login page.

2. Create an instance.
Once you're logged in, Lightsail will give you a friendly message with a robot on it, prompting you to create an instance. A Lightsail instance is a Linux server running on a virtual machine inside an Amazon datacenter.

Lightsail screenshot: No instances
When you have no instances, Lightsail gives you a picture of an orange robot and suggests that you create an instance.

3. Choose an instance image: Ubuntu
Lightsail supports a lot of different instance types. An instance image is a particular software setup, including an operating system and optionally built-in applications.

For this project, you'll want a plain Ubuntu Linux image. There are two settings to make here. First, choose "OS Only" (rather than "Apps + OS"). Second, choose Ubuntu as the operating system.

Lightsail setup: Pick your instance image (Ubuntu)
When you create an instance, Lightsail asks what kind you want.
For this project, choose an "OS Only" instance with Ubuntu.

4. Choose your instance plan.
The instance plan controls how powerful of a server you get. It also controls how much money they want to charge you. For this project, the lowest tier of instance is just fine. And as long as you complete the project within a month and shut your instance down, the price will be zero.

Lightsail setup: Pricing options. (Choose $5/month with first month free.)
Lightsail's options for instance pricing.
For this project, pick the lowest one to get free-tier access.

Be aware: If you enable additional features in Lightsail, you may be charged extra for them.

5. Give your instance a hostname.
Every instance needs a unique hostname. You can use any name you like, as long as it doesn't have spaces or unusual characters in it. Your instance's name will be visible to you and to the project reviewer.

Lightsail setup: Setting a name for your instance.
I've named my instance silly-name-here.

6. Wait for it to start up.
It may take a few minutes for your instance to start up.

Lightsail: Instance starting.
While your instance is starting up, Lightsail shows you a grayed-out display.

Lightsail: Instance running.
Once your instance is running, the display gets brighter.

7. It's running; let's use it!
Once your instance has started up, you can log into it with SSH from your browser.

The public IP address of the instance is displayed along with its name. In the above picture it's 54.84.49.254.

Note: When you set up OAuth for your application, you will need a DNS name that refers to your instance's IP address. You can use the xip.io service to get one; this is a public service offered for free by Basecamp. For instance, the DNS name 54.84.49.254.xip.io refers to the server above.

Lightsail: Main page for an instance.
The main page for my silly-name-here instance.
The big orange "Connect using SSH" button is the next step.

Explore the other tabs of this user interface to find the Lightsail firewall and other settings. You'll need to configure the Lightsail firewall as one step of the project.

When you SSH in, you'll be logged as the ubuntu user. When you want to execute commands as root, you'll need to use the sudo command to do it.

Lightsail: SSH window.
An SSH window logged into the server instance.
From here, it's just like any other Linux server.

8. Project time.
Now that you have a working instance, you can get right into the project!

------- ------  -------   --------
------- ------  -------   --------
- Through ANOTHER MACHINE*

Creating the RSA Key Pair
On your local machine, you will first have to set up the public and private key pair. This key pair will be used to authenticate yourself while securely logging in to the server via SSH. The private key will be kept with you in your local machine, and the public key will be stored in the server.

To generate a key pair, run the following command:

$ ssh-keygen
When it asks to enter a passphrase, you may either leave it empty or enter some passphrase. A passphrase adds an additional layer of security to prevent unauthorized users from logging in.

The whole process would look like this:

Generating public/private rsa key pair.
Enter file in which to save the key (/home/jennie/.ssh/id_rsa): /home/jennie/.ssh/udacity_project
Created directory '/home/jennie/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/jennie/.ssh/udacity_project.
Your public key has been saved in /home/jennie/.ssh/udacity_project.pub.
The key fingerprint is:
SHA256:Swe2345vfpIZqstirqadAqVmGSF/tGhtDfXAfL2fs/U8 jennie@jennie-VirtualBox
The key's randomart image is:
+---[RSA 2048]----+
|                 |
|       . . .     |
|      o + o +    |
| .   o o = o =   |
|+.o o o S . = .  |
|+o+. =.= .   . . |
|o= o.oB.=       E|
|+   *=.= +     ..|
|   .oo.o+ .     o|
+----[SHA256]-----+

Save your ssh keygen !!!

------- ------  -------   --------
------- ------  -------   --------

- GET BACK TO YOUR AWS LIGHTSAIL MACHINE
- START WORKING:

. Updating the System
Run the following command to update the virtual server:

 # apt update && apt upgrade
This will update all the packages. If the available update is a kernel update, you might need to reboot the server by running the following command:

# reboot
. Changing the SSH Port from 22 to 2200
Open the /etc/ssh/sshd_config file with nano or any other text editor of your choice:

# nano /etc/ssh/sshd_config
Find the line #Port 22  and ADD  Port 2200 BELOW IT. Then, save the file.
Note: **DO NOT MODIFY PORT 22 OR ERASE IT, OR, AWS LIGHTSAIL SERVER WILL LOCK YOU OUT !**

Restart the SSH server to reflect those changes:

# service ssh restart
To confirm whether the changes have come into effect or not, run:

# exit
This will take you back to your host machine. After you are back to your local machine, run:

$ ssh root@172-26-13-147 -p 2200
You should now be able to log in to the server as root on port 2200. The -p option explicitly tells at what port the SSH server operates on. It now no more operates on port number 22.

. Configure Timezone to Use UTC
To configure the timezone to use UTC, run the following command:

# sudo dpkg-reconfigure tzdata
It then shows you a list. Choose None of the Above and press enter. In the next step, choose UTC and press enter.

You should now see an output like this:

Current default time zone: 'Etc/UTC'
Local time is now:      Thu May 24 11:04:59 UTC 2018.
Universal Time is now:  Thu May 24 11:04:59 UTC 2018.


. Setting Up the Firewall
Now we would configure the firewall to allow only incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123):

# ufw allow 2200/tcp
# ufw allow 80/tcp
# ufw allow 123/udp
To enable the above firewall rules, run:

# ufw enable
To confirm whether the above rules have been successfully applied or not, run:

# ufw status
You should see something like this:

Status: active

To                         Action      From
--                         ------      ----
2200/tcp                   ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
123/udp                    ALLOW       Anywhere
2200/tcp (v6)              ALLOW       Anywhere (v6)
80/tcp (v6)                ALLOW       Anywhere (v6)
123/udp (v6)               ALLOW       Anywhere (v6)



. Creating the User grader and Adding it to the sudo Group
. Creating the User grader
While being logged into the virtual server, run the following command and proceed:

  # adduser grader
The output would look like this:

  Adding user `grader' ...
  Adding new group `grader' (1000) ...
  Adding new user `grader' (1000) with group `grader' ...
  Creating home directory `/home/grader' ...
  Copying files from `/etc/skel' ...
  Enter new UNIX password:
  Retype new UNIX password:
  passwd: password updated successfully
  Changing the user information for grader
  Enter the new value, or press ENTER for the default
	  Full Name []: Grader
	  Room Number []:
	  Work Phone []:
	  Home Phone []:
	  Other []:
  Is the information correct? [Y/n]
Note: Above, the UNIX password I have entered for the user grader is, root.



. Adding grader to the Group sudo
Run the following command to add the user grader to the sudo group to grant it administrative access:

  # usermod -aG sudo grader
. Adding SSH Access to the user grader
To allow SSH access to the user grader, first log into the account of the user grader from your virtual server:

# su - grader
You should see a prompt like this:

grader@ubuntu-s-1vcpu-1gb-sgp1-01:~$
Now enter the following commands to allow SSH access to the user grader:

$ mkdir .ssh
$ chmod 700 .ssh
$ cd .ssh/
$ touch authorized_keys
$ chmod 644 authorized_keys
After you have run all the above commands, go back to your local machine and copy the content of the public key file ~/.ssh/udacity_project.pub. Paste the public key to the server's authorized_keys file using nano or any other text editor, and save.

After that, run exit. You would now be back to your local machine. To confirm that it worked, run the following command in your local machine:

ubuntu@ip-172-26-13-147:~$ ssh grader@172-26-13-147 -p 2200
You should now be able to log in as grader and would get a prompt to enter commands.

Next, run exit to go back to the host machine and proceed to the following step to disable root login.



. Disabling Root Login
Run the following command on your local machine to log in as root in the server:

$ ssh root@172-26-13-147 -p 2200
After you are logged in, open the file /etc/ssh/sshd_config with nano:

# nano /etc/ssh/sshd_config
Find the line PermitRootLogin yes and change it to PermitRootLogin no.

Restart the SSH server:

# service ssh restart
Terminate the connection:

# exit
After you are back to the host machine, when you try to log in as root, you should get an error:

ubuntu@ip-172-26-13-147:~$ ssh root@172-26-13-147 -p 2200
root@172-26-13-147: Permission denied (publickey).



. Installing Apache Web Server
To install the Apache Web Server, run the following command after logging in as the grader user via SSH:

$ sudo apt update
$ sudo apt install apache2
To confirm whether it successfully installed or not, enter the URL http://3.16.67.23 in your Web browser:

If the installation has succeeded, you should see the following Webpage:

Screenshot



. Installing pip
The package pip or pip3 will be required to install certain packages.

If you are using Python 2, to install it, run:

$ sudo apt install python-pip
If you are using Python 3, to install it, run:

$ sudo apt install python3-pip
To confirm whether or not it has been successfully installed, run:

$ pip --version
Or

$ pip3 --version
(Run either depending upon the Python version you'd want to use.)

You should see something like this if it has been successfully installed:

pip 9.0.1 from /usr/lib/python3/dist-packages (python 3.6)



. Installing and Configuring Git
12.1. Installing Git
In Ubuntu 18.04, git might already be pre-installed. If it isn't, run the following commands:

$ sudo add-apt-repository ppa:git-core/ppa
$ sudo apt update
$ sudo apt install git


. Configuring Git
To continue using git, you will have to configure a username and an email:

$ git config --global user.name "Jennie Kone"
$ git config --global user.email "djeniekone@domain.com"


. Installing and Configuring PostgreSQL
. Installing PostgreSQL
Create the file /etc/apt/sources.list.d/pgdg.list:

$ nano /etc/apt/sources.list.d/pgdg.list
And, add the following line to it:

deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main
Import the repository signing key, and update the package lists:

$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
$ sudo apt update
Install PostgreSQL:

$ sudo apt install postgresql-10


. Configuring PostgreSQL
Log in as the user postgres that was automatically created during the installation of PostgreSQL Server:

$ sudo su - postgres
Open the psql shell:

$ psql
This will open the psql shell. Now type the following commands one-by-one:

postgres=# CREATE DATABASE catalog;
postgres=# CREATE USER catalog;
postgres=# ALTER ROLE catalog WITH PASSWORD 'yourpassword';
postgres=# GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;
Then exit from the terminal by running \q followed by exit.



. Setting Up Apache to Run the Flask Application
. Installing mod_wsgi
The module mod_wsgi will allow your Python applications to run from Apache server.

If you are running Python 2, install it by running the following command:

$ sudo apt install libapache2-mod-wsgi
If you are running Python 3, run this:

$ sudo apt install libapache2-mod-wsgi-py3
This would also enable wsgi. So, you don't have to enable it manually.

After the installation has succeeded, restart the Apache server:

$ sudo service apache2 restart



. Cloning the Item Catalog Flask application
Change the current working directory to /var/www/:

$ cd /var/www/
Create a directory called FlaskApp and change the working directory to it:

$ sudo mkdir FlaskApp
$ cd FlaskApp/
Clone your GitHub repository of your Item Catalog application project (Flask project) as the directory FlaskApp. For example:

$ sudo git clone https://github.com/SDey96/Udacity-Item-Catalog-Project.git FlaskApp
Change the current working directory to the newly created directory:

$ cd FlaskApp/
The directory tree should now look like this:

FlaskApp
 └── FlaskApp
     ├── LICENSE
     ├── README.md
     ├── __init__.py
     ├── client_secrets.json
     ├── database_setup.py
     ├── drop_tables.py
     ├── fake_db_populator.py
     ├── static
     │   └── style.css
     └── templates
         ├── delete.html
         ├── delete_category.html
         ├── edit_category.html
         ├── index.html
         ├── items.html
         ├── layout.html
         ├── login.html
         ├── new-category.html
         ├── new-item-2.html
         ├── new-item.html
         ├── update-item.html
         └── view-item.html


. Then:
- Rename the application.py file to __init__.py using: mv application.py __init__.py.

In __init__.py, replace last line:
# app.run(host="0.0.0.0", port=8000, debug=True)
app.run()

In database.py, replace:
# engine = create_engine("sqlite:///catalog.db")
engine = create_engine('postgresql://catalog:PASSWORD@localhost/catalog')

------   -------   -------   ---------   ------   -------   -------   ---------
------   -------   -------   ---------   ------   -------   -------   ---------

. Install required packages:

$ sudo pip install --upgrade Flask SQLAlchemy httplib2 oauth2client requests psycopg2 psycopg2-binary
Or

$ sudo pip3 install --upgrade Flask SQLAlchemy httplib2 oauth2client requests psycopg2 psycopg2-binary
(Choose either of above depending on the Python version you're using.)


. Setting Up the VirtualHost Configuration
Run the following command in terminal to set up a file called FlaskApp.conf to configure the virtual hosts:

$ sudo nano /etc/apache2/sites-available/FlaskApp.conf
Add the following lines to it:


<VirtualHost *:80>
   ServerName  3.16.67.23
   ServerAlias http://3.16.67.23.xip.io/
   ServerAdmin jenniekone@domain.com
   WSGIScriptAlias / /var/www/FlaskApp/FlaskApp/flaskapp.wsgi
   <Directory /var/www/FlaskApp/FlaskApp/>
       Require all granted
   </Directory>
   Alias /static /var/www/FlaskApp/FlaskApp/static
   <Directory /var/www/FlaskApp/FlaskApp/static/>
       Require all granted
   </Directory>
   ErrorLog ${APACHE_LOG_DIR}/error.log
   LogLevel warn
   CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

Note: Kindly change the IP address to your own server's public IP and the email to yours.

Enable the virtual host:
$ sudo a2ensite FlaskApp

Restart Apache server:
$ sudo service apache2 restart


Creating the .wsgi File
Apache uses the .wsgi file to serve the Flask app. Move to the /var/www/FlaskApp/ directory and create a file named flaskapp.wsgi with following commands:

$ cd /var/www/FlaskApp/FlaskApp/
$ sudo nano flaskapp.wsgi
Add the following lines to the flaskapp.wsgi file:

import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/FlaskApp/FlaskApp/")

from application import app as application
In the above code, replace application with the name of the main module.


Restart Apache server:

$ sudo service apache2 restart
Now you should be able to run the application at http://3.16.67.23.xip.io

Note: You might still see the default Apache page despite setting everything up correctly. To resolve it and see your Flask app running, run the following commands in order:

$ sudo a2dissite 000-default.conf
$ sudo service apache2 restart


Debugging
If you are getting an Internal Server Error or any other error(s), make sure to check out Apache's error log for debugging:

$ sudo cat /var/log/apache2/error.log

. Other Helpful Resources
https://knowledge.udacity.com/questions/58974
http://sqlalche.me/e/e3q8
https://stackoverflow.com/questions/43268415/postgres-error-role-username-doesnt-exist-role-username-already-exists/56658832#56658832https://www.postgresql.org/docs/9.1/server-start.html
