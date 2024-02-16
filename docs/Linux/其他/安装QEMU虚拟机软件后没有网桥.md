# 创建网桥

## 使用`brctl-utils`包

需要安装`bridge-utils`包，并手动创建一个网桥：

~~~shell
 brctl addbr "bridgename"
 ~~~

 比如添加一个名为br0的网桥：

~~~shell
brctl addbr br0
~~~

命令需要root权限。

创建的网桥关机后就自动没了，因此每次启动都需要手动重新创建这个网桥。

参考：[linux基金会官网](https://wiki.linuxfoundation.org/networking/bridge)

## 使用`iprout2`包

`brctl-utils2`包已经被弃用了, 现在应该用`iproute2`包.

~~~shell
ip link add name br0 type bridge # 创建一个名为br0的网桥
ip link set dev br0 up           # 激活网桥
~~~

详细信息: [arch wiki-Network bridge](https://wiki.archlinux.org/title/Network_bridge)

但现在还无法设置我的物理无限网卡和这个网桥关联, 只能之后有空再试了

TODO 找出能让物理网卡与网桥关联的方法, 使虚拟机联网.
