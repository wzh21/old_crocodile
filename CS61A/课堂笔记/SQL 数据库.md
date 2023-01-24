# SQL

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116224736547.png" alt="image-20230116224736547" style="zoom:50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116224750152.png" alt="image-20230116224750152" style="zoom: 50%;" />

+ **declarative language : 声明式语言**   例如 SQL Prolog
+ **imperative language : 命令式语言**   例如 python scheme

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116230114658.png" alt="image-20230116230114658" style="zoom:50%;" />

> select 涵盖了SQL的大部分核心

> from scratch : 从头开始		project : 投影

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116231957082.png" alt="image-20230116231957082" style="zoom:50%;" />

## projecting 

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116232759030.png" alt="image-20230116232759030" style="zoom:50%;" />

## Arithmetic

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230116233428830.png" alt="image-20230116233428830" style="zoom:50%;" />

**==SELECT 选择的是一列==**

## group

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230117223806840.png" alt="image-20230117223806840" style="zoom:67%;" />

# Lab 13 SQL

## SQL Basics

### Creating Tables

**You can create SQL tables either from scratch or from existing tables 您可以从头开始或从现有表创建SQL表。.**

下面的语句通过指定列名和值而不引用另一个表来创建表。每个“SELECT”子句指定一行的值，**“UNION”用于将行连接在一起。**“AS”子句为每一列指定一个名称；它不需要在第一行之后的后续行中重复。

```sql
CREATE TABLE [table_name] AS
  SELECT [val1] AS [column1], [val2] AS [column2], ... UNION
  SELECT [val3]             , [val4]             , ... UNION
  SELECT [val5]             , [val6]             , ...;
```

假设我们想制作一个名为“big_game”的下表，记录每年大赛的得分。此表有三列：“伯克利”、“斯坦福”和“年份”。

