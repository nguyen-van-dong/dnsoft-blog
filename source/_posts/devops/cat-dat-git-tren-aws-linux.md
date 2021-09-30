---
title: Cài đặt nodejs và git trên aws linux
date: 2021-09-30 15:12:16
author: Đồng Nguyễn
tags: [CI / CD, Git, NodeJS]
categories:
- [DevOps]
---

Xin chào 500 anh em. Nay mình sẽ hướng dẫn các bạn cài Nodejs và Git trên aws linux nhé.
Cài đặt khá dễ dàng, các bạn có thể cài 1 trong 2 version dưới đây.

## Cài version mới nhất của Nodejs

```angular2html
sudo yum install -y gcc-c++ make 
curl -sL https://rpm.nodesource.com/setup_16.x | sudo -E bash - 
```

## Cài version ổn định nhất

```angular2html
sudo yum install -y gcc-c++ make 
curl -sL https://rpm.nodesource.com/setup_14.x | sudo -E bash - 
```
Việc còn lại là chạy `sudo yum install -y nodejs` để cài NodeJS

Chúng ta sẽ tiếp tục cài git bằng các command sau:

```angular2html
sudo yum update -y
sudo yum install git -y
```

Sau khi chạy xong 2 command line trên thì kiểm tra lại tất cả xem thế nào nhé.

```angular2htmlv
node -v
git --version
npm -v
```
Nếu ra như bên dưới là ok rồi đấy
```angular2html
v14.18.0
git version 2.32.0
6.14.15
```

Bonus: cài yarn trên aws linux

Nếu các bạn chưa biết yarn là gì thì có thể hiểu nó đơn giản giống như là npm (node packages module) vậy đó. Ok, chỉ 1 câu lệnh thôi lâ chúng ta có thể cài nó được rồi.

`sudo npm install yarn -g`

Kiểm tra xem đã cài ok chưa.

`yarn -v`

Nếu log ra như bên dưới thì chúng ta đã ok rồi
```angular2html
1.22.15
```

Hẹn gặp lại các bạn ở bài viết tiếp theo. Nếu có thắc gì các bạn cứ nhiệt tình hỏi mình nhé. =)))

And

Don't forget: <br>
`Whatever you do, do it with all your heart, your mind and your strength.`
