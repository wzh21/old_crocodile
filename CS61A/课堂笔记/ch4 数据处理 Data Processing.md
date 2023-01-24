

# 4.1  Introduction

To process large data sets efficiently, programs are organized into **pipelines of manipulations on sequential streams of data.** In this chapter, we consider a suite of techniques process and manipulate sequential data streams efficiently.为了有效地处理大数据集**，程序被组织成对连续的数据流进行操作的管道**。在本章中，我们考虑了一套有效处理和操作顺序数据流的技术。

# 4.2  Implicit Sequences 隐式序列

A sequence can be represented without each element being stored explicitly in the memory of the computer. That is, we can construct an object that provides access to all of the elements of some sequential dataset without computing the value of each element in advance. Instead, we compute elements on demand.一个序列可以被表示，而不需要将每个元素明确地存储在计算机的内存中。也就是说，我们可以构造一个对象，提供对一些序列数据集的所有元素的访问，而**不用事先计算每个元素的值。相反，我们按需计算元素。**

这个想法的一个例子出现在第二章中介绍的范围容器类型中。一个范围代表一个连续的、有边界的整数序列。然而，该序列的每个元素并不是在内存中明确表示的。相反，当一个元素从一个范围中被请求时，它被计算出来。因此，**我们可以在不使用大块内存的情况下表示非常大的整数范围。只有范围的端点被存储为范围对象的一部分。**

```py
>>> r = range(10000, 1000000000)
>>> r[45006230]
45016230
```

在这个例子中，当构建 range 实例时，这个范围中的所有 999,990,000 个整数并没有被存储。相反，range对象将第一个元素10,000添加到索引45,006,230中，产生元素45,016,230。**==按需计算数值，而不是从现有的表示中检索它们，是惰性计算的一个例子。==**

**==In computer science, *lazy computation* describes any program that delays the computation of a value until that value is needed.在计算机科学中，惰性计算描述了任何将一个值的计算推迟到需要该值时进行的程序.==**



## 4.2.1  Iterators 迭代器

1.Python 和许多其他编程语言提供了一种统一的方法来顺序处理一个容器值的元素，称为迭代器。**==迭代器是一个对象==**，它提供对值的顺序访问，一个接一个。

迭代器的抽象有两个组成部分:

+ **检索正在处理的序列中的下一个元素的机制**
+ **标志着序列的终点已经到达，没有其他元素了的机制。**

```py
>>> primes = [2, 3, 5, 7]
>>> type(primes)
>>> iterator = iter(primes)
>>> type(iterator)
>>> next(iterator)
2
>>> next(iterator)
3
>>> next(iterator)
5
```

2.Python发出没有更多可用值的信号的方式是在调用next时引发==**StopIteration异常**==。可以使用try语句处理此异常。

```py
>>> next(iterator)
7
>>> next(iterator)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>> try:
        next(iterator)
    except StopIteration:
        print('No more values')
No more values
```

3.An iterator maintains local state to represent its position in a sequence. Each time `next` is called, that position advances. Two separate iterators can track two different positions in the same sequence. However, two names for the same iterator will share a position, because they share the same value.迭代器维护本地状态以表示其在序列中的位置。每次调用next时，该位置都会前进。**两个独立的迭代器可以跟踪同一序列中的两个不同位置。但是，同一迭代器的两个名称将共享一个位置，因为它们共享相同的值。**Calling `iter` on an iterator will return that iterator, not a copy. This behavior is included in Python so that a programmer can call `iter` on a value to get an iterator without having to worry about whether it is an iterator or a container**.在迭代器上调用“iter”将返回该迭代器，而不是副本。**Python中包含了这种行为，这样程序员就可以对值调用“iter”来获取迭代器，而不必担心它是迭代器还是容器。

