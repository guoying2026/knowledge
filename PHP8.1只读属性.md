1、属性为public时，外部可以覆盖它的属性

2、php8.1通过引入readonly来解决什么问题，一旦属性被设置就不可以被覆盖。

> 为了解决创建私有属性，需要创建大量的getter方法来让外部读到属性。

3、只读属性必须声明类型，否则会报错

![[Pasted image 20231110051504.png]]

4、只读属性默认情况下不能有默认值，否则会报错。只有和属性提升一起时才有默认值。

![[Pasted image 20231110051811.png]]

>为什么和属性提升时可以有默认值，因为转译后的结果符合要求。

```
class A{
    
    public function __construct(
        public readonly int $a = 1,
    ){}
}
```

在底层转译成

```

class A{
    
    public readonly int $a;
    
    public function __construct(
        int $a = 1,
    ){
        $this->a = $a;
    }
}
```

5、继承的时候不可以更改只读属性

![[Pasted image 20231110052348.png]]

6、只读属性不可以unset

7、在实际应用中，需要不可变的场景应当使用readonly属性。

8、php的反射提供了`ReflectionProperty::isReadOnly()` 方法，以及一个 `ReflectionProperty::IS_READONLY` 标志。这些用于确定一个属性是否被声明为 `readonly`，即它们是用于反射API中，以编程方式检查属性的只读状态。

9、现阶段讨论了关于克隆对象时 `readonly` 属性的行为。在 PHP 中，`readonly` 属性不能在克隆时被覆盖。这意味着当你克隆一个对象时，你不能改变其 `readonly` 属性的值，因为它们是不可变的。然而，有一个提议可能会在未来添加一个 `clone with` 构造，允许在克隆对象时改变 `readonly` 属性。

尽管当前没有官方的 `clone with` 功能，但已经有人创建了一个第三方包，这个包使用一些反射的技巧允许克隆对象的同时覆盖 `readonly` 属性。示例中展示了一个使用该包的 `Post` 类，这个类有一个 `readonly` 属性 `$title`，并且展示了如何使用这个包来创建一个新的 `Post` 对象，其 `title` 属性与原对象不同。

这个第三方包名为 `spatie/php-cloneable`，图片中鼓励读者去 GitHub 查看这个包的源码。

