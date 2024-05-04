# Install Web Server, MariaDB and Multiple PHP versions on Linux
**This guide is a fully detailed guide to install of several versions of PHP (php5.6, php7.4 and php8.1)**
I had installed multiple PHP version a long time ago. Thus a lots of commands might have been applied before and I may miss them. If I miss any instruction that causes issues, please put a comment, I will update.

The operating system used when I implemented multiple PHP versions was Ubuntu 23.04 with the last update in May 2024


## Add repository

https://launchpad.net/%7Eondrej/+archive/ubuntu/php
https://launchpad.net/~ondrej/+archive/ubuntu/apache2

    #sudo add-apt-repository ppa:ondrej/php
    #sudo add-apt-repository ppa:ondrej/apache2
    #sudo apt-get update
    #sudo apt-get dist-upgrade
    #sudo apt list --upgradable
    #sudo apt install python-software-properties
    
## Install Multiple PHP Version (php5.6, php7.4 and php8.1)

    #sudo apt-get install php7.4 php7.4-fpm php7.4-mysql php7.4-mbstring php7.4-xml php7.4-gd php7.4-curl
    #sudo apt-get install php5.6 php5.6-fpm php5.6-mysql php5.6-mbstring php5.6-xml php5.6-gd php5.6-curl
    #sudo apt-get install php8.1 php8.1-cli php8.1-common \
                          php8.1-zip php8.1-gd php8.1-mbstring php8.1-tokenizer \
                          php8.1-curl php8.1-xml php8.1-bcmath php8.1-xml \
                          php8.1-intl php8.1-sqlite3 php8.1-mysql
     
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