4.The usefulness of iterators is derived from the fact that the underlying series of data for an iterator may not be represented explicitly in memory. An iterator provides a mechanism for considering each of a series of values in turn, but all of those elements do not need to be stored simultaneously. Instead, when the next element is requested from an iterator, that element may be computed on demand instead of being retrieved from an existing memory source.**==迭代器的用处来自这样一个事实：迭代器的底层数据系列可能不会在内存中明确表示。迭代器提供了一个依次考虑一系列数值的机制，但所有这些元素不需要同时存储。相反，当从一个迭代器中请求下一个元素时，该元素可以按需计算，而不是从现有的内存源中检索出来。==**



## 4.2.2  Iterables 可迭代值

1.**任何可以产生迭代器的值都被称为可迭代值。**在 Python 中，一个可迭代的值是任何可以传递给内置 iter 函数的东西。可迭代值包括序列值(sequence)，如字符串(string)和图元(tuple)，以及其他容器，如集合(set)和字典(dictionary)。**==迭代器也是可迭代的，因为它们可以被传递给iter函数。==**

**==iter作用于迭代器,返回迭代器本身,但是是浅拷贝,不连接在一个对象上==**

***之所以这样,是为了接受的范围更广泛***

2.**即使是像字典这样的无序集合，在产生迭代器时也必须对其内容定义一个排序。**字典和集合是无序的，因为程序员不能控制迭代的顺序，但是 Python 在其规范中保证了关于它们的顺序的某些属性。

==**在历史中,字典是无序的,在python3.6以后, 是有序的**==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230102154148271.png" alt="image-20230102154148271" style="zoom:67%;" />

```py
>>> d = {'one': 1, 'two': 2, 'three': 3}
>>> d
{'one': 1, 'three': 3, 'two': 2}
>>> k = iter(d)
>>> next(k)
'one'
>>> next(k)
'three'

>>> v = iter(d.values())
>>> next(v)
1
>>> next(v)
3

>>> p = iter(d.items())
>>> next(p)
('one', 1)
```

==**关于key,value,item(上面的例子)**==

If a dictionary changes in structure because a key is added or removed, then all iterators become invalid and future iterators may exhibit arbitrary changes to the order their contents. On the other hand, changing the value of an existing key does not change the order of the contents or invalidate iterators.**==如果字典的结构因添加或删除键而发生变化==**，那么所有迭代器都将无效，未来的迭代器可能会对其内容顺序进行任意更改。另一方面，==**更改现有键的值不会更改内容的顺序或使迭代器无效。**==

```py
>>> d.pop('two')
2
>>> next(k)
       
RuntimeError: dictionary changed size during iteration
Traceback (most recent call last):
```

## 4.2.3  Built-in Iterators

Several built-in functions take as arguments iterable values and return iterators. These functions are used extensively for **lazy sequence processing**.几个内置函数将可迭代值作为参数，并返回迭代器。这些函数广泛用于**延迟序列处理。**

**“map”函数是惰性的**:调用它不会执行计算结果元素所需的计算。相反，创建了一个迭代器对象，如果使用“next”进行查询，它可以返回结果。我们可以在下面的示例中观察到这一事实，其中对“print”的调用被延迟，直到从“doubled”迭代器请求相应的元素。

```py
>>> def double_and_print(x):
        print('***', x, '=>', 2*x, '***')
        return 2*x
>>> s = range(3, 7)
>>> doubled = map(double_and_print, s)  # double_and_print not yet called
>>> next(doubled)                       # double_and_print called once
*** 3 => 6 ***
6
>>> next(doubled)                       # double_and_print called again
*** 4 => 8 ***
8
>>> list(doubled)                       # double_and_print called twice more
*** 5 => 10 ***
*** 6 => 12 ***
[10, 12]
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230102163124954.png" alt="image-20230102163124954" style="zoom:80%;" />

### zip

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230102163816486.png" alt="image-20230102163816486" style="zoom:80%;" />

```py
>>> t = [1, 2, 3, 2, 1]
>>> reversed(t) == t
False
>>> list(reversed(t)) == t
True
```

**==reverse()返回的是迭代器而不是list,所以不等于t==**

```py
def palindrome(s):
    """Return whether s is the same sequence backward and forward.

    >>> palindrome([3, 1, 4, 1, 5])
    False
    >>> palindrome([3, 1, 4, 1, 3])
    True
    >>> palindrome('seveneves')
    True
    >>> palindrome('seven eves')
    False
    """
    # return s == reversed(s)  # This version doesn't work 
    return all([a == b for a, b in zip(s, reversed(s))])
    return list(s) == list(reversed(s))

