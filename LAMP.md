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

MariaDB is the [default implementation](https://archlinux.org/news/mariadb-replaces-mysql-in-repositories/) of MySQL in Arch Linux, provided with the [mariadb](https://archlinux.org/packages/extra/x86_64/mariadb/) package. 
```
systemctl stop mysqld
pacman -S mariadb libmariadbclient mariadb-clients
systemctl start mysqld
mysql_upgrade -p

```

Run the following command before starting the `mariadb.service` or `mysqld`
```
mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```
### Configuration
Once you have started the MySQL server and added a root account, you may want to change the default configuration.

To log in as `root` on the MySQL server, use the following command: 
```
mysql -u root -p
```
#### Add user
Creating a new user takes two steps: create the user; grant privileges. In the below example, the user monty with some_pass as password is being created, then granted full permissions to the database mydb: 
```
# mysql -u root -p

MariaDB> CREATE USER 'monty'@'localhost' IDENTIFIED BY 'some_pass';
MariaDB> GRANT ALL PRIVILEGES ON mydb.* TO 'monty'@'localhost';
MariaDB> FLUSH PRIVILEGES;
MariaDB> quit

```
### Security
#### Improve initial security
The `mysql_secure_installation` command will interactively guide you through a number of recommended security measures, such as removing anonymous accounts and removing the test database:
```
mysql_secure_installation
```

*Warning*: After running this, please note that TCP port 3306 will still be open, but refusing connections with an error message. To prevent MySQL from listening on an external interface, see the [#Listen only on the loopback address](https://wiki.archlinux.org/title/MariaDB#Listen_only_on_the_loopback_address) and [#Enable access locally only via Unix sockets sections](https://wiki.archlinux.org/title/MariaDB#Enable_access_locally_only_via_Unix_sockets).

#### Listen only on the loopback address
By default, MySQL will listen on the 0.0.0.0 address, which includes all network interfaces. In order to restrict MySQL to listen only to the loopback address, add the following line in `/etc/my.cnf.d/server.cnf`:
```
[mysqld]
bind-address = 127.0.0.1
```

## Reference
* [Apache HTTP Server - ArchWiki](https://wiki.archlinux.org/title/Apache_HTTP_Server#PHP)
* [PHP - ArchWiki](https://wiki.archlinux.org/title/PHP#Installation)
* [MariaDB - ArchWiki](https://wiki.archlinux.org/title/MariaDB)
* [phpMyAdmin - ArchWiki](https://wiki.archlinux.org/title/PhpMyAdmin)
* [Instalar PHPMyAdmin Manjaro (Install phpmyadmin in arch linux) - YouTube](https://www.youtube.com/watch?v=a4tXdznN5YE)
* [Arch Linux: The LAMP stack - YouTube](https://www.youtube.com/watch?v=GYnmm97bPxg)
* [Linux安裝php和關於架網站 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10208287?sc=iThelpR)
