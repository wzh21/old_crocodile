​									

​										**Project 4: Scheme Interpreter**

# Part 1: The Evaluator

In Part 1, you will develop the following features of the interpreter:

- **Symbol evaluation**
- **Calling built-in procedures**
- **Definitions**

在提供给您的starter实现中，求值器只能计算自求值表达式：数字、布尔值和“nil”。

First, read the relevant(相关的) code. In the "Eval/Apply" section of `scheme_eval_apply.py`:

- `scheme_eval` evaluates a Scheme expression in the given environment. This function is nearly complete but is missing the logic for call expressions.`scheme_eval`计算给定环境中的scheme表达式。此函数已接近完成，但缺少调用表达式的逻辑。
- When evaluating a special form, `scheme_eval` redirects evaluation to an appropriate `do_?_form` function found in `scheme_forms.py`在计算特殊形式时，“scheme_eval”将计算重定向到适当的“do\_?\_form`函数在`scheme_forms.py中找到
- `scheme_apply` applies a procedure to some arguments. This function has cases for the various types of procedures (builtin procedures, user-defined procedures, and so forth) that you will implement.`scheme_apply将过程应用于某些参数。此函数为将要实现的各种类型的过程（内置过程、用户定义过程等）提供了案例。

In the "Environments" and "Procedures" section of `scheme_classes.py`:在“scheme_classes.py”的“环境”和“过程”部分中：

- The `Frame` class implements an environment frame.“Frame”类实现环境框架。
- The `LambdaProcedure` class (in the "Procedures" section) represents user-defined procedures.“LambdaProcedure”类（在“Procedures”部分）表示用户定义的过程。

These are all of the essential components of the interpreter. `scheme_forms.py` defines special forms, `scheme_builtins.py` defines the various functions built into the standard library, and `scheme.py` defines input/output behavior.这些都是Interpreter的重要组成部分.`scheme_forms.py`定义特殊形式，`scheme_builtins.py`定义标准库中内置的各种函数，`scheme.py`定义输入/输出行为。

## 问答环节

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112202702932.png" alt="image-20230112202702932" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112203518411.png" alt="image-20230112203518411" style="zoom:33%;" />

> 在env值environment, 在Frame类里面

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112203655515.png" alt="image-20230112203655515" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112203918684.png" alt="image-20230112203918684" style="zoom:33%;" />

> 选4, Ⅰ和Ⅱ

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112204010715.png" alt="image-20230112204010715" style="zoom:33%;" />

## Problem 1 (1 pt)

> ==P1目标 : 完成define(绑定name) 和 lookup(查找name)==

Implement the `define` and `lookup` methods of the `Frame` class in `scheme_classes.py`. Each `Frame` object has the following instance attributes:在“scheme_classes.py”中实现“Frame”类的“define”和“lookup”方法。每个“Frame”对象具有以下实例属性：

- `bindings` is a dictionary representing the bindings in the frame. Each item associates a Scheme symbol (represented as a Python string) to a Scheme values.`bindings`是表示框架中绑定的字典。每个项都将Scheme符号（表示为Python字符串）与Scheme值相关联。
- `parent` is the parent `Frame` instance. **The parent of the Global Frame is `None`.**

In `scheme_classes.py`:

1. `define` takes **a symbol** (**represented by a Python string**) and **a value.** It binds the symbol to the value in the `Frame` instance.
2. `lookup` takes a symbol and returns the value bound to that symbol in the first frame of the environment in which the symbol is bound. The *environment* for a `Frame` instance consists of that frame, its parent frame, and all its ancestor frames, including the Global Frame. When looking up a symbol:`“查找”获取一个符号，并返回绑定该符号的环境的第一帧中绑定到该符号的值。“帧”实例的*环境*由该帧、其父帧及其所有祖先帧（包括全局帧）组成。查找符号时：
   - If the symbol is bound in the current frame, return its value.
   - If the symbol is not bound in the current frame and the frame has a parent frame, look up the symbol in the parent frame.
   - If the symbol is not found in the current frame and there is no parent frame, raise a `SchemeError`.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112211032556.png" alt="image-20230112211032556" style="zoom:33%;" />

> 别发傻啊, self.parent.lookup()    顺序别搞错

## Problem 2 (2 pt)

==P2目标 : 完成内置的函数的apply==

