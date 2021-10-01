---
title: Cài đặt jenkins trên ubuntu 18.04
date: 2021-09-23 11:02:46
author: Đồng Nguyễn
tags: [CI / CD, Jenkins, Ubuntu 18.04]
thumbnail: 1.jpg
categories:
- [DevOps]
---

## Nội dung

Xin chào 500 anh em =))), chuyện là mình được giao một cái task liên quan tới CI CD nên mình nhảy vô tìm hiểu thằng jenkins. Sau thời gian vọc thì mình muốn chia sẻ đến các bạn thằng khứa này. Chúng ta sẽ học cách cài đặt jenkins trên ubuntu để làm CI (Continuous Integration), CD (Continuous Deployment).
Trong phần tiếp theo thì mình sẽ hướng dẫn cách tích hợp jenkins với dự án nodejs và laravel nhé. Ok, bắt đầu thôi nào.

Chuẩn bị:
- Đương nhiên là một con VPS server rồi, các bạn có thể đăng ký 1 tài khoản và dùng free trên aws (dùng nhiều sẽ bị tính phí đấy nhé :D ). Mình sẽ không hướng dẫn tạo thế nào nhé, mặc định là các bạn đã có rồi nhen.
Mình sẽ dùng con Server EC2 với hệ điều hành ubuntu, các bạn dùng OS nào cũng được miễn là quen là quất thôi =)).
- Sau khi có VPS server rồi thì các bạn login vào nhé, dùng file .pem để login vào bằng câu lệnh: <br>
`ssh -i "path_file_pem" ubuntu@your_public_address` <br>

Sau khi đã login vào rồi thì chúng ta tiến hành các bước sau: <br>

Bước 1:
Cài đặt java sdk, mình cài java 8 nhé, java 11 nó không support thằng ruby runtime khi cài plugin cho gitlab nên mình dùng java 8 cho ổn định nhé.<br/>
1. `sudo apt update` <br>
2. `sudo apt install openjdk-8-jdk openjdk-8-jre`
- Sau khi cài xong thì chúng ta kiểm tra nhẹ thử đã thành công chưa bằng câu lệnh<br>
  `java -version`
- Nếu log ra màn hình tựa tựa như thế này thì đã ok nhé.
```
openjdk version "1.8.0_252"
OpenJDK Runtime Environment (build 1.8.0_252-8u252-b09-1ubuntu1-b09)
OpenJDK 64-Bit Server VM (build 25.252-b09, mixed mode)
```

Bước 2:
Sau khi cài java xong thì chúng ta sẽ cài jenkins, cái này mới chính nek =)) <br>
Các bạn sẽ gõ lần lượt các câu lệnh sau:
1. `wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -`
2. `sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'`
3. `sudo apt update`
4. `sudo apt install jenkins -y`

Sau khi chạy xong câu lệnh thứ 4 thì chúng ta sẽ start jenkins lên: <br>
`sudo systemctl start jenkins` <br>
Kiểm tra status của jenkins
`sudo systemctl status jenkins`
```
● jenkins.service - LSB: Start Jenkins at boot time
   Loaded: loaded (/etc/init.d/jenkins; generated)
   Active: active (exited) since Wed 2021-09-22 13:56:05 +07; 20h ago
     Docs: man:systemd-sysv-generator(8)
    Tasks: 0 (limit: 4915)
   CGroup: /system.slice/jenkins.service

Thg 9 22 13:56:03 dong systemd[1]: Starting LSB: Start Jenkins at boot time...
Thg 9 22 13:56:03 dong jenkins[1997]: Correct java version found
Thg 9 22 13:56:03 dong jenkins[1997]:  * Starting Jenkins Automation Server jenkins
Thg 9 22 13:56:03 dong su[2181]: Successful su for jenkins by root
Thg 9 22 13:56:03 dong su[2181]: + ??? root:jenkins
Thg 9 22 13:56:03 dong su[2181]: pam_unix(su:session): session opened for user jenkins by (uid=0)
Thg 9 22 13:56:04 dong su[2181]: pam_unix(su:session): session closed for user jenkins
Thg 9 22 13:56:05 dong jenkins[1997]:    ...done.
Thg 9 22 13:56:05 dong systemd[1]: Started LSB: Start Jenkins at boot time.
```

Nếu thấy status đang là `active` thì chúng ta đã cài jenkins thành công rồi đấy, tuy nhiên, còn vài bước nhỏ nhỏ nữa.

Mặc định thì jenkins sẽ chạy port `8080` nên nếu server của bạn chưa mở port này lên thì các bạn phải mở nó lên nhé
1. `sudo ufw allow OpenSSH`
2. `sudo ufw enable`
3. `sudo ufw allow 8080` <br>
Hoặc các bạn cũng có thể mở bằng cơm (thủ công) bằng cách vào server của mình để mở nó ra nhen.

Ok, thế là xong rồi đấy, các bạn vào trình duyệt gõ: <br>
`your_domain_on_aws:8080` <br>

Ở đây các bạn sẽ thấy nó bảo mình nhập password, nó có để đường dẫn để các bạn vào lấy, mình dùng `vim` hoặc `nano` để vào lấy nhé.
Nếu các bạn chưa biết dùng `vim` hoặc `nano` thì lên mạng search cách sử dụng cơ bản nhen :D.

Sau khi nhập password đúng thì sẽ đến phần tạo tài khoản, ở đây các bạn cứ nhiệt tình tạo tài khoản gì tùy thích nhé. <br>
Xong rồi nhé :D. <br>
Hẹn gặp lại các bạn ở bài viết tiếp theo về cách tích hợp với source Nodejs và laravel nhé.
Cảm ơn các bạn đã kiên nhẫn đọc tới đây :D

Don't forget: <br>
`Whatever you do, do it with all your heart, your mind and your strength.`