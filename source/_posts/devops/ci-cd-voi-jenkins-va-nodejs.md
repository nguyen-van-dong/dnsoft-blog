---
title: CI, CD với Jenkins và source Nodejs
author: Đồng Nguyễn
tags: [CI / CD, Jenkins, NodeJS]
categories:
- [DevOps]
---
Ở loạt series này thì mình sẽ dùng hoàn toàn bằng aws, nên các bạn hãy tạo thêm một instance EC2 trên aws nữa để tạo webserver chạy Nodejs nhé.

Sau khi đã login vô con server EC2 rồi thì chúng ta sẽ cài nodejs, và git cho nó.
Cách cài 2 thằng này thì mình đã có bài viết hướng dẫn rồi nhé. [Link](https://dnsoft-blog.netlify.app/devops/cai-dat-git-tren-ubuntu-18.04.html)

### I. Setup webserver

Bước 1:

Đầu tiên chúng ta sẽ tạo một thư mục để chứa mã nguồn của chúng ta nhé. <br>
`sudo mkdir -p /app/my-node-app`

Bước 2:

Chúng ta sẽ clone source trên gitlab về đúng cái thư mục mà chúng ta vừa tạo.<br>
`git clone git@gitlab.com:dongbuh/nodejs-test.git /app/my-node-app`

Bước 3:

Mình sẽ tạo 1 user để chuyên phục vụ cho việc deploy code, nhớ là phải switch sang user `root` trước khi tạo user `www-user-node` nhé, tạo thêm password cho nó nữa. <br>
```angular2html
sudo su -
useradd www-user-node
passwd www-user-node
```
Cho user này có quyền truy cập vào thư mục `/app/my-node-app` nhé

`chown -Rf www-user-node:node /app/my-node-app`

`node` sau dấu `:` là group user nhé, có nghĩa là group `node` cũng có quyền truy cập vào thư mục này.
Nếu các bạn chưa có group `node` thì sẽ tạo group bằng lệnh `groupadd node` nhé.

Bây giờ chúng ta sẽ switch về user `www-user-node` ở trên bằng lệnh <br>
`sudo su - www-user-node`

Sau khi switch xong thì chúng ta sẽ tạo ssh key cho user này nhé <br>
`ssh-keygen -t rsa`

Các bạn cứ enter nhiệt tình vào nhé, đừng tạo password cho file này chứ không trong quá trình deploy nó báo lỗi đấy =))). 

Vẫn đang ở user này, chúng ta sẽ vào thư mục  `/home/www-user-node/.ssh` và đổi tên file `id_rsa.pub` thành `authorized_keys` cho dễ sử dụng.

Tiếp tục chúng ta sẽ cho quyền `600` cho file này nhé (quyền `600` là quyền gì thì các bạn lên mạng search sẽ ra nhé =))).

`chmod 600 authorized_keys`

>>Các bạn lưu ý kỹ chỗ này nha, không là tí mình remote từ server jenkins vào server node này nó bị lỗi permission đấy. 

Chúng ta vào lại thư muc `/app/my-node-app` bạn chạy `npm install` để nó cài đặt các dependencies của project chúng ta nhé.

Sau khi cài các dependencies xong thì bạn gõ `npm index.js` (giả sử file `index.js` là file chạy chính của bạn)

Sau đó ra ngoài trình duyệt gõ ip_address:port (ví dụ `13.229.51.248:4001`) để test thử coi nó chạy không nhé, dĩ nhiên là sau khi bạn stop node server thì sẽ không truy cập được.

> **Chú ý**: Nếu các bạn chưa mở port 4001 thì nó sẽ không start lên đâu nhé. Bởi vậy nên các bạn phải open port trên server của mình nha, nếu chưa biết open như thế nào thì bạn vào đây để xem hướng dẫn nè:
<a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html" target="_blank">Hướng dẫn Open port.</a>!

Đây là cái log ra màn hình mình lúc chạy `node index`: `Server is running on port 4001`

