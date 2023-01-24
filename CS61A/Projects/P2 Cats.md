​																				**Project 2 : Cats**

In this project, you will write a program that measures typing speed. Additionally, you will implement typing autocorrect , which is a feature that attempts to correct the spelling of a word after a user types it. This project is inspired by [typeracer](https://play.typeracer.com/).在这个项目中，您将编写一个**测量打字速度**的程序。此外，您还将**实现键入自动更正**功能，这是一种在用户键入单词后尝试更正单词拼写的功能。该项目的灵感来自typeracer。

**Throughout the project, we will be making changes to functions in `cats.py`.**

在整个项目中，我们都将在“cats.py”中进行更改。

# Phase 1: Typing

## Prob 1 (1 pt)

```py
def pick(paragraphs, select, k):
    """
    >>> ps = ['hi', 'how are you', 'fine']
    >>> s = lambda p: len(p) <= 4
    >>> pick(ps, s, 0)
    'hi'
    >>> pick(ps, s, 1)
    'fine'
    >>> pick(ps, s, 2)
    ''
    """
    # 找到第k个满足要求的,如果满足要求的小于<k,那就返回空字符串''
    "*** YOUR CODE HERE ***"
    count = -1
    for elePara in paragraphs:
        if select(elePara):
            count += 1
        if count == k:
            return elePara
    return ''
```

## Prob 2 (1 pt)

实现`about`，它包含`topic`列表。它返回一个函数，该函数获取一个段落并返回一个布尔值，指示该段落是否包含`topic`中的任何单词。

```py
def about(topic):
    """Return a select function that returns whether
    a paragraph contains one of the words in TOPIC.
    Arguments:
        topic: a list of words(单词) related to a subject
        topic: 与主题相关的单词列表
    >>> about_dogs = about(['dog', 'dogs', 'pup', 'puppy'])
    >>> pick(['Cute Dog!', 'That is a cat.', 'Nice pup!'], about_dogs, 0)
    'Cute Dog!'
    >>> pick(['Cute Dog!', 'That is a cat.', 'Nice pup.'], about_dogs, 1)
    'Nice pup.'
    """
    assert all([lower(x) == x for x in topic]), 'topics should be lowercase.'
    # 复习一下all函数,检查是不是这个列表的所有元素都是True
    "*** YOUR CODE HERE ***"
    def select(elePara):
        for tpc in topic:
            split_list = split(remove_punctuation(elePara))
            for spt_ele in split_list:
                if lower(tpc) == lower(spt_ele):
                    return True
        return False 

    return select
    # version 1.0 的问题是,看的是有没有这个单词,不是看字符串里有没有
    # version 2.0 的问题是,单词必须完全相同(除了大小写),而不是包含关系(如cak和cake)
    # version 3.0 的问题是,必须去除逗号
    # better solution:
    def select(paragraph):
        words = set(split(lower(remove_punctuation(paragraph))))
        for x in topic:
            if x in words:
                return True
        return False
    return select
```

## Prob 3 (2 pts)

Implement `accuracy`, which takes a `typed` paragraph and a `source` paragraph. **It returns the percentage of words in `typed` that exactly match the corresponding words in `source`**. Case and punctuation must match as well. "Corresponding" here means that two words must occur at the same indices in `typed` and `source`—the first words of both must match, the second words of both must match, and so on.

A *word* in this context is any sequence of characters separated from other words by whitespace, so treat "dog;" as a single word.用空格作为分割

如果typed比source长，那么typed中没有对应词的额外词在source中都是不正确的。

如果typed和source都是空的，那么准确率为100.0。如果输入的是空的，但source不是空的，那么准确率为零。如果typed不是空的，但是source是空的，那么准确率是0。

```py
def accuracy(typed, source):
    """Return the accuracy (percentage of words typed correctly) of TYPED
    when compared to the prefix of SOURCE that was typed.

    Arguments:
        typed: a string that may contain typos
        source: a string without errors

    >>> accuracy('Cute Dog!', 'Cute Dog.')
    50.0
    >>> accuracy('A Cute Dog!', 'Cute Dog.')
    0.0
    >>> accuracy('cute Dog.', 'Cute Dog.')
    50.0
    >>> accuracy('Cute Dog. I say!', 'Cute Dog.')
    50.0
    >>> accuracy('Cute', 'Cute Dog.')
    100.0
    >>> accuracy('', 'Cute Dog.')
    0.0
    >>> accuracy('', '')
    100.0
    """
    typed_words = split(typed)
    source_words = split(source)
    # BEGIN PROBLEM 3
    "*** YOUR CODE HERE ***"
    type_len = len(typed_words)
    source_len = len(source_words)
    if type_len == 0:
        if source_len == 0:
            return 100.0
        else:
            return 0.0
    if source_len == 0:
        return 0.0

    correct = 0
    min_len = min(type_len, source_len)
    for i in range(0, min_len):
        if typed_words[i] == source_words[i]:
            correct += 1
    return 100 * correct / len(typed_words) 
```

## Prob 4 (1 pt)

返回一分钟打多少个' word ', 五个字符(算上空格)是一个'word'	

```python
def wpm(typed, elapsed): # elapse: [v] (时间)消逝
    """Return the words-per-minute (WPM) of the TYPED string.
    Arguments:
        typed: an entered string
        elapsed: an amount of time in seconds

    >>> wpm('hello friend hello buddy hello', 15)
    24.0
    >>> wpm('0123456789',60)
    2.0
    """
    assert elapsed > 0, 'Elapsed time must be positive'
    "*** YOUR CODE HERE ***"
    word_count = len(typed) / 5
    return word_count * (60 / elapsed)
```

# Phase 2: Autocorrect

## Problem 5 (2 pts)

Implement `autocorrect`, which takes a `typed_word`, a `word_list`, a `diff_function`, and a `limit`.

If the `typed_word` is contained **inside the `word_list`**, `autocorrect` returns that word.

*Otherwise*, `autocorrect` returns the word **from `word_list`** that has the lowest difference from the provided `typed_word` based on the `diff_function`. However, if the lowest difference between `typed_word` and any of the words in `word_list` **is greater than `limit`, then `typed_word` is returned instead.**(**注意,必须大于limit,没有等于**)

> **Important**: If `typed_word` is not contained inside `word_list`, and multiple strings have the same lowest difference from `typed_word` according to the `diff_function`, `autocorrect` should return the string that appears first in `word_list`.(有相同的difference,这时返回最先出现的那个最小差异的word)

diff函数接受三个参数。第一个是typed_word，第二个是source_word（在本例中是来自word_list的单词），第三个参数是limit。diff函数的输出是一个数字，表示两个字符串之间的差异量。

> **Note**: Some diff functions may be asymmetric (meaning flipping the first two parameters may yield a different output), so make sure to pass in the arguments to `diff_function` in the correct order. We will see an example of an asymmetric diff function in problem 7.某些diff函数可能是不对称的（这意味着翻转前两个参数可能会产生不同的输出），因此请确保以正确的顺序将参数传递给“diff_function”。我们将在问题7中看到一个不对称diff函数的示例。

Here is an example of a diff function that computes the minimum of `1 + limit` and the difference in length between the two input strings:下面是一个diff函数的示例，它计算“1+limit”的最小值和两个输入字符串之间的长度差：

```py
>>> def length_diff(w1, w2, limit):
...     return min(limit + 1, abs(len(w2) - len(w1)))
>>> length_diff('mellow', 'cello', 10)
1
>>> length_diff('hippo', 'hippopotamus', 5)
6
```

Implement:

==**better solution 真的是太棒了,一定要学会**==

```py
def autocorrect(typed_word, word_list, diff_function, limit):
    """Returns the element of WORD_LIST that has the smallest difference
    from TYPED_WORD. Instead returns TYPED_WORD if that difference is greater
    than LIMIT.

    Arguments:
        typed_word: a string representing a word that may contain typos
        word_list: a list of strings representing source words
        diff_function: a function quantifying the difference between two words
        limit: a number

    >>> ten_diff = lambda w1, w2, limit: 10 # Always returns 10
    >>> autocorrect("hwllo", ["butter", "hello", "potato"], ten_diff, 20)
    'butter'
    >>> first_diff = lambda w1, w2, limit: (1 if w1[0] != w2[0] else 0) # Checks for matching first char
    >>> autocorrect("tosting", ["testing", "asking", "fasting"], first_diff, 10)
    'testing'
    """
    # BEGIN PROBLEM 5
    "*** YOUR CODE HERE ***"
    if typed_word in word_list:
        return typed_word #一开始忘了这个了

    # my solution
    min_diff = diff_function(typed_word, word_list[0], limit)
    min_index = 0
    for i in range(0, len(word_list)):
        diff = diff_function(typed_word, word_list[i], limit)
        if diff < min_diff:
            min_diff = diff
            min_index = i
    
    if min_diff > limit:
        return typed_word
    else:
        return word_list[min_index]

    # better solution:
    words_diff = [diff_function(user_word, w, limit) for w in valid_words]
    similar_word, similar_diff = min(zip(valid_words, words_diff), key=lambda item: item[1])
    if similar_diff > limit:
        return user_word
    else:
        return similar_word


    # END PROBLEM 5
```

better solution利用了这一点:

```py
>>> a, b = (1, 2)
>>> a
1
>>> b
2
```

补充知识:

==**zip**Python 的 zip() 函数创建了一个迭代器，它将聚合来自两个或多个[可迭代对象](https://so.csdn.net/so/search?q=可迭代对象&spm=1001.2101.3001.7020)的元素。==您可以使用生成的迭代器快速一致地解决常见的编程问题，例如创建字典。

```py
>>> numbers = [1, 2, 3]
>>> letters = ['a', 'b', 'c']
>>> zipped = zip(numbers, letters)
>>> zipped  # Holds an iterator object
<zip object at 0x7fa4831153c8>
>>> type(zipped)
<class 'zip'>
>>> list(zipped)
[(1, 'a'), (2, 'b'), (3, 'c')]
```

## Problem 6 (3 pts)

Implement `feline_fixes`, which is a diff function that takes two strings. It returns the minimum number of characters that must be changed in the `typed` word in order to transform it into the `source` word. If the strings are not of equal length, the difference in lengths is added to the total.

==**Important**: You may not use `while`, `for`, or list comprehensions in your implementation. Use recursion.==

```py
def feline_fixes(typed, source, limit):
    """A diff function for autocorrect that determines how many letters
    in TYPED need to be substituted to create SOURCE, then adds the difference in
    their lengths and returns the result.
    Arguments:
        typed: a starting word
        source: a string representing a desired goal word
        limit: a number representing an upper bound on the number of chars that must change
    >>> big_limit = 10
    >>> feline_fixes("nice", "rice", big_limit)    # Substitute: n -> r
    1
    >>> feline_fixes("pill", "pillage", big_limit) # Don't substitute anything, length difference of 3.
    3
    >>> feline_fixes("roses", "arose", big_limit)  # Substitute: r -> a, o -> r, s -> o, e -> s, s -> e
    5
    >>> feline_fixes("rose", "hello", big_limit)   # Substitute: r->h, o->e, s->l, e->l, length difference of 1.
    5
    """
    if len(typed) == 0:
        return len(source)
    if len(source) == 0:
        return len(typed)
    if typed[0] != source[0]:
        if limit == 0:
            return 1
        return 1 + feline_fixes(typed[1:], source[1:], limit - 1)
    else:
        return feline_fixes(typed[1:], source[1:], limit)
```

limit:必须更改的字数的上限(超过就不更改了)

==**本体精华:切片的使用和递归的结合!!!!!!!!!!!!!**==

## Problem 7 (3 pts)

返回将起始词转化为目标词所需的最小编辑操作数。

编辑有三种: 增添, 删除, 替换

这道题已经提供了模板:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20221231105145133.png" alt="image-20221231105145133" style="zoom: 25%;" />

> **Hint:** This is a recursive function with three recursive calls. One of these recursive calls will be similar to the recursive call in `feline_fixes`.这是一个具有三个递归调用的递归函数。其中一个递归调用类似于“feline_fixes”中的递归调用。

>重要提示：如果所需的编辑次数大于限制，则minium_mewations应返回任何大于限制的数字，并应尽量减少执行此操作所需的计算量。
>这两次调用minium_mewations需要大约相同的时间来评估：
>
>````py
>>>> limit = 2
>>>> minimum_mewtations("ckiteus", "kittens", limit) > limit
>True
>>>> minimum_mewtations("ckiteusabcdefghijklm", "kittensnopqrstuvwxyz", limit) > limit
>True
>````



# Phase 3: Multiplayer 多人游戏

Typing is more fun with friends! You'll now implement multiplayer functionality, so that when you run `cats_gui.py` on your computer, it connects to the course server at [cats.cs61a.org](https://cats.cs61a.org/) and looks for someone else to race against.与朋友一起打字更有趣！现在您将实现多人游戏功能，这样当您在计算机上运行“cats_gui.py”时，它将连接到[cats.cs61a.org.]上的课程服务器(https://cats.cs61a.org/)并寻找其他人与之竞争。

## Problem 8 (2 pts)

Implement `report_progress`, which is called every time the user finishes typing a word. It takes a list of the words `typed`, a list of the words in the `prompt`, the user's `user_id`, and a `upload` function that is used to upload a progress report to the multiplayer server. There will never be more words in `typed` than in `prompt`.实现report_progress，每当用户打完一个字就会调用这个函数。它需要一个`typed`的单词列表，一个`prompt`列表，用户的 `user_id`，以及一个`upload`函数，用于将进度报告上传到多人游戏服务器。`typed`单词永远不会多于`prompt`的单词。

**Your progress is a ratio of the words in the `prompt` that you have typed correctly**, up to the first incorrect word, divided by the number of `prompt` words. For example, this example has a progress of `0.25`:您的进度是“prompt”中您键入正确的单词与第一个错误单词的比率，除以“prompt”单词的数量。例如，此示例的进度为“0.25”：

**Your `report_progress` function should do two things: upload a message to the multiplayer server and return the progress of the player with `user_id`.您的“report_proggress”函数应该做两件事：将消息上传到多人服务器，并使用“user_id”返回玩家的进度.**

You can upload a message to the multiplayer server by calling the `upload` function on a two-element dictionary containing the keys `'id'` and `'progress'`. You should then return the player's progress, which is the ratio of words you computed.您可以通过调用包含键“id”和“progress”的双元素字典上的“上传”功能将消息上传到多人服务器。然后，您应该返回玩家的进度，这是您计算的单词比率。

> **Hint:** See the dictionary below for an example of a potential input into the `upload` function. This dictionary represents a player with `user_id` 1 and `progress` 0.6.
>
> ```py
> {'id': 1, 'progress': 0.6}
> ```

```PY
def report_progress(typed, prompt, user_id, upload):
    """Upload a report of your id and progress so far to the multiplayer server.
    Returns the progress so far.
    Arguments:
        typed: a list of the words typed so far
        prompt: a list of the words in the typing prompt
        user_id: a number representing the id of the current user
        upload: a function used to upload progress to the multiplayer server
    >>> print_progress = lambda d: print('ID:', d['id'], 'Progress:', d['progress'])
    >>> # The above function displays progress in the format ID: __, Progress: __
    >>> print_progress({'id': 1, 'progress': 0.6})
    ID: 1 Progress: 0.6
    >>> typed = ['how', 'are', 'you']
    >>> prompt = ['how', 'are', 'you', 'doing', 'today']
    >>> report_progress(typed, prompt, 2, print_progress)
    ID: 2 Progress: 0.6
    0.6
    >>> report_progress(['how', 'aree'], prompt, 3, print_progress)
    ID: 3 Progress: 0.2
    0.2
    # 返回的是progress的
    """
    # BEGIN PROBLEM 8 
    "*** YOUR CODE HERE ***"# 复习:用大括号表示字典 {}
    # 再次复习一下zip和双赋值法
    correct_sofar = 0
    for t, p in zip(typed, prompt):
        if t == p:
            correct_sofar += 1
        else:
            break
    progress = correct_sofar / len(prompt)
    upload({'id':user_id, 'progress':progress})
    return progress    
    # END PROBLEM 8
```

**==再次复习一下zip和双赋值法!!!!!!!!!!!!!!!!!!!!!==**

## Problem 9 (2 pts)

Implement `time_per_word`, which takes in a list `words` and `times_per_player`, a list of lists for each player with **==timestamps时间戳==** indicating when each player finished typing every individual word in `words`. It returns a `match` with the given information.实现`time_per_word`，它接受一个列表`words`和`times_per-player`，这是每个玩家的列表，带有时间戳，指示每个玩家何时完成键入`words`中的每个单词。它返回与给定信息的`match`。

A `match` is a dictionary that stores `words` and `times`. The `times` are stored as a list of lists of how long it took each player to type every word in `words`. Specifically, `times[i][j]` indicates how long it took player `i` to type `words[j]`.

**`match`是一个存储`words`和`times`的字典。`times`被存储为每个玩家打完`words`中的每个单词所需时间的列表。具体来说，`times[i][j]`表示玩家`i`花了多长时间输入`word[j]`。**

For example, say `words = ['Hello', 'world']` and `times = [[5, 1], [4, 2]]`, then `[5, 1]` corresponds to the list of times for player 0, and `[4, 2]` corresponds to the list of times for player 1. Thus, player 0 took `5` units of time to write the word `'Hello'`.

> **Important**: **Be sure to use the `match` constructor when returning a `match`.** **The tests will check that you are using the `match` dictionary rather than assuming a particular data format.**
>
> Read the definitions for the `match` constructor in `cats.py` to learn more about how the dictionary is implemented.

```py
def time_per_word(words, times_per_player):
    """Given timing data, return a match dictionary, which contains a
    list of words and the amount of time each player took to type each word.
    Arguments:
        words: a list of words, in the order they are typed.
        times_per_player: A list of lists of timestamps including the time
                          the player started typing, followed by the time
                          the player finished typing each word.
    >>> p = [[75, 81, 84, 90, 92], [19, 29, 35, 36, 38]]
    >>> match = time_per_word(['collar', 'plush', 'blush', 'repute'], p)
    >>> match["words"]
    ['collar', 'plush', 'blush', 'repute']
    >>> match["times"]
    [[6, 3, 6, 2], [10, 6, 1, 2]]
    **`match`是一个存储`words`和`times`的字典。`times`被存储为每个玩家打完`words`中的每个单词所需时间的列表。
    具体来说，`times[i][j]`表示玩家`i`花了多长时间输入`word[j]`。**
    """
    "*** YOUR CODE HERE ***"
    # 关键,学会用 [list].append
    tpp_list = []
    for tpp in times_per_player:
        time = []
        for i in range(0, len(tpp) - 1):
            time.append(tpp[i + 1] - tpp[i])
        tpp_list.append(time)
    return tpp_list
```

## Problem 10 (2 pts)

> **Important**: Be sure to use the `match` constructor when returning a `match`. The tests will check that you are using the `match` dictionary rather than assuming a particular data format.

> Make sure your implementation does not mutate the given player input lists. For the example above, calling `fastest_words` on `[player_0, player_1]` should **not** mutate `player_0` or `player_1`.