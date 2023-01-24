# HW2 HOF

## Q1 Product (高阶函数)

Write a function called `product` that returns `term(1) * ... * term(n)`.

```python
def product(n, term):
    """Return the product of the first n terms in a sequence.

    n: a positive integer
    term:  a function that takes one argument to produce the term

    >>> product(3, identity)  # 1 * 2 * 3
    6
    >>> product(5, identity)  # 1 * 2 * 3 * 4 * 5
    120
    >>> product(3, square)    # 1^2 * 2^2 * 3^2
    36
    >>> product(5, square)    # 1^2 * 2^2 * 3^2 * 4^2 * 5^2
    14400
    >>> product(3, increment) # (1+1) * (2+1) * (3+1)
    24
    >>> product(3, triple)    # 1*3 * 2*3 * 3*3
    162
    """
    total, k = 1, 1
    while k <= n:
        total, k = term(k) * total, k + 1
    return total
```

## Q2 Accumulate

让我们来看看 product 是一个更一般的函数的实例，叫做 accumulate ，我们想实现它。

```py
def accumulate(merger, start, n, term):
    """Return the result of merging the first n terms in a sequence and start.
    The terms to be merged are term(1), term(2), ..., term(n). 
    merger is a two-argument commutative function.

    >>> accumulate(add, 0, 5, identity)  # 0 + 1 + 2 + 3 + 4 + 5
    15
    >>> accumulate(add, 11, 5, identity) # 11 + 1 + 2 + 3 + 4 + 5
    26
    >>> accumulate(add, 11, 0, identity) # 11 
    11
    >>> accumulate(add, 11, 3, square)   # 11 + 1^2 + 2^2 + 3^2 (merge 是 +)
    25
    >>> accumulate(mul, 2, 3, square)    # 2 * 1^2 * 2^2 * 3^2 (merge 是 *)
    72
    >>> # 2 + (1^2 + 1) + (2^2 + 1) + (3^2 + 1)
    >>> accumulate(lambda x, y: x + y + 1, 2, 3, square)
    19
    >>> # ((2 * 1^2 * 2) * 2^2 * 2) * 3^2 * 2
    >>> accumulate(lambda x, y: 2 * x * y, 2, 3, square)
    576
    >>> accumulate(lambda x, y: (x + y) % 17, 19, 20, square)
    16
    """
    total = start
    k = 1
    while k <= n:
        total = merger(total, term(k))
        k += 1
    return total
```

merge是把这些term整合起来的方法(自己定义的)



接下来完成

```python
def summation_using_accumulate(n, term):
    """Returns the sum: term(0) + ... + term(n), using accumulate.

    >>> summation_using_accumulate(5, square)
    55
    >>> summation_using_accumulate(5, triple)
    45
    >>> # You aren't expected to understand the code of this test.
    >>> # Check that the bodies of the functions are just return statements.
    >>> # If this errors, make sure you have removed the "***YOUR CODE HERE***".
    >>> import inspect, ast
    >>> [type(x).__name__ for x in ast.parse(inspect.getsource(summation_using_accumulate)).body[0].body]
    ['Expr', 'Return']
    """
    return accumulate(add, 0, n, term)

def product_using_accumulate(n, term):
    """Returns the product: term(1) * ... * term(n), using accumulate.

    >>> product_using_accumulate(4, square)
    576
    >>> product_using_accumulate(6, triple)
    524880
    >>> # You aren't expected to understand the code of this test.
    >>> # Check that the bodies of the functions are just return statements.
    >>> # If this errors, make sure you have removed the "***YOUR CODE HERE***".
    >>> import inspect, ast
    >>> [type(x).__name__ for x in ast.parse(inspect.getsource(product_using_accumulate)).body[0].body]
    ['Expr', 'Return']
    """
    return accumulate(mul, 1, n, term)
```

**Takeaway:** Notice how quick it is now to create accumulator functions with different `merger` functions! This is because we **abstracted away** the logic of `product` and `summation` into the `accumulate` function. Without this abstraction, our code for a `summation` function would be just as long as our code for the `product` function from Question 1, and the logic would be highly redundant!

**要点：**请注意，现在用不同的“合并”函数创建累加器函数有多快！这是因为我们将“积”和“求和”的逻辑抽象为“累加”函数。如果没有这种抽象，我们的“求和”函数代码将与问题1中的“积”函数代码一样长，逻辑将是高度冗余的！

# HW3 Recursion, Tree Recursion

数出所给的数里有几个数字“8”

```py
def num_eights(pos):
    """Returns the number of times 8 appears as a digit of pos.

    >>> num_eights(3)
    0
    >>> num_eights(8)
    1
    >>> num_eights(88888888)
    8
    >>> num_eights(8782089)
    3
    不能用赋值语句,循环语句,只能用递归
    """
    "*** YOUR CODE HERE ***"
    if pos == 0:
        return 0
    if pos % 10 == 8:
        return 1 + num_eights(pos // 10)
    else:
        return num_eights(pos // 10)

    # 另外的标准答案:
    if pos % 10 == 8:
        return 1 + num_eights(pos // 10)
    elif pos < 10:
        return 0
    else:
        return num_eights(pos // 10)
```

## Q2: Ping-pong

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221229214302131.png" alt="image-20221229214302131" style="zoom:50%;" />

如果数字含"8"或者是8的倍数,那么就改变单调性

> **Hint:** If you're stuck, **first try implementing `pingpong` using assignment statements and a `while` statement.** Then, to convert this into a recursive solution,write a helper function that has a parameter for each variable that changes values in the body of the while loop.

> **Hint:** There are a few pieces of information that we need to keep track of. One of these details is the direction that we're going (either increasing or decreasing). Building off of the hint above, **think about how we can keep track of the direction throughout the calls to the helper function.**

```py
def pingpong(n):
    """Return the nth element of the ping-pong sequence.

    >>> pingpong(8)
    8
    >>> pingpong(10)
    6
    >>> pingpong(15)
    1
    >>> pingpong(100)
    -6
    不能用赋值语句,循环语句,只能用递归
    """
    "*** YOUR CODE HERE ***"
    def helper(result, i, step):
        if i == n:
            return result
        elif i % 8 == 0 or num_eights(i) > 0:
            return helper(result - step, i + 1, -step)
        else:
            return helper(result + step, i + 1, step)
    return helper(1, 1, 1)

# Alternate solution 1
def pingpong_next(x, i, step):
    if i == n:
        return x
    return pingpong_next(x + step, i + 1, next_dir(step, i+1))

def next_dir(step, i):
    if i % 8 == 0 or num_eights(i) > 0:
        return -step
    return step

# Alternate solution 2
def pingpong_alt(n):
    if n <= 8:
        return n
    return direction(n) + pingpong_alt(n-1)

def direction(n):
    if n < 8:
        return 1
    if (n-1) % 8 == 0 or num_eights(n-1) > 0:
        return -1 * direction(n-1)
    return direction(n-1)
```

**==非常非常非常经典,因为这个是从前往后(从数字上看似)递归的==**

==**要设计出不同的变量, i来*跟踪*数字, step用来表示变化量(1 / -1)**==

## Q3: Count coins

有五种硬币,面值分别为1, 5, 10, 25

给定一个面额,返回不同硬币的组合方法数

> 可以用给定的两个方法`next_larger_coin`和`next_smaller_coin`**之一**

可以参照1.7.5节的example<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221229220505416.png" alt="image-20221229220505416" style="zoom: 50%;" />

