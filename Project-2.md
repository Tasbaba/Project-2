##  **PROJECT 2: LEMP STACK IMPLEMENTATION**

### **Installing the Nginx Web Server**
### I will be installing Nginx for displaying the web pages to our site visitors.
### I will start the installation Nginx by updating my server’s package index if it is the first time I am using the apt on the server, and I will be using the below command for the update.

**`sudo apt update`**

![apt-update Nginx](./images/apt-update.jpg)

### When the apt update is completed I will have to install the Nginx web server using the below command.

**`sudo apt install Nginx`**

![Install Nginx](./images/Nginx-installation.jpg)

### When the Nginx installation is completed. I will have to confirm the **status** if it is running. And the **active running** will be in green font color, and this means that the installation is successful, and the web server is launched in the cloud.

### I used the below command to confirm the Nginx status.

**`sudo systemctl status Nginx`**

![Nginx status](./images/Nginx-status.jpg)

## **Enable Traffic access to the Web server**

### Before the web server can recieve any traffic I will need to open TCP port 80 which is default port that web browsers use to access web pages from the Internet.
### To achieve this in the AWS. I will need to add a rule to EC2 configuration to open inbound connection through port 80, as shown below.

### - Check the box in front of the EC2 instance
### - Select security
### - Under inbound rules, select security group
### - Click on edit inbound rules and select HTTP from the options to add port 80 with source IP:0.0.0.0

![Enable port 80](./images/adding%20port%2080.jpg)

### **Accessing the web server locally**
### I can access the web server locally through the Internet Source 0.0.0.0/0 which  means from any IP address
### I tried to access the web server locally in my Ubuntu shell by running either of the below url.

**`curl http://localhost:80`** 

**`curl http://127.0.0.1:80`** 

![local-access.1](./images/Local-access.1.jpg)

![local-access.2](./images/Local-access.2.jpg)

### **Nginx Server Respond to Requests**
### Now is the time to test if my Nginx server can respond to requests from the Internet. 
### I will have to open any web browser from my system and try to access it using the below url, by adding my EC2 instace public IP

**`http://<Public-IP-Address>:80`** 

![Nginx confirmation from localhost](./images/Nginx-confirmation.jpg)

### **INSTALLING MYSQL**

### After the web server is already installed, we will have to installed MySQL to to enable me store and manage data for the site as a Database Management System (DBMS)  MySQL is a popular relational database management system

### In other to get the MySQL installed I will use the command below, and when prompted, confirm installation by typing Y, and then press the ENTER key.

**`sudo apt install mysql-server`** 

![mysql install-1](./images/mysql%20install-1.jpg)

![mysql install-2](./images/mysql%20install-2.jpg)

### After the installation is completed, I will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo, and -p in the command, and this will prompt you for the password, I then entered the password to access the MySQL to ensure all I have done is okay. 

### Lastly, I exit MySQL console using my exit command below.

**`sudo mysql -p`** & **`exit`**

![Access Mysql](./images/Access%20MySQL.jpg)

![exit Mysql](./images/exit-Mysql.jpg)

## **INSTALLING PHP**

### At this stage I will be install PHP to support in processing code and generate dynamic content for the web server.

### In other to allows a better performance in all PHP-based websites as a requirement, Nginx will requires an external program to handle the PHP process, and also act as a bridge between the PHP interpreter and the web server.

### Inother to get a better performance I installed php-fpm (PHP fastCGI process manager), which tell's Nginx to pass PHP requests to the software for processing. php-mysql will also be installed and it is a PHP module that allows PHP to communicate with MySQL-based databases, and lastly, the core PHP packages will automatically be installed as dependencies.

### **The below command will be used to install the two (2) packages, php-fpm & php-mysql at once.**

**`sudo apt install php-fpm php-mysql`**

![installing php-fpm & php-mysql](./images/installing%20php-fpm%20and%20php-mysql.jpg)

## **CONFIGURING NGINX TO USE PHP PROCESSOR**
### Alfter we have PHP components installed the next action will be to configure Nginx to use the php-fpm & php-mysql.

