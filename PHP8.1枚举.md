1、枚举的好处在于，这些值可以被类型化。

```
enum Status
{
    case DRAFT;
    case PUBLISHED;
    case ARCHIVED;
}
```

```
class BlogPost
{
    public function __construct(
        public Status $status,
    ) {}
}

$post = new BlogPost(Status::DRAFT);
```

2、枚举像类一样，里面也可以定义方法，这是一个非常强大的方法，特别是与匹配运算符结合使用时：

```
enum Status
{
    case DRAFT;
    case PUBLISHED;
    case ARCHIVED;

    public function color(): string
    {
        return match($this)
        {
            Status::DRAFT => 'grey',
            Status::PUBLISHED => 'green',
            Status::ARCHIVED => 'red',
        };
    }
}

$status = Status::ARCHIVED;
echo $status->color(); // 输出 'red'

```

3、静态方法在枚举中的用法

```
enum Status
{
    // ...
    
    public static function make(): Status
    {
        // ...
    }
}

```

4、可以在枚举中使用self关键字

```
enum Status
{
    // ...

    public function color(): string
    {
        return match($this)
        {
            self::DRAFT => 'grey',
            self::PUBLISHED => 'green',
            self::ARCHIVED => 'red',
        };
    }
}

```

5、枚举也可以实现接口，就像普通类一样

```
interface HasColor
{
    public function color(): string;
}

enum Status implements HasColor
{
    case DRAFT;
    case PUBLISHED;
    case ARCHIVED;

    public function color(): string { /* ... */ }
}

```

6、带有后备值的枚举

```
enum Status: string
{
    case DRAFT = 'draft';
    case PUBLISHED = 'published';
    case ARCHIVED = 'archived';
}

```

7、int表示给所有的case都是int. 枚举值的类型只能是int和string

```
enum Status: int
{
    case DRAFT = 1;
    case PUBLISHED = 2;
    case ARCHIVED = 3;
}

```

8、如果要实现接口，枚举类型必须紧跟在枚举名称其后

```
enum Status: string implements HasColor
{
    case DRAFT = 'draft';
    case PUBLISHED = 'published';
    case ARCHIVED = 'archived';
}

//访问

$status = Status::DRAFT;
echo $status->value; // '1' for int-backed enums

$status = Status::PUBLISHED;
echo $status->name; // 'PUBLISHED'

```

9、序列化枚举
![[Pasted image 20231113041039.png]]

```
$status = Status::from(2); // 得到 Status::PUBLISHED 枚举值

$status = Status::tryFrom('unknown'); // 如果值不是有效的枚举值，则返回 null

$status = Status::from('unkonwn'); //如果使用from，就会抛出异常


// 序列化和反序列化
$serialized = serialize($status);
$unserializedStatus = unserialize($serialized);

// 转换为JSON
$json = json_encode($status);

```

![[Pasted image 20231113040436.png]]
![[Pasted image 20231113040539.png]]

![[Pasted image 20231113040622.png]]

![[Pasted image 20231113040655.png]]

![[Pasted image 20231113040712.png]]

10、返回全部枚举

![[Pasted image 20231113040836.png]]

11、枚举的值在内部表示为对象，实际上是单例对象，这意味着你可以使用全等操作符 `===` 来比较枚举对象。

```
$statusA = Status::PENDING;
$statusB = Status::PENDING;
$statusC = Status::ARCHIVED;

$statusA === $statusB; // true
$statusA === $statusC; // false
$statusC instanceof Status; // true

```

12、**枚举作为数组键**：枚举值实际上是对象，因此目前不能将它们用作数组键。你只能在 `SplObjectStorage` 和 `WeakMaps` 中将枚举作为键使用。

![[Pasted image 20231113042345.png]]

13、**特性（Traits）**：枚举可以像类一样使用特性，但有一些限制：不能覆盖内建的枚举方法，也不能包含类属性。

14、**常量表达式**：在引入枚举时，无法在常量表达式中获取枚举的名称和值。从PHP 8.2开始，这些语法是有效的，不会再抛出错误。

15、**反射和属性**：有几个反射类专门用于处理枚举：`ReflectionEnum`, `ReflectionEnumUnitCase` 和 `ReflectionEnumBackedCase`。还有一个新的函数 `enum_exists`，枚举和它们的情况（cases）可以使用属性注解。`TARGET_CLASS` 过滤器也将包含枚举。

16、在常量表达式中使用枚举

```
class Post
{
    #[DefaultValue(Status::Draft->name)]
    public string $status = Status::Draft->name;

    public function updateStatus(
        string $status = Status::Draft->name
    ): void {
        /* ... */
    }
}

const STATUS = Status::Draft->name;

```
