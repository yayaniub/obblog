---
{"dg-publish":true,"permalink":"/Digital-Garden/利用Cloudflare tunnel搭建远程web ssh/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-12-11T00:38:54.733+08:00","updated":"2023-12-13T23:45:59.957+08:00"}
---

#### 1.安装cloudflared

- 打开cloudflare zero trust界面左侧栏的Access- tunnel-create a tunnel

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/15201113.png)
![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/15201151.png)


- 选择适合自己操作系统的安装命令安装cloudflared

- 创建tunnel隧道选择自己喜欢的域名，type选择ssh，url填写内网IP加22端口


![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/15201237.png)

#### 2.搭建远程webssh
 - 回到cloudflare zero trust界面，点击左侧栏的Access-Application-Add an application,
 选择Self-hosted
 
![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/1520134.png)

- 输入你喜欢的名称填写刚才在创建隧道域名，其他保持默认并点击下一步

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/15201322.png)

- 添加一个规则，规则名可以随便写，规则配置我选择邮件登录验证,其他保持默认并点击下一步

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/15201343.png)

- 添加设置项选择ssh其他不需要填写最后点击Add application即可完成配置

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/1520145.png)

#### 3.登录web ssh

- 在浏览器输入刚才创建的cloudflared tunnel地址

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/15201428.png)

- 输入在第二步设置的邮箱，填写验证码

![](https://jihulab.com/images1/blog/-/raw/main/pictures/2023/08/15201437.png)

- 最后填写你的ssh用户名和密码，就可以登录webssh了
#### 4.最后

通过这种方式远程登录自己的服务器，在可以不开放22端口也可以远程操作，同时通过邮箱

验证加ssh账号密码的方式认证极大提高了安全性