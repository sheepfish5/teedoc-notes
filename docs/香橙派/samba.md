# samba

## 介绍

在debian上需要安装包`smb`和`smbclient`，前者包含两个服务`smbd`和`nmbd`，后者是客户端，用于测试。

如果需要将远程samba服务在fstab中设置开机自动挂载，还需要安装`cifs-utils`。

`smbd`占用139和445端口，`nmbd`占用137和138端口。

## 常用命令

### `pdbedit` 查看samba中已有的用户

~~~bash
pdbedit -L # 列出所有用户（简略）

pdbedit -Lv # 列出所有用户的详细信息
~~~

### `smbpasswd` 管理账号

~~~bash
smbpasswd -a smb1  # 向samba的数据库中增加用户smb1 (smb1必须是linux中已有的用户，最好设置/sbin/nologin)

smbpasswd -x smb1  # 从samba数据库中删除用户smb1

smbpasswd smb1  # 修改smb1的samba密码
~~~

### `smbstatus` 服务器状态

~~~bash
smbstatus  # 列出samba服务的详细状态
~~~

### `smbclient` linux客户端

~~~bash
# smbclient 需要安装同名包才能使用

smbclient -L 192.168.1.103  # 访问对应ip上的samba服务
                            # 执行后默认使用名为root的samba用户，会提示输入密码
                            # 如果不输密码直接回车，就是匿名()访问

smbclient -L 192.168.1.103 -U smb1%hello  # 指定了用户smb1，密码hello

# UNC路径(Universal Naming Convention, 通用命名规范), 格式如：
# \\sambaserver\sharename
smbclient //192.168.1.103/sharefile -U smb1%hello  # 使用UNC路径访问
~~~

### `testparm` 检查smb.conf的语法错误

## 具体配置

见`linux\模板配置文件\smb.conf`

## samba配置中的宏定义

- %m 客户端主机的NetBIOS名
- %M 客户端主机的FQDN
- %H 当前用户home目录路径
- %U 当前用户的用户名
- %g 当前用户所属组
- %h samba服务器的主机名
- %L samba服务器的NetBIOS名
- %I 客户端主机的IP
- %T 当前日期和时间
- %S 可登录的用户名

## Windows缓存账号密码

使用Windows访问了linux上的samba服务，Windows会缓存输入了账号和密码。

可以通过重启或注销清除缓存。

或者打开cmd（不需要管理员权限），使用`net use`命令查看缓存项。

使用`net use \\192.168.1.103\share /del`删除缓存项。

删除之后，等待一会，再次访问，会提示重新输入密码。
