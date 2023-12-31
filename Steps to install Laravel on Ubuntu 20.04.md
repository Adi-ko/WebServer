Steps to install Laravel on Ubuntu 20.04
Step 1: Install Apache web server

To host the Laravel program, you need to install a web server, which can install either Nginx or Apache web servers. To choose one of the two proposed web servers, you can Compare Apache with Nginx and select one after familiarizing yourself with them. In this tutorial, we preferred installing the Apache web server. For this purpose, enter the apache2 command:

$ sudo apt install apache2

After installing the Apache web server, you should see it running. If it doesn’t work for any reason, start Apache:

$ sudo systemctl start apache2

Then enter the following command to enable Apache at boot time:

$ sudo systemctl enable apache2

You can check the Apache status and make sure it is running without errors using the following command:

$ sudo systemctl status apache2

Step 2: Installing PHP and its additional plugins

PHP 7.3 and above is a prerequisite for using Laravel 8, which is available in the Ubuntu repositories, PHP 7.4. As a result, type the following command to install PHP and its required modules:

$ sudo apt install php libapache2-mod-php php-mbstring php-cli php-bcmath php-json php-xml php-zip php-pdo php-common php-tokenizer php-mysql

Verify the PHP version after successfully installing PHP using the following command:

$ php –v

Step3: creating a database

To store the data generated by the Laravel program, you need a database. At this stage, you must create a database for the Laravel program. Since Laravel is compatible with MariaDB, MySQL, SQLite, Postgres, or SQL Server database systems, we will install the MariaDB database engine:

$ sudo apt install mariadb-server

To access the MariaDB prompt after installing the database server, you need to use the following command:

$ sudo mysql -u root -p

Sign in, then make a database and a user for it; grant the database user the necessary permissions.

CREATE DATABASE laravel_db;

CREATE USER 'laravel_user'@'localhost' IDENTIFIED BY 'secretpassword';

GRANT ALL ON laravel_db.* TO 'laravel_user'@'localhost';

FLUSH PRIVILEGES;

QUIT;

How to install lavarel on ubuntu 20.04
Step 4: Install Composer

Composer is a package manager and prerequisite management tool for PHP and manages the libraries and dependencies required by PHP based on the particular framework. In this section, we will use it to download the Laravel core and install all the necessary Laravel components, so to install Composer, it is required to download the Composer installer (composer. phar file) by executing the curl command and move the file to the usr/local/bin/ directory:

$ curl -sS https://getcomposer.org/installer | php

After executing the above command, the composer. phar file will be downloaded, and to move the file to the usr/local/bin/ path, run the following command:

$ sudo mv composer.phar /usr/local/bin/composer

Assign authority to execute:

$ sudo chmod +x /usr/local/bin/composer

Verifying the Composer version after installing it is done through the following command:

$ composer -–version

Step 5: Install Laravel 8 on Ubuntu

After completing Composer installation, it is time to install Laravel.

To install Laravel, you must first go to the webroot directory, and for this purpose, you must type the following command:

$ cd /var/www/html

Then, to install Laravel, you must use the create-project command for the composer:

$ sudo composer create-project laravel/laravel laravelapp

With the help of this command, you create a new directory called laravelapp to install the necessary files and directories of Laravel. The web server user is set to own the Laravel directory:

sudo chown -R www-data:www-data /var/www/html/laravelapp

sudo chmod -R 775 /var/www/html/laravelapp/storage

Then you can choose a new directory name for laravelapp.

Once the installation process is finished, go to the installation directory to check the version of Laravel:

$ cd laravelapp

$ php artisan

Step 6: Configure Apache to serve Laravel

To host a Laravel site, it is necessary to configure the Apache web server, and for this purpose, you must launch the virtual host file:

$ sudo vim /etc/apache2/sites-available/laravel.conf

In this step, skip the displayed content and change the example.com address of the ServerName Instructions to the fully qualified domain name (FQDN) or the server’s public IP (or private IP if the server is located in a LAN network).

<VirtualHost *:80>
ServerName example.com
ServerAdmin admin@example.com
DocumentRoot /var/www/html/laravelapp/public
<Directory /var/www/html/laravelapp>
AllowOverride All
</Directory>
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

Now you need to save the changes made and close the file. In the next step, you must execute these two commands to activate the Laravel site and the Apache rewrite module.

$ sudo a2ensite laravel.conf

$ sudo a2enmod rewrite

For your changes to take effect, restart Apache with the following command:

$ sudo systemctl restart apache2

Step 7: Run Laravel in a web browser

Finally, to access Laravel, refer to the Fully Qualified Domain Name (FQDN) or IP address of the server where you installed Laravel. If you do not specify a URL, Laravel will use the default URL. If you followed the steps correctly and the installation was successful, you will be presented with the Laravel page.
