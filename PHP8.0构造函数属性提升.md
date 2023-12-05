1、php8.0 构造函数属性提升

```
class A{
	public function __construct(
		public string $name,
		public string $email,
		public DateTimeImmutable $birth_date,
	){}
}

```

2、基本思想很简单： 放弃类属性和变量赋值，并在构造函数参数前加上public、protected或private.

3、php将采用新的语法，在执行代码之前，运行时转译自身（并缓存结果以获得更好的性能）。这是有趣的想法，考虑到前一章关于静态分析以及我分享了语言在静态期间添加，仅在静态分析期间使用功能的愿景。

4、只能用在构造函数。但是不能用抽象构造函数里，虽然实际中没用过抽象构造函数，也不知道能做什么。（chatgpt说构造函数不能声明成抽象），虽然在trait中的构造函数不报错，但是不常用。

![[Pasted image 20231110033209.png]]
![[Pasted image 20231110033318.png]]
![[Pasted image 20231110033436.png]]
5、不是所有的属性都必须被提升，可以混合搭配。这种可以用，但不推荐。混合的话请考虑普通构造函数。

```
class B
{
	public string $b;
	
	public function __construct(
		public string $a,
		string $b,
	){
		$this->b = $b;
	}
}
```

6、不能重复

```
class C
{
	public string $a;
	public function __construct(
		public string $a;//这是重复。因为这个属性提升在运行时，被转译成类属性
	){}
}
```

7、可以给属性提升 加默认值

```
public function __construct(){
	public string $name = 'Brent';
	public DateTimeImmutable $date = new DateTimeImmutable();
}{}
```

8、可以在构造函数体中读取已提升的属性。如果您想进行额外的验证检查，这可能会很有用。您可以同时使用局部变量和实例变量，两者都可以正常工作。

```
public function __construct(
	public int $a,
	public int $b,
) {
	assert($this->a >= 100);
	if ($b >= 0) {
		throw  InvalidArgumentException('…');
	}
}
```

9、可以添加文档块儿去提升属性

```
class MyClass{
	public function __construct(
		/**@var string*/
		public $a, 	
	){}
}
$property = new ReflectionProperty(Myclass::class, 'a');

$property->getDocComment();// /** @var string
```

10、

```
class MyClass{
	public function __construct(
		#[MyAttribute]
		public $a, 	
	){}
}
```

上面的代码这样写，将会转译成

```
class MyClass{
	#[MyAttribute]
	public $a, 
		
	public function __construct(
		#[MyAttribute] $a, 	
	){
		$this->a = $a;
	}
}
```

11、属性提升不能用var来声明，应该用public/protected/private来定义访问控制级别。

12、可变参数无法提升。可变参数本质上是数组。

>可变参数函数使用剩余操作符`...`，这是在PHP 5.6中添加的一个特性。它允许你定义一个函数输入变量，它会取所有的“剩余”变量并将它们组合成一个数组。换句话说：它是`func_get_args()`的简写形式。

```
class A
{ 
	public function __construct( 
		public string ...$a, 
	){}
}
```

应该改成这个

```
class A 
{ 
	public array $a; 
	
	public function __construct(string ...$a) 
	{ 
		$this->a = $a; 
	} 
}
```

13、因为php构造函数不需要继承父类构造函数，所以如果非要继承。就需要手动传递属性

```
class A{
    public function __construct(
        public $a,
    ){}
}

class B{
	public function __construct(
		$a,
		public $b,
	){
		parent::__construct($a);
	}
}
```