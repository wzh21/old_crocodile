# 1 Filtered List

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230202105753241.png" alt="image-20230202105753241" style="zoom: 80%;" />

## 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230202110230569.png" alt="image-20230202110230569" style="zoom:55%;" /><img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230202110237905.png" alt="image-20230202110237905" style="zoom:55%;" />

> 第二个方案是复制一份给iterator， 但是效率不如第一个方案

# 2

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230202105921947.png" alt="image-20230202105921947" style="zoom:67%;" />

实现IteratorOfIterators，它将接受包含整数的Iterator对象列表作为参数。对`next()`的第一次调用应该返回列表中第一个迭代器的第一个项。对`next()`的第二次调用应该返回列表中第二个迭代器的第一个项。如果列表包含n个迭代器，即我们第n+1次调用`next()`，我们将返回列表中第一个迭代的第二项。

注意，如果在此过程中迭代器为空，我们将继续下一个迭代器。然后，一旦所有迭代器都为空，`hasNext`应该返回false。例如，如果我们有3个迭代器A、B和C，使得A包含值[1、3、4、5]，B为空，而C包含值[2]，则对迭代器OfIterators的`next()`调用将返回[1、2、3、4、5]。

## 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230202110822113.png" alt="image-20230202110822113" style="zoom: 50%;" /><img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230203104959228.png" alt="image-20230203104959228" style="zoom: 33%;" />

> ==利用了List的方法(`removeFirst`和`addLast`)==
>
> ==构造函数保证了一个invariant : every iterator in "list" has something left==
>
> **==是`iterator<Integer>`的原因 : 因为要返回的元素(`next()`)是Integer!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!==**
>
> > 第二个效率不如第一个方案

# 3 多态

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230203104719668.png" alt="image-20230203104719668" style="zoom: 50%;" />

实现Comparator DMSComprator，用于比较Animal实例。如果一个Animal实例的**动态类型**更具体，则该实例大于另一个Animate实例。请参见下面右侧的示例。

在比较方法的第二个和第三个空格中，只能使用预定义的整数变量（第一个、第二个等）、关系/相等运算符（\=\=、>等）、布尔运算符（&&和||）、整数和括号。

作为一项挑战，请使用相等运算符（== 或 !=），而不要使用关系运算符（>、<=等）。可能有多种解决方案。

## 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230203104742984.png" alt="image-20230203104742984" style="zoom:50%;" /><img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230203104754842.png" alt="image-20230203104754842" style="zoom:50%;" />

