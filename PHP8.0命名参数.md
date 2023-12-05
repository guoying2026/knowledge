1、根据参数名称来传递参数，而不是参数位置。

2、可以跳过具有默认值的参数

3、可以这样

![[Pasted image 20231110061441.png]]

但不可以这样

![[Pasted image 20231110061503.png]]

4、命名参数+...操作符（数组展开功能）将一个关联数组动态传递给函数或构造函数，而不需要手动列出每一个参数。

![[Pasted image 20231110063519.png]]

5、如果其中一个没有指明键名，就要按参数顺序了

![[Pasted image 20231110063655.png]]

6、注解+命名参数

![[Pasted image 20231110071343.png]]

7、不可以把普通变量作为命名参数。错错❌

![[Pasted image 20231110071520.png]]

8、在继承中的注意事项

```
interface EventListener { 
	public function on($event, $handler); 
}

class MyListener implements EventListener { 
	public function on($myEvent, $myHandler) { // ... }
}
```

PHP允许继承中修改参数的名字，但是在调用的时候必须用具体实现类或子类定义的参数名称。

```
public function register(EventListener $listener) {
	$listener->on( myEvent: $this->event, myHandler: $this->handler, ); 
}
```

不可以用❌ event和handler。为了避免这个问题，尽量保持参数名一致。