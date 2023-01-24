​											

​																	**HW 0: A Java Crash Course**

# HW Goals

在本作业中，我们将学习基本的Java语法概念。虽然此作业是可选的，您不会提交答案，但强烈建议那些没有Java经验的人使用此作业。讲座不会明确地涵盖这一材料，尽管你应该理解它。

本教程假设您至少有一个学期的编程语言的丰富经验，并且只想强调Java做一些以前熟悉的事情的方法。

虽然我希望这份文件对好奇和有上进心的学生来说是独立的，但如果提供了建议的补充阅读，你可能会发现它很有帮助。

**请随意以您觉得舒服的速度浏览和阅读。**可以跳过部分HW。用你最好的判断。这些指示可能比必要的更详细。

# Conditionals

*Optional Supplementary Reading: [Shewchuk](https://sp21.datastructur.es/materials/hw/hw0/hw0_supplementary_conditionals.txt)*

## Curly Braces (and Conditionals)

也可以响应单个条件执行多个语句。我们通过用大括号包装语句来实现这一点，如[本程序](http://goo.gl/wzuPsZ)中所示（或[此](https://goo.gl/JouKwc))，如下所示：

```java
public class ConditionalsWithBlocks {
   public static void main(String[] args) {
      int x = 5;

      if (x < 10) {
         System.out.println("I shall increment x by 10.");
         x = x + 10;
      }

      if (x < 10) {
         System.out.println("I shall increment x by 10.");
         x = x + 10;
      }

      System.out.println(x);
   }
}
```

Curly braces are very important in Java! Unlike Python, statements are grouped by braces, and NOT by indentation. For an example of how this can go terribly wrong, try running the [following program](http://goo.gl/GwhZXB) (or [use this link](https://goo.gl/Czywp3)), which is supposed to print the absolute value of `x`. **Then try changing the value of `x` to a positive number. Run it and make sure you understand why things go wrong.**

```java
public class PrintAbsoluteValue {
    public static void main(String[] args) {
        int x = -5;

        if (x < 0)
            System.out.println("I should negate X");
            x = -x;

        System.out.println(x);
    }
}
```

> If 不加大括号就只会执行一个

与Python不同，大多数空格（包括缩进）与程序的功能无关。事实上，您可以用一个空格替换整个程序中的每个空格（假设分号是语句之间的分隔符），尽管这是一个可怕的想法，如果您编写的程序类似于以下有效的Java程序，我们会非常难过：

```java
public class ClassNameHere { public static void main(String[] args) { int x = 5; if (x < 10) { System.out.println("I shall increment x by 10."); x = x + 10; } if (x < 10) { System.out.println("I shall increment x by 10."); x = x + 10; } System.out.println(x); } }
```

## Curly Brace Standards

**There are two common styles for curly braces:**

```java
if (x > 5) {              if (x > 5)
    x = x + 5;            {
}                            x = x + 5;
                          }
```

Which of these two styles is “correct” is a bit of a holy war. **Both are fine for use in 61B**. Note that in this example, we’ve wrapped curly braces around a single statement, which isn’t required in Java. ==**In 61B, we’ll ALWAYS use curly braces, even if we have only one statement to execute.**== This is to avoid bugs.不要太担心这些小细节，如果你做了一些粗鲁的事情，自动风格检查器会对你大吼大叫。

For more than you ever wanted to know about **indentation styles (缩进样式)**, see [this wiki page](http://en.wikipedia.org/wiki/Indent_style).

# Else

The `else` keyword allows you to specify behavior that should occur if a condition’s result is false (if the condition is not met). For example, [this program](http://goo.gl/bzCra2) (or [this](https://goo.gl/ZbbFHx)), shown below:

```java
int x = 9;
if (x - 3 > 8) {
    System.out.println("x - 3 is greater than 8");
} else {
    System.out.println("x - 3 is not greater than 8");
}
```

We can also chain `else` statements, like in [this program](https://goo.gl/ggFo2c) (or [this](https://goo.gl/tAC9Pf)):

```java
int dogSize = 20;
if (dogSize >= 50) {
    System.out.println("woof!");
} else if (dogSize >= 10) {
    System.out.println("bark!");
} else {
    System.out.println("yip!");
}
```

Note that in the code above, I’ve used `>=`, which means greater than or equal.

# While

*Optional Supplementary Reading: [Shewchuk](https://sp21.datastructur.es/materials/hw/hw0/hw0_supplementary_loops.txt)*

The `while` keyword lets you repeat a block of code as long as some condition is true. For example, [this program](http://goo.gl/mB8DXi) (or [this](https://goo.gl/GQpkPx)), shown below:

```java
int bottles = 99;
while (bottles > 0) {
    System.out.println(bottles + " bottles of beer on the wall.");
    bottles = bottles - 1;
}
```

Important note: You should think of your program as running in order, line by line. If the condition becomes false in the middle of the loop, the code does not simply stop. 

# Doubles and Strings

以上，我们所有的变量都是“int”类型。还有许多其他类型可以在Java中使用。其中两个例子是“double”和“String”`double`存储实数的近似值，而String`存储字符串。[以下程序](http://goo.gl/25irIR)（也[此处](https://goo.gl/PKTBgW))模拟阿基里斯和乌龟之间的比赛。阿基里斯的速度是乌龟的两倍，因此应该超越乌龟（它的领先距离为100公里）。

```java
String a = "Achilles";
String t = "Tortoise";
double aPos = 0;
double tPos = 100;
double aSpeed = 20;
double tSpeed = 10;
double totalTime = 0;
while (aPos < tPos) { 
    System.out.println("At time: " + totalTime);
    System.out.println("    " + a + " is at position " + aPos);
    System.out.println("    " + t + " is at position " + tPos);

    double timeToReach = (tPos - aPos) / aSpeed;
    totalTime = totalTime + timeToReach;
    aPos = aPos + timeToReach * aSpeed;
    tPos = tPos + timeToReach * tSpeed;
}
```

# Defining Functions (a.k.a. Methods)

The following four pieces of code are all equivalent in Python, MATLAB, Scheme, and Java. Each defines a function that returns the maximum of two values and then prints the maximum of `5` and `15`.

## Python

```PY
def max(x, y):
    if (x > y):
        return x
    return y

print(max(5, 15))
```

## Scheme

```scheme
(define max (lambda (x y) (if (> x y) x y)))
(display (max 5 15)) (newline)
```

## Java

```java
public static int max(int x, int y) {
    if (x > y) {
        return x;
    }
    return y;
}

public static void main(String[] args) {
    System.out.println(max(10, 15));
}
```

(program link [1](http://goo.gl/LzWQD4), [2](https://goo.gl/8x5DYQ))

Java中的函数和变量一样，具有特定的返回类型。“max”函数的返回类型为“int”（在函数名前面用“int”表示）。Java中的函数也被称为方法，所以我将从现在开始永远这样调用它们。

==We refer to the entire string `public static int max(int x, int y)` as the method’s **signature**==, **as it lists the parameters, return type, name, and any modifiers.** Here our modifiers are `public` and `static`, though we won’t learn what these mean for a few days.

对于这个家庭作业，所有方法的签名前面都会有`public static`。现在就接受这个吧。我们将在周五的讲座中进一步讨论这个问题。

# Arrays

*Optional Supplementary Reading: [Shewchuk](https://sp21.datastructur.es/materials/hw/hw0/hw0_supplementary_arrays.txt)*

Our final new syntax item of this HW is the array. Arrays are like vectors in Scheme, lists in Python, and arrays in MATLAB.

The following four programs in Python, MATLAB, Scheme, and Java declare a new array of the integers `4`, `7`, and `10`, and then prints the `7`.

## Python

```py
numbers = [4, 7, 10]
print(numbers[1])
```

## Scheme

```scheme
(define numbers #(4 7 10))
(display (vector-ref numbers 1)) (newline)
```

## Java

```java
int[] numbers = new int[3];
numbers[0] = 4;
numbers[1] = 7;
numbers[2] = 10;
System.out.println(numbers[1]);
```

Or in an alternate (but less general) shorthand:

## Alternate Java

```
int[] numbers = new int[]{4, 7, 10};
System.out.println(numbers[1]);
```

([program link](http://goo.gl/U6xLky))

You can get the length of an array by using `.length`, for example, the following code would print `3`:

```java
int[] numbers = new int[]{4, 7, 10};
System.out.println(numbers.length);
```

# For Loops

Consider the function below, which sums the elements of an array.

```java
public class ClassNameHere {
    /** Uses a while loop to sum a. */
    public static int whileSum(int[] a) {
      int i = 0; //initialization
      int sum = 0;
      while (i < a.length) { //termination
            sum = sum + a[i];
            i = i + 1; //increment
      }
      return sum;
    }
}
```

Programmers in the 1950s observed that it was very common to have code that featured **==initialization==** of a variable, followed by a loop that begins by checking for a **==termination==** condition and ends with an **==increment==** operation. ==Thus was born the [`for` loop](https://en.wikipedia.org/wiki/For_loop).==

The `sum` function below uses a basic `for` loop to do the exact same job of the `whileSum` function above.

```java
public class ClassNameHere {
    /** Uses a basic for loop to sum a. */
    public static int sum(int[] a) {
      int sum = 0;
      for (int i = 0; i < a.length; i = i + 1) {
        sum = sum + a[i];
      }
      return sum;
    }
}
```

Try it out using [this link](https://goo.gl/5fvnE7).

In Java, the [`for` keyword](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/for.html) has the syntax below:

```java
for (initialization; termination; increment) {
    statement(s)
}
```

==**The initialization, termination, and increment must be semicolon separated. Each of these three can feature multiple comma-separated statements**==, e.g.初始化、终止和增量必须用分号分隔。这三个语句中的每一个都可以包含多个逗号分隔的语句，例如。

```java
for (int i = 0, j = 10; i < j; i += 1) {
    System.out.println(i + j);
}
```

**Comma separated `for` loops should be used sparingly.逗号分隔的“for”循环应谨慎使用。**

# Break and Continue

Occasionally, you may find it useful to use the `break` or `continue` keywords. The `continue` statement skips the rest of the current iteration of the loop, effectively jumping straight to the increment condition.

For example the code below prints each String from an array three times, but skips any strings that contain “horse”. You can try it out at [this link](https://goo.gl/SqX8oz).

```java
public class ContinueDemo {
    public static void main(String[] args) {
        String[] a = {"cat", "dog", "laser horse", "ketchup", "horse", "horbse"};

        for (int i = 0; i < a.length; i += 1) {
            if (a[i].contains("horse")) {
                continue;
            }
            for (int j = 0; j < 3; j += 1) {
                System.out.println(a[i]);
            }
        }
    }
}
```

By contrast, the `break` keyword completely terminates the innermost loop when it is called. For example the code below prints each `String` from an array three times, except for strings that contain horse, which are only printed once. You can try it out at [this link](https://goo.gl/e7X3E9).

```java
public class BreakDemo {
    public static void main(String[] args) {
        String[] a = {"cat", "dog", "laser horse", "ketchup", "horse", "horbse"};

        for (int i = 0; i < a.length; i += 1) {
            for (int j = 0; j < 3; j += 1) {
                System.out.println(a[i]);
                if (a[i].contains("horse")) {
                    break;
                }                
            }
        }
    }
}
```

`break` and `continue` also work for `while` loops and `do-while` loops. If you’re curious about `do-while` loops, see the [official Java looping tutorial](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/while.html).

# ==The Enhanced For Loop==

Java also supports iteration through an array using an “enhanced for loop”. ==**The basic idea is that there are many circumstances where we don’t actually care about the index at all**==. In this case, we avoid creating an index variable using a special syntax involving a colon.

For example, in the code below, we do the exact thing as in `BreakDemo` above. However, in this case, we do not create an index `i`. Instead, the `String` `s` takes on the identity of each `String` in `a` exactly once, starting from `a[0]`, all the way up to `a[a.length - 1]`. You can try out this code at [this link](https://goo.gl/wmhVPM).

```java
public class EnhancedForBreakDemo {
    public static void main(String[] args) {
        String[] a = {"cat", "dog", "laser horse", "ketchup", "horse", "horbse"};

        for (String s : a) {
            for (int j = 0; j < 3; j += 1) {
                System.out.println(s);
                if (s.contains("horse")) {
                    break;
                }                
            }
        }
    }
}
```