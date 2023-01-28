# circuit switching 电路交换

1.![image-20230127231408105](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127231408105.png)

> 没有任何限制, 那么总的最大连接数就是所有连接数之和

2.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127231654090.png" alt="image-20230127231654090" style="zoom:50%;" />

If all connections are full, the request will be blocked

3.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127231755083.png" alt="image-20230127231755083" style="zoom:50%;" />

> 两个约束条件, 一个是必须经过两个连续的跳数, 第二个是必须是顺时针的

> 解答过程 : Find the max between the sum of the bottleneck(瓶颈) links for cases A->C and B->D, but don't forget about bottleneck links

> 答案: 26

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127232402286.png" alt="image-20230127232402286" style="zoom:50%;" />

> 答案 : No ,超过了我们在3题中计算的值

# quantitative comparison of packet switching and circuit switching 分组交换与电路交换的定量比较

> ==概率论还没学, 就先不写完了==

 <img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127232106299.png" alt="image-20230127232106299" style="zoom: 50%;" />

> 电路交换最多支持的用户数 : 10

2.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127232808101.png" alt="image-20230127232808101" style="zoom:50%;" />

> Hint : How much bandwidth does each user need? Is this less than the total bandwidth?

> 答案 : 电路交换不能支持这么多

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127233023043.png" alt="image-20230127233023043" style="zoom:50%;" />

> Hint : Start with calculating the likelihood nobody is transmitting

> 答案 : 0.0036

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230127233244993.png" alt="image-20230127233244993" style="zoom: 50%;" />

> Hint : The probability will be higher than the previous question

> 答案 : 0.068

