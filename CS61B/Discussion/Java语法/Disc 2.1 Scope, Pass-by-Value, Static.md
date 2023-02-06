​														

​														Dics 2 Scope, Pass-by-Value, Static

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230122193642537.png" alt="image-20230122193642537" style="zoom: 67%;" />

> switcheroo(foobar, baze);		`foobar.x:10 ` `foobar.y:20 ` `baz.x:30 ` `baz.y:40 ` 
>
> fliperoo(foobar, baze); 			 `foobar.x:30 ` `foobar.y:40 ` `baz.x:10 ` `baz.y:20 `
>
> swaperoo(foobar, baze);          `foobar.x:10 ` `foobar.y:20 ` `baz.x:10 ` `baz.y:20 `

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230122194506465.png" alt="image-20230122194506465" style="zoom: 67%;" />

> {2, 3, 3, 4}   ==**enhanced loop 只是把arr中的每个元素逐个赋值给x(pass by value)**==
>
> {4, 6, 6, 8}
>
> {6, 7} ==**并不会改变 , 因为是pass by value**==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230122194935405.png" alt="image-20230122194935405" style="zoom:67%;" />

> **==本题较难而且非常重要!!!!!!!	而且有益于我们理解什么是static!!!!!!==**

==**static 是 相较于class 的, 所有的class的static member都共享!!!!!(和python(CS61A)差不多)**==

**==换句话说, static是所有类共有的特征, non static是具体实例特有的特征==**

> 1.`compile`
>
> 2.`compile`
>
> 3.`error`
>
> 4.`error`
>
> 5.`compile` ==但不是好的编程习惯, 此时应该用**类名访问静态方法**!!!!!!, 改为`Book.library = this`==

**==可以在任何方法内直接访问static变量, 但在static方法里不能访问实例变量==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230122195839399.png" alt="image-20230122195839399" style="zoom: 67%;" />

![image-20230122202132055](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230122202132055.png)

![image-20230122202139477](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230122202139477.png)