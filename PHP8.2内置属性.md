1、#[AllowDynamicProperties]添加在类上，允许动态属性。
2、#[SensitiveParameter]用于替换堆栈跟踪中的参数。出现问题时，php允许您查看堆栈追踪和每个堆栈帧相关联的所有参数，这对调试非常有帮助，但对于敏感数据可能会造成灾难性的后果。
对于密码类的敏感数据加上#[SensitiveParameter]
![[Pasted image 20231118130900.png]]
现在输出结果就是
![[Pasted image 20231118130955.png]]
