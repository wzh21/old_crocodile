# 本课程相关

1.study guide 是 homework的补充习题,本人简称为SG

2.WWPD: what would python display

3.==**HOF**== : High order Function : **高阶函数**



​																**chapter1 使用==函数==构造抽象**

# 1.1 引言

**调试(debugging)**的一些指导原则：

1. **逐步测试**：每个写好的程序都由小型的组件模块组成，这些组件可以独立测试。尽快测试你写好的任何东西来捕获错误
2. **隔离错误**：复杂程序的输出、表达式，或语句中的错误，通常更可以归于特定的组件模块。在尝试诊断问题、修正错误前，一定要先将它跟踪到最小的代码片段。
3. 询问他人

# 1.2 编程元素

## 1.2.3 导入库函数

例如：`from operator import add, sub, mul`
其中operator为模块名称（math也是模块名称），add,sub,mul是被导入模块的具名属性。

## 1.2.4 名称和环境

如果一个值被赋予了名称，我们就说这个名称绑定到了值上面。

**==环境：将名称绑定到值上，以及随后通过名称来检索这些值的可能，意味着解释器必须维护某种内存来跟踪这些名称和值的绑定。这些内存叫做环境。==**

名称也可以绑定到函数，比如：

```py
>>> max
<built-in function max>
```

我们可以使用赋值运算符来给现有的函数取新的名字：

```py
>>> f = max
>>> f
<built-in function max>
>>> f(3, 4)
4
```

如果给函数名赋值为其他数据类型（如：int），则再次调用该函数时会报错：

```py
>>> max(3,4)
4
>>> max = 2
>>> max(3,4)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'int' object is not callable
```

## 1.2.5 嵌套表达式的求解

为了求出调用表达式，Python 会执行下列事情：

- 求出运算符和操作数子表达式，之后
- 在值为操作数子表达式的参数上调用值为运算符子表达式的函数。

这个简单的过程大体上展示了一些过程上的重点。第一步表明为了完成调用表达式的求值过程，我们首先必须求出其它表达式。所以，**求值过程本质上是递归的，也就是说，它会调用其自身作为步骤之一。**

<img src="http://composingprograms.com/img/expression_tree.png" alt="img" style="zoom:50%;" />

这是一个表达式树(expression tree).在计算机科学中，树从顶端向下生长。每一点上的对象叫做节点(codes),比如该例中的mul,add(2, mul(4, 6)),add(3, 5).

要求根节点（root node），也就是整个表达式，即`mul(add(2, mul(4, 6)), add(3, 5))`,首先需要求出枝干节点，也就是子表达式。

叶子节点(leaf expressionns)(即：没有子节点的节点)的表达式表示函数或数值，如:mul,add,2,4,6等.
内部节点分为两部分:

1. 想要应用求值规则的表达式
2. 表达式的结果

求值：

1. 数字求值为它标明的数值
2. **名称求值为当前为当前==环境==中这个名称所关联的值**

## 1.2.6 纯函数与非纯函数

+ 纯函数:调用时除了返回值之外没有其他效果.

- 非纯函数:除了返回值,还会产生一些改变解释器或计算机状态的**==副作用==**,一个普遍的副作用就是在返回值之外生成额外的输出,比如`print()`

  ![image-20221223133759191](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223133759191.png)

![image-20221223101744134](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223101744134.png)

纯函数的特性利用:

1.第4章将说明纯函数对于编写**并发程序**至关重要,其中可以同时计算多个调用表达式.

2.pure functions can be composed more reliably into compound call expressions.

3.pure functions tend to be simpler to test. 

# 1.3 定义新的函数

```py
def <name>(<formal parameters>):
    return <return expression>
```

1.第二行**必须缩进**(一般是缩进4空格(**1个Tab**))

2.**User-defined functions are used in exactly the same way as built-in functions**. Indeed, one cannot tell from the definition of `sum_squares` whether `square` is built into the interpreter, **imported from a ==module==, or defined by the user.**

用户定义的函数与内置函数的使用方式完全相同。事实上，我们无法从 sum_squares 的定义中看出 square 是内置于解释器中的，还是从模块中导入的，或是由用户定义的。



3.return表达式并不是立即求值，它储存为**新定义函数的一部分**，只在函数最终调用时会被求出

## 1.3.1 环境

 1.What if a formal parameter has the same name as a built-in function? Can two functions share names without confusion? To resolve such questions, we must describe environments in more detail.如果一个*形式参数*与一个*内置函数*有相同的名字怎么办？两个函数可以共享名字而不产生混淆吗？为了解决这样的问题，我们必须更详细地描述**==环境==**。

