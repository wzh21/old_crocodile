# 1 Iterators Warmup

![image-20230201214203231](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201214203231.png)

![image-20230201214304168](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201214304168.png)

![image-20230201214217669](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201214217669.png)

![image-20230201214324325](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201214324325.png)

![image-20230201214333151](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201214333151.png)

![image-20230201214339957](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201214339957.png)

> **==注意, 第一题问的是`Iterable`, 第二题是`Iterator`==**

**==实际上,`Iterable`会要求重写一个`Iterator`的方法, 经常地,我们会使用一个嵌套class, 这个class implement `Iterator`==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201215200104.png" alt="image-20230201215200104" style="zoom: 33%;" />

# 2

## part1

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201215226981.png" alt="image-20230201215226981" style="zoom:50%;" />

> queue : 队列

首先，让我们定义一个迭代器。创建一个类OHIterator，该类在OHRequest对象上实现迭代器，该对象只返回具有良好描述的请求。我们的OHIterator的构造函数将接受一个OHRequest对象，该对象表示队列中的第一个OHRequest。我们提供了一个函数isGood，它接受一个描述，并说明描述是否正确。如果我们用完了办公时间请求，当迭代器尝试获取另一个请求时，我们应该抛出NoSuchElementException。

### 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201225418443.png" alt="image-20230201225418443" style="zoom: 50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201225431924.png" alt="image-20230201225431924" style="zoom:50%;" />

> **==`hasnext()`也可以有指针后移的操作!!!!==**
>
> **==迭代器初始化时可以把要迭代的对象传进去!!!!==**
>
> **==这个迭代器的class也可以不内嵌==**

## part2

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201230248115.png" alt="image-20230201230248115" style="zoom:50%;" />

### 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201230144057.png" alt="image-20230201230144057" style="zoom:50%;" />

## part3

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201230258709.png" alt="image-20230201230258709" style="zoom:50%;" />

### 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201230202770.png" alt="image-20230201230202770" style="zoom:50%;" />

# 3

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201230718375.png" alt="image-20230201230718375" style="zoom:67%;" />

现在我们有了问题2中的OHQueue，我们想添加一些功能。我们注意到了办公时间系统中的一个错误：每当一张票的描述中包含“谢谢”字样时，该票就会被放入队列两次。为了解决这个问题，我们想定义一个新的迭代器TYIterator

如果当前项目的描述包含单词“thank u”，那么它应该跳过队列中的下一个项目，因为我们知道下一个项是错误系统中的意外重复。例如，如果队列中有4个OHRequest对象具有描述[“thank u”、“thank”、“im bored”、“help me”]，则对next（）的调用应返回队列中的第0个、第2个和第3个OHRequest。注意：我们仍在对队列执行良好的描述！

提示-要检查描述中是否包含“thank u”，可以使用：

`curr.description.contains("thank u")`

### 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201230906146.png" alt="image-20230201230906146" style="zoom:50%;" />

> ==用`extends`==

# 4 (未作,仅贴)

对于testPeople类主方法中的每一行，如果打印了一些内容，请将其写在该行旁边。如果该行导致错误，那么接下来写它是编译时错误还是运行时错误，然后像该行不存在一样继续

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201231144530.png" alt="image-20230201231144530" style="zoom: 50%;" />

## 答案

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230201231204132.png" alt="image-20230201231204132" style="zoom:50%;" />