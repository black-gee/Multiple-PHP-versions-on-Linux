# Install Web Server, MariaDB and Multiple PHP versions on Linux
**This guide is a fully detailed guide to install of several versions of PHP (php5.6, php7.4 and php8.1)**
I had installed multiple PHP version a long time ago. Thus a lots of commands might have been applied before and I may miss them. If I miss any instruction that causes issues, please put a comment, I will update.

The operating system used when I implemented multiple PHP versions was Ubuntu 23.04 with the last update in May 2024


## Add repository

    #sudo add-apt-repository ppa:ondrej/php
    #sudo add-apt-repository ppa:ondrej/apache2
    #sudo apt-get update
    #sudo apt-get dist-upgrade
    #sudo apt list --upgradable
    #sudo apt install software-properties-common
    
## Install Multiple PHP Version (php5.6, php7.4 and php8.1)

    #sudo apt-get install php5.6 php5.6-fpm php5.6-mysql php5.6-mbstring php5.6-xml php5.6-gd php5.6-curl
    #sudo apt install php7.4-{fpm,mysql,common,cli,json,opcache,readline,mbstring,xml,gd,curl,zip}
    #sudo apt install php8.1-{fpm,mysql,common,cli,opcache,readline,mbstring,xml,gd,curl,zip}
     
  Check Version 

    #php -v
        PHP 8.1.12-1ubuntu4.3 (cli) (built: Aug 17 2023 17:37:48) (NTS)
        Copyright (c) The PHP Group
        Zend Engine v4.1.12, Copyright (c) Zend Technologies
        with Zend OPcache v8.1.12-1ubuntu4.3, Copyright (c), by Zend Technologies

    
## install Web Server - APACHE2
  Because the script I use is for Ubuntu, of course you also need to use Ubuntu (or its family) to follow this tutorial. 
  And don't forget that Apache must also be installed. We can try checking first using command.
 
    #sudo apt install apache2
    #sudo systemctl start apache2
     
  Check Version 

     #apache2 -v
         Server version: Apache/2.4.55 (Ubuntu)
         Server built:   2023-10-26T13:37:01
  
  
## Install Database - MariaDB 

    #sudo apt update
    #sudo apt install mariadb-server
    #sudo systemctl start mariadb
    #sudo systemctl status mariadb
    #sudo mysql_secure_installation
     
  Check Version 

    #sudo mariadb -v
              Welcome to the MariaDB monitor.  Commands end with ; or \g.
              Your MariaDB connection id is 33
              Server version: 10.11.2-MariaDB-1 Ubuntu 23.04
    
## Switch PHP version

    #sudo update-alternatives --config php
                There are 3 choices for the alternative php (providing /usr/bin/php).
                
                  Selection    Path             Priority   Status
                ------------------------------------------------------------
                  0            /usr/bin/php8.1   81        auto mode
                  1            /usr/bin/php5.6   56        manual mode
                  2            /usr/bin/php7.4   74        manual mode
                * 3            /usr/bin/php8.1   81        manual mode
                
                Press <enter> to keep the current choice[*], or type selection number: 3
   
    #sudo systemctl restart apache2

  ## Restore to Normal
    
    #sudo a2dissite 000-default.conf
    #sudo systemctl restart apache2


## Change PHP Version
Make vhost.conf

    # sudo nano /etc/apache2/sites-available/vhost.conf


                    <VirtualHost *:80>
                        ServerName example.com
                        ServerAlias www.example.com
                        DocumentRoot /var/www/html
                     
                        <Directory /var/www/html>
                            Options -Indexes +FollowSymLinks +MultiViews
                            AllowOverride All
                            Require all granted
                        </Directory>
                     
                        <FilesMatch \.php$>
                            # 2.4.10+ can proxy to unix socket
                            SetHandler "proxy:unix:/var/run/php/php7.4-fpm.sock|fcgi://localhost"
                        </FilesMatch>
                     
                        ErrorLog ${APACHE_LOG_DIR}/error.log
                        CustomLog ${APACHE_LOG_DIR}/access.log combined
                    </VirtualHost>
The difference between using php7.4 or php8.0 or php8.1

                    PHP7.4
                    ---------------------------------------
                    <FilesMatch \.php$>
                    # 2.4.10+ can proxy to unix socket
                            SetHandler "proxy:unix:/var/run/php/php7.4-fpm.sock|fcgi://localhost"
                    </FilesMatch>
                    ---------------------------------------
                    PHP8.0
                    ---------------------------------------
                    <FilesMatch \.php$>
                    # 2.4.10+ can proxy to unix socket
                            SetHandler "proxy:unix:/var/run/php/php8.0-fpm.sock|fcgi://localhost"
                    </FilesMatch>
                    ---------------------------------------
                    PHP8.1
                    ---------------------------------------
                    <FilesMatch \.php$>
                    # 2.4.10+ can proxy to unix socket
                            SetHandler "proxy:unix:/var/run/php/php8.1-fpm.sock|fcgi://localhost"
                    </FilesMatch>
                    
                    
    # sudo #apachectl configtest
    # sudo a2ensite vhost
    # sudo nano /etc/apache2/sites-available/vhost.conf
    # sudo systemctl restart apache2

