## LAMP
A *LAMP* stack is a group of open-source software that is typically installed together to enable a server to host dynamic websites and web apps. This term is actually an acronym which represents the **L**inux operating system, with the **A**pache web server. The site data is stored in a **M**ySQL database, and dynamic content is processed by **P**HP.

## HTTP Server
Install the [apache](https://archlinux.org/packages/?name=apache) package. 
```
pacman -S apache
```
Apache configuration files are located in `/etc/httpd/conf`. The main configuration file is `/etc/httpd/conf/httpd.conf`, which includes various other configuration files. The default configuration file should be fine for a simple setup. By default, it will serve the directory `/srv/http` to anyone who visits your website.

To start Apache, start `httpd.service` using [systemd](https://wiki.archlinux.org/title/Systemd#Using_units). 

## Reference
* [Instalar PHPMyAdmin Manjaro (Install phpmyadmin in arch linux) - YouTube](https://www.youtube.com/watch?v=a4tXdznN5YE)
* [Arch Linux: The LAMP stack - YouTube](https://www.youtube.com/watch?v=GYnmm97bPxg)
* [Apache HTTP Server - ArchWiki](https://wiki.archlinux.org/title/Apache_HTTP_Server#PHP)
* [phpMyAdmin - ArchWiki](https://wiki.archlinux.org/title/PhpMyAdmin)
* [Linux安裝php和關於架網站 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10208287?sc=iThelpR)
