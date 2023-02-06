# 1 ==递归复杂度分析的一般方法==

## 1.1

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204151418821.png" alt="image-20230204151418821" style="zoom:50%;" />

### 答案

![image-20230204151629379](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204151629379.png)

==**这个问题的元策略是探索一个严格的框架来分析递归过程的运行时间。具体来说，可以通过绘制递归树并考虑三条信息来导出运行时间。**==

+ **==树的高度。==**
+ **==每个节点有多少个分支==**
+ **==每个分支的工作量是多少(注意此时N(输入规模)变了)==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204151836280.png" alt="image-20230204151836280" style="zoom:33%;" />

## 1.2

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204153136563.png" alt="image-20230204153136563" style="zoom: 50%;" />

### 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204153252486.png" alt="image-20230204153252486" style="zoom:50%;" />

> ==第二种情况的快速计算 : 一共有`logN`层, 每一层的工作量都是固定不变的 , 都是`N`==

## 1.3

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204153858762.png" alt="image-20230204153858762" style="zoom:67%;" />

### 答案

注意, 这是两个分叉

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204154145827.png" alt="image-20230204154145827" style="zoom: 67%;" />

## 1.4

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204154221429.png" alt="image-20230204154221429" style="zoom:50%;" />

### 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204154244376.png" alt="image-20230204154244376" style="zoom:50%;" />

对于spacejam（N），最坏和最好的情况是O（N·N!）。现在对于第i层，节点的数量是n·（n−1）··（n−i），因为分支因子从n开始，每层递减1。实际上计算总和有点棘手，因为有一个讨厌的(n−i)!分母中的项。我们可以通过去除分母来上限和，但在最严格的意义上，我们现在有一个大O界，而不是大Θ。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204161845858.png" alt="image-20230204161845858" style="zoom: 67%;" />

> ==**非常重要的思想 : 计算每层的工作量, 然后相加, 可以有`i`层这个参数**==

# 2

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204154310909.png" alt="image-20230204154310909" style="zoom:67%;" />

## 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204154329242.png" alt="image-20230204154329242" style="zoom: 50%;" />

# 3 Asymptotic Notation 算法复杂标识符与实际的转换

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204162208407.png" alt="image-20230204162208407" style="zoom:50%;" />

## 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204162312468.png" alt="image-20230204162312468" style="zoom: 33%;" />

>  cyan region : 青色区域

# 4

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204162352344.png" alt="image-20230204162352344" style="zoom:50%;" />

## 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204163050254.png" alt="image-20230204163050254" style="zoom:50%;" />

---

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204162409863.png" alt="image-20230204162409863" style="zoom:50%;" />

## 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204163223016.png" alt="image-20230204163223016" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230204163227610.png" alt="image-20230204163227610" style="zoom:33%;" />

> **严格证明仍然不会, 等补足基础回来再看**

---