2.An environment in which an expression is evaluated consists of a sequence of **==frames(帧)==**, depicted(描绘,刻画,描述) as boxes. (像下面这样)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223104742462.png" alt="image-20221223104742462" style="zoom:33%;" />

在线python助学:[Online Python Tutor - Composing Programs - Python 3](https://pythontutor.com/cp/composingprograms.html#mode=edit)可以帮助显示环境帧

**函数也会出现在环境图中**



**(像mul这样的内置函数没有正式的参数名称，所以总是用...来代替)**



**different names may refer to the same function, but that function itself has only one intrinsic name.**一个函数的名字会重复两次，一次在框架中，另一次作为函数本身的一部分。出现在函数中的名字被称为内在的名字。在框架中的名字是绑定的名字。两者之间有一个区别：**不同的名字可以指代同一个函数，但该函数本身只有一个内在的名字**。



==总结:==![image-20221223113433190](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223113433190.png)



3.**Function Signatures(函数签名)**包括 函数名,参数列表(参数类型,参数个数,参数顺序)

4.Applying a user-defined function introduces a second *local* frame, which is only accessible to that function. To apply a user-defined function to some arguments:(会多出一个独立于Global的局部frame<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223110446814.png" alt="image-20221223110446814" style="zoom:33%;" />)

5.**==The body of a function is not executed until the function is called (not when it is defined).函数的主体在函数被调用之前不会被执行(而不是在它被定义时).==**

The "**Return value**" in the `square()` frame **is not a name binding;** instead it indicates the value returned by the function call that created the frame.返回值不是名称绑定,他只是指示出返回值是什么

6.环境中帧的顺序会影响由表达式中的名称检索返回的值。我们之前说名称求解为当前环境中与这个名称关联的值。我们现在可以更精确一些：

- **==名称求解(Name evaluate)为 : 当前环境中，最先发现该名称的帧中，绑定到这个名称的值.==**
  ==-> 这个原则非常重要==

  

  **==总结==**:

  ![image-20221223113813004](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223113813004.png)

7.在**第三章**中，我们将看到这个模型如何作为实现编程语言的工作解释器的蓝图(预告)。



## 1.3.4 局部名称

1.the binding for x in different local frames are **unrelated**. The model of computation is carefully designed to ensure this independence.不同的局部框架中的x的绑定是不相关的。计算的模型被精心设计以确保这种独立性。

==**形式参数的选择不应影响函数行为**.==

```py
>>> def square(x):
        return mul(x, x)
>>> def square(y):
        return mul(y, y)
```

这个原则导致的最简单的记过就是函数参数名称应保留在函数体的局部范围中.

## 1.3.5 名称的选择

1**.函数名小写**，单词之间用**下划线分隔**。尽量使用描述性名称

2.**参数名称小写**，单词之间用**下划线分隔**，**首选单字名称（指只包含一个单词的名称）**

3.参数名称应该体现参数在函数中的作用

4.当它们的作用很明显时，单字母参数名称是可以接受的，但避免使用“l”（小写字母 ell）、“O”（大写字母 oh）或“I”（大写字母 i）以**避免与数字混淆。**

## 1.3.6 作为抽象的函数

1.要正确使用一个函数，我们应当了解它的几个方面：

1. The domain(域) of a function is the set of arguments it can take.
   函数的域是它可以采用的参数集（**输入是什么**）

2. The range of a function is the set of values it can return.
   函数的范围是它可以返回的值的集合（**输出是什么**）

3. The intent(目的) of a function is the relationship it computes between inputs and output (as well as any side effects it might generate)

   该函数输入与输出之间的关系（**==以及它可能产生的任何副作用==**）
   以`square()`函数为例：
   domain：一个实数
   range：一个非负实数
   intent：输出是输入的平方

2.The users of the function may not have written the function themselves, but may have obtained it from another programmer as a "black box". A programmer should not need to know how the function is implemented in order to use it. The Python Library has this property. Many developers use the functions defined there, but few ever inspect their implementation.函数的使用者可能不是自己写的，而是作为一个"**黑盒子** "从另一个程序员那里获得的。**程序员应该不需要知道函数是如何实现的，就可以使用它。Python 库具有这种特性。**许多开发者使用那里定义的函数，但很少有人检查过它们的实现。



## 1.3.7 运算符

```python
5 / 4等价于truediv(5, 4)
5 // 4等价于floordiv(5, 4)
```

# 1.4 函数的设计

- 定义函数时的几个注意事项：

1. **==DRY原则——Don’t repeat yourself：代码尽量不要有重复的部分，有就写成函数==**
2. **Functions should be defined generally **.函数应该被==**一般性定义**==
   如：python内没有square()函数的原因是已经有了pow()函数

## 1.4.1 文档字符串

- docstring

1. **A function definition will often include documentation describing the function, called a *docstring***, which must be indented along with the function body. 在python的def关键词下的一行用"""包裹的文字是叫做==docstring==，用来解释这个函数所做的事情。通常，第一行解释函数的作用，接下来的行分别解释参数的意义：

```python
def pressure(v, t, n):
    """Compute the pressure in pascals of an ideal gas.
	Applies the ideal gas law: http://en.wikipedia.org/wiki/Ideal_gas_law

	v -- volume of gas, in cubic meters
	t -- absolute temperature in degrees kelvin
	n -- particles of gas
	"""
	k = 1.38e-23  # Boltzmann's constant
	return n * k * t / v
```

2.使用`help(<name>)`可以获取该函数的docstring

type q可以退出help模式

```py
help(pressure)
```

3.在编写 Python 程序时，除了最简单的函数之外，所有的函数都需要包含文档字符串。

> Remember, code is written only once, but often read many times.

4.**#及其后的注释不会出现在help中**

- doctests
  在==**docstring中如果加入了>>>**==，这就是**==doctest==**，用来测试函数的正确性，比如上面那段代码
- #后面是**注释(comments)**

## 1.4.2 参数默认值(Default Argument Values)

定义函数时，默认参数应该跟在可变参数的后面。

准则：用于函数体的大多数数据值应该表示为具名参数的默认值，这样便于查看，以及被函数调用者修改。一些值**永远不会改变**，就像基本常数k_b，**应该定义在全局帧中**。

# 1.5 控制

`return`语句会重定向控制：无论什么时候执行`return`语句，函数调用的流程都会中止，返回表达式的值会作为被调用函数的返回值。

## 1.5.2 复合语句(Compound Statements)

1.At its highest level, the Python **interpreter's job is to execute programs, composed of statements.**

2.In general, Python code is a sequence of **==statements(语句)==**. **A simple statement is a single line that doesn't end in a colon(冒号)**. A compound statement is so called because it is composed of other statements (simple and compound). **Compound statements typically span multiple lines and start with a one-line ==header(标头)== ending in a ==colon==, *which identifies the type of statement*.** Together, a header and an **indented(缩进的) ==suite(块,语句块)==** of statements is called a ==**clause(子句)**==. A compound statement consists of one or more clauses:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223173759926.png" alt="image-20221223173759926" style="zoom: 50%;" />

我们可以用这些术语来理解我们已经介绍过的语句:

- Expressions, return statements, and assignment statements are simple statements.**表达式、返回语句和赋值语句是简单语句。**
- A `def` statement is a compound statement. The suite that follows the `def` header defines the function body. **def语句是一个复合语句。def标头后面的语句块定义了函数体。**

3.**每一个"suite(语句块)"内的缩进的空格数==必须==一致**

`if`语句示例

```python
>>> def absolute_value(x):
	"""Compute abs(x)."""
	if x > 0:
	return x
	elif x == 0:
	return 0
	else:
	return -x
>>> absolute_value(-2) == abs(-2)
	True
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223175342468.png" alt="image-20221223175342468" style="zoom:67%;" />

## 1.5.3 定义函数II:本地赋值(Defining Functions II : Local Assignment)

**==赋值语句的作用是将一个名字绑定到当前环境的第一个框架中的一个值==。**因此，函数体中的赋值语句不能影响全局框架。**==函数只能操作其本地环境==**这一事实对于创建模块化程序至关重要，在这种程序中，纯函数只通过它们获取和返回的值进行交互。

## 1.5.4 条件语句(Conditional Statements)

| 值   | 定义                           |
| ---- | ------------------------------ |
| 假值 | False，0，‘’，None，()，[]，{} |
| 真值 | 除了假值以外的所有值           |

三个基本的逻辑运算符: **==and or not(或与非), 而不是&&, ||, !==**

==python 中, True False **首字母大写**==

**2.==其中`and`和`or`遵循短路原则==。**

3.==**执行比较并返回布尔值的函数通常以 is 开头，后面不跟下划线（例如，isfinite、isdigit、isinstance 等**.==

## 布尔值Boolean Operators

Python also includes the **boolean operators** `and`, `or`, and `not`. These operators are used to combine and manipulate boolean values.

- not 返回表达式的相反布尔值，**并且总是返回 True 或 False。**
- and 按顺序评估表达式，**一旦到达第一个假值就停止评估（短路），然后返回(返回这个false的值)。如果所有的值都评估为真值，则返回最后一个值。**
- or按顺序评估表达式，**在第一个真值处短路，然后返回(返回这个true的值)。如果所有的值都评估为假值，则返回最后一个值。**

For example:

```python
>>> not None
True
>>> not True
False
>>> -1 and 0 and 1
0
>>> False or 9999 or 1/0
9999
```

## 1.5.5 迭代(Iteration)

`while`用法：

```python
while <expression>:
    <suite>
```

不终止的while语句叫做无限循环。**按下-C可以强制让 Python 停止循环。**

## 1.5.6 测试（Testing）

### assert

- **==assert statements(断言语句)==**
  Python中的assert语法为后面跟一个condition，如果这个condition为false，则打印AssertionError:<…>，比如：

  ```python
  assert a > 0, 'a must greater than 0'
  ```

  当被断言的表达式评估为一个真值时，执行断言语句没有任何影响。当它是一个假值时，assert会导致一个错误，停止执行。

在文件中编写Python时，可以通过使用doctest命令行选项启动Python来运行文件中的所有doctest。

### doctest

在文件中编写Python时，可以通过使用doctest命令行选项启动Python来运行文件中的所有doctest:

python3 -m doctest <python_source_file>。

有效测试的关键是在实现新功能后立即编写（并运行）测试。甚至在实现之前写一些测试也是很好的做法，以便在脑海中有一些输入和输出的例子。一个应用单一功能的测试被称为**单元测试**。**详尽的单元测试是良好程序设计的标志。**

# 1.6 高阶函数（Higher-Ord1er Functions）

Functions that manipulate functions are called higher-order functions.

高阶函数：接受**==其他函数作为参数的函数==**，或者将**==函数作为返回值的函数==**。

## 1.6.3 函数的嵌套定义（Nested Definitions）

```py
def square_root(x):
    def update(guess):
        return average(guess, x/guess)
    def test(guess):
        return approx_eq(square(guess), x)
    return iter_improve(update, test)
```

**词法作用域**：局部定义的函数也可以访问它们定义所在作用域的名称绑定。这个例子中，update引用了名称x，它是外层函数square_root的一个形参。这种在嵌套函数中共享名称的规则叫做词法作用域。严格来说，**内部函数能够访问==定义所在环境（而不是调用所在位置）的名称==。**

词法作用域的两个关键优势：

1. **局部函数的名称并不影响定义所在函数外部的名称，因为局部函数的名称绑定到了定义处的当前局部环境中，而不是全局环境。**
2. **局部函数可以访问外层函数的环境。这是因为局部函数的函数体的求值环境扩展于定义处的求值环境。**

e.g.：

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221224112834318.png" alt="image-20221224112834318" style="zoom:50%;" />

## 1.6.4 作为返回值的函数

```py
>>> def compose1(f, g):
        def h(x):
			return f(g(x))
        return h
>>> add_one_and_square = compose1(square, successor)
>>> add_one_and_square(12)
169
```

1.本地定义函数

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221224111629838.png" alt="image-20221224111629838" style="zoom:50%;" />

==这里要理解"frame(帧) 的概念,帧是存在于环境当中的,上图中返回adder,里面的k已经变为了第一次赋值的值,这个帧仍然保存在环境中,把这个帧作为函数返回值return了回去"==

2.按照顺序寻找帧中的变量

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221224113746725.png" alt="image-20221224113746725" style="zoom:50%;" />

从1->2->.......->last frame,从第一帧到最后一帧找到f(==**按顺序**==)

e.g.:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221224124629008.png" alt="image-20221224124629008" style="zoom:50%;" />

"**==从下向上找每个帧,例如这个n,先从f2里面找,没有,找到f1==**"

**还要记下parent是谁,方便于寻找parent frame里的变量**

3.**=="环境是一系列的帧"==**

![image-20221224125829817](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221224125829817.png)

4.环境变量图的big example

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221224130749833.png" alt="image-20221224130749833" style="zoom:50%;" />

## 1.6.5 ==牛顿法逼近零点==

1.We will implement an algorithm that is used broadly in machine learning, scientific computing, hardware design, and optimization.我们将实现一种广泛用于机器学习、科学计算、硬件设计和优化的算法,**牛顿法**

2.**牛顿法**是一种经典的寻找**零点**的**迭代**算法,**它被应用于求解平方根(根号),下面将要介绍原理**

(利用求解 f(x) - a = 0 的零点)

3.具体原理:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221224220343169.png" alt="image-20221224220343169" style="zoom:50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221224220403872.png" alt="image-20221224220403872" style="zoom:50%;" />

```python
完成牛顿迭代法逼近零点:
def newton_update(f, df): # 返回一个函数,对这个函数应用x,可以得到新的x的值(X k+1)
	def update(x):
		return x - f(x) / df(x)
	return update

def improve(update, close, guess = 1, max_update = 100):#max_uptade决定精度(最多更新几次)
    k = 0
    while not close(guess) and k < max_update:
        guess = update(guess);
        k = k + 1
    return guess

def approx_eq(x, y, tolerance = 1e-15):
    return abs(x - y) < tolerance

def find_zero(f, df):
	def near_zero(x):
		return approx_eq(f(x), 0)
	return improve(newton_update(f, df), near_zero)


#以上是实现细节,下面用上面的函数找零点,从而实现各种功能
def square_root_newton(a): #求a的平方根
	def f(x):
		return x * x - a
	def df(x):
		return 2 * x #求导后的函数表达式
	return find_zero(f, df)

def power(x, n):
	"""Return x * x * x * ... * x for x repeated n times."""
	product, k = 1, 0
	while k < n:
		product, k = product * x, k + 1
	return product

def nth_root_of_a(n, a):
	def f(x):
		return power(x, n) - a
	def df(x):
		return n * power(x, n-1)
	return find_zero(f, df)

>>> nth_root_of_a(2, 64)
8.0
>>> nth_root_of_a(3, 64)
4.0
>>> nth_root_of_a(6, 64)
2.0
```

4.补充:前提是函数**可微**

## 1.6.6 Currying(柯里化)

1.**定义:**

We can use higher-order functions to convert a function that takes multiple arguments into a chain of functions that **==each take a single argument==**. More specifically, given a function `f(x, y)`, we can define a function `g` such that `g(x)(y)` is equivalent to `f(x, y)`. Here, **`g` is a higher-order function that takes in a single argument `x` and returns another function that takes in a single argument `y`**. **This transformation is called *currying*.**

e.g.:

```python
def curried_pow(x):#柯里化的pow
	def h(y):
		return pow(x, y)
	return h

>>> curried_pow(2)(3)

def map_to_range(start, end, f):#(提前预习)实现map映射模式
	while start < end:
        print(f(start))
        start = start + 1

>>> map_to_range(0, 5, curried_pow(2))
1
2
4
8
16
```

2.当我们需要一个只接受一个参数的函数时，Currying是有用的。(如map映射模式)

3.柯里化和非柯里化的对比:

```py
>>> def curry2(f):
        """Return a curried version of the given two-argument function."""
        def g(x):
            def h(y):
                return f(x, y)
            return h
        return g
>>> def uncurry2(g):
        """Return a two-argument version of the given curried function."""
        def f(x, y):
            return g(x)(y)
        return f
```

## 1.6.7 Lambda表达式

1.

```py
>>> def compose1(f,g):
        return lambda x: f(g(x))
```

我们可以通过构造相应的英文语句来理解 Lambda 表达式：

```py
     lambda            x            :          f(g(x))
    "A function that    takes x    and returns     f(g(x))"
```

==**lambda和def的不同:lambda是匿名的(即使给一个名字,在给名字之前也是匿名的)**==

2.在 Python 中，我们可以使用 lambda 表达式来临时创建函数值，**它可以评估为未命名的函数**。一个 lambda 表达式评估为一个函数，它的主体是一个单一的返回表达式。赋值和控制语句是不允许的。

3.**一个lambda表达式的结果被称为lambda函数。它没有内在的名字 (因此 Python 打印出 <lambda> 作为名字)，但除此之外，它的行为与其它函数一样。**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221224224624281.png" alt="image-20221224224624281" style="zoom: 33%;" />

4.复合lambda表达式尽管简短，但却出了名的难以辨认。

一般来说，Python 风格倾向于显式 def 语句，而不是 lambda 表达式，但在需要一个简单的函数作为参数或返回值的情况下允许它们。

5.预告 : 当我们在第三章研究解释器的设计时，我们将重新审视这个话题。



==**链接:Lab 3 -> Q6 丘奇数**==



## 1.6.8  Abstractions and ==First-Class Functions==(抽象和函数一等公民)

1.The significance of higher-order functions is that they enable us to represent these abstractions explicitly as elements in our programming language, so that they can be handled just like other computational elements.**==高阶函数==的意义在于，它使我们能够在编程语言中明确地将这些==抽象概念==表示为==元素==，从而使它们能够像其他计算元素一样被处理。**

2.

编程语言对计算元素的操作方式施加限制。限制最少的元素被说成是具有一流的地位。第一类元素的一些 "权利和特权 "是：

They may be bound to names.它们可以被绑定到名字上。
		They may be passed as arguments to functions.它们可以作为参数传递给函数。
		They may be returned as the results of functions.它们可以作为函数的结果被返回。
		They may be included in data structures.它们可以被包含在数据结构中。

**==Python 给予函数完全的一流地位==**，由此获得的表达能力是巨大的。

## 1.6.9 函数装饰器（Function Decorators）

Python 提供了特殊的语法，将高阶函数用作执行`def`语句的一部分，叫做装饰器。

可以使用库的trace, 也可以自己定义trace

```py
>>> def trace1(fn):
          def wrapped(x):
            print('-> ', fn, '(', x, ')')
            return fn(x)
        return wrapped
>>> @trace1
    def triple(x):
        return 3 * x
>>> triple(12)
->  <function triple at 0x102a39848> ( 12 )
36
```

这个例子中，定义了高阶函数`trace1`，它返回一个函数，这个函数在调用它的参数之前执行`print`语句来输出参数。`triple`的`def`语句拥有一个注解，`@trace1`，它会影响`def`的执行规则

像通常一样，函数`triple`被创建了，但是，`triple`的名称并没有绑定到这个函数上，而是绑定到了在新定义的函数`triple`上调用`trace1`的返回函数值上。**==在代码中，这个装饰器等价于：==**

```py
>>> def triple(x):
      return 3 * x
>>> triple = trace1(triple)
```

<img src="https://img-blog.csdnimg.cn/837c1873dfdb42a2a267001474427770.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVHVuX3dx,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center" alt="在这里插入图片描述" style="zoom:50%;" />

注意triple这个名称绑定了`wrapped`函数，因为`triple = trace1(triple)`

再次强调,==**装饰器的精髓是 `triple` 等价于 `trace(triple)`**==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227234506491.png" alt="image-20221227234506491" style="zoom: 33%;" />

不用下面这个的原因是:不是所有的python 程序员都理解高阶函数(但对我们来说不是问题)



# 1.7 Recursive Functions 递归函数

**如果一个函数的主体直接或==间接==地调用该函数本身，则该函数被称为递归**。递归函数在 Python 中没有使用任何特殊的语法，但是它们确实需要花费一些精力去理解和创建。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221228101047823.png" alt="image-20221228101047823" style="zoom:33%;" />

This example also illustrates how functions with simple bodies can evolve complex computational processes by using recursion.此示例还说明了具有简单实体的函数如何通过使用递归来演化复杂的计算过程。



==**所有递归都可以转化为迭代,迭代是递归的特例**==



## 1.7.1  The Anatomy of Recursive Functions(递归函数的剖析)

==Treating a recursive call as a **functional abstraction** has been called a **recursive leap of faith**==

**函数抽象**通常会更清楚。也就是说，**我们不应该关心fact(n-1)在fact的主体中是如何实现的；我们应该简单地相信它能计算出n-1的阶乘。**==**将递归调用视为一个函数抽象，被称为递归信仰的飞跃**==。我们用函数本身来定义一个函数，但在验证函数的正确性时，只需相信较简单的情况会正确工作。在这个例子中，我们相信fact(n-1)会正确地计算(n-1)！；我们必须只检查n！在这个假设成立的情况下被正确计算。这样，验证一个递归函数的正确性就是一种==**归纳证明**==的形式。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221228104459544.png" alt="image-20221228104459544" style="zoom:50%;" />

​														用递归可以简化一些东西,例如不用起名字

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221228104813513.png" alt="image-20221228104813513" style="zoom:33%;" />

​						==**信仰之跃: 假设func(n - 1) 是正确的,我们只需要检查 func(n)是不是正确的就可以了**==



## 1.7.2  Mutual Recursion(互相递归调用)

例子:

- a number is even if it is one more than an odd number
- a number is odd if it is one more than an even number
- 0 is even

```py
def is_even(n):
    if n == 0:
        return True
    else:
        return is_odd(n-1)

def is_odd(n):
    if n == 0:
        return False
    else:
        return is_even(n-1)

result = is_even(4)
```

Mutually recursive functions can be turned into a single recursive function by breaking the abstraction boundary between the two functions. **通过打破两个函数之间的抽象边界，可以将相互递归的函数变成一个单一的递归函数**

As such, mutual recursion is no more mysterious or powerful than simple recursion, and it provides a mechanism for maintaining abstraction within a complicated recursive program.因此，**相互递归并不比简单递归更神秘、更强大，它提供了一种在复杂递归程序中保持抽象的机制。**



1.互相递归调用的基础条件可能在两个函数中都有,也可能只存在于一个函数

2.补充: return可以return不止一个值.如

```py
return 1, 2
```

## 1.7.3  Printing in Recursive Functions(==Order of Recursive Calls==)

例一:

```py
>>> def cascade(n):
        """Print a cascade of prefixes of n."""
        if n < 10:
            print(n)
        else:
            print(n)
            cascade(n//10)
            print(n)
>>> cascade(2013)
2013
201
20
2
20
201
2013
```

**==在递归调用之前表达基例并不是一个硬性要求==**。事实上，通过观察print(n)在条件语句的两个子句中都有重复，因此可以在它前面表达这个函数，这样可以更紧凑。

```py
>>> def cascade(n):
        """Print a cascade of prefixes of n."""
        print(n)
        if n >= 10:
            cascade(n//10)
            print(n)
```

环境图:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221228120407145.png" alt="image-20221228120407145" style="zoom: 33%;" />

例二:

```py
# 描述:如果桌子上的卵石数量是偶数，则鲍勃移走两个卵石，否则移走一个。给出n个初始卵石，由爱丽丝开始，谁赢得游戏？
>>> def play_alice(n):
        if n == 0:
            print("Bob wins!")
        else:
            play_bob(n-1)
>>> def play_bob(n):
        if n == 0:
            print("Alice wins!")
        elif is_even(n):
            play_alice(n-2)
        else:
            play_alice(n-1)
```

这个问题的一个自然分解是**将每个策略封装在自己的函数中。这样，==我们就可以在不影响其他策略的情况下修改一个策略，保持两者之间的*抽象屏障*==。**为了结合游戏的回合性质，这两个函数在每个回合结束时互相调用。

在play_bob中，我们看到函数体中可能会出现多个递归调用。然而，在本例中，对play_bob的每次调用最多调用一次play_alice。在*下一节*中，我们将考虑当单个函数调用进行*多个直接递归调用*时会发生什么情况。



例三:反级联

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221228121926347.png" alt="image-20221228121926347" style="zoom:33%;" />

```py
def inverse_cascade(n):
    """Print an inverse cascade of prefixes of n.
    >>> inverse_cascade(1234)
    1
    12
    123
    1234
    123
    12
    1
    """
    grow(n)
    print(n)
    shrink(n)

def f_then_g(f, g, n):
    if n:
        f(n)
        g(n)

grow = lambda n: f_then_g(grow, print, n//10)
shrink = lambda n: f_then_g(print, shrink, n//10)
# 写成这样相当于是lambda递归函数
```

==**lambda函数也可以用于递归!!!**==

## 1.7.4  Tree Recursion(树递归)

顺序是相当于**==中序遍历==**

```py
def fib(n):
    if n == 1:
        return 0
    if n == 2:
        return 1
    else:
        return fib(n-2) + fib(n-1)

result = fib(6)
```



<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221228132655121.png" alt="image-20221228132655121" style="zoom:33%;" />

但是到目前为止,这种方法不是有效的,因为在递归的过程中,**==有大量的重复计算==**,如下图,fib(3)就被计算了2次

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221228133421360.png" alt="image-20221228133421360" style="zoom:33%;" />

在后面的课程中将要学到如何**避免这种重复,到那时,tree recursion 就会变得高效**

## 1.7.5  Example: Partitions(困难的例子)

count_partition(m, n):对m进行分解,用不超过n的数,按升序输出

如count_partition(6, 4)

```py
6 = 2 + 4
6 = 1 + 1 + 4
6 = 3 + 3
6 = 1 + 2 + 3
6 = 1 + 1 + 1 + 3
6 = 2 + 2 + 2
6 = 1 + 1 + 2 + 2
6 = 1 + 1 + 1 + 1 + 2
6 = 1 + 1 + 1 + 1 + 1 + 1
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221228134659512.png" alt="image-20221228134659512" style="zoom: 50%;" />

分割成两种: use 4 和 don't use 4

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221228135022539.png" alt="image-20221228135022539" style="zoom:50%;" />

```py
def count_partitions(n, m):
    """Count the partitions of n using parts up to size m.
    >>> count_partitions(6, 4)
    9
    """
    if n == 0:
        return 1
    elif n < 0:
        return 0
    elif m == 0:
        return 0
    else:
        with_m = count_partitions(n-m, m)
        without_m = count_partitions(n, m-1)
        return with_m + without_m
```

==**homework 3 的Q3和本题有异曲同工之妙**==



# 第1章补充知识(来自期中复习补充)

## ==Environment diagram with lambda==

==**搞懂以下两题,相当于掌握了第一章"环境" "绑定" 等概念的精髓**==

### eg1

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227105316966.png" alt="image-20221227105316966" style="zoom:50%;" />

```py
lambda y : a + y #其中的a是绑定在Global的a = 1上的,因为这是不缩进的
f(lambda y : a + y)(a) #后面那个a, 也是绑定在Global那个

# 之后 (lambda y : a + y) 被应用, y被绑定在外层的那个(a)上(a = 1)
变为: (lambda y: (a * lambda y : 1 + y))(1) =  2 * (1 + 1) = 4
#(里层那个a是绑定在2上面的)
```

### eg2

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227115028205.png" alt="image-20221227115028205" style="zoom:80%;" />

```py
lambda horse : horse(2) # 中的horse是绑定到global的horse函数上,不是自由变量!!!
```

=="**自由变量**"==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227133853554.png" alt="image-20221227133853554" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227133917423.png" alt="image-20221227133917423" style="zoom:33%;" />

**==以上两幅图 : f1帧里面,"mask"这个名字先后绑定在了不同是两个名字上面(第一个parent = global,第二个parent  = f1)==**

链接:[Online Python Tutor - Composing Programs - Python 3](https://pythontutor.com/cp/composingprograms.html#mode=display)

```py
def horse(mask):
    horse = mask
    def mask(horse):
        return horse#这个函数是给它什么就返回什么
    return horse(mask)#此时,mask和horse都已经有了新的意义

mask = lambda horse : horse(2)
#这里,表明horse是一个函数,这个lambda表达式接受一个函数作为参数,返回horse(2)
#mask本身也是一个lambda函数

horse(mask) # = 2
#最后的结果是返回mask(horse),等于2
```

## 如何起名字

如下:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227113318407.png" alt="image-20221227113318407" style="zoom:67%;" />



## Errors and traceback

如下:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227113943841.png" alt="image-20221227113943841" style="zoom: 67%;" />



## Function Examples

如下:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227114132648.png" alt="image-20221227114132648" style="zoom:67%;" />



## ==**位置参数与关键字参数**==

[python](https://so.csdn.net/so/search?q=python&spm=1001.2101.3001.7020)强大的类型推导，有时也会带来一些副作用，比如有时编译器会报如下错误：

```
TypeError: Function takes at most 1 positional arguments (2 given)
# 函数最多接受一个位置参数，却提供了两个
```

所谓positional argument位置参数，是指用相对位置指代参数。关键字参数（keyword argument），见名知意使用关键字指代参数。位置参数或者按顺序传递参数，或者使用名字，自然使用名字时，对顺序没有要求。

> A positional argument is a name that is not followed by an equal assign（=） and default value.
>
> A keyword argument is followed by an equal sign and an expression that gives its default value.

以上的两条引用是针对**函数的定义**（definition of the function）来说的，与**函数的调用**（calls to the function），也即在函数的调用端，既可以使用位置标识参数，也可使用关键字。

```py
def fn(a, b, c=1):
    return a*b+c
print(fn(1, 2))          # 3, positional(a, b) and default(c)
print(fn(1, 2, 3))       # 5, positional(a, b)
print(fn(c=5, b=2, a=2)) # 9, named(b=2, a=2)
print(fn(c=5, 1, 2))     # syntax error
print(fn(b=2, a=2))      # 5, named(b=2, a=2) and default
print(fn(5, c=2, b=1))   # 7, positional(a), named(b).
print(fn(8, b=0))        # 1, positional(a), named(b), default(c=1)
```

