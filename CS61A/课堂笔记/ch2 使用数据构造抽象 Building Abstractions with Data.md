

​																**chapter2 使用==对象==构造抽象**

**==这一章会专注于数据==**

# 2.1 Introduction 引言

> 即使我们还不能精确描述对象如何工作，我们还是可以开始将数据看做对象，因为==**Python 中万物皆对象**==。

## 2.1.1 [Native](https://so.csdn.net/so/search?q=Native&spm=1001.2101.3001.7020) Data Types 原始(本地)数据类型

Python 中每个对象都拥有一个类型。**`type`函数可以让我们查看对象的类型：**

```python
>>> type(today)
<class 'datetime.date'>
```

**==Python包含了三种原始数值类型：整数（int）、实数（float）和复数（complex）==**

### float只是近似值

```python
>>> 7 / 3 * 3
7.0
>>> 1 / 3 * 7 * 3
6.999999999999999
```



# 2.2 Data Abstraction 数据抽象

1.To represent positions, we would like our programming language to have the capacity to couple together a latitude and longitude to form a pair, **a *compound data* value** that our programs can manipulate **as a single conceptual unit**, but which **also has two parts that can be considered individually**

2.**Data abstraction** is similar in character to **functional abstraction**. When we create a functional abstraction, the details of how a function is implemented can be suppressed, and the particular function itself can be replaced by any other function with the same overall behavior. In other words, **we can make an abstraction that separates the way the function is used from the details of how the function is implemented**. Analogously, **data abstraction isolates how a compound data value is used from the details of how it is constructed**.数据抽象的特征类似于函数抽象。当我们创建函数抽象时，函数如何实现的细节被隐藏了，而且特定的函数本身可以被任何具有相同行为的函数替换。换句话说，**我们可以构造抽象来使==函数的使用方式==和==函数的实现细节==分离。与之相似，==数据抽象是一种方法论，使我们将复合数据对象的使用细节与它的构造方式隔离==**。

3.The basic idea of data abstraction is to **structure programs so that they operate on abstract data**. That is, our programs should use data in such a way as to make ==**as few assumptions about the data as possible**==. At the same time, **a concrete data representation is defined as an independent part of the program.**数据抽象的basic idea是**构造操作抽象数据的程序**。也就是说，我们的程序应该以一种方式来使用数据，**==对数据做出尽可能少的假设==**。同时，**==需要定义具体的数据表示，独立于使用数据的程序。==**

These two parts of a program, the part that operates on abstract data and the part that defines a concrete representation, are connected by a small set of functions that implement abstract data in terms of the concrete representation. To illustrate this technique, we will consider how to design a set of functions for manipulating rational numbers.程序的这两部分，即对抽象数据进行操作的部分和定义具体表示的部分，被一小部分函数连接起来，这些函数以具体表示的方式实现抽象数据。为了说明这种技术，我们将考虑如何设计一套用于操作有理数的函数。

## 2.2.1 Example: Rational Numbers 有理数的算术

有理数通常可表示为整数的比值（即：分数）：`<numerator>/<denominator>`

用分子和分母构造有理数：

- ==**构造器**==：`make_rat(n, d)` 返回分子为n和分母为d的有理数
- **==选择器==**
  `numer(x)` 返回有理数x的分子
  `denom(x)` 返回有理数x的分母

We are using here a powerful strategy for designing programs: ***==wishful thinking==***. We haven't yet said how a rational number is represented, or how the functions `numer`, `denom`, and `rational` should be implemented. Even so, if we did define these three functions, we could then add, multiply, print, and test equality of rational numbers:

==**我们在这里正在使用一个强大的合成策略：心想事成**==。**我们并没有说有理数如何表示，或者`numer`、`denom`和`make_rat`如何实现。即使这样，如果我们拥有了这三个函数，我们就可以执行加法、乘法，以及测试有理数的相等性**，通过调用它们：

```py
>>> def add_rat(x, y):
        nx, dx = numer(x), denom(x)
        ny, dy = numer(y), denom(y)
        return make_rat(nx * dy + ny * dx, dx * dy)
>>> def mul_rat(x, y):
        return make_rat(numer(x) * numer(y), denom(x) * denom(y))
>>> def eq_rat(x, y):
        return numer(x) * denom(y) == numer(y) * denom(x)
```

现在我们拥有了由选择器函数`numer`和`denom`，以及构造器函数`make_rat`定义的有理数操作。但是我们还没有定义这些函数。我们需要以某种方式来将分子和分母粘合为一个单元。

## 2.2.2 Pairs 偶对

To enable us to implement the concrete level of our data abstraction, Python provides a compound structure called a `list`, which can be constructed by placing expressions within square brackets(方括号) separated by commas(逗号). Such an expression is called a list literal(字面上).

```
>>> [10, 20]
[10, 20]
```

元组的解构：

- 法一：**多重赋值**

  ```py
  >>> x, y = pair
  >>> x
  1
  >>> y
  2
  ```

+ 法二：**通过下标运算符**

```py
>>> pair[0]
1
>>> pair[1]
2
```

+ 法三：**通过内置函数**

```py
>>> from operator import getitem
>>> getitem(pair, 0)
1
```

通过二元组实现有理数构造器和选择器函数：

```py
>>> from fractions import gcd

>>> def make_rat(n, d):
        g = gcd(n, d) # gcd用于求n和d的最大公约数,是python的内置函数
        return (n//g, d//g)
>>> def numer(x):
        return getitem(x, 0)
>>> def denom(x):
        return getitem(x, 1)
```

## 2.2.3 Abstraction Barriers 抽象界限

1.In general, the underlying idea of data abstraction is to identify a basic set of operations in terms of which all manipulations of values of some kind will be expressed, and then to use only those operations in manipulating the data. 通常，==数据抽象的底层概念是，**基于某个值的类型的操作如何表达，为这个值的类型确定一组*基本的操作*。之后*使用这些操作来操作数据***。==



2.**我们可以将有理数系统想象为一系列层级：**

| **Parts of the program that...**                             | **Treat rationals as...**             | **Using only...**                                            |
| :----------------------------------------------------------- | :------------------------------------ | :----------------------------------------------------------- |
| Use rational numbers to perform computation使用有理数执行计算 | whole data values                     | `add_rational, mul_rational, rationals_are_equal, print_rational` |
| Create rationals or implement rational operations创建有理数或实施有理数操作 | numerators and denominators分子和分母 | `rational, numer, denom`                                     |
| Implement selectors and constructor for rationals有理数的选择器和构造函数的实现 | two-element lists                     | list literals and element selection                          |

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221229135653364.png" alt="image-20221229135653364" style="zoom:50%;" />

(图中tuple getitem改成 two-element lists)

平行线表示隔离系统不同层级的界限。该图表明了：利用`two element list`实现有理数；有理数利用`make_rat`,`numer`和`denom`实现解构，获得分子和分母；分子和分母利用`add_rat`,`mul_rat`,`eq_rat`实现运算。

这样，**==抽象界限可以表现为一系列函数。==**



3.An **abstraction barrier violation** occurs whenever a part of the program that **==can use a higher level function instead uses a function in a lower level==**. For example, a function that computes the square of a rational number is best implemented in terms of `mul_rational`, which does not assume anything about the implementation of a rational number.**==当程序的某个部分可以使用较高层次的函数而使用较低层次的函数时，就会发生违反抽象障碍。==**例如，一个计算有理数平方的函数最好用`mul_rational`来实现，它的实现不需要有任何关于有理数的假定。

例如:我们应该这样定义有理数的平方(应该使用mul_rational)

```py
>>> def square_rational(x):
        return mul_rational(x, x)
```

Referring directly to numerators and denominators would violate one abstraction barrier.==直接引用分子和分母将**违反一个抽象界限**。(像下面这样)==

```py
>>> def square_rational_violating_once(x):
        return rational(numer(x) * numer(x), denom(x) * denom(x))
```

Assuming that rationals are represented as two-element lists would violate two abstraction barriers.假设理性被表示为两个元素列表将==**违反两个抽象界限**。(像下面这样)==

```py
>>> def square_rational_violating_twice(x):
        return [x[0] * x[0], x[1] * x[1]]
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221231202014276.png" alt="image-20221231202014276" style="zoom:33%;" />

4.抽象障碍使程序更容易维护和修改。依赖于某个特定表示法的函数越少，当人们想改变这个表示法时，所需的修改就越少。`square_rational`的所有这些实现都有正确的行为，**但只有第一个实现对未来的变化是稳健(==robust,稳健的,鲁棒性==)的。即使我们改变了有理数的表示方法，`square_rational`函数也不需要更新。**相比之下，`square_rational_violating_once`在选择器或构造函数签名发生变化时就需要改变，而`square_rational_violating_twice`在有理数的实现发生变化时就需要更新。

## 2.2.4 The properties of Data 数据属性

1.通常，我们可以==**将抽象数据类型当做一些选择器和构造器的集合，并带有一些行为条件。只要满足了行为条件，这些函数就组成了数据类型的有效表示。**==

```py
>>> def pair(x, y):
        """Return a function that represents a pair."""
        def get(index):
            if index == 0:
                return x
            elif index == 1:
                return y
        return get
>>> def select(p, i):
        """Return the element at index i of pair p."""
        return p(i)
