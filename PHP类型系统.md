1、在php7之前，我们可以使用类型，但只能用于输入参数。这导致文档块类型和内联类型的混乱。

从php7.0之后可以既有输入参数类型声明。返回值也有类型声明。类里面还有类型化属性。

标量类型和对象都是允许的，就像参数返回类型一样。

但都是不是强制的。php只会在我们尝试访问属性时才会抛出错误。

所以会有新的错误类型叫未初始化。如果类里面的属性，没有类型，他可以是简单的null。

有类型的属性可以是可空的。因此很重要的为了区分 “忘记的类型” 和 为 “null” 的类型。

这就是为什么添加了“未初始化”。

      有几件关于未初始化的重要事情需要记住：

-  你不能从未初始化的属性中读取数据，这样会导致致命错误。
-  因为只有在访问属性时 会检查未初始化的状态，因此可以创建一个具有未初始化属性的对象。
-  在读取未初始化属性之前，您可以对其进行写入，这意味着您可以在构造后执行$money->amout = 1;
- 对于已声明类型的属性使用unset将其未初始化，而对于未声明类型的属性使用unset将其变成null;
- 当读取属性的值时，只有未初始化状态才会被检查，而在写入属性时会进行类型验证，这意味着您可以确保任何无效类型都不会成为属性的值。

2、处理空值
在谈论未初始化状态的同时，让我们也讨论下空值。

在php7.0之后，?? null合并操作符，此操作符对其左侧执行isset检查，如果返回false,它将返回右操作数提供的备用值。在这种情况下，是支付日期的时间戳或null。这是一个很好的补充，可以减少我们代码的复杂性。

php7.4的时候引入 ??= null合并赋值操作符。这不仅支持默认值备用，还会直接将其写入左操作数。

??= 一个更常见的用例是记忆化函数，一种在计算后存储结果的函数。

记忆化函数将对具有某个模式的字符串进行正则表达式匹配，但如果提供相同的字符串和相同的模式，它将简单地返回缓存的结果。

```
function match_pattern(string $input, string $pattern) {
	static $cache = [];
	return $cache[$input][$pattern] ??= 
		(function (string $input, string $pattern) {
			preg_match($pattern,$input,$matches);
			
			return $matches[0];
		})($input, $pattern);
}
```

在 PHP 8.0中新增了一个与null相关的功能：空安全运算符。

$invoice->paymentDate->format();如果付款日期是null，会发生什么，你会收到一个错误叫做call to a member function  format() on null;

null合并运算符无法处理空方法调用。

所以php8.0有了空安全运算符,只在函数方法后面调用，就可以使用$invoice->getPaymentDate()?->format('Y-m-d');如果方法可执行就调用。如果不行就返回null.

处理空值还有另外一种形式，替代方案为空对象模式。

```
interface Invoice
{
	public function getPaymentDate():Date;
}

class Date{

}

class UnknowDate extends Date{
	public function format():string
	{
		return '/';
	}
}

class PaidInvoice implements Invoice{
	public function getPaymentDate():Date
	{
		return $this->date;
	}
}

class PendingInvoice implements Invoice{
	public function getPaymentDate():UnknowDate
	{
		return new UnknownDate();
	}
}
```

不再是日期或空，而是日期或未知日期，不再是“带有状态的发票”，而是“已付发票或待付发票”。
您将不需要担心空对象

$invoice->getPaymentDate()->format();// A date or '/'

在上述的代码中发生了一些变更，我们在继承的过程中更改了返回类型。这个是php7.4引入的类型变异。返回类型和输入参数类型在继承过程中允许被更改，但两者有不同的规则。

更改类型

```
interface 中的getPaymentDate声明的返回值类型为Date
但是class PendingInvoice implements Invoice{
	里面的getPaymentDate方法 返回值类型为 UnknowDate。
}
```

另外一个有趣的细节是
使用 ?  和默认 = null值的可空类型之间存在差异；在先前的示例中，您已经看到它们同时使用。php区别在于：如果您使一个类型为可空，你仍然需要向该函数传递一些内容，你不能只是跳过这个参数。

```
function createOrUpdate(?Offer $offer):void{

}
createOrUpdate();//错误，//未捕获的ArgumentCountError:参数太少，无法调用
createOrUpdate(null);//正确

所以需要这样改

function createOrUpdate(?Offer $offer = null):void{

}
createOrUpdate();//正确
createOrUpdate(null);//正确
这样就正确了。但由于向后兼容性，这个系统有个小问题。如果你给一个变量赋予默认值为null，它将始终是可空的。

不管类型是否明确可空，都将允许这样做：

function createOrUpdate(Offer $offer = null):void{
	
}
createOrUpdate();//正确
createOrUpdate(null);//正确
```