要能够调用内置过程（如“+”），需要在“scheme_eval_apply.py”中的“scheme_apply”函数中完成“内置过程”案例。通过调用实现该过程的相应Python函数来应用内置过程。
> To see a list of all Scheme built-in procedures used in the project, look in the `scheme_builtins.py` file. Any function decorated with `@builtin` will be added to the globally-defined `BUILTINS` list.
>
> 要查看项目中使用的所有Scheme内置过程的列表，请查看“Scheme_builtins.py”文件。任何用“@builtin”修饰的函数都将添加到全局定义的“BUILTINS”列表中。

A `BuiltinProcedure` has two instance attributes:

- `py_func`: the *Python* function that implements the built-in Scheme procedure.
- `need_env`: a Boolean flag that indicates whether or not this built-in procedure will need the current environment to be passed in as the last argument. The environment is required, for instance, to implement the built-in `eval` procedure.**一个布尔标志，指示此内置过程是否需要将当前环境作为最后一个参数传入。例如，实现内置的“eval”过程需要环境。**

`scheme_apply` takes the `procedure` object, a list of argument values, and the current environment. `args` is a Scheme list represented as a `Pair` object or `nil`.

> Your implementation should do the following:
>
> - Convert the Scheme list to a Python list of arguments. *Hint:* `args` is a `Pair`, which has a `.first` and `.rest` attribute.将Scheme列表转换为Python参数列表**==*提示：*“args”是一个“Pair”，它具有“.first”和“.rest”属性。==**
> - **If `procedure.need_env` is `True`, then add the current environment `env` as the last argument to this Python list.**
> - Return the result of calling `procedure.py_func` on all of those arguments. Use `*args` notation: `f(1, 2, 3)` is equivalent to `f(*[1, 2, 3]`). Do this part within the `try` statement provided, after the line that says `try:`.

We have already implemented the following behavior for you:

- If calling the function results in a `TypeError` exception being raised, then the wrong number of arguments were passed. The `try` statement handles this exception and raises a `SchemeError` with the message `'incorrect number of arguments'`.

  <img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112215827176.png" alt="image-20230112215827176" style="zoom:33%;" />

```py
def scheme_apply(procedure, args, env):
    """Apply Scheme PROCEDURE to argument values ARGS (a Scheme list) in
    Frame ENV, the current environment."""
    validate_procedure(procedure)
    if not isinstance(env, Frame):
       assert False, "Not a Frame: {}".format(env)
    if isinstance(procedure, BuiltinProcedure):
        # BEGIN PROBLEM 2
        "*** YOUR CODE HERE ***"
        # 第一步,把Scheme列表转换为python列表
        def change_to_lst(lst_pair):
            if lst_pair is nil:
                return []
            return [lst_pair.first] + change_to_lst(lst_pair.rest)
        args_py = change_to_lst(args)
        # END PROBLEM 2
        try:
            # BEGIN PROBLEM 2
            "*** YOUR CODE HERE ***"
            if procedure.need_env:
                args_py.append(env)
                return procedure.py_func(*args_py)
            else: 
                return procedure.py_func(*args_py)
            # END PROBLEM 2
        except TypeError as err:
            raise SchemeError('incorrect number of arguments: {0}'.format(procedure))
```

> **args是加到参数列表(args_py)里的最后一个(用append方法)**

**==列表前面要加星号,作用是：将*列表*解开成几个独立的参数，传入函数!!!!!!!!!!!!==**

## Problem 3 (2 pt)

==P3目标 : 解析一个表达式(上一个prob是计算*单个*表达式)==

The `scheme_eval` function (in `scheme_eval_apply.py`) evaluates a Scheme expression (represented as a `Pair`) in a given environment. The provided code already looks up names in the current environment, returns self-evaluating expressions (such as numbers) and evaluates special forms.		“scheme_eval”函数（在“scheme_eval_apply.py”中）计算给定环境中的scheme表达式（表示为“Pair”）。提供的代码已经在当前环境中查找名称，返回自求值表达式（如数字）并计算特殊形式。

Implement the missing part of `scheme_eval`, which evaluates a call expression. To evaluate a call expression:实现“scheme_eval”的缺失部分，该部分计算调用表达式。要计算调用表达式，请执行以下操作：

1. Evaluate the **operator** (**which should evaluate to an instance of `Procedure`**).
2. Evaluate **all of the operands.**
3. Apply the procedure to the evaluated operands **by calling `scheme_apply`**, then return the result.

**You'll have to recursively call `scheme_eval` in the first two steps.** Here are some other functions/methods you should use:**在前两个步骤中，您必须递归调用“scheme_eval”。**以下是您应该使用的其他函数/方法：

