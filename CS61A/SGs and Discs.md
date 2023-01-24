# SG1 Functions and control

1.![image-20221223203555436](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223203555436.png)

==print 不会显示引号,但是return会显示引号==

2.优先级顺序 not > or = and

## ==**error 的类型**==

| Error Types                 | Descriptions                                                 |
| :-------------------------- | :----------------------------------------------------------- |
| SyntaxError(语法错误)       | Contained improper syntax (e.g. missing a colon after an `if` statement or forgetting to close parentheses/quotes) |
| IndentationError(缩进错误)  | Contained improper indentation (e.g. inconsistent indentation of a function body) |
| TypeError(类型错误)         | Attempted operation on incompatible types (e.g. trying to add a function and a number) or called function with the wrong number of arguments |
| ZeroDivisionError(除零错误) | Attempted division by zero                                   |

# SG2 环境图的绘制

## Q1

```py
def f(x):
    return lambda y: x(y)

def g(x):
    return lambda : f(x) + f(y)

y = 2
result = f(g(f))
```



<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227225349005.png" alt="image-20221227225349005" style="zoom:33%;" />

`f(g(f))` 先绘制出g的帧,而不是f的,==**因为要从内层向外调用**==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227225538369.png" alt="image-20221227225538369" style="zoom:50%;" />

## Q2

```py
def always_roll(n):
    return lambda s0, s1: n

def make_bad_strategy(p):
    def strategy(s0, s1):
        # next line is bad style!
        return always_roll(1 - p)(s0, s1)
    return strategy

num_rolls = make_bad_strategy(1)(50, 50)
```



<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227230017653.png" alt="image-20221227230017653" style="zoom: 33%;" />