```



## 4.2.4  For Statements	for语句

The `for` statement in Python operates on iterators. Objects are *iterable* (an interface) if they have an `__iter__` method that returns an *iterator*. Iterable objects can be the value of the `<expression>` in the header of a `for` statement:

Python中的“for”语句对迭代器进行操作。**==如果对象具有返回*迭代器*的`__iter__`方法，则对象是*可迭代* 的（接口）==**。可迭代对象可以是“for”语句头中“＜expression＞”的值：

```py
for <name> in <expression>:
    <suite>
```

To execute a `for` statement, Python evaluates the header `<expression>`, which must yield an iterable value. Then, the `__iter__` method is invoked on that value. Until a `StopIteration` exception is raised, Python repeatedly invokes the `__next__` method on that iterator and binds the result to the `<name>` in the `for` statement. Then, it executes the `<suite>`.要执行“for”语句，Python计算头“＜expression＞”，它必须产生一个可迭代的值。然后，==对该值调用“\_\_\_iter\_\_\_”方法。==在引发“StopIteration”异常之前，Python会反复调用该迭代器上的“__next_'”方法，并将结果绑定到“for”语句中的“\<name>”。然后，它执行“＜suite＞”

```py
>>> counts = [1, 2, 3]
>>> for item in counts:
        print(item)
1
2
3
```

Above, the iterator returned by invoking the `__iter__` method of `counts` is bound to a name `items` so that it can be queried for each element in turn. The handling clause for the `StopIteration` exception does nothing, but handling the exception provides a control mechanism for exiting the `while` loop.在上面，**通过调用“count”的`__iter___`方法返回的迭代器绑定到一个名称“items”，以便可以依次查询每个元素。****“StopIteration”异常的handling子句不执行任何操作，但处理该异常提供了退出“while”循环的控制机制。**

To use an iterator in a for loop, the iterator must also have an `__iter__` method. The Iterator types <http://docs.python.org/3/library/stdtypes.html#iterator-types> `_ section of the Python docs suggest that an iterator have an ``__iter__` method that returns the iterator itself, so that all iterators are iterable**要在for循环中使用迭代器，迭代器还必须有`__iter___`方法。迭代器类型<http://docs.python.org/3/library/stdtypes.html#iterator-Python文档的types>_部分建议==迭代器具有返回迭代器本身的`__iter_`方法==，==因此所有迭代器都是可迭代的==**

### Reasons for Using Iterators

![image-20230102164821569](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230102164821569.png)

==**使用迭代器可以让我们对数据本身的假设很少**==

## 4.2.5  Generators and Yield Statements 生成器和`yield`语句

对于复杂序列，`__next__()`很难在计算中节省空间。生成器允许我们通过利用 Python 解释器的特性定义更复杂的迭代。

A *generator* is an iterator returned by a special class of function called a *generator function*. Generator functions are distinguished from regular functions in that rather than containing `return` statements in their body, they use `yield` statement to return elements of a series**.生成器是由一类特殊函数，叫做生成器函数返回的迭代器**。生成器函数不同于普通的函数，因为它不在函数体中包含`return`语句，**而是使用`yield`语句来返回序列中的元素。**

```py
>>> def letters_generator():
        current = 'a'
        while current <= 'd':
            yield current
            current = chr(ord(current)+1)