==**要学会在函数里面def函数,因为是要写一个新的递归函数!!!以前是写在外面,但是在python中不用了!!!**==

```py
def count_coins(change):
    """Return the number of ways to make change using coins of value of 1, 5, 10, 25.
    >>> count_coins(15)
    6
    >>> count_coins(10)
    4
    >>> count_coins(20)
    9
    >>> count_coins(100) # How many ways to make change for a dollar?
    242
    """
    "*** YOUR CODE HERE ***"
    def constrained_count(change, smallest_coin):
        if change == 0:
            return 1#用光了,说明是一种合理的方法!!!!!!!!!
        if change < 0:
            return 0
        if smallest_coin == None:
            return 0
        without_coin = constrained_count(change, next_larger_coin(smallest_coin))
        with_coin = constrained_count(change - smallest_coin, smallest_coin)
        return without_coin + with_coin
    return constrained_count(change, 1)

def next_larger_coin(coin):
    """Returns the next larger coin in order.
    >>> next_larger_coin(1)
    5
    >>> next_larger_coin(5)
    10
    >>> next_larger_coin(10)
    25
    >>> next_larger_coin(2) # Other values return None
    """
    if coin == 1:
        return 5
    elif coin == 5:
        return 10
    elif coin == 10:
        return 25
```



## Q4: Busy Beaver Contest(Optional)

编写一个尽可能多得 调用f的一行的函数(娱乐性质)

> f是一个接受0个参数,输出为无的函数

```py
def beaver(f):
    # Shortest solution (73 characters)
    (lambda g: g(g(g(g(g(g(g(f))))))))(lambda f: lambda: f() or f() or f())()
```



# HW4: Sequences, Trees

## Arms-length recursion 保持距离的递归(功能分离)

递归的补充材料:

Before we get started, a quick comment on recursion with tree data structures. Consider the following function.在我们开始之前，先简单介绍一下使用树数据结构的递归。考虑以下函数。

```py
def min_depth(t):
    """A simple function to return the distance between t's root and its closest leaf"""
    if is_leaf(t):
        return 0 # Base case---the distance between a node and itself is zero
    h = float('inf') # Python's version of infinity
    for b in branches(t):
        if is_leaf(b): return 1 # !!!
        h = min(h, 1 + min_depth(b))
    return h
```

The line flagged with `!!!` is an "arms-length" recursion violation. Although our code works correctly when it is present, by performing this check we are doing work that should be done by the next level of recursion—we already have an if-statement that handles any inputs to `min_depth` that are leaves, so we should not include this line to eliminate redundancy in our code.标记为“！！！”的行是“臂长”递归冲突。*虽然我们的代码在出现时可以正常工作*，但通过执行此检查，**==我们正在做下一级递归应该做的工作==，**我们已经有了一个if语句，它处理“min_depth”的任何输入，这些输入都是叶子，**因此我们不应该包括这一行以消除代码中的冗余。**

```py
def min_depth(t):
    """A simple function to return the distance between t's root and its closest leaf"""
    if is_leaf(t):
        return 0
    h = float('inf')
    for b in branches(t):
        # Still works fine!
        h = min(h, 1 + min_depth(b))
    return h
```

Arms-length recursion is not only redundant but often complicates our code and obscures the functionality of recursive functions, making writing recursive functions much more difficult. We always want our recursive case to be handling one and only one recursive level. We may or may not be checking your code periodically for things like this.臂长递归不仅是冗余的，而且常常使我们的代码复杂化，**==并且模糊了递归函数的功能，使得编写递归函数变得更加困难。我们总是希望我们的递归案例处理一个且只有一个递归级别。==**我们可能会或可能不会定期检查您的代码，以了解类似情况。

## Mobiles

