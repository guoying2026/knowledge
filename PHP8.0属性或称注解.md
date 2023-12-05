> 在 PHP 中，从 8.0 版本开始引入的 `Attributes` 通常被翻译为“属性”。这是因为它们用于在类、方法、函数参数等的声明前提供额外的元数据。然而，在其他编程语言的上下文中，尤其是 Java，相似的概念被称为“注解”（Annotations）。

尽管在不同的语言中可能称为注解，但在 PHP 社区和文档中，“属性”是官方接受的翻译，并且是 PHP 代码中的关键字。因此，当讨论 PHP 代码时，应该使用“属性”这个词。如果你在一个更加广泛的编程概念上下文中谈论，或者在比较不同语言的相似特性时，可能会用到“注解”这个词来帮助那些熟悉其他语言的开发者理解。

选择属性的语法花费了数月时间，并考虑了多种不同的选项，包括使用 `<<` 和 `>>`，`@@` 以及其他一些较为奇特的变体。最简单的选项 `@` 无法使用，因为它已经被作为错误抑制运算符使用了。最终，开发者们决定使用 `#[]` 语法，这与Rust编程语言中注解的语法相同，其优势是可以一次性分组多个属性。

> 使用属性后，不需要在运行时解析文本块，因为PHP提供了内置的反射支持。文本指出，将会在本章的后续部分深入介绍这些内容。

1、
```
class BlogController { 

	#[Route('/blog', name: 'blog_index')]
	
	public function index() { 
		// ... 
	} 
}
```

2、举例使用：

>`CheckPermission` 是一个用PHP的属性语法（`#[\Attribute]`）定义的类。这个特殊的属性类可以在运行时被反射API识别，并且可以用来检查用户是否有执行某个操作的权限。它的构造函数接收一个参数（在这个例子中是一个权限字符串），这个参数定义了需要检查的权限。

```
namespace Security\Attributes;

#[\Attribute]
class CheckPermission {
    public function __construct(public string $permission)
    {
        // 构造函数接收一个权限字符串，例如 'view_reports'
    }
}

```

```
use Security\Attributes\CheckPermission;

class ReportController
{
    #[CheckPermission('view_reports')]
    public function showMonthlyReport() {
        // 显示月度报告的逻辑
    }

    #[CheckPermission('edit_reports')]
    public function editMonthlyReport() {
        // 编辑月度报告的逻辑
    }
}

```

> 这通常会结合反射API使用

```
$reflectionClass = new ReflectionClass(ReportController::class);

foreach ($reflectionClass->getMethods() as $method) {

    $attributes = $method->getAttributes(CheckPermission::class);
    
    foreach ($attributes as $attribute) {
        $permission = $attribute->newInstance()->permission;
        // 现在你可以检查当前用户是否有$permission指定的权限
    }
}

```

3、属性相对于类型提示的优势在于：

1. **声明性**：属性以声明性的方式提供关于代码如何运作的信息。在上面的例子中，`Route`属性声明了应该如何响应特定的HTTP请求。
    
2. **灵活性**：属性可以包含更多的自定义信息，并且不仅限于类型。例如，它们可以声明性能参数、权限需求、路由信息等。
    
3. **面向切面编程（AOP）支持**：属性可以用于支持AOP，这是一种编程范式，允许开发者将横切关注点（如日志、事务管理等）与业务逻辑分离。属性可以作为定义这些横切关注点的标记。
    
4. **框架集成**：框架可以使用属性来提供高级功能，如自动路由配置、缓存、事件监听器注册等，而不需要开发者显式编写这些引导代码。
    

总之，属性提供了一种在代码中嵌入额外信息的方法，这些信息不仅限于类型，而且可以在运行时被动态读取和处理。这种方式打开了更多高级编程技术的可能性，为编写更复杂、更富表现力的应用程序提供了工具。

4、php8.0 定义 属性/注解 ListensTo，自定义属性是个简单的类。

```
#[Attribute]
class ListensTo
{
    public function __construct(public string $event) {}
}
```

