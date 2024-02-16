# 匿名类和嵌套类的Class对象

## 匿名类

Class对象是反射API的入口，要想启用Java的反射特性，一般首先要获得对应类的Class对象。

创建匿名类的语法：

~~~Java
Object anonymous = new Object() { 
    public void m() {} 
};
~~~

创建了一个继承自`Object`的匿名类，直接实例化了一个对象，并用变量`anonymous`引用了这个变量。

获取匿名类的Class对象，并直接输出`toString()`方法的结果

~~~Java
Class<?> c = anonymous.getClass().getEnclosingClass();
System.out.println(c.toString());
~~~

最后输出的结果是`class test.AnnotationToolTest$1`(变量`anonymous`和`c`都是类`test.AnnotationToolTest`下的静态字段)

这个输出结果*猜测*是和Java匿名类的实现有关，我*猜测*它可能在编译时自动创建了一个名为`test.AnnotationToolTest$1`的有名类。

## 嵌套类

我们再来看看嵌套类Class对象的`toString()`方法输出

在`test.AnnotationToolTest$1`类下有一个静态嵌套类`NestedClass`:

~~~Java
static class NestedClass {
    public void tmp() {}
}
~~~

在enclosing class的main方法中，输出该嵌套类Class对象`toString()`方法的结果：

~~~Java
System.out.println(NestedClass.class.toString());
~~~

结果为`class test.AnnotationToolTest$NestedClass`

同样猜测，这个输出结果应该和Java嵌套类的实现有关，Java可能在编译时自动创建了一个名为`test.AnnotationToolTest$NestedClass`的有名类。就像泛型的类型擦除一样，JVM实际不知道Java有泛型这种特性。

## 验证Java泛型的类型擦除

通过《Java 核心技术 卷一》和Java官网的描述，Java会在编译时对泛型进行类型擦除，因此运行时，就已经没有泛型的类型参数了，类型参数出现的地方会被替换成Object或（有束缚时）上界类型。

反射API正好是在运行时起作用，可以向上面那样运行时输出类的类名，

~~~Java
static List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 4));
~~~

还是`test.AnnotationToolTest$1`类里的静态字段，`list`引用了一个`ArrayList<Integer>`对象，`getClassf()`获取`ArrayList<Integer>`的Class对象，然后再用`toString()`方法输出：

~~~Java
System.out.println(list.getClass().toString());
~~~

结果是`class java.util.ArrayList`，而不是`ArrayList<Integer>`，可以验证，类型参数已经被擦除了。

## Class的类方法getCanonicalName()

对于上面提到的三种情况：匿名类，嵌套类，实例化的泛型类，如果不是直接对它们的Class对象用`toString()`方法输出，而是使用`getCanonicalName()`，情况会有所不同，输出分别是：

- 匿名类：`null`
- 嵌套类：`test.AnnotationToolTest.NestedClass`
- 实例化的泛型类：`java.util.ArrayList`

除了泛型类还是正常类型擦除，其他两个都有所变化：匿名类的规范名直接是null了，嵌套类有规范名。

在[Java官网关于`getCanonicalName()`的说明中](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getCanonicalName--)就提到如果Class对应的类没有规范名的话（[局部类](<https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html> 说人话就是定义在方法里的类)和匿名类，以及没有规范名的类组成的数组），就返回null。规范名是由Java语言规范定义的。

那么看来嵌套类是有规范名的。

## toGenericString()方法输出泛型声明的类型参数（但不能输出泛型实例化时传入的类型参数）

对于泛型类型输出没有类型参数的情况，`toGenericString()`方法输出泛型声明的类型参数。比如对上面的`ArrayList<Integer>`的Class对象使用，输出为`public class java.util.ArrayList<E>`。

至少能表明这是一个泛型类，但还是不能输出泛型实例化时传入的类型参数（在这里是`Integer`）。

`Field`，`Method`，`Constructor`等对象也分别有`toGenericString()`方法，可以输出泛型声明时的类型参数。
