# wordpress-runbook
## Installing wordpress with ubuntu 16.04 on the server

Begin by logging into your server using ssh.
Your command should look something like this:
	``` $ ssh root@IP_ADDRESS -p PORT_NUMBER ```


Now install the following commands:
$ sudo apt update
$ sudo apt install wordpress php libapache2-mod-php mysql-server php-mysql

When mysql prompts you to enter a password, enter a password of your choice. Remember it because this is what you will use to log into mysql in the future.

Follow these commands next to get the specific extensions of php that wordpress needs to run:
$ sudo apt update
$ sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip

The next adjustments are for apache:
$ sudo systemctl restart apache2

Go into the cd /etc/apache2/sites-available directory. Create a new file by doing $ Sudo nano wordpress.conf 

Insert the information below into the file but make sure it uses the correct path for where your wordpress install will be located.
<Directory /var/www/wordpress>
    Options FollowSymLinks
    AllowOverride Limit Options FileInfo
    DirectoryIndex index.php
    Order allow,deny
    Allow from all
</Directory>

Exit file.

write these commands to the terminal: 
$ sudo a2enmod rewrite
$ service apache2 restart
$ sudo apache2ctl configtest
$ sudo systemctl restart apache2

Go to this file path:  /var/www/html/
write this file sudo nano info.php.

Write to your file: 
<?php
				phpinfo();
 ?>
 
exit.

Go to your ip adress to see php info. Make sure this works.
 http://your_server_ip_address/info.php
 
 Go to var/www
 Write these commands: 
 $ wget -c http://wordpress.org/latest.tar.gz
 $ tar -xzvf latest.tar.gz
 $ chown -R www-data:www-data /var/www/wordpress
 
 The next steps set up your mysql database: 
 $ mysql -u root -p
   Enter the mysql password that you set up above.
    CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci; (Name the database what you would like but it is important that you remember the name.)
    GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost' IDENTIFIED BY ‘password'; (remember the username and password that you set.)
    FLUSH PRIVILEGES;
    EXIT;
    
Now that you have exited my sql, go into the wordpress directory and write these commands:
$ sudo mv wp-config-sample.php wp-config.php
$ sudo nano wp-config.php

The wp-config.php file, should look something like this:

// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress_db');

/** MySQL database username */
define('DB_USER', 'wordpress_user');

/** MySQL database password */
define('DB_PASSWORD', 'PASSWORD');

/** MySQL hostname */
define('DB_HOST', 'localhost');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');

edit these details with the database, username and password that you set.
exit.

Write this to the server:
$ systemctl restart apache2
$ systemctl restart mysql

These are your permissions:
$ sudo find /var/www/wordpress/ -type d -exec chmod 750 {} \;
$ sudo find /var/www/wordpress/ -type f -exec chmod 640 {} \;

$ sudo service reboot

Go to: your_ip_address/wordpress

Sign up and follow their instructions to set up your site!

Congratulations! You have successfully set up your wordpress site!

   
