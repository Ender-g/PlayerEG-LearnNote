# 将阿里云域名DNS解析改为Cloudflare第三方DNS解析

## 教程概述

> 本教程介绍如何将阿里云域名DNS解析改为Cloudflare第三方DNS解析。
>
> 最终实现以下效果：
>
> [![效果](../img/p002/p0.png)](https://test.playereg.top/)
>
> 点击图片可以进入测试页面demo
>
> 成功使用Cloudflare的Pages服务部署测试网页，并且能够通过你的域名访问

## Cloudflare简介

> *Cloudflare*是一家**美国的跨国科技企业**，总部位于旧金山，在英国伦敦亦设有办事处。Cloudflare以向客户提供网站安全管理、性能优化及相关的技术支持为主要业务。通过基于**反向代理的内容分发网络(CDN, Content Delivery Network)、任播(Anycast)技术、基于nginx+lua架构的Web应用防火墙(WAF, Web Application Firewall)及分布式域名解析服务(Distributed Domain Name Server)**等技术，Cloudflare可以帮助受保护站点抵御包括分布式拒绝服务攻击(DDoS, Distributed Denial of Service)在内的大多数网络攻击，确保该网站长期在线，同时提升网站的性能、访问速度以改善访客体验。
>
> 官网：<https://cloudflare.com/>
>
> 官方文档：<https://www.cloudflare.com/zh-cn/about-overview/>

## 具体步骤

### 登录Cloudflare

- [>>> Cloudflare登录页面 <<<](https://dash.cloudflare.com/login)

![p17](../img/p002/p1.png)

### 登陆成功，来到控制台，点击`+ 添加`

![p18](../img/p002/p2.png)

### 再点击`连接域`

![p19](../img/p002/p3.png)

### 在`输入现有域`下面的输入框中输入你的域名

![p20](../img/p002/p4.png)

### 点击`继续`

![p21](../img/p002/p5.png)

### 将会让你选择计划，个人建议选择使用免费计划，如果条件允许可以选择付费计划

![p22](../img/p002/p6.png)

### 首先等待域名解析完成，任何直接点击`继续`

![p23](../img/p002/p7.png)

### 接下来更改域名的DNS解析，复制这个页面提供的两个`Cloudflare名称服务器`的地址

![p24](../img/p002/p8.png)

### 来到你的阿里云控制台,点击全部域名

- [>>> 阿里云控制台 <<<](https://dc.console.aliyun.com/next/index)

![p25](../img/p002/p9.png)

### 来到域名列表，找到你刚刚购买的域名，点击`管理`

![p26](../img/p002/p10.png)

### 来到域名详情页，点击`DNS修改`

![p27](../img/p002/p11.png)

### 点击`修改DNS服务器`

![p28](../img/p002/p12.png)

### 将刚刚Cloudflare提供的两个`Cloudflare名称服务器`的地址，复制到对应框中（①对①，②对②）

>- 复制：
>
>![p24](../img/p002/p8.png)
>
>- 粘贴：
>
>![p29](../img/p002/p13.png)

### 点击`确定`

![p30](../img/p002/p14.png)

### 进行一下身份安全验证

![p31](../img/p002/p15.png)

![p32](../img/p002/p16.png)

### 成功后会跳转回域名`DNS修改`页面，如果DNS服务器变成了`xxx.cloudflare.com`，则表示成功

![p33](../img/p002/p17.png)

## 验证

### 来到Cloudflare的控制台，点击`Workers和Pages`

- [>>> Cloudflare的控制台 <<<](https://dash.cloudflare.com/)

![p34](../img/p002/p18.png)

### 在Workers和Pages页面点击`创建`

![p35](../img/p002/p19.png)

### 来到Workers和Pages创建页面，选择`Pages`

![p36](../img/p002/p20.png)

### 点击使用直接上传的`开始使用`

![p37](../img/p002/p21.png)

### 来到创建页面，填入项目名称

![p38](../img/p002/p22.png)

### 接着点击`创建项目`

![p39](../img/p002/p23.png)

### 复制以下HTML代码复制到`test\index.html`中

![p40](../img/p002/p24.png)

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>TEST</title>
</head>
<body>
<div style="
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 200px;
">
    TEST
</div>
</body>
</html>
```

### 将test文件夹上传并部署

![p41](../img/p002/p25.png)

![p42](../img/p002/p26.png)

### 点击`添加自定义域`

![p43](../img/p002/p27.png)

### 点击`设置自定义域`

![p44](../img/p002/p28.png)

### 填入你的域名的子域`test.你的域名`

![p45](../img/p002/p29.png)

### 点击`继续`

![p46](../img/p002/p30.png)

### 来到激活域的页面，点击`激活域`

![p47](../img/p002/p31.png)

### 接下来打开你喜爱的游戏，等待子域被激活

![p48](../img/p002/p32.png)

### 子域激活成功后，用浏览器打开子域地址

![p49](../img/p002/p33.png)

### 如果看到这个页面说明部署成功

![p50](../img/p002/p0.png)