使用属性,在一个类中使用同一个方法来处理多个事件，而不是为每一个事件类型定义单独的方法。

```
class MailLogEventSubscriber//事件监听器类
{
	//多个事件监听器类。在服务提供者里的register方法里面把所有监听器都拿出来，添加到事件分发器中。
    #[ListensTo(OrderCreatedEvent::class)]
    #[ListensTo(InvoiceCreatedEvent::class)]
    #[ListensTo(InvoicePaidEvent::class)]
    
    //LoggableEvent $event是事件类
    public function handleLoggableEvent(LoggableEvent $event) 
    {
        // ...
    }
}
```

```
public function boot()
{
    $events = collect([
        // 这里我们将收集应用中定义的所有事件
    ]);

    $events->each(function ($eventClass) {
		Event::listen($eventClass[MailLogEventSubscriber::class,
                      'handleLoggableEvent']);
    });
}

```

5、在laravel中，用服务提供者举例。读取属性并将它们绑定到事件。

>这个服务提供者的实现展示了如何在 Laravel 应用程序中注册基于属性的事件监听器，这是一个比直接在事件分发器中手动注册每个监听器更为动态和可扩展的方法。

	记住属性的目标就是给类和方法添加元数据。

```

//`EventServiceProvider` 类继承了 Laravel 的 `ServiceProvider` 类，它是所有服务提供者的基类。
class EventServiceProvider extends ServiceProvider
{
    // 在现实场景中，我们会自动解析和缓存所有订阅者，
    // 而不是使用手动数组。
    //`$subscribers` 属性定义了一个包含事件订阅者类名的数组。在实际应用中，这个数组可能通过自动化的方式生成，例如通过扫描目录或使用其他逻辑来收集所有订阅者。
    private array $subscribers = [
        ProductSubscriber::class,
    ];
    
	//`register` 方法是服务提供者的核心方法之一，它在服务提供者注册期间被调用。
    public function register(): void
    {
        // 事件分发器是从服务容器中解析出来的
        $eventDispatcher = $this->app->make(EventDispatcher::class);
		
		//对于 `$subscribers` 数组中的每一个订阅者，`resolveListeners` 方法被调用来获取订阅者监听的所有事件及其对应的处理方法。
		
        foreach ($this->subscribers as $subscriber) {
        
            // 我们将解析所有监听器
            //（带有 ListensTo 属性的方法）
            // 并将它们添加到分发器中。
            
            // as [$event,$listener] 是数组解构
            
            foreach ($this->resolveListeners($subscriber) as [$event,$listener]){
                
                //对于返回的每个事件和监听器对，
                //使用事件分发器的 `listen` 方法来注册监听器，使得当指定的事件被触发时，
                //相应的监听器会被调用。
                
                $eventDispatcher->listen($event, $listener);
            }
        }
    }
    
	//`resolveListeners` 方法包含了解析订阅者类并读取其方法中 `ListensTo` 属性的逻辑，以确定该订阅者监听哪些事件。
    private function resolveListeners(string $subscriberClass): array{
    
		//使用 `$subscriberClass`（一个字符串，表示订阅者类的名称）创建一个新的 `ReflectionClass` 对象。
	    $reflectionClass = new ReflectionClass($subscriberClass);
	    
		//初始化一个空数组 `$listeners` 用于存储解析出来的事件监听器。
	    $listeners = [];

	    foreach ($reflectionClass->getMethods() as $method) {
	    
		    //对于每个方法，使用 `$method->getAttributes(ListensTo::class)` 来获取所有 `ListensTo` 属性。
	        $attributes = $method->getAttributes(ListensTo::class);

	        foreach ($attributes as $attribute) {

				//对于每个找到的 `ListensTo` 属性，调用 `$attribute->newInstance()` 来创建属性的一个实例。这一步是必须的，因为属性实例不会在加载时自动创建，它们只在显式调用 `newInstance()` 时实例化。
	            $listener = $attribute->newInstance();
	            
		        $listeners[] = [
		            // 事件名称是从属性中配置的,属性实例化后，可以通过属性实例访问在属性中定义的参数。在这个例子中，`$listener->event` 指的是属性中配置的事件名称。
		            $listener->event,
		            // 这个事件的监听器
		            [$subscriberClass, $method->getName()],
	            ];
	        }
	    }

		//方法最后返回 `$listeners` 数组，这个数组包含了所有解析出的事件和对应监听器的键值对。这个`resolveListeners`方法的实现展示了如何在运行时动态地将事件监听器与事件关联起来，这是一种非常灵活的事件注册方法，允许开发者通过简单地在类方法上添加属性来声明事件监听器，而无需在多个地方手动注册监听器。
	    return $listeners;
	}

}
```

