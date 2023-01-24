​							

​																							**Project 3 : Ants**

# 项目总览

**==本项目尽全力自己写,不看答案==**

The game ends either when a bee reaches the end of the tunnel (you lose), the bees destroy the `QueenAnt` if it exists (you lose), or the entire bee fleet has been vanquished (you win).当一只蜜蜂到达隧道尽头（你输了），蜜蜂摧毁了“QueenAnt”（如果它存在的话）（你输），或者整个蜜蜂舰队被征服（你赢了），游戏结束。

## Core concepts

**The Colony 群落**. **This is where the game takes place**. **The colony consists of several `Place`s** that are chained together to form a tunnel(地下隧道) where bees can travel through. The colony also has some quantity of food which can be expended in order to place an ant in a tunnel.蚁群也有一定数量的食物，可以用来把蚂蚁放在隧道里。

**Places**. A place links to another place to form a tunnel. The player can put a single ant into each place. However, there can be many bees in a single place.**位置**。一个地方与另一个地方相连形成一条隧道。玩家可以在每个地方放一只蚂蚁。然而，一个地方可能有很多蜜蜂。

**The Hive**. This is the place where bees originate. Bees exit the beehive to enter the ant colony.**蜂巢**。这是蜜蜂起源的地方。蜜蜂离开蜂巢进入蚁群。

**Ants**. Players place an ant into the colony by selecting from the available ant types at the top of the screen. Each type of ant takes a different action and requires a different amount of colony food to place. The two most basic ant types are the `HarvesterAnt`, which adds one food to the colony during each turn, and the `ThrowerAnt`, which throws a leaf at a bee each turn. You will be implementing many more!**蚂蚁**。玩家通过从屏幕顶部的可用蚂蚁类型中进行选择，将一只蚂蚁放入蚁群。每种类型的蚂蚁采取不同的行动，需要不同数量的蚁群食物。两种最基本的蚂蚁类型是**“收获蚁”**，它在每一个回合向蜂群中添加一种食物，以及**“抛叶蚁”**，每一回合向蜜蜂扔一片树叶。您将完成更多！

**Bees**. In this game, bees are the antagonistic forces that the player must defend the ant colony from. Each turn, a bee either advances to the next place in the tunnel if no ant is in its way, or it stings the ant in its way. Bees win when at least one bee reaches the end of a tunnel.**蜜蜂**。在这个游戏中，蜜蜂是对抗力量，玩家必须保护蚁群免受其害。每转一圈，蜜蜂要么前进到隧道中的下一个地方，如果没有蚂蚁挡着它的路，要么就螫死蚂蚁。当至少一只蜜蜂到达隧道末端时，蜜蜂获胜。

## Core classes

The concepts described above each have a corresponding class that encapsulates the logic for that concept. Here is a summary of the main classes involved in this game:上面描述的每个概念都有一个相应的类，该类封装了该概念的逻辑。以下是本游戏涉及的主要课程摘要：

- **`GameState`**: Represents the colony and some state information about the game, including how much food is available, how much time has elapsed, where the `AntHomeBase` is, and all the `Place`s in the game.**`GameState`**：**表示蚁群和有关游戏的一些状态信息，包括有多少食物、经过了多少时间、“AntHomeBase”在哪里以及游戏中的所有“Place”。**

- **`Place`**: Represents a single place that holds insects. At most one `Ant` can be in a single place, but there can be many `Bee`s in a single place. `Place` objects have an `exit` to the left and an `entrance` to the right, which are also places. Bees travel through a tunnel by moving to a `Place`'s `exit`.**`地点`**：**表示存放昆虫的单个地点。一个地方最多只能有一只“蚂蚁”，但一个地方可以有许多“蜜蜂”**地点对象在左边有一个“出口”，在右边有一个‘入口’，**这也是地点**。蜜蜂通过移动到“场所”的“出口”穿过隧道。

- **`Hive`**蜂巢: Represents the place where `Bee`s start out (on the right of the tunnel).

