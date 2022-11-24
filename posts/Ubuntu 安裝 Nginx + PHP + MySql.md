---
title: Ubuntu 安裝 Nginx + PHP + MySql
description: Ubuntu 環境建置
date: 2022-11-11
tags:
    - Ubuntu
    - Nginx
    - PHP
    - MySql
layout: layouts/post.njk
image: /img/posts/php_nginx_mysql_ubuntu.png
---

### 前言
因為一些需求，需要有個環境來執行 PHP，那就自己搞一個吧！

### 環境
使用 **Hyper-V** 安裝 **[Ubuntu Server 22.04 LTS](https://www.ubuntu-tw.org/modules/tinyd0/)**

### PHP
- 新版安裝
```bash
    sudo apt-get updae
    sudo apt-get install php php-fpm php-mysql
```

- 舊版安裝
可以依據想安裝的版本，修改版本號，以下以5.6範例
```bash
    sudo add-apt-repository ppa:ondrej/php
    sudo apt-get update
    sudo apt-get install php5.6 php5.6-fpm php5.6-mysql php5.6-gd
```

- 如何徹底刪除PHP？
```bash
    sudo apt-get purge 'php*'
```

### Nginx
1. 安裝
```bash
sudo apt-get update
sudo apt-get install nginx
```
    
2. 設定nginx的default
使用 vim 修改
```bash
sudo vim /etc/nginx/sites-available/default
```

將 location  的部分，參考以下修改  
fastcgi_pass 參考 /etc/php/5.6/fpm/pool.d/www.conf (listen)
```bash
location ~ \.php$ {
            try_files $uri =404;
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass unix:/run/php/php5.6-fpm.sock;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
        }
```

儲存之後重啟 nginx
```bash
sudo service nginx restart
```

# MySql
1. 安裝
```bash
sudo apt update && sudo apt install mysql-server
```

2. [設定安全性（Security）](https://ui-code.com/archives/293)
```bash
sudo mysql_secure_installation
```

3. 新增使用者及建立資料庫權限
```bash
    # 在指令模式用 MySQL 的 root 帳號連接到 MySQL
    mysql -u root -p

    # 建立資料庫
    mysql> CREATE DATABASE 'newdatabase';

    # 建立 MySQL 新帳號
    mysql> CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'newpassword';

    # 建立 MySQL 新帳號 (可使用密碼連線登入)
    mysql> CREATE USER 'newuser'@'localhost' IDENTIFIED WITH mysql_native_password BY 'newpassword'

    # 刷新配置启用
    mysql> FLUSH PRIVILEGES;

    # 後給予新帳號 “newuser” 權限讀寫新資料庫 “newdatabase”:
    mysql> GRANT ALL PRIVILEGES ON newdatabase.* TO 'newuser'@'localhost';
```

4. 修改默认加密方式为 mysql_native_password
```bash
    #末尾增加 default_authentication_plugin=mysql_native_password
    sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf

    # 然後重新啟動 MySQL:
    systemctl restart mysql
```