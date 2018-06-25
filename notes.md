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

1. PHP是一种弱类型语言，即变量不需要申明为特定的数据类型。

2. 整型、字符串、布尔型等数据类型被称为基本类型。
>从更高层次来说，每个类都定义了一种数据类型。因此ShopProduct对象属于基本类型，但同时也属于ShopProduct类这一类型。

3. 在PHP中，定义方法和函数时不需要指定参数的数据类型，即参数可以是任意类型，这一种便利，也是一种麻烦。

#### 基本类型

1. 在PHP中不是强制性的要设置参数的数据类型。每个赋给变量的值都有一种数据类型。可以使用PHP的类型检测函数来确定变量的类型。例子：

类型检查函数  | 类型    | 描述
------------ | -----  | -----
is_bool()    | 布尔型  | 值为true或false 
is_integer() | 整型    | 整型
is_double()  | 双精度型| 浮点数(有小数点)
is_string()  | 字符串  | 字符数据
is_object()  | 对象    | 对象
is_array()   | 数组    | 数组
is_resource()| 资源    | 用于识别和处理外部资源(如数据库或文件)的句柄
is_null()    | NULL    | 未分配的值

2. 也可以用var_dump()*查询*数据类型。一般来说，上面的代码是配合if语句用来*检测*某个数据是否存在或匹配而使用的。

#### 获得提示：对象类型

1. 正如参数变量可以包含任何基本类型的数据，参数默认情况下也可以包含任何类型的对象。

2. 假设有一个设计为*接收ShopProduct对象*的方法。例子：
```php
class ShopProduct {
	public $title;
	public $price = 5;
	// 构造函数，设置初始值。
	public function __construct($myTitle,$myPrice) {
		$this->title = $myTitle;
		$this->price = $myPrice;
	}

	public function getProduct() {
		return "{$this->title}";
	}
}

class ShopProductWriter {
	// 现在write方法只接收包含类名为ShopProduct对象的$ShopProduct参数
	public function write (ShopProduct $ShopProduct) {
		$str = "{$ShopProduct->title}: ".
			   $ShopProduct->getProduct().
			   "({$ShopProduct->price})\n";
		print $str;
	}
}
// 构造函数在使用new操作符生成对象时自动调用
$product1 = new ShopProduct('myTitle',5);
$write = new ShopProductWriter();
$write->write($product1);
// 在这个例子中，大小写貌似是不受限制的。
```
2. 2-1 如果传入了的参数为*错误的类名所实例化的对象*，则报错：
```php
// Catchable fatal error: Argument 1 passed to ShopProductWriter::write() must be an instance of ShopProduct, null given, called in D:\Wamp64\www\oop\1.php on 
// line 28 and defined in D:\Wamp64\www\oop\1.php on line 18
```

#### 继承extends

1. 继承是从一个基类得到一个或者多个派生类的机制。

2. 继承自另一个类的类被称为该类(父类或者说是超类)的子类。这种关系通常用父亲和孩子来比喻。

3. 子类将继承父类的特性，这些特性由属性和方法组成。子类可以增加父类(也称为超类，superclass)之外的新功能，因此子类也被称为父类的“扩展”。

4. instanceof操作符可以与if语句配合，判断一个对象是否是一个类的实例化。

5. 继承可以解决代码重复性。继承能为每种类型提供一个单独的功能，同时希望提供公共的功能来避免重复，又允许每种类型处理不同的方法调用。这正是我们使用继承的原因。

#### 使用继承

1. >创建继承树的第一步是找到现有基类元素中不适合放在一起，或者不需要进行特殊处理的类方法。

2. 子类默认继承父类所有的public和protecetd方法或属性。

3. 子类是无法继承private方法或者属性的。

4. 子类继承父类的构造函数时，若属性中已经有了默认值，在使用new操作符创建对象时，可以不加这个属性，不会报错。

#### 构造方法和继承

1. >要调用父类的方法，首先要找到一个引用类本身的途径，句柄(handle)。PHP为此提供了parent关键字。

2. >要引用一个类而不是对象的方法，可以使用::而不是->。例子：
```php
	function __construct($title,$firstName,$mainName,$price,$numPages) {
		parent::__construct($title,$firstName,$mainName,$price);
		$this->numPages = $numPages;
	}
```

3. 每个子类都会在设置自己的属性前调用父类的构造方法。基类现在仅知道自己的数据。子类一般是父类的特例。我们应该避免告诉父类任何关于子类的信息，这是一条经验规则。