> 使用反射，我们可以从类的方法中读取属性，使用new ReflectionClass($subscriberClass)->getMethods()其中一个$$method->getAttributes(ListensTo::class)->newInstance()来实例化自定义属性类，这是一个重要的细节。我们的属性只有在调用newInstance()才会被构造。而不是被加载。

>你通过反射API获取属性并想要实例化它时，你可以选择直接使用 `$attribute->getArguments()` 来获取构造函数的参数，而不是先实例化属性对象。这给了你灵活性，可以在不实际创建属性实例的情况下，获取传递给属性构造函数的原始参数。

> 可以在同一个方法、类、属性或常量上添加多个属性

>  `ReflectionMethod::getAttributes()` 方法，这是用来检索方法上的所有属性的。你可以传递两个参数给这个方法：第一个是你想要检索的属性类名，第二个是一个标志位，用来过滤结果。

总结来说明整个过程：

- 定义事件和监听器

- 使用属性，对于事件系统，创建自定义属性 ListenTo,这个属性指定某个方法应该监听哪个事件。

- 使用属性作为事件监听器的声明，不需要在代码多个地方手动注册每个事件监听器。相反，你可以直接在监听器方法上声明它们应该响应的事件。

- 事件注册，在服务提供者中，使用反射API读取这些属性，并将事件名称与监听器方法关联起来。这样当事件发生时，事件分发器dispatcher就知道要调用哪个监听器。

- 反射api，可以在运行时检查类和对象的类型信息，并访问其方法和属性，包括自定义属性。

- **动态解析监听器**：通过在服务提供者中定义一个方法（如`resolveListeners`），可以动态地解析出哪些方法监听了哪些事件，并将它们注册到事件分发器中。


- 定义一个接口，如`LoggableEvent`，它声明了事件对象应该实现的方法。
- 创建一个或多个事件类，它们代表了应用程序中可能发生的不同事件。
- 创建一个事件监听器类，如`MailLogEventSubscriber`，在其中定义方法来响应这些事件。
- 在监听器方法上使用`#[ListensTo]`属性来声明这些方法应该监听的事件。
- 在服务提供者中，编写代码来读取这些属性，并自动将事件与其监听器关联起来。

![[Pasted image 20231113024912.png]]

为了精细控制自定义属性的使用位置，可以配置它们只能在特定位置使用。所有标志位仅在$attribute->getInstance()时，才会被检查。在此之前如果没有通过反射或静态分析，可能发现不出错误。

![[Pasted image 20231113025359.png]]

![[Pasted image 20231113024933.png]]

```
属性提供了一种强大的方式来添加元数据，这些元数据可以用于文档、编译时检查、运行时行为修改等。

PhpStorm IDE使用属性来增强静态分析，例如通过 `#[ArrayShape]` 属性指定数组的期望结构。

使用这些信息，开发者可以更好地理解和利用代码，提高代码的可读性和可维护性。同时，静态分析工具可以使用这些属性来提供更丰富的错误检查和代码洞察。
```

![[Pasted image 20231113030712.png]]

![[Pasted image 20231113030742.png]]

正确用法
![[Pasted image 20231113031113.png]]

![[Pasted image 20231113031140.png]]

![[Pasted image 20231113031537.png]]
![[Pasted image 20231113031608.png]]


错误用法❌

![[Pasted image 20231113030957.png]]

![[Pasted image 20231113031012.png]]