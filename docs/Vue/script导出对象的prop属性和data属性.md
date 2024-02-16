# script导出对象的`props`属性和`data`属性

`props`属性有点像进程的环境变量，你只需要在开启进程前设置好，然后进程就按照设置去执行。而且往往进程不会修改环境变量。`props`属性也是在调用组件的时候传入，然后组件内部永远不应该主动修改`props`属性。

`data`属性类似对象的私有属性，外部调用者不直接与私有属性接触，一个对象的私有属性往往在不断变化。`data`属性也是不与组件调用者接触，在组件内部可通过`props`的值间接得到（比如计算）。

## 整个组件由于`v-if`被重新渲染，是完全被销毁然后再重建

### 必应

您好，这是一个很好的问题。根据我从网上搜索到的信息，v-if指令会根据条件的真假来动态地渲染或移除一个元素 。当条件为真时，元素会被渲染到DOM中；当条件为假时，元素会被完全从DOM中移除 。这意味着，如果一个组件被v-if移除了，它的生命周期钩子函数也会被销毁和重建。因此，被重新渲染的组件内部的data属性会被重新赋值，除非它们是从父组件传递过来的props。
如果您想要保留组件的状态，而不是每次都重新渲染它，您可以使用v-show指令代替v-if指令。v-show指令只是通过CSS的display属性来控制元素的可见性，而不会影响它在DOM中的存在 。这样，组件的data属性就不会被重置了。
希望这能回答您的问题。如果您还有其他问题，请随时提问。😊

参考：[vue文档：v-if vs. v-show](https://cn.vuejs.org/guide/essentials/conditional.html#v-if-vs-v-show)

这个重新渲染导致被销毁还是稍微注意一下，如果条件渲染控制的是组件外部的元素，那么会导致组件被销毁后重建；如果条件渲染是在某个组件的`template`元素里面，那么这个组件并不会被销毁，其`data`等属性得以被保留。如下：

~~~javascript
// ...
    // 组件some-component会被销毁
    <div v-if="false">
        <some-component></some-component>  
    </div>
// ...


// ...
    // 在some-component组件的template内部：
    // some-component组件不会被销毁
    <div v-if="false">
        <other-component></other-component>  
    </div>
~~~
