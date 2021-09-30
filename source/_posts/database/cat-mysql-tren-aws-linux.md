---
title: Cài đặt mysql trên aws linux
date: 2021-09-30 16:15:54
author: Đồng Nguyễn
tags: [Database, PHP]
categories:
- [Database]
---

Đầu tiên chúng sẽ update lại các package mà chúng ta đã cài.

`sudo yum update`

Trong aws linux mặc định có chứa gói cài đặt Mariadb rôi chúng ta chỉ cần móc ra và cài thôi. Ở bài này chúng ta sẽ cài version 5.7 cho ổn định nek.

`sudo rpm -Uvh https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm`

Tiếp theo chúng ta sẽ chạy <br>
`sudo yum install mysql-community-server`

Trong quá cài nó sẽ hỏi chúng ta là có cài các package khác không thì các bạn cứ gõ `y` nhiệt tình nhé.

Sau khi cài xong thì chúng ta sẽ enable nó lên bằng lệnh

`sudo systemctl enable mysqld`

Tiếp theo sẽ start nó nào

`sudo systemctl start mysqld`

Sau đó kiểm tra status của nó như thế nào nhé, bằng câu lệnh

`sudo systemctl status mysqld`

Nếu nó show ra thế này là chúng ta đã cài thành công bước đầu rồi đấy.
```angular2html
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2021-09-30 07:44:59 UTC; 7s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 11628 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid $MYSQLD_OPTS (code=exited, status=0/SUCCESS)
  Process: 11579 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 11633 (mysqld)
   CGroup: /system.slice/mysqld.service
           └─11633 /usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid

Sep 30 07:44:54 ip-172-31-24-136.ap-southeast-1.compute.internal systemd[1]: Starting MySQL Server...
Sep 30 07:44:59 ip-172-31-24-136.ap-southeast-1.compute.internal systemd[1]: Started MySQL Server.
```

Sau khi cài xong thì mặc định nó sẽ tạo cho chúng ta 1 cái root password, các bạn gõ command sau để xem

`sudo grep 'temporary password' /var/log/mysqld.log`

Của mình nó sẽ ra như thế này
```angular2html
2021-09-30T07:44:56.572227Z 1 [Note] A temporary password is generated for root@localhost: Deq.l3?:Y.-3
```

Tiếp theo chúng ta sẽ cài security cho mysql server nhé.

Các bạn gõ<br>
`sudo mysql_secure_installation`

Nó bảo mình nhập root password thì các bạn nhập vào nhé, như của mình là `Deq.l3?:Y.-3`.
Sau đó nó bảo nhập password mới, sau khi re-type xong thì nó hỏi mình là có chắc chắn thay đổi password hay không, các bạn gõ `y` để đồng ý.

Sau đó nó sẽ hỏi thêm 1 số cái nữa, các bạn cứ yes nhiệt tình nhé.

Ok, sau khi đã xong bước này thì chúng ta thử truy cập vào nha. <br>
`mysql -u root -p"Your password"`

Nếu nó show lên như thế này là ok.

```angular2html
[root@your_id_address]# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 5.7.35 MySQL Community Server (GPL)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

Ok, giờ mình sẽ check thử có bao nhiêu databases bằng command line `show databases`:
```angular2html
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql>
```

Các bạn có thể tạo mới database bằng `CREATE DATABASE you_db_name`

```angular2html
mysql> CREATE DATABASE x_wallet_api;
Query OK, 1 row affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| x_wallet_api       |
+--------------------+
5 rows in set (0.00 sec)
```
Chúng ta vừa mới tạo ra database là x_wallet_api, giờ các bạn hãy use nó để tạo table nhé.
`use x_wallet_api;`
Chúng ta kiểm tra thử có table nào chưa bằng: `show tables;`
```angular2html
mysql> use x_wallet_api;
Database changed
mysql> show tables;
Empty set (0.00 sec)

mysql>
```
Database của mình chưa có table nào nên nó show ra `Empty set` như vậy, nếu có thì nó sẽ show ra tất cả tables.

Ok, chúng ta đã xong mọi thứ, giờ các bạn thích tạo database hoặc tạo table gì đó thì cứ thoải mái nhé.

Xin chào và hẹn gặp lại các bạn ở bài viết tiếp theo.

And<br>
Don't forget: <br>
`Whatever you do, do it with all your heart, your mind and your strength.`
