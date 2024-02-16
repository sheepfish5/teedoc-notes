# `--privileged`选项

通过docker部署v2raya时，官方文档中使用了`--privileged`选项。

根据docker官方文档，`--privileged`标志为容器提供所有功能。当操作员执行`docker run --privileged`时，Docker将启用对主机上所有设备的访问，并在AppArmor或SELinux中设置一些配置，以允许容器对主机的访问几乎与在主机上的容器外部运行的进程相同。有关使用`--privileged`运行的更多信息，请访问[Docker博客][def]。

[def]: https://www.docker.com/blog/docker-can-now-run-within-docker/?_gl=1*i5uajp*_ga*MTkyOTI2MzM1OC4xNjc5ODA3OTI5*_ga_XJWPQMJYHQ*MTY4NzQ4OTUyNi4xMi4xLjE2ODc0ODk2MjQuNjAuMC4w
