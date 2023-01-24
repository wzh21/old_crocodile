​																	

​																			Lab 1: IntelliJ, Java, git

## Before You Begin

- 在开始本实验之前，请完成[lab1setup](https://sp21.datastructur.es/materials/lab/lab1setup/lab1setup)安装61B所需的软件。若你们在任何时候都被卡住了，可以随时在Ed上发帖或参加实验室，或来到办公时间。
- 请注意，第一周有大量的设置步骤**不要气馁**，如果遇到困难，一定要寻求帮助！
- 警告：此实验室运行时间有点长，如果您在实验室期间没有完成，这是正常的，特别是如果您最终遇到需要大量帮助的棘手设置问题。
- 当你在这个实验室（或任何其他实验室）工作时，与其他学生交谈是可以的，但最终你应该自己完成所有的命令输入/编程。这个实验室里有很多重要的设置信息，你需要独立于其他人完成。

## GitHub and Beacon

Instead of bCourses, CS61B uses an in-house system for centralizing your grades and student information called Beacon.

In this section, we’ll set up your Beacon account as well as your CS 61B GitHub repository (“repo”), which you will need to submit all coding assignments.

1. Create an account at [GitHub.com](https://github.com/). If you already have an account, you do not need to create a new one.
2. Go to [the Beacon website](https://sp21.beacon.datastructur.es/register/) and you’ll be guided through a few steps to complete your GitHub repository registration. Please follow them carefully! You must be logged in to your Berkeley account to complete the Google Form quiz. If any errors occur while you’re working through the steps, please let your TA know immediately.
3. After completing all of the steps, you should receive an email inviting you to collaborate on your course GitHub repository, accept the email invitation from **both** emails and you should be good to go. This email will be sent to the **email that you used to create your GitHub account, which may not necessarily be your Berkeley email**. Hooray! **Don’t follow the instructions that GitHub says you might want to do** – instead, follow the instructions given later in this lab.

> **==非Berkeley的学生是肯定用不了Beacon的==**

### More details about your repository

Your repository will have a name containing a number that is unique to you! For instance, if your repo is called “sp21-s42”, you’ll be able to visit your private repository at https://github.com/Berkeley-CS61B-Student/sp21-s42 (when logged into GitHub) If your repo number is not “42” this link will not work for you. Replace “42” with your own to see your repo on Github.

此外，讲师、助教和导师将能够查看您的存储库。这意味着你可以（也应该！）在Ed上提问私人调试问题时链接到你的代码。其他学生将无法查看你的存储库。作为提醒，即使在完成课程后，您也不能公开发布本课程的代码。这样做违反了我们的课程政策，您可能会受到纪律处分。

如果您与实验室合作伙伴合作，您还将收到一个单独的共享存储库，您应该将其用于实验室。更多详情请参见我们的《合作伙伴指南》，您可以在我们的课程信息页面上找到。

Additionally, after registering with Beacon, you will be invited to collaborate on another repo of the form “snaps-sp21-sXXX” - don’t worry about what this repo is for now, just accept the invitation so you have access to it. We will have more information about this repo in a later section of this lab!

## A. Getting the Starter Files

For most of the assignments in this class (including this lab), **we’ll provide a set of starter files.**

**==To get the starter files, we’ll be using the git version control system==**, which you’ll learn more about later in this lab and throughout the course. For now, let’s simply use git to get the files for this lab.

If you need help with creating directories, creating files, changing directories, etc., see section C of [lab1setup](https://sp21.datastructur.es/materials/lab/lab1setup/lab1setup.html).

1. First, tell Git your name and email address so that your commits are correctly attributed to you. Run the following commands:

   Replace “Your Name” with your name, and “you@berkeley.edu” with your email address. You should use the email address that you used to sign up for GitHub. If that’s not your “@berkeley.edu” email address, that’s okay.

   ```bash
    $ git config --global user.email "you@berkeley.edu"
    $ git config --global user.name "Your Name"
   ```


<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230119190904777.png" alt="image-20230119190904777" style="zoom:50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230119192759062.png" alt="image-20230119192759062" style="zoom: 50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230119192926205.png" alt="image-20230119192926205" style="zoom: 33%;" />







### Task: Creating Projects 

The following instructions apply for both this lab and for future assignments. **Each time after pulling from `skeleton` to get new lab or project files, you will need to run through the following steps again.**

1. Start up IntelliJ.

2. Upon opening IntelliJ, click on the “Open” option.

   ![intellij-1](https://fa22.datastructur.es/materials/lab/lab01/img/intellij_home_screen.png)

3. Find and choose the directory of your current assignment (for today’s lab, it should be the `lab01` directory), then press the OK button.

   ![intellij-2](https://fa22.datastructur.es/materials/lab/lab01/img/intellij-2.png)

4. You should at this point be greeted with the main screen of IntelliJ which should look like the following. If you are greeted with any additional screens when setting up the project you can go ahead and select the default options for now.

   ![intellij-3](https://fa22.datastructur.es/materials/lab/lab01/img/intellij-3.png)

5. Now we will set up the SDKs. Navigate to the **“File -> Project Structure”** menu,

   ![intellij-4](https://fa22.datastructur.es/materials/lab/lab01/img/project-structure.png)

   and then make sure you are in **Project** tab. Afterwards, set your project SDK to your installed Java version (17 or 18). If 17 or higher isn’t available in the dropdown, make sure you downloaded the Java SDK earlier in the [setup steps](https://fa22.datastructur.es/materials/lab/lab01/#configure-your-computer), and then execute the following:

   ![intellij-5](https://fa22.datastructur.es/materials/lab/lab01/img/set-SDK-1.png)

   ![intellij-6](https://fa22.datastructur.es/materials/lab/lab01/img/set-SDK-2.png)

6. Finally we will add the Java libraries. Note that you don’t need the libraries for this lab, but it is good practice walking through these steps going forward. Navigate to the **“File -> Project Structure”** menu, then select **“Libraries”** in the sidebar. You should be greeted with a screen that looks like this.

   ![intellij-4](https://fa22.datastructur.es/materials/lab/lab01/img/intellij-4.png)

7. Click the “**+ -> Java**” button, which should prompt you to select the file location. Navigate to your `library-fa22` repo and select all of the `*.jar` files (i.e. select all of the files that end in `.jar` in the `library-fa22` folder).

   ![intellij-5](https://fa22.datastructur.es/materials/lab/lab01/img/select-jars.png)

   Next, if prompted to add the library to the `lab01` module, select “**Ok**”. After these steps the libraries tab should look like the following with all of the `.jar` files listed here.

   ![intellij-5](https://fa22.datastructur.es/materials/lab/lab01/img/jar-window.png)

   At this point if things have been configured correctly each Java file should have a blue circle next to its name, and when you open the file `LeapYear.java` file you should see a green triangle near the line numbers on line 5. Further, none of the code should be higlighted in red.



> **==jar : 是压缩文件格式的一种,类似于zip!!!!!!!!!!!!!!!!!!!!==**



## E. Pushing Your Work to GitHub

我们将在这个类中使用Git工具将您的工作副本存储在所谓的“存储库”中。我们将在稍后的实验室中看到，您还可以使用Git从存储库中检索旧版本的工作。事实上，Git是现实世界中最流行的代码版本控制软件，如果你从事软件工程，几乎可以保证你每天都会使用这个工具。

私有公司GitHub拥有能够存储git存储库的服务器，其网站GitHub.com提供了一个方便的web界面来查看存储库。In this section, we’ll see how to “push” your repository to GitHub.

首先，转到与存储库对应的GitHub URL。例如，如果您的存储库编号是343，您可以转到https://github.com/Berkeley-CS61B-Student/sp21-s343.转到与存储库对应的URL。你会发现那里什么都没有。如果您尝试其他存储库编号，您将看到您没有访问权限。也就是说，只有您（以及课程人员）可以查看您的简历。

要推送代码，我们将使用以下三个命令：“add”、“push”和“commit”。现在不要担心细节，只需遵循以下步骤：

1. Open a terminal window and navigate to the folder containing your `sp21-s***` repository, **NOT** your `snaps-sp21-s***` repo. If you type the “ls” command, you should see your lab1 folder sitting there.

2. Enter the following command to confirm that you’re in the right directory.

   ```
    $ git status
   ```

   If everything is working correctly you should see something like:

   ```
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git restore <file>..." to discard changes in working directory)
        modified:   lab1/Collatz.java
   ```

   What Git is telling you is that you’ve changed something about your lab1/Collatz.java folder that GitHub has not recorded. **Make sure that it says lab1/Collatz.java, not just Collatz.java**. If it says just Collatz.java, you should use `cd..` to go up one directory.

3. Enter the command

   ```
    $ git add lab1/*
   ```

4. Enter the following command to confirm that add worked properly.

   ```
    $ git status
   ```

   If everything is working correctly, you should see something like:

   ```
    On branch master
    Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            modified:   lab1/Collatz.java
   ```

   Now Git is telling you that it is acknowledging that you want to record the changes you made to Collatz.java.

5. Enter the command

   ```
    $ git commit -m "done with Collatz"
   ```

   When you do this, Git will make a recording of the changes you made to Collatz.java. It will also record the message “done with Collatz” along with the recording. These messages can be helpful if you’re looking to find a specific change at some point in the past. We’ll discuss further in a later lab. It will also print some output the terminal, and you’ll see something like below, though the number of insertions and deletions may be different.

   ```
    [master e2c138b] done with Collatz
    1 file changed, 15 insertions(+), 1 deletion(-)
   ```

6. As before, enter the git status command.

   ```
    $ git status
   ```

   If commit worked correctly, you should see something like:

   ```
    On branch master
    nothing to commit, working tree clean
   ```

   The fact that the “working tree” is “clean” means that all of your work is backed up.

7. Refresh the URL for your repo on GitHub.com. You’ll see that your changes STILL aren’t showing up. This is because we have one last step.

8. Enter the command:

   ```
    $ git push origin master
   ```

9. Refresh the URL for your repo on GitHub.com. You should see that your changes to Collatz.java are now visible, along with the message you entered.

Note that normally you won’t enter `git status` after every single command. We were only doing this to make sure that you entered the commands correctly. The three commands that we entered that were actually necessary to get your code on GitHub are summarized below:

```
git add lab1/*
git commit -m "done with Collatz"
git push origin master
```

A lot of questions probably come to mind:

- Why is it useful to have to manually track my changes rather than just using a tool like Dropbox that automatically makes backups?
- What do these individual commands do? If commit made a recording of my work, but it wasn’t visible until I entered push, then where was the backup made?
- Why does it take 3 commands instead of just one? Why not just a single command like ‘git upload -m “done with Collatz”’ or whatever?

Let’s address them in turn:

**Q: Why wouldn’t people just use Dropbox (or similar)?** Dropbox is backing up everything at sporadic intervals. When writing programs, we often want to make backups at specific times and only of specific files. For example, when you finally get a program working, you might want to back it up at that point so you can come back to it if you break it later. Also the ability to add a message makes it easy to organize the backups so you can find the ones you want.

**What does add do?** Add marks a file (or set of files) as something you want to backup.

**What does commit do?** Commit creates a backup with the given message.

**If commit makes a backup but not to GitHub, where did it back up my files?** On your own computer!

**What good is a backup on your own computer?** If you want to go back to an old version, you can do that using git using other commands. By storing the backup locally, restoring old backups is very fast and doesn’t require an internet connection.

**What if my computer dies, isn’t the backup made by commit lost?** Yes. That’s why there’s a push command.

**Why not do add, commit, and push in one command?** Sometimes you only want to add a small number of files, so you might call add on only those files before finally doing a commit. And sometimes you want to make commits but don’t want to push, for example because you don’t have internet access or because you’re working on code that is too sensitive to be placed on any internet site.

**How do I see the history of old backups and how do I restore them?** If you want to see old commits, you can use `git log`, and you can use `git checkout` to restore old copies of your code. We’ll cover these later. Alternately you can also use the web interface at GitHub.com to explore and even download old copies.

We will come to understand git better throughout the semester.

Bonus: As an analogy for those of you have seen the 1984 film Ghostbusters (which I watched literally every day as a small kid), the `add` command is somewhat like using your proton accelerator on a ghost (or ghosts), the `commit` command is a somewhat like pulling the ghost (or ghosts) into the trap, and the `push` command is somewhat like unloading the trap into the Containment Unit.

## F. Submitting Lab 1

The last step is to submit your work with Gradescope. Gradescope is the site that you’ll use to submit homework, lab, and project assignments. To sign up for Gradescope (if you haven’t already), head to [gradescope.com](http://gradescope.com/) and click on the white “Sign up for free” button in the middle. You should use your Berkeley email. You should have already been enrolled in our Gradescope page with your Berkeley email. If you have not been added to Gradescope yet, you can refer to our welcome post on Ed for an enrollment code.

Once you’re enrolled in the class on Gradescope, click on “Lab 1: Welcome to Java” to submit your code.

After clicking this button, you’ll be taken to a screen where you select your repository and branch (shown below). The first time you do this, you’ll have to link your GitHub account to Gradescope (similar to what you did on the registration website). Click the “Connect to GitHub” button and follow the instructions.

Now select your repository and branch. If your repository is “sp21-s57”, you’ll select “sp21-s57” in the top box, and in the bottom box you’ll pick “master”.

Later, you can create your own “branches” (as described in a later optional part of a 61Blab) if you want those graded instead, though that won’t be required in 61B.

![Github Repo and Branch Selection](https://sp21.datastructur.es/materials/lab/lab1/img/github_repo_and_branch_selection.png)

Now press “Upload” to submit!

Please report any issues you may have to Ed. Entire error messages and/or screenshots are welcome.

***Important:\*** *We HIGHLY encourage you to make frequent commits!* **Lack of proper version control will not be considered an excuse for lost work, particularly after the first few weeks.**