- **The `map` method of `Pair` returns a new Scheme list constructed by applying a *one-argument function* to every item in a Scheme list.	“Pair”的“map”方法返回一个新的Scheme列表，该列表通过对Scheme列表中的每个项应用*一个参数函数*来构造。**
- The `scheme_apply` function applies a Scheme procedure to arguments represented as a Scheme list (a `Pair` instance).“scheme_apply”函数将scheme过程应用于表示为scheme列表（“Pair”实例）的参数。

> Important: do not mutate the passed-in `expr`. That would change a program as it's being evaluated, creating strange and incorrect effects.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112220555810.png" alt="image-20230112220555810" style="zoom:33%;" />

```py
def scheme_eval(expr, env, _=None):  # Optional third argument is ignored
    """Evaluate Scheme expression EXPR in Frame ENV.

    >>> expr = read_line('(+ 2 2)')
    >>> expr
    Pair('+', Pair(2, Pair(2, nil)))
    >>> scheme_eval(expr, create_global_frame())
    4
    """
    # Evaluate atoms
    if scheme_symbolp(expr): # 如果是symbol (同时这一个expr就是symbol)
        return env.lookup(expr) # symbol也可以是producer!!!!!!!!!!!!!!!!!!!!!!!!!!
    elif self_evaluating(expr): # Return whether EXPR evaluates to itself.
        return expr # symbol也可以是producer!!!!!!!!!!!!!!!!!!!!!!!!!!

    # All non-atomic expressions are lists (combinations)
    if not scheme_listp(expr): #看看是不是一个well-defined的list
        raise SchemeError('malformed list: {0}'.format(repl_str(expr)))
    first, rest = expr.first, expr.rest
    if scheme_symbolp(first) and first in scheme_forms.SPECIAL_FORMS:
        return scheme_forms.SPECIAL_FORMS[first](rest, env)
    else:
        # BEGIN PROBLEM 3
        "*** YOUR CODE HERE ***"
        procedure = scheme_eval(first, env) # recursive call
        validate_procedure(procedure) # 先检查是不是valid, 再apply
        arguments = rest.map(lambda expr: scheme_eval(expr, env)) # recursive call
        # Pair类 自定义的map函数
        return scheme_apply(procedure, arguments, env)
        # 好难!!还没完全看懂
        # END PROBLEM 3
```

> **==本题又用到了递归调用, 比较难!!!!!!!!!!!==**

## Problem 4 (2 pt)

**==P4 目标 : 实现define==**