### **- Configure Nginx to Use PHP Processor**
### When using the Nginx web server, I can create server blocks to encapsulate configuration details and host multiple domain on a single server. In this project, I will use projectLEMP as my domain name.

### Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. this works well for a single site, but it is difficult to manage if it is hosting multiple sites. 
### We will create another directory structure  within /var/www for my domain website, while leaving /var/www/html to serve as the default directory if cient request do not match any other sites.

### To create the root web directory for my domain i use the below command.

**`sudo mkdir /var/www/projectLEMP`**

![mkdir for projectLEMP](./images/mkdir%20for%20projectLEMP.jpg)

### Once I have created the domain (/var/www/projectLEMP/), The next action is to assign ownership to the directory with the $USER environment variable, which will reference my current system user by using the below command.

**`sudo chown -R $USER:$USER /var/www/projectLEMP`**

![assign user to the domain](./images/assign%20user%20to%20the%20domain.jpg)

### After the user is created, I open a new configuration file in Nginx’s sites-available directory using nano command-line editor by using the below command line, and this create a new blank file, and I paste in the following bare-bones configuration as shown below.

**`sudo nano /etc/nginx/sites-available/projectLEMP`**

### **bare-bones configuration**
![bare bones configurations](./images/bare%20bones%20config.jpg)

### After I have review bare bones I copiee and pasted in the nano editor, I save it and close the file via CTRL+X, CTRL+Y, and ENTER to confirm.

### **- Activate Configuration**
### I then activate the configuration via linking the configed file from Nginx sites-enabled directory by using the below command.

**`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`**

![Activate Config](./images/activate%20configuration.jpg)

### By activating the configuration, it tell's Nginx to use the configuration the next time it is reloaded, and I tested my configuration for syntax errors through the below command.

**`sudo nginx -t`**

![Check for syntax error](./images/syntax%20error%20test.jpg)

### Since check for syntax error is confirmed okay, i need to disable the default Nginx host currently configured to listen on port 80 by running the below command.

**`sudo unlink /etc/nginx/sites-enabled/default`**

![Un-link default enable sites](./images/Un-link%20default%20using%20port%2080.jpg)

### After I disable the default Nginx host listening on port 80, I reloadde Nginx to apply the changes by using the below command.

**`sudo systemctl reload nginx`**

![reload to apply changes](./images/reload%20to%20apply%20changes.jpg)

### My new website is active, but the web root /var/www/projectLEMP is empty, and I will create an index.html file in that location so that I can test the new server block is working as expected.

### The below command is used with my public intance V4 IP address.

**`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`**

![create index](./images/create%20index.jpg)

### The Index is created as shown above, and I will lunch my browser and try to open my website URL using the public IP address. as shown below.

**`http://<Public-IP-Address>:80`**

![open website](./images/open%20website.jpg)

## **

### In other to test and validate that Nginx can correctly hand .php files off to PHP processor.

### I created a test PHP file in my document root, and open a new file called info.php within the document root in the text editor.
### I use the below command to access the nano editor.

**`sudo nano /var/www/projectLEMP/info.php`**

### I pasted the below syntax lines into the new nano file, as it will return information about my server.

**`<?php`**

**`phpinfo();`**

### I can now use the web browser by visiting the domain name or public IP address I set up in my Nginx configuration file, followed by /info.php as shown below, and I will see a web page containing detailed information about my server.

**`http://`server_domain_or_IP`/info.php`**

![php detail info](./images/detail%20information%20about%20php.jpg)

### After confirm all the relevant information about my PHP server through the above page, it is best to remove the file I created as it contains sensitive information about MY PHP environment and the Ubuntu server by running the remove (rm) command to remove that file as shown below.

**`sudo rm /var/www/your_domain/info.ph`**

### **Retrieving data from MySQL database with PHP**

### At this stage I will create a test database (DB) with simple “To do list” and configure access to it, so the Nginx website would be able to query data from the DB and display it.

### I will create a database called example_database and a user named example_user.

### Firstly, I will connect to MYSQL console via the the command line below















































