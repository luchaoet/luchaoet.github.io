---
title: 使用Python发送QQ邮件
date: 2019-02-11 20:48:48
tags: ['Python', '发送邮件']
summary:
---
SMTP是发送邮件的协议，Python内置对SMTP的支持，可以发送纯文本邮件、HTML邮件以及带附件的邮件<br />Python对SMTP支持有`smtplib`和`email`两个模块，`email`负责构造邮件，`smtplib`负责发送邮件

### 发送QQ邮件
python发送QQ邮件需要QQ邮箱的授权码
#### 开启QQ邮箱SMTP服务，获得授权码
设置 -> 帐号<br />![20190210220219.png](https://cdn.nlark.com/yuque/0/2019/png/115449/1549808802557-02370ec9-27cb-46ca-88a5-9a5456e3a372.png#align=left&display=inline&height=158&linkTarget=_blank&name=20190210220219.png&originHeight=374&originWidth=1186&size=215923&width=500)

往下翻页可找到**POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV服务**<br />![22_19_27__02_10_2019.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1549808873926-77f0ea5c-afbc-4cdb-ac69-434d2b47d65f.jpeg#align=left&display=inline&height=159&linkTarget=_blank&name=22_19_27__02_10_2019.jpg&originHeight=526&originWidth=1654&size=652969&width=500)

点击开启，验证密保<br />![22_20_54__02_10_2019.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1549808923604-80cf498a-6cec-4651-95ce-86a6ba9c48f6.jpeg#align=left&display=inline&height=243&linkTarget=_blank&name=22_20_54__02_10_2019.jpg&originHeight=756&originWidth=1554&size=340818&width=500)

获取授权码<br />![22_22_13__02_10_2019.jpg](https://cdn.nlark.com/yuque/0/2019/jpeg/115449/1549808984974-844a5297-031a-47d6-a1d1-69a38dfa3e3d.jpeg#align=left&display=inline&height=257&linkTarget=_blank&name=22_22_13__02_10_2019.jpg&originHeight=738&originWidth=1434&size=301450&width=500)

#### 发送纯文本邮件
使用SSL的通用配置如下：<br />接收邮件服务器：pop.qq.com ,使用SSL,端口 995<br />发送邮件服务器： smtp.qq.com,使用SSL,端口 465或 587
```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
import os.path
import smtplib
from email.mime.text import MIMEText
from email.utils import formataddr

class SendEmail:
    def send_email(self):
        mail_host="smtp.qq.com"           # 设置服务器
        mail_user="*****@qq.com"          # 发送邮件的邮箱
        mail_pass="ebevmrxtzfbybfdi"      # 上一步获取的授权码

        sender = '*****@qq.com' 	# 发送邮件的邮箱
        receivers = '*****@icloud.com'   # 接收邮件的邮箱，可设置为你的QQ邮箱或者其他邮箱

        message = MIMEText('这是邮件内容，假装这里有点东西', 'plain', 'utf-8')    # 邮件正文内容
        # 昵称便于让收件人知道发件人是谁
        message["From"] = formataddr(["YUMI", '*****@qq.com']) # 发件人昵称与邮箱
        message["To"] = formataddr(["Lucy", '*****@icloud.com']) # 收件人昵称与邮箱
        message['Subject'] = '邮件标题' # 邮件标题

        try:
            smtpObj = smtplib.SMTP_SSL(mail_host)
            smtpObj.connect(mail_host, 465)        # 25 为 SMTP 端口号  qq 465 587
            smtpObj.login(mail_user, mail_pass)
            smtpObj.sendmail(sender, receivers, message.as_string())
            print("邮件发送成功")
            
        except smtplib.SMTPException as e:
            print("Error: 邮件发送失败")
            print(e)
        
        
notice = SendEmail()
notice.send_email()
```

发送结果<br />![IMG_4934.PNG](https://cdn.nlark.com/yuque/0/2019/png/115449/1549883980853-b02242d4-54d1-47c3-a603-b00ee3b43eb6.png#align=left&display=inline&height=230&linkTarget=_blank&name=IMG_4934.PNG&originHeight=431&originWidth=750&size=80485&width=400)

#### 发送HTML邮件
写一个html文件

```html
<!DOCTYPE html>
<html lang="en">
<body>
        <h1>使用python发送QQ邮件</h1>
        <p>SMTP是发送邮件的协议，Python内置对SMTP的支持，可以发送纯文本邮件、HTML邮件以及带附件的邮件</p>
        <p>Python对SMTP支持有<code>smtplib</code>和<code>email</code>两个模块，<code>email</code>负责构造邮件，<code>smtplib</code>负责发送邮件</p>
        <p><br /></p>
        <h3 id="4a415f6d">发送QQ邮件</h3>
        <p>python发送QQ邮件需要QQ邮箱的授权码</p>
        <h4 id="918c165f">开启QQ邮箱SMTP服务，获得授权码</h4>
        <p>设置 -&gt; 帐号</p>
        <p><img alt="20190210220219.png" src="https://cdn.nlark.com/yuque/0/2019/png/115449/1549808802557-02370ec9-27cb-46ca-88a5-9a5456e3a372.png#align=left&amp;display=inline&amp;height=158&amp;linkTarget=_blank&amp;name=20190210220219.png&amp;originHeight=374&amp;originWidth=1186&amp;size=215923&amp;width=500" style="max-width: 600px; width: 500px;" /></p>
</body>
</html>
```

读取该文件文本，发送
```python
contents = ''
with open('email.html') as file_obj:
    contents = file_obj.read()

print(contents)
class SendEmail:
    def send_email(self):
        mail_host="smtp.qq.com"           # 设置服务器
        mail_user="550502661@qq.com"     # 用户名
        mail_pass="ebevmrxtzfbybfdi"      # 口令(qq邮箱非密码)

        sender = '550502661@qq.com'
        receivers = 'luchaoet@icloud.com'   # 接收邮箱，可设置为你的QQ邮箱或者其他邮箱
        # 发送html文件文本
        message = MIMEText(contents, 'html', 'utf-8')
        message["From"] = formataddr(["YUMI", '550502661@qq.com'])
        message["To"] = formataddr(["Lucy", 'luchaoet@icloud.com'])
        message['Subject'] = '邮件标题'

        try:
            smtpObj = smtplib.SMTP_SSL(mail_host)
            smtpObj.connect(mail_host, 465)        # 25 为 SMTP 端口号  qq 465 587
            smtpObj.login(mail_user, mail_pass)
            smtpObj.sendmail(sender, receivers, message.as_string())
            print("邮件发送成功")
            
        except smtplib.SMTPException as e:
            print("Error: 无法发送邮件")
            print(e)
             
notice = SendEmail()
notice.send_email()
```

发送结果<br />![IMG_4936.PNG](https://cdn.nlark.com/yuque/0/2019/png/115449/1549888706162-eff14b6b-aaef-44f5-bbbc-e7a73f21ce50.png#align=left&display=inline&height=853&linkTarget=_blank&name=IMG_4936.PNG&originHeight=1280&originWidth=750&size=171477&width=500)