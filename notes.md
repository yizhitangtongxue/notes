# 《深入PHP，面向对象、模式与实践》笔记

## 面对对象介绍

1. 面向对象首次出现在PHP3中，1997年8月27日,在那个时期，面向对象只是个边缘话题。

2. PHP4为PHP打下了坚实的基础，在PHP4中，大部分核心代码都被重写了，Zend引擎为PHP提供动力，Zend引擎是PHP的主要组成部分之一，任何可以被调用的PHP函数，都是PHP高级扩展层的一部分，它们一般以完成的工作命名。

3. Zend引擎管理内存，将控制转给其他组件，将PHP语法转换为可执行的字节码(bytecode)，类是由Zend引擎引入的核心特性。

4. 在PHP4中，将对象传值给变量，传递对象到函数，从方法返回对象，都会创建对象的副本，会创建并存在N个相同的对象，而不是对同一个对象的引用，这会导致出现各种Bug，在PHP4中，按引用赋值需要加上&符号。很明显，之后的PHP面向对象，只传递指向对象的引用，而不是对象的副本。

5. PHP5已经完全支持面向对象的程序设计，但不是只支持面向对象，还支持面向过程的程序设计。在PHP5中，最重要的改变是默认按引用传递的行为代替了问题多多的对象赋值、创建N个同一对象副本的方法。面向对象是被认为开发企业级系统的重要方法。

6. PHP6建立在全新的ZendEngine3基础上，提供了Unicode的内在支持，使得PHP更好的支持国际化，有一个被规划在PHP6中的特性，已经在PHP5.3中支持，那就是命名空间，通过命名空间，你就可以在不同的类中，使用相同名称的类，而不会产生冲突。提示返回类型的特性也被规划在PHP6中，这样你就可以在函数的申明中申明它所返回的对象类型了。当然PHP6因为种种原因最终也没有发布。

