# vscode中的JUnit

基本全自动。

装了Java插件包（Extension Pack for Java）后，选边栏上的测试图标，点一下自动配置，选`JUnit Jupyter`，然等待自动下载就行了。

具体使用也只需要在写好的测试方法上加`@Test`注解就行了。

常用注解包括`@Before` `@After` `@Test` `@BeforeEach` `@AfterEach`。主要还是一个`@Test`就足够用了。

注意vscode里的Java测试无法输出到控制台，得用日志。