4. 在子类中定义构造方法时，需要传递参数给父类的构造方法，否则你构造的可能时一个不完整的对象。

#### public、private、protected：管理类的访问

1. >类中的元素可以被申明为public、private、protected。

2. >在任何地方都可以访问public属性或方法。

3. >只能在当前类中才能访问private方法或属性，即使在子类中也不能访问。

4. >可以在当前类或子类中访问protected方法或属性，其他外部代码无权访问。

5. >最好将类属性初始化为private或protected，然后在需要的时候再放松限制条件。类中许多方法都可以时public，但是如果拿不定主意的话，就限制一下。有些类方法只为类中其他方法提供本地功能，与类外部的代码没有任何关系，应该将其设置为private或protected。

6. 当程序员需要使用类中保存的值时，通常比较好的做法是不要允许直接访问属性，而是提供方法来取得需要的值，这样的方法被称为访问方法，也可以被称为getter和setter。

7. 可以提供public的方法来获取private或protected的属性。

8. 不要允许直接访问属性值，最好通过方法来访问。

## 高级特性

### 静态方法和属性

1. 类当作生成对象的模板，把对象作为活动组件，对象的方法可以被调用，对象的属性可以被访问。

2. 静态方法是以类作为作用域的函数。**静态方法不能访问这个类中的普通属性，因为那些属性属于对象，但是可以访问静态属性**。

3. 通过类而不是实例来访问静态元素，所以访问静态元素时不需要引用对象的变量，而是使用::来连接类名和属性或类名和方法。

4. 要从当前类(不是子类)中访问静态方法或属性，可以使用self关键字。例子：
```php
class ShopPrduct {
	static private $price = 100;
	static public function price() {
		return self::$price;
	}
}


print ShopPrduct::price();
```

5. self关键字指向当前类，就像伪变量$this指向当前对象一样。

6. 在类外部可以使用::连接类名和属性或类名和方法来访问属性或方法。在类内部(当前类)可以使用self关键字加::加属性或方法来访问属性或方法。

7. 根据定义，我们无法在对象中调用静态方法，因此静态方法和属性又被称为*类变量和属性*。因此我们也无法在静态方法中使用伪变量$this。

8. 静态属性和静态方法是全局的，只要类是可访问的。

9. 在类的外部，静态属性和方法无法被对象调用。但是在实例化对象之前的类中是可以被调用的。

### 常量属性

1. 类常量一旦设置就不能被改变。常量属性用const关键字申明。例子：
```php
class ShopProduct {
	const AVAILABLE = 0;
	const OUT_OF_STOCK = 1;
}

print ShopProduct::AVAILABLE;
```

2. 按照惯例，只能用大写字母来命名常量。

3. 常量属性只包含基本数据类型的值。不能将一个对象指派给常量。

4. 像静态元素一样，常量只能通过类而不能通过类的实例访问。

5. 给已经申明过的常量赋值会引起解析错误。

6. 当需要在类的所有实例中都能访问某个属性，并且属性值无需改变时，应该使用常量。

### 抽象类

1. >引入抽象类(abstract class)是PHP5的一个主要变化。这个新特性正是PHP朝面向对象设计发展的另一个标志。

2. >抽象类不能被直接实例化。抽象类中只定义(或部分实现)子类需要的方法。子类可以继承它并且通过它实现其中的抽象方法，使抽象类具体化。

3. 使用abstract关键字定义个抽象类。

4. 抽象方法用abstract关键字申明，**其中不能有具体内容**，要以分号结尾而不是方法体。例子：
```php
	// 创建一个抽象类。
abstract class ShopProductWriter {

	// 申明抽象方法write()，以分号结束，并且其中不能有具体内容。
	abstract public function write();
}
```

5. **创建抽象方法后，要确保所有子类中都必须实现了该方法，但实现的细节可以先不确定。**

6. >抽象类的每个子类都必须实现抽象类中的所有抽象方法，或者把它们自己也申明为抽象方法。扩展类不仅仅负责简单实现抽象类中的方法，**还必须重新申明方法。**

7. >新的实现方法的访问控制不能比抽象方法的访问控制更严格。新的实现方法的参数个数应该和抽象方法的**参数个数一样**。例子：
```php
	// 创建一个抽象类。
abstract class ShopProductWriter {

	// 申明抽象方法write()，以分号结束，并且其中不能有具体内容。
	abstract public function write();
}

class abstractProduct extends ShopProductWriter {
	// 抽象类的每个子类都必须实现抽象类中的所有抽象方法
	public function write() {
	// 方法体
	}
}
```

