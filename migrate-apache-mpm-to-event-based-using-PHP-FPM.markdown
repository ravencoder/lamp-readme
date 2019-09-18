# Migrating Apache from Prefork to Event-based using PHP-FPM in CentOS 7

By default, Apache comes with the Prefork MPM which is Non-threaded, process-based 
and consumes more resources than the other ones like (Worker, Event).

This solution requires Apache version 2.4 or higher.

Update Apache configuration

Edit 00-mpm.conf under /etc/httpd/conf.modules.d:

```shell
sudo vim /etc/httpd/conf.modules.d/00-mpm.conf
```

Save and restart httpd:

```shell
sudo service httpd restart
```

To verify MPM is set to Event, run this command:

```shell
httpd -V | grep -i 'version\|mpm'
```

You should get:

```shell
Server version: Apache/2.4.37 (Amazon)
Server MPM:     event
```

Set PHP to use FastCGI

In order to take advantage of event MPM, PHP has to run under FastCGI, so install it first. 
Check the version of PHP that you’re running and install the right version:

```shell
yum list installed | grep php
```

Install the right PHP-FPM version, e.g. ius PHP-7.1.xx

```shell
yum install php71u-fpm -y
```
Update php.conf to use FastCGI:

```bash
sudo vim /etc/httpd/conf.d/php.conf
```

Comment out `SetHandler application/x-httpd-php` and add `SetHandler “proxy:fcgi://127.0.0.1:9000”`:

```bash
#SetHandler application/x-httpd-php
SetHandler "proxy:fcgi://127.0.0.1:9000"
```

Start PHP-FPM:

```bash
sudo service php-fpm start
```

Set it to auto-start on Startup:

```bash
systemctl enable php-fpm
```

Check if it is enabled properly

```bash
systemctl list-unit-files --state=enabled
```
