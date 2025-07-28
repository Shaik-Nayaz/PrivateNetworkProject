# PrivateNetworkProject
🗃️ *UDC Server: Step-by-Step Installation & Configuration Guide*

📖 *Project Overview*
The User Data Collector (UDC) is a PHP-based web application for securely collecting user data (name, age, country) and file uploads, saving them into a MySQL database. Deployed on two VMs: one as the web server, one as the database server.

⚙️ *Prerequisites*
2 CentOS 8/RHEL 8 VMs (Web and DB server)
Sudo/root privileges
Internet access
![WhatsApp Image 2025-07-28 at 13 10 06_5032658d](https://github.com/user-attachments/assets/1ee46c19-13ea-46da-a009-fe8b0bdaf62e)

1️⃣ *Web Server Setup (Apache + PHP)*
Step 1: Install and Start Apache
yum install httpd -y
systemctl enable --now httpd


Step 2: Install PHP (and PHP-MySQL extension)
bash
dnf module -y enable php:8.1
dnf module -y install php:8.1/common
yum install php-mysqli -y
php -v

Step 3: Test PHP with a "Hello World" Page
bash
echo "<?php echo 'Hello World!'; ?>" > /var/www/html/php_test.php
In browser, visit: http://<Web-Server-IP>/php_test.php
<img width="1919" height="1076" alt="image" src="https://github.com/user-attachments/assets/bd2d544e-2c53-4de1-9fd6-e4d1d4eb7794" />



2️⃣ *Database Server Setup (MySQL)*
Step 4: Install and Start MySQL Server
bash
dnf install -y mysql-server
systemctl enable --now mysqld
mysql --version
    1<img width="588" height="527" alt="image" src="https://github.com/user-attachments/assets/209a90be-39a0-475c-9797-7384106a24a8" />


Step 5: Create Application Database and User
bash
mysql -u root -p
In MySQL shell:





sql
CREATE DATABASE udc;
CREATE USER 'udc'@'%' IDENTIFIED BY 'Welcome@123';
GRANT ALL PRIVILEGES ON udc.* TO 'udc'@'%';
exit;

Step 6: Test Remote MySQL Access from Web Server
bash
mysql -h <DB-SERVER-IP> -u udc -p

3️⃣ *UDC Application Configuration*
Step 7: Test Database Connection from PHP
Create /var/www/html/db_check.php as described.
<img width="1600" height="822" alt="image" src="https://github.com/user-attachments/assets/26136fcb-65d3-4c08-99b8-e834eafe2211" />


Visit: http://<Web-Server-IP>/db_check.php

Step 8: Create users Table
From MySQL shell on DB server:

sql
USE udc;
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  age INT,
  country VARCHAR(100),
  file VARCHAR(255)
);

Step 9: Upload Directory on DB Server
bash
mkdir -p /var/udc/uploads
chmod -R 777 /var/udc
ls -ld /var/udc/uploads

Step 10: Deploy and Use UDC main.php
Edit /var/www/html/main.php (see earlier for code).
In browser, visit: http://<Web-Server-IP>/main.php
Fill out the form and submit.
<img width="767" height="298" alt="Screenshot 2025-07-25 130152 (1)(1)" src="https://github.com/user-attachments/assets/015322f0-b480-45b3-9bf7-358abe8dfb5d" />