8. 在PHP5中，抽象类在被解析时就被检测，所以更加安全。

9. 抽象类中可以定义属性和方法，可以包含静态方法，静态方法可以有方法体，而抽象方法不能有方法体，且需要以分号结束。

### 接口

1. 抽象类提供了具体实现的标准，而接口(interface)则是纯粹的模板。**接口只能定义功能，而不包含实现的内容**。

2. 接口可以用关键字interface来申明。**接口可以包含属性和方法申明，但是方法体为空。**例子：
```php
interface Chargeable {
	public function getPrice();
}
```

3. 任何实现接口的类都要实现接口中所定义的所有方法，否则该类必须申明为abstract。

4. 一个类可以在申明中使用implements关键字来实现某个接口。例子：
```php
interface Chargeable {
	public function getPrice();
}

class ShopProduct implements Chargeable {
	public function getPrice() {
	}
}
```

5. >PHP只支持继承一个父类，因此extends关键字只能在一个类名之前。

6. >一个类可以同时继承一个父类夫实现N个接口。extends子句应该在implements子句之前。例子：
```php
class test extends A implements book,cd {
	// ...
}
```

7. 接口可以将需要的方法关联起来。

8. 接口只能继承接口，不能继承类。不然会提示it is not an interface。

9. 类可以同时继承一个类和实现多个接口。

10. 抽象类是一个具体实现的约束标准，而接口是定义功能的模板。

### 静态延迟绑定：static关键字

1. 工厂方法是生成包含类的实例的一种方法。

2. 调用create()方法，即可将本类实例化。例子：
```php
abstract class DomainObject {
	public static function create() {
		return new static();
	}
}




class User extends DomainObject {}
```

3. 使用static::方法调用一个方法的时候，被调用的方法必须是静态的。

4. static::可以作为调用**静态方法**的标识符。例子：
```php
abstract class DomainObject {
	private $group;
	public function __construct() {
		$this->group = static::getGroup();
	}

	public static function create() {
		return new static();
	}

	public function getGroup() {
		return "default";
	}
}

class User extends DomainObject {

}
```

### 异常

1. >PHP5中引入了异常(exception)。异常是从PHP5内置的Exception类(或子类)实例化得到的特殊对象。Exception类型的对象用于存放和报告错误信息。

2. Exception类的构造方法接受两个可选的参数：消息字符串和错误代码。

3. Exception类提供了一些游泳的方法来分析错误条件。Exception类的public方法：
方法  | 类型
---- | -----
getMessage() | 获得传递给结构方法的消息字符串
getCode()    | 获得传递给构造方法的错误代码
getFile()    | 获得产生异常的文件
getLine()    | 获得产生异常的行号
getPrevious  | 获得一个嵌套的异常对象
getTrace()   | 获得一个多维数组，这个数组追踪导致异常的方法调用，包含方法、类、文件和参数数据
getTraceAsString() | 获得getTrace()返回数据的字符串版本
\_\_toString()     | 在字符串中使用Exception对象时自动调用。返回一个描述异常细节的字符串

#### Try、throw 和 catch

1. 适当的处理异常代码应该包括：
Throw - 里规定如何触发异常。每一个 "throw" 必须对应至少一个 "catch"。
Try - 使用异常的函数应该位于 "try" 代码块内。如果没有触发异常，则代码将照常继续执行。但是如果异常被触发，会抛出一个异常。
Catch - "catch" 代码块会捕获异常，并创建一个包含异常信息的对象。

```php
// 创建一个有异常处理的函数
function checkNum($number)
{
    if($number>1)
    {
        throw new Exception("变量值必须小于等于 1");
    }
        return true;
}
// 在 try 块 触发异常
try
{
    checkNum(2);
    // 如果抛出异常，以下文本不会输出
    echo '如果输出该内容，说明 $number 变量';
}
// 捕获异常
catch(Exception $e)
{
    echo 'Message: ' .$e->getMessage();
}
```

### 更多的PHP异常处理可以参考[此处](http://www.runoob.com/php/php-exception.html)

1. 简单的来说，就是先检测异常throw，再触发异常try，再捕捉异常catch，抛出错误。

### Final类和方法

1. final关键字只能被用于类和方法。

2. final关键字可以终止类的继承。final类不能有子类，final方法不能被覆写。例子：
```php
final class A {

}

class B extends A {

}

// 报错 Fatal error: Class B may not inherit from final class (A) in D:\Wamp64\www\oop\8.php on line 9
```

