# 用msmtp发邮件

## 先安装

~~~bash
apt install msmtp
~~~

## 设置配置文件

配置文件为`~/.msmtprc`

内容实例：

~~~.msmtprc
account default
  auth login
  host smtp.163.com
  from hello@163.com
  user hello@163.com
  password XXXX
  tls on
  tls_certcheck on

logfile ~/.msmtp.log
~~~

## 将邮件内容写在文件里

内容实例

~~~
To: hello@foxmail.com
From: hello@163.com
Subject: 邮件主题

邮件内容
~~~

## 执行命令

~~~bash
cat /opt/bin/end.mail | msmtp --read-{envelope-from,recipients}
~~~