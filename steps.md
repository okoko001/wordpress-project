HOW TO LAUNCH A WORDPRESS WEBSITE USING AWS EC2 INSTANCE AND AWS RDS AND ASSIGN A STATIC IP ADDRESS:

#STEP ONE

Login to AWS console
navigate to EC2 and launch an EC2 instance
when launched and initialized, copy the public ip Address
navigate to your ubuntu and ssh into your instance with your public ip.
After ssh into your instance:
sudo apt update : to update your ubuntu
#STEP TWO #Install Apache, Apache HTTP Server is the web server running on top of Linux in the LAMP stack. The web server uses HTTP to process requests and transmit information through the internet.

To install the Apache package, run the following command: sudo apt install apache2 -y
Check if Apache installed correctly by checking the Apache service status: sudo service apache2 status
Next, make sure that the UFW firewall contains the Apache profiles by typing in the following command: sudo ufw app list
Ensure the Apache Full profile allows the traffic on ports 80 and 443 by running the command: sudo ufw app info "Apache Full"
To confirm that Apache is running, enter the IP address of your server in the address bar of an internet browser and press ENTER.
#STEP THREE:

Install MySQL and Create a Database, MySQL is a relational database management system for creating and maintaining dynamic enterprise-level databases. It is compatible with all major OS platforms, which makes it a good fit for web application development.
Install MySQL by typing the following command: sudo apt install mysql-server -y
#STEP FOUR:

Install PHP, Although other programming languages, such as Python and Pearl, also work well within LAMP, PHP is usually the final layer of the stack because it integrates well with MySQL. As a dynamically typed language, PHP embeds into HTML, improving the speed and reducing the complexity of web applications.
Install PHP by following the steps below.

Obtain the necessary PHP packages by typing: sudo apt install php libapache2-mod-php php-mysql -y
Modify the way Apache serves files by opening the dir.conf file in a text editor with root privileges: sudo vi /etc/apache2/mods-enabled/dir.conf
Edit the list so that the index.php file is in the first position and then save exit.
For the changes to take effect, restart the Apache service by typing: sudo systemctl restart apache2
#STEP FIVE:

Test PHP Processing on Web Server: To test the new LAMP installation, create a basic PHP script and place it in the web root directory located at /var/www/html/, then check if the script is accessible via an internet browser. The steps below explain the procedure for performing this test.
Create a file in the web root directory by typing the following command: sudo vi /var/www/html/info.php
Inside the file, type the PHP code:
and the save and EXIT.

Open an internet browser and type the following address: [server-ip-address]/info.php
#STEP SIX:

Download and extract the latest version of WordPress:
wget https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz
Move the WordPress files to the Apache document root:
sudo mv wordpress/* /var/www/html/
Change the ownership of the files to the Apache user:
sudo chown -R www-data:www-data /var/www/html/
#STEP SEVEN: #CREATING A DATABASE MYSQL LOCALLY:

run the following command to log in to the MySQL prompt as the root user:
sudo mysql -u root -p
Create a new database for WordPress: CREATE DATABASE wordpress;
Create a new MySQL user with the following command: CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
Grant privileges to the new user on the newly created database: GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
Flush the privileges to make the changes effective: FLUSH PRIVILEGES;
Exit the MySQL prompt: EXIT;
#STEP EIGHT: #CREATING AWS RDS:

login to aws console and naviagate to RDS
Click on CREATE DATABASE
After you have login you will see: #Engine options: Engine type:
Amazon Aurora

MySQL

MariaDB

PostgreSQL

Oracle

Microsoft SQL Server : SELECT MySQL

Templates:
Production Use defaults for high availability and fast, consistent performance.

Dev/Test This instance is intended for development use outside of a production environment.

Free tier Use RDS Free Tier to develop new applications, test existing applications, or gain hands-on experience with Amazon RDS.

: SELECT FREE TIER.

SETTINGS:
IN HERE YOU WILL HAVE TO CREATE A DB CLUSTER IDENTIFIER/NAME OF YOUR CHOICE: database-1

#CREDENTIALS SETTINGS: IN HERE YOU WILL HAVE TO CREATE A MASTER USERNAME AND MASTER PASSWORD AND COMFIRM THE PASSWORD OF YOUR CHOICE:

master username: master password: confirm password:

CONNECTIVITY:
IN HERE YOU WILL HAVE TO CLICK 'YES' TO GIVE PUBLIC ACCESS

AFTER ALL IS DONE, CLICK ON CREATE DATABASE
#STEP NINE:

Configure WordPress to use the RDS instance: Open the WordPress configuration file: nano /var/www/html/wp-config-sample.php
Fill in the database connection details: define('DB_NAME', 'database_name'); define('DB_USER', 'database_user'); define('DB_PASSWORD', 'database_password'); define('DB_HOST', 'rds-instance-endpoint');
SAVE AND EXIT
#STEP TEN: CREATE ELASTIC IP ADDRESS FOR YOUR EC2 INSTANCE:

click on Elastic IPs, left to your instance.
Click on ALLOCATE ELASTIC IP ADDRESS
IN THE ALLOCATE ELASTIC IP ADDRESS PAGE:
scroll down click on ADD NEW TAGS and add tags of your choice.
click on ALLOCATE after you are done.
after allocating click on ACTIONS above:
and then associate your ip to your EC2 instance.
#STEP ELEVEN: after all is done, your have to check if your wordpress is configure:

paste your ELASTIC IP to your BROWSER
It will load and you will be prompt to 'wp-admin'
You will fill in: - DATABASE NAME: - DATABASE USER: - DATABASE PASSWORD: - DATABASE URL:
After then you will be prompt to the wordpress backend, Above you will see the MY PAGE, click on it and you will see the front view of your website.
DONE.
