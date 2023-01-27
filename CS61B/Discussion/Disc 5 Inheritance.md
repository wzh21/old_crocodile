# 1

## a

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230125142648245.png" alt="image-20230125142648245" style="zoom: 67%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230125142705360.png" alt="image-20230125142705360" style="zoom: 67%;" />

## 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230125142911973.png" alt="image-20230125142911973" style="zoom:50%;" />

> 第四行是因为没有传入参数, 所以调用的是父类的`Animal`的`play()`

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230125143523281.png" alt="image-20230125143523281" style="zoom:50%;" />

> ==第六行的`(Cat) a`实际上做了红笔所做的工作,==
>
> ==**即类型转换实际上是改变对象的静态类型(编译时类型)!!!!!!!**==

> ==**最后一行`c = a`不符合类型检查(再次复习, 编译器只能看见静态类型!!!!!!!!!!!!!!!!!!!!!!!!!!)**==

## b

![image-20230125144259404](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230125144259404.png)

剧透! 最后一行(第60行)出现错误。我们如何修复此错误？

## 答案

The compilation error on line 60 is because we are trying set c, which is of static type Cat to be equal to a, when the static type of a is Animal. Even though at runtime, a really does have dynamic type Cat, **the compiler only sees static types so it doesn’t believe that this assignment is valid.** The compiler only sees that we are trying to set a Cat variable to point to an Animal, and an Animal isn’t a Cat! One way to fix this error would be to cast a to be a Cat, such that the line reads c = (Cat) a;. This would be a valid cast, as the compiler agrees that a variable of static type Animal could potentially hold a Cat, and so our request is feasible. Because the cast works, then the assignment is also now valid because a variable of static type Cat can be told to point to the same thing as another variable of (temporary) static type Cat. At runtime, this line will be fine because we were telling the truth: a really is a Cat dynamically!

> 用casting , 即类型强制转换

# 2

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230125144553389.png" alt="image-20230125144553389" style="zoom:67%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230125144603995.png" alt="image-20230125144603995" style="zoom:67%;" />

划掉所有导致编译器错误的行，以及由于编译器错误而失败的后续行。通过运行时错误（如果有）设置X。不要只将搜索限制为main，A类、B类、C类可能会有错误。删除这些行后，D.main输出什么？

## 答案

![image-20230125145826881](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230125145826881.png)

![image-20230125145834163](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230125145834163.png)

> 1.**==不能做`super.super`这样的事==**
>
> 2.`b0.m2(61)`是不对的, 因为A类里没有`m2 (int y)`这样的方法, ==**编译器要在编译之前就做各种检查, 包括这个静态类型里有没有你要调用的这样方法, 所以"动态方法替换"成立的前提是, 静态类型的方法里有这个方法(非常重要!!!!!!)**==

> ==NOT RUNTIME ERROR这将强制转换方法返回的结果，并返回void，因此编译时出错 (**因为编译器知道c0.m2()返回的是void, 不能进行`(C)`的转换**)==

> - ==从子类到父类的类型可以**自动进行**==
> - ==从父类到子类的类型转换必须通过造型（**强制类型转换**）实现==
> - ==无继承关系的引用类型间的转换是**非法的**==

