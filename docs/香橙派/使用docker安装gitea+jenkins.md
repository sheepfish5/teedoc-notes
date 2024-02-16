# 使用docker安装gitea+jenkins

参考：[Gitea 与 Jenkins 的集成实践，打造你的专属 CI/CD 系统](https://www.cnblogs.com/Gitea/p/jenkins.html)

## 注意

### 1. Jenkins的版本问题

Jenkins不能像linux的包管理器那样自动处理各种版本冲突，因此第一次安装插件时不出意外一定会失败。但手动处理冲突（再官网上依次下载适合当前Jenkins版本的插件）实在是过于麻烦，因此选择手动将Jenkins升至最新版，这样一般能直接满足所有插件对Jenkins的版本要求。具体做法是从官网下载Jenkins的最新版war包，运行docker容器，将下载的war包替换掉容器中的`/usr/share/jenkins/jenkins.war`，然后再重启容器即可。重启之后，所有插件自动安装成功。

> 替换最新版war包参考：[Docker部署的Jenkins如何更新版本呢](https://blog.csdn.net/tomonkey/article/details/106942162)

### 2. Jenkins版本终极解决方法

先上Docker Hub搜jenkins，然后使用他们最新版本的tag，注意不要用默认的`latest`，那个不是最新版。比如现在的最新版是`jenkins/jenkins:2.414`。

不要再手动替换了，那样太麻烦了。

### 3. gitea的webhook无法触发

要在jenkins的系统配置里为Gitea Server增加URL别名，具体可以点Alias URL旁边的问号。

> 参考：[stackoverflow](https://stackoverflow.com/a/68432979/21743075)

~~但即使是使用了这种方法，我也不能让webhook稳定触发。至少是我使用Gitea Webhook提供的假事件无法触发。~~看起来不是jenkins的问题，假事件的确无法触发，但每次提交都能正常触发构建，这样就算是解决了问题。

### 4. gitea的版本问题

和jenkins一样，默认的`latest`标签并不是最新版，也得上Docker Hub手动搜gitea并确定最新版。比如目前的最新版是`gitea/gitea:1.20-nightly`。

另外附上gitea普通版和nightly版的区别：

> 你好，这是Bing。我可以帮你找到一些关于gitea普通版和gitea nightly的区别的信息。😊
> 根据我的搜索结果，gitea普通版是指gitea的正式发布版本，它经过了充分的测试和验证，保证了稳定性和安全性。gitea nightly是指gitea的每日构建版本，它包含了最新的开发进展和功能，但可能存在一些错误和问题。
> 如果你想使用gitea的最新功能和特性，你可以选择gitea nightly，但你需要注意备份你的数据和配置，并且及时反馈任何问题。如果你想使用gitea的稳定版本，你可以选择gitea普通版，但你可能无法享受到一些新的改进和优化。
> 你可以在这里下载gitea的不同版本：<https://dl.gitea.io/gitea/>
> 希望这些信息对你有帮助。如果你还有其他问题，欢迎继续提问。😊

### 5. 新版gitea的Webhook报错 webhook.ALLOWED_HOST_LIST

使用新版（1.20）gitea，在测试到jenkins的webhook时，无法推送，报错如下：

~~~text
Delivery: Post "http://jenkins:8080/gitea-webhook/post": dial tcp 172.26.0.2:8080: webhook can only call allowed HTTP servers (check your webhook.ALLOWED_HOST_LIST setting), deny 'jenkins(172.26.0.2:8080)'
~~~

原因是gitea无法发送post请求，因为新版gitea默认禁用了webhook（？？？why？？？），在`/data/gitea/conf/app.ini`中加上：

~~~text
[webhook]
ALLOWED_HOST_LIST = *
~~~

之后重启容器（`docker container restart gitea`）即可。

参考：[Webhook报错 webhook.ALLOWED_HOST_LIST](https://discourse.gitea.io/t/webhook-webhook-allowed-host-list-setting/4655)
