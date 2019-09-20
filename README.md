Set SE Linux to permissive

```bash
vi /etc/selinux/config    # set SELINUX=permissive
```

Set SELinux as permissive without restarting

```setenforce 0```

Clear cache

```yum clean all```

Update all libaries

```yum update -y```

Update timezone, check timezone format

```bash
timedatectl list-timezones
```

Set timezone

```timedatectl set-timezone [time_zone]```

Restart server for some updates that requires it

```bash
shutdown --reboot now
```


Add the EPEL repository

```bash
yum -y install epel-release

Install the IUS repository

```bash
yum -y install https://rhel7.iuscommunity.org/ius-release.rpm
```


Install PHP

```bash
yum -y install php71u php71u-pdo php71u-mysqlnd php71u-opcache php71u-xml php71u-mcrypt php71u-gd php71u-devel php71u-mbstring php71u-json php71u-soap
```

Install necessary libraries

```yum -y install mc wget bzip2 git gitflow```

Install Yum replace plugin: https://github.com/iuscommunity/yum-plugin-replace

This plug-in provides a yum replace command that replaces a specified package and installs any required dependencies at the same time.

```yum install yum-plugin-replace```

------------------------------------------------------

Installing Database as on premise

Install mariaDB 10.1

```yum -y install mariadb101u-server```

Once the installation is complete, we start the daemon with the following command:

```systemctl start mariadb```

Systemctl doesnt display the outcome of all service management commands, so to be sure we succeeded, use the following command:

```systemctl status mariadb```

Securing the MariaDB Server

```mysql_secure_installation```

Enable mariadb to run on Startup

```systemctl enable mariadb```

------------------------------------------------------

Installing Composer

Install Composer using cURL

```bash
curl -sS https://getcomposer.org/installer | php
```

Make Composer globally accessible

```mv composer.phar /usr/local/bin/composer```

------------------------------------------------------

Create user

```useradd -m -p[password] -G [group] [username]```

 Assign groups

```usermod -a -G [group] [username]```


------------------------------------------------------

Create project folder in /var/www/html

```bash
mkdir [projectname]
cd [projectname]
```

Clone codebase from bitbucket

```bash
git clone git@bitbucket.org:[company]/[projectname].git .
```

Create mysql database

```sql
CREATE DATABASE [projectname];
GRANT ALL PRIVILEGES ON [projectname].* TO "[projectname]"@"localhost" IDENTIFIED BY "password";
```
 
Make the necessary \*.conf changes for HTTPD

```bash
cd /etc/httpd/sites

vi [projectname].conf

 <VirtualHost *:80>
     ServerName [projectname]
     ServerAlias [projectname]
     DocumentRoot /var/www/html/[projectname]

     Other directives here
    <Directory "/var/www/html/[projectname]">
        Options Indexes FollowSymLinks MultiViews
        DirectoryIndex index.php index.html index.htm
        AllowOverride All
        Require all granted
    </Directory>
     ErrorLog logs/[projectname]-error.log
     CustomLog logs/[projectname]-access.log combined
 </VirtualHost>
```

Restart apache

```systemctl restart httpd```

Check status if OK

```systemctl status httpd```

Run configuration file syntax test

```apachectl configtest```

Enable httpd to run on Startup

```systemctl enable httpd```
