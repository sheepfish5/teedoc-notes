# KDE程序`Calender Reminder`占用内存过大

这个程序开机就自动占用500MiB到800MiB, 我也不知道这个程序是干什么用的(上网搜了听说是记录各种事件, 也不清楚是真是假), 尝试卸载过Kalender, 这个程序仍然开机自启动.

根据网上的方法, 这和KDE的`Akonadi`系统有关, 具体有什么关系也我也不深究了, 关闭Akonadi可以让Calender Reminder的内存占用缩小至几十MiB.

~~~shell
akonadictl stop
~~~

已经试过把这条命令放在`~/.bash_profile`和`~/.bashrc`里, 都无法做到开机自动关闭Akonadi, 猜测前两个文件运行得比后者更早.
