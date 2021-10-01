---
title: Làm thế nào để lấy lại password admin trên jenkins
date: 2021-10-01 16:16:41
author: Đồng Nguyễn
tags: [CI / CD, Jenkins]
categories:
- [DevOps]
---

Xin chào tất cả các bạn, chuyện là hôm nay mình bị quên cái password admin login vào jenkins. Sau thời gian search google thì mình đã lấy lại được
nên hôm nay mình sẽ chia sẻ lại cho anh em bị giống mình =)))

Mình sẽ làm trên môi trường là ec2 aws linux nhé.

Đầu tiên các bạn login vào server. Sau đó các bạn gõ lệnh sau để disable chức năng đăng nhập bằng tài khoản bình thường.

`sudo vi /var/lib/jenkins/config.xml`

Ở trong file này các bạn sẽ thấy 1 dòng là

```
<useSecurity>true</useSecurity>
```
Sau đó các bạn đổi giá trị `true` => `false`

Restart lại jenkins một phát. <br>
` sudo service jenkins restart`

Ra ngoài browser gõ url của các bạn thì nó sẽ auto login vào luôn mà không yêu cầu nhập thông tin tài khoản.

Sau khi vào được dashboard rồi thì các bạn vào <strong>Manage Jenkins</strong> => <strong>Configure Global Security</strong>. Ở section `Security Realm` các chọn select cái `Jenkins’ own user database`, sau đó `Apply` và `Save`.
Phía dưới `Configure Global Security.` thì các bạn vào <strong>Manage Users</strong>, chọn user mà bạn muốn edit.
Bên trái các bạn chọn `Configure`, kéo xuống phía dưới sẽ thấy phần nhập password. Các bạn cứ sửa cách nhiệt tình nhé =))). Sau đó save lại là ok.

Chúc các bạn thành công !!!
Link youtube cho bạn nào cần sống động =))): https://www.youtube.com/watch?v=tm69qZXYzRU

Don't forget: <br>
`Whatever you do, do it with all your heart, your mind and your strength.`