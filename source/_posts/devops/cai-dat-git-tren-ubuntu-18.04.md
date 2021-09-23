---
title: Cài đặt git trên ubuntu 18.04
date: 2021-09-24 08:12:16
author: Đồng Nguyễn
tags: [CI / CD, Git]
categories:
- [DevOps]
---

Xin chào các bạn, hôm nay mình sẽ hướng dẫn các bạn cài git nhanh nhất đơn giản nhất, chúng ta sẽ cài version 14.x nhé, version này là bản stable nhất ở thời điểm hiện tại. Ok chúng ta bắt đầu thôi nào.
1. Đầu tiên các bạn hãy cài NodeSource PPA vào:
```angular2html
cd ~
curl -sL https://deb.nodesource.com/setup_14.x -o setup_14.sh
```
2. Tiếp theo sẽ thêm nó vào <Br>
`sudo sh ./setup_14.sh`
   

3. Bước thứ 3 là cài Nodejs
```angular2html
sudo apt update
sudo apt install nodejs
```

4. Kiểm tra kết quả nào<br>
```angular2html
node -v
npm -v
```
   
In ra màn hình thế này là ổn định rồi nhé

```angular2html
ubuntu@ip-xx-xx-xx-xx:~$ node -v
v14.17.6
ubuntu@ip-xx-xx-xx-xx:~$ npm -v
6.14.15
```

Thế là xong, đơn giản phải không các bạn, hy vọng nó sẽ k làm khó được các bạn.<br>
Hẹn gặp lại các bạn ở các bài viết tiếp theo.

Don't forget: <br>
`Whatever you do, do it with all your heart, your mind and your strength.`