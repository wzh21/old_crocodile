

前言：本指南假定您对计算机上的命令行有基本的了解。如果您从未使用过命令行，请考虑阅读[lab01]中的“terminal”部分

# A. Intro to Version Control Systems

***Version control systems* are tools to keep track of changes to files over time.** Version control allows you to view or revert(还原,恢复) back to previous iterations of files. **Some aspects of version control are actually built into commonly used applications.** *Think of the `undo` command or how you can see the revision history of a Google document.*

在编码的上下文中，版本控制系统可以跟踪代码修订的历史，从代码的当前状态一直到第一次跟踪。这允许用户引用他们工作的旧版本，并与其他人（如开发人员）共享代码更改。

版本控制已成为软件开发和行业协作的支柱。在本课程中，我们将使用[Git](http://git-scm.com/). Git有优秀的[文档](http://git-scm.com/documentation)因此，我们强烈鼓励那些有兴趣的人阅读更多关于本指南其余部分总结的内容。

## Intro to Git

==Git是一个[分布式版本控制系统](http://en.wikipedia.org/wiki/Distributed_revision_control) , 与集中式版本控制系统相反==。==**这意味着每个开发人员的计算机都存储整个项目的整个历史(包括所有旧版本)!**==这与Dropbox等工具截然不同，Dropbox将旧版本存储在其他人拥有的远程服务器上。我们将整个项目的整个历史称为“存储库”。**存储库存储在本地的事实导致我们能够在自己的计算机上本地使用Git，即使没有互联网连接。**

实验室计算机已经在命令行上安装了[Git指南](https://fa22.datastructur.es/materials/lab/lab01/index.md)解释了如何在自己的计算机上安装git。除了我们将在本指南中学习使用的基于文本的界面之外，还有一个[Git GUI（图形用户界面）](http://git-scm.com/downloads/guis). 我们不会正式支持图形GUI的使用。

# B. Local Repositories (Narrative Introduction 叙述性介绍)

让我们通过一个叙述性的例子来说明如何使用git。我们将在这个故事中使用许多不熟悉的术语和想法。有关此叙事示例的视频版本，请参见[此视频](https://www.youtube.com/watch?v=KoPxeLqWRTY).

假设我们想在计算机上存储各种各样的食谱(recipe)，并且想在改变食谱时跟踪这些食谱的历史。我们可以先为seitan(面筋)和tofu(豆腐)食谱创建目录，**然后使用sublime（在我的计算机上使用subl命令调用）**创建每个食谱。

> sublime是GUI text editor的一种

我们假设你只是在读这个，而不是自己尝试命令。如果您想继续键入所有内容，则需要使用安装在计算机上的文本编辑器，而不是`subl`。

```bash
$ cd /users/sandra 			;;;sandra是一个女名
$ mkdir recipes
$ cd recipes
$ mkdir seitan
$ mkdir tofu
$ cd seitan
$ subl smoky_carrot_tahini_seitan_slaw.txt
$ subl boiled_seitan.txt
$ cd ../tofu
$ subl kung_pao_tofu.txt
$ subl basil_ginger_tofu.txt
```

Now we have four recipes, two for tofu, and two for seitan.要设置git存储库以存储配方演变过程中的历史记录，我们将使用以下命令：

```bash
$ cd /users/sandra/recipes
$ git init
```

**==`git init`所做的是告诉git版本控制系统，我们希望跟踪当前目录的历史记录==，在本例中是“/users/sandra/recipes”。** **然而，此时*存储库中没有存储任何内容*。就像我们买了一个保险箱，但我们还没有放任何东西**

**要将所有内容存储在存储库中，我们需要首先add files(添加文件)**。例如，我们可以执行以下操作：

```bash
git add ./tofu/kung_pao_tofu.txt
```

Now here’s where git is going to start seeming weird. Even after calling the add command, we *still* haven’t stored our recipe in the repository (i.e. in the safe).现在，git开始变得奇怪了。即使在调用add命令之后，我们仍然没有将我们的配方存储在存储库中（即保险箱中）。

相反，我们所做的是将`kung_pao_tofu.txt`添加到要跟踪的文件列表中（即稍后添加到保险箱中）。**==其想法是，您可能不想跟踪/users/sandra/recipes文件夹中的每个文件，因此add命令告诉git应该跟踪哪些文件。==**==通过使用git status命令，我们可以看到该命令的效果。==

```bash
$ git status
```

在这种情况下，您将看到以下响应：

```bash
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   tofu/kung_pao_tofu.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    seitan/
    tofu/basil_ginger_tofu.txt
```

The “**changes to be committed**” portion of the output **lists all files that are currently being tracked and whose changes are *ready* be committed** (i.e. that are ready to be put in the safe). We also see that there are some untracked files, namely the seitan folder, and the `tofu/basil_ginger_tofu.txt` file. These are untracked because we have not added them using `git add`.

Let’s try adding `tofu/basil_ginger_tofu.txt`, and check the status once more:

```bash
$ git add ./tofu/basil_ginger_tofu.txt
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   tofu/basil_ginger_tofu.txt
    new file:   tofu/kung_pao_tofu.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    seitan/
```

我们看到两种豆腐配方都被跟踪，但两种生煎配方都没有被跟踪。==接下来，我们将使用`commit`命令将豆腐食谱的副本粘贴到存储库==（即保险箱）中。为此，我们使用git commit命令，如下所示：

```bash
$ git commit -m "add tofu recipes"
```

执行commit命令时，commit命令会将所有添加文件（即当前豆腐食谱）的快照存储到存储库中。因为我们没有在seitan食谱上使用“git add”，所以它们没有包含在存储库中的快照中。这张我们工作的快照现在是永远安全的（只要我们的计算机硬盘没有故障或者我们没有损坏秘密存储库文件）。==-m命令允许我们向提交添加一条消息，这样我们就可以记住这次提交最重要的内容。**使用[命令式](https://en.wikipedia.org/wiki/Imperative_mood)是常见的惯例而不是过去式。So for example, the commit message above says “add tofu recipes” rather than “added tofu recipes”.**==

作为另一个类比，你可以把整个过程想象成在相机上拍摄全景照片。add命令捕获图像的一部分，commit命令将所有“添加”的项目缝合到一个全景中，并将该全景放入保险箱。**正如全景图只包含你指向的东西（而不是你周围的整个360度圆圈）一样**，**commit命令只包含那些使用add命令添加的文件（而不是recipes目录中的所有文件）。**

After using commit, you’ll note that `git status` no longer lists files under “changes to be committed.” This is similar to how after you finish taking a panoramic photo, all of the temporary tiny image files are thrown away. The result of `git status` at this point is shown below:	**==使用commit后，您会注意到“git-status”不再在“要提交的更改”下列出文件==。这类似于完成全景照片后，所有临时的小图像文件都会被丢弃。此时“gitstatus”的结果如下所示：**

```bash
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    seitan/

nothing added to commit but untracked files present (use "git add" to track)
```

**==如果您查看tofu文件夹中的文件，您会发现提交过程不会影响计算机上的原始文件。==**这很像当你为你的朋友拍摄全景照片时，他们不会被你手机里的网络地狱所吸引。

We can see evidence of our snapshot by using the special `git log` command.

```bash
$ git log

commit 9f955d85359fc8e4504d7220f13fad34f8f2c62b
Author: Sandra Upson <sandra@Sandras-MacBook-Air.local>
Date:   Sun Jan 17 19:06:48 2016 -0800

    added tofu recipes
```

That giant string of characters 9f955d85359fc8e4504d7220f13fad34f8f2c62b **is the ID of the commit**. We can use ==the `git show` command==to peek inside of this commit(查看commit的内部信息).

```bash
$ git show 9f955d85359fc8e4504d7220f13fad34f8f2c62b

commit 9f955d85359fc8e4504d7220f13fad34f8f2c62b
Author: Sandra Upson <sandra@Sandras-MacBook-Air.local>
Date:   Sun Jan 17 19:06:48 2016 -0800

    add tofu recipes

diff --git a/tofu/basil_ginger_tofu.txt b/tofu/basil_ginger_tofu.txt
new file mode 100644
index 0000000..9a56e7a
--- /dev/null
+++ b/tofu/basil_ginger_tofu.txt
@@ -0,0 +1,3 @@
+basil
+ginger
+tofu
diff --git a/tofu/kung_pao_tofu.txt b/tofu/kung_pao_tofu.txt
new file mode 100644 index
0000000..dad9bd9
--- /dev/null
+++ b/tofu/kung_pao_tofu.txt
@@ -0,0 +1,3 @@
+szechuan peppers
+tofu
+peanuts
+kung
+pao
```

The `git show` command lets us peer(端详,仔细看) right into the beating heart of a commit. We don’t expect all of its innards(内部结构) to make sense to you, but you can maybe glean that the **commit is a snapshot of both the names and contents of the files.** 

> 注意：在现实生活中或61B中，您很少使用`gitshow`命令，但在这里，它对于窥探commit内部以更好地了解它们是什么非常有用。

Suppose we now want to revise(修改) our kung pao(之前已经有了) recipe, because we decided it should have *bok choy* in it.

```bash
$ subl ./tofu/kung_pao_tofu.txt
```

**The changes** we just made to `kung_pao_tofu.txt` are **not saved** in the repository. In fact, if we do git status again, we’ll get:

```bash
$ git status

On branch master
Changes not staged for commit: ;;;未暂存用于提交的更改：
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   tofu/kung_pao_tofu.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    seitan/
```

> ==**stage : 暂存**==

You might think to yourself “OK, we’ll I’ll just do commit again”. However, **if we try to commit, git will say that there’s nothing to do:**

```bash
$ git commit -m "add bok choy"

On branch master
Changes not staged for commit:
    modified:   tofu/kung_pao_tofu.txt

Untracked files:
    seitan/

no changes added to commit
```

This is because even though `kung_pao_tofu.txt` is being tracked, **we have not staged(暂存) our changes for commit**. To store our changes in the repository, **we first need to use the add command again**, which will stage the changes for commit (or in other words, we need to **take a picture of our new** `kung_pao_tofu.txt` **before we can create the new** panorama(全景) that we want to put in the safe).

```bash
$ git add ./tofu/kung_pao_tofu.txt
$ git status

On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:   tofu/kung_pao_tofu.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    seitan/
```

**We see that our change to `kung_pao_tofu.txt` is now “to be committed”, meaning that the next commit will include changes to this file.** We commit just like before, and use git log to see the list of all snapshots that have been taken.

```bash
$ git commit -m "add bok choy"
$ git log

commit cfcc8cbd88a763712dec2d6bd541b2783fa1f23b
Author: Sandra Upson <sandra@Sandras-MacBook-Air.local>
Date:   Sun Jan 17 19:24:45 2016 -0800

    added bok choy

commit 9f955d85359fc8e4504d7220f13fad34f8f2c62b
Author: Sandra Upson <sandra@Sandras-MacBook-Air.local>
Date:   Sun Jan 17 19:06:48 2016 -0800

    added tofu recipes
```

**We now see that there are ==TWO commits==**. We could again use show to see what changed in cfcc8cbd88a763712dec2d6bd541b2783fa1f23b, but we won’t in this guide.

Suppose we later decide bok choy is gross(令人厌恶的). We can ==**roll back our files using the checkout command**==, as shown below: 假设我们稍后决定bok-choy是令人厌恶的。我们可以使用checkout命令回滚文件，如下所示：

```bash
$ git checkout 9f955d85359fc8e4504d7220f13fad34f8f2c62b ./recipes/tofu
```

Think of the checkout command as a robot that goes to our safe, **figures out what the tofu recipe looked like back when** the newest panorama was 9f955d85359fc8e4504d7220f13fad34f8f2c62b, and finally rearranges everything in the actual recipes/tofu folder so that it is exactly like it was at the time snapshot 9f955d85359fc8e4504d7220f13fad34f8f2c62b was created. If we now look at the contents of `recipes/tofu/kung_pao_tofu.txt` after running this command, we’ll see that bok choy is gone (phew)!

```bash
szechuan
peppers
tofu
peanuts
kung
pao
```

*Important: ==**The checkout command does not change the commit history!**==* Or in other words, the safe containing our panoramic photos is entirely **unaffected by the checkout command.** **The entire point of git is to create a log of everything that has EVER happened to our files**. In other words, if you took a panoramic photo of your room in 2014 and in 2015 and put them in a safe, then decided in 2016 to put it exactly back like it was in 2014, you would not set the panoramic photo from the year 2015 on fire. Nor would you a picture of it in 2016 magically appear inside your safe. **If you wanted to record what it looked like in 2016, you’d need to take another photo (with the appropriate -m message to remember what you just did).**

> 总结上面一段 : 只有当你add到历史记录(GIT)中时,这个时间段的快照才会被记录, 这和你当前的状态(处于什么时期)没有任何关系

*Also important:* Make sure to specify a file (or directory) when you use checkout. Otherwise, you’re using a more powerful version of checkout that will probably confuse you. If that should happen, see the [61B git-WTFS](https://fa22.datastructur.es/materials/guides/git/wtfs/index.md) (git weird technical failure scenarios).*同样重要的是：*确保在使用checkout时指定文件（或目录）。否则，您使用的是更强大的checkout版本，这可能会让您感到困惑。如果发生这种情况，请参阅[61B git WTFS](https://fa22.datastructur.es/materials/guides/git/wtfs/index.md)（git奇怪的技术故障场景）。

> 也就是,==要指定恢复这一时间点的哪几个文件 , 如果不指明 , 那会整体都回到这一时间点==

If we want to actually commit a snapshot of the newest kung pao tofu (which no longer has bok choy), we’d have to commit:

```bash
$ git commit -m "revert back to the original recipe with no bok choy"
$ git log

commit 4be06747886d0a270bf1d618d58f3273f0219a69
Author: Sandra Upson <sandra@Sandras-MacBook-Air.local>
Date:   Sun Jan 17 19:32:37 2016 -0800

    revert back to the original recipe with no bok choy

commit cfcc8cbd88a763712dec2d6bd541b2783fa1f23b
Author: Sandra Upson <sandra@Sandras-MacBook-Air.local>
Date:   Sun Jan 17 19:24:45 2016 -0800

    add bok choy

commit 9f955d85359fc8e4504d7220f13fad34f8f2c62b
Author: Sandra Upson <sandra@Sandras-MacBook-Air.local>
Date:   Sun Jan 17 19:06:48 2016 -0800

    add tofu recipes
```

We could then use show to see the contents of this most recent commit.

```bash
$ git show 4be06747886d0a270bf1d618d58f3273f0219a69

commit 4be06747886d0a270bf1d618d58f3273f0219a69
Author: Sandra Upson <sandra@Sandras-MacBook-Air.local>
Date:   Sun Jan 17 19:32:37 2016 -0800

    revert back to the original recipe with no bok choy

diff --git a/tofu/kung_pao_tofu.txt b/tofu/kung_pao_tofu.txt
index 35a9e71..dad9bd9 100644
--- a/tofu/kung_pao_tofu.txt
+++ b/tofu/kung_pao_tofu.txt
@@ -1,4 +1,3 @@
szechuan
peppers
tofu
peanuts
kung
pao
-bok choy
\ No newline at end of file
```

不太重要的注意事项：非常细心的读者可能注意到，在我承诺删除白菜之前，我没有使用“git add”。这是因为一个有趣的事实，**==即checkout实际上也会对由于回滚而更改的任何文件进行自动“git添加”。==**

This is the foundation of git. To summarize, using our photo analogy:

- `git init`: 创建一个永久存储全景图片的框。
- `git add`: 为一件事拍摄临时照片，以后可以将其合成全景照片。
- `git commit`: 将所有可用的临时照片组合成全景照片。同时销毁所有临时照片。
- `git log`: 列出我们拍摄过的所有**==全景照片(不包括临时)==**。
- `git show`: 查看特定全景照片中的内容。
- `git checkout`: Rearranges files back to how they looked in a given panoramic photo. Does not affect the panormiac photos in your box in any way.

关于git还有更多需要学习的地方，但在我们到达之前，让我们对我们刚刚做的事情做一个更正式的解释。

# C. Local Repositories (Technical Overview)

## Initializing Local Repositories

让我们首先从*本地存储库*开始。如上所述，存储库存储文件以及这些文件的更改历史。为了开始，您必须在终端*中键入以下命令来初始化Git存储库，同时要将其历史记录存储在本地存储库*中。如果您使用的是Windows，那么在键入这些命令时应该使用GitBash终端窗口。提醒：如果您不确定如何使用终端窗口，请考虑查看[实验室1设置](https://fa22.datastructur.es/materials/lab/lab01/)的“学习使用终端”部分

```bash
$ git init
```

Extra for experts：当您初始化Git存储库时，Git会创建一个`.Git`子目录。在这个目录中，它将存储一堆元数据，以及文件的实际快照。然而，==您永远不需要实际打开这个.git目录的内容，而且您应该明确地不直接更改内部的任何内容！==

Depending on your operating system, you may not see the folder, because folders whose names start with “.” are not shown by your OS by defaut. The UNIX command `ls -la` will list all directories, including your `.git` directory, so you can use this command to check that your repo has been initialized properly.	根据您的操作系统，您可能看不到文件夹，因为默认情况下，操作系统不会显示名称以“.”开头的文件夹。UNIX命令“ls-la”将列出所有目录，包括“.git”目录，因此您可以使用此命令检查repo是否已正确初始化。

## Tracked vs. Untracked Files

Git repos **start off not tracking any files.** In order to save the revision history of a file, you need to track it. The Git documentation has an excellent section on [recording changes](http://git-scm.com/book/en/Git-Basics-Recording-Changes-to-the-Repository). An image from that section is placed here for your convenience:

![File Status Lifecyle](https://fa22.datastructur.es/materials/guides/img/file-status.png)

如图所示，文件分为两大类：

1. *untracked* files: These files have either never been tracked or were removed from tracking. Git is not maintaining history for these files.这些文件要么从未被跟踪，要么已从跟踪中删除。Git没有维护这些文件的历史记录。
2. *tracked* files: These files have been added to the Git repository and can be in various stages of modification: unmodified, modified, or staged.这些文件已添加到Git存储库中，**可以处于不同的修改阶段：未修改、修改或暂存**。
   1. An *unmodified* file is one that has had no new changes since the last version of the files was added to the Git repo.一个*未修改的*文件是自上一个版本的文件添加到Git repo以来没有任何新更改的文件。
   2. A *modified* file is one that is different from the last one Git has saved.*修改的*文件与Git保存的上一个文件不同。
   3. A *staged* file is one that a user has designated as part of a future commit (usually through use of the `git add` command). We can think of these as files which have lights shining upon them.*暂存*文件是用户在将来提交时指定的文件（通常通过使用“git add”命令）。**我们可以将这些文件视为有灯光照射的文件。**

The following Git command allows you see the status of each file, i.e. whether it is untracked, unmodified, modified, or stageds:以下Git命令允许您查看每个文件的状态，即是否未跟踪、未修改、已修改或已暂存：

```bash
$ git status
```

==The `git status` command== is extremely useful for determining the exact status of each file in your repository. If you are confused about what has changed and what needs to be committed, it can remind you of what to do next.

## Staging & Committing

**A *commit* is a specific snapshot of your working directory at a particular time**. Users must specify what exactly composes the snapshot **by *staging* files**.

**The `add` command lets you stage a file** (called `FILE` in the example below).

```bash
$ git add FILE
```

Once you have staged all the files you would like to include in your snapshot, you can commit them as one block with a message.一旦暂存了所有要包含在快照中的文件，就可以将它们作为一个带有message的块提交。

```bash
$ git commit -m MESSAGE
```

Your message should be descriptive and explain what changes your commit makes to your code. You may want to quickly describe bug fixes, implemented classes, etc. so that your messages are helpful later when looking through your commit log.您的消息应该是描述性的，并解释您的提交对代码所做的更改。您可能需要快速描述bug修复、实现的类等，以便稍后查看提交日志时，您的消息会有所帮助。

In order to see previous commits, you can use the log command:为了查看以前的提交，可以使用log命令：

```bash
$ git log
```

The Git reference guide has a helpful section on [viewing commit history](http://git-scm.com/book/en/Git-Basics-Viewing-the-Commit-History) and filtering log results when searching for particular commits. It might also be worth checking out `gitk`, which is a GUI prompted by the command line.		Git参考指南有一个关于[查看提交历史](http://git-scm.com/book/en/Git-Basics-Viewing-the-Commit-History)的有用部分以及在搜索特定提交时过滤日志结果。它可能还值得查看`gitk`，这是一个由命令行提示的GUI。

As a side note on development workflow, it is a good idea to commit your code as often as possible. Whenever you make significant (or even minor) changes to your code, make a commit. If you are trying something out that you might not stick with, commit it (perhaps to a different branch, which will be explained below).	作为开发工作流的补充说明，==**尽可能频繁地提交代码是一个好主意。每当您对代码进行重大（甚至微小）更改时，请提交 . 如果你正在尝试一些你可能坚持不下去的东西，那就把它提交给另一个分支，这将在下面解释）**==。

**Rule of Thumb**: If you commit, you can always revert your code or change it. However, if you don’t commit, you won’t be able to get old versions back. So commit often!**经验法则**：如果你提交了，你总是可以还原代码或更改代码。但是，如果你不提交，你将无法恢复旧版本。==**所以要经常commit!!!!!!!**==

## Undoing Changes

The Git reference has a great section on [undoing things](http://git-scm.com/book/en/Git-Basics-Undoing-Things). Please note that while Git revolves around the concept of history, it is possible to lose your work if you revert with some of these undo commands. Thus, be careful and read about the effects of your changes before undoing your work.	Git参考文献中有一个关于[撤销](http://git-scm.com/book/en/Git-Basics-Undoing-Things)的章节. 请注意，虽然Git围绕着历史的概念，但如果您使用这些撤销命令进行恢复，则可能会丢失您的工作。因此，在撤消工作之前，请仔细阅读更改的影响。

- **==Unstage a file that you haven’t yet committed:==**

  ```bash
  $ git reset HEAD [file]
  ```

  This will take the file’s status back to modified, leaving changes intact. Don’t worry about this command undoing any work. This command is the equivalent of deleting one of the temporary images that you’re going to combine into a panorama.这将使文件的状态恢复为“已修改”，保留更改不变。**不要担心这个命令会取消任何工作**。此命令相当于删除要合并为全景的临时图像之一。

  Why might we need to use this command? Let’s say you accidentally started tracking a file that you didn’t want to track. (an embarrassing video of yourself, for instance.) Or you were made some changes to a file that you thought you would commit but no longer want to commit quite yet.为什么我们需要使用此命令？假设你不小心开始跟踪一个你不想跟踪的文件。（例如，你自己的一段尴尬视频。）或者你对一个文件做了一些更改，**你以为你会提交，但现在还不想提交。**

- ==**Amend latest commit (changing commit message or add forgotten files):修改最新提交(更改提交消息或添加忘记的文件):**==

  ```bash
  $ git add [forgotten-file]
  $ git commit --amend
  ```

  ==**Please note that this new amended commit will *replace* the previous commit.**==

- ==**Revert a file to its state at the time of the most recent commit: 将文件恢复到最近提交时的状态：**==

  ```bash
  $ git checkout -- [file]
  ```

  This next command is useful if you would like to actually undo your work. Let’s say that you have modified a certain `file` since committing previously, but you would like your file back to how it was before your modifications.如果您想实际撤消工作，下一个命令很有用。假设您在之前提交后修改了某个“文件”，但您希望文件恢复到修改前的状态。

  **Note**: This command is potentially quite dangerous because any changes you made to the file since your last commit will be removed. Use this with caution! **注意**:此命令可能非常危险,因为自上次提交后对文件所做的任何更改都将被删除.小心使用！

## Getting previous versions of files

Suppose you’re working on a lab, and halfway through, you realize you’ve been doing it all wrong. Oh no! If only there were a way to get back the **skeleton** code so you can start over!

If you haven’t committed yet, you can checkout the files from the latest commit:如果尚未提交，则可以从最新提交中签出文件：

```bash
git checkout -- lab1000/
```

Which will revert all the files in the `lab1000/` folder. Note that you’ll lose any progress you’ve made since the latest commit. If you only want to revert a few files, you can use `git checkout -- [file]` for each file you want to revert.这将还原“lab1000/”文件夹中的所有文件。请注意，您将失去自最近一次提交以来所取得的任何进展。如果只想还原几个文件，可以对每个要还原的文件使用`gitcheckout--[file]`。

But what if you’ve already committed some changes? Rather than checking out the files in the latest commit, you can use the **more powerful form** of `git checkout`:

```bash
git checkout [commit or branch] -- [file or folder]
```

which can get files from an arbitrary point in time. For example, let’s say you’ve accidentally deleted `lab1000/`, and committed that change. Oh no! To fix this, you can get back the skeleton code with 

```bash
git checkout skeleton/main -- lab1000/
```

Which will allow you to start over on `lab1000`.

However, let’s say you made some progress on `lab1000/Cheese.txt`, and want to get back the version of that file from 3 commits ago. **==You can find the correct commit with `git log`, and then run==**

```bash
git checkout abcd1234efgh7890abcd1234c7ee5e7890c7ee5e -- lab1000/Cheese.txt
```

where `abcd1234efgh7890abcd1234c7ee5e7890c7ee5e` is the commit hash from `git log`.

**If you’re working on lab 1, it’s time to return and do the git exercise.**

> 以上为Lab1的预学内容

# D. Remote Repositories

One especially handy feature of git is the ability to store copies of your repository on computers other than your own. Recall that our snapshots are all stored on our computer in a secret folder. That means if our computer is damaged or destroyed, so are all our snapshots.	git的一个特别方便的功能是能够将存储库的副本存储在自己的计算机上。回想一下，我们的快照都存储在计算机上的一个秘密文件夹中。这意味着，如果我们的计算机被损坏或毁坏，我们所有的快照也是如此。

假设我们想将豆腐和生煎食谱推送到另一台计算机上，我们通常会使用以下命令。

```bash
$ git push origin master
```

然而，如果我们尝试了这一点，我们只会得到以下信息：

```bash
fatal: 'origin' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights and the repository exists.
```

This is because we have not yet told git where to send the files. As it happens, there is a for-profit private company called GitHub that has made a business out of storing copies of people’s repositories.这是因为我们还没有告诉git将文件发送到哪里。事实上，有一家名为GitHub的营利性私营公司以存储人们的存储库副本为生。

在61B中，我们将使用GitHub存储存储库。要在GitHub上创建存储库，您可能需要使用他们的web界面。然而，我们已经为您做了这件事。

下面列出了最重要的远程存储库命令，以及可能还没有意义的技术描述。如果你正在研究实验室1，回到实验室了解更多关于这些命令的信息。

- `git clone [remote-repo-URL]`: Makes a copy of the specified repository, but on your local computer. Also creates a working directory that has files arranged exactly like the most recent snapshot in the download repository. Also records the URL of the remote repository for subsequent network data transfers, and gives it the special remote-repo-name “origin”.**在本地计算机上创建指定存储库的副本。**还将创建一个工作目录，其中的文件排列方式与下载存储库中的最新快照完全相同。还记录远程存储库的URL，用于后续网络数据传输，**并为其提供特殊的远程存储库名称“origin”。**
- `git remote add [remote-repo-name] [remote-repo-URL]`: Records a new location for network data transfers.记录网络数据传输的新位置。
- `git remote -v`: Lists all locations for network data transfers.列出网络数据传输的所有位置。
- `git pull [remote-repo-name] master`: Get the most recent copy of the files as seen in remote-repo-name获取远程存储库名称中显示的文件的最新副本
- `git push [remote-repo-name] master`: Pushes the most recent copy of your files to the remote-repo-name.将文件的最新副本推送到远程存储库名称。





![image-20230119145843829](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230119145843829.png)



# 廖雪峰的Git教程

[廖雪峰的Git教程](https://www.liaoxuefeng.com/wiki/896043488029600/896202780297248)

## 创建版本库

### 疑难解答

Q：输入`git add readme.txt`，得到错误：`fatal: not a git repository (or any of the parent directories)`。

A：Git命令必须在Git仓库目录内执行（`git init`除外），在仓库目录外执行是没有意义的。

Q：输入`git add readme.txt`，得到错误`fatal: pathspec 'readme.txt' did not match any files`。

A：添加某个文件时，该文件必须在当前目录下存在，用`ls`或者`dir`命令查看当前目录的文件，看看文件是否存在，或者是否写错了文件名。

### 小结

现在总结一下今天学的两点内容：

初始化一个Git仓库，使用`git init`命令。

添加文件到Git仓库，分两步：

1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
2. 使用命令`git commit -m <message>`，完成。

## 时光机穿梭

### 小结

- 要随时掌握工作区的状态，使用`git status`命令。
- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

### 版本回退

好了，现在我们启动时光穿梭机，准备把`readme.txt`回退到上一个版本，也就是`add distributed`的那个版本，怎么做呢？

首先，Git必须知道当前版本是哪个版本，==**在Git中，用`HEAD`表示当前版本**==，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），==**上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`**==。

现在，==我们要把当前版本`append GPL`回退到上一个版本`add distributed`**，就可以使用`git reset`命令**：==

```bash
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

**`--hard`参数有啥意义**？这个后面再讲，现在你先放心使用。

看看`readme.txt`的内容是不是版本`add distributed`：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
```

果然被还原了。

还可以继续回退到上一个版本`wrote a readme file`，不过且慢，让我们用`git log`再看看现在版本库的状态：

```bash
$ git log
commit e475afc93c209a690c39c13a46716e8fa000c366 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

最新的那个版本`append GPL`已经看不到了！好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了，肿么办？

办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`append GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本：

```bash
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```

**版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。**

再小心翼翼地看看`readme.txt`的内容：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

果然，我胡汉三又回来了。

**==Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从指向`append GPL`：==**

```ascii
┌────┐
│HEAD│
└────┘
   │
   └──▶ ○ append GPL
        │
        ○ add distributed
        │
        ○ wrote a readme file
```

改为指向`add distributed`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   │    ○ append GPL
   │    │
   └──▶ ○ add distributed
        │
        ○ wrote a readme file
```

**然后顺便把工作区的文件更新了**。所以你让`HEAD`指向哪个版本号，你就把当前版本定位在哪。

现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？

在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的commit id。==Git提供了一个命令`git reflog`用来记录你的每一次命令==：

```bash
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

终于舒了口气，从输出可知，`append GPL`的==commit id==是`1094adb`，现在，你又可以乘坐时光机回到未来了。

#### 小结

现在总结一下：

- ==`HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。==
- ==穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。==
- ==要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。==

Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。



### 工作区与暂存区

#### 工作区（Working Directory）

就是你在电脑里能看到的目录，比如我的`learngit`文件夹就是一个工作区：

![working-dir](https://www.liaoxuefeng.com/files/attachments/919021113952544/0)

#### 版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，==其中最重要的就是**称为stage（或者叫index）的暂存区**，还有Git为我们自动创建的**第一个分支`master`**，以及**指向`master`的一个指针叫`HEAD`**。==

![git-repo](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

分支和`HEAD`的概念我们以后再讲。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，**需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。**

俗话说，实践出真知。现在，我们再练习一遍，先对`readme.txt`做个修改，比如加上一行内容：

```bash
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
```

然后，在工作区新增一个`LICENSE`文本文件（内容随便写）。

先用`git status`查看一下状态：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	LICENSE

no changes added to commit (use "git add" and/or "git commit -a")
```

Git非常清楚地告诉我们，`readme.txt`被修改了，而`LICENSE`还从来没有被添加过，所以它的状态是`Untracked`。

现在，使用两次命令`git add`，把`readme.txt`和`LICENSE`都添加后，用`git status`再查看一下：

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   LICENSE
	modified:   readme.txt
```

现在，暂存区的状态就变成这样了：

![git-stage](https://www.liaoxuefeng.com/files/attachments/919020074026336/0)

所以，`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

```bash
$ git commit -m "understand how stage works"
[master e43a48b] understand how stage works
 2 files changed, 2 insertions(+)
 create mode 100644 LICENSE
```

==**一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：**==

```bash
$ git status
On branch master
nothing to commit, working tree clean
```

现在版本库变成了这样，暂存区就没有任何内容了：

![git-stage-after-commit](https://www.liaoxuefeng.com/files/attachments/919020100829536/0)

#### 小结

暂存区是Git非常重要的概念，弄明白了暂存区，就弄明白了Git的很多操作到底干了什么。

没弄明白暂存区是怎么回事的童鞋，请向上滚动页面，再看一次。



### 管理修改

现在，假定你已经完全掌握了暂存区的概念。下面，我们要讨论的就是，==**为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。**==

你会问，什么是修改？比如你新增了一行，这就是一个修改，删除了一行，也是一个修改，更改了某些字符，也是一个修改，删了一些又加了一些，也是一个修改，甚至创建一个新文件，也算一个修改。

为什么说Git管理的是修改，而不是文件呢？我们还是做实验。第一步，对readme.txt做一个修改，比如加一行内容：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
```

> concatenate : 拼接 -> cat

然后，添加：

```bash
$ git add readme.txt
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   readme.txt
#
```

然后，再修改readme.txt：

```bash
$ cat readme.txt 
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```

提交：

```bash
$ git commit -m "git tracks changes"
[master 519219b] git tracks changes
 1 file changed, 1 insertion(+)
```

提交后，再看看状态：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

咦，怎么第二次的修改没有被提交？

别激动，我们回顾一下操作过程：

第一次修改 -> `git add` -> 第二次修改 -> `git commit`

==**你看，我们前面讲了，Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。**==

> **==非常重要!!!!  第一次的add不会被提交(并没有被放入暂存区)==**

提交后，==用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别==：

```bash
$ git diff HEAD -- readme.txt 
diff --git a/readme.txt b/readme.txt
index 76d770f..a9c5755 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,4 +1,4 @@
 Git is a distributed version control system.
 Git is free software distributed under the GPL.
 Git has a mutable index called stage.
-Git tracks changes.
+Git tracks changes of files.
```

可见，第二次修改确实没有被提交。



那怎么提交第二次修改呢？**你可以继续`git add`再`git commit`，==也可以别着急提交第一次修改，先`git add`第二次修改，再`git commit`，就相当于把两次修改合并后一块提交了：==**

第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

好，现在，把第二次修改提交了，然后开始小结。

#### 小结

现在，你又理解了Git是如何跟踪修改的，每次修改，==**如果不用`git add`到暂存区，那就不会加入到`commit`中**==。



### 撤销修改

自然，你是不会犯错的。不过现在是凌晨两点，你正在赶一份工作报告，你在`readme.txt`中添加了一行：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.
```

在你准备提交前，一杯咖啡起了作用，你猛然发现了`stupid boss`可能会让你丢掉这个月的奖金！

既然错误发现得很及时，就可以很容易地纠正它。你可以删掉最后一行，手动把文件恢复到上一个版本的状态。如果用`git status`查看一下：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

你可以发现，Git会告诉你，==**`git checkout -- file`可以丢弃工作区的修改**==：

```bash
$ git checkout -- readme.txt
```

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

==**一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；**==

==**一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。**==

总之，==**就是让这个文件回到最近一次`git commit`或`git add`时的状态!!!!!!!!!!!!!!!!**==

现在，看看`readme.txt`的文件内容：

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```

文件内容果然复原了。

**==`git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。==**



现在假定是凌晨3点，你不但写了一些胡话，**还`git add`到暂存区了：**

```bash
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.

$ git add readme.txt
```

庆幸的是，在`commit`之前，你发现了这个问题。用`git status`查看一下，修改只是添加到了暂存区，还没有提交：

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.txt
```

Git同样告诉我们，用命令==**`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区**==：

```bash
$ git reset HEAD readme.txt
Unstaged changes after reset:
M	readme.txt
```

==**`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。**==

再用`git status`查看一下，**现在暂存区是干净的，工作区有修改**：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
```

**还记得如何丢弃工作区的修改吗？**

```bash
$ git checkout -- readme.txt

$ git status
On branch master
nothing to commit, working tree clean
```

整个世界终于清静了！



现在，假设你不但改错了东西，还从暂存区提交到了版本库，怎么办呢？还记得[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节吗？可以回退到上一个版本。**不过，这是有条件的，就是你还没有把自己的本地版本库推送到远程。还记得Git是分布式版本控制系统吗？我们后面会讲到远程版本库，一旦你把`stupid boss`提交推送到远程版本库，你就真的惨了……**

#### 小结

又到了小结时间。

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：**当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。**

场景3：**已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库。**

### 删除文件

在Git中，**删除也是一个修改操作**，我们实战一下，先添加一个新文件`test.txt`到Git并且提交：

```bash
$ git add test.txt

$ git commit -m "add test.txt"
[master b84166e] add test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
```

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：

```bash
$ rm test.txt
```

这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

==现在你有两个选择，**一是确实要*从版本库中*删除该文件，那就用命令`git rm`删掉**，并且`git commit`：==

```bash
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

现在，文件就从版本库中被删除了。



> 小提示：先手动删除文件，然后使用git rm <file>和git add<file>效果是一样的。

==另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：==

```bash
$ git checkout -- test.txt
```

==**`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”**。==

==注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！==

#### 小结

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

## 远程仓库

### 简介

到目前为止，我们已经掌握了如何在Git仓库里对一个文件进行时光穿梭，你再也不用担心文件备份或者丢失的问题了。

可是有用过集中式版本控制系统SVN的童鞋会站出来说，这些功能在SVN里早就有了，没看出Git有什么特别的地方。

没错，如果只是在一个仓库里管理文件历史，Git和SVN真没啥区别。为了保证你现在所学的Git物超所值，将来绝对不会后悔，同时为了打击已经不幸学了SVN的童鞋，**本章开始介绍Git的杀手级功能之一（注意是之一，也就是后面还有之二，之三……):远程仓库。**

Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。怎么分布呢？最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。

你肯定会想，至少需要两台机器才能玩远程库不是？但是我只有一台电脑，怎么玩？

其实一台电脑上也是可以克隆多个版本库的，只要不在同一个目录下。不过，现实生活中是不会有人这么傻的在一台电脑上搞几个远程库玩，因为一台电脑上搞几个远程库完全没有意义，而且硬盘挂了会导致所有库都挂掉，所以我也不告诉你在一台电脑上怎么克隆多个仓库。

**实际情况往往是这样，找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交。**

**完全可以自己搭建一台运行Git的服务器**，不过现阶段，为了学Git先搭个服务器绝对是小题大作。**好在这个世界上有个叫[GitHub](https://github.com/)的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。**

在继续阅读后续内容前，请自行注册GitHub账号。**由于==你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的==**，所以，需要一点设置：

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key :

```bash
$ ssh-keygen -t rsa -C "youremail@example.com"
```

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230119183005078.png" alt="image-20230119183005078" style="zoom: 33%;" />

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

<img src="https://www.liaoxuefeng.com/files/attachments/919021379029408/0" alt="github-addkey-1"  />

点“Add Key”，你就应该看到已经添加的Key：

![github-addkey-2](https://www.liaoxuefeng.com/files/attachments/919021395420160/0)

为什么GitHub需要SSH Key呢？**因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。**

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。

如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。

确保你拥有一个GitHub账号后，我们就即将开始远程仓库的学习。

#### 小结

“有了远程仓库，妈妈再也不用担心我的硬盘了。”——Git点读机

### 添加远程库

现在的情景是，**你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。**

首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：

![github-create-repo-1](https://www.liaoxuefeng.com/files/attachments/919021631860000/0)

在Repository name填入`learngit`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：

![github-create-repo-2](https://www.liaoxuefeng.com/files/attachments/919021652277920/0)

目前，在GitHub上的这个`learngit`仓库还是空的，GitHub告诉我们，**可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联**，然后，把本地仓库的内容推送到GitHub仓库。

现在，我们根据GitHub的提示，在本地的`learngit`仓库下运行命令：

```bash
$ git remote add origin git@github.com:michaelliao/learngit.git
```

请千万注意，**把上面的`michaelliao`替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库**，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，**但是`origin`这个名字一看就知道是远程库。**

下一步，就可以把本地库的所有内容推送到远程库上：

```bash
$ git push -u origin master
Counting objects: 20, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
Total 20 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To github.com:michaelliao/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

**由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。**



推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

![github-repo](https://www.liaoxuefeng.com/files/attachments/919021675995552/0)

从现在起，只要本地作了提交，就可以通过命令：

```bash
$ git push origin master
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

### SSH警告

当你第一次使用Git的`clone`或者`push`命令连接GitHub时，会得到一个警告：

```bash
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```

这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入`yes`回车即可。

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：

```bash
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```

这个警告只会出现一次，后面的操作就不会有任何警告了。

如果你实在担心有人冒充GitHub服务器，输入`yes`前可以对照[GitHub的RSA Key的指纹信息](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/)是否与SSH连接给出的一致。

### 删除远程库

如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息：

```bash
$ git remote -v
origin  git@github.com:michaelliao/learn-git.git (fetch)
origin  git@github.com:michaelliao/learn-git.git (push)
```

然后，根据名字删除，比如删除`origin`：

```bash
$ git remote rm origin
```

**此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库**。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

#### 小结

==要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；==

关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；

**关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；**

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！



### 从远程库克隆

上次我们讲了先有本地库，后有远程库的时候，如何关联远程库。

**现在，假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。**

首先，登陆GitHub，创建一个新的仓库，名字叫`gitskills`：

![github-init-repo](https://www.liaoxuefeng.com/files/attachments/919021808263616/0)

我们勾选`Initialize this repository with a README`，这样GitHub会自动为我们创建一个`README.md`文件。创建完毕后，可以看到`README.md`文件：

![github-init-repo-2](https://www.liaoxuefeng.com/files/attachments/919021836828288/0)

现在，远程库已经准备好了，下一步是用命令`git clone`克隆一个本地库：

```bash
$ git clone git@github.com:michaelliao/gitskills.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
Receiving objects: 100% (3/3), done.
```

注意把Git库的地址换成你自己的，然后进入`gitskills`目录看看，已经有`README.md`文件了：

```bash
$ cd gitskills
$ ls
README.md
```

如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

你也许还注意到，GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。实际上，Git支持多种协议，**默认的`git://`使用ssh，但也可以使用`https`等其他协议**。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。

#### 小结

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，**但`ssh`协议速度最快。**



> **==关于分支管理的问题以后再学吧, 太难了==**



## 使用GitHub

我们一直用GitHub作为免费的远程仓库，如果是个人的开源项目，放到GitHub上是完全没有问题的。其实GitHub还是一个开源协作社区，通过GitHub，既可以让别人参与你的开源项目，也可以参与别人的开源项目。

在GitHub出现以前，开源项目开源容易，但让广大人民群众参与进来比较困难，因为要参与，就要提交代码，而给每个想提交代码的群众都开一个账号那是不现实的，因此，群众也仅限于报个bug，即使能改掉bug，也只能把diff文件用邮件发过去，很不方便。

但是在GitHub上，利用Git极其强大的克隆和分支功能，广大人民群众真正可以第一次自由参与各种开源项目了。

如何参与一个开源项目呢？比如人气极高的bootstrap项目，这是一个非常强大的CSS框架，你可以访问它的项目主页https://github.com/twbs/bootstrap，点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone：

```bash
git clone git@github.com:michaelliao/bootstrap.git
```

一定要从自己的账号下clone仓库，这样你才能推送修改。如果从bootstrap的作者的仓库地址`git@github.com:twbs/bootstrap.git`克隆，因为没有权限，你将不能推送修改。

Bootstrap的官方仓库`twbs/bootstrap`、你在GitHub上克隆的仓库`my/bootstrap`，以及你自己克隆到本地电脑的仓库，他们的关系就像下图显示的那样：

```ascii
┌─ GitHub ────────────────────────────────────┐
│                                             │
│ ┌─────────────────┐     ┌─────────────────┐ │
│ │ twbs/bootstrap  │────>│  my/bootstrap   │ │
│ └─────────────────┘     └─────────────────┘ │
│                                  ▲          │
└──────────────────────────────────┼──────────┘
                                   ▼
                          ┌─────────────────┐
                          │ local/bootstrap │
                          └─────────────────┘
```

如果你想修复bootstrap的一个bug，或者新增一个功能，立刻就可以开始干活，干完后，往自己的仓库推送。

如果你希望bootstrap的官方库能接受你的修改，你就可以在GitHub上发起一个pull request。当然，对方是否接受你的pull request就不一定了。

如果你没能力修改bootstrap，但又想要试一把pull request，那就Fork一下我的仓库：https://github.com/michaelliao/learngit，创建一个`your-github-id.txt`的文本文件，写点自己学习Git的心得，然后推送一个pull request给我，我会视心情而定是否接受。

### 小结

- 在GitHub上，可以任意Fork开源仓库；
- 自己拥有Fork后的仓库的读写权限；
- 可以推送pull request给官方仓库来贡献代码。



## 分支管理

分支就是科幻电影里面的平行宇宙，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。

如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，你既学会了Git又学会了SVN！

![learn-branches](https://www.liaoxuefeng.com/files/attachments/919021987875136/0)

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。**==你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。==**

其他版本控制系统如SVN等都有分支管理，但是用过之后你会发现，这些版本控制系统创建和切换分支比蜗牛还慢，简直让人无法忍受，结果分支功能成了摆设，大家都不去用。

但Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。

### 创建与合并分支

在[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，==这个分支叫主分支，即`master`分支==。==`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的==，所以，`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

```ascii
                  HEAD
                    │
                    │
                    ▼
                 master
                    │
                    │
                    ▼
┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘
```

每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长。

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

```ascii
                 master
                    │
                    │
                    ▼
┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘
                    ▲
                    │
                    │
                   dev
                    ▲
                    │
                    │
                  HEAD
```

**你看，==Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！==**

**不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：**

```ascii
                 master
                    │
                    │
                    ▼
┌───┐    ┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘    └───┘
                             ▲
                             │
                             │
                            dev
                             ▲
                             │
                             │
                           HEAD
```

**假如我们在`dev`上的工作完成了，就==可以把`dev`合并到`master`上==。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：**

```ascii
                           HEAD
                             │
                             │
                             ▼
                          master
                             │
                             │
                             ▼
┌───┐    ┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘    └───┘
                             ▲
                             │
                             │
                            dev
```

**==所以Git合并分支也很快！就改改指针，工作区内容也不变！==**

**合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉**，删掉后，我们就剩下了一条`master`分支：

```ascii
                           HEAD
                             │
                             │
                             ▼
                          master
                             │
                             │
                             ▼
┌───┐    ┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘    └───┘
```

真是太神奇了，你看得出来有些提交是通过分支完成的吗？

下面开始实战。

首先，我们创建`dev`分支，然后切换到`dev`分支：

```bash
$ git checkout -b dev
Switched to a new branch 'dev'
```

==**`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令**==：

```bash
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

然后，==用`git branch`命令查看当前分支==：

```bash
$ git branch
* dev
  master
```

`git branch`命令会列出所有分支，**==当前分支前面会标一个`*`号==**。

然后，我们就可以在`dev`分支上正常提交，比如对`readme.txt`做个修改，加上一行：

```bash
Creating a new branch is quick.
```

然后提交：

```bash
$ git add readme.txt 
$ git commit -m "branch test"
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
```

现在，`dev`分支的工作完成，**我们就可以切换回`master`分支：**

```bash
$ git checkout master
Switched to branch 'master'
```

**切换回`master`分支后，再查看一个`readme.txt`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：**

![git-br-on-master](https://www.liaoxuefeng.com/files/attachments/919022533080576/0)

现在，我们把`dev`分支的工作成果合并到`master`分支上：

```
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

==**`git merge`命令用于合并指定分支到当前分支**==。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

**注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”**，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

当然，**也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。**

**合并完成后，就可以放心地==删除`dev`分支了：`git branch -d dev`==**

```
$ git branch -d dev
Deleted branch dev (was b17d20e).
```

删除后，查看`branch`，就只剩下`master`分支了：

```
$ git branch
* master
```

**==因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。==**

#### switch

**我们注意到切换分支使用`git checkout <branch>`，而前面讲过的撤销修改则是`git checkout -- <file>`，同一个命令，有两种作用，确实有点令人迷惑。**

==实际上，切换分支这个动作，用`switch`更科学。因此，**最新版本的Git提供了新的`git switch`命令来切换分支：**==

**==创建并切换到新的`dev`分支，可以使用：==**

```
$ git switch -c dev
```

直接切换到已有的`master`分支，可以使用：

```
$ git switch master
```

**使用新的`git switch`命令，比`git checkout`要更容易理解。**

#### 小结

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`



### 解决冲突

人生不如意之事十之八九，合并分支往往也不是一帆风顺的。

准备新的`feature1`分支，继续我们的新分支开发：

```bash
$ git switch -c feature1
Switched to a new branch 'feature1'
```

修改`readme.txt`最后一行，改为：

```bash
Creating a new branch is quick AND simple.
```

在`feature1`分支上提交：

```bash
$ git add readme.txt

$ git commit -m "AND simple"
[feature1 14096d0] AND simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

切换到`master`分支：

```bash
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
```

**Git还会自动提示我们当前`master`分支比远程的`master`分支要超前1个提交。**

在`master`分支上把`readme.txt`文件的最后一行改为：

```bash
Creating a new branch is quick & simple.
```

提交：

```bash
$ git add readme.txt 
$ git commit -m "& simple"
[master 5dc6824] & simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

现在，`master`分支和`feature1`分支各自都分别有新的提交，变成了这样：

```ascii
                            HEAD
                              │
                              │
                              ▼
                           master
                              │
                              │
                              ▼
                            ┌───┐
                         ┌─▶│   │
┌───┐    ┌───┐    ┌───┐  │  └───┘
│   │───▶│   │───▶│   │──┤
└───┘    └───┘    └───┘  │  ┌───┐
                         └─▶│   │
                            └───┘
                              ▲
                              │
                              │
                          feature1
```

**==这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突==**，我们试试看：

```bash
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```

果然冲突了！**Git告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件：**

```bash
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

我们可以直接查看readme.txt的内容：

```bash
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存：

```bash
Creating a new branch is quick and simple.
```

再提交：

```bash
$ git add readme.txt 
$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed
```

现在，`master`分支和`feature1`分支变成了下图所示：

```ascii
                                     HEAD
                                       │
                                       │
                                       ▼
                                    master
                                       │
                                       │
                                       ▼
                            ┌───┐    ┌───┐
                         ┌─▶│   │───▶│   │
┌───┐    ┌───┐    ┌───┐  │  └───┘    └───┘
│   │───▶│   │───▶│   │──┤             ▲
└───┘    └───┘    └───┘  │  ┌───┐      │
                         └─▶│   │──────┘
                            └───┘
                              ▲
                              │
                              │
                          feature1
```

用带参数的`git log`也可以看到分支的合并情况：

```bash
$ git log --graph --pretty=oneline --abbrev-commit
*   cf810e4 (HEAD -> master) conflict fixed
|\  
| * 14096d0 (feature1) AND simple
* | 5dc6824 & simple
|/  
* b17d20e branch test
* d46f35e (origin/master) remove test.txt
* b84166e add test.txt
* 519219b git tracks changes
* e43a48b understand how stage works
* 1094adb append GPL
* e475afc add distributed
* eaadf4e wrote a readme file
```

最后，删除`feature1`分支：

```bash
$ git branch -d feature1
Deleted branch feature1 (was 14096d0).
```

工作完成。



#### 小结

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

==**用`git log --graph`命令可以看到分支合并图**==。



## 分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，**Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。**

下面我们实战一下`--no-ff`方式的`git merge`：

首先，仍然创建并切换`dev`分支：

```bash
$ git switch -c dev
Switched to a new branch 'dev'
```

修改readme.txt文件，并提交一个新的commit：

```bash
$ git add readme.txt 
$ git commit -m "add merge"
[dev f52c633] add merge
 1 file changed, 1 insertion(+)
```

现在，我们切换回`master`：

```bash
$ git switch master
Switched to branch 'master'
```

准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```bash
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

**因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。**

合并后，我们用`git log`看看分支历史：

```bash
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
...
```

可以看到，不使用`Fast forward`模式，merge后就像这样：

![git-no-ff-mode](https://www.liaoxuefeng.com/files/attachments/919023225142304/0)



### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，==`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活==；

**那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；**

**你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。**

所以，团队合作的分支看起来就像这样：

![git-br-policy](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

### 小结

Git分支十分强大，在团队开发中应该充分应用。

**合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。**

## Bug分支

软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，**所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。**

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交：

```bash
$ git status
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
```

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

**幸好，==Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作==：**

```
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```

现在，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

```bash
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git checkout -b issue-101
Switched to a new branch 'issue-101'
```

现在修复bug，需要把“Git is free software ...”改为“Git is a free software ...”，然后提交：

```bash
$ git add readme.txt 
$ git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

```bash
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

太棒了，原计划两个小时的bug修复只花了5分钟！现在，是时候接着回到`dev`分支干活了！

```bash
$ git switch dev
Switched to branch 'dev'

$ git status
On branch dev
nothing to commit, working tree clean
```

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```bash
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

**==一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；==**

**==另一种方式是用`git stash pop`，恢复的同时把stash内容也删了==**

```bash
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```

再用`git stash list`查看，就看不到任何stash内容了：

```bash
$ git stash list
```

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```bash
$ git stash apply stash@{0}
```

在master分支上修复了bug后，我们要想一想，dev分支是早期从master分支分出来的，所以，这个bug其实在当前dev分支上也存在。

那怎么在dev分支上修复同样的bug？重复操作一次，提交不就行了？

有木有更简单的方法？

有！

同样的bug，要在dev上修复，我们只需要把`4c805e2 fix bug 101`这个提交所做的修改“复制”到dev分支。注意：我们只想复制`4c805e2 fix bug 101`这个提交所做的修改，并不是把整个master分支merge过来。

为了方便操作，Git专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支：

```bash
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Git自动给dev分支做了一次提交，注意这次提交的commit是`1d4b803`，它并不同于master的`4c805e2`，因为这两个commit只是改动相同，但确实是两个不同的commit。用`git cherry-pick`，我们就不需要在dev分支上手动再把修bug的过程重复一遍。

有些聪明的童鞋会想了，既然可以在master分支上修复bug后，在dev分支上可以“重放”这个修复过程，那么直接在dev分支上修复bug，然后在master分支上“重放”行不行？当然可以，不过你仍然需要`git stash`命令保存现场，才能从dev分支切换到master分支。

### 小结

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。
