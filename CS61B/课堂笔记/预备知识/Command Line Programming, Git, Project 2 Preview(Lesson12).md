# 预备知识及复习

## command line compilation

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126173912842.png" alt="image-20230126173912842" style="zoom: 25%;" />

> IntelliJ为我们同时做了`javac(compiler)`和`java(Interpreter)`两件事, 我们可以手动用命令行分开做

## 命令行参数

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126174246061.png" alt="image-20230126174246061" style="zoom: 25%;" />

# 实现

## approach 1-3

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126184124595.png" alt="image-20230126184124595" style="zoom: 33%;" />

> 方案二看似是合适的,实际上也是低效的:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126184420826.png" alt="image-20230126184420826" style="zoom:33%;" />

为了确定要复制哪些文件，我们必须从提交1开始遍历整个提交历史(以便找到我们要使用哪个版本的)。

> 为了解决这个问题, 我们提出了方案三

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126185611222.png" alt="image-20230126185611222" style="zoom:33%;" />

我们可以用"cache"的思路, 在每个版本中记录下每个文件使用的是哪个版本的

## **Avoiding Redundancy with “Hashing”**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230126195123273.png" alt="image-20230126195123273" style="zoom:33%;" />

> ==**Git是分布式的控制系统, 没有中央服务器**==