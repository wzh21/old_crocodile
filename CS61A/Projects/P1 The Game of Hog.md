​													

​																**Project 1 : The Game of Hog**

# 项目介绍

在Hog比赛中，两名选手轮流轮次，试图成为第一个以至少GOAL总分结束轮次的选手，其中GOAL***默认*为100分。**在每一回合，当前玩家**选择一定数量**的骰子进行掷骰，**最多10个**。该玩家的回合分数是骰子结果的总和。然而，掷骰子过多的玩家会面临以下风险：

+ **Sow Sad**. If any of the dice outcomes is a 1, the current player's score for the turn is `1`.

如果任一骰子结果为1，则当前玩家该回合的得分为“1”。



以上是普通模式下的全部规则,但为了增加游戏的趣味性,我们又加入了以下规则:

+ **Pig Tail**. A player who chooses to roll zero dice(骰子) scores `2 * abs(tens - ones) + 1` points; where `tens`, `ones` are the tens and ones digits of the opponent's score. The ones digit refers to the rightmost digit and the tens digit refers to the second-rightmost digit.

  选择掷零骰子的玩家得分为`2*abs(tens - one) + 1”分`；其中`ten`、`ones`是对手分数的十位数和一位数。一位数是指最右边的数字，十位数是指第二右边的数字。

  e.g.	

  The opponent has `46` points, and the current player chooses to roll zero dice. `2 * abs(4 - 6) + 1 = 5`, so the player gains `5` points.

+ **Square Swine**. After a player gains points for their turn, if the resulting score is a perfect square, then increase their score to the next higher perfect square. A perfect square is any integer `n` where `n = d * d` for some integer `d`.

  如果是平方数,那就增长到下一平方数(无论这个平方数是如何得来的)

# 注意事项

1.不要修改 除了` Hog.py`以外的文件

2.DEBUG :

```PY
 print("DEBUG:", x) 
```

which will produce an output in your terminal without causing OK tests to fail with extra output.

这将在您的终端中产生输出，而不会导致OK测试因额外输出而失败。



# 具体实现

> dice.py是对dice的模拟的相关操作,只需要阅读,不需要改动

## prob 1 返回投掷一次骰子返回的点数

1.即使在掷骰子的过程中发生了Sow Sad，也要准确地调用dice()	num_rolls次。这样做，你就能正确地模拟所有的骰子一起滚动（用户界面也能正确工作）。

2.确保你准确地调用骰子num_rolls次！

如果你把它称为更少或更多，它就不会在下一轮的循环中处于正确的位置

(即如果call 了 make_dice_test(1, 2, 3, 4)三次,   下一次call同一个函数, 会返回4, 而不是从1 从头开始)

## prob 2 实现pig_tail

此方法用于实现让这个方法普适一点,假设score > 100也可以处理

==**要尽可能把代码写简洁**==

## prob 3 整合前两步

返回一整个回合后,返回的点数

## prob 4 实现 平方跃迁

**新小方法介绍:==判断一个数是不是完全平方数: 1 + 3 + 5 .... + (2n - 1) = n ^ 2==**

## prob 5 整合起来

==**更换两个player的技巧!**==

- ==If `who` is the current player, the next player is `1 - who`.==

## prob 6 实现`always_roll_n()`

nothing

## prob 7 实现`is_always_roll()`

nothing

## ==prob 8 实现 `make_averaged()`==

是一个高阶函数,接受一个函数作为参数,返回一个函数

**Important:** To implement this function, you will need to use a new piece of Python syntax. We would like to write a function that accepts an arbitrary number of arguments, and then calls another function using exactly those arguments. Here's how it works.要实现此函数，您需要使用新的Python语法。我们希望编写一个接受任意数量参数的函数，然后使用这些参数调用另一个函数。这是它的工作原理。

Instead of listing formal parameters for a function, you can write `*args`, which represents all of the **arg**ument**s** that get passed into the function. We can then call another function with these same arguments by passing these `*args` into this other function. For example:==**您可以编写`*args`，而不是列出函数的形式参数，它表示传递到函数中的所有arguments。然后，我们可以通过将这些`*args`传递给另一个函数来调用具有相同参数的另一个功能。**例如：==

```py
>>> def printed(f):
...     def print_and_return(*args):
...         result = f(*args)
...         print('Result:', result)
...         return result
...     return print_and_return
>>> printed_pow = printed(pow)
>>> printed_pow(2, 8)
Result: 256
256
>>> printed_abs = printed(abs)
>>> printed_abs(-10)
Result: 10
10
```

Here, we can pass any number of arguments into `print_and_return` via the `*args` syntax. We can also use `*args` inside our `print_and_return` function to make another function call with the same arguments.在这里，我们可以通过`*args`语法将任意数量的参数传递到`print_and_return`中。我们还可以在  `print_and_return`函数中使用`*args`来使用相同的参数进行另一个函数调用。





是不是 **==高阶函数==** 的决定性因素: 是不是接受一个**==函数作为参数==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221226220149325.png" alt="image-20221226220149325" style="zoom:50%;" />



```py
def make_averaged(original_function, total_samples=1000):
    """Return a function that returns the average value of ORIGINAL_FUNCTION
    called TOTAL_SAMPLES times.

    To implement this function, you will have to use *args syntax.

    >>> dice = make_test_dice(4, 2, 5, 1)
    >>> averaged_dice = make_averaged(roll_dice, 40)
    >>> averaged_dice(1, dice)  # The avg of 10 4's, 10 2's, 10 5's, and 10 1's
    3.0
    """
    # BEGIN PROBLEM 8

    def aver_ret(*args):
        total = 0
        i = 0
        while i < total_samples:
            total += original_function(*args)
            i += 1
        return total / total_samples
    return aver_ret

    # END PROBLEM 8
```



## prob9 实现 `max_scoring_num_rolls`

做个实验,看看roll几次会得到最高的分

```py
def max_scoring_num_rolls(dice=six_sided, total_samples=1000):
    """Return the number of dice (1 to 10) that gives the highest average turn score
    by calling roll_dice with the provided DICE a total of TOTAL_SAMPLES times.
    Assume that the dice always return positive outcomes.

    >>> dice = make_test_dice(1, 6)
    >>> max_scoring_num_rolls(dice)
    1
    """
    # BEGIN PROBLEM 9
    # in this function, you should call max_average() and roll_dice()
    i = 1
    max_score = 0
    while i <= 10:
        averaged_roll_dice = make_averaged(roll_dice, total_samples)
        max_score_i = averaged_roll_dice(i, dice)
        if max_score_i > max_score:
            max_score = max_score_i
            max_score_num = i
        i += 1 
    return max_score_num
    # END PROBLEM 9
```

## prob 10 实现tail_strategy

A strategy can try to take advantage of the *Pig Tail* rule by rolling 0 when it is most beneficial to do so. Implement `tail_strategy`, which returns 0 whenever rolling 0 would give **at least** `threshold(阈值)` points and returns `num_rolls` otherwise. This strategy should **not** also take into account the Square Swine rule.

只考虑 pig_tail, 不考虑 平方跃迁

## prob 11 实现square_strategy

同时考虑到 pig_tail 和 平方跃迁