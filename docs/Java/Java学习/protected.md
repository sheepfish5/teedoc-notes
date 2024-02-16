超类中protected方法或字段：

1. 只允许自己和子类访问
2. 若子类和超类不在同一个包中，子类中的方法只能访问子类实例的protected字段或方法，而不能访问超类实例的protected字段或方法

比如，tmp1包中有一个SuperClass类，它是tmp2包中SubClass类的超类。
SuperClass类有两个protected字段state1和state2，还有一个private字段p1，这个private字段配有一个protected访问器getp1()。
SubClass类有一个方法`void test(SuperClass sup, SubClass sub)`，在这个方法中是无法调用sup.getp1()和sup.state2的，可以正常调用sub.getp1()和sub.state2。这就表示了第二条：子类和超类不在同一个包中，子类方法中只能访问子类实例的protected字段或方法，而不能访问超类实例的protected字段或方法
