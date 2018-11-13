 Set SE Linux to permissive
```vi /etc/selinux/config    set SELINUX=permissive```

 Clear cache
```yum clean all```

 Update all libaries
```yum update -y```

 Restart server for some updates that requires it
```shutdown --reboot now```

 Add the epel repo
```
yum -y install epel-release
yum -y install https://rhel7.iuscommunity.org/ius-release.rpm
```

 Install PHP
```yum -y install php71u php71u-pdo php71u-mysqlnd php71u-opcache php71u-xml php71u-mcrypt php71u-gd php71u-devel php71u-mysql php71u-mbstring php71u-json php71u-soap```

 Install necessary libraries
```yum -y install mc wget bzip2 git gitflow```

 Install mariaDB 10.1
```yum -y install mariadb101u-server```

 Once the installation is complete, we start the daemon with the following command:
```systemctl start mariadb```

 systemctl doesnt display the outcome of all service management commands, so to be sure we succeeded, use the following command:
```systemctl status mariadb```

 Securing the MariaDB Server
```mysql_secure_installation```

 Install Composer using cURL
```curl -sS https://getcomposer.org/installer | php```

 make Composer globally accessible
```mv composer.phar /usr/local/bin/composer```

 ------------------------------------------------------

 Create user
```
useradd -m -p [password] -G adelphi [username]

e.g. useradd -p "fiqLj3&lbre" -m -G adelphi nmehra
```
 Assign groups
```usermod -a -G [group] [username]```


 ------------------------------------------------------

create project folder in /var/www/html
```
mkdir [projectname]
cd [projectname]
```

 clone codebase from bitbucket
```git clone git@bitbucket.org:adelphidigital/[projectname].git .```

 create mysql database
```
mysql> CREATE DATABASE [projectname];
mysql> GRANT ALL PRIVILEGES ON [projectname].* TO "[projectname]"@"localhost" IDENTIFIED BY "password";

GRANT ALL PRIVILEGES ON magento.* TO "magento"@"localhost" IDENTIFIED BY "villanueva";
```

 setup httpd - needs to be root
sudo su -
 
cd /etc/httpd/sites
```
vi [projectname].conf

 <VirtualHost *:80>
     ServerName www.wordpress.adelphidev
     ServerAlias wordpress.adelphidev
     DocumentRoot /var/www/html/wordpress

     Other directives here
    <Directory "/var/www/html/wordpress">
        Options Indexes FollowSymLinks MultiViews
        DirectoryIndex index.php index.html index.htm
        AllowOverride All
        Require all granted
    </Directory>
     ErrorLog logs/wordpress-error.log
     CustomLog logs/wordpress-access.log combined
 </VirtualHost>
```
 set /etc/hosts
```
vi /etc/hosts
127.0.0.1   www.wordpress.adelphidev
127.0.0.1   wordpress.adelphidev
```

 restart apache
```systemctl restart httpd```

 check status if OK
```systemctl status httpd```

 Run configuration file syntax test
```apachectl configtest```
