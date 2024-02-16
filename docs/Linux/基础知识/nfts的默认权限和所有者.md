# nfts的默认权限和所有者

我使用一个ntfs分区来作为我双系统的共享分区，但是在linux下，这个分区里的文件默认所有者和所有组是root:root。

我要改为sheepfish:sheepfish，要在挂载的时候设置相应参数。

如果使用`mount`命令挂载，就设置命令行参数。

我用的是`/etc/fstab`默认挂载，于是在配置文件中按如下方式设置：

~~~Shell Script
UUID=8A12645D1264506D /home/sheepfish/media ntfs defaults,nls=utf8,umask=000,uid=1000,gid=1000 0 0
~~~
