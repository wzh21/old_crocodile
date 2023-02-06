

​															**==本练习较难,且比较重要==**

# 1

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204132615713.png" alt="image-20230204132615713" style="zoom:67%;" />

## 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204132628465.png" alt="image-20230204132628465" style="zoom:67%;" />

# 2 递归复杂度分析

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204143133356.png" alt="image-20230204143133356" style="zoom: 50%;" />

## 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204143154253.png" alt="image-20230204143154253" style="zoom: 50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204144038531.png" alt="image-20230204144038531" style="zoom:50%;" />

> **==只能一层一层地分析, 没有捷径!!!!!!==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204144045191.png" alt="image-20230204144045191" style="zoom:67%;" />

> **==记住, `9 ^ log N ≈ O(N)`==**
>
> **==递归分析就是一层一层地去分析!!!!!==**
>
> + 分析最差的结果
> + 画递归图解决

# 3 更多的复杂度分析

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204143607406.png" alt="image-20230204143607406" style="zoom:50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204143634421.png" alt="image-20230204143634421" style="zoom: 50%;" />

## 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204145501436.png" alt="image-20230204145501436" style="zoom: 50%;" />

> ==这里的P4, 不管Math.pow的时间复杂度是什么, 这里已经给了一个具体的,不大的数字了, **所以可以当作常数对待**==

> 这里的P5, 1 + 2 + 4 + 8 + .... + N * N = 2 N * N - 1 = O(N ^ 2)