- **`AntHomeBase`**: Represents the place `Ant`s are defending (on the left of the tunnel). If `Bee`s get here, they win :(

- **`Insect`**: A superclass for `Ant` and `Bee`. All insects have `health` attribute, representing their remaining health, and a `place` attribute, representing the `Place` where they are currently located. Each turn, every active `Insect` in the game performs its `action`.**昆虫**：“蚂蚁”和“蜜蜂”的超类。所有昆虫都具有**“健康”属性**，表示它们的剩余健康，**以及“地点”属性**，代表它们当前所在的“地点”。每个回合，游戏中每个活跃的“昆虫”都会**执行其“动作”**。

- **`Ant`**: Represents ants. Each `Ant` subclass has special attributes or a special `action` that distinguish it from other `Ant` types. For example, a `HarvesterAnt` gets food for the colony and a `ThrowerAnt` attacks `Bee`s. Each ant type also has a `food_cost` attribute that indicates how much it costs to deploy one unit of that type of ant.

  **`Ant`**：表示蚂蚁。每个“Ant”子类都有特殊的属性或特殊的“action”，以区别于其他“Ant”类型。例如，**“HarvesterAnt”为蜂群获取食物，“ThrowerAnt”攻击“蜜蜂”。**每种蚂蚁类型还具有一个“food_cost”属性，该属性指示部署一个此类蚂蚁单元的成本。

- **`Bee`**: Represents bees. Each turn, a bee either moves to the `exit` of its current `Place` if the `Place` is not `blocked` by an ant, or stings the ant occupying its same `Place`.**`蜜蜂`**：表示蜜蜂。每转一圈，蜜蜂要么移动到当前“地点”的“出口”（如果该“地点”未被蚂蚁“阻挡”），要么螫伤占据同一“地点”位置的蚂蚁。

## Game Layout

Below is a visualization of a GameState. As you work through the unlocking tests and problems, we recommend drawing out similar diagrams to help your understanding.

![img](https://cs61a.org/proj/ants/assets/colony-drawing.png)

## Object map

To help visualize how all the classes fit together, we've also created an object map for you to reference as you work, which you can find [here](https://cs61a.org/proj/ants/diagram/ants_diagram.pdf):

# Phase 1: Basic gameplay

## Problem 0 (0 pt) 一些理解

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105221500727.png" alt="image-20230105221500727" style="zoom:33%;" />

> 选3

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105221516045.png" alt="image-20230105221516045" style="zoom:33%;" />

health 是实例属性, damage是类属性(**因为同一子类的ant有一样的damage**)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105221938708.png" alt="image-20230105221938708" style="zoom:33%;" />



## Problem 1 (1 pt)

**Part A**: Currently, there is no cost for placing any type of `Ant`, and so there is no challenge to the game. The base class `Ant` has a `food_cost` of zero. Override this class attribute for `HarvesterAnt` and `ThrowerAnt` according to the "Food Cost" column in the table below. 

**A**部分：目前，放置任何类型的“蚂蚁”都不需要支付任何费用，因此游戏没有任何挑战。基类“Ant”的“food_cost”为零。根据下表中的“食物成本”列，重写`HarvesterAnt`和`ThrowerAnt`的这个类属性。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105222144319.png" alt="image-20230105222144319" style="zoom: 33%;" />

**Part B**: Now that placing an `Ant` costs food, we need to be able to gather more food! To fix this issue, implement the `HarvesterAnt` class. A `HarvesterAnt` is a type of `Ant` that adds one food to the `gamestate.food` total as its `action`.
		**B部分**：既然放置“蚂蚁”需要食物，我们需要能够收集更多的食物！要解决此问题，请实现“HarvesterAnt”类。“HarvesterAnt”是“蚂蚁”的一种类型，它在“gamestate.food”总数中添加一种食物作为其“动作”。

### 理解

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105222807339.png" alt="image-20230105222807339" style="zoom: 33%;" />

不是每回合吃food,而是放置ant要消耗food(类似于PVZ里面的阳光)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105222933732.png" alt="image-20230105222933732" style="zoom:50%;" />

food_cost 是类属性(不是实例属性),因为所有相同的子类都有同样的消耗(==**不会随着时间的变化而变化**==)

### 完成

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105225000059.png" alt="image-20230105225000059" style="zoom: 33%;" />

## Problem 2 (1 pt)

In this problem, you'll complete `Place.__init__` by adding code that tracks entrances. Right now, a `Place` keeps track only of its `exit`. We would like a `Place` to keep track of its entrance as well. A `Place` needs to track only one `entrance`. Tracking entrances will be useful when an `Ant` needs to see what `Bee`s are in front of it in the tunnel.在这个问题中，您将完成`Place.__init__`通过添加跟踪入口的代码。现在，“地点”只跟踪其“出口”。我们也希望有一个“地方”来跟踪它的入口。“地点”只需要跟踪一个“入口”。当“蚂蚁”需要看到“蜜蜂”在隧道中的前方时，跟踪入口将非常有用。

However, simply passing an entrance to a `Place` constructor will be problematic; we would need to have both the exit and the entrance before creating a `Place`! (It's a [chicken or the egg](https://en.wikipedia.org/wiki/Chicken_or_the_egg) problem.) To get around this problem, we will keep track of entrances in the following way instead. `Place.__init__` should use this logic:然而，简单地通过“Place”构造函数的入口是有问题的；在创建“场所”之前，我们需要同时拥有出口和入口！（这是一只[鸡还是蛋](https://en.wikipedia.org/wiki/Chicken_or_the_egg)问题。）为了解决这个问题，我们将按照以下方式跟踪入口`地点__init__`应该使用以下逻辑：

- A newly created `Place` always starts with its `entrance` as `None`.新创建的“Place”始终以其“entry”开头为“None”。
- If the `Place` has an `exit`, then the `exit`'s `entrance` is set to that `Place`.如果“地点”有“出口”，则“出口”的“入口”设置为“地点”。

> *Hint:* Remember that when the `__init__` method is called, the first parameter, `self`, is bound to the newly created object*提示：*请记住，当调用`__init__`方法时，第一个参数`self`将绑定到新创建的对象

> *Hint:* Try drawing out two `Place`s next to each other if things get confusing. In the GUI, a place's `entrance` is to its right while the `exit` is to its left.*提示：*如果事情变得混乱，请尝试画出两个相邻的“Place”。在GUI中，一个地方的“入口”位于其右侧，而“出口”位于其左侧。

> *Hint:* Remember that `Place`s are not stored in a list, so you can't index into anything to access them. This means that you **can't** do something like `colony[index + 1]` to access an adjacent `Place`. How *can* you move from one place to another?*提示：*请记住，“Place”没有存储在列表中，因此您无法索引任何内容来访问它们。这意味着你**不能**像“colony[index+1]”那样访问相邻的“Place”。你怎么能从一个地方搬到另一个地方？

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105225656707.png" alt="image-20230105225656707" style="zoom: 50%;" />

让后面的找到前面的作为`entrance`

## Problem 3 (2 pt)

In order for a `ThrowerAnt` to throw a leaf, it must know which bee to hit. The provided implementation of the `nearest_bee` method in the `ThrowerAnt` class only allows them to hit bees in the same `Place`. Your job is to fix it so that a `ThrowerAnt` will `throw_at` the nearest bee in front of it **that is not still in the `Hive`.** This includes bees that are in the same `Place` as a `ThrowerAnt`

为了让“ThrowerAnt”扔树叶，它必须知道该打哪只蜜蜂。“ThrowerAnt”类中提供的“nearest_bee”方法的实现只允许它们在同一个“Place”中击打蜜蜂。你的工作是修复它，使“ThrowerAnt”将“throw_at”它前面最近的蜜蜂,**但它不在“蜂巢”中.** **这包括与“ThrowerAnt”处于同一“位置”的蜜蜂**

> *Hint:* All `Place`s have an `is_hive` attribute which is `True` when that place is the `Hive`.
>
> *提示：*所有“Place”都有一个“is_hive”属性，当该位置为“hive”时，该属性为“True”。

Change `nearest_bee` so that it returns a random `Bee` from the nearest place that contains bees. Your implementation should follow this logic:	更改“nearest_bee”，使其从包含蜜蜂的最近位置随机返回一个“蜜蜂”。您的实现应该遵循以下逻辑：

- Start from the current `Place` of the `ThrowerAnt`.从“ThrowerAnt”的当前“Place”开始。

- For each place, return a random bee if there is any, and if not, inspect the place in front of it (stored as the current place's `entrance`).对于每个地方，如果有，返回一只随机蜜蜂，如果没有，检查前面的地方（存储为当前地方的“入口”）。

- If there is no bee to attack, return `None`.如果没有蜜蜂攻击，返回“无”。

  (即**攻击自己的格子或者==所有格子中的最近的那个==**)

> *Hint*: The `random_bee` function provided in `ants.py` returns a random bee from a list of bees or `None` if the list is empty.
>
> *提示*：“ants.py”中提供的“random_bee”函数返回蜜蜂列表中的随机蜜蜂，如果列表为空，则返回“None”。

> *Hint*: As a reminder, if there are no bees present at a `Place`, then the `bees` attribute of that `Place` instance will be an empty list.
>
> *提示*：作为提醒，如果“Place”中没有蜜蜂，那么该“Place”实例的“bees”属性将是一个空列表。

> *Hint*: Having trouble visualizing the test cases? Try drawing them out on paper! The sample diagram provided in [Game Layout](https://cs61a.org/proj/ants/#game-layout) shows the first test case for this problem.
>
> *提示*：难以可视化测试用例？试着在纸上画出来！ [Game Layout](https://cs61a.org/proj/ants/#game-layout) 显示了该问题的第一个测试用例。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230105233617379.png" alt="image-20230105233617379" style="zoom: 50%;" />

==**可以在方法函数内定义一个递归的子函数!!!!!!!!!!!!**==

**==此时不一定是用于高阶函数,单纯就是想要用递归!!!!!!!!!!!!!!!!!!!!!!!!!!!!==**

# Phase 2: More Ants!

## Problem 4 (2 pt)

A `ThrowerAnt` is a powerful threat to the bees, but it has a high food cost. **In this problem, you'll implement two subclasses of `ThrowerAnt` that are less costly but have constraints on the distance they can throw:** **在这个问题中，您将实现“ThrowerAnt”的两个子类，它们成本较低，但对投掷距离有限制**

- The `LongThrower` can only `throw_at` a `Bee` that is found **after following at least 5 `entrance` transitions.** It cannot hit `Bee`s that are in the same `Place` as it or the first 4 `Place`s in front of it. If there are two `Bee`s, one too close to the `LongThrower` and the other within its range, the `LongThrower` should only throw at the farther `Bee`, which is within its range, instead of trying to hit the closer `Bee`. 不能投近的
- The `ShortThrower` can only `throw_at` a `Bee` that is found after following at most 3 `entrance` transitions. It cannot throw at any bees further than 3 `Place`s in front of it.不能投远的

Neither of these specialized throwers can `throw_at` a `Bee` that is exactly 4 `Place`s away.**这两名专业投掷运动员都不能“扔”出正好4个“地点”外的“蜜蜂”。**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230106110248423.png" alt="image-20230106110248423" style="zoom:25%;" />

To implement these new throwing ants, your `ShortThrower` and `LongThrower` classes should inherit the `nearest_bee` method from the base `ThrowerAnt` class. The logic of choosing which bee a thrower ant will attack is the same, **except the `ShortThrower` and `LongThrower` ants where their range is limited by a lower and upper bound, respectively.**

To do this, modify the `nearest_bee` method to reference `lower_bound` and `upper_bound` attributes, and only return a bee if it is within range.**要做到这一点，请修改“nearest_bee”方法以引用“lower_bound”和“upper_bind”属性，并仅在蜜蜂在范围内时返回蜜蜂。**

Make sure to give these `lower_bound` and `upper_bound` attributes appropriate values in the `ThrowerAnt` class so that the behavior of `ThrowerAnt` is unchanged. Then, implement the subclasses `LongThrower` and `ShortThrower` with appropriately constrained ranges.

You should **not** need to repeat any code between `ThrowerAnt`, `ShortThrower`, and `LongThrower`.

> *Hint:* `float('inf')` returns an infinite positive value represented as a float that can be compared with other numbers.
>
> *提示：*`float（'inf'）`返回一个无限正值，表示为一个可以与其他数字进行比较的浮点数。

> *Hint:* You can chain inequalities in Python: e.g. `2 < x < 6` will check if `x` is between 2 and 6. Also, `lower_bound` and `upper_bound` should mark an inclusive range.
>
> *提示：*==您可以在Python中链接不等式：例如，“2<x<6”将检查“x”是否介于2和6之间。此外，`lower_bound`和`upper_bound`应标记包含范围==

> **Important:** Make sure your class attributes are called `upper_bound` and `lower_bound` The tests directly reference these attribute names, and will error if you use another name for these attributes.
>
> **重要提示：**确保类属性被称为“upper_bound”和“lower_bound”。测试直接引用这些属性名称，如果为这些属性使用其他名称，则会出错。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230106120321532.png" alt="image-20230106120321532" style="zoom:50%;" />

## Problem 5 (3 pt)

Implement the `FireAnt`, which does damage when it receives damage. Specifically, if it is damaged by `amount` health units, it does a damage of `amount` to all bees in its place (this is called *reflected damage*). If it dies, it does an additional amount of damage, as specified by its `damage` attribute, which has a default value of `3` as defined in the `FireAnt` class.实现“FireAnt”，当它受到伤害时会造成伤害。具体来说，如果它受到“数量”生命单位的伤害，它会对其所在位置的所有蜜蜂造成“数量”的伤害（这称为“反射伤害”）。如果它死了，它会造成额外的伤害，这由它的“damage”属性指定，默认值为“3”，如“FireAnt”类中定义的。

To implement this, **override `Insect`'s `reduce_health` method**. Your overriden method should call the `reduce_health` method inherited from the superclass (`Ant`) *which inherits from it's superclass Insect* to reduce the current `FireAnt` instance's health. Calling the *inherited* `reduce_health` method on a `FireAnt` instance reduces the insect's `health` by the given `amount` and removes the insect from its place if its `health` reaches zero or lower.

> *Hint:* Do *not* call `self.reduce_health`, or you'll end up stuck in a recursive loop. (Can you see why?)
>
> *提示：*不要*调用'self.reduce_health'，否则您将陷入递归循环。（你能明白为什么吗？）

However, your method needs to also include the reflective damage logic:

- Determine the reflective damage amount: start with the `amount` inflicted on the ant, and then add `damage` if the ant's health has dropped to or below 0.
- For each bee in the place, damage them with the total amount by calling the appropriate `reduce_health` method for each bee.

> **Important:** Remember that when any `Ant` loses all its health, it is removed from its `place`, so pay careful attention to the order of your logic in `reduce_health`.
>
> **重要提示：**请记住，**当任何“蚂蚁”失去所有健康时，它将从其“位置”移除**，因此请注意“reduce_health”中逻辑的顺序。

> *Hint:* Damaging a bee may cause it to be removed from its place.**==If you iterate over a list, but change the contents of that list at the same time, you [may not visit all the elements](https://docs.python.org/3/tutorial/controlflow.html#for-statements).==** This can be prevented by making a copy of the list. You can either use a list slice, or use the built-in `list` function to make sure we do not affect the original list.
>
> ```py
>  >>> lst = [1,2,3,4]
>  >>> lst[:]
> [1, 2, 3, 4]
>  >>> list(lst)
> [1, 2, 3, 4]
>  >>> lst[:] is not lst and list(lst) is not lst
> True
> ```
>
> **==常用技巧  :  切片是浅拷贝,只是复制一下==**

Once you've finished implementing the `FireAnt`, give it a class attribute `implemented` with the value `True`.

> *Note:* Even though you are overriding the superclass's `reduce_health` function (`Ant.reduce_health`), you can still use this method in your implementation by calling it. Note this is not recursion. (Why not?)
>
> *注意：*即使您正在重写超类的`reduce_health`函数（`Ant.reduce_health`），您仍然可以通过调用它在实现中使用此方法。注意，这不是递归。（为什么不呢？）

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230106142415908.png" alt="image-20230106142415908" style="zoom:50%;" />

==**要习惯于在方法内定义函数,而目的并不是返回高阶函数,而是单纯的要去多次使用或者递归使用**==

## Problem 6 (1 pt)

We are going to add some protection to our glorious home base by implementing the `WallAnt`, an ant that does nothing each turn. A `WallAnt` is useful because it has a large `health` value.我们将通过实施“WallAnt”来为我们的光荣家园增添一些保护，这是一种每次都不做任何事情的蚂蚁。“WallAnt”很有用，因为它具有很大的“健康”值。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230106142641528.png" alt="image-20230106142641528" style="zoom:25%;" />

Unlike with previous ants, we have not provided you with a class header. Implement the `WallAnt` class from scratch. Give it a class attribute `name` with the value `'Wall'` (so that the graphics work) and a class attribute `implemented` with the value `True` (so that you can use it in a game).与以前的蚂蚁不同，我们没有为您提供类头。从头开始实现“WallAnt”类。给它一个类属性“name”，值为“all”（这样图形就可以工作），一个类特性“implemented”，值“True”（这样你就可以在游戏中使用它）。

> *Hint*: To start, take a look at how the previous problems' ants were implemented!
>
> *提示*：首先，看看前面的问题蚂蚁是如何实现的！

> *Hint*: Make sure you implement the `__init__` method too so the `WallAnt` starts off with the appropriate amount of `health`!
>
> *提示*：确保您也实现了`__init__`方法，以便`WallAnt `以适当的`health`量开始！

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230106143307580.png" alt="image-20230106143307580" style="zoom:33%;" />

## Problem 7 (3 pt)

> *Hint:* When a `Bee` is eaten, it should lose all its health. Is there an existing function we can call on a `Bee` that can reduce its health to 0?
>
> 提示：当“蜜蜂”被吃掉时，它会失去所有的健康。**我们是否可以调用一个“蜜蜂”的现有函数，使其生命值降至0？**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230106143721777.png" alt="image-20230106143721777" style="zoom:25%;" />

**给“HungryAnt”一个“chewing_turns”==类==属性，该属性存储“Hungry Ant”需要咀嚼的圈数（设置为3）。此外，给每个“饥饿蚂蚁”一个==实例==属性“turns_to_chew”，该属性计算它还剩下的咀嚼次数**（初始化为0，因为它一开始没有吃过任何东西。您也可以将“turns_to_chew”视为“饥饿蚁”可以吃掉另一只“蜜蜂”之前的旋转次数）。

Implement the `action` method of the `HungryAnt`: First, check if it is chewing; if so, decrement its `turns_to_chew`. Otherwise, eat a random `Bee` in its `place` by reducing the `Bee`'s health to 0. Make sure to set the `turns_to_chew` when a `Bee` is eaten!执行“饥饿蚂蚁”的“动作”方法：**首先，检查它是否在咀嚼；如果是，则将其“turns-tochew”递减。否则，通过将“蜜蜂”的生命值降低到0，在其“位置”随机吃一只“蜜蜂”。当“蜜蜂”被吃掉时，确保设置“turns_to_chew”！**

> *Hint*: Other than the `action` method, make sure you implement the `__init__` method too in order to define any instance variables and make sure that `HungryAnt` starts off with the appropriate amount of `health`!
>
> *提示*：除了“action”方法之外，请确保还实现了“__init__”方法，以便定义任何实例变量，并确保“HungryAnt”以适当的“health”量开始！

**==千万不要忘了加self,否则只是一个普通的变量!!!!!!!!!!==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230106152420664.png" alt="image-20230106152420664" style="zoom: 33%;" />

## Problem 8 (3 pt)

Right now, our ants are quite frail. We'd like to provide a way to help them last longer against the onslaught of the bees. Enter the `BodyguardAnt`.现在，我们的蚂蚁非常脆弱。我们想提供一种方法，帮助它们在蜜蜂的攻击下保持更长的时间。输入“保镖”。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230106151038646.png" alt="image-20230106151038646" style="zoom:25%;" />

To implement the `BodyguardAnt`, we will break up this problem into 3 subparts. In each part, we will making changes in either the `ContainerAnt` class, `Ant` class, or `BodyguardAnt` class.为了实现“BodyguardAnt”，我们将把这个问题分成3个子部分。在每个部分中，我们将对“ContainerAnt”类、“Ant”类或“BodyguardAnt”类进行更改。

### Problem 8a

“BodyguardAnt”与普通蚂蚁不同，因为它是==`ContainerAnt`==；**它可以容纳另一只蚂蚁并保护它，所有这些都在一个`place`**。**当“蜜蜂”在一只蚂蚁包含另一只蚂蚁的“地方”螫伤蚂蚁时，只有容器受损。容器内的蚂蚁仍然可以执行其原始动作。如果容器腐烂，所装的蚂蚁仍留在原处（然后可能会被损坏）。**

Each `ContainerAnt` has an instance attribute `ant_contained` that stores the ant it contains. This ant, `ant_contained`, initially starts off as `None` to indicate that there is no ant being stored yet. Implement the `store_ant` method so that it sets the `ContainerAnt`'s `ant_contained` instance attribute to the passed in `ant` argument. Also implement the `ContainerAnt`'s `action` method to perform its `ant_contained`'s action if it is currently containing an ant.**每个“ContainerAnt”都有一个实例属性“ant_contained”，用于存储其包含的蚂蚁。**这个蚂蚁“ant_contained”最初以“None”开头，表示还没有蚂蚁被存储**。实现“store_ant”方法，以便将“ContainerAnt”的“ant_contained”实例属性设置为传入的“ant”参数。**如果当前包含蚂蚁，还可以实现“ContainerAnt”的“action”方法来执行其“ant_contained”操作。

In addition, to ensure that a container and its contained ant can both occupy a place at the same time (a maximum of two ants per place), but only if exactly one is a container, we can create an `can_contain` method.此外，为了确保一个容器及其包含的蚂蚁可以同时占据一个位置（每个位置最多两个蚂蚁），但只有只有一个是容器，我们才能创建“can_contain”方法。

There is already an `Ant.can_contain` method, but it always returns `False`. Override the method `ContainerAnt.can_contain` so that it takes an ant `other` as an argument and returns `True` if:

- This `ContainerAnt` does not already contain another ant.
- The other ant is not a container.

> *提示：*您可能会发现每个“Ant”都有“is_container”属性，用于检查特定的“Ant”是否为容器。

**==新知识:kargs,也叫字典参数==**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230106195005502.png" alt="image-20230106195005502" style="zoom: 50%;" />

#### ==**位置参数与关键字参数**==

[python](https://so.csdn.net/so/search?q=python&spm=1001.2101.3001.7020)强大的类型推导，有时也会带来一些副作用，比如有时编译器会报如下错误：

```
TypeError: Function takes at most 1 positional arguments (2 given)
# 函数最多接受一个位置参数，却提供了两个
```

所谓positional argument位置参数，是指用相对位置指代参数。关键字参数（keyword argument），见名知意使用关键字指代参数。位置参数或者按顺序传递参数，或者使用名字，自然使用名字时，对顺序没有要求。

> A positional argument is a name that is not followed by an equal assign（=） and default value.
>
> A keyword argument is followed by an equal sign and an expression that gives its default value.

以上的两条引用是针对**函数的定义**（definition of the function）来说的，与**函数的调用**（calls to the function），也即在函数的调用端，既可以使用位置标识参数，也可使用关键字。

```py
def fn(a, b, c=1):
    return a*b+c
print(fn(1, 2))          # 3, positional(a, b) and default(c)
print(fn(1, 2, 3))       # 5, positional(a, b)
print(fn(c=5, b=2, a=2)) # 9, named(b=2, a=2)
print(fn(c=5, 1, 2))     # syntax error
print(fn(b=2, a=2))      # 5, named(b=2, a=2) and default
print(fn(5, c=2, b=1))   # 7, positional(a), named(b).
print(fn(8, b=0))        # 1, positional(a), named(b), default(c=1)
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230106201503961.png" alt="image-20230106201503961" style="zoom:33%;" />

### Problem 8b

Modify `Ant.add_to` to allow a container and a non-container ant to occupy the same place according to the following rules:

修改`Ant.add_to`以允许容器和非容器蚂蚁根据以下规则占据同一位置：

- If the ant originally occupying a place can contain the ant being added, then both ants occupy the place and original ant contains the ant being added.如果最初占据一个位置的蚂蚁可以包含被添加的蚂蚁，那么两个蚂蚁都占据了该位置，而最初的蚂蚁包含被添加蚂蚁。

- If the ant being added can contain the ant originally in the space, then both ants occupy the place and the (container) ant being added contains the original ant.如果被添加的蚂蚁可以容纳空间中的蚂蚁，那么两个蚂蚁都占据了这个位置，而被添加的（容器）蚂蚁包含了原始蚂蚁。(**无论如何都是container包含另一个**)

- If neither `Ant` can contain the other, raise the same `AssertionError` as before (the one already present in the starter code).如果两个“Ant”都不能包含另一个，则引发与之前相同的“AssertionError”（启动器代码中已存在的错误）。

- **Important:** If there are two ants in a specific `Place`, the `ant` attribute of the `Place` instance should refer to the container ant, and the container ant should contain the non-container ant**.**

  **重要提示：如果在一个特定的“Place”中有两个蚂蚁，“Place”实例的“ant”属性应该引用容器蚂蚁，而容器蚂蚁应该包含非容器蚂蚁。**

> *Hint*: You should also take advantage of the `can_contain` method you wrote and avoid repeating code.
>
> *提示*：您还应该利用您编写的“can_contain”方法，避免重复代码。

> **Note:** If you're getting an "unreachable code" warning for `Ant.add_to` via the VSCode Pylance extension, it's fine to ignore this specific warning, as the code is actually run (the warning *in this case* is inaccurate).
>
> **注意：**如果您通过VSCodePylance扩展获得“无法访问的代码”警告“Ant.add_to”，可以忽略此特定警告，因为代码实际上正在运行（在这种情况下，警告*不准确）。

### Problem 8c

添加`BodyguardAnt__init__`设置蚂蚁的初始健康量。我们不需要在这里创建“action”方法，因为“BodyguardAnt”类从“ContainerAnt”类继承了它。

## Problem 9 (2 pt)

The `BodyguardAnt` provides great defense, but they say the best defense is a good offense. The `TankAnt` is a `ContainerAnt` that protects an ant in its place and also deals 1 damage to all bees in its place each turn. Like any `ContainerAnt`, a `TankAnt` allows the ant that it contains to perform its action each turn.“保镖”提供了很好的防守，但他们说最好的防守是好的进攻**。“TankAnt”是一种“集装箱蚂蚁”，它保护一只蚂蚁在它的位置，并且每回合对该位置的所有蜜蜂造成1点伤害。与任何“ContainerAnt”一样，“TankAnt”允许它所包含的蚂蚁每回合执行其动作。**

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230106210058880.png" alt="image-20230106210058880" style="zoom:25%;" />

We have not provided you with a class header. Implement the `TankAnt` class from scratch. Give it a class attribute `name` with the value `'Tank'` (so that the graphics work) and a class attribute `implemented` with the value `True` (so that you can use it in a game).我们没有为您提供类标题。从头开始实现“TankAnt”类。给它一个值为“Tank”的类属性“name”（以便图形工作）和一个值“True”的类特性“implemented”（以便在游戏中使用）。

You should not need to modify any code outside of the `TankAnt` class. If you find yourself needing to make changes elsewhere, look for a way to write your code for the previous question such that it applies not just to `BodyguardAnt` and `TankAnt` objects, but to container ants in general.您不需要修改“TankAnt”类之外的任何代码。如果您发现自己需要在其他地方进行更改，请寻找一种方法来编写前一个问题的代码，使其不仅适用于“BodyguardAnt”和“TankAnt”对象，而且适用于一般的容器蚂蚁。

> *Hint*: The only methods you need to override from `TankAnt`'s parent class are `__init__` and `action`.
>
> *提示*：需要从“TankAnt”的父类重写的方法只有“__init__”和“action”。

> *Hint*: Like with `FireAnt`, it is possible that damaging a bee will cause it to be removed from its place.
>
> *提示*：与“FireAnt”一样，损坏蜜蜂可能会导致它离开它的位置。

# Phase 3: Water and Might

In the final phase, you're going to add one last kick to the game by introducing a new type of place and new ants that are able to occupy this place. One of these ants is the most important ant of them all: the queen of the colony!

在最后阶段，你将通过引入一种新的地方和能够占据这个地方的新蚂蚁，为游戏增加最后一个机会。其中一只蚂蚁是所有蚂蚁中最重要的一只：蚁群的女王！

## Problem 10 (1 pt)

Let's add water to the colony! Currently there are only two types of places, the `Hive` and a basic `Place`. To make things more interesting, we're going to create a new type of `Place` called `Water`.让我们给殖民地加水！目前只有两种类型的场所，“蜂巢”和基本的“场所”。为了让事情更有趣，我们将创建一种新的“场所”，称为“水”。

Only an insect that is waterproof can be placed in `Water`. In order to determine whether an `Insect` is waterproof, add a new class attribute to the `Insect` class named `is_waterproof` that is set to `False`. Since bees can fly, set their `is_waterproof` attribute to `True`, overriding the inherited value.只有防水的昆虫才能放在“水中”。为了确定“昆虫”是否防水，请将名为“is_waterproof”的新类属性添加到设置为“False”的“昆虫”类中。因为蜜蜂会飞，所以将它们的“is_waterproof”属性设置为“True”，覆盖继承的值。

Now, implement the `add_insect` method for `Water`. First, add the insect to the place regardless of whether it is waterproof. Then, if the insect is not waterproof, reduce the insect's health to 0. ***Do not repeat code from elsewhere in the program**.* Instead, use methods that have already been defined.现在，为“Water”实现“add_insect”方法。首先，无论是否防水，都要将昆虫添加到该位置。然后，如果昆虫不防水，则将昆虫的健康降为0*不要重复程序中其他地方的代码。*而是使用已经定义的方法。

## Problem 11 (1 pt)

Currently there are no ants that can be placed on `Water`. Implement the `ScubaThrower`, which is a subclass of `ThrowerAnt` that is more costly and waterproof, *but otherwise identical to its base class*. A `ScubaThrower` should not lose its health when placed in `Water`.目前没有蚂蚁可以放在“水”上。实现“ScubaThrower”，它是“ThrowerAnt”的一个子类，成本更高，防水性更强，但在其他方面与基类相同。“飞毛腿”放在“水中”时不应失去健康。 (飞毛腿)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230106214633650.png" alt="image-20230106214633650" style="zoom:25%;" />

(本题的class需要从头开始写)

## Problem 12 (3 pt)

Finally, implement the `QueenAnt`. The queen is a waterproof `ScubaThrower` that inspires her fellow ants through her bravery. In addition to the standard `ScubaThrower` action, the `QueenAnt` doubles the damage of all the ants behind her each time she performs an action. Once an ant's damage has been doubled, it is *not* doubled again for subsequent turns.最后，实现“QueenAnt”。女王是一只防水的“飞毛腿”，通过她的勇敢激励着她的同伴们。除了标准的“飞毛腿”动作外，“QueenAnt”每次执行动作时，其身后所有蚂蚁的伤害都会加倍。**一旦一只蚂蚁的伤害增加一倍，它在接下来的回合中不会再增加一倍。**

> Note: The reflected damage of a `FireAnt` should not be doubled, only the extra damage it deals when its health is reduced to 0.
>
> 注意：“FireAnt”的反射伤害不应加倍，只有当其生命值降为0时所造成的额外伤害。

However, with great power comes great responsibility. The `QueenAnt` is governed by three special rules:然而，强大的力量带来巨大的责任。“QueenAnt”由三条特殊规则管辖：

1. 如果女王的生命值降到0，蚂蚁就会输。在这种情况下，您需要重写“QueenAnt”中的“Insect.reduce_health”并调用“ants_lose（）”，以便向模拟器发出游戏结束的信号。（如果有蜜蜂到达隧道尽头，蚂蚁也会输。）
2. There can be only one queen. A second queen cannot be constructed. To check if an Ant can be constructed, we use the `Ant.construct()` class method to either construct an Ant if possible, or return `None` if not. You will need to override `Ant.construct` as a class method of `QueenAnt` in order to add this check. To keep track of whether a queen has already been created, you can use an instance variable added to the current `GameState`. (The `GameState` class is defined near the bottom of `ants.py`).只能有一个女王。第二个皇后是无法建造的。为了检查是否可以构造Ant，我们使用`Ant.construct（）`类方法在可能的情况下构造Ant，否则返回`None`。为了添加此检查，您需要将“Ant.construct”重写为“QueenAnt”的类方法。要跟踪女王是否已经创建，可以使用添加到当前“GameState”的实例变量。（“GameState”类定义在“ants.py”的底部附近）。
3. The queen cannot be removed. Attempts to remove the queen should have no effect (but should not cause an error). You will need to override `Ant.remove_from` in `QueenAnt` to enforce this condition.女王不能被移除。试图移除女王应该没有效果（但不应该导致错误）。您需要重写`QueenAnt `中的`Ant.remove_from`来强制执行此条件。

> *提示：*为了使她身后所有蚂蚁的伤害加倍，您可以填写“Ant”类中定义的“double”方法。“double”方法可以在适当的QueenAnt实例方法中使用。**为了避免蚂蚁的伤害加倍，请在调用“QueenAnt.action”时，以持续的方式标记受到双重伤害的蚂蚁。**

> *提示：*当蚂蚁的伤害加倍时，请记住一个“地方”可能有不止一只蚂蚁，比如一只蚂蚁在保护另一只蚂蚁。

> *Hint:* Think about how you can call the `construct` method of the superclass of `QueenAnt`. Remember that you ultimately want to construct a `QueenAnt`, not a regular `Ant` or a `ScubaThrower`.*提示：*考虑如何调用“QueenAnt”超类的“construct”方法。请记住，您最终想要构建一个“QueenAnt”，而不是一个常规的“Ant”或“ScubaThrower”。

> 注意：“construct”方法的参数为“cls”。这与我们在实例方法中使用“self”的方式类似。因此，当我们调用类方法时，不需要使用“cls”作为参数来调用它。阅读有关类方法的更多信息。检查[这个](https://pencilprogrammer.com/python-self-vs-cls/)关于“cls”与“self”的在线指南。

> *Hint:* You can find each `Place` in a tunnel behind the `QueenAnt` by starting at the ant's `place.exit` and then repeatedly following its `exit`. The `exit` of a `Place` at the end of a tunnel is `None`.*提示：*您可以从蚂蚁的“Place.ext”开始，然后重复跟随它的“exit”，在“QueenAnt”后面的隧道中找到每个“Place”。隧道末端“地点”的“出口”是“无”。

==@classmethod **是将此方法声明为类方法**==

## Extra Credit (2 pt)

Implement a final thrower ant that does zero damage, but instead applies a temporary effect on the `action` method of a `Bee` instance that it calls `throw_at` on. We will be implementing this new ant, `SlowThrower`, which inherits from `ThrowerAnt`.实现一个零伤害的最终投掷蚂蚁，但对它调用“throw_at”的“Bee”实例的“action”方法施加临时效果。我们将实现这个新蚂蚁“SlowThrower”，它继承自“ThrowAnt”。

`SlowThrower` throws sticky syrup at a bee, slowing it for 5 turns. When a bee is slowed, it takes the regular Bee action when `gamestate.time` is even, and takes no action (does not move or sting) otherwise. If a bee is hit by syrup while it is already slowed, it is slowed for 5 turns starting from the *most recent* time it is hit by syrup. That is, if a bee is hit by syrup, takes 2 turns, and is hit by syrup again, it will be slowed for 5 turns after the *second* time it is hit by syrup.`SlowThrower向蜜蜂投掷粘稠糖浆，使其减速5圈。当蜜蜂减速时，它会在“gamestate.time”为偶数时采取常规的蜜蜂动作，否则不采取任何动作（不移动或刺痛）。如果一只蜜蜂被糖浆击中而它已经减速，那么它会从最近一次被糖浆击中开始减速5圈。也就是说，如果一只蜜蜂被糖浆击中，转了两圈，又被糖浆击中了，那么在被糖浆击中的第二次之后，它将减速5圈。

In order to complete the implementations of this `SlowThrower`, you will need to set its class attributes appropriately and implement the `throw_at` method in `SlowThrower`.为了完成此“SlowThrower”的实现，您需要适当设置其类属性，并在“SlowThrower”中实现“throw_at”方法。

**Important Restriction:** You may *not* modify any code outside the `SlowThrower` class for this problem. That means you may *not* modify the `Bee.action` method directly. Our tests will check for this.**重要限制：**对于此问题，您不能*修改“SlowThrower”类之外的任何代码。这意味着您可以*不*直接修改“Bee.action”方法。我们的测试将对此进行检查。

> *Hint*: Assign `target.action` to a function that sometimes calls `Bee.action`. You can use an instance attribute to track how many more turns the bee will be slowed, and once the effect is over, `Bee.action` should be called every turn.*提示*：将“target.action”分配给一个有时调用“Bee.action”的函数。您可以使用实例属性来跟踪蜜蜂还会减速多少圈，一旦效果结束，应在每一圈调用“Bee.action”。

![image-20230107002219256](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230107002219256.png)