

​											Lab 3: Timing Tests and Randomized Comparison Tests

> ==计时测试和随机比较测试==

## Introduction

在这个实验室中，您将为“`SLList`”和“`AList`”类创建一个计时测试。您还将为“`AList`”类的错误实现创建随机比较测试。

**Along the way, we will also explore three new debugger features: ==conditional breakpoints, the resume button, and execution breakpoints.== 在此过程中，我们还将探索三个新的调试器特性：条件断点、恢复按钮和执行断点。**

## Timing Tests for List61B

对于实验室的这一部分，请确保您打开的是“`timingtest`”包中的代码，而不是“`randomizedtest`”包的代码。

### Timing the construction of an AList with a bad resize strategy

如[lecture](https://docs.google.com/presentation/d/1ZKSPKdEjlLlzmf7LoQJlTUC3w0MPInSXy2DxTEva0yo/edit#slide=id.g625dc7e36_0943)所述，乘法调整大小策略将导致快速添加操作/良好性能，而加法调整大小策略会导致缓慢添加操作/较差性能。在实验室的这一部分，我们将展示如何用经验证明这一点。

在“`timingtest`”包中，我们提供了讲座中创建的“`AList`”类，其中包含以下错误的调整大小策略：

```java
    public void addLast(Item x) {
        if (size == items.length) {
            resize(size + 1);
        }

        items[size] = x;
        size = size + 1;
    }
```

这部分实验室的目标是编写代码，使用上面的“`addLast`”方法将创建各种大小的“`AList`”所需的时间列表。此计时测试的输出将如下所示：

```
Timing table for addLast
           N     time (s)        # ops  microsec/op
------------------------------------------------------------
        1000         0.00         1000         0.20
        2000         0.01         2000         0.20
        4000         0.01         4000         1.20
        8000         0.04         8000         4.30
       16000         0.10        16000        10.00
       32000         0.50        32000        49.70
       64000         1.15        64000       114.80
      128000         3.74       128000       374.30
      
    //数据的规模		 总的时间	  调用addLast方法的次数   平均每次方法调用所需要的微秒数
```

The first column `N` gives the size of the data structure (how many elements it contains). The second column `time (s)` gives the time required to complete all operations. The third column `# ops` gives the number of calls to `addLast` made during the timing experiment. And finally the fourth column `microsec/op` gives the number of microseconds it took on average to complete each call to `addLast`. Note that for this experiment, `N` and `# ops` is redundant, since the result of making 128,000 calls to `addLast` will result in an `N` of 128,000.

The important thing to notice here is that `addLast` is not “constant time”. That is, the time it takes each `addLast` call to complete varies significantly with the size of the list: 374.30 microseconds when the list is long, and only 0.20 microseconds when the list is short. This is essentially how our autograder tests work for the `LinkedListDeque` and `ArrayDeque` classes from project 1, i.e. we make sure that the time was constant for operations that should have been constant.

您可能会注意到，对于N=1000和N=2000，每个“`addLast`”操作的时间是相同的。这在定时测试中很常见。对于小输入，结果是不可靠的，原因有两个：运行时的差异很大（由于缓存(caching)、进程切换(process switching)、分支预测(branch prediction)等问题，如果您使用61C，您将了解这些问题），而且我们的计时器（毫秒）的精度不足以解决N=1000和N=2000之间的差异。这甚至会导致N=1000的运行时间大于N=2000的运行时间。**因此，当我们运行经验时序测试时，我们希望关注大N的行为**，例如N=32000 vs N=64000。

您可能会注意到的另一件事是，您从该表中获得的机器时间可能与上面所写的时间有很大不同。没关系，只要“总体趋势(general trend)”是一样的。在61C中，您将看到为什么相同的代码在不同的硬件上花费的时间会有很大的不同。在61B中（以及在大多数基于理论的课程中），我们只关注一般趋势，这些趋势掩盖了非理想性，例如您使用的处理器类型。虽然关于“总体趋势”的推理可能看起来很棘手，但我们将在本课程的稍后部分学习形式主义(formalism)（渐近线(asympotics)）。现在，用你的直觉！

现在您已经了解了上面的表，请将函数“`public void timeAListConstruction`”添加到为“`AList`”生成上面表的类“`TimeAList`”中。注意：如果您的计算机速度有点慢，您可能希望停止在64000而不是128000。确保将对“`timeAListConstruction`”的函数调用添加到“`TimeAList`”类的“`main`”方法中。

For your convenience, we’ve provided a method called `printTimingTable(AList<Integer> Ns, AList<Double> times, AList<Integer> opCounts)` that will print the table above, where `Ns` is the first column, `times` is the second column, and `opCounts` is the third column. The fourth column (`microsec/op`) is automatically computed for you. Your times should be in seconds. You should use the `Stopwatch` class. ==See `stopwatchDemo` for an example. Interestingly, note that we are using the same data structure that we are timing (an `AList`) to store the timing data!== The way we accomplish this is by using many different instances of the data structure; the one that is being timed is not the same as the ones that are storing the timing data.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124131928567.png" alt="image-20230124131928567" style="zoom:33%;" />

### Timing the construction of an AList with a good resize strategy

Now modify the `AList` class so that the resize strategy is multiplicative instead of additive and rerun `timeAListConstruction`. Your `AList` objects should now be constructed nearly instantly, even for N = 128,000, and each add operation should only take a fraction of a microsecond.

Optional: Try increasing the maximum N to larger values, e.g. 10 million. You should see that the time per add operation remains constant.可选：尝试将最大N增加到更大的值，例如1000万。您应该看到每次添加操作的时间保持不变。

可选：尝试使用不同的调整大小因子，并查看运行时的变化。例如，如果您按1.01的因子调整大小，您仍然应该得到恒定的时间addLast操作！注意，要使用非整数因子，需要将其转换为最接近的整数。例如，可以使用“`Math.round()`”。

```java
    public void addLast(Item x) {
        if (size == items.length) {
            resize((int) (size * 1.01));
        }

        items[size] = x;
        size = size + 1;
    }
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124132230226.png" alt="image-20230124132230226" style="zoom:33%;" /><img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124132056159.png" alt="image-20230124132056159" style="zoom: 33%;" />

> 左边是bad , 右边是better

### Timing the getLast method of SLList

上面，我们展示了如何对数据结构的**构建进行计时**。然而，有时我们感兴趣的是方法的**运行时依赖于已经构建的现有数据结构的大小**。

For example, in your `LinkedListDeque`, you are supposed to have `addLast` operations that are fast… a single `addLast` operation must take “constant time”, i.e. execution time should not depend on the size of the deque.

In this part of the lab, we’ll show you how to empirically test whether a method’s runtime depends on the size of the data structure.

Suppose we want to compute the time per operation for `getLast` for an `SLList` and want to know how this runtime depends on N. To do this, we need to follow the procedure below:

1. Create an `SLList`.
2. Add N items to the `SLList`.
3. Start the timer.
4. Perform M getLast operations on the `SLList`.
5. Check the timer. This gives the total time to complete all M operations.

**==It’s important that we do not start the timer until after step 2 has been completed.(不能把构建的时间也加进去)==** Otherwise the timing test includes the runtime to build the data structure, whereas we’re only interested how the runtime for `getLast` depends on the size of the `SLList`.**重要的是，在第2步完成之前，我们不要启动计时器。**否则，计时测试包括构建数据结构的运行时，而我们只关心“getLast”的运行时如何取决于“`SLList`”的大小。

In the `TimeSLList` class, edit the function `timeGetLast` to perform the procedure above, and generate a table similar to the one shown below:

```
Timing table for getLast
           N     time (s)        # ops  microsec/op
------------------------------------------------------------
        1000         0.02        10000         1.70
        2000         0.03        10000         3.10
        4000         0.06        10000         6.20
        8000         0.13        10000        12.50
       16000         0.25        10000        25.00
       32000         0.53        10000        52.80
       64000         1.35        10000       135.30
      128000         2.57        10000       257.30
```

Note that the `N` and `# ops` columns are no longer the same. This is because we are always calling `getLast` the same number of times regardless of the size of the list, i.e. `M = 10000` for step 4 of the procedure described above.

Note that the operations are again not constant time! (If your results imply that the operations *are* constant time, make sure you’re running your tests on the `SLList` instead of the `AList`!). This means that as the list gets bigger, the `getLast` operation becomes slower. This would be a serious problem in a real world application. For example, suppose the list is of ATM transactions, and the `getLast` operation was being called in order to get the most recent transaction to print a receipt. Every time the ATM is used, the next receipt would take a little bit longer to print. Eventually over many months or years, the list would become so large that the `getLast` operation would be unusably slow. While this is a contrived example, similar problems have plagued real world systems!

因此，需要在项目1中构建的“`LinkedListDeque`”具有独立于数据结构大小的运行时。换句话说，最后一列将是某个近似恒定的值。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124133825458.png" alt="image-20230124133825458" style="zoom:33%;" /><img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124133850257.png" alt="image-20230124133850257" style="zoom: 50%;" />

**Optional question to ponder: Why is `getLast` so slow? What is special about your `LinkedListDeque` that makes the `getLast` function faster?**

## Randomized Comparison Tests(下一个主题, 随机比较测试)

For this part of the lab, make sure you’re using the code in the `randomizedtest` package, not the `timingtest` package.

#### Simple Comparison Test

测试代码的一种技术是进行“比较测试”。在这样的测试中，我们有两个相同类的实现。一种实现是已知的（或强烈认为）正确的，另一种正在开发中，尚未验证。

例如，我们提供了“`AListNoResizing`”类。**这个类不支持任何调整大小的操作，只是具有1000的硬编码数组大小。**这意味着它实际上没有什么用处，因为它永远不能容纳超过1000件物品。然而，由于它非常简单，我们对它的工作很有信心。

相比之下，我们还提供了“`BuggyAList`”类。这个类有一个基础数组，根据存储的数据量上下调整大小。由于调整大小有点棘手，所以我们更怀疑这个类的正确性。顾名思义，它确实有一个bug。这个实验室的其他人的目标是找到这个bug。

让我们先写一个JUnit测试练习作为热身。编写一个名为“`testThreeAddThreeRemove”`的测试，将相同的值添加到正确和错误的AList实现中，然后检查三个后续removeLast调用的结果是否相同。例如，您可以将“`addLast`”4同时添加到两者，然后将“`addLast`”5同时添加到二者，然后将`addLast`6同时添加到这两者。然后，您将从两者中“`removeLast`”，并验证结果是否相等。然后再次从两者中删除Last，并检查它们是否相等。最后，您将“`removeLast`”(即4项), 并验证它们是否相等。当你完成后，或者如果你被卡住了，请查看代码[在此链接](https://sp21.datastructur.es/materials/lab/lab3/testThreeAddThreeRemove.txt)对于解决方案，请先尝试而不查看代码！

运行测试，你会发现它应该通过了。这个测试不够强大，无法识别“`BuggyAList`”中的错误。<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124140449153.png" alt="image-20230124140449153" style="zoom: 50%;" />

#### Randomized Function Calls

In principle, it’s possible to carefully craft a set of comparison tests that will eventually find the bug. **However, an alternate and complementary(互补的) strategy is to use a randomized approach where we make random calls to both implementations and use JUnit methods to verify that they always return the same values.**

作为一个随机调用“`AList`”方法的函数的示例，下面的代码随机调用“`AListNoResizing`”对象上的“`addLast`”和“`size`”，总共调用N个函数。

```java
        AListNoResizing<Integer> L = new AListNoResizing<>();

        int N = 500;
        for (int i = 0; i < N; i += 1) {
            int operationNumber = StdRandom.uniform(0, 2);
            if (operationNumber == 0) {
                // addLast
                int randVal = StdRandom.uniform(0, 100);
                L.addLast(randVal);
                System.out.println("addLast(" + randVal + ")");
            } else if (operationNumber == 1) {
                // size
                int size = L.size();
                System.out.println("size: " + size);
            }
        }
```

Create a new JUnit test called `randomizedTest()` and copy and paste the code above and you should see something like this:

```
size: 0
addLast(68)
size: 1
size: 1
addLast(12)
addLast(19)
addLast(79)
...
size: 265
```

#### Conditional Breakpoints

Though it won’t be useful for finding the bug today, let’s introduce two new debugger features: ==“resume” and “conditional breakpoints”==.虽然现在它对查找bug没有什么用处，但让我们介绍两个新的调试器功能：“恢复”和“条件断点”。

1. Set a breakpoint on the line that says `int operationNumber = StdRandom.uniform(0, 2);`.
2. Then use the debug option to stop on this line in IntelliJ. If you don’t remember how to use the debug option, see [lab 2](https://sp21.datastructur.es/materials/lab/lab2/lab2).
3. Click the visualizer, and you’ll see an array with lots and lots of nulls that will eventually store the data being added to our list.
4. Click step over, and you’ll see that operationNumber is set to either 0 or 1. This is because the `StdRandom.uniform(0, 2)` function returns a random integer in the range [0, 2), i.e. exclusive of the right argument. If the chosen number is 0, then a random number will be added to the end of the list. If the chose number is 1, then the size will be printed.
5. Click the `resume` button on the debugger, highlighted in yellow below, and our code will run into it hits the breakpoint again.![folder structure](https://sp21.datastructur.es/materials/lab/lab3/img/resume_button.png)
6. Try clicking resume a few times, and you’ll see values start filling out the array. Note that every time you click resume, the code is running (as if you pressed step-over a bunch of times) until it gets back to the breakpoint again.
7. We can also switch away from the visualizer and back to being able to see the output of our print statements. To do this, click on `Debugger` again (next to `Java Visualizer`) and continue to click resume. On some machines, you may have to click on `Console` instead of `Debugger`. Each type you click resume, you’ll see another print statement corresponding to either a call to addLast or size.
8. Let’s now try out a conditional breakpoint. Right-click on your breakpoint and you’ll see a box pop up that says “Condition:”. In the box, type `L.size() == 12`.![folder structure](https://sp21.datastructur.es/materials/lab/lab3/img/conditional_breakpoint.png)
9. Click resume, and the code will run until the condition of the breakpoint is met, i.e. the size is 12. Try it out and click the visualizer, and you should see the size is now 12, with 12 items in the array. If you accidentally click too far, you must unfortunately restart the test.

> ==**总结 : resume就是再次来到断点(中间的执行依然会),  条件断点可以和resume配合使用**==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124141756729.png" alt="image-20230124141756729" style="zoom:33%;" />

这两个新特性（恢复和条件断点）对实验3的其余部分没有用处。然而，它们可能会在未来的项目中派上用场，您需要在实验室4中使用它们。此时您应该删除条件断点，这样它不会影响实验室的其他部分。

#### Adding More Randomized Calls

Modify the randomized test so that it now has the possibility of two additional operations: `getLast` and `removeLast`. Note, you’ll need to change the call to `StdRandom.uniform` so that it picks a number between 0 and 3.

Important: You should only call `getLast` and `removeLast` if `L.size` is greater than 0! These methods will crash if size is 0. In other words, have an `if` statement that skips the call to `getLast` or `removelast` if the size is zero.

Optional: Once you’ve added these methods, use a breakpoint to stop the code on the line that calls `removeLast`, and use the step-over feature of the debugger to convince yourself that `removeLast` looks like it is working correctly.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124140942480.png" alt="image-20230124140942480" style="zoom: 50%;" />

#### Adding Randomized Comparisons

The code we built above only makes calls to our known good implementation. Modify the code so that for every call made to the `AListNoResizing` method, it also makes a call to the same method in `TestBuggyAList`. Your code should also compare the return values of every method which has a return value.

If you’re stuck, see the code [at this link](https://sp21.datastructur.es/materials/lab/lab3/partialRandomizedComparisons.txt) for a *partial* solution, but try first without looking at the code!

#### Running our Randomized Test

尝试多次运行测试。它可能通过，也可能失败。现在尝试将N增加到5000。它几乎每次都会失败。

这就引出了随机测试的一个重要问题：如果你应用随机操作，并且错误相当模糊，那么你的随机操作序列可能无法检测到错误！有一些方法可以改进随机测试以避免这个问题，但这超出了我们课程的范围。

另一个注意事项：==**随机测试不应被用作精心设计的单元测试的替代品！我个人通常倾向于可能的非随机测试，并将随机测试视为补充测试方法**==。参见[此线程](https://news.ycombinator.com/item?id=24349522)就这个问题进行辩论。

#### Fixing the Bug and Execution Breakpoints

现在我们有一个测试失败了，我们想知道为什么。

你会注意到，每次测试失败时，我们得到的信息都是这样的：

```java
java.lang.ArrayIndexOutOfBoundsException: Index 7 out of bounds for length 7

at randomizedtest.BuggyAList.resize(BuggyAList.java:31)
```

一种方法是在`BuggyAList.java `的第31行设置一个条件断点，条件为`i==items.length`。这会很好，欢迎您尝试。

However, we’ll take this opportunity to instead show you how to set up an “==**Execution Breakpoint**==” so that we can stop the code and visualize what’s going on when your code crashes.然而，我们将借此机会向您展示如何设置“==**执行断点**==”，==以便在代码崩溃时停止代码并可视化发生的情况。==

To this, click “Run -> View Breakpoints”. You should see a window like this pop up:

<img src="https://sp21.datastructur.es/materials/lab/lab3/img/breakpoints.png" alt="folder structure" style="zoom:50%;" />

Click on the checkbox on the left that says “any exception” and then click on that says “Condition:” and in the window and enter exactly:

```java
this instanceof java.lang.ArrayIndexOutOfBoundsException
```

Once you’ve done this, your breakpoints window should look like:

<img src="https://sp21.datastructur.es/materials/lab/lab3/img/breakpoints_filled_in.png" alt="folder structure" style="zoom: 50%;" />

单击调试按钮，==代码应该在异常即将发生时立即停止!!!!!!(不设置的话, 就会崩溃了以后再停止)==。单击可视化工具并尝试找出代码崩溃的原因。现在真正的问题解决可以开始了！

在可视化工具窗口中有一堆关于问题到底是什么的线索。这是一个棘手而微妙的问题，但如果您的方法系统化，您应该能够识别问题。如果卡住了，请突出显示下一段中的隐藏文本：

Focus on the parameter passed to the resize function. Try to figure out what's wrong with it, and using that information try to work out by looking at the code of `removeLast` how that bad parameter ended up getting passed in.

Once you’ve identified the bug, fix it. Rerun the test to verify that `BuggyAList` works correctly now.

**==NOTE: If you use the debug feature without specifying a condition, your code will stop in some various mysterious places. Make sure you never have “Any Exception” checked without having a specified condition.==** This is because the process of starting JUnit tests generates a bunch of exceptions that ultimately get ignored. This is well beyond the scope of our class. If you’re done using an execution breakpoint, you should uncheck the “Java Exceptions Breakpoints” box in the top left.**注意：如果您在没有指定条件的情况下使用调试功能，那么代码将在某些神秘的地方停止。确保在没有指定条件的情况下从未检查过“AnyException”。**这是因为启动JUnit测试的过程会生成一堆最终被忽略的异常。这远远超出了我们班的范围。如果使用了执行断点，则应取消选中左上角的“Java异常断点”框。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230124144605519.png" alt="image-20230124144605519" style="zoom: 50%;" />

#### Cleaning Up

Lastly: Our JUnit tests have print statements in them to create a log of all the randomized calls. While this was useful for instruction purposes, it is not something you want in real world tests since it just generates a big mess of text that isn’t useful. Remove all print statements from your JUnit tests.最后：JUnit测试中有打印语句，用于创建所有随机调用的日志。虽然这对于指令目的很有用，**但在实际测试中这不是你想要的，因为它只会生成一大堆无用的文本。从JUnit测试中删除所有打印语句。**

Note: Often it is useful to “log” rather than print out the function calls made by randomized tests. For more on this, complete the project 1 extra credit assignment.注意：**==“log(记录)”而不是打印出随机测试所做的函数调用通常是有用的。要了解更多信息，请完成项目1额外的学分分配。==**

## Conclusion

In this lab, we’ve seen how to:

- Empirically measure the time it takes to construct a data structure.根据经验衡量构建数据结构所需的时间。
- Empirically measure the runtime of a data structure’s methods as a function of the size of the data structure.根据数据结构的大小，根据经验测量数据结构方法的运行时间。
- Perform a comparison test between two implementations of a class.在类的两个实现之间执行比较测试。
- Randomly call methods inside of a class.随机调用类内的方法。
- Perform random comparison tests between two implementations of a class.在类的两个实现之间执行随机比较测试。
- Use the resume button in IntelliJ.使用IntelliJ中的恢复按钮。
- Add a condition to a breakpoint.向断点添加条件。
- Create an execution breakpoint.创建执行断点。

## Submission

As usual, submit your code to the autograder. The autograder will examine the output of your timing tests and the correctness of your `BuggyAList` class.