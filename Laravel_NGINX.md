# Deploy Laravel App on AWS EC2 Instance ubuntu 22.04 Nginx Server

## Steps to Deploy a Laravel App on EC2 Instance Ubuntu 22.04 Nginx Server

This guide will walk you through the steps of deploying a Laravel application on an AWS EC2 instance running Ubuntu 22.04 with Nginx as the web server.

### Steps

1. Create An EC2 Instance on AWS console

- Login to AWS console and navigate to the EC2 service.
- Click on "Launch Instances" to create a new EC2 instance.
- Give a name to your instance
- Select an Ubuntu 22.04 and choose the appropriate instance type for your application.
- Configure the instance's network settings.
- Configure the security group to allow inbound traffic on port 22 (SSH), port 80 (HTTP) and port 443 (HTTPS).
      - To do this click on edit on network card
- Configure your storage
- Launch the instance.

After creating your instances copy the instances address (IP address). This will be used together with the created security group to login to your instances.

To login paste the following;

``` ssh -i your_key_pair.pem ubuntu@instance_address ```

2. Install Nginx Server

```
# Update the package repository
sudo apt update

# Install Nginx
sudo apt install nginx

# Verify that Nginx is running
systemctl status nginx

# Restart Nginx if not working
sudo systemctl restart nginx

# To test the Nginx configuration
sudo nginx -t  
```

3. Install PHP with required packages

```

# Update the package repository 
sudo apt update

# Install PHP 8.1 and the required packages
sudo apt install --no-install-recommends php8.1
sudo apt-get install -y php8.1-cli php8.1-common php8.1-mysql php8.1-zip php8.1-gd php8.1-mbstring php8.1-curl php8.1-xml php8.1-bcmath php8.1-fpm

```
4. Install Composer (Php package manager)

```
# Install the dependencies for Composer

sudo apt update
sudo apt install php-cli unzip

# Download and install Composer

cd ~
curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php

# Using curl, fetch the latest signature and store it in a shell variable:

HASH=`curl -sS https://composer.github.io/installer.sig`

# download and install Composer as a system-wide command named composer, under /usr/local/bin:

sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer

# Test your Composer installation

composer

```

5. Install MySQL Database

```

# Install MySQL

sudo apt update
sudo apt install mysql-server

# Configure MySQL

mysql

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your password';

exit

# read more https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-22-04

#

```

6. Install phpmyadmin and configure with nginx server

#### Step 1 — Installing phpMyAdmin

```

sudo apt update
sudo apt install phpmyadmin

sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin

 Check Permissions

 sudo chown -R www-data:www-data /var/www/html/phpmyadmin
 sudo chmod -R 755 /var/www/html/phpmyadmin
 sudo chown -R www-data:www-data /var/www/html

// change permission for root file
sudo chmod -R 755 /var/www/html/

```

#### Step 2 - Editing nginx
- Goto laravel website select version you are currently using
- Click on deployment; copy the line of code
- goto your terminal; type:

``` sudo nano /etc/nginx/sites-available/filename```

if you don't know available filename; you can check list of files using

``` ls -al /etc/nginx/sites-available/filename ```

- paste the lines you copied  using ***crtl + insert*** or ***crtl + shift + v***

once all this is done you can pull your project from github into your instance server root (/var/www/html)