>>> for letter in letters_generator(): 
    # 此处可以用for statement 的原因是,这个函数有yield语句,所以是可迭代的(是生成器函数),而且yield自己返回值
        print(letter)
a
b
c
d
```

即使我们永不显式定义`__iter__()`或`__next__()`方法，Python 会理解当我们使用`yield`语句时，我们打算定义生成器函数。调用时，**生成器函数并不返回特定的产出值，而是返回一个生成器（一种迭代器），它自己就可以返回产出的值。生成器对象拥有`__iter__`和`__next__`方法，每个对`__next__`的调用都会从上次停留的地方继续执行生成器函数，==直到另一个`yield`语句执行的地方。==**

> yield英文:产出

==**用yield语句而不是return语句返回, 可以多次返回**==

`__next__`第一次调用时，程序从`letters_generator`的函数体一直执行到进入`yield`语句。之后，它暂停并返回`current`值。`yield`语句并不破坏新创建的环境，而是为之后的使用保留了它。当`__next__`再次调用时，执行在它停留的地方恢复。`letters_generator`作用域中`current`和任何所绑定名称的值都会在随后的`__next__`调用中保留。

我们可以通过手动调用`__next__()`来遍历生成器：

```py
>>> letters = letters_generator()
>>> type(letters)
<class 'generator'>
>>> letters.__next__()
'a'
>>> letters.__next__()
'b'
>>> letters.__next__()
'c'
>>> letters.__next__()
'd'
>>> letters.__next__()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

**在第一次`__next__()`调用之前，生成器并不会开始执行任何生成器函数体中的语句。**

### ==yield from==

**==yield from一个可迭代的对象==**

**==yield from iterable object 等价于 for item in iterable object: yield item==**

```py
yield from iterable object ===
for item in iterable object:
    yield item
```



<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230104190157462.png" alt="image-20230104190157462" style="zoom:80%;" />

**类似于递归**

使用`yield from`可以让我们的程序更加易读,简洁

```py
def count_partitions(n, m):
    """Count partitions.
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
# 改写成下面这种 (用exact match)
def count_partitions(n, m):
    """Count partitions.
    >>> count_partitions(6, 4)
    9
    """
    if n <= 0:
        return 0
    elif m == 0:
        return 0
    else:
        exact_match = 0
        if n == m:
            exact_match = 1
        with_m = count_partitions(n-m, m) 
        without_m = count_partitions(n, m-1)
        return exact_match + with_m + without_m
```

```py
def partitions(n, m):
    """List partitions.
    >>> for p in partitions(6, 4): print(p)
    """
    if n <= 0:
        return []
    elif m == 0:
        return []
    else:
        exact_match = []
        if n == m:
            exact_match = [str(m)]
        with_m = [p + ' + ' + str(m) for p in partitions(n-m, m)]
        # 递归函数的返回值作为可迭代的对象
        without_m = partitions(n, m-1)
        return exact_match + with_m + without_m

def yield_partitions(n, m):
    """List partitions.

    >>> for p in yield_partitions(6, 4): print(p)
    2 + 4
    1 + 1 + 4
    3 + 3
    1 + 2 + 3
    1 + 1 + 1 + 3
    2 + 2 + 2
    1 + 1 + 2 + 2
    1 + 1 + 1 + 1 + 2
    1 + 1 + 1 + 1 + 1 + 1
    """
    if n > 0 and m > 0:
        if n == m:
            yield str(m)
        for p in yield_partitions(n-m, m):# yield_p(2,4)->yield_p(2,3)->yield_p(2,2)->str(2)
            # ->yiled(2,1)
            yield p + ' + ' + str(m)
        yield from yield_partitions(n, m-1)
```

==**yield : contribute a element**==

**==yiled from: contribute a whole group of elements to the output==**  



可以理解为 **==递归+找到下一个可以返回的yield语句!!!!!!!!!!!!!!!!!!!!!!!!!==**

## 4.2.6  Iterable Interface

之后略,不是CS61A重点了
