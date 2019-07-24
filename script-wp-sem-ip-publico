#!/bin/bash

#INSTALA O O QUE É NECESSÁRIO
sudo apt -y update
sudo apt -y install php-curl php-gd php-mbstring php-xml php-xmlrpc
sudo apt-get -y install mysql-server
sudo apt -y install apache2
sudo apt -y install php libapache2-mod-php php-mysql

#CONFIGURA MYSQL PRA RECEBER O WORDPRESS
sudo mysql <<EOF

CREATE DATABASE wordpress;

CREATE USER 'wp_admin'@'localhost' IDENTIFIED BY 'root';

GRANT ALL ON wordpress.* TO 'wp_admin'@'localhost';

FLUSH PRIVILEGES;
EOF

#ALGUMAS MODIFICAÇÕES NO APACHE2

sudo sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/apache2/apache2.conf
sudo a2dissite 000-default.conf
sudo systemctl stop apache2.service
sudo systemctl start apache2.service
sudo systemctl enable apache2.service

#BAIXANDO WORDPRESS

wget https://wordpress.org/latest.tar.gz
tar -zxvf latest.tar.gz
sudo mv wordpress /var/www/html/wordpress

#SETANDO PERMIÇÕES NECESSÁRIAS
sudo chown -R www-data:www-data /var/www/html/wordpress/
sudo chmod -R 755 /var/www/html/wordpress/

sudo echo -e "<VirtualHost *:80> \n \
   ServerAdmin admin@example.com \n \
   DocumentRoot /var/www/html/wordpress/ \n \
   ServerName example.com \n \
   ServerAlias www.example.com \n \n \
   <Directory /var/www/html/wordpress/> \n 
        Options +FollowSymlinks \n \
        AllowOverride All \n \
        Require all granted \n \
   </Directory> \n \n \
   ErrorLog ${APACHE_LOG_DIR}/error.log \n \
   CustomLog ${APACHE_LOG_DIR}/access.log combined \n \n \
</VirtualHost>" > /etc/apache2/sites-available/wordpress.conf

#ATIVA O SITE DO WORDPRESS.CONF

sudo a2ensite wordpress.conf
sudo a2enmod rewrite
sudo systemctl restart apache2


#CONFIGURANDO BD DO WORDPRESS

sudo mv /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php

sudo sed -i 's/database_name_here/wordpress/g' /var/www/html/wordpress/wp-config.php
sudo sed -i 's/username_here/wp_admin/g' /var/www/html/wordpress/wp-config.php 
sudo sed -i 's/password_here/root/g' /var/www/html/wordpress/wp-config.php