Q3-Q6省略,链接:[Higher-Order Functions: exam (albertwu.org)](http://albertwu.org/cs61a/review/hof/exam.html)



# Discussion 6: Mutability, Iterators, Generators

## Mutability

```py
>>> x = [1, 2, 3]
>>> y = x
>>> x += [4]
>>> x
[1, 2, 3, 4]
>>> y
[1, 2, 3, 4]
```

```py
>>> x = [1, 2, 3]
>>> y = x
>>> x = x + [4]
>>> x
[1, 2, 3, 4]
>>> y
[1, 2, 3]
```

**==`+=`是在同一个对象上加, `=`是赋值,是另一个对象了==**

**==`[:]`是浅拷贝,不是同一个对象==**

```py
>>> s1[:3] is s2[:3]
False
>>> s1[:3] == s2[:3]
True
```

**==切片产生的是一个新的对象==**

## Iterators

```py
>>> s = "cs61a"
>>> s_iter = iter(s)

>>> next(s_iter)
'c'

>>> next(s_iter)
's'

>>> list(s_iter)
[‘6’, ‘1’, ‘a’] #剩下的生成下一个
#"输出这个,并指向下一个"
```

"**==输出这个,并指向下一个==**"

```py
>>> s = [[1, 2, 3, 4]]
>>> i = iter(s)
>>> j = iter(next(i))
>>> next(j)
1
```

**==两层迭代器==**

```py
>>> s.append(5)
>>> next(i)

5
```

**==对列表list来说, append元素后,迭代器仍然有效!!!!!!!!!!!!!!!!!!!(无效的是字典)==**

## Generators

### Q4: WWPD: Generators

```py
>>> def infinite_generator(n):
...     yield n
...     while True:
...     	n += 1
...     	yield n

>>> next(infinite_generator)
TypeError
```

因为

```py
>>> infinite_generator
<function infinite_generator at 0x_______>
>>> infinite_generator(2)
<generator object infinite_generator at 0x______>
```



```py
>>> gen_obj = infinite_generator(1)
>>> next(gen_obj)
1
>>> next(gen_obj)
2
>>> list(gen_obj)
Infinite Loop # 无限循环
```

### Q5: Filter-Iter 过滤器的生成器

Implement a generator function called `filter_iter(iterable, f)` that only yields elements of `iterable` for which `f` returns True.

```py
def filter_iter(iterable, f):
    """
    >>> is_even = lambda x: x % 2 == 0
    >>> list(filter_iter(range(5), is_even)) # a list of the values yielded from the call to filter_iter
    [0, 2, 4]
    >>> all_odd = (2*y-1 for y in range(5))
    >>> list(filter_iter(all_odd, is_even))
    []
    >>> naturals = (n for n in range(1, 100))
    >>> s = filter_iter(naturals, is_even)
    >>> next(s)
    2
    >>> next(s)
    4
    """
    "*** YOUR CODE HERE ***"
    for elem in iterable:
        if f(elem):
            yield elem
# Alternate solution (only works if iterable is not an infinite iterator)
# 只有当不是无限循环的可迭代对象时生效
def filter_iter(iterable, f):
    yield from [elem for elem in iterable if f(elem)]
    # 理解yield from!!!!!!!!!!!!!!!!!!!!
```

**==yield from iterable object 等价于 for item in iterable object: yield item==**

### Q6: Primes Generator

Write a function `primes_gen` that takes a single argument `n` and yields all prime numbers less than or equal to `n` in decreasing order. Assume `n >= 1`. You may use the `is_prime` function included below, which we implemented in [Discussion 3](https://cs61a.org/disc/disc03/).编写一个函数“primes_gen”，它接受一个参数“n”，**并按降序生成所有小于或等于“n”的素数**。假设“n>=1”。您可以使用下面包含的“is_prime”函数，我们在讨论3中实现了该函数

框架:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230104230421042.png" alt="image-20230104230421042" style="zoom:33%;" />

```py
def is_prime(n):
    """Returns True if n is a prime number and False otherwise.
    >>> is_prime(2)
    True
    >>> is_prime(16)
    False
    >>> is_prime(521)
    True
    """
    def helper(i):
        if i > (n ** 0.5): # Could replace with i == n
            return True
        elif n % i == 0:
            return False
        return helper(i + 1)
    return helper(2)

def primes_gen(n):
    """Generates primes in decreasing order.
    >>> pg = primes_gen(7)
    >>> list(pg)
    [7, 5, 3, 2]
    """
    if n == 1:
        return
    if is_prime(n):
        yield n
    yield from primes_gen(n - 1)
    #再次理解 yield from !!!!!!!!!!!!!
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230104231543834.png" alt="image-20230104231543834" style="zoom:50%;" />

可以理解为 **==递归+找到下一个可以返回的yield语句!!!!!!!!!!!!!!!!!!!!!!!!!==**



### Q7: Generate Preorder

**Part 1:**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230104231809219.png" alt="image-20230104231809219" style="zoom:50%;" />

```py
def preorder(t):
    """Return a list of the entries in this tree in the order that they
    would be visited by a preorder traversal (see problem description).

    >>> numbers = tree(1, [tree(2), tree(3, [tree(4), tree(5)]), tree(6, [tree(7)])])
    >>> preorder(numbers)
    [1, 2, 3, 4, 5, 6, 7]
    >>> preorder(tree(2, [tree(4, [tree(6)])]))
    [2, 4, 6]
    """
    "*** YOUR CODE HERE ***"
    if branches(t) == []:
        return [lable(t)]
    flattened_branches = []
    for child in branches(t):
        flattened_branches += preorder(child)
    return [lable(t)] + flattened_branches
```

**Part 2:**

Similarly to `preorder` defined above, define the function `generate_preorder`, which takes in a tree as an argument and now instead `yield`s the entries in the tree in the order that `print_tree` would print them.

> **Hint:** How can you modify your implementation of `preorder` to `yield from` your recursive calls instead of returning them?

```py
def generate_preorder(t):
    """Yield the entries in this tree in the order that they
    would be visited by a preorder traversal (see problem description).

    >>> numbers = tree(1, [tree(2), tree(3, [tree(4), tree(5)]), tree(6, [tree(7)])])
    >>> gen = generate_preorder(numbers)
    >>> next(gen)
    1
    >>> list(gen)
    [2, 3, 4, 5, 6, 7]
    """
    "*** YOUR CODE HERE ***"
    yield lable(t)
    for b in branches(t):
        yield from generator_preorder(b)
```
