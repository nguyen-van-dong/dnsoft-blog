---
title: Cài đặt jenkins trên ec2 aws linux
date: 2021-10-01 15:00:16
author: Đồng Nguyễn
tags: [CI / CD, Jenkins]
categories:
- [DevOps]
---

Xin chào các bạn, hôm nay mình sẽ hướng dẫn các bạn cài jenkins trên ec2 aws linux nhé.

Trước khi cài thì các bạn chạy `sudo yum update` phát cho nó update lại các package trong server của mình nhé.

Đầu tiên chúng ta sẽ cài java. Phải có thằng này để tạo môi trường chạy cho jenkins nhé.
`sudo yum install java-1.8.0`

Kiểm tra java đã được cài xong chưa bằng lệnh `java -version`.  Nếu ra như dưới đây thì các bạn đã cài ổn rồi đấy.
```angular2html
openjdk version "1.8.0_302"
OpenJDK Runtime Environment (build 1.8.0_302-b08)
OpenJDK 64-Bit Server VM (build 25.302-b08, mixed mode)
```
Sau khi có môi trường rồi thì chúng ta tiếp tục thêm repo Jenkins vào nhé. <br>
`sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo`

Import một key file từ Jenkins-CI để enable cài đặt từ package:
`sudo rpm — import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key`

Tiếp theo chúng ta sẽ cài Jenkins

`sudo yum install jenkins -y`

Nếu ở đây các bạn cài bị lỗi như vầy thì cứ bình tĩnh xem tiếp nhé
```angular2html
jenkins/primary_db                                                                                                       | 175 kB  00:00:00     
Resolving Dependencies
--> Running transaction check
---> Package jenkins.noarch 0:2.314-1.1 will be installed
--> Processing Dependency: daemonize for package: jenkins-2.314-1.1.noarch
--> Finished Dependency Resolution
Error: Package: jenkins-2.314-1.1.noarch (jenkins)
           Requires: daemonize
 You could try using --skip-broken to work around the problem
 You could try running: rpm -Va --nofiles --nodigest
```

Các bạn fix lỗi đó bằng cách chạy lần lượt các câu lệnh này nhé
```angular2html
1. sudo amazon-linux-extras install epel -y 
2. sudo yum update -y 
3. sudo yum install jenkins java-1.8.0-openjdk-devel 
```

Sau khi cài xong thì chúng ta sẽ start jenkins lên: <br>
`sudo systemctl start jenkins` <br>
Kiểm tra status của jenkins<br>
`sudo systemctl status jenkins`

Nếu thấy status là `active` nhừ zầy thì là ok rồi.
```
[root@ip-172-31-89-236 ~]# sudo systemctl status  jenkins
● jenkins.service - LSB: Jenkins Automation Server
   Loaded: loaded (/etc/rc.d/init.d/jenkins; bad; vendor preset: disabled)
   Active: active (running) since Fri 2021-10-01 08:07:19 UTC; 11s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 4868 ExecStart=/etc/rc.d/init.d/jenkins start (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/jenkins.service
           └─4872 /etc/alternatives/java -Djava.awt.headless=true -DJENKINS_HOME=/var/lib/jenkins -jar /usr/lib/jenkins/jenkin...

Oct 01 08:07:19 ip-172-31-89-236.ec2.internal systemd[1]: Starting LSB: Jenkins Automation Server...
Oct 01 08:07:19 ip-172-31-89-236.ec2.internal jenkins[4868]: Starting Jenkins [  OK  ]
Oct 01 08:07:19 ip-172-31-89-236.ec2.internal systemd[1]: Started LSB: Jenkins Automation Server.
```
Các bạn vào trình duyệt gõ: <br>
`your_domain_on_aws:8080` <br>

Nhớ là các bạn phải mở port `8080` lên nhé, không là nó sẽ không start đâu.

Ở đây các bạn sẽ thấy nó bảo mình nhập password, nó có để đường dẫn để các bạn vào lấy, mình dùng `vim` hoặc `nano` để vào lấy nhé.
Nếu các bạn chưa biết dùng `vim` hoặc `nano` thì lên mạng search cách sử dụng cơ bản nhen :D.

Sau khi nhập password đúng thì sẽ đến phần tạo tài khoản, ở đây các bạn cứ nhiệt tình tạo tài khoản gì tùy thích nhé. <br>
Xong rồi nhé :D. Cảm ơn các bạn đã kiên nhẫn đọc tới đây :D

Don't forget: <br>
`Whatever you do, do it with all your heart, your mind and your strength.`
