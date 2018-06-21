#《深入PHP,面向对象、模式与实践》笔记

##1.面向对象首次出现在PHP3中，1997年8月27日,在那个时期，面向对象只是个边缘话题。

##2.PHP4为PHP打下了坚实的基础，在PHP4中，大部分核心代码都被重写了，Zend引擎为PHP提供动力，Zend引擎是PHP的主要组成部分之一，任何可以被调用的PHP函数，都是PHP高级扩展层的一部分，它们一般以完成的工作命名。

##3.Zend引擎管理内存，将控制转给其他组件，将PHP语法转换为可执行的字节码(bytecode)，类是由Zend引擎引入的核心特性。

##4.在PHP4中，将对象传值给变量，传递对象到函数，从方法返回对象，都会创建对象的副本，会创建并存在N个相同的对象，而不是对同一个对象的引用，这会导致出现各种Bug，在PHP4中，按引用赋值需要加上&符号。很明显，之后的PHP面向对象，只传递指向对象的引用，而不是对象的副本。

##5.PHP5已经完全支持面向对象的程序设计，但不是只支持面向对象，还支持面向过程的程序设计。在PHP5中，最重要的改变是默认按引用传递的行为代替了问题多多的对象赋值、创建N个同一对象副本的方法。面向对象是被认为开发企业级系统的重要方法。

##6.PHP6建立在全新的ZendEngine3基础上，提供了Unicode的内在支持，使得PHP更好的支持国际化，有一个被规划在PHP6中的特性，已经在PHP5.3中支持，那就是命名空间，通过命名空间，你就可以在不同的类中，使用相同名称的类，而不会产生冲突。提示返回类型的特性也被规划在PHP6中，这样你就可以在函数的申明中申明它所返回的对象类型了。当然PHP6因为种种原因最终也没有发布。

##7.PHP7在性能发面得到了极大的提升，PHP7大概有PHP5.X的两倍的性能，>后来鸟哥加入到了PHP核心开发项目，鸟哥发起了PHP解释引擎重构的项目，叫做PHPNG，大家可以参考这篇[wiki](https://wiki.php.net/phpng),PHPNG项目主要是对PHP的引擎进行重构，很快鸟哥的项目组取得了非凡的成就，获得了PHP开发社区的的认可，合并到了PHP的主干，也就是我们现在说的PHP7版本。