3. 如果只是再A类中申明某个方法为final，而不是将整个类申明为final，那么继承A类就不会出现致命错误。例子：
```php
class A {
	final function totalize() {
		
	}
}

class B extends A {

}
// 不会报错
```
```php
class A {
	final function totalize() {

	}
}

class B extends A {
	// 覆写final方法会导致报错
	function totalize() {
		
	}
}
// 报错Fatal error: Cannot override final method A::totalize() in D:\Wamp64\www\oop\8.php on line 13
```
4. 如果申明了一个final的属性。就会报错，并说明final只能用于申明类和方法。例子：
```php
class A {
	final $a;
}

class B extends A {

	}
}
// 报错Fatal error: Cannot declare property A::$a final, the final modifier is allowed only for methods and classes
```

5. final关键字应该放在其他修饰词之前(如protected或者static)。

6. 当程序已经发布之后，请谨慎使用final。

### 使用拦截器

1. 拦截器(interceptor)可以拦截发送到*未定义方法和属性*的消息。也被称为重载(overloading)。

2. \_\_get()和\_\_set()方法用于处理类(或者父类)中**未申明的属性**。

3. \_\_get()方法。**当客户端代码试图访问未申明的属性时，__get方法会被自动调用。**例子：
```php
class Person {
	// 设置了俩私有属性
	private $name;
	private $age;
	// 当客户端代码试图访问未申明的属性时，__get方法会被自动调用。
	function __get($property) {
		return '您访问的属性未被申明';
	}
}
$Person = new Person();
// sex属性是不存在的，所以会提示'您访问的属性未被申明'
print $Person->sex;
```

4. \_\_isset()方法。**当客户端代码试图对一个未申明的属性调用isset()方法时，__isset方法会被自动调用**例子：
```php
class Person {
	// 设置了俩私有属性
	private $name;
	private $age;
	// 当客户端代码试图对一个未申明的属性调用isset()方法时，__isset方法会被自动调用
	function __isset($property) {
		print '你正在对一个未申明的属性调用isset()方法！';
	}
}
$Person = new Person();

// 此时调用isset()方法，传入的属性并未被申明，所以会报错
if(isset($Person->sex)) {

}
```

5. \_\_set()方法。**在客户端代码试图给未申明的属性赋值时被调用。**例子：
```php
class Person {
	// 设置了俩私有属性
	private $name;
	private $age;

	function __set($property,$value) {
		print '你正在对一个未申明的属性赋值！';
	}

	function setUnknow() {
		// $this代表当前对象
		// sex是一个未申明的属性
		// $this->sex的意思是，当前对象下未申明的sex属性
		$this->sex = '女';
		// sex代表了$property，女代表了$value
	}
}
$Person = new Person();

$Person->setUnknow();
// 调用setUnKnow方法对sex(未申明的属性)进行赋值。会提示'你正在对一个未申明的属性赋值！'
}
```

6. \_\_unset()方法。**当客户端代码试图对一个未申明的属性调用unset()方法时**，\_\_unset()方法会被调用。例子：
```php
class Person {
	// 设置了俩私有属性
	private $name;
	private $age;

	function __unset($property) {
		print '你正在对一个未申明的属性使用unset()方法!';
	}

	function getUnSet($property) {
		unset($this->$property);
	}
}

$Person = new Person();
// getUnSet方法接收一个属性参数。sex属性未被申明，所以会报错'你正在对一个未申明的属性使用unset()方法!'
$Person->getUnSet('sex');
```

7. \_\_call()方法可能是最有用的拦截器方法。**当客户端代码要调用类中未定义的方法时，\_\_call()方法会被调用。**例子：
```php
	// 第一个参数方法名在调用未申明的方法时，未申明的方法就是方法名，所以第一个参数不用填。
	// 第二个参数是方法需要接收的所有参数。
	function __call($method,$args) {
		// 参数是一个一维数组，所以需要用逗号来分割数组，生成一个字符串，这样才能被打印。
		print '方法名：'.$method.'<br>参数名：'.implode(', ', $args);
	}
}
$Person = new Person();
// 因为调用了一个不存在的(未申明的)方法，触发并调用了__call()方法。所以报错：
// 方法名：getMethod
// 参数名：aaa
$Person->getMethod('aaa');
```

8. \_\_call()方法还有更强大的用处，比如**实现委托**。*太高级还玩不来。*

9. **method_exists()函数接受对象和方法名作为参数。检查方法在对象中是否存在**。

### 析构方法

