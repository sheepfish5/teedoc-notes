# `async`和`await`

从结果来说，由`async`声明的函数和普通函数没有什么不同，其中的javascript代码都是同步执行的。

主要差别在于`async`函数中`await`关键字的效果。

[MDN文档中][MDN]已经说的很详细了，这里再总结下：在`await`之前，`async`函数都是同步执行，执行到`await`后的异步操作时（如`fetch(url)`），浏览器就使用其他线程或进程执行网络或文件读取等操作，javascript主线程就可以将`async`函数直接返回一个Promise对象，然后继续顺序执行。等`async`函数内的`await`操作结束后，javascript主线程再回到`async`函数中继续往下执行。补充两点，`async`函数的`return`子句和`await`表达式都是直接返回Promise所代理的对象，比如`await fetch(url)`直接返回`response`对象。

[MDN]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function