Ok, giờ chẳng lẽ chúng ta cứ phải vào server node để run nó lên bằng `node index` như vậy à ??
Không ổn rồi :D, phải có cách gì chứ, ok thắc mắc của các bạn là đúng rồi đấy =)))

Chúng ta sẽ dùng trình `pm2` để giúp mình xử lý chỗ này nha.

Các bạn chạy command ` npm install pm2@latest -g` để cài thằng pm2 này, chúng ta cài ở global nên ở phía sau có thêm `-g` đấy =)))

Sau khi cài xong thì mình vào thư mục chứa source chạy `pm2 start index.js`, nếu nó show ra như thế này thì là các bạn làm xong rồi đấy.

```angular2html
[PM2] PM2 Successfully daemonized
[PM2] Starting /app/my-node-app/index.js in fork_mode (1 instance)
[PM2] Done.
┌────┬────────────────────┬──────────┬──────┬───────────┬──────────┬──────────┐
│ id │ name               │ mode     │ ↺    │ status    │ cpu      │ memory   │
├────┼────────────────────┼──────────┼──────┼───────────┼──────────┼──────────┤
│ 0  │ index              │ fork     │ 0    │ online    │ 0%       │ 30.0mb   │
└────┴────────────────────┴──────────┴──────┴───────────┴──────────┴──────────┘
```

Ok, hiện tại server node của mình đã chạy mà không cần dùng `node index` rồi đấy, giờ còn tích hợp với jenkins nữa là chúng ta sẽ xong. Tiếp tục nào, rán lên bạn ưi =)))

## II. Setup jenkins

Chúng ta sẽ vào server jenkins và tạo 1 thư mục `jenkins` trong thư mục `home`, sau đó cd vào thư mục vừa tạo nha.

Bước tiếp theo chúng ta sẽ quay về server node các bạn copy private key. Bạn `cd /home/www-user-node/.ssh` và `cat id_rsa`,
sau đó copy toàn bộ nội dung file này và quay về server jenkins trong thư mục jenkins ở folder home và tạo 1 file pem để paste đống vừa copy vào
`vi web-server-node.pem`, sau đó change quyền cho nó là 400 thôi nha, `chmod 400 web-server-node.pem`.

Tiếp theo `cd ..` ra ngoài home để change quyền cho cả thư mục jenkins này luôn nha.

`chown -Rf jenkins:root jenkins/`

Tiếp theo các bạn switch qua user là `jenkins` bằng command `su - jenkins`.
Nếu không switch qua được thì các bạn vào `vi /etc/passwd` xuống cuối file và đổi thành `bash` cho user `jenkins` nhé, sau đó switch vào sẽ được.
Sau khi đổi xong thì chúng ta sẽ được như thế này.<br>
`jenkins:x:111:115:Jenkins,,,:/var/lib/jenkins:/bin/bash`

Ok, sau khi switch qua thì chúng ta sẽ remote vào server node bằng user ``jenkins`` này nhé

`ssh -i "/home/jenkins/web-server-node.pem" www-user-node@địa_chỉ_ip_web_server`

```angular2html
jenkins@ip-172-31-22-31:/home$ ssh -i "/home/jenkins/web-server-node.pem" www-user-node@13.229.51.248
Last login: Fri Sep 24 08:15:14 2021

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
[www-user-node@ip-172-31-37-247 ~]$
```
Thấy được như thế này là thơm rồi nhé :D, tức là đứng từ server jenkins đã có quyền truy cập vào web server của mình rồi, đương nhiên là nó muốn deploy thì nó phải có quyền chứ, đâu phải muốn truy cập là truy cập đâu, muốn ăn phải lăn vào bếp chứ đúng không nào :D, ok tiếp tục nhé =))).

Tiếp theo chúng ta sẽ tạo 1 webhook trên gitlab để thông báo cho jenkins khi có code mới được push lên.

Các bạn vào source trên gitlab của mình, sidebar bên trái bạn vào `webhook`.

.....
To be continue



Don't forget: <br>
`Whatever you do, do it with all your heart, your mind and your strength.`