![img](https://inst.eecs.berkeley.edu/~cs61a/fa22/lab/lab13/assets/big-game.png)

我们可以使用以下“CREATE TABLE”语句执行此操作：

```sql
CREATE TABLE big_game AS
  SELECT 30 AS berkeley, 7 AS stanford, 2002 AS year UNION
  SELECT 28,             16,            2003         UNION
  SELECT 17,             38,            2014;
```

### Selecting From Tables

更常见的是，我们将通过使用“SELECT”语句从现有表中选择所需的特定列来创建新表，如下所示：

```sql
SELECT [columns] FROM [tables] WHERE [condition] ORDER BY [columns] LIMIT [limit];
```

Let's break down this statement:

- `SELECT [columns]` tells SQL that we want to include the given columns in our output table; `[columns]` is a comma-separated list of column names, and `*` can be used to select all columns ==`SELECT[columns]`告诉SQL，我们希望在输出表中包含给定的列`[columns]`是以逗号分隔的列名列表，`*`可用于选择所有列==

> ==*是选择所有的意思==

- `FROM [table]` tells SQL that the columns we want to select are from the given table; see the [joins section](https://inst.eecs.berkeley.edu/~cs61a/fa22/lab/lab13/) to see how to select from multiple tables

- `WHERE [condition]` filters the output table by only including rows whose values satisfy the given `[condition]`, a boolean expression   `WHERE[condition]`通过仅包含值满足给定“[condition]”（布尔表达式）的行来过滤输出表

- `ORDER BY [columns]` orders the rows in the output table by the given comma-separated list of columns

- `LIMIT [limit]` limits the number of rows in the output table by the integer `[limit]`

  ==`LIMIT[LIMIT]`通过整数`[LIMIT]`限制输出表中的行数==

> *Note:* We capitalize SQL keywords purely because of style convention. It makes queries much easier to read, though they will still work if you don't capitalize keywords.*注意：*我们将SQL关键字大写纯粹是因为样式约定。它使查询更容易阅读，但如果不将关键字大写，它们仍然有效。

Here are some examples:

Select all of Berkeley's scores from the `big_game` table, but only include scores from years past 2002:

```sql
sqlite> SELECT berkeley FROM big_game WHERE year > 2002;
28
17
```

Select the scores for both schools in years that Berkeley won:

```sql
sqlite> SELECT berkeley, stanford FROM big_game WHERE berkeley > stanford;
30|7
28|16
```

Select the years that Stanford scored more than 15 points:

```sql
sqlite> SELECT year FROM big_game WHERE stanford > 15;
2003
2014
```

### SQL operators

Expressions in the `SELECT`, `WHERE`, and `ORDER BY` clauses can contain one or more of the following operators:“SELECT”、“WHERE”和“ORDER BY”子句中的表达式可以包含以下一个或多个运算符：

- comparison operators: ==`=`==, `>`, `<`, `<=`, `>=`,==`<>` or `!=` ("not equal") **这俩都是不等于**==
- ==boolean operators: `AND`, `OR`==
- arithmetic operators: `+`, `-`, `*`, `/`
- ==concatenation operator: `||`==串联运算符

> 注:<> 这个是最早的用法.	!=是后来才加上的.
>
> 两者意义相同  ,  在可移植性上前者优于后者
>
> 故而sql语句中尽量使用<>来做不等判断

Here are some examples:

Output the ratio of Berkeley's score to Stanford's score each year:

```sql
sqlite> select berkeley * 1.0 / stanford from big_game;
0.447368421052632
1.75
4.28571428571429
```

Output the sum of scores in years where both teams scored over 10 points:

```sql
sqlite> select berkeley + stanford from big_game where berkeley > 10 and stanford > 10;
55
44
```

Output a table with a single column and single row containing the value "hello world":输出一个包含值“helloworld”的单列单行表：

```sql
sqlite> SELECT "hello" || " " || "world";
hello world
```

## Joins

To select data from multiple tables, we can use joins. There are many types of joins, but the only one we'll worry about is the inner join. To perform an inner join on two on more tables, simply list them all out in the `FROM` clause of a `SELECT` statement:要从多个表中选择数据，我们可以使用联接。有很多类型的联接，但我们唯一要担心的是内部联接。要对两个或多个表执行内部联接，只需在“SELECT”语句的“FROM”子句中列出所有表：

```sql
SELECT [columns] FROM [table1], [table2], ... WHERE [condition] ORDER BY [columns] LIMIT [limit];
```

We can select from multiple different tables or from the same table multiple times.我们可以从多个不同的表中选择，也可以多次从同一个表中选择。

Let's say we have the following table that contains the names of head football coaches at Cal since 2002:假设我们有下表，其中包含自2002年以来加州大学足球教练的姓名：

```sql
CREATE TABLE coaches AS
  SELECT "Jeff Tedford" AS name, 2002 as start, 2012 as end UNION
  SELECT "Sonny Dykes"         , 2013         , 2016        UNION
  SELECT "Justin Wilcox"       , 2017         , null;
```

When we join two or more tables, the default output is a [cartesian product](https://en.wikipedia.org/wiki/Cartesian_product). For example, if we joined `big_game` with `coaches`, we'd get the following:当我们连接两个或多个表时，默认输出为[笛卡尔乘积](https://en.wikipedia.org/wiki/Cartesian_product). 例如，如果我们将“big_game”与“coaches”连接，则会得到以下结果：

<img src="https://inst.eecs.berkeley.edu/~cs61a/fa22/lab/lab13/assets/joins.png" alt="img" style="zoom:50%;" />

If we want to match up each game with the coach that season, we'd have to compare columns from the two tables in the `WHERE` clause:

```sql
sqlite> SELECT * FROM big_game, coaches WHERE year >= start AND year <= end;
17|38|2014|Sonny Dykes|2013|2016
28|16|2003|Jeff Tedford|2002|2012
30|7|2002|Jeff Tedford|2002|2012
```

The following query outputs the coach and year for each Big Game win recorded in `big_game`:

```sql
sqlite> SELECT name, year FROM big_game, coaches
...>        WHERE berkeley > stanford AND year >= start AND year <= end;
Jeff Tedford|2003
Jeff Tedford|2002
```

In the queries above, none of the column names are ambiguous. For example, it is clear that the `name` column comes from the `coaches` table because there isn't a column in the `big_game` table with that name. However, if a column name exists in more than one of the tables being joined, or if we join a table with itself, we must disambiguate the column names using *==aliases别名==*.在上面的查询中，没有一个列名是不明确的。例如，很明显，“name”列来自“coaches”表，因为“big_game”表中没有具有该名称的列。但是，如果一个列名存在于要连接的多个表中，或者如果我们将一个表与其自身连接，则必须使用*alias*消除列名的歧义。

For examples, let's find out what the score difference is for each team between a game in `big_game` and any previous games. Since each row in this table represents one game, in order to compare two games we must join `big_game` with itself:例如，让我们来看看“big_game”中的一场比赛与之前的任何一场比赛之间，每支球队的得分差距是多少。由于此表中的每一行代表一个游戏，为了比较两个游戏，我们必须将“big_game”与其自身连接起来：

```sql
sqlite> SELECT b.Berkeley - a.Berkeley, b.Stanford - a.Stanford, a.Year, b.Year
...>        FROM big_game AS a, big_game AS b WHERE a.Year < b.Year;
-11|22|2003|2014
-13|21|2002|2014
-2|9|2002|2003
```

In the query above, we give the alias `a` to the first `big_game` table and the alias `b` to the second `big_game` table. We can then reference columns from each table using dot notation with the aliases, e.g. `a.Berkeley`, `a.Stanford`, and `a.Year` to select from the first table.在上面的查询中，我们将==别名“a”==赋予第一个“big_game”表，将别名“b”赋予第二个“big_game”表格。然后，我们可以使用带别名的点符号引用每个表中的列，例如“a.Berkeley”、“a.Stanford”和“a.Year”，以便从第一个表中进行选择。

## SQL Aggregation

Previously, we have been dealing with queries that process one row at a time. When we join, we make pairwise combinations of all of the rows. When we use `WHERE`, we filter out certain rows based on the condition. Alternatively, applying an [aggregate function](http://www.sqlite.org/lang_aggfunc.html) such as `MAX(column)` combines the values in multiple rows.以前，我们一直在处理一次处理一行的查询。当我们连接时，我们对所有行进行成对组合。当我们使用“WHERE”时，我们根据条件过滤掉某些行。或者，应用[聚合函数](http://www.sqlite.org/lang_aggfunc.html)诸如“MAX（列）”**==组合多行中的值。==**

By default, we combine the values of the *entire* table. For example, if we wanted to count the number of flights from our `flights` table, we could use:

```sql
sqlite> SELECT COUNT(*) from FLIGHTS;
13
```

What if we wanted to group together the values in similar rows and perform the aggregation operations within those groups? We use a `GROUP BY` clause.

Here's another example. For each unique departure, collect all the rows having the same departure airport into a group. Then, select the `price` column and apply the `MIN` aggregation to recover the price of the cheapest departure from that group. The end result is a table of departure airports and the cheapest departing flight.

```sql
sqlite> SELECT departure, MIN(price) FROM flights GROUP BY departure;
AUH|932
LAS|50
LAX|89
SEA|32
SFO|40
SLC|42
```

Just like how we can filter out rows with `WHERE`, we can also filter out groups with `HAVING`. Typically, a `HAVING` clause should use an aggregation function. Suppose we want to see all airports with at least two departures:

```sql
sqlite> SELECT departure FROM flights GROUP BY departure HAVING COUNT(*) >= 2;
LAX
SFO
SLC
```

Note that the `COUNT(*)` aggregate just counts the number of rows in each group. Say we want to count the number of *distinct* airports instead. Then, we could use the following query:

```sql
sqlite> SELECT COUNT(DISTINCT departure) FROM flights;
6
```

This enumerates all the different departure airports available in our `flights` table (in this case: SFO, LAX, AUH, SLC, SEA, and LAS).

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230117121206758.png" alt="image-20230117121206758" style="zoom: 50%;" />

## Questions

### Q1: What Would SQL print?

> Note: there is no submission for this question

First, load the tables into sqlite3.

```sql
$ python3 sqlite_shell.py --init lab13.sql
```

Before we start, inspect the schema of the tables that we've created for you:

```sql
sqlite> .schema
```

This tells you the name of each of our tables and their attributes.

Let's also take a look at some of the entries in our table. There are a lot of entries though, so let's just output the first 20:

```sql
sqlite> SELECT * FROM students LIMIT 20;
```

If you're curious about some of the answers students put into the Google form, open up `data.sql` in your favorite text editor and take a look!

For each of the SQL queries below, think about what the query is looking for, then try running the query yourself and see!

```sql
sqlite> SELECT * FROM students LIMIT 30; -- This is a comment. * is shorthand for all columns!
______
sqlite> SELECT color FROM students WHERE number = 7;
______
sqlite> SELECT song, pet FROM students WHERE color = "blue" AND date = "12/25";
______
```

==**Remember to end each statement with a `;`!**== To exit out of SQLite, type `.exit` or `.quit` or hit `Ctrl-C`.

### Q2: Go Bears! (And Dogs?)

Now that we have learned how to select columns from a SQL table, let's filter the results to see some more interesting results!既然我们已经学习了如何从SQL表中选择列，那么让我们**过滤结果**以查看一些更有趣的结果！

It turns out that 61A students have a lot of school spirit: the most popular favorite color was `'blue'`. You would think that this school spirit would carry over to the pet answer, and everyone would want a pet bear! Unfortunately, this was not the case, and the majority of students opted to have a pet `'dog'` instead. That is the more sensible choice, I suppose...事实证明，61A学生有很多校风：最受欢迎的颜色是“蓝色”。你会认为这种学校精神会延续到宠物的回答上，每个人都会想要一只宠物熊！不幸的是，事实并非如此，大多数学生选择养宠物“狗”。我想这是更明智的选择。。。

Write a SQL query to create a table that contains both the column `color` and the column `pet`, using the keyword `WHERE` to restrict the answers to the most popular results of color being `'blue'` and pet being `'dog'`.编写一个SQL查询，创建一个同时包含列“color”和列“pet”的表，使用关键字“WHERE”将答案限制为颜色为“blue”和宠物为“og”的最流行结果。

You should get the following output:

```sql
sqlite> SELECT * FROM bluedog;
blue|dog
blue|dog
blue|dog
blue|dog
blue|dog
blue|dog
blue|dog
blue|dog
blue|dog
blue|dog
CREATE TABLE bluedog AS
  SELECT "REPLACE THIS LINE WITH YOUR SOLUTION";
```

This isn't a very exciting table, though. Each of these rows represents a different student, but all this table can really tell us is how many students both like the color blue and want a dog as a pet, because we didn't select for any other identifying characteristics. Let's create another table, `bluedog_songs`, that looks just like `bluedog` but also tells us how each student answered the `song` question.不过，这不是一张非常令人兴奋的桌子。每一行代表一个不同的学生，但这张表真正能告诉我们的是，有多少学生既喜欢蓝色，又想要狗作为宠物，因为我们没有选择任何其他识别特征。让我们创建另一个表“bluedog_songs”，它看起来就像“bluedog”，但也告诉我们每个学生如何回答“歌曲”问题。

You should get the following output:

```sql
sqlite> SELECT * FROM bluedog_songs;
blue|dog|Glimpse of Us
blue|dog|The Other Side of Paradise
blue|dog|Leave the Door Open
blue|dog|Bohemian Rhapsody
blue|dog|Dancing Queen
blue|dog|Palette
blue|dog|Bohemian Rhapsody
blue|dog|good 4 u
blue|dog|Clair de Lune
blue|dog|Dancing Queen
CREATE TABLE bluedog_songs AS
  SELECT "REPLACE THIS LINE WITH YOUR SOLUTION";
```

This distribution of songs actually largely represents the distribution of song choices that the total group of students made, so perhaps all we've learned here is that there isn't a correlation between a student's favorite color and desired pet, and what song they could spend the rest of their life listening to. Even demonstrating that there is no correlation still reveals facts about our data though!	不过，这不是一张非常令人兴奋的桌子。每一行代表一个不同的学生，但这张表真正能告诉我们的是，有多少学生既喜欢蓝色，又想要狗作为宠物，因为我们没有选择任何其他识别特征。让我们创建另一个表“bluedog_songs”，它看起来就像“bluedog”，但也告诉我们每个学生如何回答“歌曲”问题。

```sql
CREATE TABLE bluedog AS
  SELECT color, pet FROM students WHERE color = "blue" AND pet = "dog";

CREATE TABLE bluedog_songs AS
  SELECT color, pet, song FROM students WHERE color = "blue" AND pet = "dog";
```

### Q3: The Smallest Unique Positive Integer

Who successfully managed to guess the smallest unique positive integer value? Let's find out!谁成功地猜到了最小的唯一正整数值？让我们来看看！

Write an SQL query to create a table with the columns `time` and `smallest` which contains the timestamp for each submission that made a unique guess for the smallest unique positive integer - that is, only one person put that number for their guess of the smallest unique integer. Also include their guess in the output.编写一个SQL查询，创建一个包含“time”和“minimal”列的表，其中包含对最小唯一正整数进行唯一猜测的每个提交的时间戳，也就是说，只有一个人将该数字用于对最小唯一整数的猜测。在输出中也包括他们的猜测。

> *Hint:* Think about what attribute you need to `GROUP BY`. Which groups do we want to keep after this? We can filter this out using a `HAVING` clause. If you need a refresher on aggregation, see the topics section.
>
> *提示：*考虑“GROUP BY”需要什么属性。在此之后，我们希望保留哪些组？我们可以使用“HAVING”子句将其过滤掉。如果需要复习聚合，请参阅主题部分。

The submission with the timestamp corresponding to the minimum value of this table is the timestamp of the submission with the smallest unique positive integer!具有与此表的最小值对应的时间戳的提交是具有最小唯一正整数的提交的时间戳！

```sql
CREATE TABLE smallest_int_having AS
  SELECT time, smallest FROM students GROUP BY smallest HAVING COUNT(*) = 1; -- count(*) is the number of column 
```

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230117130608983.png" alt="image-20230117130608983" style="zoom:50%;" />

> 相当于找出来这个smallest是独一无二的列表

### Q4: Matchmaker, Matchmaker

你希望找到新的朋友吗？你真走运！有了所有这些数据，我们很容易找到您的完美匹配。如果两个学生想要同一只宠物，对音乐有着相同的品味，那么他们显然就是朋友！为了给潜在的一对提供更多的信息，让我们把这两个人最喜欢的颜色也包括进来！

In order to match up students, you will have to do a join on the `students` table with itself. When you do a join, SQLite will match every single row with every single other row, so make sure you do not match anyone with themselves, or match any given pair twice!为了匹配学生，您必须在“学生”表上与自己进行连接。当您执行联接时，**SQLite将将每一行与其他每一行进行匹配，因此请确保您不将任何人与自己进行匹配，或将任何给定的对进行两次匹配！**

> **Important Note:** When pairing the first and second person, make sure that the first person responded first (i.e. they have an earlier `time`). This is to ensure your output matches our tests.**重要提示：**当第一个人和第二个人配对时，确保第一个人首先做出反应（即他们有更早的“时间”）。这是为了确保您的输出符合我们的测试。
>
> *Hint:* When joining table names where column names are the same, use dot notation to distinguish which columns are from which table: `[table_name].[column name]`. This sometimes may get verbose, so it’s stylistically better to give tables an alias using the `AS` keyword. The syntax for this is as follows:*提示：*当连接列名相同的表名时，请使用点符号来区分哪些列来自哪个表：`[table_name].[columen]`。这有时可能会变得冗长，因此使用“AS”关键字为表提供别名在风格上更好。其语法如下：
>
> ```sql
> SELECT <[alias1].[column name1], [alias2].[columnname2]...>
>     FROM <[table_name1] AS [alias1],[table_name2] AS [alias2]...> ...
> ```
>
> The query in the [football example](https://inst.eecs.berkeley.edu/~cs61a/fa22/lab/lab13/#joins) from earlier uses this syntax.

Write a SQL query to create a table that has 4 columns:

- The shared preferred `pet` of the pair
- The shared favorite `song` of the pair
- The favorite `color` of the first person
- The favorite `color` of the second person

```sql
CREATE TABLE matchmaker AS
  SELECT a.pet, a.song, a.color, b.color FROM students AS a, students AS b
    WHERE a.time < b.time AND a.pet = b.pet AND a.song = b.song
```

**==可以让同一张表"之间"聚合!!!!!!!!!!!!!!!!==**

### Q5: Sevens

Let's take a look at data from both of our tables, `students` and `numbers`, to find out if students that really like the number 7 also chose `'7'` for the obedience question. Specifically, we want to look at the students that fulfill the below conditions:让我们看看两张表中的数据，“学生”和“数字”，看看真正喜欢数字7的学生是否也选择了“7”作为服从问题。具体来说，我们希望考察满足以下条件的学生：

Conditions:

- reported that their favorite number (column `number` in `students`) was 7
- have `'True'` in column `'7'` in `numbers`, meaning they checked the number `7` during the survey

In order to examine rows from both the `students` and the `numbers` table, we will need to perform a join.

How would you specify the `WHERE` clause to make the `SELECT` statement only consider rows in the joined table whose values all correspond to the same student? If you find that your output is massive and overwhelming, then you are probably missing the necessary condition in your `WHERE` clause to ensure this.

> *Note:* The columns in the `numbers` table are strings with the associated number, so you must put quotes around the column name to refer to it. For example if you alias the table as `a`, to get the column to see if a student checked 9001, you must write `a.'9001'`.

**Write a SQL query to create a table with just the column `seven` from `students`, filtering first for students who said their favorite number (column `number`) was 7 in the `students` table and who checked the box for seven (column `'7'`) in the `numbers` table.**

The first 10 lines of this table should look like this:

```sql
sqlite> SELECT * FROM sevens LIMIT 10;
7
I find this question condescending
7
seven
7
7
7
7
```

```sql
CREATE TABLE sevens AS
   SELECT s.seven FROM students AS s, numbers AS c 
    WHERE s.number = 7 AND c.'7' = 'True' AND s.time = c.time;
```

> ==技巧 : 可以用s.time = c.time的方法确保是同一个元素==

## Cyber Monday疯狂(网购)星期一

### Q6: Price Check

Let's start off by surveying our options. Using the `products` table, write a query that creates a table `average_prices` that lists categories and the average price of items in the category (using [MSRP](https://en.wikipedia.org/wiki/List_price) as the price).让我们从调查我们的选择开始。使用“products”表，编写一个查询，创建一个表“average_prices”，列出类别和类别中项目的平均价格（使用[MSRP](https://en.wikipedia.org/wiki/List_price)作为价格）。

You should get the following output:

```sql
sqlite> SELECT category, ROUND(average_price) FROM average_prices;
computer|109.0
games|350.0
phone|90.0
```

```sql
CREATE TABLE average_prices AS
  SELECT category as category, AVG(msrp) as average_price FROM products GROUP BY category;
  -- alternate solution
  -- SELECT category, SUM(msrp)/COUNT(*) FROM products GROUP BY category;

```

### Q7: The Price is Right

Now, you want to figure out which stores sell each item in products for the lowest price. Write a SQL query that uses the `inventory` table to create a table `lowest_prices` that lists items, the stores that sells that item for the lowest price, and the price that the store sells that item for.现在，你想知道哪些商店以最低的价格销售每种产品。编写一个SQL查询，使用“inventory(库存)”表创建一个表“lowest_prices”，该表列出了项目、以最低价格销售该项目的商店以及该商店销售该项目所需的价格。

You should expect the following output:

```sql
sqlite> SELECT * FROM lowest_prices;
Hallmart|GameStation|298.98
Targive|QBox|390.98
Targive|iBook|110.99
RestBuy|kBook|94.99
Hallmart|qPhone|85.99
Hallmart|rPhone|69.99
RestBuy|uPhone|89.99
RestBuy|wBook|114.29
```

```sql
CREATE TABLE lowest_prices AS
  SELECT store, item, MIN(price) FROM inventory GROUP BY item;
  -- alternate solution
  -- SELECT * FROM inventory GROUP BY item HAVING MIN(price);
```

**==`MIN()`是聚合函数, 和下面这个`SELECT * FROM inventory GROUP BY item HAVING MIN(price)`是一个意思==**



### Q8: Bang for your Buck

You want to make a shopping list by choosing the item that is the best deal possible for every category. For example, for the "phone" category, the uPhone is the best deal because the MSRP price of a uPhone divided by its ratings yields the lowest cost. That means that uPhones cost the lowest money per rating point out of all of the phones. Note that the item with the lowest MSRP price may not necessarily be the best deal.

Write a query to create a table `shopping_list` that lists the items that you want to buy from each category.

After you've figured out which item you want to buy for each category, add another column that lists the store that sells that item for the lowest price.

You should expect the following output:

```sql
sqlite> SELECT * FROM shopping_list;
GameStation|Hallmart
uPhone|RestBuy
wBook|RestBuy
```



> **Hint**: You should use the `lowest_prices` table you created in the previous question.
>
> **Hint 2**: One approach to this problem is to create another table, then create shopping_list by selecting from that table.







### Q9: Driving the Cyber Highways

Using the Mb (megabits) column from the `stores` table, write a query to calculate the total amount of bandwidth needed to get everything in your shopping list.

```
CREATE TABLE total_bandwidth AS
  SELECT "REPLACE THIS LINE WITH YOUR SOLUTION";
```



> **Hint**: You should use the `shopping_list` table you created in the previous question.

