解释如何将类和接口打包进package，如何使用在package中的类，如何管理文件系统使得编译器可以找到源文件。

## 为什么要使用package

为了让Java types更容易找到和使用、避免命名冲突和控制访问，程序员将相关的类型装进package中。
> [!Note] Definition
> 一个package是一组提供访问保护和命名空间管理的相关联Java types。注意这里的types指的是class、interface、enumeration还有annotation类型。enumeration和annotation类型分别是特殊的class和interface，因此在这里，types指的是class和interface。

作为Java平台关键部分的types，是各种按功能打包class的package的成员：比如基础class在java.lang包里，用于读写（输入输出）的class在java.io里，等等。我们也可以把自定义的types放进package里。
假设我们写了一组表示图形对象的class，比如圆、矩形、线和点，我们还写了一个interface——Draggable，之前的class如果要能被鼠标拖动就要实现这个接口：

~~~Java
// in the Draggable.java file
public interface Draggable {
 ...
}

// in the Graphic.java file 
public abstract class Graphic {
 ...
}

// in the Circle.java file 
public class Circle extends Graphic 
 implements Draggable {
 ...
}

// in the Rectangle.java file
public class Rectangle extends Graphic
 implements Draggable {
 ...
}

// in the Point.java file
public class Point extends Graphic
 implements Draggable {
 ...
}

// in the Line.java file
public class Line extends Graphic
 implements Draggable {
 ...
}
~~~

出于不少于以下几个原因，我们应该将这些class和这个interface装进一个包：

- 所有人可以一目了然这些types是相关的；
- 所有人可以知道在哪找到提供图形相关方法的types；
- package创建了一个命名空间，因此我们types的名字不会和其他package的types的名字冲突；
- package中的types对package中的另一个type访问没有限制，但是这个package之外的types访问package内的types是有限制的。

## 创建一个package

要创建一个package，首先要确定这个package的名字（命名规范在后面会提到），然后在这些包含types的源文件的顶部加上一条语句：

~~~Java
package packagename;
~~~

这条语句必须在源文件的第一行！每一个源文件只能有一条语句，这会对文件中的所有types起作用。
> [!Note]
> 如果要放多个types在一个源文件中，只有一个type可以为public，并且这个type的名字必须和源文件相同。
> 虽然可以将non-public types和public type放在同一文件中（强烈不推荐除非non-public types特别小并且和public type有紧密联系），但只有public type可以被package之外的types访问。所有的top-level non-public types都是*package private*（可被package之内的types访问，不可被package之外的types访问）

如果将之前的一个interface和五个class装进一个名为graphics的package中，会得到下面这样的六个源文件：

~~~Java
// in the Draggable.java file
package graphics;
public interface Draggable {
 ...
}

// in the Graphic.java file 
package graphics;
public abstract class Graphic {
 ...
}

// in the Circle.java file 
package graphics;
public class Circle extends Graphic 
 implements Draggable {
 ...
}

// in the Rectangle.java file
package graphics;
public class Rectangle extends Graphic
 implements Draggable {
 ...
}

// in the Point.java file
package graphics;
public class Point extends Graphic
 implements Draggable {
 ...
}

// in the Line.java file
package graphics;
public class Line extends Graphic
 implements Draggable {
 ...
}
~~~

（唯一变化就是每个文件顶部多了一条`package graphics;`语句）
如果没有`package graphics;`语句，这个类型最终属于一个无名package。一般来说，无名package只被用于小而临时的用途，或者，刚刚开始进行开发。否则，应将class和interface放在命名了的package中。

## 为package命名

由于全世界有广泛的程序员使用Java语言来写class和interface，非常可能许多程序员为不同的types使用相同的名字。事实上，在上面的例子中就已经出现了：我们定义了一个Rectangle class，但在java.awt package中已经存在了一个Rectangle class了。但如果这两个class在不同package中，编译器就允许它们同名。每个Rectangle class的fully qualified name（完全限定名）包括package名。在graphics package中的Rectangle的fully qualified name是graphics.Rectangle，java.awt package中的Rectangle class的fully qualified name是java.awt.Rectangle。
这样看来很有效，除非两个独立的程序员使用同一个package名。如何防止这种情况发生呢？命名规范。

### 命名规范Naming Conventions

package名全部使用小写字母以防和class名或interface名冲突。

公司使用该公司的翻转了的互联网域名作为package名的开头：比如，package名`com.example.mypackage`是example.com下的程序员创建的一个名为mypackage的package。
公司内部的命名冲突需要由公司内部的规范自行解决，可能通过加上地区或项目名称来解决：`com.example.region.mypackage`。

Java语言本身的package以java.或javax.开头。

一些情况下，域名不是一个有效package名。当域名包含一个连字符或其他特殊字符时，或当package名以数字或其他不能作为Java命名开头的字符时，又或者package包含Java关键字时，域名不是一个有效的package名。在这种情况下，推荐的规范是加一个下划线：

| Domain Name | Package Name Prefix |
| --- | --- |
| hyphenated-name.example.org | org.example.hyphenated_name |
| example.int | int_.example |
| 123name.example.com | com.example.\_123name |

---

## 使用package成员

组成package的types被称为package members（成员）。

要从外部使用一个public package member，必须通过以下方式之一：

- 使用fully qualified name描述这个member；
- 使用`import`关键字导入该package member；
- 使用`import`关键字导入包含该package member的整个package。

每一种方法都有适用的不同情况。

### 使用fully qualified name