1. 在实例化对象时，\_\_construct()方法会被自动调用。PHP5也提供了一个对应大的方法\_\_destruct()。

2. \_\_destruct()方法只在对象被垃圾收集器收集前(即对象从内存中删除之前)自动调用。

3. 你可以利用这个方法进行最后必要的清理工作。

4. 销毁Person对象。例子：
```php
function __destruct() {
	//....
}
```

5. 调用unset函数应该会触发析构方法。

### \_\_clone()复制对象

1. **在PHP中，对象的赋值和传递都是通过引用进行的。**

2. 在一个对象上调用clone关键字时，其\_\_clone()方法就会被自动调用。

3. 可以通过\_\_clone()方法控制复制什么。例子：
```php
class Person {
	private $name;
	private $age;
	private $id;
	private $infomation = Array();
	public $account;

	function __construct($name,$age, Account $account) {
		$this->name = $name;
		$this->age = $age;
		$this->account = $account;
	}

	function setId($id) {
		$this->id = $id;
	}
	//当客户端调用clone关键字时，此方法会被自动调用
	//并设置id为0，防止两个不同对象指向数据库中的同一数据
	function __clone() {
		$this->id = 0;
		// 显式地在\_\_clone()方法中复制指定的对象
		$this->account = clone $this->account;
	}

	function getInformation() {
		$this->infomation = ['id'=>$this->id,'name'=>$this->name,'age'=>$this->age,'account'=>$this->account];
		return $this->infomation;
	}
}

class Account {
	public $balance;
	function __construct($balance) {
		$this->balance = $balance;
	}
}


$person = new Person('Bob',44,new Account(200));
$person->setId(343);
var_dump($person->getInformation());
// 结果
// D:\Wamp64\www\oop\9.php:30:
// array (size=3)
//  'id' => int 343
//  'name' => string 'Bob' (length=3)
//  'age' => int 44



// 使用clone关键字复制对象，此时会自动调用__clone()方法。产生一个新副本：对象$person2，并将id设置为0
$person2 = clone $person;
var_dump($person2->getInformation());

//D:\Wamp64\www\oop\9.php:34:
// array (size=3)
//  'id' => int 0
//  'name' => string 'Bob' (length=3)
//  'age' => int 44
//  
//  
// 给person增加余额，此时已经在__clone()方法中改变了复制的新副本获得值的方式，现在已经不会与原对象共享数据了
$person->account->balance +=10;
// 测试
var_dump($person->getInformation());
var_dump($person2->getInformation());


```

4. \_\_clone()方法复制出来的新对象是一个指向原对象的引用。也就是说，更改了原对象的属性值，副本对象也会同时改变值。如果不希望这样，可以显式地在\_\_clone()方法中复制指定的对象。例子：
```php
function __clone() {
	$this->account = clone $this->account;
}
```
5. 可以通过对象属性保存另一个对象。

### 定义对象的字符串值

1. PHP5中引入的另一个受JAVA启发的功能是_toString()方法。在PHP5.2*之前*可以直接打印对象，PHP会把对象解析成一个字符串来输出。

2. 从PHP5.2开始(以及之后)，直接打印一个对象会报错。例子：
```php
class StringThing {}
$st = new StringThing();
print $st;
// 报错：
// Catchable fatal error: Object of class StringThing could not be converted to string in D:\Wamp64\www\oop\11.php on  line 4
```

3. 当把一个对象传递给print或者echo时，会自动调用\_\_toString()方法，返回一个字符串。我们可以通过此方法控制字符串输出的格式。例子：
```php
class Person {
	function getName() {return "Bob";}
	function getAge() {return 44;}
	function __toString() {
		$desc = $this->getName();
		$desc .= " (age ".$this->getAge().")";
		return $desc;
	}
}

$person = new Person();
print $person;
```

4. >对于日志和错误报告，\_\_toString()方法非常有用，\_\_toString()方法也可以用于设计专门用来传递信息的类，比如Exception类可以把关于异常数据的总结信息写道\_\_toString()方法中。

### 回调、匿名函数和闭包

1. 回调：通俗的解释就是把函数作为参数传入进另一个函数中使用。

2. 匿名函数：>也叫闭包函数（closures），允许临时创建一个没有指定名称的函数。最经常用作回调函数（callback）参数的值。
```php
 $log=function(){};
 ```

3. 匿名函数中的use关键字，查看[这里](https://blog.csdn.net/qq_25600055/article/details/78879816)。

4. **回调函数的关键是匿名函数？**