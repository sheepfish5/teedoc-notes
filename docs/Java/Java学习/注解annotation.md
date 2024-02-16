## 定义

annotation（注解），一种元数据，提供一个程序的相关信息，但它本身并不是程序的一部分。annotation对它所注解的代码的动作没有直接影响。

## 三种annotation

1. 提供给编译器的信息——方便编译器错误检查或suppress warnings
2. 编译时和开发时处理——有专门的软件工具（如Javadoc）通过annotation生成代码、XML文档等等
3. 运行时处理——有些annotation在运行时发挥作用

## annotation格式

基本格式：

~~~Java
@Override
void mySuperMethod() {...}
~~~

以@符开头表示annotation
annotation可以包含有名字或无名字的elements：

~~~Java
@Author(
 name = "Benjamin Franklin"
 date = "3/27/2003"
)
class MyClass {...}
~~~

如果只有一个名为value的element，可以直接省略value：

~~~Java
@SuppressWarings("unchecked")
~~~

如果一个element都没有，那就不用带括号，就像`@Override`一样。
可以对一个声明用多个注解：

~~~Java
@Author(name = "Jane Doe")
@EBook
class MyClass {...}
~~~

有的annotation类型已经在Java SE API的java.lang或java.lang.annotaion包中预定义好了。我们也可以定义自己的annotation类型，前面例子中的Author和EBook就是自定义的。

## annotations可以用在哪

annotations可以用在各种声明中，包括类、字段、方法和其他程序元素的声明。
约定俗成：当annotations被用于声明中时，每个annotation单独占一行。

## 声明一个annotation类型

可以用annotation替代部分代码中的注释
假设一个软件开发小组传统地在每个类的前面加上下面的注释来提供重要信息：

~~~Java
public class Generation3List extends Generation2List {
 // Author: John Doe
 // Date: 3/17/2002
 // Current revision: 6
 // Last modified: 4/12/2004
 // By: Jane Doe
 // Reviewers: Alice, Bill, Cindy

 // class code goes here
}
~~~

为了用annotation替代注释来提供这些信息，我们需要首先定义一个annotation类型，语法如下：

~~~Java
@interface ClassPreamble {
 String author();
 String date();
 int currentRevision() default 1;
 String lastModified() default "N/A";
 String lastModifiedBy() default "N/A";
 // Note use of array
 String[] reviewers();
}
~~~

interface关键字的使用让annotation定义看起来和接口类型定义类似，但前者用的是`@interface`。
定义的body包含该annotation类型的elements的声明，elements的声明看起来很像方法的声明。可以用default关键字提供可选的默认值。
定义完annotation类型后，我们就可以使用定义好的annotation了，注意填上elements的值：

~~~Java
@ClassPreamble (
 author = "John Doe",
 date = "3/17/2002",
 currentRevision = 6,
 lastModified = "4/12/2004",
 lastModifiedBy = "Jane Doe",
 //Note array notation
 reviewers = {"Alice", "Bob", "Cindy"}
)
public class Generation3List extends Generation2List {
 // class code goes here
}
~~~

如果要让`@ClassPreamble`的内容出现在Javadoc生成的文档中，必须在`@ClassPreamble`的定义前使用`@Documented`：

~~~Java
// import this to use @Documented
import java.lang.annotation.*;

@Documented
@interface ClassPreamble {
 // annotation element definitions
}
~~~

## 预定义好的annotation类型

一系列annotation类型已经在Java SE API中预定义好了。有些annotation类型被编译器使用，有些则被其他annotation类型调用。

### 被Java语言使用的annotation

预定义在java.lang包中的类型是`@Deprecated`（已弃用）、`@Override`（重载）和`@SuppressWarnings`（删除警告）。
`@Deprecated`类型表示被标记的Java特性已被弃用。编译器会生成一个warning不管何时有程序使用了被标记的方法、类和字段。当一个Java特性已被弃用时，还应该用Javadoc的`@deprecated`标签记录下来：

~~~Java
// Javadoc comment follows
/**
 * @deprecated
 * explanation of why it was deprecated
 */
@Deprecated
static void deprecatedMethod() {...}
~~~

@符同时在Javadoc注释和annotation中被使用并不是巧合：他们在概念上是相关联的。同时，Javadoc标签使用小写字母d，而annotation使用大写字母D。
`@Override`类型告知编译器这个Java特性诣在重载超类中的Java特性：

~~~Java
// mark method as a superclass method
// that has been overridden
@Override
int overriddenMethod() {...}
~~~

虽然并不强制要求在每个重载Java特性前加上`@Override`，但这有助于防止错误。如果一个被`@Override`标记的方法没有成功重载超类的方法，编译器会生成一个错误。
`@SuppressWarnings`类型告诉编译器删除特定的本来会生成的warnings。在下面的例子中，使用了一个已被弃用的方法，编译器会生成一个warning，但我们使用的`@SuppressWarnings`可以让这个warning被抑制。

~~~Java
// use a deprecated method and tell
// compiler not to generate a warning
@SuppressWarnings("deprecation")
void useDeprecatedMethod() {
 // deprecation warning
 // - suppressed
 objectOne.deprecatedMethod();
}
~~~

每个编译器warning都属于一个类别，Java语言规范列出了两个类别：deprecation和unchecked类别。unchecked警告发生在处理遗留代码的时候，这些遗留代码是在generics（泛型）出现之前被写的。如果想同时抑制两种类型的warning，使用下面的语法：

~~~Java
@SuppressWarnings({"unchecked", "deprecation"})
~~~

`@SafeVarargs`类型，当被用在一个方法或构造器上时，断言这段代码不会执行关于varargs参数的不安全操作。当这个annotation被使用时，与varargs参数使用相关的unchecked warnings会被抑制。
`@FunctionalInterface`类型，在Java SE 8中出现，表示下面的声明将会是Java语言规范定义的一个函数型接口。

### 被其他annotation调用的annotation

被其他annotation调用的annotation被称为meta-annotation。下面是几种预定义在java.lang.annotation包中的meta-annotation。
`@Retention`类型规定了被标记的annotation的存储方式：

- RetentionPolicy.SOURCE——被标记的annotation只保留在源文件层，而会直接被编译器忽略。
- RetentionPolicy.CLASS——被标记的annotaion在编译时仍被编译器保留，但会被JVM忽略。
- RetentionPolicy.RUNTIME——被标记的annotation会被JVM保留，因此它可以被运行时环境使用。
`@Documented`类型表示不论何时使用了被标记的annotation，其elements都会被Javadoc记录。
`@Target`类型标记另一个annotation类型，以限制这个annotation类型能够被用于的Java特性。
`@Repeatable`类型，引入于Java SE 8，表示被标记的annotation类型可以在一个声明之上重复使用多次。

## Type Annotations and Pluggable Type Systems
