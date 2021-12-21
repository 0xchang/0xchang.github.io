---
title: centos7安装php
date: 2021-11-24 17:30:07
tags: 
    - centos7
    - php
---

## centos7安装php

环境是apache2+mariadb+php

先安装php

```shell
[root@cn7]/# yum -y install php
```

安装php相关组件

```shell
[root@cn7]/# yum -y install php-mysql php-gd php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-bcmath php-mhash
[root@cn7]/# systemctl restart mariadb
[root@cn7]/# systemctl restart httpd
```

通过php -v 查看信息

```shell
[root@cn7]/var/www/html# php -v
PHP 5.4.16 (cli) (built: Apr  1 2020 04:07:17)
Copyright (c) 1997-2013 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2013 Zend Technologies
```

进入/var/www/html目录编写一个测试页面

```shell
[root@cn7]/var/www/html# vim index.php
<?php
phpinfo();
?>
```

最后访问页面，有php相关信息成功