3、联合类型

使用多种类型对变量进行类型提示/声明。

interface Repository
{
	public function find(int|string $id);
}
无法使用?运算符将联合类型变成可空，例如 ?int|string这种不对，必须写成 int|string|null

但是不要过度使用联合类型，用的类型多了，表明一个函数可以做多种事情。

一个框架可能允许你从控制器方法中返回response和view对象。有时直接返回view很方便，
而其他时候你可能想要对响应进行精细化控制。这些情况下使用Response|View的联合类型是合适的。另一方面，如果你有一个接受array|Collection|string联合类型的函数，这可能表明该函数需要
做太多的事情，最好考虑寻找一个共同点。

4、php8.1交集类型。

php8.1添加了另一种编写类型的方式，它们可能看起来与联合类型类似，但有一个小差别。联合类型要求给定的输入是类型A或类型B，而交集类型要求输入是类型A和类型B。

比如有两个类型的接口。
一个类post要可以同时实现它们中的任何一个。

```
interface WithUuid{
	public function getUuid():Uuid;
}

interface WithSlug{
	public function getSlug():string;
}

class Post implements WithUuid,WithSlug{/**...*/}

```

但如果我们想要创建一个需要同时实现两个接口的对象的函数呢？

```
function url($object):string{
	
}
```

我们之前可能会需要

```
interface WithUrl extends WithUuid,WithSlug{

}

class Post implements WithUrl {

}

function url(WithUrl $obj):string{

}
```

但是有了交集类型，我们不需要中间的那个WithUrl接口了

```
class Post implements WithUuid, WithSlug{

}

function url(WithUuid&WithSlug $object): string{

}
```

5、php8.2添加了DNF类型。联合类型和交集类型为语言增添了许多好处。引入了析取范式类型。可以构建更复杂的类型。 交集和单一类型的并集 （ A & B ）| C $obj 或 （A & C）|  (C &B) $obj

```
interface WithUrlSegments(){
	public function getUrlSegments():array;
}

class Page implements WithUrlSegments(){
	
}

function url((WithUuid&WithSlug)| WithUrlSegments $object):string
{

}
该功能现在允许实现了WithUuid&WithSlug接口对象，或者实现了单一WithUrlSegments接口对象。
```

还有DNF类型最突出的用例可能是使交集类型为可空：

```
function url((WithUuid&WithSlug)| null $object): ?string{

}
```

6、php8中引入了静态返回类型，它表示函数返回调用该函数的类。它与self不同，因为self总是指向父类，而static可以指示子类。

```
abstract class Parent{
	public static function make():static
	{
		
	}
}
class Child extends Parent{

}
$child = Child::make();
```

在这个例子中，静态分析工具和IDE现在知道$child是Child的一个实例，如果使用self作为返回类型。它们会认为它是parent的一个实例。还要注意，static关键字和静态返回类型是两个不同的概念。它们只是碰巧有相同的名称。（许多语言都是这样）

7、php7.1引入void返回类型，它检查函数是否没有返回任何类型。请注意void不能与其他类型合并为联合类型。

8、php8.0新增加了mixed类型，mixed可以用来对“任何东西”进行类型提示。它是array|bool|callable|int|float|null|object|resource|string的联合类型的简写，将mixed可以视为void的相反。

9、php8.1中新增加了nerver类型，nerver和void的区别在于void表示函数不返回任何内容，而nerver实际上永远不会返回任何内容。这可能发生在函数调用exit或总是抛出异常的情况下。这是对静态分析器非常有用的类型，可以进行更详细的分析。

10、php8.2中可以使用null和false类型。在属性、参数或返回类型中使用这些类型会导致致命错误。从这个php版本开始，这些类型可以作为独立类型使用，并添加了true类型，请注意，无法创建?null、false|bool,true|bool,false|bool或false|true类型，否则会导致严重错误。

11、你可能不会经常使用新添加的null、true、或false类型。它们主要是为了构建完整的类型系统并修复一些内置方法中的不一致之处。使用这些新类型的一个有趣案例可能如下。

```
class User {
	function isAdmin():bool
}

class Admin extends User{
	function isAdmin():true
	{
		
	}
}
class Employee extends User{
	function isAdmin():false
	{
	
	}
}
```

12、php8.1支持枚举类型

```
enum Status: string {
	case draft = 'draft';
	case published = 'published';
	case archived = 'archived';
}
```

13、泛型，php的类型系统到现在都不支持泛型，但静态分析器支持它们，所以泛型不是一种一流的语言特性，但我们今天可以用它们进行静态分析。