# 理解基于Promise的API

## 回顾一下`Promise`调用过程

~~~javascript
console.log('before async');

fetch(url)
    .then((response) => {
    if (!response.ok) {
        throw new Error(`HTTP 错误: ${response.status}`);
    }
    return response.text();
    })
    .then((text) => poemDisplay.textContent = text)
    .catch((error) => poemDisplay.textContent = `获取诗歌失败：${error}`);

console.log('after async');
~~~

以`fetch(url)`为例，调用完函数后，函数立即返回Promise对象，后续两个`then()`函数在返回的Promise对象上注册处理函数，最后`catch()`函数注册异常处理函数。`fetch()`返回后，javascript主线程继续向下执行`console.log('after async');`，同时浏览器后台可以通过其他线程或进程执行网络操作，这些非javascript操作成功结束后，javascript主线程将之前注册了的两个处理函数加入到任务队列中，等待空闲时再执行，也就是说，`fetch(url)`函数内部的操作不归javascript管，但处理函数的注册与执行、加入任务队列，以及任务队列本身都归javascript管。

## 自己编写基于Promise的API

~~~javascript
function alarm(person, delay) {
  return new Promise((resolve, reject) => {
    if (delay < 0) {
      throw new Error('Alarm delay must not be negative');
    }
    window.setTimeout(() => {  // 将这个看作真正的浏览器使用其他线程或进程执行的网络操作。
      resolve(`Wake up, ${person}!`);
    }, delay);
  });
}
~~~

函数`alarm()`与`fetch()`类似，也是返回一个Promised对象，其他操作也都类似，就不再赘述。

具体看看`alarm()`函数的内部。事实上，`alarm()`就是一个普通函数，如果其内部有一些普通javascript同步代码，javascript就正常同步执行。最后返回一个新创建的Promise对象，Promise对象的构造器需要一个执行器函数，这个函数是Promise异步操作的关键。

> 这个执行器本身采用两个参数，这两个参数都是函数，通常被称作 `resolve` 和 `reject`。在你的执行器实现里，你调用原始的异步函数。如果异步函数成功了，就调用 `resolve`，如果失败了，就调用 `reject`。如果执行器函数抛出了一个错误，`reject` 会被自动调用。你可以将任何类型的单个参数传递到 `resolve` 和 `reject` 中。

总之，执行器函数内通过调用`resole()`和`reject()`设置Promise对象的状态。

这个执行器函数会在 Promise 对象被创建时立即执行，这个动作也是同步的，执行器内部的javascript代码也同步执行，一直到开启真正的异步操作（这里以`window.setTimeout()`进行模拟），这时，由浏览器安排其他线程或进程进行网络或文件读取等操作，然后javascript主线程继续向下执行，直到执行完执行器函数。再之后，就创建完Promise对象了，不过由于`resole()`和`reject()`函数尚未被调用，Promise对象的状态还是`pending`。`alarm()`将创建好的Promise对象返回，然后由`then()`方法注册处理函数，再之后，javascript主线程继续向下执行。

一段时间后，异步操作结束，正如`window.setTimeout()`里所写，由javascript调用`resole()`或`reject()`设置Promise对象状态为`fulfilled`或`rejected`，然后javascript主线程将注册的处理函数或异常处理函数加到任务队列中。后续处理之前已经说过了。虽然不常用，但如果真正来写基于Promise的异步API时，执行器函数中，`resole()`和`reject()`两个函数要写到异步操作里。
