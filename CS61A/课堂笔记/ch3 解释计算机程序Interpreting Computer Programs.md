

​									 **ch 3 Interpreting Computer Programs**

==scheme的比较详细的, 官方的指导:==

**==[CS 61A Scheme Specification | CS 61A Fall 2022](https://cs61a.org/articles/scheme-spec/#about-this-specification)==**



# 3.1 引言

**第一章和第二章描述了编程的==两个基本元素：数据和函数之间的紧密联系==**。我们看到了高阶函数如何将函数当做数据操作。我们也看到了数据可以使用消息传递和对象系统绑定行为。我们已经学到了组织大型程序的技巧，例如函数抽象，数据抽象，类的继承，以及泛用函数。这些核心概念构成了坚实的基础，来构建模块化，可维护和可扩展的程序。

**这一章专注于编程的==第三个基本元素：程序自身==**。Python 程序只是文本的集合。只有通过解释过程，我们才可以基于文本执行任何有意义的计算。类似 Python 的编程语言很实用，因为我们可以定义解释器，它是一个执行 Python 求值和执行过程的程序。把它看做编程中最基本的概念并不夸张。解释器只是另一个程序，它确定编程语言中表达式的意义。

接受这一概念，需要改变我们自己作为程序员的印象。**我们需要将自己看做语言的设计者，而不只是由他人设计的语言用户。**

## 3.1.1 编程语言

程序设计语言在其语法结构、功能和应用领域方面有很大的不同。在通用编程语言中，函数定义和函数应用的结构是普遍存在的。另一方面，也有一些强大的语言不包括对象系统、高阶函数、赋值，甚至不包括控制结构，如while和for语句。作为一个具有最小特征的强大语言的例子，**我们将介绍Scheme编程语言。本文介绍的Scheme的子集完全不允许可变值。**

在本章中，我们将研究解释器的设计以及它们在执行程序时产生的计算过程。为通用编程语言设计解释器的前景可能看起来很艰巨。毕竟，解释器是可以进行任何可能的计算的程序，这取决于其输入。然而，**许多解释器有一个优雅的共同结构：两个相互递归的函数。第一个评估环境中的表达式；第二个将函数应用于参数。**

这些函数是递归的，因为它们是相互定义的：应用一个函数需要评估其主体中的表达式，而评估一个表达式可能涉及应用一个或多个函数。

# 3.2  Functional Programming

scheme的在线解释器(CS61A)	:	[61A Code (cs61a.org)](https://code.cs61a.org/)

## Scheme基础语法与表达式

==Lab 10 中有详细的总结==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110183736556.png" alt="image-20230110183736556" style="zoom: 33%;" />

**Scheme是Lisp的方言**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110183837330.png" alt="image-20230110183837330" style="zoom:33%;" />

**缩进对scheme是不必要的,但是有利于人的阅读**

Scheme是Lisp的一种方言，Lisp是第二古老的编程语言，至今仍被广泛使用（仅次于Fortran）。Lisp程序员社区几十年来一直在蓬勃发展，Lisp的新方言，如Clojure，是所有现代编程语言中发展最快的开发者社区之一。

### if表达式

```scheme
(if <predicate> <consequent> <alternative>)
```

要计算“if”表达式，解释器首先计算表达式的“＜predicate＞”部分。如果“＜predicate＞”求值为真值，则解释器随后求值“＜consequent＞”并**返回其值**。否则，它将计算“<alternate\>”并**返回其值。**

Numerical values can be compared using familiar comparison operators, but prefix notation is used in this case as well:数值可以使用熟悉的比较运算符进行比较，但在这种情况下也使用前缀表示法：

```scheme
(>= 2 1)
#t 
```

==#t是True,#f是False==

> - `(and <e1> ... <en>)` **==一个一个求值,只要有一个是假,就返回假(短路求值),否则返回真==**
> - `(or <e1> ... <en>)` 一个一个求值,只要有一个是真,就返回真(短路求值),否则返回假
> - `(not <e>)` The value of a not expression is `true` when the expression `<e>` evaluates to `false`, and `false` otherwise.

### define functions

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110193027748.png" alt="image-20230110193027748" style="zoom:50%;" />

```scheme
(define (<name> <formal parameters>) <body>) # 定义函数的方式
```

### lambda表达式

**==lambda表达式在scheme里面非常重要!!!!!!!!!!!!!!!==**

```scheme
(lambda (<formal-parameters>) <body>) #lambda表达式的形式

(define (plus4 x) (+ x 4)) 
(define plus4 (lambda (x) (+ x 4))) // 和上边这个是等价的

((lambda (x y z) (+ x y (square z))) 1 2 3)
12
```

lambda表达式和定义函数几乎是一样的,不同的是lambda表达式不会在环境中创建一个名字绑定在这个函数上

### cond 和 begin

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110201822192.png" alt="image-20230110201822192" style="zoom: 33%;" />

> cond是condition的缩写

**begin的作用是"开始接下来几个括号的语句",因为本来那个位置只能容纳一个语句**

**cond类似于条件语句**

### let

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110202438760.png" alt="image-20230110202438760" style="zoom:67%;" />

**==a和b在用完之后就解除绑定(被释放了)了==**

```scheme
(let (临时绑定)计算出结果)
```

### list(linked_list)

```scheme
(cons 1
      (cons 2
            (cons 3
                  (cons 4 nil))))
(1 2 3 4)
```

> nil表示是空链表

```scheme
(define one-through-four (list 1 2 3 4))
(car one-through-four)
1
(cdr one-through-four)
(2 3 4)
(car (cdr one-through-four))
2
```

==`car`表示自己所含的值	`cdr`表示所指向的值==

## 3.2.4  Symbolic Data 符号数据

All the compound data objects we have used so far were constructed ultimately from numbers. One of Scheme's strengths is working with arbitrary symbols as data.到目前为止，我们使用的所有复合数据对象最终都是由数字构成的。**==Scheme的优势之一是将任意符号用作数据。==**

为了操纵符号，我们需要语言中的一个新元素：引用数据对象的能力。假设我们要构造列表“（a b）”。我们不能用“（list a b）”来实现这一点，因为这个表达式构造的是“a”和“b”的值列表，而不是符号本身。在Scheme中，我们通过在符号“a”和“b”前面加上一个引号来引用它们，而不是它们的值：

```scheme
(define a 1)
(define b 2)

(list a b)
(a b)
```

```scheme
(list 'a 'b)
(a b)
```

```scheme
(list 'a b)
(a 2)
```

在==Scheme中，**任何未求值的表达式都被称为*引用***==。这种引用的概念源于一种经典的哲学区分，即一种东西，比如一条狗，它四处奔跑并吠叫，而“狗”这个词是一种用于指定这种东西的语言构造。当我们在引号中使用“狗”时，我们不是特别指某只狗，而是指一个词。在语言中，引用允许我们谈论语言本身，因此在Scheme中：

```scheme
(list 'define 'list)
(define list)
```

引号还允许我们输入复合对象，使用列表的传统打印表示法。

```scheme
(car '(a b c))
a
```

```scheme
(cdr '(a b c))
(b c)
```

完整的Scheme语言包含其他功能，如变异操作、向量和映射。然而，到目前为止，我们介绍的子集提供了一种丰富的函数式编程语言，能够实现我们在本文中讨论的许多思想。

## 内置的处理过程

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110210158095.png" alt="image-20230110210158095" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110213210060.png" alt="image-20230110213210060" style="zoom: 25%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110222557751.png" alt="image-20230110222557751" style="zoom: 50%;" />

==**必须加`**==

```scheme
`(1 2 3 4) 是指整个list
(1 2 3 4)是四个参数!!!
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110223103947.png" alt="image-20230110223103947" style="zoom: 50%;" />

# Lab 10 补充内容 (Scheme)

## Expression

### Primitive Expressions

Just like in Python, atomic, or primitive, expressions in Scheme take a single step to evaluate. **These include numbers, booleans, symbols.**

#### numbers

```scheme
scm> 1234    ; integer
1234
scm> 123.4   ; real number
123.4
```

#### Symbols

Out of these, the symbol type is the only one we didn't encounter in Python. A **symbol** acts a lot like a Python name, but not exactly. Specifically, a symbol in Scheme is also a type of value. On the other hand, in Python, names only serve as expressions; a Python expression can never evaluate to a name.其中，符号类型是我们在Python中唯一没有遇到的类型。**符号**的作用与Python名称非常相似，但并不完全相同。**==具体来说，Scheme中的符号也是一种值==**。另一方面，在Python中，名称仅用作表达式；Python表达式永远不能计算为名称。

```scheme
scm> quotient      ; A name bound to a built-in procedure
#[quotient] ;#[]表示built-in
scm> 'quotient     ; An expression that evaluates to a symbol
quotient
scm> 'hello-world!
hello-world!
```

**==核心就是"未	计	算"==**

#### Booleans

In Scheme, *all* values except the special boolean value `#f` are interpreted as true values (unlike Python, where there are some false-y values like `0`). Our particular version of the Scheme interpreter allows you to write `True` and `False` in place of `#t` and `#f`. This is not standard.**==在Scheme中，除了特殊布尔值“#f”之外的所有*值都被解释为真值==（与Python不同，Python中有一些假y值，如“0”）**。我们特定版本的Scheme解释器允许您编写“True”和“False”来代替“#t”和“#f”。这不是标准。

```scheme
scm> #t
#t
scm> #f
#f
```

### Call Expressions

Like Python, the operator in a Scheme call expression comes before all the operands. Unlike Python, the operator is included within the parentheses and the operands are separated by spaces rather than with commas. However, evaluation of a Scheme call expression follows the exact same rules as in Python:与Python一样，Scheme调用表达式中的运算符位于所有操作数之前。与Python不同，运算符包含在括号中，操作数用空格而不是逗号分隔。然而，Scheme调用表达式的求值遵循与Python中完全相同的规则：

1. Evaluate the operator. It should evaluate to a procedure.评估操作符。它应该评估为一个程序。
2. Evaluate the operands, left to right.从左到右计算操作数。
3. Apply the procedure to the evaluated operands.将该过程应用于计算的操作数。

Here are some examples using built-in procedures:

```scheme
scm> (+ 1 2)
3
scm> (- 10 (/ 6 2))
7
scm> (modulo 35 4)
3
scm> (even? (quotient 45 2))
#t
```

### Special Forms

The operator of a special form expression is a special form. What makes a special form "special" is that they do not follow the three rules of evaluation stated in the previous section. Instead, each special form follows its own special rules for execution, such as short-circuiting before evaluating all the operands.特殊形式表达式的运算符是一种特殊形式。特殊形式之所以“特殊”，是因为它们不遵循上一节所述的三个求值规则。**相反，每个特殊形式都遵循自己的特殊执行规则，例如在计算所有操作数之前进行短路。**

Some examples of special forms that we'll study today are the `if`, `cond`, `define`, and `lambda` forms. Read their corresponding sections below to find out what their rules of evaluation are!我们今天要研究的一些特殊形式的例子是“if”、“cond”、“define”和“lambda”形式。阅读下面相应的章节，了解他们的评估规则！

## Control Structures

### `if` Expressions

“if”特殊形式允许我们基于谓词计算两个表达式中的一个。它接受两个必需的参数和一个**==可选的第三个参数(可以没有else)==**：

```scheme
(if <predicate> <if-true> [if-false])
```

**你能理解为什么这个表达式是一种特殊的形式吗？比较正则调用表达式和“if”表达式之间的规则。有什么不同？**

> Step 2 of evaluating call expressions requires evaluating all of the operands in order. However, an `if` expression will only evaluate two of its operands, the conditional expression and either `<true-result>` or `<false-result>`. Because we don't evaluate all the operands in an `if` expression, it is a special form.计算调用表达式的第2步要求按顺序计算所有操作数。但是，“if”表达式将只计算其两个操作数，即条件表达式和“<true result>”或“<false result>”。**==因为我们不计算“if”表达式中的所有操作数，所以它是一种特殊的形式。==**

Let's compare a Scheme `if` expression with a Python `if` statement:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110225106976.png" alt="image-20230110225106976" style="zoom: 33%;" />

Although the code may look the same, what happens when each block of code is evaluated is actually very different. Specifically, the Scheme expression, given that it is an expression, evaluates to some value. However, the Python `if` statement simply directs the flow of the program.**==尽管代码看起来可能相同，但在计算每个代码块时发生的情况实际上是非常不同的。具体来说，Scheme表达式（假定它是一个表达式）的*计算结果为某个值*!!!!!!!!!。然而，Python“if”语句只是指导程序的流程。==**

Another difference between the two is that it's possible to add more lines of code into the suites of the Python `if` statement, while ==**a Scheme `if` expression expects just a single expression**== for each of the true result and the false result**.两者之间的另一个区别是，可以将更多的代码行添加到Python“if”语句的套件中，而Scheme“if”表达式只能为true结果和false结果中的每一个添加一个表达式。**

One final difference is that in Scheme, you cannot write `elif` cases. If you want to have multiple cases using the `if` expression, you would need multiple branched `if` expressions:**==最后一个区别是，在Scheme中，不能编写“elif”案例。如果要使用“If”表达式进行多个case，则需要多个分支的“If”语句==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110225054433.png" alt="image-20230110225054433" style="zoom:33%;" />

### `cond` Expressions

使用嵌套的“if”表达式似乎不是处理多个情况的非常实用的方法。相反，我们可以使用“cond”特殊形式，这是一种通用条件表达式，类似于Python中的多子句if/elif/else条件表达式**`cond‘接受任意数量的参数，称为*子句(clauses)***。子句被写成包含两个表达式的列表：“（＜p＞＜e＞）”。

```scheme
(cond
    (<p1> <e1>)
    (<p2> <e2>)
    ...
    (<pn> <en>)
    [(else <else-expression>)])
```

As you can see, `cond` is a special form because it does not evaluate its operands in their entirety; the predicates are evaluated separately from their corresponding return expression. In addition, the expression short circuits upon reaching the first predicate that evaluates to a truth-y value, leaving the remaining predicates unevaluated.正如您所看到的，**“cond”是一种特殊的形式，==因为它不计算其全部操作数==**；谓词与它们对应的返回表达式分开计算。此外，**表达式在到达第一个求值为truth-y值的谓词时短路，剩下的谓词未求值。**

The following code is roughly equivalent (see the explanation in the [if expression section](https://cs61a.org/lab/lab10/#if-expressions)):

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110225425406.png" alt="image-20230110225425406" style="zoom:50%;" />

## List

scheme列表与我们在Python中使用的链接列表非常相似。就像链接列表是如何由一系列“链接”对象构成的一样，Scheme列表是由一系列对构成的，**这些对是用构造函数“cons”创建的。**

方案列表要求“cdr”是另一个列表或“nil”，一个空列表。列表在解释器中显示为一系列值（类似于“Link”对象的“__str__”表示）。例如

```scheme
scm> (cons 1 (cons 2 (cons 3 nil)))
(1 2 3)
```

<img src="https://cs61a.org/lab/lab10/assets/list1.png" alt="list" style="zoom:50%;" />

我们可以使用“car”和“cdr”过程从列表中检索值，这两个过程现在的工作方式与Python“Link”的“first”和“rest”属性类似。(好奇这些奇怪的名字来自哪里？[看看它们的词源。](https://en.wikipedia.org/wiki/CAR_and_CDR))<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230110230314116.png" alt="image-20230110230314116" style="zoom: 50%;" />

```scheme
scm> (define a (cons 1 (cons 2 (cons 3 nil))))  ; Assign the list to the name a
a
scm> a
(1 2 3)
scm> (car a)
1
scm> (cdr a)
(2 3)
scm> (car (cdr (cdr a)))
3
```

If you do not pass in a pair or nil as the second argument to `cons`, it will error:如果不将pair或nil作为第二个参数传递给“cons”，则会出错：

```scheme
scm> (cons 1 2)
Error
```

### `list` Procedure

There are a few other ways to create lists. The `list` procedure takes in an arbitrary number of arguments and constructs a list with the values of these arguments:还有其他几种创建列表的方法。**==“list”过程接受任意数量的参数，并用这些参数的值构造一个列表：==**

```scheme
scm> (list 1 2 3)
(1 2 3)
scm> (list 1 (list 2 3) 4)
(1 (2 3) 4)
scm> (list (cons 1 (cons 2 nil)) 3 4)
((1 2) 3 4)
```

Note that all of the operands in this expression are evaluated before being put into the resulting list.==**请注意，此表达式中的所有操作数在放入结果列表之前都会进行求值!!!!!!!!!!!!!!!!!!!!!**==

### Quote Form 引用

We can also use the quote form to create a list, which will construct the exact list that is given. Unlike with the `list` procedure, **==the argument to `'` is *not* evaluated==.**

```scheme
scm> '(1 2 3)
(1 2 3)
scm> '(cons 1 2)           ; Argument to quote is not evaluated
(cons 1 2)
scm> '(1 (2 3 4))
(1 (2 3 4))
```

### Built-In Procedures for Lists

There are a few other built-in procedures in Scheme that are used for lists. Try them out in the interpreter!Scheme中还有一些其他内置过程用于列表。

```scheme
scm> (null? nil)                ; Checks if a value is the empty list
True
scm> (append '(1 2 3) '(4 5 6)) ; Concatenates two lists
(1 2 3 4 5 6)
scm> (length '(1 2 3 4 5))      ; Returns the number of elements in a list
5
```

## Defining Names

The special form `define` is used to define variables and functions in Scheme. There are two versions of the `define` special form. To define variables, we use the `define` form with the following syntax:特殊形式“define”用于定义Scheme中的变量和函数。“define”特殊形式有两种版本。要定义变量，我们使用“define“形式，语法如下：

```scheme
(define <name> <expression>)
```

1.计算此表达式的规则如下

> 对“＜expression＞”求值。
>
> 将其值绑定到当前帧中的“＜name＞”。
>
> ==返回`<name>`!!!!!!!!!!!!==

The second version of `define` is used to define procedures:“define”的第二个版本用于定义过程：

```scheme
(define (<name> <param1> <param2> ...) <body> )
```

1.要计算此表达式，请执行以下操作：

> **使用给定的参数和“＜body＞”创建lambda过程。**
>
> 将过程绑定到当前帧中的“＜name＞”。
>
> 返回`<name>`。

The following two expressions are equivalent:**==以下两个表达式等效：==**

```scheme
scm> (define foo (lambda (x y) (+ x y)))
foo
scm> (define (foo x y) (+ x y))
foo
```

`define` is a special form because its operands are not evaluated at all! For example, `<body>` is not evaluated when a procedure is defined, but rather when it is called. `<name>` and the parameter names are all names that should not be evaluated when executing this `define` expression.	define是一种特殊形式，因为它的操作数根本不计算！例如，“＜body＞”在定义过程时不求值，而是在调用过程时求值<name\>和参数名称都是在执行此“define”表达式时不应计算的名称。

## Lambda Functions

All Scheme procedures are lambda procedures. To create a lambda procedure, we can use the `lambda` special form:**==所有Scheme程序都是lambda程序。要创建lambda过程，我们可以使用“lambda”特殊形式：==**

```scheme
(lambda (<param1> <param2> ...) <body>)
```

This expression will create and return a function with the given parameters and body, but it will not alter the current environment. This is very similar to a `lambda` expression in Python!此表达式将创建并返回具有给定参数和主体的函数，==但不会改变当前环境==。这与Python中的“lambda”表达式非常相似！

```scheme
scm> (lambda (x y) (+ x y))        ; Returns a lambda function, but doesn't assign it to a name
(lambda (x y) (+ x y))
scm> ((lambda (x y) (+ x y)) 3 4)  ; Create and call a lambda function in one line
7
```

一个过程可以采用任意数量的参数。“＜body＞”可以包含多个表达式。Scheme中没有Python“return”语句的等效版本。**函数将只返回正文中最后一个表达式的值。**

==**外层必须加上括号，不然不会被当成lambda表达式!!!!!!!!!!**==

# 3.3 Exceptions 异常

异常是这一节的话题，它为程序的错误处理提供了通用的机制**。产生异常是一种技巧，终止程序正常执行流，发射异常情况产生的信号，并直接返回到用于响应异常情况的程序的封闭部分。**Python 解释器每次在检测到语句或表达式错误时抛出异常。**用户也可以使用==`raise`或`assert`语句==来抛出异常。**

## raising exception

**抛出异常。**==**异常是一个对象实例，它的类直接或间接继承自`BaseException`类**==。第一章引入的`assert`语句产生`AssertionError`类的异常。通常，异常实例可以使用`raise`语句来抛出。`raise`语句的通用形式在 [Python 文档](http://docs.python.org/py3k/reference/simple_stmts.html#raise)中描述。`raise`的最常见的作用是构造异常实例并抛出它。

```py
>>> raise Exception('An error occurred')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
Exception: an error occurred
```

**当异常产生时，当前代码块的语句不会继续执行。除非异常被解决了（下面会描述），解释器会直接返回到“读取-求值-打印”交互式循环中，或者在 Python 以文件参数启动的情况下会完全终止。**此外，**==解释器会打印栈回溯，它是结构化的文本块，描述了执行分支中的一系列嵌套的活动函数，它们是异常产生的位置==。**在上面的例子中，文件名称==`<stdin>`==表示异常由用户在交互式会话中产生，而不是文件中的代码。

## handling exception

**处理异常, 异常可以使用封闭的`try`语句来处理。`try`语句由多个子句组成，第一个子句以`try`开始，剩下的以`except`开始。**

```py
try:
    <try suite>
except <exception class> as <name>:
    <except suite>
...
```

当`try`语句执行时，`<try suite>`总是会立即执行。**`except`子句组只在`<try suite>`执行过程中的异常产生时执行。**每个`except`子句指定了需要处理的异常的特定类。例如，**==如果`<exception class>`是`AssertionError`，那么任何继承自`AssertionError`的类实例都会被处理==**，标识符`<name> `绑定到所产生的异常对象上，但是这个绑定在`<except suite>`之外并不有效。

例如，我们可以使用`try`语句来处理异常，在异常发生时将`x`绑定为`0`。

```py
>>> try:
        x = 1/0
    except ZeroDivisionError as e:
        print('handling a', type(e))
        x = 0
handling a <class 'ZeroDivisionError'>
>>> x
0
```

**==这说明了,产生异常,不一定程序不能正确运行==**

`try`语句能够处理产生在函数体中的异常，函数在`<try suite>`中调用。当异常产生时，控制流会直接跳到最近的`try`语句的能够处理该异常类型的`<except suite>`的主体中。

```py
>>> def invert(x):
        result = 1/x  # Raises a ZeroDivisionError if x is 0
        print('Never printed if x is 0')
        return result
>>> def invert_safe(x):
        try:
            return invert(x)
        except ZeroDivisionError as e:
            return str(e)
>>> invert_safe(2)
Never printed if x is 0
0.5
>>> invert_safe(0)
'division by zero'
```

这个例子表明，`invert`中的`print`表达式永远不会求值，反之，控制流跳到了`handler`中的`except`子句组中。将`ZeroDivisionError e`强制转为字符串会得到由`handler: 'division by zero'`返回的人类可读的字符串。

## 3.4.1 异常对象

异常对象本身就带有属性，例如在`assert`语句中的错误信息，以及有关异常产生处的信息。用户定义的异常类可以携带额外的属性。

在第一章中，我们实现了牛顿法来寻找任何函数的零点。下面的例子定义了一个异常类，无论何时`ValueError`出现，它都返回迭代改进过程中所发现的最佳猜测值。数学错误（`ValueError`的一种）在`sqrt`在负数上调用时产生。这个异常由抛出`IterImproveError`处理，它将牛顿迭代法的最新猜测值储存为参数。

首先，我们定义了新的类，继承自`Exception`。

```py
>>> class IterImproveError(Exception):
        def __init__(self, last_guess):
            self.last_guess = last_guess
```

下面，我们定义了`IterImprove`，我们的通用迭代改进算法的一个版本。**这个版本通过抛出`IterImproveError`异常，储存最新的猜测值来处理任何`ValueError`**。像之前一样，`iter_improve`接受两个函数作为参数，每个函数都接受单一的数值参数。`update`函数返回新的猜测值，而`done`函数返回布尔值，表明改进是否收敛到了正确的值。

```py
>>> def iter_improve(update, done, guess=1, max_updates=1000):
        k = 0
        try:
            while not done(guess) and k < max_updates:
                guess = update(guess)
                k = k + 1
            return guess
        except ValueError:
            raise IterImproveError(guess)
```

最后，我们定义了`find_root`，它返回`iter_improve`的结果。`iter_improve`应用于由`newton_update`返回的牛顿更新函数。`newton_update`定义在第一章，在这个例子中无需任何改变。`find_root`的这个版本通过返回它的最后一个猜测之来处理`IterImproveError`。

```py
>>> def find_root(f, guess=1):
        def done(x):
            return f(x) == 0
        try:
            return iter_improve(newton_update(f), done, guess)
        except IterImproveError as e:
            return e.last_guess
```

考虑使用`find_root`来寻找`2 * x ** 2 + sqrt(x)`的零点。这个函数的一个零点是`0`，但是在任何负数上求解它会产生`ValueError`。我们第一章的牛顿法实现会产生异常，并且不能返回任何零点的猜测值。我们的修订版实现在错误之前返回了最新的猜测值。

```py
>>> from math import sqrt
>>> find_root(lambda x: 2*x*x + sqrt(x))
-0.030211203830201594
```

虽然这个近似值仍旧距离正确的答案`0`很远，一些应用更倾向于这个近似值而不是`ValueError`。

异常是另一个技巧，帮助我们将程序细节划分为模块化的部分。在这个例子中，Python 的异常机制允许我们分离迭代改进的逻辑，它在`try`子句组中没有发生改变，以及错误处理的逻辑，它出现在`except`子句中。我们也会发现，异常在使用 Python 实现解释器时是个非常实用的特性。



# 3.4 Interpreters for Languages with Combination

机器语言->高阶语言

**==元语言抽象(*Metalinguistic abstraction*)==** -- 建立了新的语言 -- 并在所有工程设计分支中起到重要作用。它对于计算机编程尤其重要，因为我们不仅仅可以在编程中构想出新的语言，我们也能够通过构建解释器来实现它们。**编程语言的解释器(Interpreter)是一个函数，它在语言的表达式上调用，执行求解表达式所需的操作。**

我们将首先为一种语言定义解释器，这种语言是Scheme的一个有限的子集，称为**Calculator**。然后，我们将为整个Scheme开发一个解释器的草图。我们创建的解释器将是完整的，因为它将使我们能够用Scheme编写完全通用的程序。为此，它将实现我们在第1章中为Python程序介绍的同样的评估环境模型。

## 3.4.1  A Scheme-Syntax Calculator scheme句法计算器

1. `+` 接受任意的参数,把它们都加起来
2. `-`如果接受一个参数,就返回它的负数,如果接受多个参数,就让第一个参数减去剩下的所有参数
3. `/`和`-`类似,如果是一个参数,就返回1 / n,多个参数就第一个参数除以剩下的所有参数

```scheme
> (/ 10)
0.1
> (+)
0
> (*)
1
```

## 3.4.2   Expression Trees 表达式树

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112112908938.png" alt="image-20230112112908938" style="zoom:50%;" />

|    Lexical analysis : 词法分析    | Syntactic analysis : 语法分析 |
| :-------------------------------: | :---------------------------: |
|            迭代的过程             |         树递归的过程          |
| 检查是否存在格式错误的令牌(Token) |           匹配括号            |
|       确定令牌(Token)的类型       |          返回树结构           |
|           一次处理一行            |         一次处理一行          |

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112114353333.png" alt="image-20230112114353333" style="zoom: 33%;" />

> base case 是symbols and numbers

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112115325338.png" alt="image-20230112115325338" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112115330615.png" alt="image-20230112115330615" style="zoom:33%;" />

==好的交互式循环在遇到异常时不会退出,而是把异常是什么print出来,**在当前环境下**让user继续测试==



# 3.5  Interpreters for Languages with Abstraction

We now turn to the task of defining a general programming language that supports abstraction by **binding names to values** and **defining new operations.**

> 这一节的具体实现在`project`之中

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112152700969.png" alt="image-20230112152700969" style="zoom: 33%;" />



# ==补充 : 动态和静态的作用域==

=="**μ表达式**"==

![image-20230113172052234](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113172052234.png)

>==**Lexical scope**: the parent of a frame is the environment in which a procedure was **defined**==
>
>==**Dynamic scope**: the parent of a frame is the environment in which a procedure was **called**==

# 补充:尾递归

## 函数式编程的特点和优势

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114155805486.png" alt="image-20230114155805486" style="zoom: 25%;" />

> 特点: 
>
> 1.所有的函数都是纯函数
>
> 2.没有重新赋值和可变数据类型
>
> 3.名字和值的绑定是永久性的
>
> 优势:
>
> 1.一个表达式的值不依赖于子表达式的evaluate的顺序
>
> 2.子表达式可以安全地==**并行计算**==，也可以仅按需==**(惰性)计算**==
>
> 3.引用透明(Referential transparency) : 当我们将其子表达式之一替换为该子表达式的值时，表达式的值不会改变

### 引用透明 (补充)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114160810356.png" alt="image-20230114160810356" style="zoom: 25%;" />

但是,没有了while , for表达式,只依靠递归,效率能高吗? 

答案是可以的,为此, 我们引入尾递归的概念

## 尾递归引入

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114161221499.png" alt="image-20230114161221499" style="zoom:15%;" />

> 在这个例子中,递归和迭代都是线性时间,但是空间复杂度闪上不一样,如何弥补这zhezhijian之间的差距?

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114161433107.png" alt="image-20230114161433107" style="zoom:33%;" />

==**Scheme的实现需要正确的尾部递归。这允许在恒定空间中执行迭代计算，即使计算由语法递归过程描述。**==

> 如何实现的? 通过	删除不必要的帧

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114161952071.png" alt="image-20230114161952071" style="zoom:15%;" />

这些中间的`Return value`都是240,所以没必要保留n和k,和这个帧了,python中并不会删去这些帧,但是在scheme中会删去,实现尾递归.

## 尾调用 Tail Calls

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114162518986.png" alt="image-20230114162518986" style="zoom:33%;" />

+ ==**lambda表达式的最后一个子表达式**==
+ ==**2 / 3长度的if表达式的子表达式**==
+ 尾部上下文cond中的所有非谓语子表达式
+ 尾部上下文中的最后一个子表达式和、或begin或let

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114163313052.png" alt="image-20230114163313052" style="zoom: 50%;" />

==**A call expression is not a tail call if more computation is still required  in the calling procedure 如果调用过程中仍需要更多计算，则调用表达式不是尾部调用**==

> **==因为 `+ 1`还没有进行(即不知道用什么去`+ 1`, 所以不能删去这个帧 , 要保留)==**
>
> **==而新定义的length-iter就不需要等待什么去计算(在最后一行,该计算的都计算了)==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114172024210.png" alt="image-20230114172024210" style="zoom:50%;" />

### 尾递归练习

找出下面四个哪个是尾递归

![image-20230114192801448](C:\Users\weiziheng\Desktop\image-20230114192801448.png)

+ length()不是尾递归 : 虚线框住的语句在tail context里, 但整个if语句不在tail context(尾递归)中

  所以小红框里的if子表达式也不是在tail context里(必须从外到里都是tail context才行)

+ contains() 是尾递归

+ fib() 不是尾递归, 最下面的语句在尾上下文里, 但红框里的代码不在tail context里

+ has-repeat是尾递归

## Example : Reduce and Map

![image-20230114195200964](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114195200964.png)

> 本身是一个tail context , 但是==是不是常数空间取决于所传入的过程(Procedure)是不是常数空间的函数==

![image-20230114195906968](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114195906968.png)

> 可以改成右边的尾递归的版本

## General Computing machine

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114201210403.png" alt="image-20230114201210403" style="zoom: 18%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114201236390.png" alt="image-20230114201236390" style="zoom:25%;" />



# 补充 :  Programs as Data 

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230114201655526.png" alt="image-20230114201655526" style="zoom: 25%;" />



## generate programs

你可以用scheme, lisp产生program

### quasiquotation 准引用

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116101309127.png" alt="image-20230116101309127" style="zoom: 25%;" />

> `(反引号)是准引用 ,(逗号)是解引用

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116101926088.png" alt="image-20230116101926088" style="zoom:25%;" />

使人们对lisp和scheme如此着迷的原因之一就是人们可以编写代码**从而生成所需的代码**

# 补充 : Macros 宏

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116102652884.png" alt="image-20230116102652884" style="zoom: 50%;" />

> 以上是回顾`与 ,

![image-20230116104351659](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116104351659.png)

**宏是在==求值之前==对程序==源代码==执行的操作**

宏存在于多种语言中，但在Lisp等语言中最容易正确定义

**Scheme有一个定义宏特殊形式，用于定义源代码转换**

宏调用表达式的求值过程：

+ 求值运算符子表达式，该表达式求值为宏
+ 对操作数表达式调用宏过程，==**而不首先对其求值**==
+ ==evaluate==从宏过程返回的表达式

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116110330432.png" alt="image-20230116110330432" style="zoom: 33%;" />

有两点变化: 

+ 不用对print 2 引用(')了
+ ==**替换和eval是同时(但有先后)进行的**==

############################

\# **==核心思想:==**											   #

\# **=="先生成代码, 再执行(evaluate)代码"==**#

############################

## example

![image-20230116111323011](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116111323011.png)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116111331397.png" alt="image-20230116111331397" style="zoom: 50%;" />

==passed是双引号的原因是,要先解引用到**'生成代码'**, 再eval的时候,也是"passed"这个单词==

## 用macro编写(模拟)for语句

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116112205493.png" alt="image-20230116112205493" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116112209812.png" alt="image-20230116112209812" style="zoom: 33%;" />

## 用macro编写trace

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116112747369.png" alt="image-20230116112747369" style="zoom:50%;" />

在scheme中编写trace比在python中更灵活, 因为可以随时决定是否被trace

## 编写宏的要领

![image-20230116192427631](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116192427631.png)

**==写宏的要领就是先写一个普通的不是宏的写法, 然后再改写成带宏的写法!!!!==**



**==相当于传入宏的时候自动就是传入的引用==**





# 补充 : REPL (Interpreter 是 如何运行的) 

The interpreters we study in this course are designed around a **Read-Eval-Print-Loop**.

The interpreter waits for our input, looping until we type in a string of characters, `(+ 1 2)`.

1. In the **Read** stage, we take the string of characters and convert those characters into data structures that can be understood by the interpreter. In the Scheme interpreter, we'd like to take the string of characters `(+ 1 2)` **and convert it into a `Pair` structure, `Pair('+', Pair(1, Pair(2, nil)))`.**
   - The **==Lexer(词法分析器)==** takes the string `(+ 1 2)` and **splits into meaningful *==tokens==* wherever there is whitespace or a syntax character like the open parenthesis `(`.** The lexer returns a structure that kind of resembles a list of individual characters like `['(', '+', 1, 2, ')']`.
   - The **==Parser(语法分析器)==** takes the list of smaller strings and converts them into a data structure that understands the nesting in the language. It's not too tricky to convert the example above from `['(', '+', '1', '2', ')']` to `Pair('+', Pair(1, Pair(2, nil)))`. **But the pair structure is much more useful when we want to work on a more nested example like `(= (+ 1 2) 3)`** which the lexer converts into the flat list `['(', '=', '(', '+', 1, 2, ')', 3, ')']`. As the expressions get more and more complicated, using a nested data structure helps the computer make sense of the expression **in chunks**, much like how we humans make sense of a Scheme expression as well.
2. In the **Eval** stage, we evaluate the nested data structure to a value.
   - If the expression is simple, like a number or a boolean, we can just return the number or boolean itself without any further evaluation. Evaluating 1 in the global frame returns the number `1`. If we pass in a name like 'x', we'll try to evaluate the name 'x' in the current environment, looking up through the parent frames as necessary.
   - If we pass a call expression like `Pair('+', Pair(1, Pair(2, nil)))` into the evaluator, we'll follow the call expression evaluation rules to determine the value of the value of the expression. We'll evaluate the operator, `'+'`, to the built-in procedure which adds values, and each operand, 1 and 2, before applying the procedure to the arguments.
   - if the expression is a special form like `Pair('define', Pair('x', Pair(1, nil)))` (after parsing `(define x 1)`), then we'll follow the special form evaluation rules defined in the Scheme interpreter. In this example, we don't need to evaluate the name 'x' (it's just a name!) but we do need to evaluate the value, the number `1`.
3. In the **Print** stage, we take the value we determined in the evaluation stage and figure out how to display the value to the user. Even if an expression evaluates to a number, the computer actually stores that number in its memory as a bunch of 0 and 1 binary digits! We take that number and convert it back to a more readable representation like '3'!
4. Once we've displayed the value, we can wait in a **Loop** until the user asks another question to the interpreter.

There are some special rules in the parser to handle scenarios like the quote operator `'`. Run `python3 scheme_reader.py` to explore its behavior.

```
read> '1
str : (quote 1)
repr: Pair('quote', Pair(1, nil))
read> '(1 2 3)
str : (quote (1 2 3))
repr: Pair('quote', Pair(Pair(1, Pair(2, Pair(3, nil))), nil))
```

Likewise, the evaluator also needs to handle the behavior of special forms differently from standard call expressions. The [Scheme Specification](https://cs61a.org/articles/scheme-spec/#special-forms) contains more information, and the special form evaluation rules will also be briefly described by example here.





## Counting Calls

了解Scheme解释器中发生的情况的一种方法是计算计算表达式时对“Scheme_eval”的调用次数。

像数字、布尔值、字符串、符号等原子数据类型只需一步即可计算。像名称这样的基元表达式也可以在一个步骤中计算出一个值。

```scheme
Expression: 1
Eval:       -       (1 scheme_eval)

Expression: #t
Eval:       --      (1 scheme_eval)

Expression: 'cs61a
Eval:       ------  (1 scheme_eval)
```

### Call Expressions

Call expressions follow the call expression evaluation procedure.调用表达式遵循调用表达式求值过程。

```scheme
Expression: (+ 1 2)
Eval:       -------
             - - -  (4 scheme_eval)
Apply:      ~~~~~~~ (1 scheme_apply)

Expression: (= (quotient 5 1) (+ 1 2))
Eval:       --------------------------
             - --------------
                -------- - -
Apply:         ~~~~~~~~~~~~~~
Eval:                         -------
                               - - -   (10 scheme_eval)
Apply:                        ~~~~~~~
            ~~~~~~~~~~~~~~~~~~~~~~~~~~ (3 scheme_apply)
```

### Special Forms

Special forms follow different evaluation rules. A more complete description of their behaviors is described in the [Scheme Project](https://cs61a.org/proj/scheme) and the [Scheme Specification](https://cs61a.org/articles/scheme-spec/#special-forms).

Unlike call expressions where we need to evaluate the operator, special forms do not need to evaluate the first term. In `scheme_eval`, the logic looks like this.

```py
SPECIAL_FORMS = {
    'and': do_and_form,
    'begin': do_begin_form,
    'cond': do_cond_form,
    'define': do_define_form,
    'if': do_if_form,
    'lambda': do_lambda_form,
    'let': do_let_form,
    'or': do_or_form,
    'quote': do_quote_form,
}

first, rest = expr.first, expr.second
if scheme_symbolp(first) and first in SPECIAL_FORMS:
    return SPECIAL_FORMS[first](rest, env)
```

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
