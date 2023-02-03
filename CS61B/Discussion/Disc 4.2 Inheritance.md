# 1

## a

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126142751559.png" alt="image-20230126142751559" style="zoom: 67%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126142811948.png" alt="image-20230126142811948" style="zoom:80%;" />

### 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126143240867.png" alt="image-20230126143240867" style="zoom: 50%;" />

> ==再次复习动态方法选择 : 首先要在**static type**里看看有没有这个方法签名, 如果有, 可以通过 **编译**, **并且锁定这个方法签名**==
>
> ==但在实际的run-time时, 会考虑"Can we do better ?" ,会从**实际的对象类型(dynamic type)开始往上面找**, 看看有没有符合方法签名的, 一旦符合, 就调用(**中间的类也可以**).==
>
> > **==在run-time时,即使子类里有更好的(更符合的),也不选, 因为我们只会看有没有我们"最初锁定的那个签名的更better的", 不会考虑"其他有没有更better"的==**
> >
> > > `chirasree.speakTo((SoccerPlayer) sohum); // respect`体现了这一点

> **==类型转化的实质是暂时改变静态类型==**

## b

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126145600372.png" alt="image-20230126145600372" style="zoom: 50%;" />

### 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126151223254.png" alt="image-20230126151223254" style="zoom:50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126151225825.png" alt="image-20230126151225825" style="zoom:50%;" />

## c

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126151510918.png" alt="image-20230126151510918" style="zoom:50%;" />

### 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126151528770.png" alt="image-20230126151528770" style="zoom:50%;" />

# 2

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126151720570.png" alt="image-20230126151720570" style="zoom:67%;" />

### 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126151853420.png" alt="image-20230126151853420" style="zoom:67%;" />

> 加了一个`LastIntNode`的class

# 3

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126151921424.png" alt="image-20230126151921424" style="zoom:67%;" />

### 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126151933188.png" alt="image-20230126151933188" style="zoom:67%;" />