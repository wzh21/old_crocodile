​												

​															Project 0: 2048

## Introduction

**本项目的目的是让您有机会熟悉Java以及本课程中使用的各种工具，如用于编写和运行单元测试的IntelliJIDE和JUnit。**尽管您会在“proj0”文件夹中找到许多文件和代码，但您的任务仅驻留在“`Model.java`”中，并且仅限于四个方法。

我们将只对您是否能够（根据我们的测试）使您的程序运行并提交指定的部分进行评分**没有隐藏测试**。在未来的作业中, 我们也会对你的风格进行评分, 但本项目并非如此. 我们仍然建议遵循[style61b指南](https://sp21.datastructur.es/materials/guides/style-guide.html)因为你会发现它有助于创建干净的代码, 但你不会因此而被评分。

这个任务的规范很长，而且有很多起始代码。我们建议您在开始编程之前阅读整个规范。一开始可能会让人感到难以承受。**您可能需要重新阅读规范的部分几次才能完全理解它，并且在您完成项目的早期部分之前，后面的部分可能不完全有意义 . 最终，我们希望你能在这段经历中获得一种力量感，因为你能够驾驭如此艰巨的任务。**

## The Game

你可能看过，也可能玩过游戏“2048”，这是一款由加布里埃尔·西罗利（Gabriele Cirulli）编写的单人电脑游戏，基于Veewo Studio早期的游戏“1024”（参见他的[在线版2048](http://gabrielecirulli.github.io/2048)).

In this project, you’ll be building the core logic of this game. That is, we’ve already put together all the GUI code, handle key-presses, and a ton of other scaffolding. Your job will be to do the most important and interesting part.在这个项目中，您将构建这个游戏的核心逻辑。也就是说，我们已经将所有的GUI代码、处理按键和大量其他脚手架组合在一起。你的工作将是做最重要和最有趣的部分。

具体来说，您将在“`Model.java`”文件中填写4个方法，该文件控制用户按下某些键后发生的情况。

游戏本身很简单。它是在一个4×4的正方形网格上播放的，每个正方形可以是空的，也可以包含一个带有整数的瓦片——2的幂(大于或等于2)。在第一次移动之前，*应用程序将包含2或4的平铺添加到初始空板上的随机正方形中。选择2或4是随机的，选择2的几率为75%，选择4的几率为25%。*

然后，玩家通过箭头键选择一个方向来“倾斜”棋盘：北、南、东或西。所有平铺都沿该方向滑动，直到在运动方向上没有剩余空间（可能没有可用空间）。一个瓦片可能会与另一个获得玩家积分的瓦片“合并”。

下面的GIF是一个例子，可以看看几次移动的结果是什么样子。

<img src="https://sp21.datastructur.es/materials/proj/proj0/img/example-2048.gif" alt="2048 Examples" style="zoom: 50%;" />

下面是上图中显示的合并发生时的完整规则。

1. 两个具有相同值*的平铺合并*为一个包含两倍初始数字的平铺。
2. **合并结果的平铺将不会在该倾斜上再次合并**。例如，如果我们有[X，2，2，4]，其中X表示一个空白空间，并且我们将平铺移动到左侧，我们应该以[4，4，X，X]结尾，而不是[8，X，X]结尾。这是因为最左边的4已经是合并的一部分，因此不应该再次合并。
3. 1.当运动方向上的三个相邻瓦片具有相同的数量时，则运动方向上**前导的两个瓦片合并，而尾部瓦片不合并。**例如，如果我们有[X，2，2，2]并向左移动平铺，我们应该以[4，2，X，X]而不是[2，4，X，X]结束。

> 注意 : 如果一行/列中有四个一样的2(例如) , 那最终会只剩下两个4

The game ends when the current player has no available moves (no tilt can change the board), or a move forms a square containing 2048. **当前玩家没有可用的移动（任何倾斜都不能改变棋盘），或移动形成包含2048的正方形时，游戏结束**

The “Max Score” is the maximum score the user has achieved in that game session. **It isn’t updated until the game is over**, so that is why it remains 0 throughout the animated GIF example.

## Assignment Philosophy and Program Design

A video overview of this section of the spec can be found at https://youtu.be/3YbIOga6ZdQ.

在这个项目中，我们将为您提供**非常多**的start code，它使用了许多我们尚未涉及的Java语法，甚至一些我们在课堂上永远不会涉及的语法。

**这里的想法是，在现实世界中，你经常使用你不完全理解的代码库，并且必须进行一些修补和实验才能获得你想要的结果。**别担心，当我们下周开始项目1时，你将有机会从头开始。

下面，我们将描述由Paul Hilfinger创建的给定框架代码架构背后的一些想法。了解每一个细节并不重要，但你可能会觉得有趣。

==The skeleton exhibits two *design patterns* in common use: the Model-View-Controller Pattern (MVC), and the Observer Pattern.该框架展示了两种常用的*设计模式*：**模型-视图-控制器模式（MVC）和观察者模式**。==

The ==**MVC pattern**== divides our problem into three parts:

- The **model** represents the subject(主题) matter being represented and acted upon – in this case incorporating the state of a board game and the rules by which it may be modified. Our model resides in the `Model`, `Side`, `Board`, and `Tile` classes. The instance variables of `Model` fully determine what the state of the game is. 

  > **Note: You’ll only be modifying the `Model` class.**

- A **view** of the model, which displays the game state to the user. Our view resides in the `GUI` and `BoardWidget` classes.

- A **controller** for the game, which translates user actions into operations on the model. Our controller resides mainly in the `Game` class, although it also uses the GUI class to read keystrokes.

The MVC pattern is not a topic of 61B, nor will you be expected to know or understand this design pattern on exams or future projects.	MVC模式不是61B的主题，也不会期望您在考试或未来项目中了解或理解这种设计模式。

The second pattern utilized is the “**==Observer pattern==**”. ==Basically this means that the **model** doesn’t actually report changes to the **view**==. Instead, the **view** *registers* itself as an *observer* of the `Model` object. This is a somewhat advanced topic so we will provide no additional information here.

We’ll now go over the different classes that you will interact with.(接下来的四个class)

### Tile 瓦片, 瓷砖

This class represents numbered tiles on the board. **If a variable of type `Tile` is `null`, it’s treated as an empty tile on the board.** *You will not need to create any of these objects, though you will need have an understanding of them since you will be using them in the `Model` class. ==**The only method of this class you’ll need to use is `.value()` which returns the value of the given tile. **==For example if `Tile t` corresponds to a tile with the value 8, then `t.value()` will return `8`.

### Side

==The `Side` class is a special type of class called an `Enum`==. An enum is similar has restricted functionality. Specifically, enums may take on only one of a finite set of values. In this case, we have a value for each of the 4 sides: `NORTH`, `SOUTH`, `EAST`, and `WEST`. You will not need to use any of the methods of this class nor manipulate the instance variables.

**Enums can be assigned with syntax like `Side s = Side.NORTH`. Note that ==rather than using the `new` keyword==, we simply set the `Side` value equal to one of the four values.** Similarly if we have a function like `public static void printSide(Side s)`, we can call this function as follows: `printSide(Side.NORTH)`, which will pass the value `NORTH` to the function.

如果您想进一步了解Java enums，请参见https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html.

### Model

**This class represents the entire state of the game.** A `Model` object represents a game of 2048. It has instance variables for the state of the board (i.e. where all the `Tile` objects are, what the score is, etc) as well as a variety of methods. One of the challenges when you get to the fourth final task of this project (writing the `tilt` method) will be to figure out which of these methods and instance variables are useful.当您完成本项目的第四个最终任务（编写“倾斜”方法）时，其中一个挑战是找出这些方法和实例变量中哪些有用。

### Board

**This class represents the board of tiles itself.** It has three methods that you’ll use: `setViewingPerspective`, `tile`, `move`. Optionally, for experimentation, you can use `getRandomNonNullTile`.

**您将仅编辑此作业中的“Model.java”文件。**Gradescope将仅使用您的“Model.java”文件并使用其他文件的骨架版本，因此，如果您对“Tile.java”进行了编辑，则Gradescope将无法识别该文件。

## Getting Started

==设置一遍以后, 之后再复习可以跳过这一部分(Getting Started)==

### IntelliJ Setup

Let’s now open the files in IntelliJ. Firstly, launch IntelliJ. It will show you a list of your recent projects, but since you haven’t started this assignment yet, it won’t be there. To open the project, click the “Open” button at the top right of the application window which should bring up your operating system’s file browser. Navigate to the `proj0` folder in your student repo, and then hit open:

<img src="https://sp21.datastructur.es/materials/proj/proj0/img/intellij-pre-open.png" alt="Opening the Project" style="zoom: 50%;" /><img src="https://sp21.datastructur.es/materials/proj/proj0/img/intellij-open.png" alt="Opening the Project" style="zoom:50%;" />

On the top left of the screen, you’ll see a list of the files/folders inside the `proj0` directory. If you don’t, click the down arrow on the `proj0` folder which will expand the folder below. It should look like this:

<img src="https://sp21.datastructur.es/materials/proj/proj0/img/list-files.png" alt="Listed folders" style="zoom:67%;" />

The `.idea` folder is something that IntelliJ generates to store miscellaneous(混杂的,各种各样的) settings. You can ignore this folder.

The `game2048` folder is the source for all the Java files. Everything you need to do lies in this folder.

The `javalib` folder is a Java library. It holds three `.jar` files which are pre-compiled files of things we want to use. The three files it has allow us to run JUnit and then some UC Berkeley specific things to render the game in a nice window.

IntelliJ is usually smart enough to set up the rest of the things for you, but in case your IntelliJ application is having a hard time we’ll walk through the setup procedures.

Firstly, let’s tell IntelliJ that we’re using Java 15. Navigate to File > Project Structure:

<img src="https://sp21.datastructur.es/materials/proj/proj0/img/opening-project-structure.png" alt="Opening Project Structure" style="zoom:50%;" />

Click on the box on the left-hand side of the “Edit” button which should allow you to select your JDK (i.e. your version of Java).

<img src="https://sp21.datastructur.es/materials/proj/proj0/img/java15.png" alt="Java 15" style="zoom:50%;" />

Now we’ll tell IntelliJ that we want to use those `.jar` files in the `javalib` folder. Still in the Project Structure, on the left-hand side click the secttion of the Project Settings called “Library”. If you see that `javalib` is already added, there is nothing to do. Else, we will click on the “+” button and then “Java” which will launch our operating system’s file browser, and we’ll click on the `javalib` folder. Then, in the bottom right of the screen, hit “Apply” and then the blue “OK” button.

In all, the setup would look like this (sorry for the blurryness):

<img src="https://sp21.datastructur.es/materials/proj/proj0/img/intellij-setup.gif" alt="IntelliJ Setup" style="zoom:50%;" />

To make sure the setup is all fine, open the `game2048` folder and right click on the `Main` Java file: you’ll see a few options, but the one we care about is the green “Run Main.main()” button. It should look like the following image:

<img src="https://sp21.datastructur.es/materials/proj/proj0/img/run-main.png" alt="Run Main" style="zoom:50%;" />

Click that to launch the 2048 game. This will launch a new window with a blank board. Just close the window for now and we’ll come back to it later when we get to the section of this spec called “Main Task: Building the Game Logic”.

If nothing pops up, it means your setup is incorrect. You should redo the above steps to make sure you didn’t miss anything, but don’t spend more than 10 minutes on this. It’s best to get setup problems fixed with a TAs help, meaning you should post on Ed or go to Office Hours. If you post on Ed, you need to tell us **everything** you’ve done/tried so we can get a clear picture of what the error is. Include screenshots of everything, especially any error messages you might get.

One weird quirk you might run into is that the code compiles and runs correctly, but you still get red underlines in IntelliJ. Go to the `Model` class, and find the `addTile` method. This is a method we provided, but you might see that the `tile` variable is underlined in red with the following error message:

![IntelliJ error](https://sp21.datastructur.es/materials/proj/proj0/img/intellij-error.png)

But we know that clearly it’s correct because 1). the code ran and 2). it’s the starter code! While IntelliJ is incredibly powerful, it does get things wrong sometimes like this. To fix this, you should go to File > Invalidate / Restart, then in the following window hit “Invalidate and Restart”

<img src="https://sp21.datastructur.es/materials/proj/proj0/img/invalidate-caches.png" alt="Invalidate Caches" style="zoom:50%;" />

This will take a minute or two as IntelliJ is re-indexing your JDK and setting up your project from scratch. After it finishes, you should see no red underlines in the source files.

You won’t be able to work on the project unless the above setup is fine, so make it your priority to get setup as soon as you can.

## Your assignment

此项目的任务是修改并完成“`Model`”类，特别是“`emptySpaceExists`”、“`maxTileExists`’、“`atLeastOneMoveExists`‘和“`tilt`”方法。其他一切都已为您实现。我们建议按此顺序完成。前两个相对简单。第三种方法（“`atLeastOneMoveExists`”）更难，最后一种方法“`tilt`”可能会非常困难。我们预计“`tilt`”将需要您3到10个小时才能完成。前三种方法将在条件下处理游戏，最后一种方法“`tilt`”将在用户按键后修改游戏板。您可以阅读“checkGameOver”方法的很短的正文，了解如何使用您的方法来检查游戏是否结束。

让我们先看看前三种方法：

### public static boolean emptySpaceExists(Board b)

This method should return true if any of the tiles in the given board are null. **You should NOT modify the Board.java file in any way for this project**. *For this method, you’ll want to use the `tile(int col, int row)` and `size()` methods of the `Board` class. No other methods are necessary.*

> Note: We’ve designed the `Board` class using a special keyword `private` that disallows you from using the instance variables of `Board` directly. For example, if you try to access `b.values[0][0]`, this will not work. **This is a good thing! It forces you to learn to use the `tile` method, which you’ll use throughout the rest of the project.**

尝试打开“`TestEmptySpace.java`”文件夹。运行测试。您应该看到其中6个测试失败，2个测试通过。正确编写“`emptySpaceExists`”方法后，“`TestEmptySpace`”中的所有8个测试都应该通过。

[本视频](https://youtu.be/13rdFndFNXc)提供了如何开始编写此方法的快速概述.

### public static boolean maxTileExists(Board b)

**This method should return true if any of the tiles in the board are equal to the winning tile value 2048.** Note that rather than hard coding the constant 2048 into your code, you should use MAX_PIECE, which is a constant that is part of the `Model` class. In other words, you shouldn’t do `if (x == 2048)` but rather `if (x == MAX_PIECE)`.

Leaving in hard coded numbers like `2048` is a bad programming practice sometimes referred to as a “magic number”. The danger of such magic numbers is that if you change them in one part of your code but not another, you might get unexpected results. **==By using a variable like `MAX_PIECE` you can ensure they all get changed together.==**

After you’ve written the method, the tests in `TestMaxTileExists.java` should pass.

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230121192621087.png" alt="image-20230121192621087" style="zoom:33%;" />

自加了一泛型方法

### public static boolean atLeastOneMoveExists(Board b)

This method is more challenging. **It should return true if there are any valid moves.** By a “valid move”, we mean that if there is a button (UP, DOWN, LEFT, or RIGHT) that a user can press while playing 2048 that causes at least one tile to move, then such a keypress is considered a valid move.这种方法更具挑战性。如果有任何有效的移动，它应该返回true。所谓“有效移动”，我们的意思是，如果用户在播放2048时可以按下一个按钮（向上、向下、向左或向右），导致至少一个平铺移动，那么这样的按键被视为有效移动。

**There are two ways that there can be valid moves:**

1. **There is at least one empty space on the board.**
2. **There are two adjacent tiles with the same value.**

For example, for the board below, we should return true because there is at least one empty space.

```
|   2|    |   2|    |
|   4|   4|   2|   2|
|    |   4|    |    |
|   2|   4|   4|   8|
```

For the board below, we should return false. No matter what button you press in 2048, nothing will happen, i.e. there are no two adjacent tiles with equal values.

```
|   2|   4|   2|   4|
|  16|   2|   4|   2|
|   2|   4|   2|   4|
|   4|   2|   4|   2|
```

For the board below, we would return true since a move to the right or left would merge the two 64 tiles, and also a move up or down would merge the 32 tiles. Or in other words, there exist at least two adjacent tiles with equal values.

```
|   2|   4|  64|  64|
|  16|   2|   4|   8|
|   2|   4|   2|  32|
|   4|   2|   4|  32|
```

After you’ve written the method, the tests in `TestAtLeastOneMoveExists.java` should pass.

> ==方法 : 只考虑特定的`tile`对象, 只考虑"自己"会不会被合并==

## Main Task: Building the Game Logic

The fourth and final part of the assignment is to implement `tilt`. You should only start this method once you’re passing all the tests in `TestEmptySpace`, `TestMaxTileExists` and `TestAtLeastOneMoveExists`.

==Computer science is essentially about one thing: Managing complexity.== Writing the `tilt` method is a rich experience that will give you a chance to try just that. I must warn you, this is probably going to be a frustrating experience. It is likely that you will attempt several approaches that will ultimately fail before you have to start back over	.**==计算机科学本质上是关于一件事：管理复杂性==**。写“倾斜”方法是一种丰富的经验，它会给你一个尝试的机会。**我必须警告你，这可能会是一次令人沮丧的经历。在你必须重新开始之前，你可能会尝试几种最终失败的方法。**

Before we start talking about how `tilt` should work, let’s try running the game.在我们开始讨论“倾斜”应该如何工作之前，让我们试着运行游戏。

Open the `Main` class and click the run button. You should see the game pop up. Try pressing the arrow keys. You should see that nothing is happening. This is because you have not implemented the `tilt` method yet. When you’re done writing `tilt`, you’ll be able to play the game.打开“Main”类并单击运行按钮。你应该看到游戏弹出。尝试按箭头键。你应该看到什么都没有发生。这是因为您尚未实现“倾斜”方法。当你写完“倾斜”后，你就可以玩游戏了。

### public boolean tilt(Side side)

> 本方法是用于真正移动(上下左右)

The tilt method does the work of actually moving all the tiles around. For example, if we have the board given by:

```
|   2|    |   2|    |
|   4|   4|   2|   2|
|    |   4|    |    |
|   2|   4|   4|   8|
```

And press up, `tilt` will modify the `board` instance variable so that the state of the game is now:

```
|   2|   8|   4|   2|
|   4|   4|   4|   8|
|   2|    |    |    |
|    |    |    |    |
```

In addition to modifying the board, **two other things must happen:**

1. The score instance variable must be updated to reflect the total value of all tile merges (if any). For the example above, we merged two 4s into an 8, and two 2s into a 4, so the score should be incremented by 8 + 4 = 12.
2. If anything about the board changes, we must set the `changed` **local variable** to `true`. That’s because at the end of the skeleton code for `tilt`, you can see we call a `setChanged()` method: this informs the GUI that there is something to draw. You will not make any calls to `setChanged` yourself: only modifying the `changed` local variable.       如果board发生任何变化，我们必须将“changed”**局部变量**设置为“true”。这是因为在“tilt”的框架代码末尾，您可以看到我们调用了一个“setChanged（）”方法：它通知GUI有一些要绘制的内容。您不会自己调用“setChanged”：只修改“changed”本地变量。

All movements of tiles on the board must be completed using the `move` method provided by the `Board` class. All tiles of the board must be accessed using the `tile` method provided by the `Board` class. **Due to some details in the GUI implementation, you should only call `move` on a given tile once per call to `tilt`**. We’ll discuss this constraint further in the Tips section of this document.

A quick overview of how to get started writing this method is provided in [this video](https://youtu.be/abFbbK1QY2k).

## Tips

We strongly recommend starting by thinking only about the up direction, i.e. when the provided `side` parameter is equal to `Side.NORTH`. To support you in this, we provide a `TestUpOnly` class that has four tests: `testUpNoMerge`, `testUpBasicMerge`, `testUpTripleMerge`, and `testUpTrickyMerge`. You’ll note that these tests involve only a single move up.

When considering how to implement the up direction, consider the following:

In a given column, the piece on the top row (row 3) stays put. The piece on row 2 can move up if the space above it is empty, or it can move up one if the space above it has the same value as itself. In other words, when iterating over rows, it is safe to iterate starting from row 3 down, since there’s no way a tile will have to move again after moving once.

While this sounds like it’s not going to be very hard, it really is. Be ready to bust out a notepad and work out a bunch of examples. Strive for elegant code, though elegance is hard to achieve with this problem. **We strongly recommend the creation of one or more helper methods to keep your code clean.** For example, you might have a helper function that processes a single column of the board, since each column is handled independently. Or you might have a helper function that can return a desired row value.

Reminder: You should only call `move` on a given tile once. In other words, suppose you have the board below and press up.

```
|    |    |    |    |
|    |    |    |    |
|    |    |    |    |
|    |    |    |   2|
```

One way we could accomplish this would be as follows:

```
Tile t = board.tile(3, 0)
board.move(3, 1, t);
board.move(3, 2, t);
board.move(3, 3, t);
setChanged();
return true;
```

However, the GUI will get confused because the same tile is not supposed to move multiple times with only one call to setChanged. Instead, you’ll need to complete the entire move with one call to `move`, e.g.

```
Tile t = board.tile(3, 0)
board.move(3, 3, t);
```

In a sense, the hard part is figuring out which row each tile should end up on.

To test your understanding, you should complete this [Google Form quiz](https://forms.gle/pubhRx4fxYnPTGNX8). This quiz (and the following quizzes) are completely optional (i.e. no graded) but **highly suggested** as it’ll find any conceptual misunderstandings you might have about the game mechanics. You may attempt this quiz as many times as you’d like.

To know when you should update the score, note that the `board.move(c, r, t)` method returns `true` if moving the tile `t` to column `c` and row `r` would replace an existing tile (i.e. you have a merge operation).

To make matters seemingly much worse, even after you get tilt working for the up direction, you’ll have to do the same thing for the other three directions. If you do so naively, you’ll get a *lot* of repeated, slightly modified code, with ample opportunity to introduce obscure errors.

For this problem, we’ve given away a clean solution. This will allow you to handle the other three directions with only two additional lines of code! Specifically, the `Board` class has a `setViewingPerspective(Side s)` function that will change the behavior of the `tile` and `move` classes so that they *behave as if the given side was NORTH*.

For example, consider the board below:

```
|    |    |    |    |
|  16|    |  16|    |
|    |    |    |    |
|    |    |    |   2|
```

If we call `board.tile(0, 2)`, we’ll get `16`, since 16 is in column 0, row 2. If we call `board.setViewingPerspective(s)` where `s` is `WEST`, then the board will behave as if WEST was NORTH, i.e. you had your head turned 90 degrees to the left, as shown below:

```
|    |    |  16|    |
|    |    |    |    |
|    |    |  16|    |
|   2|    |    |    |
```

In other words, the `16` we had before would be at `board.tile(2, 3)`. If we were to call `board.tilt(Side.NORTH)` with a properly implemented `tilt`, the board would become:

```
|   2|    |  32|    |
|    |    |    |    |
|    |    |    |    |
|    |    |    |    |
```

To get the board to go back to the original viewing perspective, we simply call `board.setViewingPerspective(Side.NORTH)`, which will make the board behave as if `NORTH` was `NORTH`. If we do this, the board will now behave as if it were:

```
|    |    |    |    |
|  32|    |    |    |
|    |    |    |    |
|   2|    |    |    |
```

Observe that this is the same thing as if you’d slid the tiles of the original board to the `WEST`.

Important: Make sure to use `board.setViewingPerpsective` to set the perspective back to `Side.NORTH` before you finish your call to `tilt`, otherwise weird stuff will happen.

To test your understanding, try this third and final [Google Form quiz](https://forms.gle/AGrhEFbwfMJ7qwaB6). You may attempt this quiz as many times as you’d like.

## Testing

> 本部分可以忽略

While in the future we expect you to be able to test your own programs, for this project we’ve given you the full test suite.

The tests are split over 5 files: `TestEmptySpace`, `TestMaxTileExists`, `TestAtLeastOneMoveExists`, `TestUpOnly`, and `TestModel`. Each file tests a specific portion of the code with the exception of `TestModel` which tests all the things you write in coordination with each other. Such a test is called an *integration test* and are incredibly important in testing. While unit tests run things in isolation, integration tests run things all together and are designed to catch obscure bugs that occur as a result of the interaction between different functions you’ve written.

So do not attempt to debug `TestModel` until you’re passing the rest of the tests! In fact, the order in which we discuss the tests is the order you should attempt them in.

We’ll now take a look at each of these tests and show you how to read the error messages.

### TestEmptySpace

These tests will check the correctness of your `emptySpaceExists` method. Here is what the error message would look like if you failed one of the tests:

![TestEmptySpace all fail](https://sp21.datastructur.es/materials/proj/proj0/img/test-empty-space-all-fail.png)

On the left-hand side, you’ll see the list of all tests that were run. The yellow X means we failed a test while the green check means we passed it. On the right, you’ll see some useful error messages. To look at a single test and its error message in isolation, click the test on the left-hand side. For example, let’s say we want to look at the `testCompletelyEmpty` test.

![testCompletelyEmpty](https://sp21.datastructur.es/materials/proj/proj0/img/test-completely-empty.png)

The right-hand side is now the isolated error message for this test. The top line has a useful message: `"Board is full of empty space"` followed by a String representation of the board. You’ll see that it’s clearly empty, yet our `emptySpaceExists` method is returning `false` and causing this test to fail. The javadoc comment at the top of the code for the test also has some useful information in case you’re failing a test.

### TestMaxTileExists

These tests will check the correctness of your `maxTileExists` method. The error messages will be similar to those for `TestEmptySpace`, and you can still click on each individual test to look at them in isolation. Remember that your `maxTileExists` method should **only** look for the max tile and not anything else (i.e. shouldn’t look for empty space). If yours does, you will not pass all of these tests.

### TestAtLeastOneMoveExists

These tests will check the correctness of your `atLeastOneMoveExists` method. The error messages are similar to the above two. Since the `atLeastOneMoveExists` method depends on the `emptySpaceExists` method, you shouldn’t expect to pass these tests until you are passing all of the tests in `TestEmptySpace`.

### TestUpOnly

These tests will check the correctness of your `tilt` method, but only in the up (`Side.NORTH`) direction. The error messages for these are different, so let’s look at one. Say we run all the tests, notice we’re failing the `testUpTrickyMerge` test. After clicking that test, we’ll see this:

![testUpTrickyMerge Error Message](https://sp21.datastructur.es/materials/proj/proj0/img/test-up-error-msg.png)

The first line tells us the direction that was tilted (for these tests it’ll always be North), then what your board looked like before the tilt, then what we expected the board to look like, and finally what your board actually looked like.

You’ll see that we’re merging a tile twice on a single call to tilt which results in a single tile with value 8 instead of two tiles both with value 4. As a result, our `score` is also incorrect as you can see in the bottom of the representation of the board.

For other tests it might be difficult to notice the difference between the expected and actual boards right away; for those, you can click the blue “Click to see difference” text at the very bottom of the error message to get a side-by-side comparison of the expected (on the left) and actual (on the right) boards in a separate window. Here is what it looks like for this test:

![testUpTrickyMerge Comparison](https://sp21.datastructur.es/materials/proj/proj0/img/comparison.png)

Debugging these can be a bit tricky because it’s hard to tell what you’re doing wrong. First, you should identify which of the 3 rules listed above you’re violating. In this case, we can see that it’s rule 2 since a tile is merging more than once. The javadoc comments on these methods are good resources for this as they specifically lay out what rule/configuration they’re testing. You might also be able to figure out what rule you’re violating by just looking at the before and after boards. Then, comes the tricky party: refactoring your existing code to properly account for that rule. We suggest writing out on pen and paper the steps your code takes so you can first understand why your board looks the way it does, then coming up with a fix. These tests only call `tilt` once, so you don’t need to worry about debugging multiple calls to tilt.

### TestModel

These tests will check the correctness of everything together. The majority of these tests are similar to the tests in `TestUpOnly` as they only call `tilt` once, but we also have tests for `gameOver` (which test all of your `emptySpaceExists`, `maxTileExists`, and `atLeastOneMoveExists` methods together) and tests that make many calls to `tilt` in a sequence.

The error messages for these look exactly the same as those in `TestUpOnly`, and the javadoc comments are similarly useful in figuring out what the test is testing.

Don’t worry about the actual code for the tests: you’re not required to understand or modify any of these, though you’re welcome to read through and get an idea for how test writing works and even add some of your own if you are feeling really ambitious.

## Grading

Here is a breakdown of what percent you’d earn on this project with varying levels of completing:

1. Only implementing `emptySpaceExists` or `maxTileExists`: ~27%
2. Implementing everything except `tilt`: ~47%
3. Implementing everything, except `tilt` only works in the Up direction: ~68%
4. Implementing everything, except merging: ~64%
5. Implementing everything, except rule 2 of merging: ~93%

## Submission and Version Control

It is important that you commit work to your repository *at frequent intervals*. Version control is a powerful tool for saving yourself when you mess something up or your dog eats your project, but you must use it regularly if it is to be of any use. Feel free to commit every 15 minutes; Git only saves what has changed, even though it acts as if it takes a snapshot of your entire project.

The command `git status` will tell you what files you have modified, removed, or possibly added since the last commit. It will also tell you how much you have not yet sent to your GitHub repository.

The typical commands would look something like this:

```
git status                          # To see what needs to be added or committed.
git add <filepath>                  # To add, or stage, any modified files.
git commit -m "Commit message"      # To commit changes.
git push
```

Then you can carry on working on the project until you’re ready to commit and push again, in which case you’ll repeat the above. It is in your best interest to get into the habit of comitting frequently with informative commit messages so that in the case that you need to revert back to an old version of code, it is not only possible but easy. We suggest you commit every time you add a significant portion of code or reach some milestone (passing a new test, for example).

Once you’ve pushed your code to GitHub (i.e. ran `git push`), then you may go to Gradescope, find the `proj0` assignment, and submit the code there. Keep in mind that the version of code that Gradescope uses is the most recent commit you’ve pushed, so if you do not run `git push` before you submit on Gradescope, old code will be tested instead of the most recent code you have on your computer.