> 本题改变自 MIT Structure and Interpretation of Computer Programs, [Section 2.2.2](https://mitp-content-server.mit.edu/books/content/sectbyfn/books_pres_0/6515/sicp.zip/full-text/book/book-Z-H-15.html#%_sec_2.2.2).

We are making a planetarium mobile. A mobile is a type of hanging sculpture. A binary mobile consists of two arms. Each arm is a rod of a certain length, from which hangs either a planet or another mobile. For example, the below diagram shows the left and right arms of Mobile A, and what hangs at the ends of each of those arms.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230102104051644.png" alt="image-20230102104051644" style="zoom:50%;" />

- A `mobile` must have both a left `arm` and a right `arm`.

- An `arm` has a positive length and must have something hanging at the end, either a `mobile` or `planet`.

- A `planet` has a positive mass, and nothing hanging from it.

  实景图:方便理解`length`是什么**==length是arm的长度(即左右有多长),而不是悬挂planet的上下有多长==**<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230102105736414.png" alt="image-20230102105736414" style="zoom: 25%;" />

### Q2: Weights

Your job is to implement the `planet` data abstraction by **completing the `planet` constructor and the `mass` selector** so that a planet is represented using a two-element list where the first element is the string `'planet'` and the second element is its mass.

### Q3: Balanced

Implement the `balanced` function, which returns whether `m` is a balanced mobile. A mobile is balanced if both of the following conditions are met(满足):

1. The torque applied by its left arm is equal to that applied by its right arm. The torque of the left arm is the length of the left rod multiplied by the total weight hanging from that rod. Likewise for the right. For example, if the left arm has a length of `5`, and there is a `mobile` hanging at the end of the left arm of total weight `10`, the torque on the left side of our mobile is `50`.左臂施加的扭矩等于右臂施加的扭矩。左臂的扭矩是左杆的长度乘以悬挂在该杆上的总重量。同样，对右派也是如此。例如，如果左臂的长度为“5”，并且总重量为“10”的左臂末端悬挂着一个“移动装置”，则移动装置左侧的扭矩为“50”。
2. Each of the mobiles hanging at the end of its arms is itself balanced.悬挂在手臂末端的每个mobile本身都是平衡的。

Planets themselves are balanced, as there is nothing hanging off of them.

> **Reminder:** You may use the `total_weight` function defined for you in Question 2 (as well as any of the other functions defined in Question 2).

```py
def balanced(m):
    """Return whether m is balanced.
    >>> balanced(mobile(arm(1, v), arm(1, w)))
    False
    >>> balanced(mobile(arm(1, w), arm(1, v)))
    False
    >>> from construct_check import check
    >>> # checking for abstraction barrier violations by banning indexing
    >>> check(HW_SOURCE_FILE, 'balanced', ['Index'])
    True
    """
    "*** YOUR CODE HERE ***"
    # l, r = left(m), right(m)
    # wei_l, len_l = total_weight(end(l)), length(l)
    # wei_r, len_r = total_weight(end(r)), length(r)
    # if wei_l * len_l != wei_r * len_r:
    #     return False
    # else:
    #     if not is_leaf(l):
    #         if not is_leaf(r):
    #             return balanced(l) and balanced(r)
    #         return balanced(l)
    #     if not is_leaf(r):
    #         return balanced(r)
    #     return True
    if is_planet(m):
        return True
    else:
        # 自己写的那个版本错在:如果是一个planet,那就没有left_end和right_end了
        left_end, right_end = end(left(m)), end(right(m))
        torque_left = length(left(m)) * total_weight(left_end)
        torque_right = length(right(m)) * total_weight(right_end)
        return torque_left == torque_right and balanced(left_end) and balanced(right_end)
```

### Q4: Totals

Implement `totals_tree`, which takes in a `mobile` or `planet` and returns a `tree` whose root is the total weight of the input. For a `planet`, `totals_tree` should return a leaf. For a `mobile`, `totals_tree` should return a tree whose label is that `mobile`'s total weight, and whose branches are `totals_tree`s for the ends of its arms. As a reminder, the description of the tree data abstraction can be found [here](http://composingprograms.com/pages/23-sequences.html#trees).

```py
def totals_tree(m):
    """Return a tree representing the mobile with its total weight at the root.
    >>> t, u, v = examples()
    >>> print_tree(totals_tree(v))
    9
      3
        2
        1
      6
        1
        5
          3
          2
    >>> from construct_check import check
    >>> # checking for abstraction barrier violations by banning indexing
    >>> check(HW_SOURCE_FILE, 'totals_tree', ['Index'])
    True
    """
    "*** YOUR CODE HERE ***"
    # 生成一个树,root是总重量,如果是mobile,那它就是leaf,内容是planet的重量
    if is_planet(m):
        return [total_weight(m)]
    else:
        left_end, right_end = end(left(m)), end(right(m))
        return [total_weight(left_end) + total_weight(right_end), totals_tree(left_end), totals_tree(right_end)]
    # better solution:
    if is_planet(m):
        return tree(mass(m))
    else:
        branches = [totals_tree(end(f(m))) for f in [left, right]]
        # 列表推导式也可以用函数function作为列表!!!!
        return tree(sum([label(b) for b in branches]), branches)
```

## More Trees

### Q5: Replace Loki at Leaf

Define `replace_loki_at_leaf`, which takes a tree `t` and a value `lokis_replacement`. `replace_loki_at_leaf` returns a new tree that's the same as `t` except that every leaf label equal to `"loki"` has been replaced with `lokis_replacement`.

把每个根节点的内容替换为要替换的那个值

```py
def replace_loki_at_leaf(t, lokis_replacement):
    """Returns a new tree where every leaf value equal to "loki" has
    been replaced with lokis_replacement.

    >>> yggdrasil = tree('odin',
    ...                  [tree('balder',
    ...                        [tree('loki'),
    ...                         tree('freya')]),
    ...                   tree('frigg',
    ...                        [tree('loki')]),
    ...                   tree('loki',
    ...                        [tree('sif'),
    ...                         tree('loki')]),
    ...                   tree('loki')])
    >>> laerad = copy_tree(yggdrasil) # copy yggdrasil for testing purposes
    >>> print_tree(replace_loki_at_leaf(yggdrasil, 'freya'))
    odin
      balder
        freya
        freya
      frigg
        freya
      loki
        sif
        freya
      freya
    >>> laerad == yggdrasil # Make sure original tree is unmodified
    True
    """
    "*** YOUR CODE HERE ***"
    if is_leaf(t):
        if label(t) == 'loki':
            return tree(lokis_replacement)
        else:
            return tree(label(t))
    return tree(label(t), [replace_loki_at_leaf(branch, lokis_replacement) for branch in branches(t)])
    # better solution:
    if is_leaf(t) and label(t) == "loki":
        return tree(lokis_replacement)
    else:
        bs = [replace_loki_at_leaf(b, lokis_replacement) for b in branches(t)]
        return tree(label(t), bs)
```

### Q6: Has Path

Write a function `has_path` that takes in a tree `t` and a string `word`. It returns `True` if there is a path that starts from the root where the entries along the path spell out the `word`, and `False` otherwise. (This data structure is called a trie, and it has a lot of cool applications, such as autocomplete). You may assume that every node's `label` is exactly one character.

编写一个函数“has_path”，它接受树“t”和字符串“word”。如果存在从根开始的路径，则返回“True”，否则返回“False”。（这种数据结构被称为trie，它有很多很酷的应用程序，例如自动完成）。您可以假设每个节点的“标签”正好是一个字符。

```py
def has_path(t, word):
    """Return whether there is a path in a tree where the entries along the path
    spell out a particular word.

    >>> greetings = tree('h', [tree('i'),
    ...                        tree('e', [tree('l', [tree('l', [tree('o')])]),
    ...                                   tree('y')])])
    >>> print_tree(greetings)
    h
      i
      e
        l
          l
            o
        y
    >>> has_path(greetings, 'h')
    True
    >>> has_path(greetings, 'i')
    False
    >>> has_path(greetings, 'hi')
    True
    >>> has_path(greetings, 'hello')
    True
    >>> has_path(greetings, 'hey')
    True
    >>> has_path(greetings, 'bye')
    False
    >>> has_path(greetings, 'hint')
    False
    """
    assert len(word) > 0, 'no path for empty word.'
    "*** YOUR CODE HERE ***"
```

## Data Abstraction (Optional)

**Acknowledgements.** This interval arithmetic example is based on a classic problem from Structure and Interpretation of Computer Programs, [Section 2.1.4](https://sarabander.github.io/sicp/html/2_002e1.xhtml#g_t2_002e1_002e4).

**Introduction.** Alyssa P. Hacker is designing a system to help people solve engineering problems. One feature she wants to provide in her system is the ability to manipulate inexact quantities (such as measurements from physical devices) with known precision, so that when computations are done with such approximate quantities the results will be numbers of known precision. For example, if a measured quantity x lies between two numbers a and b, Alyssa would like her system to use this range in computations involving x.**简介。**Alyssa P.Hacker正在设计一个系统来帮助人们解决工程问题。她希望在她的系统中提供的一个功能是能够以已知的精度操纵不精确的量（例如来自物理设备的测量值），这样当使用这样的近似量进行计算时，结果将是已知精度的数字。例如，如果测量的量x位于两个数字a和b之间，Alyssa希望她的系统在涉及x的计算中使用这个范围。

Alyssa's idea is to implement interval arithmetic as a set of arithmetic operations for combining "intervals" (objects that represent the range of possible values of an inexact quantity). The result of adding, subracting, multiplying, or dividing two intervals is also an interval, one that represents the range of the result.Alyssa的想法是将区间算术实现为一组组合“区间”（表示不精确量的可能值范围的对象）的算术运算。将两个区间相加、子运算、相乘或除法的结果也是一个区间，表示结果的范围。

Alyssa suggests the existence of an abstraction called an "interval" that has two endpoints: a lower bound and an upper bound. She also presumes that, given the endpoints of an interval, she can create the interval using data abstraction. Using this constructor and the appropriate selectors, she defines the following operations:Alyssa建议存在一种称为“区间”的抽象，它有两个端点：下界和上界。她还假设，给定区间的端点，她可以使用数据抽象来创建区间。使用此构造函数和适当的选择器，她定义了以下操作：

```py
def str_interval(x):
    """Return a string representation of interval x."""
    return '{0} to {1}'.format(lower_bound(x), upper_bound(x))

def add_interval(x, y):
    """Return an interval that contains the sum of any value in interval x and
    any value in interval y."""
    lower = lower_bound(x) + lower_bound(y)
    upper = upper_bound(x) + upper_bound(y)
    return interval(lower, upper)
```

### Q7: Interval Abstraction

Alyssa's program is incomplete because she has not specified the implementation of the interval abstraction. She has implemented the constructor for you; fill in the implementation of the selectors.

```python
def interval(a, b):#区间,间隔
    """Construct an interval from a to b."""
    assert a <= b, 'Lower bound cannot be greater than upper bound'
    return [a, b]

def lower_bound(x):
    """Return the lower bound of interval x."""
    "*** YOUR CODE HERE ***"
    return x[0]
    
def upper_bound(x):
    """Return the upper bound of interval x."""
    "*** YOUR CODE HERE ***"
    return x[1]
```

### Q8: Interval Arithmetic 区间数学

After implementing the abstraction, Alyssa decided to implement a few interval arithmetic functions.

This is her current implementation for **interval multiplication**. Unfortunately there are some data abstraction violations, so your task is to fix this code before [someone sets it on fire](https://youtu.be/7zu30DhJLKU?t=444).

```py
def mul_interval(x, y):
    """Return the interval that contains the product of any value in x and any
    value in y."""
    # p1 = x[0] * y[0]
    # p2 = x[0] * y[1]
    # p3 = x[1] * y[0]
    # p4 = x[1] * y[1] 
    # return [min(p1, p2, p3, p4), max(p1, p2, p3, p4)]
    # fix:
    p1 = lower_bound(x) * lower_bound(y)
    p2 = lower_bound(x) * upper_bound(y)
    p3 = upper_bound(x) * lower_bound(y)
    p4 = upper_bound(x) * upper_bound(y) 
    return interval(min(p1, p2, p3, p4), max(p1, p2, p3, p4))
```

**Interval Subtraction**

Using a similar approach as `mul_interval` and `add_interval`, define a subtraction function for intervals. If you find yourself repeating code, see if you can reuse functions that have already been implemented.

实现减法函数,试图寻找可以用的已经实现的函数(add函数)

```py
def sub_interval(x, y):
    """Return the interval that contains the difference between any value in x
    and any value in y."""
    "*** YOUR CODE HERE ***"
    # lower = lower_bound(y) - upper_bound(x)
    # upper = upper_bound(y) - lower_bound(x)
    # return interval(lower, upper_bound)
    # changed to:
    negative_y = interval(-upper_bound(y), -lower_bound(y))
    return add_interval(x, negative_y)
```

**Interval Division**

Alyssa implements division below by multiplying by the reciprocal of `y`. A systems programmer looks over Alyssa's shoulder and comments that it is not clear what it means to divide by an interval that spans zero. Add an `assert` statement to Alyssa's code to ensure that no such interval is used as a divisor:Alyssa通过乘以“y”的倒数来实现下面的除法。一位系统程序员越过Alyssa的肩膀，评论道，不清楚除以一个跨度为零的间隔意味着什么。在Alyssa的代码中添加一个“assert”语句，以确保没有这样的间隔用作除数：

区间不能跨越0,添加assert来实现

```py
def div_interval(x, y):
    """Return the interval that contains the quotient of any value in x divided by
    any value in y. Division is implemented as the multiplication of x by the
    reciprocal of y."""
    "*** YOUR CODE HERE ***"
    assert not (lower_bound(y) <= 0 <= upper_bound(y)), 'Divide by zero' # python中可以用这种语法 
    reciprocal_y = interval(1 / upper_bound(y), 1 / lower_bound(y))
    return mul_interval(x, reciprocal_y)
```

### Q9: Par Diff

After considerable work, Alyssa P. Hacker delivers her finished system. Several years later, after she has forgotten all about it, she gets a frenzied call from an irate user, Lem E. Tweakit. It seems that Lem has noticed that the [formula for parallel resistors](https://en.wikipedia.org/wiki/Series_and_parallel_circuits#Resistors_2) can be written in two algebraically equivalent ways:经过大量工作后，Alyssa P.Hacker交付了她的成品系统。几年后，在她忘记了这一切之后，她接到了一个愤怒的用户Lem E.Tweakit的疯狂电话。Lem似乎注意到了[并联电阻器的公式](https://en.wikipedia.org/wiki/Series_and_parallel_circuits#Resistors_2)可以用两种代数等价的方式写：

```py
par1(r1, r2) = (r1 * r2) / (r1 + r2)
```

or

```py
par2(r1, r2) = 1 / (1/r1 + 1/r2)
```

He has written the following two programs, each of which computes the `parallel_resistors` formula differently:

```py
def par1(r1, r2):
    return div_interval(mul_interval(r1, r2), add_interval(r1, r2))

def par2(r1, r2):
    one = interval(1, 1)
    rep_r1 = div_interval(one, r1)
    rep_r2 = div_interval(one, r2)
    return div_interval(one, add_interval(rep_r1, rep_r2))
```

Lem points out that Alyssa's program gives different answers for the two ways of computing. Find two intervals `r1` and `r2` that demonstrate the difference in behavior between `par1` and `par2` when passed into each of the two functions.Lem指出，Alyssa的程序对两种计算方式给出了不同的答案。找到两个区间“r1”和“r2”，它们表明当传递到两个函数中的每一个时，“par1”和“par2”之间的行为差异。

Demonstrate that Lem is right. Investigate the behavior of the system on a variety of arithmetic expressions. Make some intervals `r1` and `r2`, and show that `par1` and `par2` can give different results.证明Lem是对的。研究系统在各种算术表达式上的行为。制作一些间隔“r1”和“r2”，并显示“par1”和“par2”可以给出不同的结果。



# HW5: Generators

### Q1: Merge

Write a generator function `merge` that takes in two infinite generators `a` and `b` that are in increasing order without duplicates and returns a generator that has all the elements of both generators, in increasing order, without duplicates.编写一个生成器函数“merge”，该函数接受两个无限生成器“a”和“b”，它们按递增顺序排列，没有重复，并返回一个生成器，该生成器具有两个生成器的所有元素，按递增顺序，没有重复。

```py
def merge(a, b):
    """
    >>> def sequence(start, step):
    ...     while True:
    ...         yield start
    ...         start += step
    >>> a = sequence(2, 3) # 2, 5, 8, 11, 14, ...
    >>> b = sequence(3, 2) # 3, 5, 7, 9, 11, 13, 15, ...
    >>> result = merge(a, b) # 2, 3, 5, 7, 8, 9, 11, 13, 14, 15
    >>> [next(result) for _ in range(10)]
    [2, 3, 5, 7, 8, 9, 11, 13, 14, 15]
    """
    "*** YOUR CODE HERE ***"
    # first_a, first_b = next(a), next(b)
    # while True:
    #   if first_a >= first_b:
    #     yield b
    #     yield from merge(iter(a))
    #   elif first_a < first_b:
    #     yield a
    first_a, first_b = next(a), next(b)
    while True:
        if first_a == first_b:
            yield first_a
            first_a, first_b = next(a), next(b)
        elif first_a < first_b:
            yield first_a
            first_a = next(a)
        else:
            yield first_b
            first_b = next(b)
```

### Q2: Generate Permutations

> *Hint:* If you had the permutations of all the elements in `seq` **not including the first elemen**t, how could you use that to generate the permutations of the full `seq`?
>
> 提示：如果您有seq中所有元素的排列，但不包括第一个元素，您如何使用它来生成整个seq的排列？

> *Hint:* Remember, it's possible to **loop over generator** objects because generators are iterators!
>
> 提示：记住，可以==**循环遍历生成器对象**==，因为生成器是迭代器！

排列组合可以以任何顺序产生

```py
def gen_perms(seq):
    """Generates all permutations of the given sequence. Each permutation is a
    list of the elements in SEQ in a different order. The permutations may be
    yielded in any order.

    >>> perms = gen_perms([100])
    >>> type(perms)
    <class 'generator'>
    >>> next(perms)
    [100]
    >>> try: #this piece of code prints "No more permutations!" if calling next would cause an error
    ...     next(perms)
    ... except StopIteration:
    ...     print('No more permutations!')
    No more permutations!
    >>> sorted(gen_perms([1, 2, 3])) # Returns a sorted list containing elements of the generator
    [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]
    >>> sorted(gen_perms((10, 20, 30)))
    [[10, 20, 30], [10, 30, 20], [20, 10, 30], [20, 30, 10], [30, 10, 20], [30, 20, 10]]
    >>> sorted(gen_perms("ab"))
    [['a', 'b'], ['b', 'a']]
    """
    "*** YOUR CODE HERE ***"
    if not seq:
        yield []
    else:
        for perm in gen_perms(seq[1:]):
            for i in range(len(seq)):
                yield perm[:i] + [seq[0]] + perm[i:]
    # 不理解没关系,先记住!!
```

### Q3: Yield Paths

Define a generator function `yield_paths` which takes in a tree `t`, a value `value`, and returns a generator object which yields each path from the root of `t` to a node that has label `value`.定义一个生成器函数“yield_path”，它接受树“t”和值“value”，并返回一个生成器对象，该对象生成从“t”的根到具有标签“value”的节点的每条路径。

Each path should be represented as a list of the labels along that path in the tree. You may yield the paths in any order.每个路径都应该表示为树中该路径上的标签列表。您可以按任何顺序yield路径。

We have provided a skeleton for you. You do not need to use this skeleton, but if your implementation diverges significantly from it, you might want to think about how you can get it to fit the skeleton.我们为您提供了一个框架。您不需要使用这个框架，但是如果您的实现与它有很大的差异，您可能需要考虑如何使它适合这个框架。

> *Hint:* If you're having trouble getting started, think about how you'd approach this problem if it wasn't a generator function. What would your recursive calls be? With a generator function, what happens if you make a "recursive call" within its body?
>
> *提示：*如果您在开始时遇到困难，请考虑如果不是生成器函数，您将如何处理此问题。你的递归调用是什么？对于生成器函数，如果在其主体内进行“递归调用”，会发生什么？

> *Hint:* Remember, it's possible to loop over generator objects because generators are iterators!
>
> *提示：*记住，因为生成器是迭代器，所以可以循环遍历生成器对象！

> Note: Remember that this problem should **yield items** -- do not return a list!

```py
def yield_paths(t, value):
    """Yields all possible paths from the root of t to a node with the label
    value as a list.

    >>> t1 = tree(1, [tree(2, [tree(3), tree(4, [tree(6)]), tree(5)]), tree(5)])
    >>> print_tree(t1)
    1
      2
        3
        4
          6
        5
      5
    >>> next(yield_paths(t1, 6))
    [1, 2, 4, 6]
    >>> path_to_5 = yield_paths(t1, 5)
    >>> sorted(list(path_to_5))
    [[1, 2, 5], [1, 5]]
    """
    "*** YOUR CODE HERE ***"
    if label(t) == value:
        yield [value]
    for b in branches(t):
        for path in yield_paths(b, value):#从这里开始就开始yield了!!!!!!!!!!!!!!!!!!!!
            yield [label[t]] + path
            "*** YOUR CODE HERE ***"
```

**==这个path最后一定有value,否则不会yield!!!!!!!!!!!!==**

**==从`yield_paths(b, value)`开始就开始yield了!!!!!!!!!!!!!!!!!!!!==**

### Q4: Infinite Hailstone (Optional)

Here's a quick reminder of how the hailstone sequence is defined:

1. Pick a positive integer `n` as the start.
2. If `n` is even, divide it by 2.如果“n”是偶数，则将其除以2。
3. If `n` is odd, multiply it by 3 and add 1.如果“n”是奇数，则将其乘以3并加1。
4. Continue this process until `n` is 1.继续此过程，直到“n”为1。

Try to write this generator function recursively. If you're stuck, you can first try writing it iteratively and then seeing how you can turn that implementation into a recursive one.尝试递归地编写此生成器函数。如果遇到问题，可以先尝试迭代编写，然后看看如何将该实现转换为递归实现。

> **Hint:** Since `hailstone` returns a generator, you can `yield from` a call to `hailstone`!

```py
def hailstone(n):
    """Yields the elements of the hailstone sequence starting at n.
       At the end of the sequence, yield 1 infinitely.

    >>> hail_gen = hailstone(10)
    >>> [next(hail_gen) for _ in range(10)]
    [10, 5, 16, 8, 4, 2, 1, 1, 1, 1]
    >>> next(hail_gen)
    1
    """
    "*** YOUR CODE HERE ***"
    # 虽然和答案一模一样,但是是完全自己写的!!!!!!!
    yield n 
    if n == 1:
        yield from hailstone(n)
    if n % 2 == 0:
        yield from hailstone(n // 2)#不要用 n / 2,会有小数点
    else:
        yield from hailstone(n * 3 + 1)
```

### Q5: Remainder Generator

本题要点:==**生成器也可以是高阶函数**==

Like functions, ==**generators** can also be ***higher-order***==. For this problem, we will be writing `remainders_generator`, **which yields a series of generator objects.**

`remainders_generator` takes in an integer `m`, and yields `m` different generators. The first generator is a generator of multiples of `m`, i.e. numbers where the remainder is 0. The second is a generator of natural numbers with remainder 1 when divided by `m`. The last generator yields natural numbers with remainder `m - 1` when divided by `m`.`

remainders_generator“接受整数“m”，并生成**“m”个不同的生成器**。第一个生成器是“m”的倍数的生成器，即余数为0的数字。第二个是自然数的生成器，当除以“m”时，余数为1。最后一个生成器在被“m”除时产生余数为“m-1”的自然数。

> Note that if you have implemented this correctly, each of the generators yielded by `remainder_generator` will be *infinite* - you can keep calling `next` on them forever without running into a `StopIteration` exception.
>
> 请注意，如果您正确实现了这一点，“reminder_generator”生成的每个生成器都将是*无限的*-您可以一直对它们调用“next”，而不会遇到“StopIteration”异常。

```py
def remainders_generator(m):
    """
    Yields m generators. The ith yielded generator yields natural numbers whose
    remainder is i when divided by m.

    >>> import types
    >>> [isinstance(gen, types.GeneratorType) for gen in remainders_generator(5)]
    [True, True, True, True, True]
    >>> remainders_four = remainders_generator(4)
    >>> for i in range(4):
    ...     print("First 3 natural numbers with remainder {0} when divided by 4:".format(i))
    ...     gen = next(remainders_four)
    ...     for _ in range(3):
    ...         print(next(gen))
    First 3 natural numbers with remainder 0 when divided by 4:
    4
    8
    12
    First 3 natural numbers with remainder 1 when divided by 4:
    1
    5
    9
    First 3 natural numbers with remainder 2 when divided by 4:
    2
    6
    10
    First 3 natural numbers with remainder 3 when divided by 4:
    3
    7
    11
    """
    "*** YOUR CODE HERE ***"
    def gen(i):
        for e in naturals():# 这里不会返回e,因为这里的e相当于返回了(内层环境)
            if e % m == i:
                yield e
    for i in range(m):
        yield gen(i)
```



# Homework 6: Object Oriented Programming, Linked Lists

## OOP

### Q1: Mint (造币厂)

## Linked Lists

### Q2: Store Digits

Write a function `store_digits` that takes in an integer `n` and returns a linked list where each element of the list is a digit of `n`.

> **Important**: Do not use any string manipulation functions like `str` and `reversed`.
>
> **重要**：不要使用任何字符串操作函数，如“str”和“reversed”。

```py
def store_digits(n):
    """Stores the digits of a positive number n in a linked list.
    >>> s = store_digits(1)
    >>> s
    Link(1)
    >>> store_digits(2345)
    Link(2, Link(3, Link(4, Link(5))))
    >>> store_digits(876)
    Link(8, Link(7, Link(6)))
    """
    "*** YOUR CODE HERE ***"
    all_but_last = n 
    link_now = () #tuple 是不可变的
    while all_but_last != 0:
        last_dig = all_but_last % 10
        all_but_last //= 10

        link_now = Link(last_dig, link_now)
    return link_now
```

### Q3: Mutable Mapping

Implement `deep_map_mut(func, link)`, which applies a function `func` onto all elements in the given linked list `lnk`. If an element is itself a linked list, apply `func` to each of its elements, and so on.实现“deep_map_mut（func，link）”，它将函数“func”应用于给定链接列表“lnk”中的所有元素。如果元素本身是一个链接列表，则对其每个元素应用“func”，依此类推。

Your implementation should mutate the original linked list. Do not create any new linked lists.您的实现应该改变原始链接列表。不要创建任何新的链接列表。

> **Hint**: The built-in `isinstance` function may be useful.
>
> **提示**：内置的“isinstance”函数可能有用。
>
> ```py
> >>> s = Link(1, Link(2, Link(3, Link(4))))
> >>> isinstance(s, Link)
> True
> >>> isinstance(s, int)
> False
> ```

> **Construct Check**: The last doctest of this question ensures that you do not create new linked lists. If you are failing this doctest, ensure that you are not creating link lists by calling the constructor, i.e.
>
> **构造检查**：此问题的最后一个doctest确保您不会创建新的链接列表。如果此doctest失败，请确保您没有通过调用构造函数创建链接列表，即。
>
> ```py
> s = Link(1)
> ```

```py
def deep_map_mut(func, lnk):
    """Mutates a deep link lnk by replacing each item found with the
    result of calling func on the item.  Does NOT create new Links (so
    no use of Link's constructor).

    Does not return the modified Link object.

    >>> link1 = Link(3, Link(Link(4), Link(5, Link(6))))
    >>> # Disallow the use of making new Links before calling deep_map_mut
    >>> Link.__init__, hold = lambda *args: print("Do not create any new Links."), Link.__init__
    >>> try:
    ...     deep_map_mut(lambda x: x * x, link1)
    ... finally:
    ...     Link.__init__ = hold
    >>> print(link1)
    <9 <16> 25 36>
    """
    "*** YOUR CODE HERE ***"
    # 这种要尝试用递归的方法!!!!!!
    # 元素本身也可能是列表!!!!!
    if isinstance(lnk.first, Link):
        deep_map_mut(func, lnk.first)
    else:
        lnk.first = func(lnk.first)
    if lnk.rest == Link.empty:
        return 
    else:
        deep_map_mut(func, lnk.rest)
```

### Q4: Two List

Implement a function `two_list` that takes in two lists and returns a linked list. The first list contains the values that we want to put in the linked list, and the second list contains the number of each corresponding value. Assume both lists are the same size and have a length of 1 or greater. Assume all elements in the second list are greater than 0.实现一个函数“two_list”，它接受两个列表并返回一个链接列表。第一个列表包含要放入链接列表中的值，第二个列表包含每个对应值的编号。假设两个列表大小相同，长度为1或更大。假设第二个列表中的所有元素都大于0。

```py
def two_list(vals, counts):
    """
    Returns a linked list according to the two lists that were passed in. Assume
    vals and counts are the same size. Elements in vals represent the value, and the
    corresponding element in counts represents the number of this value desired in the
    final linked list. Assume all elements in counts are greater than 0. Assume both
    lists have at least one element.

    >>> a = [1, 3, 2]
    >>> b = [1, 1, 1]
    >>> c = two_list(a, b)
    >>> c
    Link(1, Link(3, Link(2)))
    >>> a = [1, 3, 2]
    >>> b = [2, 2, 1]
    >>> c = two_list(a, b)
    >>> c
    Link(1, Link(1, Link(3, Link(3, Link(2)))))
    """
    "*** YOUR CODE HERE ***"
    vals = vals[::-1]
    counts = counts[::-1]
    list_now = Link.empty
    for i in range(len(vals)):
        val = vals[i]
        for _ in range(counts[i]):
            list_now = Link(val, list_now)
    return list_now
```

### Q5: Next Virahanka Fibonacci Object

Implement the `next` method of the `VirFib` class. For this class, the `value` attribute is a Fibonacci number. The `next` method returns a `VirFib` instance whose `value` is the next Fibonacci number. The `next` method should take only constant time.实现“VirFib”类的“next”方法。对于此类，“value”属性是斐波那契数。“next”方法返回一个“VirFib”实例，其“value”是下一个斐波那契数。“next”方法只需要恒定的时间。

Note that in the doctests, nothing is being printed out. Rather, each call to `.next()` returns a `VirFib` instance. The way each `VirFib` instance is displayed is determined by the return value of its `__repr__` method.注意，在doctest中，没有打印出任何内容。相反，每次调用“.next（）”都会返回一个“VirFib”实例。每个“VirFib”实例的显示方式由其“__repr___”方法的返回值决定

> *Hint*: Keep track of the previous number by setting a new instance attribute inside `next`. You can create new instance attributes for objects at any point, even outside the `__init__` method.
>
> *提示*：通过在“next”中设置一个新的实例属性来跟踪上一个数字。==**您可以在任何时候为对象创建新的实例属性，甚至在`__init__`方法之外。**==

```py
class VirFib():
    """A Virahanka Fibonacci number.

    >>> start = VirFib()
    >>> start
    VirFib object, value 0
    >>> start.next()
    VirFib object, value 1
    >>> start.next().next()
    VirFib object, value 1
    >>> start.next().next().next()
    VirFib object, value 2
    >>> start.next().next().next().next()
    VirFib object, value 3
    >>> start.next().next().next().next().next()
    VirFib object, value 5
    >>> start.next().next().next().next().next().next()
    VirFib object, value 8
    >>> start.next().next().next().next().next().next() # Ensure start isn't changed
    VirFib object, value 8
    """

    def __init__(self, value=0):
        self.value = value

    def next(self):
        "*** YOUR CODE HERE ***"   
# Official solution
        if self.value == 0:
            result = VirFib(1)
        else:
            result = VirFib(self.value + self.previous)
        result.previous = self.value
        return result

    def __repr__(self):
        return "VirFib object, value " + str(self.value)
```

==**可以在任何时候为对象创建新的实例属性，甚至在`__init__`方法之外!!!!!!!!!!!!!!!!!!**==

# HW7 : Scheme

## Q1: Thane of Cadr

Define the procedures `cadr` and `caddr`, which return the second and third elements of a list, respectively. If you would like a quick refresher on Scheme syntax consider looking at the [Scheme Specification](https://cs61a.org/articles/scheme-spec/) and [Scheme Built-in Procedure Reference](https://cs61a.org/articles/scheme-builtins/) (and the [Lab 10 Scheme Refresher](https://cs61a.org/lab/lab10/#scheme) when it's released).定义过程“cadr”和“caddr”，它们分别返回列表的第二个和第三个元素。如果您想快速复习Scheme语法，请考虑查看[Scheme Specification](https://cs61a.org/articles/scheme-spec/)和[方案内置程序参考](https://cs61a.org/articles/scheme-builtins/)（和[实验室10方案刷新](https://cs61a.org/lab/lab10/#scheme)当它被释放时）。

```scheme
(define (cddr s) (cdr (cdr s)))

(define (cadr s) 
    (car (cdr s))
)

(define (caddr s) 
    (car (cdr (cdr s)))
)
```

## Q2: Ascending

Implement a procedure called `ascending?`, which takes a list of numbers `asc-lst` and returns `True` if the numbers are in nondescending order, and `False` otherwise. Numbers are considered nondescending if each subsequent number is either larger or equal to the previous, that is:执行名为“升序？”的过程，它获取一个数字列表“asc-lst”，如果数字是非降序的，则返回“True”，否则返回“False”。如果每个后续数字大于或等于前一个数字，即：

```scheme
1 2 3 3 4
```

Is nondescending, but:

```scheme
1 2 3 3 2
```

Is not.

> **Hint**: The built-in `null?` function returns whether its argument is `nil`.
>
> **提示**：内置的“null？”函数返回其参数是否为“nil”。
>
> **Note**: The question mark in `ascending?` is just part of the function name and has no special meaning in terms of Scheme syntax. It is a common practice in Scheme to name a function with a question mark at the end if the function returns a boolean value indicating whether or not a condition is satisfied. For instance, `ascending?` is a function that essentially asks "Is the argument in ascending order?", `null?` is a function that asks "Is the argument `nil`?", `even?` asks "Is the argument even?", etc.
>
> **注**：“升序？”中的问号只是函数名的一部分，在Scheme语法方面没有特殊含义。Scheme中的一种常见做法是，如果函数返回一个指示是否满足条件的布尔值，则在该函数的末尾使用问号来命名该函数。例如，“升序？”是一个本质上询问“参数是否按升序排列？”、“null？”的函数是一个询问“参数是否为‘nil’？”、‘even？’的函数问“argument是否even？”等。

```scheme
(define (ascending? asc-lst) 
    (cond 
        ((null? asc-lst) #t)
        ((null? (cdr asc-lst)) #t)
        ((<= (car asc-lst) (cadr asc-lst))
            (ascending? (cdr asc-lst))
            )
        (else #f)
    )
)
; can not have chinese language ,even the comment
```

> ==scheme里面不能出现中文,哪怕是注释==

## Q3: Pow

Implement a procedure `pow` for raising the number `base` to the power of a nonnegative integer `exp` for which the number of operations grows logarithmically, rather than linearly (the number of recursive calls should be much smaller than the input `exp`). For example, for `(pow 2 32)` should take 5 recursive calls rather than 32 recursive calls. Similarly, `(pow 2 64)` should take 6 recursive calls.实现一个过程“pow”，将数字“base”提高到非负整数“exp”的幂，对于该整数，操作的数量以对数而不是线性增长（递归调用的数量应该比输入“exp”小得多）。例如，For“（pow 2 32）”应该进行5次递归调用，而不是32次递归调用。类似地，“（pow 2 64）”应该进行6次递归调用。

> *Hint:* Consider the following observations:
>
> <img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230111181351526.png" alt="image-20230111181351526" style="zoom: 50%;" />
>
> For example we see that 232 is (216)2, 216 is (28)2, etc. You may use the built-in predicates `even?` and `odd?`. Scheme doesn't support iteration in the same manner as Python, so consider another way to solve this problem.例如，我们看到232是（216）2，216是（28）2等。您可以使用内置谓词“even？”和“奇数？”。Scheme不支持以与Python相同的方式进行迭代，因此请考虑另一种方法来解决此问题。

```scheme
(define (square n) (* n n))

(define (pow base exp) 
    (if (= y 1) x
        (if (even? y)
            (square (pow x (/ y 2)))
            (* x (square (pow x (/ (- y 1) 2))))
        )
    )
)
```

# HW8 : Scheme Lists

## Q1: My Filter

Write a procedure `my-filter`, which takes a predicate `pred` and a list `s`, and returns a new list containing only elements of the list that satisfy the predicate. The output should contain the elements in the same order that they appeared in the original list.编写一个过程“我的过滤器”，它接受谓词“pred”和列表“s”，并返回一个只包含满足谓词的列表元素的新列表。输出中包含元素的顺序应与原始列表中出现的顺序相同。

> **Note:** Make sure that you are not just calling the built-in `filter` function in Scheme - we are asking you to re-implement this!
>
> **注意：**确保您不仅仅是在Scheme中调用内置的“filter”函数，我们要求您重新实现它！

> **==再次提醒, 问号?是name的一部分,不是特定的语法(目的是提醒我们会返回true 或者 false)==**

```scheme
(define (my-filter pred s) 
  ; (if (null? s) s) ;don't forget to write the basic 
  ; (if (pred (car s))
  ;   (cons (car s) (my-filter pred (cdr s)))
  ;   (my-filter pred (cdr s))
  ;   )
  (cond 
    ((null? s) s)  
    ((pred (car s)) (cons (car s) (my-filter pred (cdr s))))
    (else (my-filter pred (cdr s)))
  )
  ;;; the first line is wrong, because scheme return the last line of the program, 
  ;;; so you should use cond
)
```

**==别忘了,scheme里面没有return语句,它自动返回最后一条语句==**

## Q2: Interleave

Implement the function `interleave`, which takes a two lists `lst1` and `lst2` as arguments. `interleave` should return a new list that interleaves the elements of the two lists. (In other words, the resulting list should contain elements alternating between `lst1` and `lst2`.)实现函数“interleave”，它将两个列表“lst1”和“lst2”作为参数`interleave”应该返回一个新的列表，该列表对两个列表的元素进行交错。（换句话说，生成的列表应包含在“lst1”和“lst2”之间交替的元素。）

If one of the input lists to `interleave` is shorter than the other, then `interleave` should alternate elements from both lists until one list has no more elements, and then the remaining elements from the longer list should be added to the end of the new list.如果要“交织”的输入列表中的一个比另一个短，则“交织”应交替两个列表中的元素，直到一个列表中没有更多的元素，然后应将较长列表中的其余元素添加到新列表的末尾。

```scheme
(define (interleave lst1 lst2) 
  (cond
    ((null? lst1) lst2)
    ((null? lst2) lst1)
    (else 
      (cons 
      (car lst1) 
        (cons 
        (car lst2)
        (interleave (cdr lst1) (cdr lst2))
        )
      )
    )
  )
)
```

## Q3: Accumulate

Fill in the definition for the procedure `accumulate`, which joins the first `n` natural numbers (ie. 1 to n, inclusive) according to the following parameters:填写过程“累加”的定义，该过程根据以下参数连接前“n”个自然数（即1到n，包括1到n）：

1. `joiner`: a function of two arguments `joiner`：两个参数的函数

2. `start`: a number with which we start joining `开始`：我们开始加入的数字

3. `n`: the number of natural numbers to join `n`：要加入的自然数

4. `term`: a function of one argument that computes the *n*th term of a sequence

   `term`：一个参数的函数，用于计算序列的第*n*项

For example, we can find the product of all the numbers from 1 to 5 by using the multiplication operator as the `joiner`, and starting our product at 1:例如，我们可以通过使用乘法运算符作为“joiner”，并从1开始计算所有从1到5的数的乘积：

```scheme
scm> (define (identity x) x)
scm> (accumulate * 1 5 identity)  ; 1 * 1 * 2 * 3 * 4 * 5
120
```

We can also find the sum of the squares of the same numbers by using the addition operator as the `joiner` and `square` as the `term`:我们还可以通过使用加法运算符作为“连接符”和使用“平方”作为“项”来求相同数字的平方和：

```scheme
scm> (define (square x) (* x x))
scm> (accumulate + 0 5 square)  ; 0 + 1^2 + 2^2 + 3^2 + 4^2 + 5^2
55
scm> (accumulate + 5 5 square)  ; 5 + 1^2 + 2^2 + 3^2 + 4^2 + 5^2
60
```

You may assume that the `joiner` will always be commutative: i.e. the order of arguments do not matter.您可以假设“joiner”总是可交换的：即，参数的顺序无关紧要。

```scheme
(define (accumulate joiner start n term)
    (if (= n 0) start
        (accumulate 
          joiner 
          (joiner start (term n)) (- n 1) term ; change the start!
        )
    )
)
```

## Q4: No Repeats

Implement `no-repeats`, which takes a list of numbers `lst` as input and returns a list that has all of the unique elements of `lst` in the order that they first appear, but no repeats. For example, `(no-repeats (list 5 4 5 4 2 2))` evaluates to `(5 4 2)`.实现“no repeats”，它将数字列表“lst”作为输入，并返回一个列表，该列表按“lst’的所有唯一元素首次出现的顺序排列，但不重复。例如，“（no repeats（list 5 4 5 4 2 2）”的值为“（5 4 2）”。

> **Hint:** How can you make the first time you see an element in the input list be the first and only time you see the element in the resulting list you return?
>
> **提示：**如何使第一次看到输入列表中的元素成为第一次也是唯一一次在返回的结果列表中看到元素？

> **Hint:** You may find it helpful to use the `my-filter` procedure with a helper `lambda` function to use as a filter. To test if two numbers are equal, use the `=` procedure. To test if two numbers are not equal, use the `not` procedure in combination with `=.`
>
> **提示：**您可能会发现将“我的过滤器”过程与助手“lambda”函数一起用作过滤器很有用。要测试两个数字是否相等，请使用“=”过程。要测试两个数字是否不相等，请将“not”过程与“=”结合使用

# HW9 : Interpreters, Macros

## Interpreters

### Q1: WWSD: Eval and Apply

**补充阅读 : REPL, 在ch3课堂笔记markdown**

1.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116170910863.png" alt="image-20230116170910863" style="zoom:50%;" />

> 选0)   一共6步, + 2 4 6 8各一步 ==还有最初的`Pair(+, Pair(2, Pair(4, Pair(6), Pair(8, nil))))`这个整体的eval==
>
> 另外, apply有一次

==理解原理:==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116171109984.png" alt="image-20230116171109984" style="zoom: 50%;" />

2.`scm> (+ 2 (* 4 (- 6 8))) `

eval 10次, apply 3 次

> **eval 遇到左括号或者基本元素就 +1**

3.

Instead of continuing with the call expression evaluation procedure, we handle the special form with its own set of rules.

```scheme
Expression: (if (= 1 1) #t #f)
Eval:       ------------------
                -------        (note that if is not evaluated)
                 - - -
Apply:          ~~~~~~~        (1 scheme_apply)
Eval:                   --     (6 scheme_eval)
                               (note that #f is not evaluated)
```

We can also trace through how our Scheme interpreter handles another special form, `cond`.

```scheme
Expression: (cond ((= 1 2) #f) (else #t))
Eval:       -----------------------------
                   -------
                    - - -
Apply:             ~~~~~~~               (1 scheme_apply)
Eval:                                --  (6 scheme_eval)
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116175633488.png" alt="image-20230116175633488" style="zoom: 50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116180546878.png" alt="image-20230116180546878" style="zoom:50%;" />

> 一共是6次eval , 整个一次, (+ 1 2) 五次
>
> 一共是1次apply

4.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116180918529.png" alt="image-20230116180918529" style="zoom: 33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116180749341.png" alt="image-20230116180749341" style="zoom: 50%;" />

> 经历1次eval, 0次apply

5.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116181259186.png" alt="image-20230116181259186" style="zoom:50%;" />

> eval了8次
>
> 整个整体==(+1)==, cube(作为procedure)eval==(+1)==, eval出`3`eval了==(+1)==,
>
> 之后apply(cube, 3) ,是lambdaProcedure, scheme_all <img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116184223193.png" alt="image-20230116184223193" style="zoom: 80%;" />
>
> 然后<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116184322501.png" alt="image-20230116184322501" style="zoom:33%;" />然后(* a a a), ==(+5)==
>
> 
>
> apply 了 2 次

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116181618582.png" alt="image-20230116181618582" style="zoom: 40%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116181829104.png" alt="image-20230116181829104" style="zoom:50%;" />

## Macros

### Q2: When Macro

Using macros, define a new special form, `when`, that has the following structure:

```
(when <condition>
      (<expr1> <expr2> <expr3> ...))
```

```scheme
scm> (when (= 1 0) ((/ 1 0) 'error))
okay

scm> (when (= 1 1) ((print 6) (print 1) 'a))
6
1
a
**==相当于传入宏的时候自动就是传入的引用==** '(= 1 1)
```

If the condition is not false (a truthy expression), all the subsequent operands are evaluated in order and the value of the last expression is returned. Otherwise, the entire `when` expression evaluates to `okay`.

> Hint: you may find the [begin](https://cs61a.org/articles/scheme-spec/#begin) form useful.

![image-20230116192427631](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116192427631.png)

**==写宏的要领就是先写一个普通的不是宏的写法, 然后再改写成带宏的写法!!!!!!!!!!!!!==**



To recap, macros are called similarly to regular procedures, but the rules for evaluating them are different. We evaluated **lambda procedures** in the following way:

- Evaluate operator
- Evaluate operands
- Apply operator to operands, evaluating the body of the procedure

However, the rules for evaluating calls to **macro procedures** are:

- Evaluate operator
- Apply operator to **unevaluated operands**
- Evaluate the expression **returned by the macro** in the frame it was called in.

**==相当于传入宏的时候自动就是传入的引用==**

```scheme
(define-macro (when condition exprs)
 `(if ,condition ,(cons 'begin exprs) 'okay) ; , is to evaluate
  )
```

==**,(cons ......) 中的`,`相当于evaluate**==

### Q3: Switch

Define the macro `switch`, which takes in an expression `expr` and a list of pairs, `cases`, where the first element of the pair is some *value* and the second element is a single expression. `switch` will evaluate the expression contained in the list of `cases` that corresponds to the value that `expr` evaluates to.

```scheme
scm> (switch (+ 1 1) ((1 (print 'a))
                      (2 (print 'b))
                      (3 (print 'c))))
b
```

You may assume that the value `expr` evaluates to is always the first element of one of the pairs in `cases`. You can also assume that the first value of each pair in `cases` is a value.

```scheme
(define-macro (switch expr cases)
	(cons _________
		(map (_________ (_________) (cons _________ (cdr case)))
    			cases))
)
(define-macro (switch expr cases)
	(cons 'cond
		(map (lambda (case) (cons `(equal? ,expr (quote ,(car case))) (cdr case)))
    			cases))
)
```

因为要被macro自动eval,所以要用cons改写成易于被eval的代码

**==看不懂,先略了吧!!!(回头再学)==**
