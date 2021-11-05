## LAMP
A *LAMP* stack is a group of open-source software that is typically installed together to enable a server to host dynamic websites and web apps. This term is actually an acronym which represents the **L**inux operating system, with the **A**pache web server. The site data is stored in a **M**ySQL database, and dynamic content is processed by **P**HP.

## HTTP Server
Install the [apache](https://archlinux.org/packages/?name=apache) package. 
```
pacman -S apache
```
Apache configuration files are located in `/etc/httpd/conf`. The main configuration file is `/etc/httpd/conf/httpd.conf`, which includes various other configuration files. The default configuration file should be fine for a simple setup. By default, it will serve the directory `/srv/http` to anyone who visits your website.

![](https://github.com/a22057916w/Studio/blob/1.0/.meta/LAMP/srv_http.png)

Search and comment out the following line in `/etc/httpd/conf/httpd.conf`:
```
#LoadModule unique_id_module modules/mod_unique_id.so
```

To start Apache, start `httpd.service` using [systemd](https://wiki.archlinux.org/title/Systemd#Using_units). 

## PHP
#### Install [PHP](https://wiki.archlinux.org/title/PHP#Installation)
```
pacman -S php 
```
### Extensions
#### Using libphp
It is suitable for a light request load. It also requires you to change the **mpm module** [**(Multi-Processing Module)**](https://dotblogs.com.tw/grayyin/2020/03/15/115350), which may cause problems with other extensions (e.g. it is not compatible with #HTTP/2).

Install `php7-apache` for PHP 7 or `php-apache`
```
pacman -S php-apache
```
In `/etc/httpd/conf/httpd.conf`, comment the line:
```
#LoadModule mpm_event_module modules/mod_mpm_event.so
```
and uncomment the line:
```
LoadModule mpm_prefork_module modules/mod_mpm_prefork.so
```
**Note**: The above is required, because libphp.so included with the package does not work with mod_mpm_event, but will only work mod_mpm_prefork instead.

To enable PHP, add these lines to `/etc/httpd/conf/httpd.conf`: 

![](https://github.com/a22057916w/Studio/blob/1.0/.meta/LAMP/php_extension.png)

Restart `httpd.service`. 
```
systemctl restart httpd
```
#### [gd](https://tw511.com/a/01/28847.html)
```
pacman -S php-gd
```
For [php-gd](https://archlinux.org/packages/?name=php-gd) uncomment the line in `/etc/php/php.ini`: 
```
extension=gd
```
#### MySQL/MariaDB
Install and configure MySQL/MariaDB as described in [MariaDB](https://wiki.archlinux.org/title/MariaDB) or refer to the tutorial in next section.

Uncomment the following lines in `/etc/php/php.ini`:
```
extension=pdo_mysql
extension=mysqli
```
**Note**: extension=mysql was removed in PHP 7.0.

## MySQL/MariaDB
[MariaDB](https://mariadb.com/) is a reliable, high performance and full-featured database server which aims to be an 'always Free, backward compatible, drop-in' replacement of [MySQL](https://wiki.archlinux.org/title/MySQL). Since 2013 MariaDB is Arch Linux's default implementation of MySQL.
```
pacman -S mariadb mariadb-clients mariadb-lib
```

## Reference
* [Apache HTTP Server - ArchWiki](https://wiki.archlinux.org/title/Apache_HTTP_Server#PHP)
* [PHP - ArchWiki](https://wiki.archlinux.org/title/PHP#Installation)
* [MariaDB - ArchWiki](https://wiki.archlinux.org/title/MariaDB)
* [phpMyAdmin - ArchWiki](https://wiki.archlinux.org/title/PhpMyAdmin)
* [Instalar PHPMyAdmin Manjaro (Install phpmyadmin in arch linux) - YouTube](https://www.youtube.com/watch?v=a4tXdznN5YE)
* [Arch Linux: The LAMP stack - YouTube](https://www.youtube.com/watch?v=GYnmm97bPxg)
* [Linux安裝php和關於架網站 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10208287?sc=iThelpR)
