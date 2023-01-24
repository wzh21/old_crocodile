# 1

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124000201276.png" alt="image-20230124000201276" style="zoom:50%;" />

![image-20230124000231987](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124000231987.png)

> Party size: 2
>
> Pikachu 17 Ash
>
> Voltorb 1 Team Rocket
>
> Voltorb 1 Brock

==写错了, 答案应该是==

> Party Size: 2 
>
> Pikachu 17 Ash 
>
> Pikachu 18 Team Rocket 
>
> Pikachu 18 Brock

因为是static的change方法改变了类属性static的trainer

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124111721670.png" alt="image-20230124111721670" style="zoom:50%;" />

重要!!!!!!!!!!!!!!!: ==**传值!!!!!!!!传的是地址, poke绑定在这个地址上, 但是和外部的p无关!!!**==

**==随时有个box and pointer 图在心里面==**

![image-20230124111838503](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124111838503.png)

It is the local variable in the change method and does not have any effect on the other variables of the same name in the Pokemon class or the main method

![image-20230124112810297](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124112810297.png)

If we were to add this line to our main method, it would error. In the class, printStats() is an instance method. What would it mean to print the name and level of the class Pokemon, as opposed to a specific Pokemon’s name? It doesn’t really make sense. So when we try to run this method on our class, it errors. 

One more thing to note is the method change is declared static itself. Static methods can be called using the name of the class, as in line 19, whereas non-static methods cannot. ==**The golden rule for static methods to know is that static methods can only modify static variables.**==

# 2

![image-20230124112939207](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124112939207.png)

![image-20230124113334979](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124113334979.png)

# 3

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124113604210.png" alt="image-20230124113604210" style="zoom:50%;" />

找到第一个item是n的下标(index)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124114101179.png" alt="image-20230124114101179" style="zoom: 50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124113530616.png" alt="image-20230124113530616" style="zoom: 67%;" />

**==关于为什么要建立一个 Helper 函数:==**

> 1. 抽象界限, 用户没必要知道实现细节
> 2. 不能让用户传入额外的参数, ==**那些参数是我们求解时必须的, 但是不是用户描述需求所必须的**==

It’s not intuitive for the user to have to pass in 0 and sentinel.next every single time they’re calling findFirst(), as it is unrelated to what they’re actually requesting. Additionally, it is breaking the abstraction barrier, as it requires our user to understand how this method works under the hood. Finally, if the user didn’t understand what to pass in (because again, it’s not quite intuitive), they could pass in some random values that will result in an incorrect answer. 

Thus, it is bad programming practice to make the user pass in those extra arguments every time. However, we do need a way of keeping track of which node and index we’re on as we recurse, so we must make a helper method that can keep track of all that information.

用户在每次调用findFirst（）时都必须传入0和sentinel.next，这是不直观的，因为这与他们实际请求的内容无关。此外，它正在打破抽象障碍，因为它要求我们的用户了解这种方法在幕后是如何工作的。最后，如果用户不知道传递什么（因为这也不是很直观），他们可能会传递一些随机值，从而导致错误的答案。
		因此，让用户每次都传递这些额外的参数是不好的编程实践。然而，我们确实需要一种在递归时跟踪我们所在的节点和索引的方法，因此我们必须创建一个可以跟踪所有信息的助手方法。