```

With this implementation, we can create and manipulate pairs.

```py
>>> p = pair(20, 14)
>>> select(p, 0)
20
>>> select(p, 1)
14
```

The functional representation, although obscure, is a perfectly adequate way to represent pairs, since it fulfills the only conditions that pairs need to fulfill. The practice of data abstraction allows us to switch among representations easily.**==函数式表示法==**虽然晦涩难懂，但却是表示数据对的一种完全适当的方式，因为它满足了数据对需要满足的唯一条件。数据抽象的做法使我们可以在表示法之间轻松切换。

2.**二元组通常叫做偶对**。

# 2.3 Sequences 序列

**序列不是特定的抽象数据类型**，**而是不同类型共有的一组行为**。

序列都有一定的属性，比如：长度、元素选择（即：利用下标提取元素）。

Python includes several **native data types** that are **sequences**, the most important of which is the `list`.

## 2.3.1 List

1.List 是一个任意长度的 sequence

内置函数:	len(),返回长度

​					 list[index]取下表为index的内容

```py
>>> digits = [1, 8, 2, 8]
>>> len(digits)
4
>>> digits[3]
8
```



2.Additionally, lists can be added together and multiplied by integers. For sequences, **==addition and multiplication do not add or multiply elements, but instead combine and replicate(复制) the sequences themselves==**. 此外，列表可以被加在一起，并与整数相乘。对于序列来说，==**加法和乘法并不是加或乘元素，而是结合和复制序列本身**。==

That is, the `add` function in the `operator` module (and the `+` operator) yields a list that is the concatenation of the added arguments. The `mul` function in `operator` (and the `*` operator) can take a list and an integer `k` to return the list that consists of `k` repetitions of the original list也就是说，operator模块中的add函数（和+运算符）得到的列表是所加参数的连接。运算符中的mul函数（和*运算符）可以接受一个列表和一个整数k，以返回由原始列表的k次重复组成的列表。

```python
>>> [2, 7] + digits * 2
[2, 7, 1, 8, 2, 8, 1, 8, 2, 8]
```



### nested list

3.Any values can be included in a list, including another list. Element selection can be applied multiple times in order to select a deeply nested element in a list containing lists.**任何数值都可以包含在一个列表中**，**包括另一个列表**。**==元素选择可以被多次应用==**，以便在一个包含列表的列表中选择一个深度嵌套的元素。

```py
>>> pairs = [[10, 20], [30, 40]]
>>> pairs[1]
[30, 40]
>>> pairs[1][0]
30
```

## 2.3.2  Sequence Iteration 序列迭代

### for循环

在许多情况下，我们想在一个序列的元素上进行迭代，并依次对每个元素进行一些计算。这种模式非常普遍，以至于 Python 有一个额外的控制语句来处理序列数据：**for语句**。

1.

```py
>>> def count(s, value):
        """Count the number of occurrences of value in sequence s."""
        total = 0
        for elem in s:
            if elem == value:
                total = total + 1
        return total
```

```python
for <name> in <expression>:
    <suite>
```

A `for` statement is executed by the following procedure:

1. Evaluate the header `<expression>`, which must yield an **iterable value.**
2. For each element value in that iterable value, in order:
   1. **Bind `<name>` to that value in the current frame.**
   2. Execute the `<suite>`.

预告:术语 "**可迭代(iterable)** "的一般定义出现在第 4 章中关于迭代器的部分。



**for循环引入了另一种可以通过语句更新环境的方式。**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221229152748049.png" alt="image-20221229152748049" style="zoom:50%;" /><img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221229152933371.png" alt="image-20221229152933371" style="zoom: 25%;" />

2.**Sequence unpacking 序列解包.** A common pattern in programs is to have a sequence of elements that are themselves sequences, but all of a **fixed(固定的) length**. A `for` statement may include multiple names in its header to "unpack" each element sequence into its respective elements. For example, we may have a list of two-element lists.

```py
>>> pairs = [[1, 2], [2, 2], [2, 3], [4, 4]]
>>> same_count = 0

>>> for x, y in pairs:
        if x == y:
            same_count = same_count + 1
>>> same_count
2
```

==**在一个固定长度的序列中，这种将多个名字绑定到多个值的模式被称为序列解包；它与我们在将多个名字绑定到多个值的赋值语句中看到的模式相同**。==



### range

1.**Ranges.** A `range` is another **built-in type of sequence** in Python, which represents a range of ***integers***. Ranges are created with `range`, which takes two integer arguments: the first number and one beyond the last number in the desired range.

**range可用于整数,==负数也可以==**

**range(begin, end) 范围是[begin, end)**

```py
>>> range(1, 10)  # Includes 1, but not 10
range(1, 10)

>>> list(range(5, 8))
[5, 6, 7]
```

**If only one argument is given, it is interpreted as one beyond the last value for a range that ==starts at 0.==**

```py
>>> list(range(4)) #range(4) == range(0, 4)
[0, 1, 2, 3]
```



2.**Ranges **commonly appear as the expression in a **`for` header** to specify the number of times that the suite should be executed: 用来指定suite应执行的次数：

 A common convention is to use a single underscore character for the name in the `for` header if the name is unused in the suite:==一个常见的约定是，如果名称在suite中未使用，则在“for”标头中使用一个**下划线字符**作为名称：==

```python
>>> for _ in range(3):
        print('Go Bears!')