The `define` special form ([spec](https://cs61a.org/articles/scheme-spec/#define)) in Scheme can be used either to assign a symbol to the value of a given expression or to create a procedure and bind it to a symbol:

```py
scm> (define a (+ 2 3))  ; Binds the symbol a to the value of (+ 2 3)
a
scm> (define (foo x) x)  ; Creates a procedure and binds it to the symbol foo
foo
```

The type of the first operand tells us what is being defined:

- If it is a symbol, e.g. `a`, then the expression is defining a symbol如果它是一个符号，例如“a”，则表达式定义了一个符号
- If it is a list, e.g. `(foo x)`, then the expression is creating a procedure.如果它是一个列表，例如“（foo x）”，则表达式正在创建一个过程。

The `do_define_form` function in `scheme_forms.py` evaluates `(define ...)` expressions. There are two missing parts in this function. For this problem, implement **just the first part**, which evaluates the second operand to obtain a value and binds the first operand, a symbol, to that value. Then, `do_define_form` returns the symbol that was bound.

“scheme_forms.py”中的“do_define_form”函数计算“（define…）”表达式。此函数中缺少两个部分。对于这个问题，只实现**第一部分**，它计算第二个操作数以获得一个值，并将第一个操作数（一个符号）绑定到该值。然后，`do_define_form `返回绑定的符号。

> 代码和prob10 合并

## Problem 5 (1 pt)

**==P5 目标 : 实现quote==**

In Scheme, you can quote expressions in two ways: with the `quote` special form ([spec](https://cs61a.org/articles/scheme-spec/#quote)) or with the symbol `'`. The reader converts `'...` into `(quote ...)`, so that your interpreter only needs to evaluate the `(quote ...)` syntax. The `quote` special form returns its operand expression without evaluating it:

```scheme
scm> (quote hello)
hello
scm> '(cons 1 2)  ; Equivalent to (quote (cons 1 2))
(cons 1 2)
```

Implement the `do_quote_form` function in `scheme_forms.py` so that it simply returns the unevaluated operand of the `(quote ...)` expression.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112233840951.png" alt="image-20230112233840951" style="zoom:33%;" />

> 选3

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112234018111.png" alt="image-20230112234018111" style="zoom: 33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230112234423961.png" alt="image-20230112234423961" style="zoom: 25%;" />

```py
def do_quote_form(expressions, env):
    """Evaluate a quote form.

    >>> env = create_global_frame()
    >>> do_quote_form(read_line("((+ x 2))"), env) # evaluating (quote (+ x 2))
    Pair('+', Pair('x', Pair(2, nil)))
    """
    validate_form(expressions, 1, 1)
    # BEGIN PROBLEM 5
    "*** YOUR CODE HERE ***"
    return expressions.first
    # END PROBLEM 5
```

> 一定要加first

# Part 2: Procedures

In Part 2, you will add the ability to create and call user-defined procedures. You will add the following features to the interpreter:在第2部分中，您将添加创建和调用用户定义过程的功能。您将向解释器添加以下功能：

- **Lambda procedures**, using the `(lambda ...)` special form
- **Named procedures**, using the `(define (...) ...)` special form
- **Dynamically scoped mu procedures**, using the `(mu ...)` special form.

## Problem 6 (1 pt)

**==P6 目标 : 实现bigin==**

Change the `eval_all` function in `scheme_eval_apply.py` (which is called from `do_begin_form` in `scheme_forms.py`) to complete the implementation of the `begin` special form ([spec](https://cs61a.org/articles/scheme-spec/#begin)).

A `begin` expression is evaluated by evaluating all sub-expressions in order. The value of the `begin` expression is the value of the final sub-expression.通过按顺序计算所有子表达式来计算“begin”表达式。“begin”表达式的值是最终子表达式的值。

To complete the implementation of `begin`, `eval_all` will take in `expressions` (a Scheme list of expressions) and `env` (a `Frame` representing the current environment), evaluate all the expressions in `expressions`, and return the value of the last expression in `expressions`.

```scheme
scm> (begin (+ 2 3) (+ 5 6))
11
scm> (define x (begin (display 3) (newline) (+ 2 3)))
3
x
scm> (+ x 3)
8
scm> (begin (print 3) '(+ 2 3))
3
(+ 2 3)
```

If `eval_all` is passed an empty list of expressions (`nil`), then it should return the Python value `None`, which represents the Scheme value `undefined`.如果“eval_all”被传递了一个空的表达式列表（“nil”），那么它应该返回Python值“None”，该值表示Scheme值“undefined”。

## Problem 7 (2 pt)

**==P7 目标 : 实现lambda==**

Implement the `do_lambda_form` function ([spec](https://cs61a.org/articles/scheme-spec/#lambda)) in `scheme_forms.py`, which creates and returns a `LambdaProcedure` instance. While you cannot call a user-defined procedure yet, you can verify that you have created the procedure correctly by evaluating a lambda expression.实现`do_lambda_form`函数（[spec](https://cs61a.org/articles/scheme-spec/#lambda))在“scheme_forms.py”中，它创建并返回一个“LambdaProcedure”实例。虽然还不能调用用户定义的过程，但可以通过计算lambda表达式来验证是否正确创建了该过程。

```scheme
scm> (lambda (x y) (+ x y))
(lambda (x y) (+ x y))
```

In Scheme, it is legal to place more than one expression in the body of a procedure. (There must be at least one expression.) The `body` attribute of a `LambdaProcedure` instance is therefore a Scheme list of body expressions. The `formals` attribute of a `LambdaProcedure` instance should be a properly nested `Pair` expression. Like a `begin` special form, evaluating the body of a procedure evaluates all body expressions in order. The return value of a procedure is the value of its last body expression.在Scheme中，**在程序主体中放置多个表达式是合法的。**（必须至少有一个表达式。）因此，“LambdaProcedure”实例的“body”属性是主体表达式的Scheme列表。“LambdaProcedure”实例的“formals”属性应该是正确嵌套的“Pair”表达式。像“begin”特殊形式一样，对过程的主体求值会按顺序对所有主体表达式求值。**过程的返回值是其最后一个主体表达式的值。**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113133424403.png" alt="image-20230113133424403" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113133802017.png" alt="image-20230113133802017" style="zoom:33%;" />

```py
def do_lambda_form(expressions, env):
    """Evaluate a lambda form.

    >>> env = create_global_frame()
    >>> do_lambda_form(read_line("((x) (+ x 2))"), env) # evaluating (lambda (x) (+ x 2))
    LambdaProcedure(Pair('x', nil), Pair(Pair('+', Pair('x', Pair(2, nil))), nil), <Global Frame>)
    """
    validate_form(expressions, 2)
    formals = expressions.first
    validate_formals(formals) # 看看是不是一个valid的参数列表(是不是重复等等)
    # BEGIN PROBLEM 7
    "*** YOUR CODE HERE ***"
    return LambdaProcedure(formals, expressions.rest, env) # Lambda表达式只是创建一个过程(把参数和内容记录下来),没有应用
    # END PROBLEM 7
```

> Lambda表达式只是创建一个过程(把参数和内容记录下来),没有应用

## Problem 8 (2 pt)

**==P8 目标 : 实现创建子帧==**

Implement the `make_child_frame` method of the `Frame` class (in `scheme_classes.py`), which will be used to create new frames when calling user-defined procedures. This method takes in two arguments: `formals`, which is a Scheme list of symbols, and `vals`, which is a Scheme list of values. It should return a new child frame, binding the formal parameters to the values.实现“frame”类的“make_child_frame”方法（在“scheme_classes.py”中），在调用用户定义的过程时，该方法将用于创建新的帧。此方法接受两个参数：“formals”（符号的Scheme列表）和“vals”（值的Scheme）。**它应该返回一个新的子帧，将形式参数绑定到值。**

To do this:

- If the number of argument values does not match with the number of formal parameters, raise a `SchemeError`.如果参数值的数量与形式参数的数量不匹配，则引发“SchemeError”。
- Create a new `Frame` instance, the parent of which is `self`.创建一个新的“Frame”实例，其父级为“self”。
- Bind each formal parameter to its corresponding argument value in the newly created frame. The first symbol in `formals` should be bound to the first value in `vals`, and so on.将每个形参绑定到新创建的框架中相应的实参值。“formals”中的第一个符号应绑定到“vals”中的一个值，依此类推。
- Return the new frame.返回新帧。

> *Hint:* The `define` method of a `Frame` instance creates a binding in that frame.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113140131935.png" alt="image-20230113140131935" style="zoom:33%;" />

## Problem 9 (2 pt)

**==P9 目标 : 实现lambda过程==**

Implement the `LambdaProcedure` case in the `scheme_apply` function in `scheme_eval_apply.py`.	在“scheme_eval_apply.py”中的“scheme_apply”函数中实现“LambdaProcedure”情况。

You should first create a new `Frame` instance using the `make_child_frame` method of the appropriate parent frame, binding formal parameters to argument values. Then, evaluate each of the expressions of the body of the procedure using `eval_all` within this new frame.	**您应该首先使用适当的父帧的“make_child_Frame”方法创建一个新的“Frame”实例，将形式参数绑定到参数值。然后，==在这个新框架内==使用“eval_all”对过程主体的每个表达式求值。**

Your new frame should be a child of the frame in which the lambda is defined. Note that the `env` provided as an argument to `scheme_apply` is instead the frame in which the procedure is called. See [User-Defined Procedures](https://cs61a.org/proj/scheme/#user-defined-procedures) to remind yourself of the attributes of `LambdaProcedure`.	**新帧应该是定义lambda的帧的子帧。请注意，作为“scheme_apply”参数提供的“env”是调用过程的帧**。参见[用户定义程序](https://cs61a.org/proj/scheme/#user-定义的过程），以提醒自己“LambdaProcedure”的属性。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113140743067.png" alt="image-20230113140743067" style="zoom: 33%;" />

> ==**lambda(z x)中的x是新创建的参数,不绑定在父帧的x上**==

> 和prob 11 一起放

## Problem 10 (1 pt)

**==P9 目标 : 实现非lambda的用户定义过程==**

Currently, your Scheme interpreter is able to bind symbols to user-defined procedures in the following manner:

```scheme
scm> (define f (lambda (x) (* x 2)))
f
```

However, we'd like to be able to use the shorthand form of defining named procedures:但是，我们希望能够使用缩写形式定义命名过程：

```scheme
scm> (define (f x) (* x 2))
f
```

Modify the `do_define_form` function in `scheme_forms.py` so that it correctly handles `define (...) ...)` expressions ([spec](https://cs61a.org/articles/scheme-spec/#define)).

Make sure that it can handle multi-expression bodies. For example,

```scheme
scm> (define (g y) (print y) (+ y 1))
g
scm> (g 3)
3
4
```

There are (at least) two ways to solve this problem. One is to construct an expression `(define _ (lambda ...))` and call `do_define_form` on it. The second is to implement it directly:解决这个问题有（至少）两种方法。一种是构造表达式“（define _（lambda…））”并对其调用“do_define_form”。第二种是直接实现它：

- Using the given variables `signature` and `expressions`, find the defined function's name (symbol), formals, and body.使用给定的变量“signature”和“expressions”，查找已定义函数的名称（符号）、形式和主体。
- Create a `LambdaProcedure` instance using the formals and body. (You could call `do_lambda_form` to do this.)使用formal和body创建“LambdaProcedure”实例。（您可以调用`do_lambda_form`来执行此操作。）
- Bind the symbol to this new `LambdaProcedure` instance.将符号绑定到此新的“LambdaProcedure”实例。
- Return the symbol that was bound.将符号绑定到此新的“LambdaProcedure”实例。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113144912674.png" alt="image-20230113144912674" style="zoom:33%;" />



## Problem 11 (1 pt)

All of the Scheme procedures we've seen so far use ==*lexical scoping*==: the parent of the new call frame is the environment in which the procedure was **defined**. Another type of scoping, which is not standard in Scheme but appears in other variants of Lisp, is called ==*dynamic scoping*==: the parent of the new call frame is the environment in which the call expression was **evaluated**. With dynamic scoping, calling the same procedure with the same arguments from different parts of your code can create different behavior (due to different parent frames).到目前为止，我们看到的所有Scheme过程都使用了==*词法作用域*==：新调用框架的父级是==**定义过程**==的环境。另一种类型的作用域在Scheme中不是标准的，但在Lisp的其他变体中出现，称为***==动态作用域==***：==新调用帧的父级是对调用表达式进行**求值**的环境。使用动态作用域，从代码的不同部分使用相同的参数调用相同的过程可以创建不同的行为（由于父帧不同）==。

The `mu` special form ([spec](https://cs61a.org/articles/scheme-spec/#mu); invented for this project) evaluates to a dynamically scoped procedure.

```scheme
scm> (define f (mu () (* a b)))
f
scm> (define g (lambda () (define a 4) (define b 5) (f)))
g
scm> (g)
20
```

Above, the procedure `f` does not have `a` or `b` as arguments; however, because `f` gets called within the procedure `g`, it has access to the `a` and `b` defined in `g`'s frame.上面，==**过程“f”没有“a”或“b”作为参数；然而，由于在过程“g”中调用了“f”，因此它可以访问在“g”帧中定义的“a”和“b”。**==

Your job:

- Implement `do_mu_form` in `scheme_forms.py` to evaluate the `mu` special form. A `mu` expression evaluates to a `MuProcedure`. The `MuProcedure` class (defined in `scheme_classes.py`) has been provided for you.在“scheme_forms.py”中实现“do_mu_form”以计算“mu”特殊形式。“mu”表达式的计算结果为“MuProcedure”。已经为您提供了“MuProcedure”类（在“scheme_classes.py”中定义）。
- In addition to implementing `do_mu_form`, complete the `MuProcedure` case within the `scheme_apply` function (in `scheme_eval_apply.py`) so that when a mu procedure is called, its body is evaluated in the correct environment. When a `MuProcedure` is called, the parent of the new call frame is the environment in which that call expression was **evaluated**. As a result, a `MuProcedure` does not need to store an environment as an instance attribute.除了实现“do_mu_form”之外，还要在“scheme_apply”函数（在“scheme_eval_apply.py”中）中完成“MuProcedure”情况，以便在调用mu过程时，在正确的环境中评估其主体。当调用“MuProcedure”时，新调用帧的父级是对该调用表达式进行**求值**的环境。因此，“MuProcedure”不需要将环境存储为实例属性。

<img src="https://img2022.cnblogs.com/blog/2741744/202204/2741744-20220423205714996-1657575512.png" alt="img" style="zoom: 200%;" />



<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113160323136.png" alt="image-20230113160323136" style="zoom:33%;" />

> 答案是13
>
> **==因为y不是1,而是7, 找y的时候是看谁调用了它==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113160648563.png" alt="image-20230113160648563" style="zoom:50%;" />

**==静态作用域:为每一个procedure过程*所在的环境*创建子帧==**

**==动态作用域:为每一个过程调用者创建子帧==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113161157102.png" alt="image-20230113161157102" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113171954482.png" alt="image-20230113171954482" style="zoom:80%;" />

# Part 3: Special Forms

This section will be completed in `scheme_forms.py`.

Logical special forms include `if`, `and`, `or`, and `cond`. These expressions are special because not all of their sub-expressions may be evaluated.

**==In Scheme, only `#f` is a false value. All other values (including `0` and `nil`) are true values.==** You can test whether a value is a true or false value using the provided Python functions `is_scheme_true` and `is_scheme_false`, defined in `scheme_utils.py`.

> Scheme traditionally uses `#f` to indicate the false Boolean value. In our interpreter, that is equivalent to `false` or `False`. Similarly, `true`, `True`, and `#t` are all equivalent. However, **when unlocking tests**, use `#t` and `#f`.
>
> Scheme传统上使用“#f”来指示假布尔值。在我们的解释器中，这相当于“false”或“false”。类似地，“true”、“true”和“#t”都是等效的。但是，**在解锁测试**时，请使用“#t”和“#f”。

To get you started, we've provided an implementation of the `if` special form in the `do_if_form` function. Make sure you understand that implementation before starting the following questions.为了开始，我们在“do_if_form”函数中提供了“if”特殊形式的实现。在开始以下问题之前，请确保您了解该实现。

## Problem 12 (2 pt)

==Implement `do_and_form`and `do_or_form`== so that `and` and `or` expressions ([spec](https://cs61a.org/articles/scheme-spec/#and)) are evaluated correctly.

注意短路求值

> Internal to the interpreter, represent Scheme's `#t` as Python's `True` and Scheme's `#f` as Python's `False`.
>
> 在解释器内部，将Scheme的“#t”表示为Python的“True”，将Schem的“#f”表示为Python的“False”。

**Important:** Use the provided Python functions `is_scheme_true` and `is_scheme_false` from `scheme_utils.py` to test boolean values.**重要提示：**使用“scheme_utils.py”中提供的Python函数“is_scheme_true”和“is_sheme_false”来测试布尔值。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113225219629.png" alt="image-20230113225219629" style="zoom: 33%;" />

> 选2

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113225256485.png" alt="image-20230113225256485" style="zoom: 33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113225404174.png" alt="image-20230113225404174" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113225505708.png" alt="image-20230113225505708" style="zoom:25%;" />

> **==In Scheme, only `#f` is a false value. All other values (including `0` and `nil`) are true values.==**

(略)

## Problem 13 (2 pt)

Fill in the missing parts of `do_cond_form` so that it correctly implements `cond` ([spec](https://cs61a.org/articles/scheme-spec/#cond)), returning the value of the first result sub-expression corresponding to a true predicate, or the value of the result sub-expression corresponding to `else`.

Some special cases:

- When the true predicate does not have a corresponding result sub-expression, return the predicate value.当真谓词没有相应的结果子表达式时，返回谓词值。
- When a result sub-expression of a `cond` case has multiple expressions, evaluate them all and return the value of the last expression. (*Hint*: Use `eval_all`.)当“cond”case的结果子表达式有多个表达式时，请全部求值并返回最后一个表达式的值。（*提示*：使用`eval_all`。）

Your implementation should match the following examples and the additional tests in `tests.scm`.

```scheme
scm> (cond ((= 4 3) 'nope)
           ((= 4 4) 'hi)
           (else 'wait))
hi
scm> (cond ((= 4 3) 'wat)
           ((= 4 4))
           (else 'hm))
#t
scm> (cond ((= 4 4) 'here (+ 40 2))
           (else 'wat 0))
42
```

The value of a `cond` is `undefined` if there are no true predicates and no `else`. In such a case, `do_cond_form` should return `None`. If there is only an `else`, return the value of its result sub-expression. If it doesn't have one, return `#t`.如果没有真谓词和“else”，则“cond”的值为“undefined”。在这种情况下，“do_cond_form”应返回“None”。如果只有“else”，则返回其结果子表达式的值。如果没有，请返回“#t”。

```scheme
scm> (cond (False 1) (False 2))
scm> (cond (else))
#t
```

## Problem 14 (2 pt)

The `let` special form ([spec](https://cs61a.org/articles/scheme-spec/#let)) binds symbols to values locally, giving them their initial values. For example:

```scheme
scm> (define x 5)
x
scm> (define y 'bye)
y
scm> (let ((x 42)
           (y (* x 10)))  ; this x refers to the global value of x, not 42
       (list x y))
(42 50)
scm> (list x y)
(5 bye)
```

Implement `make_let_frame` in `scheme_forms.py`, which returns a child frame of `env` that binds the symbol in each element of `bindings` to the value of its corresponding expression. The `bindings` Scheme list contains pairs that each contain a symbol and a corresponding expression.在“scheme_forms.py”中实现“make_let_frame”，它返回“env”的子帧，将“bindings”的每个元素中的符号绑定到其相应表达式的值。“绑定”方案列表包含每对都包含一个符号和一个对应的表达式。

You may find the following functions and methods useful:您可能会发现以下函数和方法很有用：

- `validate_form`: this function can be used to validate the structure of each binding. It takes in a Scheme list `expr` of expressions and a `min` and `max` length. If `expr` is not a list with length between `min` and `max` inclusive, it raises an error. If no `max` is passed in, the default is infinity.`validateform`：此函数可用于验证每个绑定的结构。它接受表达式的Scheme列表“expr”以及“min”和“max”长度。如果“expr”不是长度介于“min”和“max”之间的列表，则会引发错误。如果没有传入“max”，则默认值为无穷大。
- `validate_formals`: this function validates that its argument is a Scheme list of symbols for which each symbol is distinct.`validate_formals：此函数验证其参数是否为每个符号都不同的符号的Scheme列表。

Remember to refer to the [spec](https://cs61a.org/articles/scheme-spec/#let) if you don't understand any of the test cases!

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113233352082.png" alt="image-20230113233352082" style="zoom:50%;" />

> **==这里参数列表里面的a是不知道是什么的==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230113234234817.png" alt="image-20230113234234817" style="zoom:33%;" />

> ==x绑定在了2 ,y绑定5(向外层寻找x)==

# Part 4: Write Some Scheme

Not only is your Scheme interpreter itself a tree-recursive program, but it is flexible enough to evaluate *other* recursive programs. Implement the following procedures in the `questions.scm` file.Scheme解释器本身不仅是一个树递归程序，而且它足够灵活，可以评估*其他*递归程序。在“questions.scm”文件中执行以下步骤。

See the [built-in procedure reference](https://cs61a.org/articles/scheme-builtins/) for descriptions of the behavior of all built-in Scheme procedures.

As you use your interpreter, you may discover additional bugs in your interpreter implementation. Therefore, you may find it useful to test your code for these questions in the staff interpreter or the [web editor](https://code.cs61a.org/scheme) and then try it in your own interpreter once you are confident your Scheme code is working. You can also use the web editor to visualize the scheme code you've written and help you debug.

## Problem 15 (2 pt)

Implement the `enumerate` procedure, which takes in a list of values and returns a list of two-element lists, where the first element is the index of the value, and the second element is the value itself.实现“enumerate”过程，该过程接受一个值列表，并返回两个元素列表的列表，其中第一个元素是值的索引，第二个元素是该值本身。

```scheme
scm> (enumerate '(3 4 5 6))
((0 3) (1 4) (2 5) (3 6))
scm> (enumerate '())
()
```

## Problem 16 (2 pt)

Implement the `merge` procedure, which takes in a comparator function `ordered?` and two lists that are sorted according to the comparator and combines the two lists into a single sorted list. A comparator defines an ordering by comparing two values and returning a true value if and only if the two values are ordered.

```scheme
scm> (merge < '(1 4 6) '(2 5 8))
(1 2 4 5 6 8)
scm> (merge > '(6 4 1) '(8 5 2))
(8 6 5 4 2 1)
scm> (merge < '(1) '(2 3 5))
(1 2 3 5)
```

```scheme
(define (enumerate s)
  ; BEGIN PROBLEM 15
  (define (myfunc s num)
    (if (null? s)
      nil
      (cons (list num (car s)) (myfunc (cdr s) (+ num 1)))
    )
  )
  (myfunc s 0)
)
```

### Problem 16 (2 pt)

Implement the `merge` procedure, which takes in a comparator function `ordered?` and two lists that are sorted according to the comparator and combines the two lists into a single sorted list. A comparator defines an ordering by comparing two values and returning a true value if and only if the two values are ordered.实现“merge”过程，该过程接受比较器函数“ordered？”以及根据比较器排序并将两个列表组合成单个排序列表的两个列表。比较器通过比较两个值并返回真值来定义排序，当且仅当两个值排序时。

```scheme
scm> (merge < '(1 4 6) '(2 5 8))
(1 2 4 5 6 8)
scm> (merge > '(6 4 1) '(8 5 2))
(8 6 5 4 2 1)
scm> (merge < '(1) '(2 3 5))
(1 2 3 5)
```









