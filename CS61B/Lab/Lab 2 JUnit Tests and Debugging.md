						Lab 2: JUnit Tests and Debugging

# Before begin

## pom.xml

For all future assignments, instead of importing the entire folder like we did for lab1 and proj0, we’ll be importing a file called `pom.xml`.

**The reason is that the `pom.xml` tells IntelliJ where to find the library for 61B.** It is also possible to manually specify where the libraries are, but this process is a bit cumbersome (see the lab2setup directions from fa20 if you’re curious).

### One-Time Setup

 **you can go back to the home screen by navigating to `File > Close Project`.**

# Introduction

==**In this lab, you will learn about how to use the IntelliJ debugger and how to use JUnit tests in IntelliJ.**==

# Debugger Basics

Repeat the “Project Setup” process from lab 2 setup. However, this time, you should “open or import” the `lab2/pom.xml` file instead of the `lab2setup/pom.xml` file.

After importing, your IntelliJ should look something like the following:<img src="https://sp21.datastructur.es/materials/lab/lab2/img2/folder_structure.png" alt="folder structure" style="zoom:33%;" />

## Breakpoints and Step Into 断点和步入

We’ll start by running the main method in `DebugExercise1`. Open up this file in IntelliJ and click the run button. You should see three statements printed to the console(控制台), one of which should strike you as incorrect. If you’re not sure how to run `DebugExercise1`, right click on it in the list of files and click the `Run DebugExercise1.main` button as shown below:![run button](https://sp21.datastructur.es/materials/lab/lab2/img2/run_button.png)

我们的代码中有一个bug，但不要仔细阅读代码！虽然您可能能够发现这个特定的错误，但如果不真正尝试运行代码并在执行过程中探测发生了什么，通常几乎不可能看到错误。

你们中的许多人都有很多使用打印语句来探究程序运行时的想法的经验。虽然打印语句对调试非常有用，但它们有一些缺点：

1. They require you to modify your code (to add print statements).它们要求您修改代码（添加打印语句）。
2. They require you to explicitly state what you want to know (since you have to say precisely what you want to print).它们要求您修改代码（添加打印语句）。
3. And they provide their results in a format that can be hard to read, since it’s just a big blob of text in the execution window.它们以一种很难阅读的格式提供结果，因为它只是执行窗口中的一大块文本。

如果您使用调试器，通常（但并非总是）需要更少的时间和精力来查找错误。IntelliJ调试器允许您在执行过程中暂停代码，逐行逐步执行代码，甚至可视化复杂数据结构（如链接列表）的组织。

虽然调试器功能强大，但必须正确使用它们才能获得任何优势。我们鼓励您进行所谓的“科学调试”，即使用与科学方法非常相似的方法进行调试！

Generally speaking, you should formulate hypotheses about how segments of your code should behave, and then use the debugger to resolve whether those hypotheses are true. With each new piece of evidence, you will refine your hypotheses, until finally, you cannot help but stumble right into the bug.一般来说，您应该制定关于代码段应该如何运行的假设，然后使用调试器来确定这些假设是否正确。随着每一个新的证据，你都会完善你的假设，直到最后，你会跌跌撞撞地发现bug。

Our first exercise introduces us to two of our core tools, the `breakpoint` and the `step over` button. In the left-hand Project view, right click (or two finger click) on the `DebugExercise1` file and this time select the `Debug` option rather than the `Run` option. If the Debug option doesn’t appear, it’s because you didn’t properly import your `lab2` project (see the instructions in `lab2setup`).<img src="https://sp21.datastructur.es/materials/lab/lab2/img2/debug_button.png" alt="folder structure" style="zoom:33%;" />

<img src="https://sp21.datastructur.es/materials/lab/lab2/img2/breakpoint.png" alt="breakpoint" style="zoom: 25%;" />

> 可以把console拖到右边, 这样可以同时看见debugger和console

![step_into](https://sp21.datastructur.es/materials/lab/lab2/img2/step_into.png)

We’ll discuss the other buttons later in this lab. **Make sure you’re pressing ‘step into’ rather than ‘step over’.** Step-into points straight down, whereas step-over points up to the right and then down to the right.

Each time you click this button, the program will advance one step. **==Before you click each time, formulate a hypothesis about how the variables should change.==**

Note that the currently highlighted line is the line *that is about to execute*, not the line that has just executed.**注意，当前高亮显示的行是*即将执行的行*，而不是刚刚执行的行。**

重复此过程，直到找到一行结果与您的期望或编写代码的人的期望不符。试着弄清楚为什么这条线没有达到你的预期。**如果您第一次错过了错误，请单击停止按钮（红色方框），然后单击调试按钮重新开始。**或者，您可以在发现错误后修复它。

## Step Over and Step Out

就像我们依靠分层抽象来构建和组成程序一样，我们也应该依靠抽象来调试我们的程序。IntelliJ中的 "`step over` "按钮使这成为可能。前面练习中的 "`step into` "显示了程序的下一步，而 "`step over`"按钮允许我们在不显示函数执行的情况下完成一个函数调用。

DebugExercise2中的main 方法应该是取两个数组，计算这两个数组的元素最大值，然后将所得的最大值相加。例如，假设两个数组是{2, 0, 10, 14}和{-5, 5, 20, 30}。元素间的最大值是{2, 5, 20, 30}，例如在第二个位置，"0 "和 "5 "中较大的是5，这个元素间的最大值之和是2 + 5 + 20 + 30 = 57。

在所提供的代码中，有两个不同的错误。你在这个练习中的任务是修复这两个bug，但有一条特别的规则：**你不应该介入max或add函数，甚至不应该试图去理解它们。**这些都是非常奇怪的函数，它们使用语法（和糟糕的风格），以令人难以置信的愚笨方式完成简单的任务。如果你发现自己不小心踏入了这两个函数中的一个，请使用 "`step out`"按钮（一个向上指向的箭头）来逃脱。

**==即使不踏入这些函数，你也应该能够知道它们是否有一个错误。这就是抽象的光辉!==** 即使我不知道一条鱼在分子水平上是如何工作的，但在有些情况下，我可以清楚地知道一条鱼已经死了。

如果你发现其中一个函数有错误，你应该完全重写它，而不是试图去修复它。

现在我们已经告诉了你 "`step over` "的作用，试着探索它具体是如何工作的，并尝试找到这两个bug。如果你遇到右上方的使用运行（或调试）按钮一直在运行DebugExercise1的问题，请右击DebugExercise2来代替它运行。

> ==逐层找bug, 找到哪个就在这个地方设置新断点(而不是重新到达这个点)==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230121150430321.png" alt="image-20230121150430321" style="zoom:33%;" />

> **incompatible : 不兼容的**  

## Recap: Debugging

By this point you should understand the following tools:

- Breakpoints
- Stepping over
- Stepping into
- Stepping out (though you might not have actually used this feature for this lab)

However, this is simply scratching the surface of the features of the debugger! Feel free to experiment. For example you might try to figure out what the “Watches” tab does. **Another handy feature we won’t cover is the “Evaluate Expression” button** (one of the last buttons on the row of step into/over/out buttons – it looks like a calculator). **In Lab 3 and 4, we will also show off some additional debugger features.**

请务必查看我们的[调试指南(也在markdown的最后)](http://sp21.datastructur.es/materials/guides/debugging-guide.html)! 它讨论了本实验室中未涵盖的一些功能（例如，条件断点），但如果您需要，它仍然是调试的一个很好的概述！

# JUnit and Unit Testing

We now turn our attention to JUnit and Unit Testing, which were covered in lecture 3.

Unit Testing is a great way to rigorously(严格地) test each method of your code and ultimately(最终) ensure that you have a working project.

The “Unit” part of Unit Testing comes from the idea that you can break your program down into units, or the smallest testable part of an application. Therefore, **Unit Testing enforces good code structure (each method should only do “One Thing”), and allows you to consider all of the edge cases for each method and test for them individually.**

In this class, you will be using JUnit to create and run tests on your code to ensure its correctness. And when JUnit tests fail, you will have an excellent starting point for debugging. Furthermore, if you have some terrible bug that is hard to fix, you can use git to revert back to a state when your code was working properly according to the JUnit tests **(we’ll talk about how to revert your code to old versions when we get to lab 4)**.	在这个类中，您将使用JUnit在代码上创建和运行测试，以确保其正确性。当JUnit测试失败时，您将有一个很好的调试起点。此外，如果您有一些难以修复的可怕错误，您可以使用git恢复到根据JUnit测试代码正常工作时的状态（我们将在进入实验室4时讨论如何将代码恢复到旧版本）。

### JUnit Syntax

As discussed in lecture, JUnit tests are written in Java.

Open `ArithmeticTest.java`.

The first thing you’ll notice are the imports at the top (IntelliJ sometimes shortens these to `import ...`; just click on the `...` to expand this and see what exactly is being imported). These imports are what give you easy access to the JUnit methods and functionality that you’ll need to run JUnit tests. For more information, see the [Testing lecture video](https://www.youtube.com/playlist?list=PL8FaHk7qbOD4ZfVY8g8lo5dFrLP-ctGmT).

Next, you’ll see that there are two methods in `ArithmeticTest.java`: `testProduct` and `testSum`. These methods follow this format:

```java
@Test
public void testMethod() {
    assertEquals(<expected>, <actual>);
}
```

`assertEquals` is a common method used in JUnit tests. It tests whether a variable’s actual value is equivalent to its expected value.

When you create JUnit test files, you should precede each test method with a `@Test` annotation, and can have one or more `assertEquals` or `assertTrue` methods (provided by the JUnit library). **All tests must be non-static.** This may seem weird since your tests don’t use instance variables and you probably won’t instantiate the class. However, this is how the designers of JUnit decided tests should be written, so we’ll go with it.创建JUnit测试文件时，应在每个测试方法之前添加“@test”注释，并且可以有一个或多个“assertEquals”或“assertTrue”方法（由JUnit库提供）==**所有测试都必须是非静态的。**这可能看起来很奇怪，因为您的测试不使用实例变量，而且您可能不会实例化类。然而，JUnit的设计者就是这样决定测试应该如何编写的==，所以我们将继续。

# Running JUnit Tests in IntelliJ

With `ArithmeticTest.java` open, click the `Run...` option under the `Run` menu at the top of IntelliJ as shown in the following screenshot.

![Run Options](https://sp21.datastructur.es/materials/lab/lab2/img/lab3_run.png)

After clicking “Run…”, you should see some number of options that will look something like the list below. The number of items in your list may vary.

![Run Options](https://sp21.datastructur.es/materials/lab/lab2/img/lab3_run_menu.png)

The option we care about is the one that says “ArithmeticTest” next to the red and green arrows (next to the 1. in the image above).

Select this one, and you should see something like:

![Run Options](https://sp21.datastructur.es/materials/lab/lab2/img/default_renderer.png)

这意味着“`ArithmeticTest.java`”第25行的测试失败。测试预期5+6为11，但“算术”类要求5+6为30。您将看到，即使“testSum”包含许多“assert”语句，也只显示一个失败。

这是因为JUnit测试是短路的——一旦方法中的一个断言失败，它将输出失败并继续下一个测试。

尝试单击屏幕底部窗口中的“`ArithmeticTest.java:27`”，IntelliJ将直接带您进入导致测试失败的行。在以后的项目上运行自己的测试时，这会很有用。

Now fix the bug, either by inspecting `Arithmetic.java` and finding the bug, or using the IntelliJ debugger to step through the code until you reach the bug.

# Application: IntLists

As discussed in Monday’s lecture, an `IntList` is our CS61B implementation for a naked recursive linked list of integers. Each `IntList` has a `first` and `rest` variable. The `first` is the `int` element contained by the node, and the `rest` is the next chain in the list (another `IntList`!).

We have created a file `IntListExercises.java` that contains three methods, each of which are buggy. Your task in this section is to find and fix the bugs! To assist you, we’ve added some helpful starter code and test skeletons, which we explain below.

## Starter Code

Added to our implementation in Monday’s lecture are two methods in the `IntList` class, `print` and `of`. The `of` method is a convenience method for creating `IntList`s. Here’s a quick demonstration of how it works. Consider the following code that you’ve seen in lecture for creating an `IntList` containing the elements 1, 2, and 3.

```java
IntList lst = new IntList(1, new IntList(2, new IntList(3, null)));
```

That’s a lot of typing, and is quite confusing! The `IntList.of` method addresses this problem. To create an IntList containing the elements 1, 2, and 3, you can simply type:

```java
IntList lst = IntList.of(1, 2, 3);
```

Isn’t that great?! It works for lists of any number of elements!

```java
// Creates an empty list!
IntList empty = IntList.of();

// Creates an IntList one element, 7
IntList oneElem = IntList.of(7);

// Creates an IntList with many elements
IntList manyElems = IntList.of(5, 4, 3, 2, 1);
```

The other method `print` returns a String representation of an IntList.

```java
IntList lst = IntList.of(1, 2, 3);
System.out.println(lst.toString())

// Output: 1 -> 2 -> 3
```

These methods don’t add any real functionality to the `IntList` class per-se, but they do provide convenient ways of creating and displaying `IntList`s, respectively. We use these convenience methods to make testing easier, and you will get some practice with these methods when you write your own JUnit test for debugging `IntList`s!

## Part A: IntList Iteration

In this part, we will be debugging the `addConstant` method in `IntListExercises.java`. This method is intended to take in an `IntList` and mutatively add a constant to each element of the list.

```java
/* Expected Behavior */

IntList lst = IntList.of(1, 2, 3);

addConstant(lst, 1);
System.out.println(lst.toString());
// Output: 2 -> 3 -> 4

addConstant(lst, 4);
System.out.println(lst.toString());
// Output: 6 -> 7 -> 8
```

Uh oh! The `addConstant` implementation we have provided in the starter code is buggy! We have provided three tests in `AddConstantTest.java` that can help you isolate the bug. These tests exercise the `IntList.toString` and `IntList.of` methods mentioned above! Step through each of these tests with the Java Debugger to help you isolate the bug. Once you’ve isolated the bug, fix it.

## Part B: Nested Helper Methods and Refactoring for Debugging

In this part, we will be debugging the `setToZeroIfMaxFEL` method in `IntListExercises.java`.

This method performs a very strange task. Specifically, it replaces the value at a node in an IntList with 0 if (and only if) the max of the IntList *starting at that node* has the same first and last digit. Thus, in the method name `FEL` is an abbreviation for “first equals last”.

For example, if we pass the IntList `55 -> 22 -> 45 -> 44 -> 5` it will set the values 55, 44, and 5 to zero so that the list becomes `0 -> 22 -> 45 -> 0 -> 0`. This is because:

- The IntList starting from 55 has max value 55, which has the same first and last digit, so this value is set to zero.
- The IntList starting from 22 has max value 45, which does not have the same first and last digit, so 22 is not changed.
- The IntList starting from 45 has max value 45, which does not have the same first and last digit, so 45 is not changed.
- The IntList starting from 44 has max value 44, which has the same first and last digit, so 44 is set to zero.
- The IntList starting from 5 has max value 5, which has the same first and last digit, so 5 is set to zero.

To test your understanding, consider the IntList `5 -> 535 -> 35 -> 11 -> 10 -> 0`. What should be the list after calling `setToZeroIfMaxFEL` is called? Check our answer by looking at `testZeroOutFELMaxes3` in `SetToZeroIfMaxFELTest`.

If you run the tests, you’ll see that the method is buggy. Specifically, test 3 fails.

Set a breakpoint on the first line of `setToZeroIfMaxFEL` and debug only `testZeroOutFELMaxes3`. You can debug a single test by opening `SetToZeroIfMaxFELTest.java`, locating the method `testZeroOutFELMaxes3()`, and clicking on the green arrow icon to the left of the method definition. If the test has already been run before, the green arrow icon may turn into a green checkmark coupled with the green arrow (if the test passed before) OR it may turn into a red exclamation mark coupled with a green arrow (if the test failed before). An example of the latter two cases is depicted below.![how-to-run-a-single-test](https://sp21.datastructur.es/materials/lab/lab2/img2/how-to-run-a-single-test.png)

Use step-in a couple of times, which will take you to the line that says `if (firstDigitEqualsLastDigit(max(p)))`. When you click step-in a third time, you’ll see both `firstDigitEqualsLastDigit` and `max` get highlighted, as shown below:

![debugger wants you to pick a function](https://sp21.datastructur.es/materials/lab/lab2/img2/debuggerPickAFunction.png)

Since we have a nested function call, IntelliJ is asking us which function we’d like to step into. If you’d like, you can click on one or the other. If you click on `max`, you’ll see all the details of the call to `max`. If you click on `firstDigitEqualsLastDigit`, the call to `max` will get stepped-over.

Personally, I find code like this hard to debug! One tactic I use in circumstances like this is to refactor my code to make it more debugging friendly. Let’s try this out!

Change the code so that it looks like this:

```java
    int currentMax = max(p);
    boolean firstEqualsLast = firstDigitEqualsLastDigit(currentMax);
    if (firstEqualsLast) {
        p.first = 0;
    }
```

Now that you’ve done this, use the step-over feature to identify which call to `max` or `firstDigitEqualsLastDigit` is yielding the wrong answer. **Important: Don’t use step-in until you’ve found a call to `max` or `firstDigitEqualsLastDigit` that yields the wrong answer.** Otherwise you’re just wasting time running through every single line of code. That is, if you’re watching every single iteration of every single call of the max function, you’re not using the debugger properly!

Once you’ve found a call to `max` or `firstDigitEqualsLastDigit` that yields a weird result, start the debugging process over, and this time, when you get back to the argument that yielded the weird result, click step-in instead of step-out. Note: In Lab 4, we’ll talk about a useful idea known as a “conditional breakpoint” that will avoid the need to start back over from the beginning.

Once you’ve identified the bug, fix it. Also feel free to change the refactored code back into the one-line version i.e. `if (firstDigitEqualsLastDigit(max(p)))`.

Note: In the real world, you would have ideally tested `max` and `firstDigitEqualsLastDigit` separately before using them in the `setToZeroIfMaxFEL` method.

## Part C: Tricky IntLists!

In this part, we will be debugging the `squarePrimes` method in `IntListExercises.java`. This method is intended to take in an `IntList`, square all its prime elements, and leave the composite (not-prime) elements alone. It returns `true` if at least one of the elements got squared, and `false` otherwise.

As an example, consider an `IntList` containing the elements 14, 15, 16, 17, and 18. After running `squarePrimes`, we expect the prime element (17) to be squared and the composite elements (14, 15, 16, 18) to remain the same. The output of the `squarePrimes` function should `true`, since it should modify the `IntList` to become `14, 15, 16, 289, 18`. In code:

```java
/* Expected Behavior */

IntList lst = IntList.of(14, 15, 16, 17, 18);
System.out.println(lst.toString());
// Output: 14 -> 15 -> 16 -> 17 -> 18

boolean changed = squarePrimes(lst);
System.out.println(lst.toString());
// Output: 14 -> 15 -> 16 -> 289 -> 18

System.out.println(changed);
// Output: true
```

The `squarePrimes` method uses the function `Primes.isPrime(int x)` as a helper method. `isPrime` simply returns `true` if its argument is a prime number, and returns `false` if its argument is composite. You don’t have to worry about how it determines whether a number is prime or not – that’s rather complicated! (Optional: For the curious reader, check out the “Fermat Primality Test” online). Instead, we expect you to treat the `isPrime` function as a blackbox. While you are debugging, use the “Step Over” feature on the `isPrime` function. This will allow you to verify its inputs and outputs are correct without being concerned about its implementation.

We’ve explained what the `squarePrimes` method is supposed to do above. Unfortunately, the `squarePrimes` method is buggy. It’s your job to find and fix the bug! In order to do this, we recommend that you:

1. Create JUnit Test(s) that test the `squarePrimes` method over a variety of different inputs. Make sure to test that it makes updates to the passed-in `IntList` correctly *and* returns the correct `boolean` value.
2. Once you’ve created a JUnit Test on which `squarePrimes` fails, you’ve made progress! Woohoo! Now use the Java Debugger to step through the problem and isolate the bug.
3. Finally, write a fix to the bug. For this particular bug, the fix is not many lines of code. Finding the bug is much more difficult than fixing it!

To get you started on this task, we’ve created one JUnit Test for you, `SquarePrimesTest.testSquarePrimesSimple`. This method checks that the example we gave above (an `IntList` of 14, 15, 16, 17, and 18) is correctly modified by the `squarePrimes` function, and that the `squarePrimes` function returns the correct value (`true` in this case). Unfortunately this test passes: You’ll have to write another test that fails! You can use `testSquarePrimesSimple` as an example of how to write a JUnit Test. Good luck!

# Submission

As before, push your code to GitHub and submit to Gradescope to test your code. One thing you’ll notice is that some of the tests are “Hidden”. This means that we don’t reveal to you what the test does, and if you fail the test, we give a purposefully vague error message. This is because for this lab, we want you to focus on learning how to debug by yourselves without relying on informative messages from the autograder.

# Full Recap

In this lab, we went over:

- Stepping into, over, and out inside the IntelliJ debugger (this will be handy for projects!)
- Unit Testing (big picture)
- JUnit syntax and details
- Writing JUnit tests
- Debugging Using JUnit
- Running the Style Checker

# FAQ and Common Issues

## Things like String or String.equals() are red!

This is a JDK issue, go to File > Project Structure > Project > Project SDK to troubleshoot. If your Java version is 15.0, then you should have a 15.0 SDK and a Level 15 “Project Language Level”.





# Debugging Guide

## A. What is Debugging?

Whenever you write a lot of code, as you do in this class, you will come across errors of various types. Some of these errors, like syntax errors will be easy to catch because the compiler will let you know exactly where they are. Unfortunately, errors in the logic of your code are not so simple. They are less likely to be caught by the system because oftentimes your code compiles, it just returns the wrong value. IntelliJ (and most other IDEs) provide an array of tools that will help you trace through your code and see exactly where an inconsistency occurs.

## B. Why Should I Learn to Debug?

The ability to debug is an extremely important part of being a programmer - no matter how clever you are, if it takes you a very long time to find out where your code is messing up, it will take you a very long time to finish a project. Every single programmer needs to know how to effectively debug. Within the scope of this class, learning to debug on your own will save you from having to wait in office hours for a TA to help you. With that in mind, this cheat sheet will give you all the tools you need to debug various types of errors!

## C. Can I Use Print Statements Instead?

Technically, yes. There are times when print statements are more efficient, such as when you only need to track one variable through a loop in a fairly isolated portion of code. You are free to debug at first with print statements and then use the debugger if you are still struggling to find the bug after 5 minutes. The caveat here is that if you come to office hours without having had stepped through your code, we will point you towards this guide instead of finding your bug.

## D. Understanding The Stack Trace

![Sample Stack Trace](https://sp21.datastructur.es/materials/guides/img/debugging-stack-trace.png)

When you see an exception like this, this means your computer caught a mistake while it was running your program. To be helpful, before it exits out, your computer tells you all the information you need to know to find out where the error happened. The first line follows this format:

```
[Which thread errored] [What error occurred] [Any more information about the error]
```

What we want to focus on is the last two segments. In this case, that looks like:

```
Exception in thread “main” java.lang.ArrayIndexOutOfBoundException: Index 5 out of bounds for length 5
```

This gives you two pieces of information:

1. The error was an *Array Index Out of Bounds Exception*.
2. We tried to access item 5 in an array of length 5.

This tells you what happened, but not where it happened. That’s where the stack trace is useful. The three (or however many) lines under the header describe where the computer was in the code when it errored. The top trace line is what the computer was executing when it crashed and the list describes what functions called each other in reverse order. Here, we see that our `main` function called `make2DArray` on line 6. Then, `make2DArray` called `makeArray` on line 14. Finally, the code errored on line 23 within `makeArray`. Clicking on the blue links will jump you to that part of the code, so you don’t have to spend time scrolling. Knowing this, you can start debugging the specific sequence of calls that caused the error.

## E. What Does This Error Mean

Perhaps you have read the stack trace, but you don’t understand what the error means. For the most part, Java errors are named such that they are understandable without prior knowledge, but in case you come across something you don’t recognize, here’s a cheat sheet:

| Error                                                    | What it Usually Means                                        |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| ___ expected                                             | The parser can’t make sense of the line because there’s a character that it doesn’t understand or a missing character. |
| cannot find ___                                          | You are calling a method or class that the computer doesn’t have access to. |
| Illegal start of expression                              | You are missing a closing brace somewhere before this line.  |
| Illegal start of type                                    | You wrote code outside of a function body that shouldn’t be there. |
| Incompatible types – expected ___                        | You are trying to assign something to a variable that is not the same type. |
| Missing method body                                      | Your function declaration line has a semicolon.              |
| Missing return statement                                 | You should be returning something in this method but you aren’t. |
| Non-static method cannot be called from a static context | You called a method on the class itself instead of an instance of the class. |
| *Program Freezes*                                        | You’re likely stuck in some sort of logical loop.            |

## F. Stepping Through With the Debugger

#### Running Code Through IntelliJ

![Run Menu](https://sp21.datastructur.es/materials/guides/img/debugging-edit-configurations.png)

To use the debugger, you need to run your code through IntelliJ instead of through the console.

1. From the main menu, go to Run > Edit Configurations.
2. Enter any arguments for your program into the field marked Program Arguments. These are the arguments you would pass into the command line if you were to run it in the console.
3. To run the debugger, either click Run > Debug or right click on the green
4. arrow next to the function and select Debug.

#### Setting Breakpoints

To examine how the code operates at runtime, we set *breakpoints*. Breakpoints pause your code at the line they are set so that you can see the state of all the variables around where an error occurred. To set a breakpoint, click on the space between a line number and the code:

![Normal Breakpoint](https://sp21.datastructur.es/materials/guides/img/debugging-normal-breakpoint.png)

This kind of breakpoint just pauses the code the first time your computer comes across this line. If you are in a situation where the error only occurs when a variable is set to a certain value, you can set a *conditional breakpoint*.

![Conditional Breakpoint](https://sp21.datastructur.es/materials/guides/img/debugging-conditional-breakpoint.png)

To do this, set a normal breakpoint and then right click the red circle that appears. Now, you can set the breakpoint condition in the given field. Your condition can be any True/False statement that would compile at this point in the code. This means you have to use variables that already exist in the current frame, ==**but they don’t necessarily have to be referenced in the current line.**==

#### Stepping Through Code

###### Top Toolbar 顶部工具栏

![Top Toolbar](https://sp21.datastructur.es/materials/guides/img/debugging-top-toolbar.png)

To step through code, you need to understand the toolbar at the top of the debug view. To see their names, hover over the icons. From left to right, they are:

| Button          | What It Does                                                 |
| --------------- | ------------------------------------------------------------ |
| Step Over       | This allows you to execute the current line of code and move on to the next line in this frame. |
| Step Into       | This allows you to step into any function calls in the current line given the functions are yours. |
| Force Step Into | This allows you to step into any function calls in the current line even if they are from some third party library. You should not need to do this. |
| Step Out        | If you are in a function or loop, this allows you to skip the rest of the frame, essentially bringing you out to wherever this function was called. |
| Drop Frame      | This allows you to reset the current frame by returning to the previous frame where it was called. This is useful if you missed the part of a function you were trying to see by essentially letting you rewind time. |
| Run to Cursor   | If you are running in debug mode, you can quickly jump to areas of interest in the code by clicking run to cursor. This will act as if there is a temporary breakpoint set wherever your cursor is. |

###### Left Toolbar

![Left Toolbar](https://sp21.datastructur.es/materials/guides/img/debugging-left-toolbar.png)

There is also a second toolbar along the left side of the debug menu. This menu is for more general controls. From top to bottom they are:

| Button                   | What It Does                                                 |
| ------------------------ | ------------------------------------------------------------ |
| Rerun                    | Rerun the debugger with the same settings as the current run. |
| ==Resume Program==       | Continues the program until it hits the next breakpoint.==继续程序，直到它到达下一个断点。== |
| ==Pause Program==        | ==**If your program seems like it’s freezing, run it in debug mode with no breakpoints and click pause when your program freezes. It will most likely pause within whatever logical loop is causing the freeze.如果您的程序似乎正在冻结，请在没有断点的调试模式下运行它，并在程序冻结时单击暂停。它很可能会在导致冻结的任何逻辑循环内暂停。**== |
| Stop Program             | If you’re done debugging, you can click this to end the program early. |
| View Breakpoints         | This opens up a window that displays all your current breakpoints. Here, you can edit their settings and toggle them on/off.这将打开一个窗口，显示所有当前断点。在这里，您可以编辑它们的设置并打开/关闭它们。 |
| Mute Breakpoints静音断点 | Toggles(切换开关) all breakpoints on/off.                    |

#### Analyzing the Current State

There are two places where you can get information about the state of your program while in debug mode.

###### Debug View

![Debug View](https://sp21.datastructur.es/materials/guides/img/debugging-debug-view.png)Firstly, in the debug view there are two columns:

1. The first column shows the stack trace up until this point. Each line describes a frame, in order of narrowest to widest. The name of the frame is first and refers to the name of the function called. Then, we see the line number we are currently on for that frame. In this example, `make2DArray` called `makeArray` on line 14, so it remains on that line until `makeArray` finishes running.
2. The second column lists all the variables in the current frame, which includes any global variables. You can’t see variables that don’t exist in the current frame. Here you can see what value they hold as of the line you are on.

If you want more information than just the values of each variable, there’s a button called *evaluate expression* in the toolbar. Pressing this allows you to essentially insert lines on the fly to the program.

![Evaluate Expression](https://sp21.datastructur.es/materials/guides/img/debugging-evaluate-expression.png)

For instance, here we have inserted the expression `arr[i] = 1000` and clicked *evaluate*, which is reflected in the *Result Panel* and in the *Variables Panel*. Essentially, it is as if we inserted the line `arr[i] = 1000` before `arr[i] = makeItem(i)`. A good use of this functionality is to ensure that two objects in your code are equal (rather than being two instances of the same class) by evaluating a == b which can be difficult to tell from the Variables Panel alone. This has the added benefit of not changing any values in the code, so you don’t have to worry about accidentally modifying the behavior you trying to observe.

> **==因为高亮的行是将要执行的行, 不是已经执行的行==**

###### Code View

The second place to get information is on the code itself.

![Code View](https://sp21.datastructur.es/materials/guides/img/debugging-code-state.png)

在调试模式下，IntelliJ显示每行代码中每行旁边引用的每个变量的值。此外，它突出显示了最近更改的任何变量的值。

## G. More Resources

If you want more details on using the IntelliJ debugger, JetBrains has some guides:

1. [Debugging Your First Application](https://www.jetbrains.com/help/idea/debugging-your-first-java-application.html)
2. [Setting Breakpoints](https://www.jetbrains.com/help/idea/using-breakpoints.html)
3. [Stepping Through a Program](https://www.jetbrains.com/help/idea/stepping-through-the-program.html)