Go Bears!
Go Bears!
Go Bears!
```

## 2.3.3  Sequence Processing 序列处理

### **List Comprehensions.(==列表推导式==)**

1.Many sequence processing operations can be expressed by evaluating a fixed expression for each element in a sequence and collecting the resulting values in a result sequence. In Python, a list comprehension is an expression that performs such a computation.可以通过对许多序列处理操作序列中的**每个元素评估一个固定的表达式，并在一个结果序列中收集结果值来表达**。在 Python 中，一个**==列表推导式==**是一个执行这种计算的表达式。

```py
>>> odds = [1, 3, 5, 7, 9]
>>> [x+1 for x in odds]
[2, 4, 6, 8, 10]
```

**==上面的for关键字不是for语句的一部分==**，而是**列表推导式**的一部分，因为它包含在方括号内。子表达式x+1被评估，x依次与odds的每个元素绑定，结果值被收集到一个列表中。

2.**另一个常见的序列处理操作是选择一个==满足某些条件的值的子集==**。列表理解也可以表达这种模式，例如;

```py
>>> [x for x in odds if 25 % x == 0]
[1, 5]
```



The general form of a list comprehension is:

```python
[<map expression> for <name> in <sequence expression> if <filter expression>]
```

==**可以在map expression里也用if表达式!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!(如下)**==

```py
def riffle(deck):#洗牌
    """Assume that len(DECK) is even.
    >>> riffle([3, 4, 5, 6])
    [3, 5, 4, 6]
    >>> riffle(range(20))
    [0, 10, 1, 11, 2, 12, 3, 13, 4, 14, 5, 15, 6, 16, 7, 17, 8, 18, 9, 19]
    """
    "*** YOUR CODE HERE ***"
    return [deck[i // 2] if i % 2 == 0 else deck[len(deck) // 2 + i // 2] for i in range(len(deck))]
```

==**可以在map expression里也用if表达式!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!1(如上)**==

### Aggregation 聚合

A third common pattern in sequence processing is to **aggregate all values in a sequence into a single value**. The built-in functions **`sum`, `min`, and `max`** are all examples of aggregation functions.

例:找出n的全部整除因子

```py
>>> def divisors(n):
        return [1] + [x for x in range(2, n) if n % x == 0]
>>> divisors(4)
[1, 2]
>>> divisors(12)
[1, 2, 3, 4, 6]
```

使用`divisors`，我们可以通过另一个**列表推导式**来计算从1到1000的所有完全数(定义看下面就明白了)。

```py
>>> [n for n in range(1, 1000) if sum(divisors(n)) == n]
[6, 28, 496]
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221230102225671.png" alt="image-20221230102225671" style="zoom:50%;" />

三个built-in的函数

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221230102521262.png" alt="image-20221230102521262" style="zoom: 33%;" />



<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221230102548410.png" alt="image-20221230102548410" style="zoom:33%;" />



<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221230102631666.png" alt="image-20221230102631666" style="zoom:33%;" />



还有min 和 any，是max 和 all 的补集

### HOF 高阶函数

```python
>>> def apply_to_all(map_fn, s):
        return [map_fn(x) for x in s]
>>> def keep_if(filter_fn, s):
        return [x for x in s if filter_fn(x)]
>>> def reduce(reduce_fn, s, initial):
        reduced = initial
        for x in s:
            reduced = reduce_fn(reduced, x)
        return reduced
```

#例如，reduce可以用来将一个序列的所有元素相乘:

```py
>>> reduce(mul, [2, 4, 8], 1)
64
```

我们还可以通过`keep_if()`找到完全数:

```py
>>> def divisors_of(n):
        divides_n = lambda x: n % x == 0
        return [1] + keep_if(divides_n, range(2, n))
```

### conventional names 传统(更加被广泛称为)的名字

在计算机科学界，**apply_to_all更常用的名字是map**，**keep_if更常用的名字是==filter(过滤器)==**。

在 Python 中，**内置的 map 和 filter 是这些函数的泛化(不只适用于列表,适用范围更加广泛)**，不返回列表。这些函数将在第 4 章中讨论。

上面的定义相当于将 list 构造函数应用于内置 map 和 filter 的调用结果。

```python
>>> apply_to_all = lambda map_fn, s: list(map(map_fn, s))
>>> keep_if = lambda filter_fn, s: list(filter(filter_fn, s))
```

The `reduce` function is built into the `functools` module of the Python standard library. In this version, the `initial` argument is optional**.“==reduce”函数内置在Python标准库的“functools”模块中==。**在此版本中，“initial”参数是可选的。

```py
>>> from functools import reduce
>>> from operator import mul
>>> def product(s):
        return reduce(mul, s)
>>> product([1, 2, 3, 4, 5])
120
```

在Python程序中，直接使用列表理解比使用高阶函数更常见，但这两种序列处理方法都被广泛使用。

## 2.3.4  Sequence Abstraction 序列抽象

1.Python includes two more behaviors of sequence types that extend(除了 length 和 选择) the sequence abstraction.

+ **Membership.** A value can be tested for membership in a sequence. Python has two operators `in` and `not in` that evaluate to `True` or `False` depending on whether an element appears in a sequence.

  ```py
  >>> digits
  [1, 8, 2, 8]
  >>> 2 in digits
  True
  >>> 1828 not in digits
  True
  ```

+ **Slicing.** 左闭右开[begin,end)

  In Python, sequence slicing is expressed similarly to element selection, using square brackets. A colon separates the starting and ending indices. **==Any bound that is omitted is assumed to be an extreme value==**: 0 for the starting index, and the length of the sequence for the ending index.**任何被省略的边界都被认为是一个极端值：0表示起始索引，结束索引表示序列的长度。**

  ```py
  >>> digits[0:2]
  [1, 8]
  >>> digits[1:]
  [8, 2, 8]
  ```

```py
my_string[0:]   # 忽略终点
my_string[:-1]  # 忽略起点
my_string[:]    # 都忽略
>>> x[::-1]
'!iksO ereht olleH'
```

==**反过来是`[::-1]`!!!!!!!!**==

**==`-1`表示是倒数第一!!!==**

```py
>>> a
'hello, world'
>>> a[:-1]
'hello, worl'
>>> a[::-1]
'dlrow ,olleh'
>>> a[-3:-1]
'rl'
>>> a[-5:]
'world'
>>> a[2::-1]
'leh'
>>> a[1::5]
'e d'
```

**==object[start_index : end_index : step]==**

**==如果没有缺省的话，表达式应该包含三个参数以及两个冒号,第三个参数是步长==**



**Many languages provide `map`, `filter`, `reduce` functions for sequences.**

```py
>>> map(lambda x: x*x, [1, 2, 3])
[1, 4, 9]
>>> filter(lambda x: x % 2 == 0, [1, 2, 3, 4])  # new list has only even-valued elements
[2, 4]
>>> reduce(lambda x, y: x + 2 * y, [1, 2, 3]) # (1 + 2 * 2) + 2 * 3
11
```



## 2.3.5  Strings 字符串

1.字符串满足我们在本节开头介绍的序列的两个基本条件：它们有一个长度，并且支持元素选择。

```py
>>> city = 'Berkeley'
>>> len(city)
8
>>> city[3]
'k'
```

**单引号和双引号在python中都是可以的,没有任何区别**

2.一个字符串的元素本身就是只有一个字符的字符串。**==与许多其它编程语言不同，Python 没有单独的字符类型；任何文本都是一个字符串，代表单个字符的字符串的长度为 1。==**

3.像列表一样，字符串也可以通过加法和乘法进行组合。

```py
>>> 'Berkeley' + ', CA'
'Berkeley, CA'
>>> 'Shabu ' * 2
'Shabu Shabu '
```

### Membership

 **The behavior of strings diverges from other sequence types in Python.** The string abstraction does not conform to the full sequence abstraction that we described for lists and ranges. In particular, the membership operator `in` applies to strings, but has an entirely different behavior than when it is applied to sequences. **It matches substrings rather than elements.**

```py
>>> 'here' in "Where's Waldo?"
True
```

### **Multiline Literals 多行文字**

```py
>>> """The Zen of Python
claims, Readability counts.
Read more: import this."""
'The Zen of Python\nclaims, "Readability counts."\nRead more: import this.'
```

1.三引号可以用来多行文字的字符串

2.单引号可以内套双引号,反过来也可以

3.\n虽然显示为两个字符（反斜杠和 "n"），**但在长度和元素选择方面，它被认为是一个单一的字符。**

len("\n") = 1

### **String Coercion 字符串强制转换**

A string can be created from any object in Python by calling the `str` constructor function with an object value as its argument. This feature of strings is useful for constructing descriptive strings from objects of various types.通过调用带有对象值作为参数的“str”构造函数，**可以从Python中的任何对象创建字符串**。字符串的这个特性对于从各种类型的对象构造描述性字符串非常有用。

```py
>>> str(2) + ' is an element of ' + str(digits)
'2 is an element of [1, 8, 2, 8]'
```

## 2.3.6  Trees 树

Our ability to use lists as the elements of other lists provides a new means of combination in our programming language. This ability is called a ==***closure property***== of a data type. 我们使用列表作为其他列表元素的能力为我们的编程语言提供了一种新的组合方式。这种能力称为数据类型的***==闭包属性==*。**

### *box-and-pointer* notation

1.Primitive values such as numbers, strings, boolean values, and `None` appear within an element box. Composite values, such as function values and other lists, are indicated by an arrow.数字、字符串、布尔值和“None”等基本值出现在元素框中。复合值（如函数值和其他列表）由箭头指示。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221230095153100.png" alt="image-20221230095153100" style="zoom:50%;" />

2.A tree has a **root label** and a sequence of **branches.** **Each branch of a tree is a tree**. A tree with no branches is called a **leaf**. Any tree contained within a tree is called **a sub-tree** of that tree (such as a branch of a branch). The root of each sub-tree of a tree is called a **node** in that tree.

The data abstraction for a tree consists of the **constructor `tree`** and the **selectors** **`label` and `branches`**. 

```py
>>> def tree(root_label, branches=[]):
        for branch in branches:
            assert is_tree(branch), 'branches must be trees'
        return [root_label] + list(branches)
>>> def label(tree):
        return tree[0]
>>> def branches(tree):
        return tree[1:]
```

A tree is well-formed only if it has a root label and all branches are also trees.

```py
>>> def is_tree(tree):
        if type(tree) != list or len(tree) < 1:
            return False
        for branch in branches(tree):
            if not is_tree(branch):
                return False
        return True
```

The `is_leaf` function checks whether or not a tree has branches.

```py
>>> def is_leaf(tree):
        return not branches(tree)
```

应用:

```py
>>> t = tree(3, [tree(1), tree(2, [tree(1), tree(1)])])
>>> t
[3, [1], [2, [1], [1]]] # 生成tree的时候从里到外
>>> label(t)
3
>>> branches(t)
[[1], [2, [1], [1]]]
>>> label(branches(t)[1]) # lable([2, [1], [1]])
2
>>> is_leaf(t)
False
>>> is_leaf(branches(t)[0])
True
```

==**youtube笔记:**==

**==没有违反抽象原则,因为在定义tree的时候就说了,tree的branch是一个抽象列表==**



例子:

+ 斐波那契树

  ```python
  >>> def fib_tree(n):
          if n == 0 or n == 1:
              return tree(n)
          else:
              left, right = fib_tree(n-2), fib_tree(n-1)
              fib_n = label(left) + label(right)
              return tree(fib_n, [left, right])
  >>> fib_tree(5)
  [5, [2, [1], [1, [0], [1]]], [3, [1, [0], [1]], [2, [1], [1, [0], [1]]]]]
  ```

+ 数叶子的个数

  ```py
  >>> def count_leaves(tree):
        if is_leaf(tree):
            return 1
        else:
            branch_counts = [count_leaves(b) for b in branches(tree)]
            return sum(branch_counts)
  >>> count_leaves(fib_tree(5))
  8
  ```

==**youtube笔记:**==

**==sum(list)只去除一层抽象(括号)如下:==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221231204402053.png" alt="image-20221231204402053" style="zoom:50%;" />

## 2.3.7  Linked Lists 链表

![image-20230108210755856](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230108210755856.png)



```py
# 实现属于linked List 的range, map, fliter
def range_link(start, end):
    """Return a Link containing consecutive integers from start to end.

    >>> range_link(3, 6)
    Link(3, Link(4, Link(5)))
    """
    if start >= end:
        return Link.empty
    else:
        return Link(start, range_link(start + 1, end))


def map_link(f, s):
    """Return a Link that contains f(x) for each x in Link s.

    >>> map_link(square, range_link(3, 6))
    Link(9, Link(16, Link(25)))
    """
    if s is Link.empty:
        return s
    else:
        return Link(f(s.first), map_link(f, s.rest))


def filter_link(f, s):
    """Return a Link that contains only the elements x of Link s for which f(x)
    is a true value.

    >>> filter_link(odd, range_link(3, 6))
    Link(3, Link(5))
    """
    if s is Link.empty:
        return s
    filtered_rest = filter_link(f, s.rest)
    if f(s.first):
        return Link(s.first, filtered_rest)
    else:
        return filtered_rest
```

# 2.4  Mutable Data 可变数据

我们已经看到了抽象在帮助我们应对大型系统的复杂性时如何至关重要。有效的程序整合也需要一些组织原则，指导我们构思程序的概要设计。特别地，**==我们需要一些策略来帮助我们构建大型系统，使之模块化。也就是说，它们可以“自然”划分为可以分离开发和维护的各个相关部分。==**

我们用于创建模块化程序的强大工具之一，是引入可能会随时间改变的新类型数据。这样，单个数据可以表示独立于其他程序演化的东西。对象行为的改变可能会由它的历史影响，就像世界中的实体那样。**向数据添加状态是这一章最终目标：面向对象编程范式的要素。**

## 2.4.1  The Object Metaphor 对象隐喻/比喻

1.当我们在数据中包含函数值时，我们承认**==数据也可以有行为。函数可以作为数据进行操作==**，但也可以被调用来执行计算。

"**==函数也是数据==**"

2.*Objects* combine data values with behavior. Objects represent information, but also *behave* like the things that they represent. The logic of how an object interacts with other objects is bundled along with the information that encodes the object's value. Objects are both information and processes, bundled together to represent the properties, interactions, and behaviors of complex things.*对象*将数据值与行为相结合。对象表示信息，但也表现得像它们所表示的事物。**对象如何与其他对象交互的逻辑与编码对象值的信息绑定在一起。==对象既是信息又是过程==**，捆绑在一起表示复杂事物的属性、交互和行为。

对象行为通过专门的对象语法和相关术语在Python中实现，我们可以通过示例介绍。date是一种对象。

```py
>>> from datetime import date
```

名称日期绑定到类。正如我们所看到的，类代表一种价值。单个日期称为该类的**实例**(==*instances*==)。可以通过对表征实例的参数调用类来构造实例(通过构造函数构造)。

3.对象具有*属性，这些属性(attributes)是作为对象一部分的命名值。在Python中，像许多其他编程语言一样，我们使用点符号来指定对象的属性。

> <expression> . <name>

 **these attribute names are not available in the general environment这些属性名称在常规环境中不可用**

4.==**Objects also have *methods*, which are function-valued attributes**==

==**对象还具有*方法*，它们是函数值属性**==

5.Dates are objects, but numbers, strings, lists, and ranges are all objects as well. 

```py
>>> '1234'.isnumeric()
True
>>> 'rOBERT dE nIRO'.swapcase()
'Robert De Niro'
>>> 'eyes'.upper().endswith('YES')
True
```

==**In fact, all values in Python are objects. That is, all values have behavior and attributes. They act like the values they represent.**==



## 2.4.2  Sequence Objects

```py
>>> chinese = ['coin', 'string', 'myriad']  # A list literal
>>> suits = chinese                         # Two names refer to the same list

>>> suits.pop()             # Remove and return the final element
'myriad'
>>> suits.remove('string')  # Remove the first element that equals the argument

>>> suits.append('cup')              # Add an element to the end
>>> suits.extend(['sword', 'club'])  # Add all elements of a sequence to the end

>>> suits
['coin', 'cup', 'spade', 'club']
```

**Sharing and Identity.** Because we have been changing a single list rather than creating new lists, the object bound to the name `chinese` has also changed, ==**because it is the same list object that was bound to `suits`!**==

```py
>>> chinese  # This name co-refers with "suits" to the same changing list
['heart', 'diamond', 'spade', 'club']
```

两个名字Chinese和suits被绑定在了同一个对象上

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230101105741292.png" alt="image-20230101105741292" style="zoom:33%;" />

```py
>>> nest = list(suits)  # Bind "nest" to a second list with the same elements
>>> nest[0] = suits     # Create a nested list
```

According to this environment, changing the list referenced by `suits` will affect the nested list that is the first element of `nest`, but not the other elements.

```py
>>> suits.insert(2, 'Joker')  # Insert an element at index 2, shifting the rest
>>> nest
[['heart', 'diamond', 'Joker', 'spade', 'club'], 'diamond', 'spade', 'club']
```

And likewise, undoing this change in the first element of `nest` will change `suit` as well.

```py
>>> nest[0].pop(2)
'Joker'
>>> suits
['heart', 'diamond', 'spade', 'club']
```

==**区分深拷贝和浅拷贝的区别**,这里的nest[0]是被绑定到一起了,但是list(suits)只是浅拷贝==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230101110159019.png" alt="image-20230101110159019" style="zoom:50%;" />

### 操作符is 和not is(identical)

**==Because two lists may have the same contents but in fact be different lists, we require a means to test whether two objects are the same. Python includes two comparison operators, called `is` and `is not`, that test whether two expressions in fact evaluate to the identical object. Two objects are identical if they are equal in their current value, and any change to one will always be reflected in the other.==**

**==Identity is a stronger condition than equality.==** 

**==is > \=\===**

```py
>>> suits is nest[0]
True
>>> suits is ['heart', 'diamond', 'spade', 'club']
False
>>> suits == ['heart', 'diamond', 'spade', 'club']
True
```

> **==is看是不是同一个对象的不同名称,而\=\=只是看值是否相等==**
>
> **==The former checks for identity, while the latter checks for the equality of contents.==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230101162814467.png" alt="image-20230101162814467" style="zoom:67%;" />

**==因为序列也是对象,万物皆对象==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230101163231259.png" alt="image-20230101163231259" style="zoom: 67%;" />

==**默认参数是函数的一部分(创建函数的时候就存在了),而不是调用函数的时候才有,所以,不同的调用函数,这个函数的默认参数都是指向同一个!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!**==

### tuples 元组

A tuple, an instance of the built-in `tuple` type, is an immutable sequence. Tuples are created using a tuple literal that separates element expressions by commas. Parentheses are optional but used commonly in practice. Any objects can be placed within tuples.元组是内置元组类型的一个实例，是一个==**不可变**==的序列。元组是用一个元组字面来创建的，用逗号来分隔元素表达式。**圆括号是可选的，但在实践中经常使用。任何对象都可以放在元组中。**

```py
>>> 1, 2 + 3
(1, 5)
>>> ("the", 1, ("and", "only"))
('the', 1, ('and', 'only'))
>>> type( (10, 20) )
<class 'tuple'>
```

Empty and one-element tuples have special literal syntax.

```py
>>> ()    # 0 elements
()
>>> (10,) # 1 element
(10,)
```

Like lists, tuples have a finite length and support element selection. T**hey also have a few methods that are also available for lists, such as `count` and `index`.**

```py
>>> code = ("up", "up", "down", "down") + ("left", "right") * 2
>>> len(code)
8
>>> code[3]
'down'
>>> code.count("down")
2
>>> code.index("left")
4
```

**==However, the methods for manipulating the contents of a list are not available for tuples because tuples are immutable.然而，由于元组是不可变的，因此用于处理列表内容的方法不适用于元组。==**

==**While it is not possible to change which elements are in a tuple, it is possible to change the value of a mutable element contained within a tuple.虽然无法更改元组中的元素，但可以更改元组中包含的可变元素的值。**==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230101111613978.png" alt="image-20230101111613978" style="zoom:50%;" />



## 2.4.3  Dictionaries

**youtube网课总结(和下面text的笔记完全重叠):**

### 一些常用的函数

```py
numerals = {'I': 1, 'V': 5, 'X': 10}
>>> len(numerals)
3
>>> list(nimerals)
['I', 'V', 'X']
# 实质上是一个key的sequence
要想 返回value,需要用到
>>> list(numerals.value())
[1, 5, 10]
>>> sum(numerals.value())
16
```

+ get()函数

之前在学习python的时候，在获取值得时候常用的方法就是直接

```python
print(dict[key])
```

但这种方法中当字典中不存在该键时会返回KeyError类型错误，此时就可以用get()函数还利用键获取值

```python
print(dict.get(key))
```

利用get()函数操作时当字典中不存在输入的键时会返回一个None，这样程序运行时就不会出异常

> 如果有两个参数,那么第二个参数就是找不到这个key时返回的value(而不返回None)

### Limitations on Dictionaries

(1) key不能是列表或者字典,但是value可以是列表或者字典

(2) 不要在一个字典中添加两个同样的key,如果一个key对应两个value,那么把这两个value放到一个list(列表)中

+ A key of a dictionary cannot be a list or a dictionary (or any mutable type) 

+ Two keys cannot be equal; There can be at most one value for a given key 

**This first restriction is tied to Python's underlying implementation of dictionaries The second restriction is part of the dictionary abstraction** **第一个限制与Python的字典底层实现有关。第二个限制是字典抽象的一部分**

**以下为text的笔记**:

1.

```python
>>> numerals['I'] = 1
>>> numerals['L'] = 50
>>> numerals
{'I': 1, 'X': 10, 'L': 50, 'V': 5}
```

Notice that `'L'` was not added to the end of the output above. Dictionaries are unordered collections of key-value pairs. When we print a dictionary, the keys and values are rendered in some order, but as users of the language we cannot predict what that order will be. The order may change when running a program multiple times请注意，“L”没有添加到上面输出的末尾。==**字典是键值对的无序集合**==。当我们打印字典时，键和值会以某种顺序呈现，但作为语言的用户，我们无法预测该顺序。==**当多次运行程序时，顺序可能会改变**==。

2.Dictionaries do have some restrictions:

- ==**A key of a dictionary cannot be or contain a mutable value.字典的键不能是或包含可变值。**==
- **==There can be at most one value for a given key.==**

**==Tuples are commonly used for keys in dictionaries because lists cannot be used.(因为tuple是不可变的,符合标准)==**

3.A useful method implemented by dictionaries is `get`, which returns either the value for a key, if the key is present, or a **default value.(缺省值)** The arguments to `get` are the key and the default value.词典也有类似于列表的理解语法。键表达式和值表达式用冒号分隔。评估词典理解能力会创建一个新的词典对象。

```py
>>> numerals.get('A', 0)
0
>>> numerals.get('V', 0)
5
```

### dictionary comprehension 字典推导式

```py
>>> {x: x*x for x in range(3,6)}
{3: 9, 4: 16, 5: 25}
```

## 2.4.4  Local State 局部状态

Lists and dictionaries have *local state*: they are changing values that have some particular contents at any point in the execution of a program

Functions can also have local state<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230101115618183.png" alt="image-20230101115618183" style="zoom:33%;" />

this user-defined function is **non-pure**. Calling the function not only returns a value, but also has the **side effect** of changing the function in some way, so that the next call with the same argument will return a different result. This side effect is a result of `withdraw` making a change to a name-value binding outside of the current frame.他的副作用是“withdraw”对当前框架之外的名称值绑定进行更改的结果。

### nonlocal

**==一句话总结: nonlocal是绑定在外部的已有的名字上的==**

函数make_withdraw是一个高阶函数，它接受一个初始余额作为参数。函数 withdraw 是其返回值。

```py
>>> def make_withdraw(balance):
        """Return a withdraw function that draws down balance with each call."""
        def withdraw(amount):
            nonlocal balance                 # Declare the name "balance" nonlocal
            if amount > balance:
                return 'Insufficient funds'
            balance = balance - amount       # Re-bind the existing balance name
            return balance
        return withdraw
```

**==nonlocal:无论什么时候我们修改了名称`balance`的绑定，绑定都会在`balance`所绑定的第一个帧中修改。==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230101154715770.png" alt="image-20230101154715770" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230101154744408.png" alt="image-20230101154744408" style="zoom:50%;" />

==只会在第一个帧里有balance==

不加nonlocal的环境图(每一个函数帧里都有balance)

**==看清楚,f2和f3的父帧是f1!!!!!!!!!!!!!!!!!!!!!!==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230101155055328.png" alt="image-20230101155055328" style="zoom:25%;" />

==**nonlocal再理解:表明这个名字不是本地的,而是更往外一层的(只是一层,不是多层)**==

### 在一个函数的主体中，一个名字的*所有实例*必须指代同一个框架

关于名字的查找，Python 也有一个不寻常的限制：**==在一个函数的主体中，一个名字的*所有实例*必须指代同一个框架==**。因此，Python 不能在非本地框架中查找一个名字的值，然后在本地框架中绑定同一个名字，因为同一个名字会在同一个函数的两个不同框架中被访问。这个限制允许 Python 在执行函数的主体之前预先计算哪个帧包含每个名字。当这个限制被违反时，会产生一个混乱的错误信息。为了演示，下面重复了 make_withdraw 的例子，去掉了非本地语句。

![image-20230101164255293](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230101164255293.png)

This `UnboundLocalError` appears because `balance` is assigned locally in line 5, and so Python assumes that all references to `balance` must appear in the local frame as well. ==出现此“UnboundLocalError”是因为“balance”在第5行中***被本地分配***==，因此Python假设所有对“balance”的引用也必须出现在本地帧中。

## 2.4.5  The Benefits of Non-Local Assignment

未完待续(可能略,非课程重点)



# 2.5  Object-Oriented Programming 面向对象编程

**A ==class== describes the general behavior of its ==instances==**

## 2.5.2  Defining Classes

### class statement

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105143733187.png" alt="image-20230105143733187" style="zoom: 67%;" />

当类的语句被执行时，新的类会被创建，并且在当前环境第一帧绑定到`<name>`上。之后会执行语句组。任何名称都会在`class`语句的`<suite>`中绑定，通过`def`或赋值语句，创建或修改类的属性。

### object construction<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105143753467.png" alt="image-20230105143753467" style="zoom:80%;" />

**==这个特殊的`__init__`是构造函数==**

**==注意:balance和holder在`__init__`里面,所以被绑定在了实例上,作为*属性*==**

### object identity

![image-20230105143612687](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105143612687.png)

```py
c = a 表明一个对象的两个名字
```

### **Methods.**

每个方法定义同样包含特殊的首个参数`self`，它绑定到方法所调用的对象上

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105144821236.png" alt="image-20230105144821236" style="zoom:50%;" />

## 2.5.3  Message Passing and Dot Expressions

**方法和函数。**当一个方法在对象上调用时，对象隐式地作为第一个参数传递给方法。也就是说，==点运算符左边值为`<expression>`的对象，会自动传给点运算符右边的方法，作为第一个参数。所以，对象绑定到了参数`self`上。==

![image-20230105145407718](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105145407718.png)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105145839016.png" alt="image-20230105145839016" style="zoom:33%;" />

**实践指南：命名惯例。** ==**类名称**==通常以==**首字母大写**==来编写（也叫作驼峰拼写法，因为名称中间的大写字母像驼峰）。**==方法名称==**遵循函数命名的惯例，使用以**==下划线分隔的小写字母==。**

有的时候，有些实例变量和方法的维护和对象的一致性相关，我们不想让用户看到或使用它们。它们并不是由类定义的一部分抽象，而是一部分实现。**==Python 的惯例规定，如果属性名称以下划线开始，它只能在方法或类中访问，而不能被类的用户访问。==**

## 2.5.4  Class Attributes

**==和`__init__`方法里面定义的属性区分,这个更类似于"`static `"==**

有些属性值在特定类的所有对象之间共享。这样的属性关联到类本身，而不是类的任何独立实例。例如，让我们假设银行以固定的利率对余额支付利息。这个利率可能会改变，但是它是在所有账户中共享的单一值。

类属性由`class`语句组中的赋值语句创建，位于任何方法定义之外。在更宽泛的开发者社群中，==**类属性也被叫做类变量或静态变量**==。下面的类语句以名称`interest`为`Account`创建了类属性。

```py
>>> class Account(object):
        interest = 0.02            # A class attribute
        def __init__(self, account_holder):
            self.balance = 0
            self.holder = account_holder
        # Additional methods would be defined here
```

这个属性仍旧可以通过类的任何实例来访问。

```py
>>> tom_account = Account('Tom')
>>> jim_account = Account('Jim')
>>> tom_account.interest
0.02
>>> jim_account.interest
0.02
```

但是，对类属性的单一赋值语句会改变所有该类实例上的属性值。

```py
>>> Account.interest = 0.04
>>> tom_account.interest
0.04
>>> jim_account.interest
0.04
```

![image-20230105151352629](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105151352629.png)

**==非常重要的图!!!!!!!==**

## 2.5.5  Inheritance 继承

> 使用场景 : 我们发现相似的类在**特化的程度**上有区别。两个类可能拥有相似的属性，但是一个表示另一个的**特殊情况**。

> 术语“父类”和“超类”通常等同于“基类”，而“派生类”通常等同于“子类”。

子类继承了基类的属性，但是可能覆盖特定属性，包括特定的方法。使用继承，我们只需要关注基类和子类之间有什么不同。**任何我们在子类未指定的东西会自动假设和基类中相同。**

继承意味着在类之间表达“is-a”关系，它和“has-a”关系相反。活期账户是（is-a）一种特殊类型的账户，所以让`CheckingAccount`继承`Account`是继承的合理使用。另一方面，银行拥有（has-a）所管理的银行账户的列表，所以二者都不应继承另一个。

## 2.5.6  Using Inheritance 使用继承

**==我们通过将基类放置到类名称后面的圆括号内来指定继承。==**首先，我们提供`Account`类的完整实现，也包含类和方法的文档字符串。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105152321098.png" alt="image-20230105152321098" style="zoom:33%;" />

`CheckingAccount`的完整实现在下面：

```py
>>> class CheckingAccount(Account):
        """A bank account that charges for withdrawals."""
        withdraw_charge = 1
        interest = 0.01
        def withdraw(self, amount):
            return Account.withdraw(self, amount + self.withdraw_charge)
```

**调用祖先。**被覆盖的属性仍然可以通过类对象来访问。例如，我们可以通过以包含`withdraw_charge`的参数调用`Account`的`withdraw`方法，来实现`CheckingAccount`的`withdraw`方法。(上例)

在类中“查找名称”的行为会在原始对象的类的继承链中的每个基类中查找。我们可以递归定义这个过程，为了在类中查找名称：

1.  如果类中有带有这个名称的属性，返回属性值。
2.  否则，如果有基类的话，在基类中查找该名称。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105160825919.png" alt="image-20230105160825919" style="zoom:50%;" />

Inheritance : `is-a`

Composition: `has-a`

### exersize

![image-20230105161256166](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105161256166.png)

b.z.z.z = 1之后1.z是error

### 2.5.7  Multiple Inheritance 多重继承

可以继承自多个基类(父类)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105162453990.png" alt="image-20230105162453990" style="zoom:33%;" />

```py
>>> class AsSeenOnTVAccount(CheckingAccount, SavingsAccount):
        def __init__(self, account_holder):
            self.holder = account_holder
            self.balance = 1           # A free dollar!
```

## ==关于 self 和cls==

**Summary:** In this tutorial, we will uncover the difference between self and cls in the Python programming language.

### self

`self` in Python ==**holds the reference of the current working instance (保存当前工作实例的引用)**==. This reference is passed as the first argument to every [instance methods](https://pencilprogrammer.com/python-tutorials/static-class-method/) of the class **==by Python itself==.**

```py
class MyClass:
    def what_is_self(self):
        print(self)
        
MyClass().what_is_self()  #outputs <__main__.MyClass object at 0x7fef4c820d00>
```

### cls

`cls` in Python **==holds the reference of the class(保存class的引用)==**. It is passed as the first argument to every [class methods](https://pencilprogrammer.com/python-tutorials/static-class-method/) (==methods with `@classmethod` decorator==) ==**by Python itself(被python自己传进去了!!!!!)**==.

```py
class MyClass:
    @classmethod
    def what_is_cls(cls):
        print(cls)
        
MyClass().what_is_cls()  #outputs <class '__main__.MyClass'>
```

> It is important to note that `self` and `cls` are not reserved keywords. They are just naming conventions in Python.需要注意的是，**==“self”和“cls”不是保留关键字。它们只是Python中的命名约定。==**
>
> It means we can rename them to whatever we want, the underlying significance will remain the same.

### self vs cls

Since `self` refers to the instance and `cls` refers to the class, they differ in terms of scope and accessibilty.

| **self**                                                     | **cls**                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| self holds the reference of the current working instance.    | cls holds the reference of the current working class.        |
| ==self also holds the reference of the class as `self.__class_**_`==** | **cls doesn’t hold any reference to any instances of the class.** |
| self has access to **both [instance and class data members](https://pencilprogrammer.com/python-tutorials/class-and-instance-variables/) of the current working instance.** | Using `cls` we can access only the class data members of the current class. |
| Variables initialized using self get declared in the instance’s scope. | Variable initialized using cls get declared in the class’s scope. |

Here are some examples that shows the difference between `self` and `cls` in Python.

### Difference 1: cls cannot access instance data members

```py
class MyClass:
    #class variable
    cvar = "class variable"
    
    def __init__(self):
        #instance variable
        self.ivar = "intance variable"
    
    @classmethod
    def display_using_cls(cls):
        print(cls.cvar)
        #print(cls.ivar)  #raises AttributeError
    
    #instance method
    def display_using_self(self):
        print(self.cvar)
        print(self.ivar)
        
obj = MyClass()
obj.display_using_cls()
obj.display_using_self()
```

### Difference 2: self contains cls, whereas vice versa is not True

```py
class MyClass:
    #instance method    
    def self_contains_cls(self):
        print(self.__class__)   #outputs <class '__main__.MyClass'>
        
obj = MyClass()
obj.self_contains_cls()
```

In conclusion, `self` refers to the current working instance and is passed as the first argument to all the instance methods whereas `cls` refers to the class and is passed as the first argument to all the class methods.

**==总之，“self”是指当前工作实例，并作为第一个参数传递给所有实例方法，而“cls”是指类，并作为所有类方法的第一个参数==。**

# 2.7  Object Abstraction

A central concept in object abstraction is a ***==generic function(泛型函数)==***, which is a function that **can accept values of multiple different types.** We will consider three different techniques for implementing generic functions: ==**shared interfaces(共享接口), type dispatching(类型分派), and type coercion(类型强制转换).**== In the process of building up these concepts, we will also discover features of the Python object system that support the creation of generic functions.

## 2.7.1  String Conversion

To represent data effectively, an object value should behave like the kind of data it is meant to represent, **including producing a string representation of itself**. String representations of data values are especially important in an **interactive language such as Python that automatically displays the string representation of the values of expressions in an interactive session.**

字符串值提供了在人类之间传递信息的基本媒介。字符序列可以在屏幕上呈现、打印到纸上、大声朗读、转换成盲文或以莫尔斯电码的形式广播。字符串也是编程的基础，因为它们可以表示Python表达式。

Python stipulates that all objects should produce two different string representations: one that is human-interpretable text and one that is a Python-interpretable expression. The constructor function for strings, `str`, returns a human-readable string. Where possible, the `repr` function returns a Python expression that evaluates to an equal object. The docstring for *repr* explains this property:Python规定所有对象都应该生成两种不同的字符串表示：==**一种是人类可解释的文本，另一种是Python可解释的表达式字符串的构造函数“str”返回人类可读的字符串。在可能的情况下，“repr”函数返回一个Python表达式，该表达式的计算结果为一个相等的对象**==*repr*的docstring解释了此属性：

```py
repr(object) -> string

Return the canonical string representation of the object.返回对象的规范字符串表示形式。
For most object types, eval(repr(object)) == object.对于大多数对象类型，eval（repr（object））== object。
```

> eval:evaluate

The result of calling `repr` on the value of an expression is what Python prints in an interactive session.

**对表达式的值调用“repr”的结果是Python在交互式会话中打印的内容。**

```py
>>> 12e12
12000000000000.0
>>> print(repr(12e12))
12000000000000.0
```

In cases where no representation exists that evaluates to the original value, Python typically produces a description surrounded by angled brackets.**如果不存在计算为原始值的表示，Python通常会生成一个用尖括号括起来的描述。**

```py
>>> repr(min)
'<built-in function min>'
```

The `str` constructor often coincides with `repr`, but provides a more interpretable text representation in some cases. For instance, we see a difference between `str` and `repr` with dates.**“str”构造函数通常与“repr”一致，但在某些情况下提供了更易于解释的文本表示。例如，我们看到带有日期的“str”和“repr”之间存在差异。**

```py
>>> from datetime import date
>>> tues = date(2011, 9, 12)
>>> repr(tues)
'datetime.date(2011, 9, 12)'
>>> str(tues)
'2011-09-12'
```

Defining the `repr` function presents a new challenge: we would like it to apply correctly to all data types, even those that did not exist when `repr` was implemented. We would like it to be a generic or *polymorphic function*, one that can be applied to many (*poly*) different forms (*morph*) of data.定义“repr”函数提出了**一个新的挑战：我们希望它能正确应用于所有数据类型，即使是在实现“repr’时不存在的数据类型。我们希望它是一个泛型或*多态函数***，**可以应用于许多（*poly*）不同形式（*morph*）的数据。**

The object system provides an elegant solution in this case: the `repr` function always invokes a method ==**called `__repr__` on its argument**==.在这种情况下，对象系统提供了一个优雅的解决方案：**==“repr”函数总是在其参数上调用名为“_\_\_repr_\_\_”的方法。==**

```py
>>> tues.__repr__()
'datetime.date(2011, 9, 12)'
```

By implementing this same method in user-defined classes, we can extend the applicability of `repr` to any class we create in the future. This example highlights another benefit of dot expressions in general, that they provide a mechanism for extending the domain of existing functions to new object types.通过在用户定义的类中实现相同的方法，**我们可以将“repr”的适用性扩展到将来创建的任何类**。这个例子突出了点表达式的另一个好处，即它们提供了一种将现有函数的域扩展到新对象类型的机制。

The `str` constructor is implemented in a similar manner: it invokes a method called `__str__` on its argument.

“str”构造函数以类似的方式实现：它在其参数上调用名为“__str__”的方法。

```py
>>> tues.__str__()
'2011-09-12'
```

These polymorphic functions are examples of a more general principle: certain functions should apply to multiple data types. Moreover, one way to create such a function is to use a shared attribute name with a different definition in each class.**这些多态函数是更一般原则的示例：某些函数应应用于多个数据类型。此外，创建此类函数的一种方法是在每个类中使用具有不同定义的共享属性名称。**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230107125157127.png" alt="image-20230107125157127" style="zoom:50%;" />



## 2.7.2  Special Methods

In Python, certain *special names* are invoked by the Python interpreter in special circumstances. For instance, the `__init__` method of a class is automatically invoked whenever an object is constructed. The `__str__` method is invoked automatically when printing, and `__repr__` is invoked in an interactive session to display values.**==在Python中，某些*特殊名称*在特殊情况下由Python解释器调用。例如，每当构造对象时，类的`__init__`方法都会自动调用。打印时自动调用`__str__`方法，在交互式会话中调用`__repr_`以显示值。==**

There are special names for many other behaviors in Python. Some of those used most commonly are described below.Python中的许多其他行为都有特殊的名称。下面介绍一些最常用的方法。

### ==`__bool__`==

**True and false values.** We saw previously that numbers in Python have a truth value; more specifically, 0 is a false value and all other numbers are true values. In fact, all objects in Python have a truth value. By default, objects of user-defined classes are considered to be true, but the special `__bool__` method can be used to override this behavior. If an object defines the `__bool__` method, then Python calls that method to determine its truth value.**真值和假值。**我们之前看到Python中的数字具有真值；更具体地说，0是一个假值，而所有其他数字都是真值。事实上，**==Python中的所有对象都有一个真值==**。默认情况下，==**用户定义类的对象被视为true，但可以使用特殊的`__bool__`方法来覆盖此行为。如果对象定义了`__bool__`方法，则Python调用该方法以确定其真值。**==

As an example, suppose we want a bank account with 0 balance to be false. We can add a `__bool__` method to the `Account` class to create this behavior.例如，假设我们希望余额为0的银行账户为假。我们可以向“Account”类添加一个“__bool__”方法来创建此行为。

```py
>>> Account.__bool__ = lambda self: self.balance != 0
```

We can call the `bool` constructor to see the truth value of an object, and we can use any object in a boolean context.我们可以调用“bool”构造函数来查看对象的真值，并且可以在布尔上下文中使用任何对象。

```py
>>> bool(Account('Jack'))
False
>>> if not Account('Jack'):
        print('Jack has nothing')
Jack has nothing
```

### ==`__len__`==

**Sequence operations.** We have seen that we can call the `len` function to determine the length of a sequence.==**序列操作。**==我们已经看到，我们可以调用“len”函数来确定序列的长度。

```py
>>> len('Go Bears!')
9
```

The `len` function invokes the `__len__` method of its argument to determine its length. All built-in sequence types implement this method.“len”函数调用其参数的“__len__”方法来确定其长度。==**所有内置序列类型都实现此方法。**==

```py
>>> 'Go Bears!'.__len__()
9
```

Python uses a sequence's length to determine its truth value, if it does not provide a `__bool__` method. Empty sequences are false, while non-empty sequences are true.**==Python使用序列的长度来确定其真值，如果它不提供'bool'方法。空序列为假，而非空序列为真。==**

```py
>>> bool('')
False
>>> bool([])
False
>>> bool('Go Bears!')
True
```

### ==**`__getitem__`**==

The `__getitem__` method is invoked by the element selection operator, but it can also be invoked directly.

**==`__getitem__`方法由元素选择运算符调用，但也可以直接调用。==**

```py
>>> 'Go Bears!'[3]
'B'
>>> 'Go Bears!'.__getitem__(3)
'B'
```

### ==**`__call__`**==

**Callable objects.** In Python, functions are first-class objects, so they can be passed around as data and have attributes like any other object. Python also allows us to define objects that can be "called" like functions by including a `__call__` method. With this method, we can define a class that behaves like a higher-order function.**可调用对象。**==在Python中,**函数是一级对象，因此它们可以作为数据传递，并具有与其他对象相同的属性**==.Python还允许我们通过包含`__call__`方法来定义可以像函数一样“调用”的对象。使用此方法，**我们可以定义一个行为类似于高阶函数的类。**

As an example, consider the following higher-order function, which returns a function that adds a constant value to its argument.例如，考虑下面的高阶函数，它返回一个将常量值添加到其参数的函数。

```py
>>> def make_adder(n):
        def adder(k):
            return n + k
        return adder
>>> add_three = make_adder(3)
>>> add_three(4)
7
```

We can create an `Adder` class that defines a `__call__` method to provide the same functionality.我们可以创建一个“Adder”类，该类定义一个“__call__”方法以提供相同的功能。

```py
>>> class Adder(object):
        def __init__(self, n):
            self.n = n
        def __call__(self, k):
            return self.n + k
>>> add_three_obj = Adder(3)
>>> add_three_obj(4)
7
```

Here, the `Adder` class behaves like the `make_adder` higher-order function, and the `add_three_obj` object behaves like the `add_three` function. We have further blurred the line between data and functions.这里，“Adder”类的行为类似于“make_Adder”高阶函数，而“add_three_obj”对象的行为类似“add_tthree”函数。**==我们进一步模糊了数据和函数之间的界限!!!!!!!!!==**

> 操作数重载见 2.7.3->特殊方法

## 2.7.3  Multiple Representations 多态表达

使用对象或函数的数据抽象是用于管理复杂性的强大工具。抽象数据类型允许我们在数据表示和用于操作数据的函数之间构造界限。但是，在大型程序中，对于程序中的某种数据类型，提及“底层表示”可能不总是有意义。首先，一个数据对象可能有多种实用的表示，而且我们可能希望设计能够处理多重表示的系统。

更重要的是，大型软件系统工程通常由许多人设计，并花费大量时间，需求的主题随时间而改变。在这样的环境中，每个人都事先同意数据表示的方案是不可能的。除了隔离使用和表示的数据抽象的界限，我们需要隔离不同设计方案的界限，以及允许不同方案在一个程序中共存。进一步，由于大型程序通常通过组合已存在的模块创建，这些模块会单独设计，**我们需要一种惯例，让程序员将模块递增地组合为大型系统。也就是说，不需要重复设计或实现这些模块。**

我们以最简单的复数示例开始。我们会看到，消息传递在维持“复数”对象的抽象概念时，如何让我们为复数的表示设计出分离的直角坐标和极坐标表示。我们会通过使用泛用选择器为复数定义算数函数（`add_complex`，`mul_complex`）来完成它。泛用选择器可访问复数的一部分，独立于数值表示的方式。所产生的复数系统包含两种不同类型的抽象界限。它们隔离了高阶操作和低阶表示。此外，也有一个垂直的界限，它使我们能够独立设计替代的表示。

所以，复数有两种不同表示，它们适用于不同的操作。然而，**==从一些人编写使用复数的程序的角度来看，数据抽象的原则表明，所有操作复数的运算都应该可用，无论计算机使用了哪个表示。==**

```py
>>> class Number:
        def __add__(self, other):
            return self.add(other)
        def __mul__(self, other):
            return self.mul(other)
```

### **接口(Interface)**

**接口** 消息传递并不仅仅提供用于组装行为和数据的方式。**它也允许不同的数据类型以不同方式响应相同消息。来自不同对象，产生相似行为的共享消息是抽象的有力手段。**

像之前看到的那样，抽象数据类型由构造器、选择器和额外的行为条件定义。与之紧密相关的概念是接口，它是共享消息的集合，带有它们含义的规定。**响应`__repr__`和`__str__`特殊方法的对象都实现了通用的接口，它们可以表示为字符串。**

在复数的例子中，接口需要实现由四个消息组成的算数运算：`real`，`imag`，`magnitude`和`angle`。我们可以使用这些消息实现加法和乘法。

我们拥有两种复数的抽象数据类型，它们的构造器不同。

+ `ComplexRI`从实部和虚部构造复数。
+ `ComplexMA`从模和角度构造复数。

使用这些消息和构造器，我们可以实现复数算数：

```py
>>> class Complex(Number):
        def add(self, other):
            return ComplexRI(self.real + other.real, self.imag + other.imag)
        def mul(self, other):
            magnitude = self.magnitude * other.magnitude
            return ComplexMA(magnitude, self.angle + other.angle)
```

术语“抽象数据类型”（ADT）和“接口”的关系是微妙的。ADT 包含构建复杂数据类的方式，以单元操作它们，并且可以选择它们的组件。在面向对象系统中，ADT 对应一个类，虽然我们已经看到对象系统并不需要实现 ADT。**==接口是一组与含义关联的消息，并且它可能包含选择器，也可能不包含。==**概念上，**==ADT 描述了一类东西的完整抽象表示，而接口规定了可能在许多东西之间共享的行为。==**

### 属性(property)

**属性（Property）。**我们希望交替使用复数的两种类型，但是对于每个数值来说，储存重复的信息比较浪费。我们希望储存实部-虚部的表示或模-角度的表示之一。(==**Property的英语意思是财产,性质**==)

==**Python 拥有一个简单的特性，用于从零个参数的函数凭空计算属性（Attribute）。`@property`装饰器(第一章学到了)允许函数不使用标准调用表达式语法来调用。**==根据实部和虚部的复数实现展示了这一点。

```py
>>> from math import atan2
>>> class ComplexRI(object):
        def __init__(self, real, imag):
            self.real = real
            self.imag = imag
        @property
        def magnitude(self):
            return (self.real ** 2 + self.imag ** 2) ** 0.5
        @property
        def angle(self):
            return atan2(self.imag, self.real)
        def __repr__(self):
            return 'ComplexRI({0}, {1})'.format(self.real, self.imag)
```

第二种使用模和角度的实现提供了相同接口，因为它响应同一组消息。

```py
>>> from math import sin, cos
>>> class ComplexMA(object):
        def __init__(self, magnitude, angle):
            self.magnitude = magnitude
            self.angle = angle
        @property
        def real(self):
            return self.magnitude * cos(self.angle)
        @property
        def imag(self):
            return self.magnitude * sin(self.angle)
        def __repr__(self):
            return 'ComplexMA({0}, {1})'.format(self.magnitude, self.angle)
```

实际上，我们的`add_complex`和`mul_complex`实现并没有完成；每个复数类可以用于任何算数函数的任何参数。对象系统不以任何方式显式连接（例如通过继承）这两种复数类型，这需要给个注解。**==我们已经通过在两个类之间共享一组通用的消息和接口，实现了复数抽象。==** 

> ==**理解接口的含义与作用**==

```py
>>> from math import pi
>>> add_complex(ComplexRI(1, 2), ComplexMA(2, pi/2))
ComplexRI(1.0000000000000002, 4.0)
>>> mul_complex(ComplexRI(0, 1), ComplexRI(0, 1))
ComplexMA(1.0, 3.141592653589793)
```

**==编码多种表示的接口拥有良好的特性。用于每个表示的类可以独立开发；它们只需要遵循它们所共享的属性名称。这个接口同时是递增的。如果另一个程序员希望向相同程序添加第三个复数表示，它们只需要使用相同属性创建另一个类。==**

### 特殊方法

>  **上面刚讲过了,再复习一下**

内建的算数运算符可以以一种和`repr`相同的方式扩展；它们是特殊的方法名称，对应 Python 的算数、逻辑和序列运算的运算符。

为了使我们的代码更加易读，我们可能希望在执行复数加法和乘法时直接使用`+`和`*`运算符。将下列方法添加到两个复数类中，**这会让这些运算符，以及`opertor`模块中的`add`和`mul`函数可用。**

```py
>>> ComplexRI.__add__ = lambda self, other: add_complex(self, other)
>>> ComplexMA.__add__ = lambda self, other: add_complex(self, other)
>>> ComplexRI.__mul__ = lambda self, other: mul_complex(self, other)
>>> ComplexMA.__mul__ = lambda self, other: mul_complex(self, other)
```

现在，我们可以对我们的自定义类使用中缀符号。

```py
>>> ComplexRI(1, 2) + ComplexMA(2, 0)
ComplexRI(3.0, 2.0)
>>> ComplexRI(0, 1) * ComplexRI(0, 1)
ComplexMA(1.0, 3.141592653589793)
```

**扩展阅读。**为了求解含有`+`运算符的表达式，Python 会检查表达式的左操作数和右操作数上的特殊方法。首先，**==Python 会检查左操作数的`__add__`方法，之后检查右操作数的`__radd__`方法。如果二者之一被发现，这个方法会以另一个操作数的值作为参数调用。==**

在 Python 中求解含有任何类型的运算符的表达值具有相似的协议，这包括**切片符号和布尔运算符**。Python 文档列出了完整的[运算符的方法名称](http://docs.python.org/py3k/reference/datamodel.html#special-method-names)。Dive into Python 3 的[特殊方法名称](http://diveintopython3.ep.io/special-method-names.html)一章描述了许多用于 Python 解释器的细节。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230108202610151.png" alt="image-20230108202610151" style="zoom:33%;" />

## 2.7.4  Generic Functions 泛型函数

我们的复数实现创建了两种数据类型，它们对于`add_complex`和`mul_complex`函数能够互相转换。现在我们要看看如何使用相同的概念，**==不仅仅定义不同表示上的泛用操作，也能用来定义不同种类、并且不共享通用结构的参数上的泛用操作。==**

我们到目前为止已定义的操作将不同的数据类型独立对待。所以，存在用于加法的独立的包，比如两个有理数或者两个复数。我们没有考虑到的是，定义类型界限之间的操作很有意义，比如将复数与有理数相加。我们经历了巨大的痛苦，引入了程序中各个部分的界限，便于让它们可被独立开发和理解。

**==我们希望以某种精确控制的方式引入跨类型的操作。便于在不严重违反抽象界限的情况下支持它们。==**在我们希望的结果之间可**能有些矛盾：我们希望能够将有理数与复数相加，也希望能够使用泛用的`add`函数，正确处理所有数值类型。同时，我们希望隔离复数和有理数的细节，来维持程序的模块化。**

让我们使用 Python 内建的对象系统重新编写有理数的实现。像之前一样，我们在较低层级将有理数储存为分子和分母。

```py
>>> from fractions import gcd
>>> class Rational(object):# 有理数
        def __init__(self, numer, denom):
            g = gcd(numer, denom) // 约分
            self.numer = numer // 分子
            self.denom = denom // 分母
        def __repr__(self):
            return 'Rational({0}, {1})'.format(self.numer, self.denom)
```

这个新的实现中的有理数的加法和乘法和之前类似。

```py
>>> def add_rational(x, y):
        nx, dx = x.numer, x.denom
        ny, dy = y.numer, y.denom
        return Rational(nx * dy + ny * dx, dx * dy)
>>> def mul_rational(x, y):
        return Rational(x.numer * y.numer, x.denom * y.denom)
```

### **Type dispatching **类型分发

**类型分发。**一种处理跨类型操作的方式是为每种可能的类型组合设计不同的函数，操作可用于这种类型。例如，我们可以扩展我们的复数实现，使其提供函数用于将复数与有理数相加。我们可以使用叫做类型分发的机制更通用地提供这个功能。

**==类型分发的概念是，编写一个函数，首先检测接受到的参数类型，之后执行适用于这种类型的代码。==Python 中，对象类型可以使用内建的`type`函数来检测。**

```py
>>> def iscomplex(z):
        return type(z) in (ComplexRI, ComplexMA)
>>> def isrational(z):
        return type(z) == Rational
```

这里，我们依赖一个事实，每个对象都知道自己的类型，并且我们可以使用Python 的`type`函数来获取类型。即使`type`函数不可用，我们也能根据`Rational`，`ComplexRI`和`ComplexMA`来实现`iscomplex`和`isrational`。

现在考虑下面的`add`实现，它显式检查了两个参数的类型。我们不会在这个例子中显式使用 Python 的特殊方法（例如`__add__`）。

```py
>>> def add_complex_and_rational(z, r):
            return ComplexRI(z.real + r.numer/r.denom, z.imag)
>>> def add(z1, z2):
        """Add z1 and z2, which may be complex or rational."""
        if iscomplex(z1) and iscomplex(z2):
            return add_complex(z1, z2)
        elif iscomplex(z1) and isrational(z2):
            return add_complex_and_rational(z1, z2)
        elif isrational(z1) and iscomplex(z2):
            return add_complex_and_rational(z2, z1)
        else:
            return add_rational(z1, z2)
```

这个简单的类型分发方式并不是递增的，它使用了大量的条件语句。*==如果另一个数值类型包含在程序中，我们需要使用新的语句重新实现`add`。==*

**==我们可以创建更灵活的`add`实现，通过以字典实现类型分发。==要想扩展`add`的灵活性，第一步是为我们的类创建一个`tag`集合，抽离两个复数集合的实现。**

```py
>>> def type_tag(x):
        return type_tag.tags[type(x)]
>>> type_tag.tags = {ComplexRI: 'com', ComplexMA: 'com', Rational: 'rat'}
```

下面，我们使用这些类型标签来索引字典，字典中储存了数值加法的不同方式。字典的键是类型标签的元素，值是类型特定的加法函数。

```py
>>> def add(z1, z2):
        types = (type_tag(z1), type_tag(z2))
        return add.implementations[types](z1, z2)
```

**`add`函数的定义本身没有任何功能；它完全地依赖于一个叫做`add.implementations`的字典去实现泛用加法。我们可以构建如下的字典。**

```py
>>> add.implementations = {}
>>> add.implementations[('com', 'com')] = add_complex
>>> add.implementations[('com', 'rat')] = add_complex_and_rational
>>> add.implementations[('rat', 'com')] = lambda x, y: add_complex_and_rational(y, x)
>>> add.implementations[('rat', 'rat')] = add_rational
```

**这个基于字典的分发方式是递增的，因为`add.implementations`和`type_tag.tags`总是可以扩展。任何新的数值类型可以将自己“安装”到现存的系统中，通过向这些字典添加新的条目。**

当我们向系统引入一些复杂性时，我们现在拥有了**泛用、可扩展的`add`函数**，可以处理混合类型。

```py
>>> add(ComplexRI(1.5, 0), Rational(3, 2))
ComplexRI(3.0, 0)
>>> add(Rational(5, 3), Rational(1, 2))
Rational(13, 6)
```

### Coercion 强制转换

**强制转换。**在完全不相关的类型执行完全不相关的操作的一般情况中，实现显式的跨类型操作，尽管可能非常麻烦，是人们所希望的最佳方案。幸运的是，我们有时可以通过利用类型系统中隐藏的额外结构来做得更好。不同的数据类通常并不是完全独立的，可能有一些方式，一个类型的对象通过它会被看做另一种类型的对象。这个过程叫做强制转换。例如，如果我们被要求将一个有理数和一个复数通过算数来组合，我们可以将有理数看做虚部为零的复数。通过这样做，我们将问题转换为两个复数组合的问题，这可以通过`add_complex`和`mul_complex`由经典的方法处理。

通常，我们可以通过设计强制转换函数来实现这个想法。强制转换函数将一个类型的对象转换为另一个类型的等价对象。这里是一个典型的强制转换函数，它将有理数转换为虚部为零的复数。

```py
>>> def rational_to_complex(x):
        return ComplexRI(x.numer/x.denom, 0)
```

现在，我们可以定义强制转换函数的字典。这个字典可以在更多的数值类型引入时扩展。

```py
>>> coercions = {('rat', 'com'): rational_to_complex}
```

任意类型的数据对象不可能转换为每个其它类型的对象。例如，没有办法将任意的复数强制转换为有理数，所以在`coercions`字典中应该没有这种转换的实现。

使用`coercions`字典，我们可以编写叫做`coerce_apply`的函数，它试图将参数强制转换为相同类型的值，之后仅仅调用运算符。`coerce_apply `的实现字典不包含任何跨类型运算符的实现。

```py
>>> def coerce_apply(operator_name, x, y):
        tx, ty = type_tag(x), type_tag(y)
        if tx != ty:
            if (tx, ty) in coercions:
                tx, x = ty, coercions[(tx, ty)](x)
            elif (ty, tx) in coercions:
                ty, y = tx, coercions[(ty, tx)](y)
            else:
                return 'No coercion possible.'
        key = (operator_name, tx)
        return coerce_apply.implementations[key](x, y)
```

`coerce_apply`的`implementations`仅仅需要一个类型标签，因为它们假设两个值都共享相同的类型标签。所以，我们仅仅需要四个实现来支持复数和有理数上的泛用算数。

```py
>>> coerce_apply.implementations = {('mul', 'com'): mul_complex,
                                    ('mul', 'rat'): mul_rational,
                                    ('add', 'com'): add_complex,
                                    ('add', 'rat'): add_rational}
```

就地使用这些实现，`coerce_apply `可以代替`apply`。

```py
>>> coerce_apply('add', ComplexRI(1.5, 0), Rational(3, 2))
ComplexRI(3.0, 0)
>>> coerce_apply('mul', Rational(1, 2), ComplexMA(10, 1))
ComplexMA(5.0, 1.0)
```

**这个强制转换的模式比起显式定义跨类型运算符的方式具有优势。虽然我们仍然需要编程强制转换函数来关联类型，我们仅仅需要为每对类型编写一个函数，而不是为每个类型组合和每个泛用方法编写不同的函数。我们所期望的是，类型间的合理转换仅仅依赖于类型本身，而不是要调用的特定操作。**

强制转换的扩展会带来进一步的优势。一些更复杂的强制转换模式并不仅仅试图将一个类型强制转换为另一个，而是将两个不同类型强制转换为第三个。想一想菱形和长方形：每个都不是另一个的特例，但是两个都可以看做平行四边形。另一个强制转换的扩展是迭代的强制转换，其中一个数据类型通过媒介类型被强制转换为另一种。一个整数可以转换为一个实数，通过首先转换为有理数，接着将有理数转换为实数。这种方式的链式强制转换降低了程序所需的转换函数总数。

虽然它具有优势，强制转换也有潜在的缺陷。例如，**强制转换函数在调用时会丢失信息**。在我们的例子中，有理数是精确表示，但是当它们转换为复数时会变得近似。

一些编程语言拥有内建的强制转换函数。实际上，**Python 的早期版本拥有对象上的`__coerce__`特殊方法。最后，内建强制转换系统的复杂性并不能支持它的使用，所以被移除了。**反之，特定的操作按需强制转换它们的参数。运算符被实现为用户定义类上的特殊方法，比如`__add__`和`__mul__`。这完全取决于你，取决于用户来决定是否使用类型分发，数据导向编程，消息传递，或者强制转换来在你的程序中实现泛用函数。

**==关于Tree和Linked List的实现见相关作业代码==**



## 2.8  Efficiency

时间复杂度,空间复杂度

### 2.8.1  Measuring Efficiency

A more reliable way to characterize the efficiency of a program is to measure how many times some event occurs, such as a function call.描述程序效率的一种更可靠的方法是**测量某个事件发生的次数**，例如函数调用。

原有的fib是低效的,因为他重复计算了大量的数值

我们可以用cache(缓存)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110105425093.png" alt="image-20230110105425093" style="zoom:33%;" /><img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110105438258.png" alt="image-20230110105438258" style="zoom:33%;" />

````py
def fib(n):
    """The nth Fibonacci number.

    >>> fib(20)
    6765
    """
    if n == 0 or n == 1:
        return n
    else:
        return fib(n-2) + fib(n-1)

# Time

def count(f): #用count作为函数装饰器,装饰fib(n)
    # fib = count(fib)
    
    """Return a counted version of f with a call_count attribute.
    >>> fib(20)
    6765
    >>> fib.call_count
    21891
    """
    def counted(*args):
        counted.call_count += 1 # python自动为我们创建了function类
        return f(*args)
    counted.call_count = 0 #不一定要放到counted前面,因为此时还没有被调用(装饰器特殊)
    return counted

# Memoization

def memo(f): # 另一个函数装饰器
    """Memoize f.

    >>> def fib(n):
    ...     if n == 0 or n == 1:
    ...         return n
    ...     else:
    ...         return fib(n-2) + fib(n-1)
    >>> fib = count(fib)
    >>> fib(20)
    6765
    >>> fib.call_count
    21891
    >>> counted_fib = count(fib)
    >>> fib  = memo(counted_fib) #相当于二重装饰器!!!!!!!!!!!!!!!!!!!!!!!!
    >>> fib(20)
    6765
    >>> counted_fib.call_count
    21
    >>> fib(35)
    9227465
    >>> counted_fib.call_count
    36
    """
    cache = {}
    def memoized(n):
        if n not in cache: # 这里的in是指是不是在字典的key列表中!!!!!!!!!!
            cache[n] = f(n)
        return cache[n]
    return memoized
````

1.the fib(n-2) points to the new fib (counted).  The name f has been bound to the original fib function, while the name fib has been bound to counted, “fib=count(fib)”. 

2.So, "return f(*args)" calls original fib funcion, and the original fib function calls fib(n-1) and fib(n-2), but the name fib no longer bound to the original fib function, then we are actually calling counted(n-1) and counted(n-2).  So, we have to use “fib=count(fib)”. If we use some thing like “countfib=count(fib)”, we will find no matter how many times the original fib function has been calld, countfib.call_count will always be 1  

3.**Counted is a function, also an instance of class ‘function’. The class ‘function’ has been defined by python already.**

==疑问:是不是装饰器特有?==

> 注意:memohe count可以一起用,是二重装饰器

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110114609084.png" alt="image-20230110114609084" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110114850681.png" alt="image-20230110114850681" style="zoom:33%;" />

> O是上线,Θ是既是上限,又是下限
>
> **n不一定是数字,还可能是列表长度等其他表示数据量的**

### 空间复杂度

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110115506894.png" alt="image-20230110115506894" style="zoom:33%;" />

**==当一个帧是"active frame"时,占用空间==**

**==active frame是还没有return的帧==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110115632741.png" alt="image-20230110115632741" style="zoom: 33%;" />



# 补充

## F-string

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230107134943071.png" alt="image-20230107134943071" style="zoom: 33%;" />

大括号内可以有表达式 

`f'a + {3 + 2 + 1}''`实际为`'a + 6'`

## Decomposition 分解(模块化编程)

**Modular design**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110121650534.png" alt="image-20230110121650534" style="zoom:33%;" />

例子:

```py
def search(query, ranking=lambda r: -r.stars):
    results = [r for r in Restaurant.all if query in r.name]
    return sorted(results, key=ranking)

def reviewed_both(restaurant, other):
    return fast_overlap(restaurant.reviewers, other.reviewers)
    return len([r for r in restaurant.reviewers if r in other.reviewers]) #这一行效率低下,因为`in`是线性的,所以整个是平方级别的,用下面这个方法就是线性级别的

def fast_overlap(s, t):
    """Return the overlap between sorted S and sorted T.

    >>> fast_overlap([2, 3, 5, 6, 7], [1, 4, 5, 6, 7, 8])
    3
    """
    count, i, j = 0, 0, 0
    while i < len(s) and j < len(t):
        if s[i] == t[j]:
            count, i, j = count + 1, i + 1, j + 1
        elif s[i] < t[j]:
            i += 1
        else:
            j += 1
    return count

class Restaurant:
    """A restaurant."""
    all = [] #类的变量
    def __init__(self, name, stars, reviewers):
        self.name = name
        self.stars = stars
        self.reviewers = sorted(reviewers) # reviewers
        Restaurant.all.append(self)

    def similar(self, k, similarity=reviewed_both):
        "Return the K most similar restaurants to SELF, using SIMILARITY for comparison."
        others = list(Restaurant.all)
        others.remove(self) # similar里面不能有自己本身
        return sorted(others, key=lambda r: -similarity(self, r))[:k] # 返回最相似的k条

    def __repr__(self):
        return '<' + self.name + '>'
    
import json

reviewers_by_restaurant = {}
for line in open('reviews.json'):
    r = json.loads(line)
    business_id = r['business_id']
    if business_id not in reviewers_by_restaurant:
        reviewers_by_restaurant[business_id] = []
    reviewers_by_restaurant[business_id].append(r['user_id'])


for line in open('restaurants.json'):
    b = json.loads(line)
    reviewers = reviewers_by_restaurant.get(b['business_id'], [])
    Restaurant(b['name'], b['stars'], reviewers)


results = search('Thai') # 先写的这个部分
for r in results:
    print(r.name, 'is similar to', r.similar(3))
```

## Data Examples

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110151030084.png" alt="image-20230110151030084" style="zoom:80%;" />

**==list和切片的效果一样,都是产生新的list==**

**==注意slice assignment!!!!!!!!!!!!!!!!!!!!!!!==**

**==但是t也必须是一个可迭代的值==**

```py
s[0:0] = t 表示在0的位置上插入t整个列表,s的原先的element往后移动
```

## OOP

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110154426132.png" alt="image-20230110154426132" style="zoom:50%;" />

## 内置函数的使用和理解

> min,max,abs,**==filter,map, any,all==**

```py
# Built-in functions & comprehensions

def min_abs_indices(s):
    """Indices of all elements in list s that have the smallest absolute value.

    >>> min_abs_indices([-4, -3, -2, 3, 2, 4])
    [2, 4]
    >>> min_abs_indices([1, 2, 3, 4, 5])
    [0]
    """
    min_abs = min(map(abs, s))
    return list(filter(lambda i: abs(s[i]) == min_abs, range(len(s))))
    # OR
    return [i for i in range(len(s)) if abs(s[i]) == min_abs]

def largest_adj_sum(s):
    """Largest sum of two adjacent elements in a list s.

    >>> largest_adj_sum([-4, -3, -2, 3, 2, 4])
    6
    >>> largest_adj_sum([-4, 3, -2, -3, 2, -4])
    1
    """
    return max([x + y for x, y in zip(s[:-1], s[1:])])
    # OR
    return max([s[i] + s[i + 1] for i in range(len(s) - 1)])
    # OR
    return max(map(lambda i: s[i] + s[i + 1], range(len(s) - 1)))

def digit_dict(s):
    """Map each digit d to the lists of elements in s that end with d.

    >>> digit_dict([5, 8, 13, 21, 34, 55, 89])
    {1: [21], 3: [13], 4: [34], 5: [5, 55], 8: [8], 9: [89]}
    """
    return {i: [x for x in s if x % 10 == i] for i in range(10) if any([x % 10 == i for x in s])}
    # OR
    last_digits = list(map(lambda x: x % 10, s))
    return {i: [x for x in s if x % 10 == i] for i in range(10) if i in last_digits}

def all_have_an_equal(s):
    """Does every element equal some other element in s?

    >>> all_have_an_equal([-4, -3, -2, 3, 2, 4])
    False
    >>> all_have_an_equal([4, 3, 2, 3, 2, 4])
    True
    """
    return min([sum([1 for y in s if x == y]) for x in s]) > 1
    # OR
    return all([s[i] in s[:i] + s[i+1:] for i in range(len(s))])
    # OR
    return all(map(lambda x: s.count(x) > 1, s))
```

###### 
