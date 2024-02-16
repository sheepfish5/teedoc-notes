# vscode中的logback

导入三个包：`slf4j.jar` `logback-classic.jar` `logback-core.jar`，再设置好`logback.xml`。就可以使用了。

使用的时候，在类上添加一个`@Slf4j`注解，方法内就可以使用`log`对象了。

由于logback无法细节到格式化浮点数输出，所以要格式化浮点数，推荐使用`DecimalFormat`，具体使用可以参考: <https://blog.csdn.net/wdd1324/article/details/70153896>
