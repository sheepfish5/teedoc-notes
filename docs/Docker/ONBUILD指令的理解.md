# ONBUILD指令的理解

ONBUILD 指令可以为镜像添加触发器。²⁴ 其参数是任意一个 Dockerfile 指令。²

当我们在一个 Dockerfile 文件中加上 ONBUILD 指令，该指令对利用该 Dockerfile 构建镜像（比如为 A 镜像）不会产生实质性影响。² 但是当我们编写一个新的 Dockerfile 文件来基于 A 镜像构建一个镜像（比如为 B 镜像）时，这时构造 A 镜像的 Dockerfile 文件中的 ONBUILD 指令就生效了，在构建 B 镜像的过程中，首先会执行 ONBUILD 指令指定的指令，然后才会执行其它指令。²

ONBUILD 指令的作用是在父镜像向子镜像传递一些指令，让子镜像在构建时执行。¹³ 这样可以减少 Dockerfile 文件的重复内容编写，也可以实现一些通用的操作。²

ONBUILD 指令只能在构建子镜像时执行，对孙子镜像构建无效。² 这是为了避免重复执行相同的指令，造成不必要的开销。³

ONBUILD 指令的语法格式如下：

~~~Dockerfile
ONBUILD <INSTRUCTION>
~~~

其中，`<INSTRUCTION>` 是任意一个有效的 Dockerfile 指令，除了 FROM、MAINTAINER 和 ONBUILD 本身。¹³

例如：

~~~text
ONBUILD RUN mkdir mydir
~~~

这条指令表示在子镜像构建时，在根目录下创建一个名为 mydir 的目录。

源: 与必应的对话， 6/22/2023
(1) docker学习笔记17：Dockerfile 指令 ONBUILD介绍 - 51kata - 博客园. <https://www.cnblogs.com/51kata/p/5265107.html>.
(2) Dockerfile ONBUILD命令详解_onbuild run_金麟十三少的博客-CSDN博客. <https://blog.csdn.net/u012373281/article/details/113624065>.
(3) Using ONBUILD instruction | dockerlabs. <https://dockerlabs.collabnix.com/beginners/using-onbuild-images/>.
(4) How to Use Dockerfile ONBUILD to Run Triggers on Downstream Builds. <https://www.howtogeek.com/devops/how-to-use-dockerfile-onbuild-to-run-triggers-on-downstream-builds/>.
(5) 【Docker】Dockerfile 最佳实践-ONBUILD - 波尔 - 博客园. <https://www.cnblogs.com/jiangbo4444/p/14218238.html>.
