# 特殊文件权限SUID, SGID, SBIT总结

SUID

- 只可用于二进制可执行文件.
- SUID文件权限如`-rwsrwxrwx`.
- 当其他人执行了SUID文件, 在执行过程中, 该进程拥有文件所有者的权限. 比如passwd是一个SUID文件, 所有者是root, 普通用户执行passwd时, passwd拥有root权限.

SGID

- 可用于目录或二进制可执行文件.
- SGID文件(目录)权限如`drwxrwsrwx`.
- 对于SGID目录, 用户在该目录下的有效群组将会变为该目录的群组, 创建在该目录下的所有文件或目录的群组默认与该目录相同. 比如有一个SGID的common目录, 其群组为root, 用户sheepfish在其中创建了一个test目录, 那么test目录的群组默认为root. 但是之后可以使用`chgrp`命令改变test的群组
- 对于SGID二进制可执行文件, 类似与SUID, 用户执行时, 进程拥有和文件相同的群组. 比如locate是一个SGID指令, 群组为slocate, 用户sheepfish执行locate时, locate可以读取只有slocate用户才有读权限的文件.

SBIT

- 只对目录有效
- SBIT目录权限如`drwxrwxrwt`
- 用户A在一个SBIT目录下创建了一个文件test, 那么, 该文件只有A和root才能删除, 即使用户B有该目录的W权限, B也不能删除test文件.
- /tmp目录就是一个SBIT目录
