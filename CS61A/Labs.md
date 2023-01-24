# Lab 0 Getting startde

![image-20221223101446551](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223101446551.png)

![image-20221223101458463](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223101458463.png)

docstring:文档字符串

doctest:文档测试



# Lab 1 Functions, Control

## 知识补充

## Using Python

+ Using no command-line options will run the code in the file you provide and return you to the command line. For example, if we want to run `lab00.py` this way, we would write in the terminal:

```
python3 lab00.py
```

- **`-i`**：`-i`选项运行Python脚本，然后打开一个**交互式会话**。在交互式会话中，您逐行运行Python代码并获得即时反馈，而不是同时运行整个文件。要退出，请在解释器提示符中键入“exit（）”。您也可以在Linux/Mac计算机上使用键盘快捷键“Ctrl-D”，或在Windows上使用“Ctrl-Z Enter”。

  下面是我们如何以**交互方式**运行“lab00.py”：

  ```
  python3 -i lab00.py
  ```

- **`-m doctest`**: 

  To run doctests for `lab00.py`, we can run:

  ```python
   python3 -m doctest lab00.py
  ```

更多关于python命令行的知识:[documentation](https://docs.python.org/3.9/using/cmdline.html).

## Using OK(61A开发的)

learn more [here](https://cs61a.org/articles/using-ok/).

**You can quickly generate most ok commands at [ok-help](https://go.cs61a.org/ok-help).**

To use Ok to run doctests for a specified function, run the following command:

```
python3 ok -q <specified function>
```

默认情况下，只有未通过的测试才会显示。您可以使用“-v”选项显示所有测试，包括已通过的测试：

```
python3 ok -v
```

You can also use the **debug printing** feature in OK by writing

```
print("DEBUG:", x)
```

Finally, when you have finished all the questions in [lab01.py](https://cs61a.org/lab/lab01/lab01.py), you must submit the assignment using the `--submit` option:

```p
python3 ok --submit
```



## 零碎知识点

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223204816836.png" alt="image-20221223204816836" style="zoom:50%;" />

==**再次强调, return 是 带 ''(引号) 的,print不带**==

1.<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223205642141.png" alt="image-20221223205642141" style="zoom:33%;" />

这个是无尽循环,==因为只要不是0的数都是True, 不要想成 <0 是False==





## ==debugging==

相关知识的网站(还未阅读):[Debugging | CS 61A Fall 2022](https://cs61a.org/articles/debugging/#traceback-messages)

1.<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223213745650.png" alt="image-20221223213745650" style="zoom: 33%;" />

选2

2.![image-20221223214050026](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223214050026.png)

选3

3.<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223215055528.png" alt="image-20221223215055528" style="zoom:33%;" />

选3, **-i是interactive的意思**

4.<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221223215355813.png" alt="image-20221223215355813" style="zoom:25%;" />

选1

5.

**==It is generally *bad* practice to release code with debugging *print statements* left in==**

**==It is generally *good* practice to release code with *assertion statements* left in==**





# Lab 2 HOF , Lambda expression

## 知识扩充

### lambda expressions(λ表达式)

1.再次明确定义:Lambda expressions are expressions that **==evaluate to functions by specifying two things: the parameters and a return expression.==**

```python
lambda <parameters>: <return expression>
```

While both `lambda` expressions and `def` statements create function objects, there are some notable differences. `lambda` expressions work like other expressions; **much like a mathematical expression** just evaluates to a number and does not alter the current environment, **==a `lambda` expression evaluates to a function without changing the current environment. .==**“  ==**lambda”表达式求值为函数，*而不更改当前环境*。**==

|                           | lambda                                                       | def                                                          |
| :------------------------ | :----------------------------------------------------------- | ------------------------------------------------------------ |
| Type                      | *Expression* that evaluates to a value                       | *Statement* that **alters the environment**                  |
| Result of execution       | Creates an **==anonymous(匿名的)==** lambda function **with no intrinsic(内在的,固有的) name.** | Creates a function **with an intrinsic name** and binds it to that name in the **current environment.** |
| Effect on the environment | Evaluating a `lambda` expression does *not* create(创建) or modify(修改) any variables. | Executing a `def` statement both creates a new function object *and* binds it to a name in the current environment. |
| Usage                     | A `lambda` expression can be used anywhere that expects an expression, such as in an assignment statement or as the **operator(操作符) or operand**(**操作数**) to a call expression. | After executing a `def` statement, the created function is bound to a name. You should use this name to refer to the function anywhere that expects an expression. |

示例:

```python
# A lambda expression by itself does not alter the environment 
lambda x: x * x 

# We can assign lambda functions to a name with an assignment statement 
square = lambda x: x * x # 我们可以给lambda表达式一个名字,但此时它仍然没有内在的名字
square(3)

# Lambda expressions can be used as an operator or operand 
negate = lambda f, x: -f(x) # operator(操作符)
negate(lambda x: x * x, 3) # operand(操作数)
```

(我们可以给lambda表达式一个名字,但此时它仍然没有内在的名字)

### Currying(柯里化)

We can transform multiple-argument functions into a chain of ==**single-argument**==, higher order functions. For example, we can write a function `f(x, y)` as a different function `g(x)(y)`. This is known as **currying**.

For example, to convert the function `add(x, y)` into its curried form:

```python
def curry_add(x):
    def add2(y):
        return x + y
    return add2
```

Calling `curry_add(1)` returns a new function which only performs the addition once the returned function is called with the second addend.该函数仅在使用第二个加数调用返回的函数时执行加法运算。

```python
>>> add_one = curry_add(1)
>>> add_one(2)
3
>>> add_one(4)
5
```





## 具体习题

### Q1 lambda表达式 

1.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221225220042215.png" alt="image-20221225220042215" style="zoom:50%;" />

选2



2.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221225220248337.png" alt="image-20221225220248337" style="zoom:50%;" />



3.一个没有参数输入的lambda表达式

```py
>>> (lambda: 3)()  # Using a lambda expression as an operator in a call exp.
3

等价于 :
    def noPara():
        return 3
```

4.**==嵌套的 lambda : x中的x并没有被绑定,因为他没有参数x(他接受0个参数)==**

```py
>>> b = lambda x: lambda: x  # Lambdas can return other lambdas!
>>> c = b(88)
>>> c

显示出Function # 因为返回的是lambda : x 这个函数(这个函数接受0个参数,返回x)

>>> c()
88 # 这个才是显示出88!!!!!!!!!!!!!!!!!!!!!!!!
```

5.==**lambda表达式也可以接受*函数*作为参数**==

```py
>>> d = lambda f: f(4)  # They can have functions as arguments as well.
>>> def square(x):
...     return x * x
>>> d(square)
16
```

6.==**注意变量的绑定(Z绑定到了环境为Global(最外层)上的那个Z上)**==

```py
>>> z = 3
>>> e = lambda x: lambda y: lambda: x + y + z
>>> e(0)(1)()
4 # 0 + 1 + 3, z = 3(是lambda外面的那个Z)

>>> f = lambda z: x + z
>>> f(3)
Error # 因为即使到了最外层的环境上,也仍然找不到要绑定的是哪个值
```

7.

```py
>>> x = None # remember to review the rules of WWPD given above!
>>> x
Nothing
```

8.**一定要传入可以绑定"对应参数"的"参数"**

```py
>>> higher_order_lambda = lambda f: lambda x: f(x)
>>> g = lambda x: x * x
>>> higher_order_lambda(2)(g)  # Which argument belongs to which function call?
Error # 先应用2,但实际上应该传入的是一个函数(对应于f)

>>> higher_order_lambda(g)(2)
4
```

### Q2 高阶函数 

1.

```py
>>> def even(f):
...     def odd(x):
...         if x < 0:
...             return f(-x)
...         return f(x)
...     return odd

>>> steven = lambda x: x
>>> stewart = even(steven)

>>> stewart
Function
>>> stewart(61)
61
>>> stewart(-4)
4
```

2.

```py
>>> def cake():
...    print('beets')
...    def pie():
...        print('sweets')
...        return 'cake'
...    return pie

>>> chocolate = cake()
beets
>>> chocolate # chocolate是一个pie函数
Function
>>> chocolate()
sweets
'cake' #(当调用一个函数时,返回值也要显示出来)
>>> more_chocolate, more_cake = chocolate(), cake
sweets
>>> more_chocolate # 此时more_chocolate是一个字符串'cake'
'cake'
```

3.(接上题)

```py
>>> def snake(x, y):
...    if cake == more_cake:
...        return chocolate
...    else:
...        return x + y

>>> snake(10, 20)
Function # 返回是一个Function

>>> snake(10, 20)()
sweets
'cake'

>>> cake = 'cake'
>>> snake(10, 20)
30
```



### Q3 lambda函数与柯里化 

1.Write a function `lambda_curry2` that will curry any two argument function using lambdas.编写一个函数“lambda_curry2”，该函数将使用lambdas来约束任何双参数函数。

==**此题很重要**==

```python
def lambda_curry2(func):
    """
    Returns a Curried version of a two-argument function FUNC.
    >>> from operator import add, mul, mod
    >>> curried_add = lambda_curry2(add)
    >>> add_three = curried_add(3)
    >>> add_three(5)
    8
    >>> curried_mul = lambda_curry2(mul)
    >>> mul_5 = curried_mul(5)
    >>> mul_5(42)
    210
    >>> lambda_curry2(mod)(123)(10)
    3
    """
    return lambda arg1: lambda arg2: func(arg1, arg2)
```

==**一定要理解func已经被绑定在了return 的 lambda表达式里面**==

==**最里层的func往外面层层去找对应的,要绑定的,最终在函数lambda_curry2 函数的环境里找到了**==

###  Q4==lambda高阶函数编程练习(重要!)==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221226103618169.png" alt="image-20221226103618169" style="zoom: 33%;" />

这些实现方式看起来非常相似 通过编写一个函数count_cond来概括这个逻辑，该函数接收一个双参数的**谓词(断言)函数condition(n, i)。**count_cond返回一个单参数的函数，该函数接收n，在调用时计算从1到n中所有满足条件的数字。

**Note:** When we say `condition` is a **==predicate([n]谓词,[v]断言) function==** , **we mean that it is a function that will return ==`True` or `False`==based on some specified condition in its body.**

答案:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221226105828814.png" alt="image-20221226105828814" style="zoom:50%;" />

### Q5 环境图练习

```py
n = 7

def f(x):
    n = 8
    return x + 1

def g(x):
    n = 9
    def h():
        return x + 1
    return h

def f(f, x):
    return f(x + n)

f = f(g, n)
g = (lambda y: y())(f)
```

画出上述代码的环境图(environment diagram)

```python
lambda y : y() # == give a function y, and return is call this function
```

具体步骤:

(1)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221226114845162.png" alt="image-20221226114845162" style="zoom: 50%;" />

(2)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221226114946058.png" alt="image-20221226114946058" style="zoom: 50%;" />

(3)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221226115013567.png" alt="image-20221226115013567" style="zoom: 50%;" />

(4)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221226115226736.png" alt="image-20221226115226736" style="zoom:50%;" />

(5)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221226115326256.png" alt="image-20221226115326256" style="zoom:50%;" />

### Q6 Composite Identity Function(复合标识函数)

Write a function that takes in two single-argument functions, `f` and `g`, and returns another **function** that has a single parameter `x`. The returned function should return `True` if `f(g(x))` is equal to `g(f(x))`. You can assume the output of `g(x)` is a valid input for `f` and vice versa(反之亦然). Try to use the `composer` function defined below for more HOF practice.

当 f(g(x)) == g(f(x))时,返回True

```py
def composer(f, g):
    """Return the composition function which given x, computes f(g(x)).

    >>> add_one = lambda x: x + 1        # adds one to x
    >>> square = lambda x: x**2
    >>> a1 = composer(square, add_one)   # (x + 1)^2
    >>> a1(4)
    25
    >>> mul_three = lambda x: x * 3      # multiplies 3 to x
    >>> a2 = composer(mul_three, a1)    # ((x + 1)^2) * 3
    >>> a2(4)
    75
    >>> a2(5)
    108
    """
    return lambda x: f(g(x))

def composite_identity(f, g):
    """
    Return a function with one parameter x that returns True if f(g(x)) is
    equal to g(f(x)). You can assume the result of g(x) is a valid input for f
    and vice versa.

    >>> add_one = lambda x: x + 1        # adds one to x
    >>> square = lambda x: x**2
    >>> b1 = composite_identity(square, add_one)
    >>> b1(0)                            # (0 + 1)^2 == 0^2 + 1
    True
    >>> b1(4)                            # (4 + 1)^2 != 4^2 + 1
    False
    """
    
    def identity(x):
        return composer(f, g)(x) == composer(g, f)(x)
    return identity
```

### Q7 ==I heard you liked functions(嵌套函数) (较困难,关键)==

定义一个函数cycle，它接收三个函数f1、f2、f3作为参数。cycle将返回另一个函数，该函数应接收一个整数参数n并返回另一个函数。最后一个函数应该接收一个参数x，并根据n的大小，循环应用f1、f2和f3到x。下面是在n的几个值中，最终函数应该对x做什么。

n = 0，返回x
		n = 1，对x应用f1，或返回f1(x)
		n = 2，对x应用f1，然后对其结果应用f2，或者返回f2(f1(x))
		n = 3，对x应用f1，对应用f1的结果应用f2，然后对应用f2的结果应用f3，或者f3(f2(f1(x)))
		n = 4，再次开始循环，应用f1，然后是f2，然后是f3，然后又是f1，或者f1(f3(f2(f1(x)))))
		以此类推。	
		提示：大部分工作是在最嵌套的函数中进行的。

```py
def cycle(f1, f2, f3):
    """Returns a function that is itself a higher-order function.

    >>> def add1(x):
    ...     return x + 1
    >>> def times2(x):
    ...     return x * 2
    >>> def add3(x):
    ...     return x + 3
    >>> my_cycle = cycle(add1, times2, add3)
    >>> identity = my_cycle(0)
    >>> identity(5)
    5
    >>> add_one_then_double = my_cycle(2)
    >>> add_one_then_double(1)
    4
    >>> do_all_functions = my_cycle(3)
    >>> do_all_functions(2)
    9
    >>> do_more_than_a_cycle = my_cycle(4)
    >>> do_more_than_a_cycle(2)
    10
    >>> do_two_cycles = my_cycle(6)
    >>> do_two_cycles(1)
    19
    """
	def ret_fn(n):
        def ret(x):
            i = 0
            while i < n:
                if i % 3 == 0:
                    x = f1(x)
                elif i % 3 == 1:
                    x = f2(x)
                else:
                    x = f3(x)
                i += 1
            return x
        return ret
    return ret_fn
```

在上题中,关键是明确ret_fn接受的参数是n,次数,ret接受的参数是x



#  Lab 3 Midterm Review

## Q1

```py
>>> 18117 % 10
7
>>> 18117 // 10
1811
```

我们可以用 % 和 //来实现这种

% 取得这个数的last digit , //取得这个数的除了last digit 外的其他的部分

## Q5 (==再次深入理解HOF==)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227220242095.png" alt="image-20221227220242095" style="zoom:50%;" />

​																**x 未应用表示 这个lambda函数接受一个参数x** 

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227215757374.png" alt="image-20221227215757374" style="zoom:50%;" />

​													**==这种先定义返回的函数,再修改要返回的函数的方法要掌握==**

## Q6 ==丘奇数(Church Numrals) 丘奇编码==

逻辑学家Alonzo Church发明了一个完全**用函数表示非负整数的系统**。其目的是表明函数足以描述所有的数论：如果我们有函数，我们就不需要假设数字的存在，而是可以**==发明==**它们。

Specifically, church numerals (such as zero, one, and two below) are functions that take in a function f and return a new function which, when called, repeats f a number of times on some argument x. 丘奇数是一种通过重复函数应用来表示非负整数的方法。具体来说，丘奇数是一个函数，它接收一个函数f，并返回一个新的函数，当被调用时，**在某个参数x上重复f若干次。**

```py
def zero(f):
    return lambda x: x

def successor(n):
    return lambda f: lambda x: f(n(f)(x))

def one(f):
    """Church numeral 1: same as successor(zero)"""
    return lambda x : f(x)

def two(f):
    """Church numeral 2: same as successor(successor(zero))"""
    return lambda x : f(f(x))
```

1.如何根据zero 和 successor 得出one和two的结果:

```py
successor(zero) = lambda f : lambda x : f(zero(f)(x)) 
				= lambda f : lambda x : f(x) 
one(f) = successor(zero) # accept a function called f, and return the function lambda x : f(x) 

two = successor(one) = lambda f : lambda x : f(one(f)(x))
					 = lambda f : lambda x : f((lambda x : f(x))(x)) 
 					 = lambda f : lambda x : f(f(x))
```

2.==**把丘奇数转换成python里面的自然数(1,2,3,4,5......) :**==

```py
def church_to_int(n):
    """Convert the Church numeral n to a Python integer.
    >>> church_to_int(zero)
    0
    >>> church_to_int(one)
    1
    """
    return n(lambda x : x + 1)(0)
```

这里的 `lambda x : x + 1` 是为了模拟''递增1", 应用(0)是为了从zero开始  



3.**模拟丘奇数的加,乘,乘方**:

```py
def add_church(m, n):
    """Return the Church numeral for m + n, for Church numerals m and n.
    >>> church_to_int(add_church(two, three))
    5
    """
    return lambda f : lambda x : m(f)(n(f)(x)) 
	# 心里想着inc(递增)的原理!!!!!!!!!!!!

def mul_church(m, n):
    """Return the Church numeral for m * n, for Church numerals m and n.
    >>> four = successor(three)
    >>> church_to_int(mul_church(two, three))
    6
    >>> church_to_int(mul_church(three, four))
    12
    """
    return lambda f : m(n(f))   # 递增的原理, 如2 * 3 ,你增1, 我增3
    # 嵌套 -> 递增 return n(m)
    
    
def pow_church(m, n):
    """Return the Church numeral m ** n, for Church numerals m and n.

    >>> church_to_int(pow_church(two, three))
    8
    >>> church_to_int(pow_church(three, two))
    9
    """
    return n(m)
	#如 pow_church(two, three):
    # three(two) :
    # lambda x : two(two(two(x))) 
    # 要two -> three次
```

## Q7 ~ Q8 绘制环境图(environment diagram)

### Q7 Doge(绘制环境图)

```py
wow = 6

def much(wow):
    if much == wow:
        such = lambda wow: 5
        def wow():
            return such
        return wow
    such = lambda wow: 4
    return wow()

wow = much(much(much))(wow)
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227230841725.png" alt="image-20221227230841725" style="zoom: 50%;" />

忽略warning,父对象实际是f1

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227231908020.png" alt="image-20221227231908020" style="zoom:50%;" />

### Q8 绘制环境图(challenge 1 - 2)

#### challenge1 

```py
def funny(joke):
    hoax = joke + 1
    return funny(hoax)

def sad(joke):
    hoax = joke - 1
    return hoax + hoax

funny, sad = sad, funny
result = funny(sad(1))
```

Multiple assignments in a single line: We will first evaluate the expressions on the right of the assignment, and then assign those values to the expressions on the left of the assignment. For example, if we had `x, y = a, b`, the process of evaluating this would be to first evaluate `a` and `b`, and then assign the value of `a` to `x`, and the value of `b` to `y`.

**==单行中的多个赋值：我们将首先计算赋值右侧的表达式，然后将这些值分配给赋值左侧的表达式。例如，如果我们有`x，y=a，b`，那么评估过程将首先评估`a`和`b`，然后将`a`的值赋给`x`，将`b`的值赋值给`y`==**

![image-20221227233417031](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221227233417031.png)



在funny内部，我们有调用funny(hoax)，这将提示我们查找funny作为一个变量所指向的内容。==**在我们的函数框架中没有本地变量被称为funny，所以我们转到全局框架，在那里变量funny指向了函数sad**==。结果，我们将在值hoax上调用sad。

==**重中之重,非常重要**:==

==**在一个函数的帧(frame)里,这个函数本身的名字是不在本地帧中存放的(如上题)**==

#### challenge 2 

注意事项同上题,result = 40 

```py
def double(x):
    return double(x + x)

first = double

def double(y):
    return y + y

result = first(10)
```



# Lab 4 Recursion, Tree Recursion, Python Lists

## Recursion/Tree Recursion

### Q1: WWPD: Squared Virahanka Fibonacci

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221229154221835.png" alt="image-20221229154221835" style="zoom: 33%;" />

再次说明:**==从左到右执行,遇到一个递归就先把它递归到底==**

### Q2 Summation

```py
def summation(n, term):
    """
    >>> summation(5, lambda x: x * x * x) # 1^3 + 2^3 + 3^3 + 4^3 + 5^3
    225
    >>> summation(9, lambda x: x + 1) # 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 10
    54
    >>> summation(5, lambda x: 2**x) # 2^1 + 2^2 + 2^3 + 2^4 + 2^5
    62
    >>> # ban iteration #不能用迭代的方法,也就是不能用for,while
    """
    assert n >= 1
    
    if n == 1:
        return term(n)

    return term(n) + summation(n - 1, term)
```

### Q3: Pascal's Triangle(杨辉三角)

```py
def pascal(row, column):
    """
    >>> pascal(0, 0)    # The top left (the point of the triangle)
    1
    >>> pascal(0, 5)	# Empty entry; outside of Pascal's Triangle
    0
    >>> pascal(4, 2)     # Row 4 (1 4 6 4 1), Column 2
    6
    """
    if column == 0:
        return 1
    elif row == 0:
        return 0
        #当column != 0 而 row == 0是不存在的
    else:
        above_left = pascal(row - 1, column - 1)
        above_right = pascal(row - 1, column)
        return above_left + above_right
```

### Q4: Insect Combinatorics

m * n 的方格,昆虫从左下向右上行走,一次走一格(**向上或向右**)输出一共有多少种路线组合

<img src="https://cs61a.org/lab/lab04/assets/grid.jpg" alt="grid" style="zoom: 67%;" />

### Q6: Double Eights(optional)





## List Comprehensions 列表推导式

### Q5: Couple

==**较困难**==

```py
def couple(s, t): #列表推导式
    """Return a list of two-element lists in which the i-th element is [s[i], t[i]].

    >>> a = [1, 2, 3]
    >>> b = [4, 5, 6]
    >>> couple(a, b)
    [[1, 4], [2, 5], [3, 6]]
    >>> c = ['c', 6]
    >>> d = ['s', '1']
    >>> couple(c, d)
    [['c', 's'], [6, '1']]
    """
    assert len(s) == len(t)
    return [[s[i], t[i]] for i in range(len(s))] 
```

### Q7: Coordinates(optional)

```py
def coords(fn, seq, lower, upper):
    """
    >>> seq = [-4, -2, 0, 1, 3]
    >>> fn = lambda x: x**2
    >>> coords(fn, seq, 1, 9)
    [[-2, 4], [1, 1], [3, 9]]
    
    (x,y)被表示为(x, fn(x)), y必须在[lower, upper]中(左闭右闭)
    """

    return [[x, fn(x)] for x in seq if fn(x) >= lower and fn(x) <= upper]
```

### Q8 Riffle Shuffle(optional)

随性洗牌

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

==**可以在map expression里也用if表达式**==

# Lab 5: Trees, Data Abstraction, Python Lists

## Lists

### Q1: Flatten (压迫,拍平)

```py
>>> lst = [1, [[2], 3], 4, [5, 6]]
>>> flatten(lst)
[1, 2, 3, 4, 5, 6]
```

> **Hint**: ***==you can check if something is a list by using the built-in `type` function==*.** For example:
>
> ```py
> >>> type(3) == list
> False
> >>> type([1, 2, 3]) == list
> True
> ```

==经典题目:把一个深层list变成一个浅层list==

```py
def flatten(s):
    """Returns a flattened version of list s.
    >>> x = [[1, [1, 1]], 1, [1, 1]] # deep list
    >>> flatten(x)
    [1, 1, 1, 1, 1, 1]
    >>> x
    [[1, [1, 1]], 1, [1, 1]]
    """
    "*** YOUR CODE HERE ***"
    #本题是用递归实现
    # 这道题的思路还是"从左到右,一个元素一个元素地依次处理"
    if not s:
      return []
    elif type(s[0]) == list:
      return flatten(s[0]) + flatten(s[1:])
    else:
      return [s[0]] + flatten(s[1:])
```

**==这道题的思路还是"从左到右,一个元素一个元素地依次处理!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!1==**

这道题是看到了思路后才自己写出来的

## Data Abstraction

1.Say we have an abstract data type for cities. A city has a name, a latitude coordinate, and a longitude coordinate.

假设我们有一个城市的抽象数据类型。城市有名称、纬度坐标和经度坐标。

2.Our data abstraction has one **constructor**:

- `make_city(name, lat, lon)`: Creates a city object with the given name, latitude, and longitude.

We also have the following **selectors** in order to get the information for each city:

- `get_name(city)`: Returns the city's name
- `get_lat(city)`: Returns the city's latitude
- `get_lon(city)`: Returns the city's longitude

````py
>>> berkeley = make_city('Berkeley', 122, 37)
>>> get_name(berkeley)
'Berkeley'
>>> get_lat(berkeley)
122
>>> new_york = make_city('New York City', 74, 40)
>>> get_lon(new_york)
40
````

All of the selector and constructor functions can be found in the lab file, if you are curious to see how they are implemented. However, the point of data abstraction is that we do not need to know how an abstract data type is implemented, but rather just how we can interact with and use the data type.所有选择器和构造函数都可以在实验室文件中找到，如果您想了解它们是如何实现的。**==然而，数据抽象的要点是，我们不需要知道抽象数据类型是如何实现的，而只需要知道如何与数据类型交互和使用数据类型。==**

### Q2: Distance

计算两个城市之间的distance,用距离公式

```py
def distance(city_a, city_b):
    """
    >>> city_a = make_city('city_a', 0, 1)
    >>> city_b = make_city('city_b', 0, 2)
    >>> distance(city_a, city_b)
    1.0
    >>> city_c = make_city('city_c', 6.5, 12)
    >>> city_d = make_city('city_d', 2.5, 15)
    >>> distance(city_c, city_d)
    5.0
    """
    "*** YOUR CODE HERE ***"
    a_lat = get_lat(city_a)
    a_lon = get_lon(city_a)
    b_lat = get_lat(city_b)
    b_lon = get_lon(city_b)
    return sqrt(pow(abs(a_lat - b_lat), 2) + pow(abs(a_lon - b_lon), 2))
    # better solution:
    lat_1, lon_1 = get_lat(city_a), get_lon(city_a)
    lat_2, lon_2 = get_lat(city_b), get_lon(city_b)
    return sqrt((lat_1 - lat_2)**2 + (lon_1 - lon_2)**2)
```

### Q3: Closer city

Next, implement `closer_city`, a function that takes a latitude, longitude, and two cities, and returns the name of the city that is relatively closer to the provided latitude and longitude.接下来，实现“closer_city”，这是一个获取纬度、经度和两个城市的函数，并返回相对接近所提供的纬度和经度的城市名称。

**==You may only use the selectors and constructors==** introduced above and the `distance` function you just defined for this question.您只能使用上面介绍的选择器和构造函数以及您刚才为这个问题定义的“distance”函数。

> **Hint**: How can you use your `distance` function to find the distance between the given location and each of the given cities?

```py
def closer_city(lat, lon, city_a, city_b):
    """
    >>> berkeley = make_city('Berkeley', 37.87, 112.26)
    >>> stanford = make_city('Stanford', 34.05, 118.25)
    >>> closer_city(38.33, 121.44, berkeley, stanford)
    'Stanford'
    >>> bucharest = make_city('Bucharest', 44.43, 26.10)
    >>> vienna = make_city('Vienna', 48.20, 16.37)
    >>> closer_city(41.29, 174.78, bucharest, vienna)
    'Bucharest'
    """
    "*** YOUR CODE HERE ***"
    #只用 selector 和 constructor 和上面写的distance函数完成此函数
    city_given = make_city('city_given', lat, lon)
    distance_a = distance(city_a, city_given)
    distance_b = distance(city_b, city_given)
    if distance_a > distance_b:
      return get_name(city_b)
    return get_name(city_a)
```

### Q4: Don't violate the abstraction barrier!

> Note: this question has no code-writing component (if you implemented the previous two questions correctly).
>
> 注意：此问题没有代码编写组件（如果您正确实现了前两个问题）。

> 本question只用来检查你上两个question是不是没有打破抽象界限

When writing functions that use an data abstraction, we should use the constructor(s) and selector(s) whenever possible instead of assuming the data abstraction's implementation. Relying on a data abstraction's underlying implementation is known as *violating the abstraction barrier*, and we never want to do this!在编写使用数据抽象的函数时，我们应该尽可能使用构造函数和选择器，而不是假设数据抽象的实现。依赖数据抽象的底层实现被称为“违反抽象障碍”，我们永远不想这样做！

The nature of the abstraction barrier guarantees that changing the implementation of an data abstraction shouldn't affect the functionality of any programs that use that data abstraction, as long as the constructors and selectors were used properly.抽象障碍的==**本质保证了只要正确使用了构造函数和选择器，更改数据抽象的实现就不会影响使用该数据抽象的任何程序的功能**==

## Trees

### Q5: Finding Berries!

The squirrels on campus need your help! There are a lot of trees on campus and the squirrels would like to know which ones contain berries. Define the function `berry_finder`, which takes in a tree and returns `True` if the tree contains a node with the value `'berry'` and `False` otherwise.校园里的松鼠需要你的帮助！校园里有很多树，松鼠们想知道哪些树含有浆果。定义函数“berryfinder”，该函数接受树，如果树包含值为“berry(浆果)”的节点，则返回“True”，否则返回“False”。

berry:浆果

> **Hint**: To iterate through each of the branches of a particular tree, you can consider using a `for` loop to get each branch.		**提示**：要遍历特定树的每个分支，可以考虑使用“for”循环来获取每个分支。

```py
def berry_finder(t):
    """Returns True if t contains a node with the value 'berry' and 
    False otherwise.
    >>> sproul = tree('roots', [tree('branch1', [tree('leaf'), tree('berry')]), tree('branch2')])
    >>> berry_finder(sproul)
    True
    >>> numbers = tree(1, [tree(2), tree(3, [tree(4), tree(5)]), tree(6, [tree(7)])])
    >>> berry_finder(numbers)
    False
    >>> t = tree(1, [tree('berry',[tree('not berry')])])
    >>> berry_finder(t)
    True
    """
    # 松鼠想要看看这棵tree上有没有'berry'(浆果)
    "*** YOUR CODE HERE ***"
    root = label(t)
    branch = branches(t)
    if root == 'berry':
      return True
    else:
      for bra in branch:
        if berry_finder(bra):
          return True
    return False
    # Alternative solution
    def berry_finder_alt(t):
      if label(t) == 'berry':
          return True
      return True in [berry_finder(b) for b in branches(t)] 
```

### Q6: Sprout leaves 嫩叶

Define a function `sprout_leaves` that takes in a tree, `t`, and a list of leaves, `leaves`. It produces a new tree that is identical to `t`, but where each old leaf node has new branches, one for each leaf in `leaves`.定义一个函数“sprout_tleaves”，它接受树“t”和树叶列表“leaves”。它生成一个与“t”相同的新树，但其中每个旧叶节点都有新的分支，“叶”中的每个叶都有一个分支。

For example, say we have the tree `t = tree(1, [tree(2), tree(3, [tree(4)])])`:

```py
  1
 / \
2   3
    |
    4
```

If we call `sprout_leaves(t, [5, 6])`, the result is the following tree:

```py
       1
     /   \
    2     3
   / \    |
  5   6   4
         / \
        5   6
```

```py
def sprout_leaves(t, leaves):
    """Sprout new leaves containing the data in leaves at each leaf in
    the original tree t and return the resulting tree.
    >>> t1 = tree(1, [tree(2), tree(3)])
    >>> print_tree(t1)
    1
      2
      3
    >>> new1 = sprout_leaves(t1, [4, 5])
    >>> print_tree(new1)
    1
      2
        4
        5
      3
        4
        5
    """
    "*** YOUR CODE HERE ***"
    # root = label(t)
    # branch = branches(t)
    # if is_leaf(root):
    #   return tree(root, leaves)
    # else:
    #   for bra in branch:
    if is_leaf(t):
        return tree(label(t), [tree(leaf_add) for leaf_add in leaves])
    return tree(label(t), [sprout_leaves(s, leaves) for s in branches(t)])
    # 本题的本质还是自上而下逐个去解决问题,看了答案才会
```

**==上面这种列表推导式的递归解法非常重要,一定要学会!!!!!!!!!==**

## Optional Questions

### Q8: Preorder 先序遍历

Define the function `preorder`, which takes in a tree as an argument and returns a list of all the entries in the tree in the order that `print_tree` would print them.定义函数“preorder”，该函数以树为参数，并按“print_tree”打印它们的顺序返回树中所有条目的列表。

The following diagram shows the order that the nodes would get printed, with the arrows representing function calls.下图显示了节点的打印顺序，箭头表示函数调用。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221231221141605.png" alt="image-20221231221141605" style="zoom:25%;" />

> Note: This ordering of the nodes in a tree is called a preorder traversal.
>
> 注意：树中节点的这种排序称为**先序遍历**。

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
      return [label(t)]
    flatted_branches = []
    for child in branches(t):
      flatted_branches += preorder(child)
    return [label(t)] + flatted_branches
    # Alternate solution
    from functools import reduce

    def preorder_alt(t):
      return reduce(add, [preorder_alt(child) for child in branches(t)], [label(t)])
```

==**再次复习reduce!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!**==

```py
>>> def reduce(reduce_fn, s, initial):
        reduced = initial
        for x in s:
            reduced = reduce_fn(reduced, x)
        return reduced
```

### Q9: Add trees

Define the function `add_trees`, which takes in two trees and returns a new tree where each corresponding node from the first tree is added with the node from the second tree. If a node at any particular position is present in one tree but not the other, it should be present in the new tree as well. At each level of the tree, nodes correspond to each other starting from the leftmost node.定义函数“add_trees”，该函数接受两棵树并返回一棵新树，其中第一棵树中的每个对应节点与第二棵树的节点相加。如果某个特定位置的节点出现在一棵树中，而另一棵树不存在，那么它也应该出现在新树中。在树的每个级别，节点从最左侧的节点开始彼此对应。

> *Hint*: You may want to use the built-in zip function to iterate over multiple sequences at once.
>
> *提示*：您可能希望使用内置的zip函数一次迭代多个序列。
>
> *Note*: If you feel that this one's a lot harder than the previous tree problems, that's totally fine! This is a pretty difficult problem, but you can do it! 
>
> *注*：如果你觉得这个问题比以前的树问题要难得多，那就完全好了！这是一个相当困难的问题，但你可以做到！

```py
def add_trees(t1, t2):
    """
    >>> numbers = tree(1,
    ...                [tree(2,
    ...                      [tree(3),
    ...                       tree(4)]),
    ...                 tree(5,
    ...                      [tree(6,
    ...                            [tree(7)]),
    ...                       tree(8)])])
    >>> print_tree(add_trees(numbers, numbers))
    2
      4
        6
        8
      10
        12
          14
        16
    >>> print_tree(add_trees(tree(2), tree(3, [tree(4), tree(5)])))
    5
      4
      5
    >>> print_tree(add_trees(tree(2, [tree(3)]), tree(2, [tree(3), tree(4)])))
    4
      6
      4
    >>> print_tree(add_trees(tree(2, [tree(3, [tree(4), tree(5)])]), \
    tree(2, [tree(3, [tree(4)]), tree(5)])))
    4
      6
        8
        5
      5
    """
    # Base case one (or both) are a leaf
    if is_leaf(t1) or is_leaf(t2):
        # Branches will be an empty list for the tree which is a leaf
        return tree(label(t1) + label(t2), branches(t2) + branches(t1))#处理层次的多出来
    else:
        new_branches = []

        # Recursively call add_trees when both t1 and t2 have a branch
        for i in range(min(len(branches(t1)), len(branches(t2)))):
            new_branches += [add_trees(branches(t1)[i], branches(t2)[i])]
        # Now add the leftover branches to new_branches
        for i in range(min(len(branches(t1)), len(branches(t2))), max(len(branches(t1)), len(branches(t2)))):#处理分支的多出来
            if len(branches(t1)) > len(branches(t2)):
                new_branches += [branches(t1)[i]]
            else:
                new_branches += [branches(t2)[i]]

    return tree(label(t1) + label(t2), new_branches)

# Alternative solution using zip
def add_trees_alternate(t1, t2):
    if not t1:
        return t2
    if not t2:
        return t1
    new_label = label(t1) + label(t2)
    t1_branches, t2_branches = branches(t1), branches(t2)
    length_t1, length_t2 = len(t1_branches), len(t2_branches)
    if length_t1 < length_t2:
        t1_branches += [None for _ in range(length_t1, length_t2)]
    elif length_t1 > length_t2:
        t2_branches += [None for _ in range(length_t2, length_t1)]
    return tree(new_label, [add_trees(branch1, branch2) for branch1, branch2 in zip(t1_branches, t2_branches)])
```

# Lab 6: Mutability, Iterators

## Mutability 突变性

### Q1: WWPD: List-Mutation

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230102170817701.png" alt="image-20230102170817701" style="zoom:50%;" />

**append并不返回什么东西,所以是`Nothing`**

```py
>>> lst = [5, 6, 7, 8]
>>> lst.append(6)
Nothing

>>> lst
[5, 6, 7, 8, 6]

>>> lst.insert(0, 9)
>>> lst
[9, 5, 6, 7, 8, 6]

>>> x = lst.pop(2)
>>> lst
[9, 5, 7, 8, 6]

>>> lst.remove(x)
>>> lst
[9, 5, 7, 8]
```

```py
>>> lst = [1, 2, 3]
>>> lst.extend([4,5])
>>> lst
[1, 2, 3, 4, 5]

>>> lst.extend([lst.append(9), lst.append(10)])
>>> lst
[1,2,3,4,5,9,10,None,None]
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230102172644047.png" alt="image-20230102172644047" style="zoom:67%;" />

关键:**==lst是一个对象(万物皆对象,所以对同一个lst做.append,会产生副作用)==**

lst.append(9),lst.append(10)后

此时lst = [1,2,3,4,5,9,10]

然后lst.extend([None,None])(**==因为append的返回值是None==**)

### Q2: Insert Items

> **Important:** Use list mutation to modify the original list. No new lists should be created or returned.

> **Note:** If the values passed into `entry` and `elem` are equivalent, make sure you're not creating an infinitely long list while iterating through it. If you find that your code is taking more than a few seconds to run, the function may be in an infinite loop of inserting new values.

从前往后查找有没有entry,发现entry后,把elem插入到这个entry后面(可以多次插入)

```py
def insert_items(lst, entry, elem):
    """Inserts elem into lst after each occurrence of entry and then returns lst.

    >>> test_lst = [1, 5, 8, 5, 2, 3]
    >>> new_lst = insert_items(test_lst, 5, 7)
    >>> new_lst
    [1, 5, 7, 8, 5, 7, 2, 3]
    >>> test_lst
    [1, 5, 7, 8, 5, 7, 2, 3]
    >>> double_lst = [1, 2, 1, 2, 3, 3]
    >>> double_lst = insert_items(double_lst, 3, 4)
    >>> double_lst
    [1, 2, 1, 2, 3, 4, 3, 4]
    >>> large_lst = [1, 4, 8]
    >>> large_lst2 = insert_items(large_lst, 4, 4)
    >>> large_lst2
    [1, 4, 4, 8]
    >>> large_lst3 = insert_items(large_lst2, 4, 6)
    >>> large_lst3
    [1, 4, 6, 4, 6, 8]
    >>> large_lst3 is large_lst
    True
    >>> # Ban creating new lists
    >>> from construct_check import check
    >>> check(HW_SOURCE_FILE, 'insert_items',
    ...       ['List', 'ListComp', 'Slice'])
    True
    """
    # 从前往后查找有没有entry,发现entry后,把elem插入到这个entry后面(可以多次插入)
    # 不能新建list,只能利用原有的list在它上面利用mutability(可变性)
    "*** YOUR CODE HERE ***"
    # for index in range(0, len(list)):
    #     if lst[index] == entry:
    #         lst.insert(index + 1, elem)
    #         if 
    
    # list的长度是随时变的,所以不能用for循环:
    # 改为for循环,让index += 2
    index = 0
    while index < len(lst): #这个len(list)是随时在改变的
        if lst[index] == entry:
            lst.insert(index + 1, elem)
            index += 2
        else:
            index += 1
    return lst
```

**==这道题非常重要,因为list是随时在改变的,所以不能用for循环,因为for循环是一个迭代器,当这个迭代器的长度改变时,这个迭代器就失效了,但是while循环不一样,它利用的不是迭代器的原理,而是条件判断,所以while循环当中的`len(lst)`是随时在改变的==**

**==只需要让index加2即可==**

## Iterators

### Q3: WWPD: Iterators

Python's built-in `map`, `filter`, and `zip` functions return **iterators**, not lists. These built-in functions are different from the `my_map` and `my_filter` functions we implemented in Discussion 05.

1.<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230102192245796.png" alt="image-20230102192245796" style="zoom:67%;" />

**==`iter(r)`返回迭代器本身==**

2.<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230102193333853.png" alt="image-20230102193333853" style="zoom:33%;" />

==**列表推导式中的for循环也会消耗迭代器**==

3.<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230102193457523.png" alt="image-20230102193457523" style="zoom:33%;" />

==**生成list时,第一个值是next的值**==

```py
>>> for e in zip([10, 9, 8], range(3)):
...   print(tuple(map(lambda x: x + 2, e)))

(line 1)? (12, 2)
(line 2)? (11, 3)
(line 3)? (10, 4)
-- OK! --
```

**map第一个参数接受一个函数名，后面的参数接受一个或多个可迭代的序列，返回的是一个集合。**

### Q4: Count Occurrences

Implement `count_occurrences`, which takes in an iterator `t` and returns the number of times the value `x` appears in the first `n` elements of `t`. A value appears in a sequence of elements if it is equal to an entry in the sequence.实现“count_occurrences”，它接受迭代器“t”，并返回值“x”在“t”的前n个元素中出现的次数。如果值等于序列中的一个条目，则该值将出现在元素序列中。

> **Note:** You can assume that `t` will have at least `n` elements.
>
> **注意：**您可以假设“t”至少有“n”个元素。

### Q4: Count Occurrences

实现repeated，它接收一个迭代器t，并返回t中连续出现k次的第一个值。

你的实现应该以这样的方式遍历项目，即如果同一个迭代器被传入重复函数两次，它应该在第二次调用中继续在第一次调用中停止的位置。这种行为的一个例子是在doctests中。

# Lab 7: Object-Oriented Programming 面向对象编程

## Q1: WWPD: Classy Cars

![image-20230105163602060](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105163602060.png)

因为没有传入具体的实例Instance

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105163916222.png" alt="image-20230105163916222" style="zoom:67%;" />

也许这个就叫做"多态"

## Q2: Retirement

向Account类添加time_to_trere方法。该方法接受一笔金额，并返回持有人需要等待多少年才能使当前余额增长到至少一个金额，假设银行在每年年底将余额乘以总余额的利率。

略(so easy)

### Q3: FreeChecking

Implement the `FreeChecking` class, which is like the `Account` class from lecture except that it charges a withdraw fee after 2 withdrawals. If a withdrawal is unsuccessful, it still counts towards the number of free withdrawals remaining, but no fee for the withdrawal will be charged.完成“FreeChecking”class，这与讲座中的“Account”class类似，只是*在两次取款后收取取款费*。如果取款不成功，*它仍然计入剩余的免费取款次数，但不会收取取款费用。*

> 提示：不要忘记FreeChecking继承自Account！检查主题中的继承部分以获得复习

```py
class FreeChecking(Account):
    "*** YOUR CODE HERE ***"
    def __init__(self, account_holder):
        super().__init__(account_holder)
        self.withdraws = 0
    
    def withdraw(self, amount):
        self.withdraws += 1
        fee = 0
        if self.withdraws > self.free_withdrawals:
            fee = self.withdraw_fee
        return super().withdraw(amount + fee)
    # Alternative solution where you don't need to include init.
    # Check out the video solution for more.
    def withdraw(self, amount):
        self.free_withdrawals -= 1
        if self.free_withdrawals >= 0:
            return super().withdraw(amount)
        return super().withdraw(amount + self.withdraw_fee)
```

## Magic: the Lambda-ing (Q4 - Q9)

In the next part of this lab, we will be implementing a card game! This game is inspired by the similarly named [Magic: The Gathering](https://en.wikipedia.org/wiki/Magic:_The_Gathering).在本实验室的下一部分，我们将实施一个纸牌游戏！这款游戏的灵感来自同名的[魔法：聚会](https://en.wikipedia.org/wiki/Magic:_The_Gathering).

Once you've implemented the game, you can start it by typing:完成游戏后，您可以通过键入以下内容开始游戏：

```
python3 cardgame.py
```

While playing the game, you can exit it and return to the command line with `Ctrl-C` or `Ctrl-D`.在玩游戏时，您可以退出游戏并使用“Ctrl-C”或“Ctrl-D”返回到命令行。

### Rules of the Game

有两个玩家。每个玩家都有一手牌和一副牌，每轮开始时，每个玩家从自己的牌组中随机抽一张牌。如果玩家在抽牌时卡组是空的，他们将自动输掉游戏。

卡片有一个名字，一个攻击值，和一个防御值。每一轮，每个玩家从自己的手中选择一张牌来玩。然后计算和比较卡牌的力量值。权力大的牌赢得这一轮。每张牌的力量值计算如下。

**(玩家卡的攻击)-(对手卡的防御)**
例如，假设玩家1打出一张2000攻击和1000防御的牌，玩家2打出一张1500攻击和3000防御的牌。他们的牌的力量计算如下。

P1: 2000 - 3000 = 2000 - 3000 = -1000
P2: 1500 - 1000 = 1500 - 1000 = 500
所以玩家2将赢得这一轮。

**第一个赢得8个回合的玩家就赢得了比赛!**

然而，我们可以添加一些效果（在可选问题部分），使这个游戏更加有趣。一张牌可以是AI、Tutor、TA或Instructor的类型，每种类型在被打出时都有不同的效果。请注意，当一张牌被打出时，该牌将从玩家的手牌中移除。这意味着，当效果发生时，这张牌不再在手中。所有的效果都是在该轮计算力量之前应用的。

一张AIC卡可以让你通过抽牌将你牌组的前两张牌加入你的手牌。
辅导卡将把你手中第一张牌的副本加入你的手牌，代价是自动失去当前回合。
TACard会丢弃你手牌中威力最大的牌，并将丢弃的牌的攻击力和防御力加到已出牌的TACard的属性中。
只要教官卡的攻击力或防御力不是负数，它就可以存活多个回合。然而，在它被打出的每个回合开始时，它的攻击和防御会各减少1000。
以后可以随时参考这一系列的规则，让我们开始制作游戏吧!

## Q4: Making Cards

To play a card game, we're going to need to have cards, so let's make some! We're gonna implement the basics of the `Card` class first.

First, implement the `Card` class' constructor in `classes.py`. This constructor takes three arguments:

- a string as the `name` of the card
- an integer as the `attack` value of the card
- an integer as the `defense` value of the card

Each `Card` instance should keep track of these values using instance attributes called `name`, `attack`, and `defense`.

You should also implement the `power` method in `Card`, which takes in another card as an input and calculates the current card's power. Refer to the [Rules of the Game](https://cs61a.org/lab/sol-lab07/#rules-of-the-game) if you'd like a refresher on how power is calculated.

(简单,略)

## Q5: Making a Player

Now that we have cards, we can make a deck, but we still need players to actually use them. We'll now fill in the implementation of the `Player` class.

**deck:牌组**

A `Player` instance has three instance attributes:

- `name` is the player's name. When you play the game, you can enter your name, which will be converted into a string to be passed to the constructor.`name`是玩家的名字。当你玩游戏时，你可以输入你的名字，它会被转换成一个字符串传递给构造函数。
- `deck` is an instance of the `Deck` class. You can draw from it using its `.draw()` method.`deck`是“deck”类的一个实例。您可以使用其`.draw（）`方法从中绘制
- `hand` is a list of `Card` instances. Each player should start with 5 cards in their hand, drawn from their `deck`. Each card in the hand can be selected by its index in the list during the game. When a player draws a new card from the deck, it is added to the end of this list.`hand`是“Card”实例的列表。每位玩家应从自己的“deck”中抽出5张牌开始。在游戏过程中，可以通过列表中的索引选择手中的每张牌。当玩家从牌组中抽到一张新牌时，该牌将被添加到此列表的末尾。

Complete the implementation of the constructor for `Player` so that `self.hand` is set to a list of 5 cards drawn from the player's `deck`.

Next, implement the `draw` and `play` methods in the `Player` class. The `draw` method draws a card from the deck and adds it to the player's hand. The `play` method removes and returns a card from the player's hand at the given index.

> **Hint:** use methods from the `Deck` class wherever possible when attempting to draw from the `deck` when implementing `Player.__init__` and `Player.draw`.
>
> **提示：**在实现“Player”时，尝试从“Deck”绘制时，尽可能使用“Deck”类中的方法__init__和Player.draw。

## Q6: AIs: Resourceful Resources

In the `AICard` class, implement the `effect` method for AIs. An `AICard` will allow you to add the top two cards of your deck to your hand via `draw`ing from your deck.

> Once you have finished writing your code for this problem, set `implemented` to `True` so that the text is printed when playing an `AICard`! *This is specifically for the* `AICard`*!* For future questions, make sure to look at the problem description carefully to know when to reassign any pre-designated variables.

之后省略(节约时间)

# Lab 8: Linked Lists, Mutable Trees

## Mutable Trees

定义`Tree`class

```py
class Tree:
    """
    >>> t = Tree(3, [Tree(2, [Tree(5)]), Tree(4)])
    >>> t.label
    3
    >>> t.branches[0].label
    2
    >>> t.branches[1].is_leaf()
    True
    """
    def __init__(self, label, branches=[]):
        for b in branches:
            assert isinstance(b, Tree)
        self.label = label
        self.branches = list(branches)

    def is_leaf(self):
        return not self.branches
```

Here is a summary of the differences between the tree data abstraction implemented as a functional abstraction vs. implemented as class:

| -                            | Tree constructor and selector functions                      | Tree class                                                   |
| :--------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Constructing a tree          | To construct a tree given a `label` and a list of `branches`, we call `tree(label, branches)` | To construct a tree object given a `label` and a list of `branches`, we call `Tree(label, branches)` (which calls the `Tree.__init__` method). |
| Label and branches           | To get the label or branches of a tree `t`, we call `label(t)` or `branches(t)` respectively | To get the label or branches of a tree `t`, we access the instance attributes `t.label` or `t.branches` respectively. |
| Mutability                   | The functional tree data abstraction is immutable because we cannot assign values to call expressions函数树数据抽象是不可变的，因为我们不能为调用表达式赋值 | The `label` and `branches` attributes of a `Tree` instance can be reassigned, mutating the tree**.==可以重新分配“树”实例的“标签”和“分支”属性，从而改变树。==** |
| Checking if a tree is a leaf | To check whether a tree `t` is a leaf, we call the convenience function `is_leaf(t)` | To check whether a tree `t` is a leaf, we call the bound method `t.is_leaf()`. This method can only be called on `Tree` objects. |

Implementing trees as a class gives us another advantage: we can specify how we want them to be output by the interpreter by implementing the `__repr__` and `__str__` methods.

Here is the `__repr__` method:

```py
def __repr__(self):
    if self.branches:
        branch_str = ', ' + repr(self.branches)
    else:
        branch_str = ''
    return 'Tree({0}{1})'.format(self.label, branch_str)
```

With this implementation of `__repr__`, a `Tree` instance is displayed as the exact constructor call that created it:

```py
>>> t = Tree(4, [Tree(3), Tree(5, [Tree(6)]), Tree(7)])
>>> t
Tree(4, [Tree(3), Tree(5, [Tree(6)]), Tree(7)])
>>> t.branches
[Tree(3), Tree(5, [Tree(6)]), Tree(7)]
>>> t.branches[0]
Tree(3)
>>> t.branches[1]
Tree(5, [Tree(6)])
```

Here is the `__str__` method. You do not need to understand how this function is implemented.

```py
def __str__(self):
    def print_tree(t, indent=0):
        tree_str = '  ' * indent + str(t.label) + "\n"
        for b in t.branches:
            tree_str += print_tree(b, indent + 1)
        return tree_str
    return print_tree(self).rstrip()
```

With this implementation of `__str__`, we can pretty-print a `Tree` to see both its contents and structure:

```py
>>> t = Tree(4, [Tree(3), Tree(5, [Tree(6)]), Tree(7)])
>>> print(t)
4
  3
  5
    6
  7
>>> print(t.branches[0])
3
>>> print(t.branches[1])
5
  6
```

### Q1: WWPD: Linked Lists

```py
>>> link = Link(1000, 2000)
Error # rest必须是Link

>>> link = Link(1000, Link())
Error # 必须有first,这个没给first参数
```

```py
>>> link = Link(5, Link(6, Link(7)))
>>> link                  # Look at the __repr__ method of Link
Link(5, Link(6, Link(7)))
>>> print(link)          # Look at the __str__ method of Link
<5, 6, 7>
```

**==first里的元素可以是list!!!!!!!==**

```py
>>> s.rest = Link(7, Link(Link(8, Link(9))))
>>> s
Link(5, Link(7, Link(Link(8, Link(9)))))
>>> print(s)                             # Prints str(s)
<5 7 <8 9>>
```

### Q2: Convert Link

Write a function `convert_link` that takes in a linked list and returns the sequence as a Python list. You may assume that the input list is shallow; that is none of the elements is another linked list.编写一个函数“convert_link”，它接受一个linked list，并将序列作为Python列表返回。您可以假设输入列表很浅；即没有元素是另一个链接列表。

Try to find both an iterative and recursive solution for this problem!尝试找到这个问题的迭代和递归解决方案！

Challenge: You may NOT assume that the input list is shallow. Hint: use the `type` built-in.挑战：你可能不会认为输入列表很浅。提示：使用内置的“type”。

```py
def convert_link(link):
    """Takes a linked list and returns a Python list with the same elements.

    >>> link = Link(1, Link(2, Link(3, Link(4))))
    >>> convert_link(link)
    [1, 2, 3, 4]
    >>> convert_link(Link.empty)
    []
    """
    "*** YOUR CODE HERE ***"
    if link == Link.empty:
        return []
    else:
        return [link.first] + convert_link(link.rest)
```

### Q3: Duplicate Link 复制链接

Write a function `duplicate_link` that takes in a linked list `link` and a `value`. `duplicate_link` will mutate `link` such that if there is a linked list node that has a `first` equal to `value`, that node will be duplicated. **Note that** you should be mutating the original link list `link`; you will need to create new `Link`s, but you should not be returning a new linked list.

> **Note**: in order to insert a link into a linked list, you need to modify the `.rest` of certain links. We encourage you to draw out a doctest to visualize!
>
> **注意**：要将链接插入链接列表，需要修改某些链接的“.rest”。我们鼓励您绘制一个可视化的doctest！

```py
def duplicate_link(link, val):
    """Mutates `link` such that if there is a linked list
    node that has a first equal to value, that node will
    be duplicated. Note that you should be mutating the
    original link list.

    >>> x = Link(5, Link(4, Link(3)))
    >>> duplicate_link(x, 5)
    >>> x
    Link(5, Link(5, Link(4, Link(3))))
    >>> y = Link(2, Link(4, Link(6, Link(8))))
    >>> duplicate_link(y, 10)
    >>> y
    Link(2, Link(4, Link(6, Link(8))))
    >>> z = Link(1, Link(2, (Link(2, Link(3)))))
    >>> duplicate_link(z, 2) # ensures that back to back links with val are both duplicated
    >>> z
    Link(1, Link(2, Link(2, Link(2, Link(2, Link(3))))))
    """
    "*** YOUR CODE HERE ***"
    if link == Link.empty:
        return
    if link.first == val:
        link.rest = Link(val, link.rest)
        duplicate_link(link.rest.rest, val)
    else:
        duplicate_link(link.rest, val)
```

## Trees

### Q4: WWPD: Trees

> 复习Tree : branches可能有很多子树, Tree(3) 表示一个节点

### Q5: Cumulative Mul 累乘

Write a function `cumulative_mul` that mutates the Tree `t` so that each node's label becomes the product of its label and all labels in the subtrees rooted at the node.编写一个函数“accumulate_mul”，对树“t”进行变异，使每个节点的标签都成为其标签和根在该节点的子树中所有标签的乘积。

> **Hint**: Consider carefully when to do the mutation of the tree and whether that mutation should happen before or after processing the subtrees.
>
> **提示**：仔细考虑何时对树进行突变，以及该突变是==**在处理子树之前还是之后发生**==。

```py
def cumulative_mul(t):
    """Mutates t so that each node's label becomes the product of all labels in
    the corresponding subtree rooted at t.

    >>> t = Tree(1, [Tree(3, [Tree(5)]), Tree(7)])
    >>> cumulative_mul(t)
    >>> t
    Tree(105, [Tree(15, [Tree(5)]), Tree(7)])
    >>> otherTree = Tree(2, [Tree(1, [Tree(3), Tree(4), Tree(5)]), Tree(6, [Tree(7)])])
    >>> cumulative_mul(otherTree)
    >>> otherTree
    Tree(5040, [Tree(60, [Tree(3), Tree(4), Tree(5)]), Tree(42, [Tree(7)])])
    """
    "*** YOUR CODE HERE ***"
    # 把所有子树的lable乘起来,但难点是要从内而外
    if t.is_leaf():
        return 
    for b in t.branches:
        cumulative_mul(b) # 目的是进入到最底层!!!!!!
    # 然后再从最底层往上层做运算
    cumu_mul = t.label
    for b in t.branches:
        cumu_mul *= b.label
    t.label = cumu_mul
```

**==超级重要的思想!!!!!!!!!!!!!!!!!!!!!!!!!!!!!==**

**==关于递归!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!==**

**==先进入最底层(什么也不做),然后再从最顶层网上捯饬!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!==**

### Q6: Every Other (Optional)

> linked list相关

Implement `every_other`, which takes a linked list `s`. It mutates `s` such that all of the odd-indexed elements (using 0-based indexing) are removed from the list. 实现“every_other”，它接受链接列表“s”。它对“s”进行变异，从而从列表中删除所有奇数索引元素（使用基于0的索引）

> Do not return anything! `every_other` should mutate the original list.

```py
def every_other(s):
    """Mutates a linked list so that all the odd-indiced elements are removed
    (using 0-based indexing).

    >>> s = Link(1, Link(2, Link(3, Link(4))))
    >>> every_other(s)
    >>> s
    Link(1, Link(3))
    """
    "*** YOUR CODE HERE ***"
    if s == Link.empty or s.rest == Link.empty:
        return
    s.rest = s.rest.rest
    every_other(s.rest)
```

### Q7: Prune Small

Complete the function `prune_small` that takes in a `Tree` `t` and a number `n` and prunes `t` mutatively. If `t` or any of its branches has more than `n` branches, the `n` branches with the smallest labels should be kept and any other branches should be *pruned*, or removed, from the tree.完成函数“prune_small”，该函数接受一个“Tree”“t”和一个数字“n”，并对“t”进行变异修剪。如果“t”或其任何分支有超过“n”个分支，则应保留带有最小标签的“n”分支，并从树上“修剪”或移除任何其他分支。

```py
def prune_small(t, n):
    """Prune the tree mutatively, keeping only the n branches
    of each node with the smallest labels.

    >>> t1 = Tree(6)
    >>> prune_small(t1, 2)
    >>> t1
    Tree(6)
    >>> t2 = Tree(6, [Tree(3), Tree(4)])
    >>> prune_small(t2, 1)
    >>> t2
    Tree(6, [Tree(3)])
    >>> t3 = Tree(6, [Tree(1), Tree(3, [Tree(1), Tree(2), Tree(3)]), Tree(5, [Tree(3), Tree(4)])])
    >>> prune_small(t3, 2)
    >>> t3
    Tree(6, [Tree(1), Tree(3, [Tree(1), Tree(2)])])
    """

    # while ___________________________:
    #     largest = max(_______________, key=____________________)
    #     _________________________
    # for __ in _____________:
    #     ___________________
    while len(t.branches) > n:
        largest = max(t.branches, key = lambda x : x.label)
        t.branches.remove(largest)
        # 一个一个移去!!!!!!!!!!!!!!
    for branch in t.branches:
        prune_small(branch, n)
```

**==一个一个移去!!!!!==**



# Lab 9: Midterm Review

## Recursion and Tree Recursion

### Q1: Subsequences

A subsequence of a sequence `S` is a subset of elements from `S`, in the same order they appear in `S`. Consider the list `[1, 2, 3]`. Here are a few of it's subsequences `[]`, `[1, 3]`, `[2]`, and `[1, 2, 3]`.序列“S”的子序列是“S”中元素的子集，其出现顺序与“S”相同。考虑列表“[1，2，3]”。下面是它的几个子序列“[]”、“[1，3]”、“[2]”和“[1，2，3]”。

Write a function that takes in a list and returns all possible subsequences of that list. The subsequences should be returned as a list of lists, where each nested list is a subsequence of the original input.编写一个函数，接收一个列表并返回该列表的所有可能的子序列。子序列应该作为列表列表返回，其中每个嵌套列表都是原始输入的子序列。

In order to accomplish this, you might first want to write a function `insert_into_all` that takes an item and a list of lists, adds the item to the beginning of each nested list, and returns the resulting list.为了实现这一点，您可能首先需要编写一个函数“insert_into_all”，该函数接受一个项和一个列表列表，将该项添加到每个嵌套列表的开头，并返回结果列表。

```py
def insert_into_all(item, nested_list):
    """Return a new list consisting of all the lists in nested_list,
    but with item added to the front of each. You can assume that
     nested_list is a list of lists.
     可以假设 nested_list 是一个列表的列表

    >>> nl = [[], [1, 2], [3]]
    >>> insert_into_all(0, nl)
    [[0], [0, 1, 2], [0, 3]]
    """
    "*** YOUR CODE HERE ***"
    for lst in nested_list:
        lst[0:0] = [item] #必须是可迭代的值
        # 写item是不对的,写list(item)也是不对的,list的参数也必须是一个可迭代的值
    return nested_list
    # better solution:
    # return [[item] + l for l in nested_list]

def subseqs(s):
    """Return a nested list (a list of lists) of all subsequences of S.
    The subsequences can appear in any order. You can assume S is a list.

    >>> seqs = subseqs([1, 2, 3])
    >>> sorted(seqs)
    [[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]
    >>> subseqs([])
    [[]]
    """
    if not s: # if len(s) == 0:
        return [s] # return [[]]
    else:
        #      要这个s[0]                               不要s[0]
        return insert_into_all(s[0], subseqs(s[1:])) + subseqs(s[1:])
```

### Q2: Non-Decreasing Subsequences

Just like the last question, we want to write a function that takes a list and returns a list of lists, where each individual list is a subsequence of the original input.就像最后一个问题一样，我们想编写一个函数，它获取一个列表并返回一个列表列表，其中每个列表都是原始输入的子序列。

This time we have another condition: we only want the subsequences for which consecutive elements are *nondecreasing*. For example, `[1, 3, 2]` is a subsequence of `[1, 3, 2, 4]`, but since 2 < 3, this subsequence would *not* be included in our result这一次我们有另一个条件：我们只需要连续元素为“非递减”的子序列。例如，“[1，3，2]”是“[1，3，2，4]”的子序列，但由于2<3，因此该子序列不会包含在我们的结果中。

You may assume that the list passed in as `s` contains only nonnegative elements.您可以假设作为“s”传入的列表只包含非负元素。

**Fill in the blanks** to complete the implementation of the `non_decrease_subseqs` function. You may assume that the input list contains no negative elements.**填写空白**以完成“non_decrease_subseqs”函数的实现。您可以假设输入列表不包含负数元素。

> You may use the provided helper function `insert_into_all`, which takes in an `item` and a list of lists and inserts the `item` to the front of each list.您可以使用提供的助手函数“insert_into_all”，该函数接受一个“item”和一个列表列表，并将“item”插入到每个列表的前面。

**==难难难!!!!!!!!!!!!!!!!!!!!==**

> =="能不能用"与"用不用"==

```py
def non_decrease_subseqs(s):
    """Assuming that S is a list, return a nested list of all subsequences
    of S (a list of lists) for which the elements of the subsequence
    are strictly nondecreasing. The subsequences can appear in any order.

    >>> seqs = non_decrease_subseqs([1, 3, 2])
    >>> sorted(seqs)
    [[], [1], [1, 2], [1, 3], [2], [3]]
    >>> non_decrease_subseqs([])
    [[]]
    >>> seqs2 = non_decrease_subseqs([1, 1, 2])
    >>> sorted(seqs2)
    [[], [1], [1], [1, 1], [1, 1, 2], [1, 2], [1, 2], [2]]
    """
    def subseq_helper(s, prev):
        if not s:
            return [[]]
        elif s[0] < prev: # s[0] can't be used # 可不可用!!!
            return subseq_helper(s[1:], prev)
        else:
            a = subseq_helper(s[1:], s[0]) # use s[0] # 用不用!!!!
            b = subseq_helper(s[1:], prev) # drop s[0]
            return insert_into_all(s[0], a) + b
    return subseq_helper(s, -1)
```

### Q3: Number of Trees

A **full binary tree** is a tree where each node has either 2 branches or 0 branches, but never 1 branch.**全二叉树**是每个节点有2个分支或0个分支，但从未有1个分支的树。

Write a function which returns the number of unique full binary tree structures that have exactly n leaves. See the doctests for visualizations of the possible full binary tree sturctures that have 1, 2, and 3 leaves.编写一个函数，返回正好有n个叶子的唯一全二叉树结构的数量。请参阅doctests，了解具有1、2和3个叶的可能的完整二叉树结构的可视化。

> **Hint**: A full binary tree can be constructed by connecting two smaller full binary trees to a root node. If the two smaller full binary trees have `a` and `b` leaves, the new full binary tree will have `a + b` leaves. For example, as shown in the first diagram below, a full binary tree with 4 leaves can be constructed by connecting a full binary tree that has three leaves (yellow) with a full binary tree that has one leaf (orange). A full binary tree with 4 leaves can also be constructed by connecting two full binary trees with 2 leaves each (second diagram)
>
> **提示**：可以通过将两个较小的完整二进制树连接到根节点来构造完整二进制树。如果两个较小的全二叉树具有“a”和“b”叶，则新的全二元树将具有“a+b”叶。例如，如下图所示，通过将具有三片叶子（黄色）的全二叉树与具有一片叶子（橙色）的全二叉树连接起来，可以构建具有四片叶子的全二元树。也可以通过连接两个全二叉树（每个树有2个叶子）来构建一个具有4个叶子的全二叉树形结构（第二张图）

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110164112139.png" alt="image-20230110164112139" style="zoom:25%;" />

找出恰好有n个叶子的全二叉树有多少种

```py
def num_trees(n):
    """Returns the number of unique full binary trees with exactly n leaves. E.g.,

    1   2        3       3    ...
    *   *        *       *
       / \      / \     / \
      *   *    *   *   *   *
              / \         / \
             *   *       *   *

    >>> num_trees(1)
    1
    >>> num_trees(2)
    1
    >>> num_trees(3)
    2
    >>> num_trees(4)
    5
    >>> num_trees(8)
    429

    """
    "*** YOUR CODE HERE ***"
    if n == 1:
        return 1
    return sum([num_trees(i) * num_trees(n - i) for i in range(1, n)])
    # 不会
```

> **==还不会,以后再来学习==**

之后的题略了(省时间了)

# Lab 10: Scheme

## Q1: WWSD: Combinations



<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110231710506.png" alt="image-20230110231710506" style="zoom:33%;" />

> quotient  是商

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110231732237.png" alt="image-20230110231732237" style="zoom:67%;" />

> ==scheme用''=''而不是''\=\=''来作比较==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110231916959.png" alt="image-20230110231916959" style="zoom:67%;" />

> ==1 是 true,返回这个值!!!(短路),而不是true!!!==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110232155929.png" alt="image-20230110232155929" style="zoom:50%;" />

> ==**define 返回的是name不是值**==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110232329055.png" alt="image-20230110232329055" style="zoom:33%;" />

> print返回的是None(scheme里叫)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110232519817.png" alt="image-20230110232519817" style="zoom:80%;" />

> **==先对每个值进行evaluate!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!==**

## Q2: WWSD: Lists

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110233046205.png" alt="image-20230110233046205" style="zoom:67%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110233229544.png" alt="image-20230110233229544" style="zoom:67%;" />

> ==加引用就是返回本name==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110233446125.png" alt="image-20230110233446125" style="zoom: 50%;" />

> ==用cons时候list外面不用加括号==

### Q3: Over or Under

Define a procedure `over-or-under` which takes in a number `num1` and a number `num2` and returns the following:

- -1 if `num1` is less than `num2`
- 0 if `num1` is equal to `num2`
- 1 if `num1` is greater than `num2`

> Challenge: Implement this in 2 different ways using `if` and `cond`!
>
> 挑战：使用“if”和“cond”以两种不同的方式实现这一点！

```scheme
(define (over-or-under num1 num2) 
    (cond ((> num1 num2) 1)
        ((< num1 num2) -1)  
        ((= num1 num2) 0)
        )
)
```

### Q4: Make Adder

Write the procedure `make-adder` which takes in an initial number, `num`, and then returns a procedure. This returned procedure takes in a number `inc` and returns the result of `num + inc`.编写过程“make adder”，它接受一个初始数“num”，然后返回一个过程。这个返回的过程接受一个数字“inc”，并返回“num+inc”的结果。

> *Hint*: To return a procedure, you can either return a `lambda` expression or `define` another nested procedure. Remember that Scheme will automatically return the last clause in your procedure.*提示*：==要返回过程，可以返回“lambda”表达式或“定义”另一个嵌套过程。**记住Scheme将自动返回程序中的最后一个子句**==。
>
> You can find documentation on the syntax of `lambda` expressions in [the 61A scheme specification!](https://cs61a.org/articles/scheme-spec/#lambda)

```scheme
(define (make-adder num) 
    (lambda (x) (+ x num)) ;外层必须加上括号，不然不会被当成lambda表达式!!!!!!!!!!
    ; define solution:
    (define (adder inc)
      (+ num inc))
    adder
)
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230111001224324.png" alt="image-20230111001224324" style="zoom:50%;" />

> **==如果不加括号会都被当初参数==**
>
> **==外层必须加上括号，不然不会被当成lambda表达式!!!!!!!!!!==**

### Q5: Compose

Write the procedure `composed`, which takes in procedures `f` and `g` and outputs a new procedure. This new procedure takes in a number `x` and outputs the result of calling `f` on `g` of `x`.编写过程“composed”，它接受过程“f”和“g”并输出一个新过程。这个新过程**接受数字“x”(假设只接受一个参数了)**，并输出对“x”的“g”调用“f”的结果。

```scheme
(define (composed f g) 
    (define (comp_f x)
        (f (g x))
        )
    comp_f
)
```

### Q6: Make a List

In this problem you will create the list with the following box-and-pointer diagram:

<img src="https://cs61a.org/lab/lab10/assets/list2.png" alt="linked list" style="zoom: 50%;" />

> Challenge: try to create this list in multiple ways, and using multiple list constructors!

```scheme
(define lst 
    (list (list 1) 2 list(3 4) 5)
)
```

### Q7: List Duplicator(Optional)

Write a Scheme function, `duplicate` that, when given a list, such as `(1 2 3 4)`, duplicates every element in the list (i.e. `(1 1 2 2 3 3 4 4)`).

# Lab 11: Interpreters

## Introduction

In the [Scheme project](https://cs61a.org/proj/scheme/), you'll be implementing a Python interpreter for Scheme.

解释Scheme表达式的过程的一部分是能够**解析(parse)**一个Scheme代码字符串，作为我们对Scheme表达式内部Python表示的输入。由于所有Scheme表达式都是Scheme列表（因此也是链接列表），因此我们使用“Pair”类来表示所有的Scheme表达式，该类的行为类似于链接列表**此类在“pair.py”中定义**

When given an input such as `(+ 1 2)`, there are two main steps we want to take.

解释表达式的第一部分是获取输入并将其分解为每个组件。在我们的示例中，我们希望将“（”、“+”、“1”、“2”和“）”中的每一个都视为一个单独的标记，然后我们可以找出如何表示。这称为**词法分析lexical analysis**，已在“scheme_token.py”中的“tokenize_lines”函数中为您实现。

现在我们已经将输入分解为其组成部分，我们希望将这些Scheme**令牌(Token)**转换为解释器的内部表示。这称为**语法分析syntactic analysis**，发生在“scheme_read”和“read_tail”函数中的“scheme_reader.py”中。

- `(` tells us we are starting a call expression.
- `+` will be the operator, as it's the first element in the call expression.
- `1` is our first operand.
- `2` is our second operand.
- `)` tells us that we are ending the call expression.

主要思想是 , **在我们对操作数进行任何求值或调用运算符等操作之前，我们希望首先识别输入所代表的内容。**

这个实验室的目标是处理解析的各个部分；在这个实验室和项目中，我们专注于Scheme语言，**我们如何设置Scheme解释器的一般想法可以应用于其他语言，例如Python本身！**

## Part 1

### Context

We store tokens ready to be parsed in **`Buffer` instances.** For example, a buffer containing the input `(+ (2 3))` would have the tokens `'('`, `'+'`, `'('`, `2`, `3`, `')'`, and `')'`.

In this part, we will **implement the `Buffer` class.**

**A `Buffer` provides a way of accessing a sequence of tokens across lines.**

Its constructor takes an **iterator, called "the `source`"**, that returns the next line of tokens as a list each time it is queried(查询), until it runs out of lines.

For example, `source` could be defined as follows:

```py
line1 = ['(', '+', 6, 1 ')']      # (+ 6 1)
line2 = ['(', 'quote', 'A', ')']  # (quote A)
line3 = [2, 1, 0]                 # 2 1 0
input_lines = [line1, line2, line3]
source = iter(input_lines)
```

In effect, the `Buffer` concatenates the sequences returned from its source and then supplies the items from them one at a time through its `pop_first` method, calling the `source` for more sequences of items only when needed.实际上，“Buffer”连接从其源返回的序列，然后通过其“pop_first”方法一次一个地提供这些序列中的项目，仅在需要时调用“source”获取更多项目序列。

In addition, `Buffer` provides a `current` method to look at the next item to be supplied, without sequencing past it.此外，“Buffer”提供了一个“current”方法，可以查看要提供的下一个项目，而不必经过它排序。

### Problem 1

> **Important:** Your code for this part should go in `buffer.py`.

本部分的工作是实现“Buffer”类的“current”和“pop_first”方法。

`current` should return **the current token of the current line** we're on in the `Buffer` instance ***without* removing it**. If there are no more tokens in the current line, then `current` should move onto the next valid line, and return the **first** token of *this* line. If there are no more tokens left to return from the entire source (we've reached the end of all input lines), then `current` should return `None` (***this logic is already provided for you in the `except StopIteration` block***).

If we call `current` multiple times in a row, we should get the same result since calls to `current` won't change what token we're returning.如果我们连续多次调用“current”，我们应该得到相同的结果，因为调用“current”不会改变我们返回的令牌。

> You may find `self.index` helpful while implementing these functions, but you are **not required** to reference it in your solution.

> **Hint:** What instance attribute can we use to keep track of where we are in the current line?
>
> **提示：**我们可以使用什么实例属性来跟踪当前行中的位置？

> **Hint:** If we've reached the end of the current line, then `self.more_on_line()` will return `False`. In that case, how do we "reset" our position to the beginning of the next line?
>
> **提示：**如果我们已经到达当前行的末尾，那么“self.more_on_line（）”将返回“False”。在这种情况下，我们如何将位置“重置”到下一行的开头？

`pop_first`应返回“Buffer”实例的当前令牌，并移动到下一个可能的令牌（将在下次调用“pop_first”时返回）。如果没有更多的令牌可以从整个源返回（我们已经到达所有输入行的末尾），那么“pop_first”应该返回“None”。

> **Hint:** Do we need to update anything to move onto the next potential token?
>
> **提示：**我们是否需要更新任何内容才能移动到下一个潜在令牌？

```py
class Buffer:
    """
    >>> buf = Buffer(iter([['(', '+'], [15], [12, ')']]))
    >>> buf.pop_first()
    '('
    >>> buf.pop_first()
    '+'
    >>> buf.current()
    15
    >>> buf.current()   # Calling current twice should not change buf
    15
    >>> buf.pop_first()
    15
    >>> buf.current()
    12
    >>> buf.pop_first()
    12
    >>> buf.pop_first()
    ')'
    >>> buf.pop_first()  # returns None
    """
    def __init__(self, source):
        """
        Initialize a Buffer instance based on the given source.
        """
        self.index = 0
        self.source = source
        self.current_line = ()
        self.current()
    def pop_first(self):
        """Remove the next item from self and return it. If self has
        exhausted its source, returns None."""
        # BEGIN PROBLEM 1
        "*** YOUR CODE HERE ***"
        current = self.current()
        self.index += 1
        return current
        # END PROBLEM 1
    def current(self):
        """Return the current element, or None if none exists."""
        while not self.more_on_line():
            self.index = 0 # added by myself
            try:
                # BEGIN PROBLEM 1
                "*** YOUR CODE HERE ***"
                self.current_line = next(self.source) # 肯定是写可能出现异常的啊
                # END PROBLEM 1
            except StopIteration:
                self.current_line = ()
                return None
        return self.current_line[self.index]
    def more_on_line(self):
        return self.index < len(self.current_line)
```

## Part 2

### Internal Representations

The reader will parse Scheme code into Python values with the following representations:读者将使用以下表示将Scheme代码解析为Python值:

| Input Example  | Scheme Expression Type | Our Internal Representation                         |
| :------------- | :--------------------- | :-------------------------------------------------- |
| `scm> 1`       | Numbers                | Python's built-in `int` and `float` values          |
| `scm> x`       | Symbols                | Python's built-in `string` values                   |
| `scm> #t`      | Booleans (`#t`, `#f`)  | Python's built-in `True`, `False` values            |
| `scm> (+ 2 3)` | Combinations           | Instances of the `Pair` class, defined in `pair.py` |
| `scm> nil`     | `nil`                  | The `nil` object, defined in `pair.py`              |

**When we refer to combinations here, we are referring to both call expressions and special forms**.当我们在这里提到组合时，我们指的是调用表达式和特殊形式。

### Problem 2

> **Important:** Your code for this part should go in `scheme_reader.py`.

Your job in this part is to write the parsing functionality, which consists of two mutually recursive functions: `scheme_read` and `read_tail`. Each function takes in a single `src` parameter, which is a `Buffer` instance.本部分的工作是**编写解析功能，它由两个相互递归的函数组成：“scheme_read”和“read_tail”。每个函数都接受一个“src”参数，这是一个“Buffer”实例。**

- `scheme_read` removes enough tokens from `src` to form a single expression and returns that expression in the correct [internal representation](https://cs61a.org/lab/lab11/#internal-representations).
- `read_tail` expects to read the rest of a list or `Pair`, assuming the open parenthesis(左括号) of that list or `Pair` has already been removed by `scheme_read`. It will read expressions (and thus remove tokens) until the matching closing parenthesis `)` is seen. This list of expressions is returned as a linked list of `Pair` instances.

In short, `scheme_read` returns the next single complete expression in the buffer and `read_tail` returns the rest of a list or `Pair` in the buffer. Both functions mutate the buffer, removing the tokens that have already been processed.简而言之，“scheme_read”返回缓冲区中的下一个完整表达式，“read_tail”返回缓冲中列表或“Pair”的其余部分。这两个函数都会改变缓冲区，删除已处理的令牌。

The behavior of both functions depends on the first token currently in `src`. They should be implemented as follows:这两个函数的行为取决于当前“src”中的第一个标记。应按以下方式实施：

`scheme_read`:

- If the current token is the string `"nil"`, return the `nil` object.
- If the current token is `(`, the expression is a pair or list. Call `read_tail` on the rest of `src` and return its result.
- If the current token is `'`, the rest of the buffer should be processed as a `quote` expression. You will implement this portion in the next problem.
- If the next token is not a delimiter, then it must be a primitive expression (i.e. a number, boolean). Return it. **Provided**
- If none of the above cases apply, raise an error. **Provided**

`read_tail`:

- If there are no more tokens, then the list is missing a close parenthesis and we should raise an error. **Provided**
- If the token is `)`, then we've reached the end of the list or pair. **Remove this token from the buffer** and return the `nil` object.
- If none of the above cases apply, the next token is the operator in a combination. For example, `src` could contain `+ 2 3)`. To parse this:
  1. `scheme_read` the next complete expression in the buffer.
  2. Call `read_tail` to read the rest of the combination until the matching closing parenthesis.
  3. Return the results as a `Pair` instance, where the first element is the next complete expression from (1) and the second element is the rest of the combination from (2).

> **Hint:** Take a look at the doctests for `scheme_read` and `read_tail` to see some examples of the usages for those methods prior to doing the unlocking questions!
>
> **提示：**在执行解锁问题之前，请查看“scheme_read”和“read_tail”的doctest，看看这些方法的用法示例！

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112181016154.png" alt="image-20230112181016154" style="zoom:50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112181143112.png" alt="image-20230112181143112" style="zoom: 33%;" />

### Problem 3

> **Important:** Your code for this part should go in `scheme_reader.py`.

Your task in this problem is to complete the implementation of `scheme_read` by allowing the function to now be able to handle quoted expressions.

In Scheme, quoted expressions such as `'<expr>` are equivalent to `(quote <expr>)`. That means that we need to wrap the expression following `'` (which you can get by recursively calling `scheme_read`) into the `quote` special form, which is a Scheme list (as with all special forms).

In our representation, a `Pair` represents a Scheme list. You should therefore wrap the expression following `'` in a `Pair`.

For example, `'bagel`, or `["'", "bagel"]` after being tokenized, should be represented as `Pair('quote', Pair('bagel', nil))`. `'(1 2)` (or `["'", "(", 1, 2, ")"]`) should be represented as `Pair('quote', Pair(Pair(1, Pair(2, nil)), nil))`.

Use Ok to unlock and test your code:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112193325130.png" alt="image-20230112193325130" style="zoom:33%;" />

> Pair('quote', Pair('x', nil)) 的原因是 Pair 的第二个元素要么是Pair,要么是 nil

# Lab 12: Macros

## Quasiquotation

回想一下，“quote”特殊形式阻止Scheme解释器执行以下表达式。我们发现，**这有助于我们创建复杂的列表，而不是重复调用“cons”或尝试使用“list”来获得正确的结构。**如果我们试图用许多嵌套列表构造复杂的Scheme表达式，这种形式似乎会很有用。

Consider that we rewrite the `twice` macro as follows:

```scheme
(define-macro (twice f)
  '(begin f f))
```

This seems like it would have the same effect, but since the `quote` form prevents any evaluation, ==the resulting expression we create would actually be `(begin f f)`, which is not what we want.==

The **quasiquote** allows us to construct literal lists in a similar way as quote, but also lets us specify if any sub-expression within the list should be evaluated.**准引号**允许我们以与引号类似的方式构造文本列表，但也允许我们指定是否应计算列表中的任何子表达式。

At first glance, the quasiquote (which can be invoked with the backtick ``\` or the `quasiquote` special form) behaves exactly the same as `'` or `quote`. However, using quasiquotes gives you the ability to **unquote** (which can be invoked with the comma `,` or the `unquote` special form). This removes an expression from the quoted context, evaluates it, and places it back in.

By combining quasiquotes and unquoting, we can often save ourselves a lot of trouble when building macro expressions.通过结合准引号和无引号，我们通常可以在构建宏表达式时省去很多麻烦。

Here is how we could use quasiquoting to rewrite our previous example:

```scheme
(define-macro (twice f)
  `(begin ,f ,f))
```

> Important Note: quasiquoting isn't necessarily related to macros, in fact it can be used in any situation where you want to build lists non-recursively and you know the structure ahead of time. **For example, instead of writing `(list x y z)` you can write ``(,x ,y ,z)` for 100% equivalent behavior**	重要提示：准引用不一定与宏相关，事实上，它可以用于任何希望以非递归方式构建列表并且提前了解结构的情况。例如，不必写“（list x y z）”，您可以写“（，x，y，z）”以实现100%的等效行为

## Macros

So far we've been able to define our own procedures in Scheme using the `define` special form. When we call these procedures, we have to follow the rules for evaluating call expressions, which involve evaluating all the operands.

We know that special form expressions do not follow the evaluation rules of call expressions. Instead, each special form has its own rules of evaluation, **which may include not evaluating all the operands.** Wouldn't it be cool if we could define our own special forms where we decide which operands are evaluated? Consider the following example where we attempt to write a function that evaluates a given expression twice:我们知道，特殊形式表达式不遵循调用表达式的求值规则。相反，每个特殊形式都有自己的求值规则，**其中可能包括不求值所有操作数。如果我们可以定义自己的特殊形式来决定计算哪些操作数，这不是很酷吗？**考虑以下示例，我们尝试编写一个函数，该函数对给定表达式求值两次：

```scheme
scm> (define (twice f) (begin f f))
twice
scm> (twice (print 'woof))
woof
```

Since `twice` is a regular procedure, a call to `twice` will follow the same rules of evaluation as regular call expressions; first we evaluate the operator and then we evaluate the operands. That means that `woof` was printed when we evaluated the operand `(print 'woof)`. Inside the body of `twice`, the name `f` is bound to the value `undefined`, so the expression `(begin f f)` does nothing at all!**由于“twice”是一个常规过程，因此对“twice”的调用将遵循与常规调用表达式相同的求值规则；==首先我们计算运算符，然后计算操作数。==这意味着当我们计算操作数“（print-woof）”时，“woof”被打印出来。在“twice”的主体中，名称“f”与值“undefined”绑定，因此表达式“（begin f f）”根本不起作用！**

The problem here is clear: we need to prevent the given expression from evaluating until we're inside the body of the procedure. This is where the `define-macro` special form, which has identical syntax to the regular `define` form, comes in:**==这里的问题很清楚：我们需要防止给定的表达式求值，直到我们进入过程主体。这就是“define macro”特殊形式的所在，它具有与常规“define”形式相同的语法：==**

```scheme
scm> (define-macro (twice f) (list 'begin f f))
twice
```

`define-macro` allows us to define what's known as a `macro`, which is simply a way for us to combine unevaluated input expressions together into another expression. When we call macros, the operands are not evaluated, but rather are treated as Scheme data. This means that any operands that are call expressions or special form expression are treated like lists.`“定义宏”允许我们定义所谓的“宏”，这只是我们将未赋值的输入表达式组合到另一个表达式中的一种方式。当我们调用宏时，不计算操作数，而是将其视为Scheme数据。这意味着任何作为调用表达式或特殊形式表达式的操作数都被视为列表。

If we call `(twice (print 'woof))`, `f` will actually be bound to the list `(print 'woof)` instead of the value `undefined`. Inside the body of `define-macro`, we can insert these expressions into a larger Scheme expression. In our case, we would want a `begin` expression that looks like the following:如果我们调用“（两次（print'woof））”，“f”实际上将绑定到列表“（print'woof）”，而不是值“undefined”。在“define macro”的主体中，我们可以将这些表达式插入到更大的Scheme表达式中。在本例中，我们需要一个如下所示的“begin”表达式：

```scheme
(begin (print 'woof) (print 'woof))
```

As Scheme data, this expression is really just a list containing three elements: `begin` and `(print 'woof)` twice, which is exactly what `(list 'begin f f)` returns. Now, when we call `twice`, this list is evaluated as an expression and `(print 'woof)` is evaluated twice.

```scheme
scm> (twice (print 'woof))
woof
woof
```

To recap, macros are called similarly to regular procedures, but the rules for evaluating them are different. We evaluated lambda procedures in the following way:概括地说，宏的调用与常规过程类似，但计算它们的规则不同。我们以以下方式评估lambda过程：

- **Evaluate operator**
- **Evaluate operands**
- **Apply operator to operands, evaluating the body of the procedure**

**==However, the rules for evaluating calls to macro procedures are:==**

- **==Evaluate operator==**
- **==Apply operator to unevaluated operands==**
- ==**Evaluate the expression returned by the macro in the frame it was called in**==.

## Quasiquotation

### Q1: WWSD: Quasiquote

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116134031595.png" alt="image-20230116134031595" style="zoom: 50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116134407003.png" alt="image-20230116134407003" style="zoom: 50%;" />

> `,1`的结果是1本身

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116134537194.png" alt="image-20230116134537194" style="zoom:67%;" />

> 对 (+ 1 x 3)进行解引用(,)的情况下进行准引用,结果是(+ 1 x 3)这个式子, 结果是6

​																				<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116134746797.png" alt="image-20230116134746797" style="zoom:50%;" /> 

> ==**所有的准引用范围内的`,`都会evaluate**==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116135036078.png" alt="image-20230116135036078" style="zoom:50%;" />

## Macros

### Q2: If Macro Scheme

Use the `define-macro` special form to define a macro, `if-macro`,which will take in the following parameters:

1. `condition` : an expression which will evaluate to the condition in our if expression
2. `if-true` : an expression which will evaluate to the value we want to return if true
3. `if-false` : an expression which will evaluate to the value we want to return if false

and evaluates the result of the if expression passed in.

```scheme
scm> (if-macro (= 0 0) 2 3)
2
scm> (if-macro (= 1 0) (print 3) (print 5))
5
```

> If you're having a hard time with the question revisit Question 3 (If Program Scheme) from [Discussion 11](https://inst.eecs.berkeley.edu/~cs61a/fa22/disc/disc11/#q3). In the past discussion, we had to `'` our arguments, and we had to call `eval` on the return value of `if-function` to actually run the line of code we produced. Because we are using the `define-macro` special form, the operands are no longer quoted, and we no longer have the perform an extra call to `eval`!

```scheme
(define-macro
 (if-macro condition if-true if-false)
 (if (eval condition) if-true if-false) ;we can use eval!!!!!
 ) ;;; 不用macro的版本

(define-macro (or-macro expr1 expr2) 
  `(let ((v1 ,expr1)) 
    (if v1 
      v1 
      ,expr2))
  )
```

> **==eval是内置函数!!!==**

> ==本题不用macro也可以==

### Q3: Or Macro

Implement `or-macro`, which takes in two expressions and `or`'s them together (**applying short-circuiting rules短路原则**). However, do this without using the `or` special form. **You may also assume the name `v1` doesn't appear anywhere outside this macro.** 

The behavior of the `or` macro procedure is specified by the following doctests:

```scheme
scm> (or-macro (print 'bork) (/ 1 0))
bork
scm> (or-macro (= 1 0) (+ 1 2))
3
(define-macro (or-macro expr1 expr2)
    `(let ((v1 ____________))
        (if _____ _____ _____))
)
```

```scheme
(define-macro (or-macro expr1 expr2)
  (let ((v1  (eval expr1)))
     (if v1
         v1
         (eval expr2)))
  )
```

### Q4: Replicate

Implement the `replicate` procedure, which takes in a value `x` and a number `n` and returns a list with `x` repeated `n` times. We've provided some examples for how `replicate` should function here:实现“复制”过程，该过程接受值“x”和数字“n”，并返回重复“x”次的列表。我们在这里提供了一些“复制”应该如何运行的示例：

```scheme
scm> (replicate '(+ 1 2) 3)
((+ 1 2) (+ 1 2) (+ 1 2))

scm> (replicate (+ 1 2) 1)
(3)

scm> (replicate 'hi 0)
()
```

Then, write a macro that takes an expression `expr` and a number `n` and repeats the expression `n` times. For example, `(repeat-n expr 2)` should behave the same as `(twice expr)` from the introduction section of this worksheet. It's possible to pass in a combination as the second argument (e.g. `(+ 1 2)`) as long as it evaluates to a number. Be sure that you evaluate this expression in your macro so that you don't treat it as a list.然后，编写一个使用表达式“expr”和数字“n”的宏，并重复表达式“n”次。例如，“（repeat-n expr 2）”的行为应与本工作表引言部分中的“（twice expr）”相同。可以将一个组合作为第二个参数（例如“（+1 2）”）传递，只要它的计算结果是一个数字。**请确保在宏中计算此表达式，以便不将其视为列表。**

Complete the implementation for `repeat-n` so that its behavior matches the doctests below.完成“repeat-n”的实现，使其行为符合下面的doctest。

```scheme
scm> (repeat-n (print '(resistance is futile)) 3)
(resistance is futile)
(resistance is futile)
(resistance is futile)

scm> (repeat-n (print (+ 3 3)) (+ 1 1))  ; Pass a call expression in as n
6
6
```

> **Hint**: Consider which method to build your list (`list`, `cons`, or `quasiquote`) might help make your implementation simpler.**提示**：考虑构建列表的方法（“list”、“cons”或“quaiquote”）可能有助于简化实现。

```scheme
(define (replicate x n) 
  (if (= n 0) nil
    (cons x (replicate x (- n 1)))
    )
 )

(define-macro (repeat-n expr n) 
  (cons 'begin (replicate expr (eval n)))
 )
```

############################

\# **==核心思想:==**											   #

\# **=="先生成代码, 再执行(evaluate)代码"==**#

############################

### Q5: List Comprehensions

Recall that list comprehensions in Python allow us to create lists out of iterables:回想一下，Python中的列表理解允许我们创建可迭代列表：

```scheme
[<map-expression> for <name> in <iterable> if <conditional-expression>]
```

Define a procedure to implement list comprehensions in Scheme that can create lists out of lists. Specifically, we want a `list-of` program that can be called as follows:定义一个过程以在Scheme中实现列表comprehension，该过程可以从列表中创建列表。具体来说，我们需要一个可以按如下方式调用的“列表”程序：

```scheme
(list-of <map-expression> 'for <name> 'in <list> 'if <conditional-expression>)
```

> Note that the symbols for, in, and if must be quoted when calling list-of so that they will not be evaluated and cause an error.		
>
> **请注意，在调用的列表时，必须引用for、in和if的符号，以便不会对它们求值并导致错误。**

Calling `list-of` will return a new list constructed by doing the following for each element in `<list>`:调用“list of”将返回通过对“＜list＞”中的每个元素执行以下操作构造的新列表：

- Bind `<name>` to the element.
- If `<conditional-expression>` evaluates to a truth-y value, evaluate `<map-expression>` and add it to the result list.如果“<条件表达式>”的计算结果为truth-y值，请计算“<映射表达式>”并将其添加到结果列表中。

Here are some examples:

```scheme
scm> (list-of '(* x x) 'for 'x 'in ''(3 4 5) 'if '(odd? x))
(map (lambda (x) (* x x)) (filter (lambda (x) (odd? x)) (quote (3 4 5))))

scm> (eval (list-of '(* x x) 'for 'x 'in ''(3 4 5) 'if '(odd? x)))
(9 25)

scm> (list-of ''hi 'for 'x 'in ''(1 2 3 4 5 6) 'if '(= (modulo x 3) 0))
(map (lambda (x) (quote hi)) (filter (lambda (x) (= (modulo x 3) 0)) (quote (1 2 3 4 5 6))))

scm> (eval (list-of ''hi 'for 'x 'in ''(1 2 3 4 5 6) 'if '(= (modulo x 3) 0)))
(hi hi)

scm> (eval (list-of '(car e) 'for 'e 'in ''((10) 11 (12) 13 (14 15)) 'if '(list? e)))
(10 12 14)
```

> *Hint:* You may use the built-in `map` and `filter` procedures. Check out the [Scheme Built-ins](https://inst.eecs.berkeley.edu/~cs61a/fa22/articles/scheme-builtins/) reference for more information.
>
> You may also find it helpful to look at the expression returned by list-of in the example above.

**==本题还没有让用macro, 只是让用scheme实现列表生成器,下题才是让用macro==**

```scheme
;;; return the code, not the value
(define
 (list-of map-expr for var in lst if filter-expr) ; the if here is just a parameter
  ; only the fisrt 'list-of' is the name of the procedure 
  `(map (lambda (,var) ,map-expr) (filter (lambda (,var) ,filter-expr) ,lst))
  ; you can look the detail of map and filter in scheme
 )
```

### Q6: List Comprehension Macro

When we wrote out our original `list-of` program in [Question 5](https://inst.eecs.berkeley.edu/~cs61a/fa22/lab/lab12/#q5), we had to do additional work to actually run the outputted program. Particularly, we needed to quote parameters and perform an extra call to `eval` to get the desired answer. To make things easier, we can use the `define-macro` special form to simplify the process.当我们在[问题5]中写下最初的“列表”时(https://inst.eecs.berkeley.edu/~cs61a/fa22/lab/lab12/#q5），我们必须做额外的工作才能实际运行输出的程序。特别是，我们需要引用参数并执行额外的“eval”调用以获得所需的答案。**==为了简化操作，我们可以使用“define macro”特殊形式来简化过程。==**

As a reminder, the `define-macro` form changes the order of evaluation to be:

1. Evaluate operator
2. Apply operator to unevaluated operands
3. Evaluate the expression returned by the macro in the frame it was called in.

Use the `define-macro` special form to implement the `list-of-macro` macro that can be called as follows:

```scheme
(list-of-macro <map-expression> for <name> in <list> if <conditional-expression>)
```

Calling `list-of-macro` will return a new list constructed by doing the following for each element in `<list>`:

- Bind `<name>` to the element.
- If `<conditional-expression>` evaluates to a truth-y value, evaluate `<map-expression>` and add it to the result list.

Here are some examples:

```scheme
scm> (list-of-macro (* x x) for x in '(3 4 5) if (odd? x))
(9 25)

scm> (list-of-macro 'hi for x in '(1 2 3 4 5 6) if (= (modulo x 3) 0))
(hi hi)

scm> (list-of-macro (car e) for e in '((10) 11 (12) 13 (14 15)) if (list? e))
(10 12 14)
```

> *Hint:* Again, you may use the built-in `map` and `filter` procedures. Check out the [Scheme Built-ins](https://inst.eecs.berkeley.edu/~cs61a/fa22/articles/scheme-builtins/) reference for more information.

```scheme
(define-macro (list-of-macro map-expr
                             for
                             var
                             in
                             lst
                             if
                             filter-expr)
    `(map (lambda (,var) ,map-expr) (filter (lambda (,var) ,filter-expr) ,lst))
  )
```

**==不需要做任何修改,复制粘贴上一题的代码即可==**



# Lab 13 SQL 

请见textbook笔记部分(SQL数据库)

