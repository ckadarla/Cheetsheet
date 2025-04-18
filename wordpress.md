Install Apache2,PHP 8.x and Mysql 
```
sudo apt install -y php php-fpm php-mysql php-opcache php-cli libapache2-mod-php apache2 mysql-server 
```

Start and enable server for all services 

```
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl enable mysql
sudo systemctl start mysql
sudo systemctl start php8.3-fpm
sudo systemctl enable php8.3-fpm
```

Secure the mysql database 

```
sudo mysql_secure_installation
```
Create password for root user
```
alter user 'root'@'localhost' IDENTIFIED BY 'Strong@password123';
```
Create Database for wordpress
```
CREATE database wordpress;
```
Create user for wordpress db 
```
CREATE USER 'dbadmin'@'localhost' IDENTIFIED BY 'Strong@@password123';
FLUSH PRIVILEGES;
```
Test PHP Config 
```
sudo nano /var/www/html/info.php
````
File content 
```
<?php
phpinfo();
?>
```
Download lastest wordpress
```
https://wordpress.org/latest.zip
```
