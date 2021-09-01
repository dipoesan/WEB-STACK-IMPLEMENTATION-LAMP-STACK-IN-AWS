# WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS

For this project, we are going to implement a LAMP stack which is a combination of these technologies - Linux, Apache, MySQL, PHP or Python, or Perl

### SETTING UP LINUX (**L**)
First off, we have to sign into our `AWS management console` and then setup the resources needed for this project. If you do not have an `AWS` account, head over to this [link](https://portal.aws.amazon.com/billing/signup#/start) to sign up for an account.

Launch a new EC2 instance of the `t2.micro` family with the Ubuntu Server 20.04 LTS (HVM) AMI under the `free tier`
![image](https://user-images.githubusercontent.com/22638955/131181775-aea02590-998a-4fac-a99b-9c6434a87c62.png)

![image](https://user-images.githubusercontent.com/22638955/131738526-662463a0-0975-4a02-baa8-b7aaed6b7848.png)

**NB**: Remember to save your `.pem` key when creating your instance.

To [ssh](https://searchsecurity.techtarget.com/definition/Secure-Shell) into my  EC2 instance, I made use of Mobaxterm. You can watch this [video](https://www.youtube.com/watch?v=g8XiC9Q2EEE&t=612s) to learn how to connect to your EC2 instance.

### INSTALLING APACHE AND UPDATING THE FIREWALL (**A**)
Install Apache using Ubuntu’s package manager ‘apt’:
```
#update a list of packages in package manager
sudo apt update

#run apache2 package installation
sudo apt install apache2
```
Verify that `apache2` is running as a Service in our OS
```
sudo systemctl status apache2
```
If apache is running, we should get a message like bwlow:
![image](https://user-images.githubusercontent.com/22638955/131739758-a75ccb2d-9f77-4d1d-a1eb-ed183226871b.png)

To receive traffic to our web server, we need to open `TCP port 80` in our EC2 instance/machine. `TCP port 80` is the default port that web browsers use to access web pages on the Internet. We would go into our AWS console, security group for our EC2 instance, and then edit inbound rules. End result should be like below:
![image](https://user-images.githubusercontent.com/22638955/131740300-b29c177b-d22b-41ae-a04e-380f1b2e7c59.png)

Our server is running and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’).

First, let us try to check how we can access it locally in our Ubuntu shell, run:
```
curl http://localhost:80
or
curl http://127.0.0.1:80
```
The `curl` command above is used to request our apache HTTP server on port 80 (specifying no port works also). For the first curl command, we tried accessing our server via the DNS name, and the second curl command is accessing by the IP address. Running the aove command gives us an output like below - 
![image](https://user-images.githubusercontent.com/22638955/131741933-36c2b819-937f-4ba2-abd0-6e7520829200.png)

We would now be testing to see if our apache server can respond to requests from the internet:
```
http://<Public-IP-Address>:80
```
![image](https://user-images.githubusercontent.com/22638955/131742187-06457df1-690b-4dd3-94af-0bffb23adabb.png)

Seeing the above page means our web server is correctly installed and accessible through our firewall.

### INSTALLING MYSQL (**M**)
We need to install a database management system to store and manage our data in a relational database. Our choice is MySQL.
```
sudo apt install mysql-server -y
```
When the installation is finished, it is recommended that we run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to our database system. Start the interactive script by running:
```
sudo mysql_secure_installation
```
This will ask if you want to configure the `VALIDATE PASSWORD PLUGIN`.
Answer `Y` for yes, or anything else to continue without enabling.
Regardless of whether you chose to set up the VALIDATE PASSWORD PLUGIN, your server will next ask you to select and confirm a password for the MySQL root user. 
This is not to be confused with the system root. The database root user is an administrative user with full privileges over the database system. Even though the default authentication method for the MySQL root user dispenses the use of a password, even when one is set, you should define a strong password here as an additional safety measure.
For the rest of the questions, press `Y` and hit the `ENTER` key at each prompt. This will remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes you have made.
When you’re finished, test if you’re able to log in to the MySQL console by typing:
```
sudo mysql
```
![image](https://user-images.githubusercontent.com/22638955/131744050-10da9631-b923-4513-a2c0-c1bc79ee3251.png)

### INSTALLING PHP (**P**)
PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the `php` package, we’ll need `php-mysql`, a PHP module that allows PHP to communicate with MySQL-based databases. 
We’ll also need `libapache2-mod-php` to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

To install these 3 packages at once, we would run:
```
sudo apt install php libapache2-mod-php php-mysql
```
Once they're done installing, we can check the version of PHP we have installed:
```
php -v
```
![image](https://user-images.githubusercontent.com/22638955/131744600-0c3b06b3-ec2e-463b-9773-e1c9cd55d1d8.png)

We have now installed all components of our LAMP stack

To test our setup with a PHP script, it’s best to set up a proper `Apache Virtual Host` to hold our website’s files and folders. Virtual host allows us to have multiple websites located on a single machine and users of the websites will not notice.

Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory.
We will leave this configuration as is and will add our own directory next next to the default one.

Create the directory for projectlamp:
```
sudo mkdir /var/www/projectlamp
```
Assign ownership of the directory with the current system user:
```
sudo chown -R $USER:$USER /var/www/projectlamp
```
Create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor.(My preferred CLI editor is `vi`)
A blank file will be created. We would be pasting the below config into this blank file:
```
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

With this VirtualHost configuration, we’re telling Apache to serve projectlamp using /var/www/projectlamp as its web root directory.

We would now use `a2ensite` command to enable the new virtual host:
```
sudo a2ensite projectlamp
```
We might want to disable the default website that comes installed with Apache. This is required if we’re not using a custom domain name, because in this case Apache’s default configuration would overwrite our virtual host.
```
sudo a2dissite 000-default
```
To make sure our configuration file doesn’t contain syntax errors, run:
```
sudo apache2ctl configtest
```
Reload Apache so these changes take effect:
```
sudo systemctl reload apache2
```
Our new website is now active, but the web root `/var/www/projectlamp` is still empty.
Create an `index.html` file in that location so that we can test that the virtual host works as expected:
```
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```
Running the above command should copy the output of the curl command (the IP address for our EC2 instance) into the `index.html` file.
![image](https://user-images.githubusercontent.com/22638955/131748033-4a281795-f99c-4007-b73b-a3991ee6faca.png)

Now go to the browser and try to open the website URL using our IP address found in the index.html file:
![image](https://user-images.githubusercontent.com/22638955/131748183-48356d78-0dc8-4b3e-9ff5-b61ee02122b3.png)

### ENABLE PHP ON THE WEBSITE
With the default **DirectoryIndex** settings on Apache, a file named `index.html` will always take precedence over an `index.php` file. This is useful for setting up maintenance pages in PHP applications, by creating a temporary `index.html` file containing an informative message to visitors. Because this page will take precedence over the `index.php` page, it will then become the landing page for the application. Once maintenance is over, the `index.html` is renamed or removed from the document root, bringing back the regular application page.

In case you want to change this behavior, you’ll need to edit the `/etc/apache2/mods-enabled/dir.conf` file and change the order in which the index.php file is listed within the DirectoryIndex directive:
```
sudo vim /etc/apache2/mods-enabled/dir.conf
```
```
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
After saving and closing the file, you will need to reload Apache so the changes take effect:
```
sudo systemctl reload apache2
```
Create a new file named index.php inside the custom web root folder:
```
vi /var/www/projectlamp/index.php
```
This will open a blank file. Add the following text, which is valid PHP code, inside the file:
```
<?php
phpinfo();
```
Reloading the previous webpage we opened in our browser should give us the below:
![image](https://user-images.githubusercontent.com/22638955/131749208-6a0f8905-7c34-4507-8169-44a8b940624a.png)
If you can see this page in your browser, then your PHP installation is working as expected.


Thanks to [darey.io](darey.io) for the opportunity to get into the DevOps sphere and also granting me access to projects like these.
