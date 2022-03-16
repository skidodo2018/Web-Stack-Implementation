# Project 1:Web-Stack-Implementation (LAMP STACK) in AWS EC2

Before going into the project details, I'd like to define a web stack.
## What is a Web Stack?
A Web stack is the collection of software required for Web development. It consists of an operating system (OS), a programming language, database software and a Web server. An example of a web stack is LAMP, but there are other different stacks as well and they are touched on below.
- **LAMP** (Linux, Apache, MySQL, PHP or Python, or Perl)
- **LEMP** (Linux, Nginx, MySQL, PHP or Python, or Perl)
- **MERN** (MongoDB, ExpressJS, ReactJS, NodeJS)
- **MEAN** (MongoDB, ExpressJS, AngularJS, NodeJS

As with software development, web development follows Software Development Life Cycle (SDLC).

## What is SDLC?
SDLC is a systematic process of building softwares that helps ensure the quality and correctness of the software that is built.
There are 7 phases in an SDLC process and they are listed as follows;
- **Phase 1: Requirement collection and analysis**
- **Phase 2: Feasibility study**
- **Phase 3: Design**
- **Phase 4: Coding**
- **Phase 5: Testing**
- **Phase 6: Installation/Deployment**
- **Phase 7: Maintenance**

You can read more about it [here](https://www.guru99.com/software-development-life-cycle-tutorial.html).

## Project Objectives:

- Become adept on the Linux Terminal.
- Deepen understanding of Web Stacks and understand the differences between the various Web stacks. 
- Increase knowledge of AWS platform and offerings that can be used to implement Web stacks.
- Implement a LAMP stack in AWS.

## Getting Started:

This project will be utilizing the AWS Cloud platform. Therefore, to get started on this project, one must have an AWS account. You can select any plan that fits your needs. I used the Free Tier Plan for this project. Click/Copy and paste the following link for a video on how to register for an AWS account: https://www.youtube.com/watch?v=v3WLJ_0hnOU

## How to Launch an EC2 instance in AWS ##
The following steps will explain how to create your first Instance.

1. Sign-in to your AWS Cloud Account as the Root user. 

![](/images/awslogin.png)

2. Type EC2 in the search bar at the top and select EC2 from the search results.

![](/images/EC2select.png)

3. Click on the launch instances button highlighted below

![](/images/launchinstance.png)

4. Select Ubuntu Server 20.04 LTS (HVM), SSD Volume Type - as shown below

![](/images/ubuntuselect.png)

5. Select T2 macro and click review and lauch button

![](/images/selectt2macro.png)

6. Click Launch, create new key pair, make sure to download the key and save it to your local system, and finally, click the launch button.

**N:B:** Without the key, you will not be able to access your instance via SSH. 

![](/images/createkey.png)

## Connecting to the EC2 instance from Linux terminal ##

For this project, I'm using Ubuntu to connect to the EC2 instance and will therefore give the steps below.

1. Change directory into the location where your PEM file is. In this case my PEM file is in the Downloads folder.

* $cd ~/Downloads
* Change permissions for the private key file (.pem), otherwise you can get an error “Bad permissions”. Use this command $ sudo chmod 0400 filename.pem where filename is the name of your AWS key which you downloaded.
* Connect to the instance by running this command $ ssh -i filename.pem ubuntu@public_ip_address

![](/images/sshEC2.png)

### STEP 1- Installing Apache and Updating the Firewall ###

* Install Apache using Ubuntu’s package manager ‘apt’:
* Update a list of packages in package manager $ sudo apt update
* Run apache2 package installation $ sudo apt install apache2
* To verify that apache2 is running as a Service in our OS, use following command $ sudo systemctl status apache2
* If it is green and running that it was done correctly.

![](/images/apache2.png)

Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web browsers use to access web pages on the Internet

As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to open inbound connection through port 80: https://www.youtube.com/watch?v=FMcJ2OGaaP0

* To access locally, run this command $ curl http://localhost:80 or $ curl http://127.0.0.1:80
* To test how Apache HTTP server can respond to requests from the Internet, open a web browser of your choice and try accessing the following url http://Public-IP-Address:80 (note: insert your EC2 IP Address between the <>)
* If you see following page, then your web server is now correctly installed and accessible through your firewall.

![](/images/apacheubuntupage.png)

### STEP 2 - Installing MySQL ###

We will be using ‘apt’ again to install the MySQL software.

Now that the web server is up and running, the next step will be to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. We will be using MySQL since it is a popular relational database management system used within PHP environments.

* Start with command: $ sudo apt install mysql-server
* When prompted, confirm installation by typing Y, and then ENTER.
* When the installation is finished, it’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. Start the interactive script by running: $ sudo mysql_secure_installation
* This will ask if you want to configure the VALIDATE PASSWORD PLUGIN. Note: You can leave validation disabled, but you should always use a strong password for database capabilities.
* Answer Y for yes, or anything else to continue without enabling.
* Validate password plugin which is used to test the strength of the password and allows the users to set said passwords. Would you like to setup validate password plugin?
* Press y|Y for Yes, any other key for No:` (If you answer “yes”, you’ll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words.)
* Press Y and hit the Enter key for the rest of the questions. *When you have finished, test if you’re able to log in to the MySQL console by typing: $ sudo mysql

*This will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command. You should see output like this:

![](/images/sudomysql.png)

* Run command mysql> exit

This will let you exit the mysql command prompt.

### STEP 3 - Installing PHP ###

This final installation of PHP will complete the LAMP stack installing and will now be functional.

Why is PHP important? PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

* To install these 3 packages at once, run: $ sudo apt install php libapache2-mod-php php-mysql
* Once the installation is finished, you can run the following command to confirm your PHP version: $ php -v

![](/images/phpv.png)

Lamp Stack is complete!

| Technology  | Status    |
| ----------- |:---------:|
| Linux       | Installed | 
| Apache      | Installed | 
| Mysql       | Installed |   
| PHP         | Installed |    

STEP 4 - Creating A Virtual Host for Your Website Using Apache

I created a domain called, PROJECTLAMP, for this project.

Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory. We will leave this configuration as is and will add our own directory to the next default one.

Create the directory for projectlamp using ‘mkdir’ command as follows: $ sudo mkdir /var/www/projectlamp

Next, assign ownership of the directory with the USER environment variable, which will reference your current system user: $ sudo chown -R $USER:$USER /var/www/projectlamp

Then, create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor. Here, we’ll be using vi: $ sudo vi /etc/apache2/sites-available/projectlamp.conf

Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text: 

Hit the esc button

Type :wq! and then hit enter to save the file and quite. (w = write and q =quit)

Now, I have exited out of the vi editor and have returned to my terminal screen, which should look like this:



(With this VirtualHost configuration, we’re telling Apache to serve projectlamp using /var/www/projectlampl as its web root directory. If you would like to test Apache without a domain name, you can remove or comment out the options ServerName and ServerAlias by adding a # character in the beginning of each option’s lines. Adding the # character there will tell the program to skip processing the instructions on those lines.)

You can now use a2ensite command to enable the new virtual host: $ sudo a2ensite projectlamp

You might want to disable the default website that comes installed with Apache. This is required if you’re not using a custom domain name, because in this case Apache’s default configuration would overwrite your virtual host. To disable Apache’s default website use a2dissite command , type: $ sudo a2dissite 000-default

To make sure your configuration file doesn’t contain syntax errors, run: $ sudo apache2ctl configtest

Finally, reload Apache so these changes take effect: $ sudo systemctl reload apache2 

Hurray! Your new website is now active, but the web root /var/www/projectlamp is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected: sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) with public IP $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html

Now go to your browser and try to open your website URL using IP address: http://:80

If you see the text from ‘echo’ command you wrote to index.html file, then it means your Apache virtual host is working as expected. In the output you will see your server’s public hostname (DNS name) and public IP address. You can also access your website in your browser by public DNS name, not only by IP - try it out, the result must be the same (port is optional): http://:80

Step 5 — Enable PHP on the website

With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. This is useful for setting up maintenance pages in PHP applications by creating a temporary index.html file containing an informative message to visitors. Because this page will take precedence over the index.php page, it will then become the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.

In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive: $ sudo vim /etc/apache2/mods-enabled/dir.conf

type i for insert and write: 

After saving and closing the file :wq!, you will need to reload Apache so the changes take effect: $ sudo systemctl reload apache2

Finally, we will create a PHP script to test that PHP is correctly installed and configured on your server.

Now that you have a custom location to host your website’s files and folders, we’ll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.

Create a new file named index.php inside your custom web root folder: $ vim /var/www/projectlamp/index.php

This will open a blank file. Add the following text, which is valid PHP code, inside the file:

?php phpinfo();

When you are finished, save and close the file, refresh the page and you will see a page similar to this:


