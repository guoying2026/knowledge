1、不需要再像传递给array_map回调函数那样编写闭包

原来这样写
```
$itemPrices = array_map(
  function (OrderLine $orderLine) {
    return $orderLine->item->price;
  },
  $order->lines
);

```

现在
```
$itemPrices = array_map(
  fn ($orderLine) => $orderLine->item->price,
  $order->lines
);

```

2、短闭包与普通闭包的区别在于两点：

- 它们只支持一个表达式，并且这个表达式就是返回语句。
-  它们不需要用`use`语句来访问外层作用域的变量。

至于其他方面，它们的行为就像普通的闭包一样，它们支持引用、参数展开、类型提示、返回类型...谈到类型，你可以用更严格的方式重写前面的示例，

```
$itemPrices = array_map(
  fn (OrderLine $orderLine): int => $orderLine->item->price,
  $order->lines
);

```

3、短闭包也支持引用作为参数或返回值

```
fn(&$x) => $x

```

4、不支持多行，即使为了格式化分布在多行，但是最终必须是一个表达式。短闭包的目的是减少冗余，如果处理多行函数，使用短闭包的优势就不大。未来可能会支持多行的短闭包，但现在还需要等待。

5、短闭包不需要 use 关键字就能处理外部作用域的数据

```
$modifier = 5;
array_map(fn($x) => $x * $modifier, $numbers);

```

6、你不能修改外部作用域中的变量，因为它们是按值绑定的，而不是按引用绑定的，除非你处理的是总是按引用传递的对象。这是PHP中对象的默认行为，不仅仅是在短闭包中。

`$this`关键字在短闭包中的行为与在普通闭包中一样。但是，你可以声明一个短闭包为静态的，这意味着你不能从中访问`$this`：

```
static fn($x) => $x * $this->modifier;
// Fatal error: Uncaught Error: Using $this when not in object context

```

![[Pasted image 20231113052256.png]]

![[Pasted image 20231113052317.png]]

