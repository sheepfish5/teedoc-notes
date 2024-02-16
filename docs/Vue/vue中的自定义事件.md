# vue中的自定义事件

## 事件自带参数传入事件处理函数

比如在组件A中，使用`this.$emit("event-name")`触发一个名为`event-name`的自定义事件，按事件冒泡的顺序传递。

如果需要在事件上附加参数，就直接往函数后加参数即可：

~~~javascript
this.$emit("event-name", para1, para2, para3);
~~~

在调用组件A的组件B中，事件处理函数可以直接用这三个参数：

~~~javascript
func1(para1, para2, para3) {
    //  处理三个参数
}
~~~

设置事件处理函数时不需要再把这三个参数再写一遍了。

~~~javascript
@event-name.modifier="func1"
~~~

## 外部参数传入事件处理函数

如果事件处理函数需要组件B的参数，比如：

~~~javascript
// 自定义事件
this.$emit("event-name"); // 这次自定义事件就不带参数了

// 事件处理函数
func2(paraB) { // paraB是组件B要传入的参数
    // 处理paraB
}
~~~

设置事件处理函数时应这么写：

~~~javascript
@event-name.modifier="func2(paraB)"
~~~

## 事件处理函数既需要外部参数，又需要事件附带的参数

关键是在设置事件处理函数时加上`$event`，然后事件附带参数就会被自动处理了。

写法：

~~~javascript
// 自定义事件带两个参数
this.$emit("event-name", para1, para2);

// 若有两个外部参数，那么事件处理函数需要处理四个参数
func3(paraB1, paraB2, para1, para2) {
    // ...
}

// 设置事件处理函数
@event-name.modifier="func3(paraB1, paraB2, $evnet)"
~~~
