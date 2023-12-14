---
{"dg-publish":true,"permalink":"/博客/宝塔面板与xui共存开启cdn教程/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-12-14T14:43:24.000+08:00","updated":"2023-12-14T22:50:25.754+08:00"}
---

### 1. 安装宝塔面板

- 到宝塔官网选取相应系统版本一键安装脚本[点此跳转]

### 2. X-UI面板安装及配置

- 这里采用一键脚本进行X-UI面板的安装：

```bash
bash <(curl -Ls [https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh](https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh))
```

- 注意：此面板运行端口为54321，需要在宝塔面板中放行该端口
- 安装完成后，通过IP:54321便可以访问对应的面板程序
- 进入面板后，选择入站规则，点击+号，添加新的节点，端口任意，协议可以是vmess也可以是vless，传输为ws，路径自己设置，但是格式不能改变，tls这里要关闭，节点配置信息如下图所示：

![https://jihulab.com/images1/tp/-/raw/main/xraycdn/x1.png](https://jihulab.com/images1/tp/-/raw/main/xraycdn/x1.png)

### 3. 宝塔面板创建站点

- 在宝塔面板中选择：站点-添加站点!

![https://jihulab.com/images1/tp/-/raw/main/xraycdn/x2.png](https://jihulab.com/images1/tp/-/raw/main/xraycdn/x2.png)

- 域名填写解析的域名地址，PHP版本这里一定要选择纯静态。开启SSL证书申请，采用Let’s Encrypt并开启强制HTTPS。

![https://jihulab.com/images1/tp/-/raw/main/xraycdn/x3.png](https://jihulab.com/images1/tp/-/raw/main/xraycdn/x3.png)

- 创建站点后，选择其配置文件进行编辑，编辑内容如下：

```bash
# /spt2/是节点中设置的路径信息，做对应修改即可
location /spt2/ {
        proxy_redirect off;
        proxy_pass http://127.0.0.1:57715;# 57715是添加节点的端口号
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $http_host;
        proxy_read_timeout 300s;
        # Show realip in v2ray access.log
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
```

- 上述内容添加到配置文件中最后一个大括号的前面，然后保存配置，并在宝塔面板中重启Nginx服务。

### 4. 节点添加和使用

经过上述设置后，节点已经可以通过在X-UI中扫码添加的方式进行使用，但是需要修改以下几个内容：

- 端口改为443
- 访问地址改为修改的域名 修改成自己的域名
- 路径信息改为/spt2/
- TLS选项打开

