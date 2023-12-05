![[Pasted image 20231119021551.png]]
![[Pasted image 20231119021610.png]]

你可以为随机数生成器（PRNG）提供一个伪随机数生成器

PHP内置了几个PRNG引擎：Mt19937、PcgOneseq128XslRr64、Xoshiro256StarStar和Secure。
只有最后一个Secure引擎适用于加密随机数生成。这个引擎模型允许在将来快速添加更安全/更快的引擎.

您的引擎可与随机器一起使用。
![[Pasted image 20231119021624.png]]