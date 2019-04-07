---
layout:     post
title:      django发送邮件
subtitle:   使用django+outlook的配置发送邮件
date:       2017-01-06
author:     BY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - django
    - smtp
    - email
---

毕设网站架设用django，创建新用户用邮箱验证，用outlook的邮箱，按照网上的教程会报错：

        raise SMTPSenderRefused(code, resp, from_addr)
        
    smtplib.SMTPSenderRefused: (530, b'5.7.57 SMTP; Client was not authenticated to send anonymous mail during MAIL FROM [HK2PR04CA0065.apcprd04.prod.outlook.com]', '=?utf-8?q?pu?= <southpush@outlook.com>')


看了django的文档之后发现是配置的问题，我估计是因为outlook不给发匿名邮件，但是网上的教程都没有设置验证信息所以才会报错的，添加这两条配置就可以了：

    EMAIL_HOST_USER = 'youremail@outlook.com'
    EMAIL_HOST_PASSWORD = 'your email password'

完整的配置和代码如下：
settings.py

    EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
    EMAIL_USE_TLS = True
    EMAIL_HOST_USER = privacyConfig.emailUser
    EMAIL_HOST_PASSWORD = privacyConfig.emailPassword
    EMAIL_PORT = 587
    EMAIL_HOST = "smtp.office365.com"
    DEFAULT_FROM_EMAIL = "pu <southpush@outlook.com>"

auth.py

    def new_account_auth_email(user, to_email):
        data = {'username': user.username}
        plaintext = get_template('email/verify.txt').render(data)
        htmltext = get_template("email/verify.html").render(data)
    
        subject, from_email = 'test text', settings.DEFAULT_FROM_EMAIL
    
        msg = EmailMultiAlternatives(subject, plaintext, from_email, [to_email])
        msg.attach_alternative(htmltext, 'text/html')
        return msg.send()

另附官方文档说明：

![](https://github.com/southpush/southpush.github.io/blob/master/postsImage/20190407225518.png)