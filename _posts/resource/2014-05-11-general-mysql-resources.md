---
layout: post
title: MySQL常用资源
category: 资源
tags: MySQL
keywords: MySQL
description: 
---

## 常用命令

### ssh隧道访问
    ssh -NCPf root@192.168.1.1 -L 7777:127.0.0.1:3306
    mysql -uroot -proot -h 127.0.0.1 -P 7777
    将192.168.1.1的3306端口映射到本地7777端口，C压缩 P用一个非特权端口进行出去的连接 f隧道后台运行 N不执行远程命令

### 登录数据库
    
    mysql -h localhost -uroot -p

### 导出数据库
    
    mysqldump  -h 192.168.1.1 -uroot -proot --databases testdb -n  --default-character-set='utf8mb4' --set-charset --skip-add-locks |gzip >./testdb-`date +"%Y-%m-%d-%H-%M-%S"`.sql.tgz

### 导入数据库
    
    mysql -uroot -p db < db.sql
    // or
    mysql -uroot -p db -e "source /path/to/db.sql"

### 开启远程登录
    
    GRANT ALL PRIVILEGES ON *.* TO 'user'@'%' IDENTIFIED BY 'pwd' WITH GRANT OPTION;
    FLUSH PRIVILEGES;
    vim /etc/mysql/my.conf
    #bind-address = 127.0.0.1
    
### 创建用户
    
    CREATE USER 'test'@'localhost' IDENTIFIED BY 'password';
    grant all privileges on *.* to test@'localhost' identified by 'test';
    
### 创建表
    
    CREATE SCHEMA testdb DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;

### 赋予数据库权限

    GRANT ALL ON testdb.* TO 'test'@'localhost';

