## javadoc简介

javadoc：由源文件生成HTML文档（Java的官方文档就是用javadoc生成的）。
javadoc从源文件中抽取以`/**......*/`为界的==特殊块注释==。
特殊块注释应放置在Java特性（模块，包，public类和接口，public或protected字段，public或protected构造器和方法）前面。
> 如果文档是面向使用类的用户或其他程序员，private特性不需要文档。但事实上，为了作者自己将来重构代码，肯定要为private特性编写注释或其他内部文档。
> protected特性也需要公开文档，这是为了方便继承。
