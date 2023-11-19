### How to make work walkman.lan site.
 First install LAMP.

'''Login to mysql'''
```$sudo mysql```
```
mysql> create database db_name;
mysql> show databases;
mysql> create user 'user_name'@'localhost' identified by 'Password';
mysql> grant all privileges on db_name.* to 'user_name'2'localhost';
```
### Download Wordpress.

```
curl -O https://wordpress.org/latest.zip

```
```
$mkdir walkman_html
$unzip latest.zip 
$sudo mv wordpress ~/walkman_html
```
```
$sudo mkdir -p /var/www/walkman.lan/public_html
$sudo chown -R $USER:$USER /var/www/walkman.lan/public_html
$sudo chmod 775 /var/www/walkman.lan/public_html
$sudo vim /var/www/walkman.lan/public_html/index.html
```
Write inside index.html
```
<html>
<head>
<title>Welcome to walkman.lan!</title>
</head>
<body>
Success! The walkman.lan is working.
</body>
</html>
```
### Configuration
```
$sudo vim /etc/apache2/site-enabled/walkman.conf
```
Paste the following in configuration block.

```
 <Directory />
    	Options All
     	Allow from all
     	Require all granted
</Directory>
     	
<VirtualHost *:80>
     		ServerName walkman.lan
     		ServerAlias www.walkman.lan 
    		DocumentRoot "/home/osboxes/walkman_html"
		ServerAdmin root@localhost
        	ErrorLog ${APACHE_LOG_DIR}/error.log
        	CustomLog ${APACHE_LOG_DIR}/access.log combined
 	<Directory "/home/osboxes/walkman_html/">
        	Options all
        	Allow from all
        	Require all granted
        	AllowOverride All
	</Directory>
	#Redirect / https://www.walkman.lan/
</VirtualHost>
    	
<VirtualHost *:443>
    		ServerName walkman.lan
        	ServerAlias www.walkman.lan
    		DocumentRoot "/home/osboxes/walkman_html"
    		SSLEngine on
    		SSLCertificateFile /etc/ssl/certs/apache.crt
    		SSLCertificatekeyFile /etc/ssl/private/apache.key
   	<Directory "/home/osboxes/walkman_html/">
    		Options all
   		Allow from all
    		Require all granted
    		AllowOverride All
    	</Directory>
    		ServerAdmin root@localhost
   		ErrorLog ${APACHE_LOG_DIR}/error.log
		CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
### Write Database name in .php file.
```
$cd walkman_html
$sudo vim wp-config-sample.php
```
```
write db_name user_name and Password in it.
```
### Check hosts...
``` 
$sudo vim /etc/hosts
```

```
write this inside hosts
127.0.0.1 www.walkman.lan walkman.lan 
```

```
$sudo service apache2 restart
$sudo chown -R www-data.www-data walkman_html
$sudo chmod -R 775 walkman_html
$sudo a2enmod php7.4
$sudo a2enmod ssl
$sudo a2enmod http2
$sudo phpenmod apache2
$sudo phpenmod mbstring
$sudo phpenmod mcrypt
$sudo a2enmod rewrite
$sudo a2enmod headers
$sudo service apache2 restart
```
```
$curl -I walkman.lan
$curl -I http://www.walkman.lan/wp-admin/setup-config.php
```

