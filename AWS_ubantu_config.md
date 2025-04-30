# Ubuntu 24.04 Server Setup Guide

This guide provides step-by-step instructions to install and configure Apache Web Server, PHP, MySQL, and other essential services on Ubuntu 24.04.

## Table of Contents

1. [Apache Web Server Installation](#apache-web-server-installation)
2. [Install PHP and MySQLi](#install-php-and-mysqli)
3. [Solve Ubuntu Rewrite Issue](#solve-ubuntu-rewrite-issue)
4. [File/Folder Permissions](#filefolder-permissions)
5. [MySQL Configuration and Management](#mysql-configuration-and-management)
6. [Connect with phpMyAdmin](#connect-with-phpmyadmin)

## Apache Web Server Installation

### Step 1: Update System Packages

Make sure your system is up-to-date:

```bash
sudo apt update
sudo apt upgrade -y

## 🌐 Step 2: Install Apache Web Server
Install Apache using the APT package manager:

```bash
sudo apt install apache2 -y
## 🔍 Step 3: Check Apache Service Status
Verify that Apache is running:

```bash
sudo systemctl status apache2
Start and enable Apache on system boot:

```bash
sudo systemctl start apache2
sudo systemctl enable apache2
## 🔒 Step 4: Configure the Firewall (UFW)
Allow HTTP and HTTPS through the firewall:

```bash
sudo ufw allow 'Apache Full'
Check firewall status:

```bash
sudo ufw status
## 🧪 Step 5: Test Apache Installation
Open your browser and visit:

arduino
http://your_server_ip
To find your server IP address:

```bash
hostname -I
You should see the Apache2 Ubuntu Default Page.

## ⚙️ Step 6: Common Apache Commands

Action	Command
Start Apache	sudo systemctl start apache2
Stop Apache	sudo systemctl stop apache2
Restart Apache	sudo systemctl restart apache2
Reload Config	sudo systemctl reload apache2
Enable on Boot	sudo systemctl enable apache2
Disable on Boot	sudo systemctl disable apache2
## 📁 Step 7: Apache Directory Structure

Directory	Purpose
/etc/apache2/	Main Apache configuration files
/etc/apache2/apache2.conf	Main Apache config file
/etc/apache2/sites-available/	Virtual host config files
/var/www/html/	Default web root directory
Place your website files in:

```bash
/var/www/html/
Edit the default index file:

```bash
sudo nano /var/www/html/index.html
## 🔄 Step 8: Enable Apache Modules (Optional)
Enable the rewrite module for .htaccess support:

```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
✅ Apache is Now Installed and Running!
You can now serve websites, configure virtual hosts, and secure your server with HTTPS.

📌 Optional: Set Up a Virtual Host
Create a directory:

```bash
sudo mkdir -p /var/www/yourdomain.com/public_html
Set permissions:

```bash
sudo chown -R $USER:$USER /var/www/yourdomain.com/public_html
Create a virtual host file:

```bash
sudo nano /etc/apache2/sites-available/yourdomain.com.conf
Paste:

apache
<VirtualHost *:80>
    ServerAdmin admin@yourdomain.com
    ServerName yourdomain.com
    ServerAlias www.yourdomain.com
    DocumentRoot /var/www/yourdomain.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
Enable the site and reload Apache:

```bash
sudo a2ensite yourdomain.com.conf
sudo systemctl reload apache2
🔐 Optional: Secure Apache with Let's Encrypt SSL
Install Certbot:

```bash
sudo apt install certbot python3-certbot-apache -y
Run Certbot:

```bash
sudo certbot --apache
Follow the prompts to secure your domain with HTTPS.

🧹 Uninstall Apache (If Needed)
To completely remove Apache:

```bash
sudo apt remove apache2 -y
sudo apt autoremove -y


## INTALL PHP AND MySQLi IN UBANTU:-
-----------------------------------
## ✅ Step-by-Step Instructions for Ubuntu 24.04
1. Install required dependencies

```bash
sudo apt update
sudo apt install -y software-properties-common lsb-release ca-certificates apt-transport-https
2. Add Ondřej Surý's PHP PPA

```bash
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update
3. Install PHP 7.4 and extensions including MySQLi

```bash
sudo apt install -y php7.4 php7.4-cli php7.4-common php7.4-mysql php7.4-curl php7.4-xml php7.4-mbstring php7.4-zip php7.4-gd
The php7.4-mysql package includes the mysqli extension.

4. Verify installation

```bash
php -v
php -m | grep mysqli

5. (Optional) Install Apache or Nginx with PHP support
For Apache:

```bash
sudo apt install apache2 libapache2-mod-php7.4
sudo systemctl restart apache2
For Nginx with PHP-FPM:

```bash
sudo apt install php7.4-fpm
sudo systemctl restart php7.4-fpm


## SOLVE UBANTU REWIRTE ISSUE:
-----------------------------
Enable mod_rewrite:
sudo a2enmod rewrite
sudo systemctl restart apache2

Possibility 2: Apache doesn't allow .htaccess (override)
Edit your site config:
sudo nano /etc/apache2/sites-available/000-default.conf

Look for this section:

<Directory /var/www/html>
    AllowOverride None
</Directory>

Change AllowOverride None to:
<Directory /var/www/html>
    AllowOverride All
</Directory>

Then restart Apache:
sudo systemctl restart apache2

Possibility 3: File Permissions Issue
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html

Possibility 4: .htaccess syntax error
Make sure your .htaccess file is valid. A minimal working example for clean URLs:
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^ index.php [L]
</IfModule>


## FILE/FOLDER PERMISSION:
--------------------------
Set writable permissions:
sudo chmod -R 775 /var/www/html/application/cache/temp

Set the proper owner (usually the web server user):
ps aux | egrep '(apache|httpd|nginx|www-data)'


Then set the ownership, for example (for Apache on Ubuntu/Debian):

sudo chown -R www-data:www-data /var/www/html/application/cache/temp

Verify the changes:
ls -ld /var/www/html/application/cache/temp


You should see something like:
drwxrwxr-x 2 www-data www-data 4096 Apr 30 12:00 /var/www/html/application/cache/temp


sudo chown -R www-data:www-data /var/www/html/application/cache/temp
sudo chmod -R 775 /var/www/html/application/cache/temp

sudo chmod -R 775 /var/www/html/application/cache
sudo chmod -R 775 /var/www/html/application

sudo chmod -R 777 /var/www/html/application/cache/temp
sudo chown -R www-data:www-data /var/www/html/application/cache/temp

sudo systemctl restart apache2
sudo systemctl restart nginx


# MySQL Configuration and Management on Ubuntu (AWS):
-----------------------------------------------------
This document includes essential MySQL commands to connect, export, and import a database from an AWS RDS instance on Ubuntu.

---
## 🔗 Connect with MySQLi

### ➤ Without Port
```bash
mysql -u admin -p -h hotelturnbddatabase-29.crcg60yeg6df.ap-south-1.rds.amazonaws.com
mysql -u turnbddb21 -p -h turnbddatabase.crcg60yeg6df.ap-south-1.rds.amazonaws.com -P 3306
mysql -u turnbddb21 -p -h turnbddatabase.crcg60yeg6df.ap-south-1.rds.amazonaws.com -P 25060
---

#COMMON QUERY
-------------
SHOW DATABASES;
USE DATABASE_NAME;
SHOW TABLES;

SELECT COUNT(*) AS total_tables 
FROM information_schema.tables 
WHERE table_schema = 'HOTEL_WTC';


## 🔗 Import MySQLi
```bash
mysql -u admin -p -h hotelturnbddatabase-29.crcg60yeg6df.ap-south-1.rds.amazonaws.com HOTEL_WTC < local_gph_v2_backup_20042025.sql
mysql -u admin -p -h hotelturnbddatabase-29.crcg60yeg6df.ap-south-1.rds.amazonaws.com HOTEL_WTC < /home/ubuntu/local_gph_v2_backup_20042025.sql


## 🔗 Export MySQLi
```bash
mysqldump -u turnbddb21 -p -h turnbddatabase.crcg60yeg6df.ap-south-1.rds.amazonaws.com \
--routines --triggers --single-transaction --set-gtid-purged=OFF GPH_V2 > abdullah_gph_v2_backup.sql
---

## CONNECT WITH PHPMYADMIN:-
-----------------------------
## 1. Check MySQL is running
Make sure MySQL is up and running:

```bash
sudo systemctl status mysql
If it’s not running, start it:

```bash
sudo systemctl start mysql
🔧 2. Check MySQL socket path
Run:

```bash
mysqladmin variables | grep socket
You will see something like:

perl
| socket     | /var/run/mysqld/mysqld.sock
Make note of that path.

🔧 3. Update phpMyAdmin config
Edit your config.inc.php file in the phpMyAdmin directory:

```bash
sudo nano /var/www/html/phpmyadmin/config.inc.php
Add or update this line near the $cfg['Servers'][$i]['host']:

php
$cfg['Servers'][$i]['host'] = '127.0.0.1'; // or 'localhost'
$cfg['Servers'][$i]['connect_type'] = 'tcp'; // forces TCP instead of socket
This tells phpMyAdmin to connect via TCP/IP instead of a Unix socket, which avoids socket path issues.

🔧 4. Make sure MySQL allows TCP connections
Check if MySQL is listening on 127.0.0.1:

```bash
sudo netstat -tlnp | grep mysql
You should see something like:

nginx
tcp        0      0 127.0.0.1:3306      0.0.0.0:*       LISTEN      [mysqld PID]
If not, check MySQL config in:

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
And make sure this line is set to:

ini
```bash
bind-address = 127.0.0.1
Then restart MySQL:

```bash
sudo systemctl restart mysql
🔧 5. Check MySQL credentials
Make sure the username/password you're using in phpMyAdmin is correct and has access.

```bash
sudo chmod -R 777 /var/www/html/phpmyadmin
sudo chown -R www-data:www-data /var/www/html/phpmyadmin

phpMyAdmin - Error
Wrong permissions on configuration file, should not be world writable!


This new error:

"Wrong permissions on configuration file, should not be world writable!"

means that your config.inc.php file in phpMyAdmin has insecure permissions, and phpMyAdmin is refusing to run for security reasons.

✅ Fix the File Permissions
Run the following command in your terminal to fix it:

```bash
sudo chmod 644 /var/www/html/phpmyadmin/config.inc.php

This sets the permissions to:

Owner: read + write
Group: read
Others: read

Which is safe and acceptable for phpMyAdmin.


