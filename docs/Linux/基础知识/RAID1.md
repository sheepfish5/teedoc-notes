# 关于RAID1的镜像模式

老师上课时讲的不是真正的镜像模式, 而是RAID1+0

真正的镜像模式应该是: 有n个硬盘, 实际存储的内容只占一个硬盘, 其他的n-1个硬盘都是这个硬盘的镜像, 也就是说有n-1份冗余.

而RAID1就是采用这种镜像模式.

> --参考[raid.wiki.kernel.org](https://raid.wiki.kernel.org/index.php/What_is_RAID_and_why_should_you_want_it%3F#RAID1.2FMirror_Mode):
> Two or more disks are combined into one volume. Reads/writes are still broken into blocks, but ALL blocks are copied to ALL disks in the volume.
> This is the first mode which actually has redundancy. This mode maintains an exact copy of the information on one disk across all the other disks. If any disk fails in the volume (or all disks but 1), the data is still available with no performance degradation.
> The size of the volume will be the size of one of the disks in the volume. If one disk is smaller than another, your volume will be the size of the smallest disk.

翻译: 两个或多个磁盘组合成一个卷。读取/写入仍然被分为块，但所有块都被复制到卷中的所有磁盘。
这是第一种实际具有冗余的模式。此模式维护一个磁盘上所有其他磁盘上信息的精确副本。如果卷中的任何磁盘（或除1个磁盘以外的所有磁盘）出现故障，则数据仍然可用，性能不会下降。
卷的大小将是卷中某个磁盘的大小。如果一个磁盘比另一个小，那么您的卷将是最小磁盘的大小。

## 使用mdadm指令在CentOS7上组软件RAID1

我的虚拟机上有4个空闲分区: /dev/vda10, /dev/vda11, /dev/vda12, /dev/vda13, 每个分区大小为1G
使用mdadm指令, 用软件模拟将这4个分区组成RAID1.

创建RAID1, 结果设备为/dev/md0.

~~~shell
mdadm --create /dev/md0 \
--auto=yes \
--level=1 \
--chunk=256K \
--raid-devices=4 \
--spare-devices=0 \
/dev/vda{10,11,12,13}
~~~

查看RAID1设备的信息:

~~~shell
mdadm --detail /dev/md0
~~~
