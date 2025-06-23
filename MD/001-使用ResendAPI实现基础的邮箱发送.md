# 使用ResendAPI实现基础的邮箱发送

> **注意**：
> 本教程将会使用到Google和github等国外服务（因为相对来说更加方便），
> 如果你的网络环境不支持直接连接外域，
> 请使用VPN或者代理

## 教程概述

> 本教程介绍如何使用ResendAPI实现基础的邮箱发送。
> 将使用Python3作为编程语言，使用ResendAPI实现基础的邮箱发送。
>
> 最终实现以下效果：
> ![效果](../img/p001/p0.png)
> 通过[`random`](https://docs.python.org/zh-cn/3/library/random.html)模块生成随机数，将随机数作为邮件内容发送给指定邮箱。
> 并判断用户输入的数字是否正确。

## ResendAPI简介

> ResendAPI是一个用于发送邮件的API，可以免费使用。
>
> 官网：<https://resend.com/>
>
> 官方文档：<https://resend.com/docs/api>

## 准备工作

1. 安装[Python3](https://www.python.org/downloads/)环境，作为本次教程的主要编程语言 | [具体教程](foundation/002-Python环境安装.md)
2. 安装[PyCharm](https://www.jetbrains.com/zh-cn/pycharm/)环境，作为本次教程的编辑器 | [具体教程](foundation/001-JetBrains全家桶安装激活.md)
3. 注册账号：
   - [阿里云](https://www.aliyun.com/)账号
   - [cloudflare](https://www.cloudflare.com/)账号
   - [resend](https://resend.com/)账号

## 具体步骤

### 一、购买阿里云上的域名

#### 1. [登录阿里云](https://account.aliyun.com/login/login.htm)

![p1](../img/p001/p1.png)

#### 2. 点击搜索框输入`域名`两字

![p2](../img/p001/p2.png)
![p3](../img/p001/p3.png)

#### 3. 点击下面的`域名注册`

![p4](../img/p001/p4.png)

#### 4. 来到[域名注册页面](https://wanwang.aliyun.com/domain)，填写你想要的域名，点击`查询`或直接回车

![p5](../img/p001/p5.png)
![p6](../img/p001/p6.png)
![p7](../img/p001/p7.png)

#### 5. 来到查找结果页面，找到你要的域名(这里作者推荐`.top`域名)，点击`立即注册`

![p8](../img/p001/p8.png)

#### 6. 来到结账页面，选择持续租借时间，选择信息模板，点击`立即购买`

![p9](../img/p001/p9.png)
![p10](../img/p001/p10.png)
![p11](../img/p001/p11.png)

> ##### 如果没有信息模板解决方案
>
> 点击`创建信息模板`
> ![p12](../img/p001/p12.png)
> 然后填写信息模板
> ![p13](../img/p001/p13.png)
> 填写完信息模板后，点击`提交`
> ![p14](../img/p001/p14.png)

#### 7. 完成上述步骤后，来到付款页面，点击`支付`

![p15](../img/p001/p15.png)

#### 8.都看到这里了，恭喜你，已经成功拥有了你自己的域名，[点击这里就能查看](https://dc.console.aliyun.com/next/index#/overview)

![p16](../img/p001/p16.png)

### 二、配置域名DNS解析

#### 1.[登录cloudflare](https://dash.cloudflare.com/login)

![p17](../img/p001/p17.png)

#### 2. 登陆成功，来到控制台，点击`+ 添加`，再点击`连接域`

![p18](../img/p001/p18.png)
![p19](../img/p001/p19.png)

#### 3. 在`输入现有域`下面的输入框中输入你的域名，然后点击`继续`

![p20](../img/p001/p20.png)
![p21](../img/p001/p21.png)

#### 4. 将会让你选择计划，个人建议选择使用免费计划，如果条件允许可以选择付费计划

![p22](../img/p001/p22.png)

#### 5. 接下来是配置DNS解析

> - 首先等待域名解析完成，任何直接点击`继续`
>   ![p23](../img/p001/p23.png)
> - 接下来更改域名的DNS解析
> - 复制这个页面提供的两个`Cloudflare名称服务器`的地址
>   ![p24](../img/p001/p24.png)
> - 来到你的[阿里云控制台](https://dc.console.aliyun.com/next/index),点击全部域名
>   ![p25](../img/p001/p25.png)
> - 来到域名列表，找到你刚刚购买的域名，点击`管理`
>   ![p26](../img/p001/p26.png)
> - 来到域名详情页，点击`DNS修改`
>   ![p27](../img/p001/p27.png)
> - 点击`修改DNS服务器`
>   ![p28](../img/p001/p28.png)
> - 将刚刚Cloudflare提供的两个`Cloudflare名称服务器`的地址，复制到对应框中（①对①，②对②）
>   - 复制：
>     ![p24](../img/p001/p24.png)
>   - 粘贴：
>     ![p29](../img/p001/p29.png)
> - 点击`确定`
>   ![p30](../img/p001/p30.png)
> - 进行一下验证
>   ![p31](../img/p001/p31.png)
>   ![p32](../img/p001/p32.png)
> - 成功后会跳转回域名`DNS修改`页面，如果NDS服务器变成了`xxx.cloudflare.com`，则表示成功
>   ![p33](../img/p001/p33.png)

#### 6. 现在我们测试一下是否配置成功

> - 来到[cloudflare的控制台](https://dash.cloudflare.com/)，点击`Workers和Pages`
>   ![p34](../img/p001/p34.png)
> - 在Workers和Pages页面点击`创建`
>   ![p35](../img/p001/p35.png)
> - 来到Workers和Pages创建页面，选择`Pages`
>   ![p36](../img/p001/p36.png)
> - 点击使用直接上传的`开始使用`
>   ![p37](../img/p001/p37.png)
> - 来到创建页面，填入项目名称
>   ![p38](../img/p001/p38.png)
> - 接着点击`创建项目`
>   ![p39](../img/p001/p39.png)
> - 复制以下HTML代码复制到`test\index.html`中
>   ![p40](../img/p001/p40.png)
>
>   ```html
>   <!-- index.html -->
>   <!DOCTYPE html>
>   <html lang="en">
>   <head>
>    <meta charset="UTF-8" />
>    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
>    <title>TEST</title>
>   </head>
>   <body>
>    <div
>      style="
>        display: flex;
>        justify-content: center;
>        align-items: center;
>        font-size: 200px;
>      "
>    >
>      TEST
>    </div>
>   </body>
>   </html>
>   ```
>
> - 将test文件夹上传并部署
>   ![p41](../img/p001/p41.png)
>   ![p42](../img/p001/p42.png)
> - 点击`添加自定义域`
>   ![p43](../img/p001/p43.png)
> - 点击`设置自定义域`
>   ![p44](../img/p001/p44.png)
> - 填入你的域名的子域`test.你的域名`
>   ![p45](../img/p001/p45.png)
> - 点击`继续`
>   ![p46](../img/p001/p46.png)
> - 来到激活域的页面，点击`激活域`
>   ![p47](../img/p001/p47.png)
> - 接下来打开你喜爱的游戏，等待子域被激活
>   ![p48](../img/p001/p48.png)
> - 子域激活成功后，用浏览器打开子域地址
>   ![p49](../img/p001/p49.png)
> - 如果看到这个页面说明部署成功
>   ![p50](../img/p001/p50.png)

### 三、配置resend邮箱路由

#### 1. [登录Resend](https://resend.com/)

![p51](../img/p001/p51.png)

#### 2. 点击`Domains`，来到[Domains页面](https://resend.com/domains)

![p52](../img/p001/p52.png)

#### 3. 点击`Add Domain`，来到[Add Domain页面](https://resend.com/domains/add)

![p53](../img/p001/p53.png)
![p54](../img/p001/p54.png)

#### 5. 填写信息，完成添加域名

> - 填入用于邮箱发送的子域，地区推荐选择US
>   ![p55](../img/p001/p55.png)
> - 点击`Add Domain`
>   ![p56](../img/p001/p56.png)
> - 来到DNS配置页面，直接点击`登录到Cloudflare`
>   ![p57](../img/p001/p57.png)
> - 弹出Cloudflare授权窗口，点击`授权`
>   ![p58](../img/p001/p58.png)
> - 现在又可以打开你喜欢的游戏，等待激活成功了。当显示```Well done! All the DNS records are verified. You are ready to start building and sending emails with this domain.```说明激活成功了
>   ![p59](../img/p001/p59.png)

#### 4. 点击`API keys`，来到[API Keys页面](https://resend.com/api-keys)

![p60](../img/p001/p60.png)

#### 5. 点击`Create API key`，创建API key

![p61](../img/p001/p61.png)
![p62](../img/p001/p62.png)
![p63](../img/p001/p63.png)

#### 6. 最后复制保存ResendAPIkey

![p64](../img/p001/p64.png)

### 四、开始发送你的第一个resend邮件

#### 1. 打开PyCharm新建项目

> - 点击`新建项目`
>   ![p65](../img/p001/p65.png)
> - 项目命名为`EmailSendTest`
>   ![p66](../img/p001/p66.png)
> - 点击`创建`
>   ![p67](../img/p001/p67.png)

#### 2. 来到项目，按照以下顺序点击创建文件

![p68](../img/p001/p68.png)

#### 3. 分别创建`__main__.py`和`resend_email_demo.py`文件

![p69](../img/p001/p69.png)
![p70](../img/p001/p70.png)

#### 4. 根据[ResendAPI文档](https://resend.com/docs/api-reference/emails/send-email)，编写`resend_email_demo.py`工具文件

```python
# 官方API
import resend
resend.api_key = "re_xxxxxxxxx"
params: resend.Emails.SendParams = {
  "from": "Acme <onboarding@resend.dev>",
  "to": ["delivered@resend.dev"],
  "subject": "hello world",
  "html": "<p>it works!</p>"
}
email = resend.Emails.send(params)
print(email)
```

```python
# resend_email_demo.py
import resend
def sendEmail(
        __fromName__,   # 发送人昵称
        __fromEmail__,  # 发送人邮箱
        __to__,         # 收件人邮箱
        __subject__,    # 标题
        __html__        # 内容
):
    # 获取api授权（填写自己的ResendAPIkey）
    resend.api_key = "re_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    # 发送邮件参数-from、to、subject、html
    params: resend.Emails.SendParams = {
        "from": f'{__fromName__} <{__fromEmail__}>',
        "to": __to__,
        "subject": __subject__,
        "html": __html__
    }
    email = resend.Emails.send(params)
    print(email) # 打印ID
```

#### 5.完善`__main__.py`文件

```python
# __main__.py
import resend_email_demo
import random

if __name__ == '__main__':
    # 生成一个随机值
    rand_num = random.randint(1, 100)
    # 发送邮件
    resend_email_demo.sendEmail(
        "Acme",  # 邮件发送者昵称
        "onboarding@resend.dev",  # 邮件发送者邮箱
        input('接收者：'),  # 邮件接收者邮箱
        "我是标题",  # 邮件标题
        f'这个是你的幸运数字 ヾ(^▽^*))) ：{rand_num}'
    )
    if int(input('你的收到的数字是：')) == rand_num:
        print('对的！没错！')
    else:
        print('好像不是的呢！')
```

#### 6.点击运行

![p0](../img/p001/p0.png)
