## Servlet中使用cookie实现Session

### 前提

http是没有状态的，Web应用无法分别出两个http请求是否来自同一个浏览器。比如用户A先请求Get index.html，在这一个html中插入了一张图片，于是浏览器又立即请求Get image.png，Web应用实际把这两个请求看成没有联系的独立的请求。

如果为同一个用户短时间内发送的所有请求分配一个识别码，这样的话，Web应用就可以把包含这个识别码的请求归为同一个用户发送的。当然，这个识别码是有时效的，超出这个失效后，Web应用就不认了。

我们将Web应用识别同一用户短时间内发送的请求称为`会话（Session）`，承载这个识别码的是HTTP协议中的Cookie机制。也就是说，Cookie配合唯一识别码实现了Session机制。

### Servlet中的HttpSession

当对用户发送的请求第一次调用getSession()方法时，Servlet底层机制自动在内存中生成一个识别码到HttpSession对象的映射，同时做出如下动作：

- 本次Web应用响应时会附带一个名为JSESSIONID的Cookie，内容就是这个识别码，之后用户发送的请求都会带上这个识别码。
- getSession()同时将HttpSession对象返回，可以对这个HttpSession对象调用getAttribute()方法，附带上有关用户本次会话的信息，方便其他Servlet使用。
下次用户的请求中带有Cookie时，再对请求调用getSession()方法，就会返回之前的HttpSession对象。