7. PHP7在性能发面得到了极大的提升，PHP7大概有PHP5.X的两倍的性能。
>后来鸟哥加入到了PHP核心开发项目，鸟哥发起了PHP解释引擎重构的项目，叫做PHPNG，大家可以参考这篇[Wiki](https://wiki.php.net/phpng),PHPNG项目主要是对PHP的引擎进行重构，很快鸟哥的项目组取得了非凡的成就，获得了PHP开发社区的的认可，合并到了PHP的主干，也就是我们现在说的PHP7版本。

## 对象基础

### 类和对象

1. 简单的来说，类是生成对象的代码模板，我们经常用对象来描述类，也经常用类来描述对象。

2. 用class关键字和任意的类名来申明类，类名可以是任意数字和字母的组合，但是不可以用数字开头。

3. 和一个类关联的代码必须用大括号括起来。

4. 创建类的例子：
```php
class ShopProduct {
	//类体
}
```

5. 如果说类是生成对象的模板，那么对象就是根据类中定义的模板所构造的数据。对象可以被说成是类的实例化，它是由类定义的数据类型。

6. new操作符和类名加小括号一起使用，就会生成一个对象。我们使用ShopProduct类作为生成两个ShopProduct对象的模板。虽然功能一样，但是它们都是由同一个类生成的相同类型的不同对象。创建对象的例子：
```php
$product1 = new ShopProduct();
$product2 = new ShopProduct();
```

7. 在PHP脚本中创建的每个对象也有唯一的身份，PHP会在一个进程中重复使用这些身份(或标识符，identifier)来访问这些对象，可以通过var_dump()输出对象来证明这一点。
```php
var_dump($product1);
var_dump($product2);
```
 7. 7-1 通过var_dump()，我们获得了它所包含的有用的信息，每个对象的内部标识符(#号后面的数字)。
```php
object(ShopProduct) #1 (0) {

}
object(ShopProduct) #2 (0) {
	
}
```
 7. 7-2 如果你开启了xdebug扩展，那么应该显示成这样：
```php
// D:\Wamp64\www\oop\1.php:8:
object(ShopProduct)[1]
// D:\Wamp64\www\oop\1.php:9:
object(ShopProduct)[2]
```

8. object是对象数据类型，好比int是整型，bool是布尔型。

9. >在PHP4和PHP5(直到PHP版本5.1)中，直接输出一个对象，得到的是包含对象ID的字符串。从PHP5.2开始，PHP不再支持这个功能，把对象当作字符串处理将会出错，除非在对象的类中定义了__toString()方法。

### 特定的数据字段-属性

1. 属性被成为成员变量，可以用来存放对象之间互不相同的数据。

2. 类的属性和标准的变量很相似，不过必须在申明属性和赋值前加一个代表可见性的关键词(keyword)，关键词可以是public、protected、private，它决定了属性的作用域。

3. 设置类中的属性，例子：
```php
class ShopProduct {
	public $title = "default product";
	//设置了属性的可见性为public，确保我们可以在对象之外访问属性。
	//属性的名称为$title。
	//属性的初始值为default product。
	//实例化类得到的任何对象都将加载默认数据。
}
```

4. >可见性关键字public、protected、private是在PHP5中引入的。如果在PHP4下将无法正常运行。在PHP4中所有的属性都用var关键字申明，效果等同于public。考虑到向后兼容，PHP5中保留了对var的支持，但会自动将var转换为public。

5. 我们可以使用->字符连接对象变量和属性名称，来访问属性变量。例子：
```php
$product1 = new ShopProduct();

echo $product1->title;
// 上面的代码将输出属性$title的值，即default product;
```

6. public关键字使得我们可以给属性赋值(也可以读取属性)，来替换类中设置的默认值。
```php
$product1 = new ShopProduct();
$product2 = new ShopProduct();

$product1->title = "My Antonia";

echo $product1->title;
echo $product2->title;
//product2的title属性值并没有被改变，因为product1和product2是两个相同类型的不同对象。
```

7. >调用类、函数或者方法的代码通常被称为该类、函数或者方法的客户端(client)或者客户端代码(client code)。

8. 使用方法，我们可以让对象可以在内部有处理属性数据的能力，减少动态设置属性出现的问题等等。

9. 在对象外部动态的给对象的属性赋值并不是一个良好的做法，最好使用方法来代替这些操作。

### 使用方法

1. 属性可以让对象存储数据，类方法则可以让对象执行任务。方法是在类中申明的特殊函数。申明方法类似与函数申明。function关键字在方法名之前，方法名之后圆括号中的是可选的参数列表。方法体用大括号括起来。例子：
```php
class ShopProduct {
	public function myMethod($argument,$another) {
		//...方法体
	}
}
```

2. 与函数不同，方法必须在类中申明，方法也可以使用可见性关键字public、protected、private。如果在方法申明中省略了可见性关键字，那么方法的可见性关键字将被隐性的申明为public，即可以在对象之外调用此方法。

3. >PHP4不能识别方法或属性的可见性关键字。增在可见性关键字将会报错。在PHP4中所有的方法都是public类型的。

4. 在大多数情况下，我们可以使用->连接对象变量和方法名来调用方法。调用方法必须使用一对圆括号，即使没有传递任何参数给方法，类似于函数调用。例子：
```php
class ShopProduct {
	public $title 			   = 'default product';
	public $producterMainName  = 'main name';
	public $producterFirstName = 'first name';
	public $price			   = 0;

	public function getProducter() {
		return "{$this->producterFirstName}"."{$this->producterMainName}";
	}
}

$product = new ShopProduct();

// print $product->producterFirstName;
print "author：{$product->getProducter()} \n";
```

5. 在上述代码中，我们使用了一个新特性$this伪变量，$this把类指向一个对象实例。可以这样理解。尝试用“当前实例”替换$this。例子：
```php
	$this->producterFirstName
// 理解为
	// 当前实例的$producterFirstName属性
```
6. 使用了方法，还是无法避免需要做大量重复性的工作，我们无法保证在初始化对象时每个属性都被设置。因此我们需要一个当类实例化对象时可以被自动调用的类方法，即构造函数/构造方法。

#### 使用构造函数/构造方法

1. 当使用new操作符创建对象时，\_\_construct()构造函数将被自动调用。构造函数可以用来确保必要的属性被设置，并且完成任何需要准备的工作。

2. >在PHP5之前的版本中，构造函数使用和所在类相同的名字，因此Shop类可以使用Shop()函数作为它的构造函数。在PHP5中，虽然这种方式依然有效，但应该将构造函数命名为__construct()，注意是两个下划线开头。以后我们看到的PHP类中其他许多特殊的方法也用两个下划线开头。

3. 构造函数命名为__construct()。例子：
```php
class ShopProduct {
	public $title;
	public $producterMainName;
	public $producterFirstName;
	public $price = 0;

	function __construct($title,$firstName,$mainName,$price) {
		$this->title 				= $title;
		$this->producterFirstName 	= $firstName;
		$this->producterMainName    = $mainName;
		$this->$price               = $price;
	}

	function getProducter() {
		return "{$this->producterFirstName} ".
			   "{$this->producterMainName}";
	}
}
// 当使用new操作符创建对象时，_construct()构造函数将被自动调用。
$product = new ShopProduct("My Antonia","Willa","Cather",5.99);
// 使用->连接对象变量和方法名来调用方法。
print "author：{$product->getProducter()}";
```

4. 构造函数使用伪变量$this给对象的每个属性赋值。$this把类指向一个对象实例。

5. >PHP4不会把__construct()方法当作构造函数/构造方法。

#### 参数和类型
