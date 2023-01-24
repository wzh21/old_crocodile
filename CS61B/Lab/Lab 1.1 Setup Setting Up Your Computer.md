

​																		Lab 1.1 Setup: Setting Up Your Computer

# Learning Goals

**In this lab**, we will set up the software that we will use throughout the course: the **==terminal, git, java, IntelliJ==**, etc. We will also look at a small Java program and learn a little bit about **Java syntax**.

# Before You Begin

如果有任何问题不起作用，或者应该出现的屏幕没有出现，请确保您寻求帮助——***不要**继续下去，因为如果您遇到了死胡同，这可能会使我们更难在以后确定问题。

# Personal Computer Setup

## Task: Installing a Text Editor

If you don’t already have a favorite text editor, we recommend installing one.

The most popular GUI text editors these days seem to be:

1. Sublime Text (free but nags you until you pay): https://www.sublimetext.com/
2. Atom (free): https://atom.io/
3. Visual Studio Code (free): https://code.visualstudio.com/

See this [text editor review](https://www.techradar.com/best/best-text-editors) for a more thorough look at these and other text editors.

The choice isn’t very important, as we will only be using a text editor a few times throughout the course. Later in this lab, and for the rest of the course, we will be using an IDE.**选择并不重要，因为我们在整个课程中只会使用几次文本编辑器。稍后在这个实验室，在课程的其余部分，我们将使用IDE。**

> 已完成 (安装了VScode)

## Task: Configure Your Computer

> configure : 配置

包括:	A. Install Java	B. Install Git	C. Install Windows Terminal (optional but recommended)

**指导网站** : [Lab 1 Setup: Windows | CS 61B Fall 2022 (datastructur.es)](https://fa22.datastructur.es/materials/lab/lab01/windows.html#a-install-java)

Advanced users on Windows may also use the new WSL feature, but we will not be providing official directions or support. Note that if you use WSL, you’ll need to do the setup on both Windows and WSL (Linux), following the instructions above.Windows上的高级用户也可以使用新的WSL功能，但我们不会提供官方指导或支持。注意，如果您使用WSL，则需要按照上面的说明在Windows和WSL（Linux）上进行设置。

> GIT 建议就安装在C盘(一些原因)

==Windows Terminal 是GitBash的替代方案, 不过我们需要把它的内核从PowerShell改成bash Shell (具体来说是Git bash)==

<img src="https://fa22.datastructur.es/materials/lab/lab01/img/windows/terminal_setup_3.png" alt="Terminal Setup 3" style="zoom:33%;" />

我们下一步要更改打开时默认打开Git Bash 而不是 PowerShell

# ==The Terminal==

## Learn to use the Terminal

在CS 61B中，我们将广泛使用终端，甚至比您在前几节课中可能使用的更多。Bash命令非常强大，可以让您创建文件夹或文件，浏览文件系统等。为了快速入门，我们提供了一个简短的指南，介绍了本课程中使用的最基本的命令。请仔细阅读并尝试熟悉命令。我们将在您开始时帮助您，但在课程结束时，我们希望您将成为bash终端的熟练用户！

- ==**`cd`: change your working directory**==

  ```bash
  cd hw
  ```

  This command will change your directory to `hw`.

- ==**`pwd`: present working directory**==

  ```bash
  pwd
  ```

  **This command will tell you the full absolute path for the current directory you are in if you are not sure where you are.如果您不确定所在位置，此命令将告诉您当前所在目录的==完整绝对路径==。**

- **==`.`: means your current directory==**

  ```bash
  cd .
  ```

  **This command will change your directory to the current directory (aka. do nothing).这个命令会将您的目录更改为当前目录（也就是不做任何事情）。**

- ==`..`: means one parent directory above your current directory`..`：表示当前目录上方的一个父目录==

  ```bash
  cd ..
  ```

  This command will change your directory to its parent. If you are in `/workspace/day1/`, the command will place you in `/workspace/`.

- ==`ls`: list files/folders in directory==

  ```bash
  ls
  ```

  This command will list all the files and folders in your current directory.

  ```bash
  ls -l
  ```

  This command will list all the files and folders in your current directory with timestamps and file permissions. This can help you double-check if your file updated correctly or change the read-write- execute permissions for your files.**此命令将列出当前目录中的所有文件和文件夹以及时间戳和文件权限。这可以帮助您再次检查文件是否正确更新或更改文件的读写执行权限。**

- ==**`mkdir`: make a directory**==

  ```bash
  mkdir dirname
  ```

  This command will make a directory within the current directory called `dirname`.

- ==**`rm`: remove a file**==

  ```bash
  rm file1
  ```

  This command will remove file1 from the current directory. It will not work if `file1` does not exist.

  ```bash
  rm -r dir1
  ```

  This command will remove the `dir1` directory recursively. In other words, it will delete all the files and directories in `dir1` in addition to `dir1` itself. Be careful with this command!**此命令将递归删除“dir1”目录。换句话说，除了“dir1”本身之外，它还将删除“dir1“中的所有文件和目录。小心这个命令！**

- ==**`cp`: copy a file**==

  ```bash
  cp lab1/original lab2/duplicate
  ```

  This command will copy the `original` file in the `lab1` directory and and create a `duplicate` file in the `lab2` directory.此命令将复制“lab1”目录中的“原始”文件，并在“lab2”目录中创建“复制”文件。

- ==**`mv`: move or rename a file**==

  ```bash
  mv lab1/original lab2/original
  ```

  **This command moves `original` from `lab1` to `lab2`. Unlike `cp`, mv does not leave original in the `lab1` directory.此命令将“original”从“lab1”移动到“lab2”。与“cp”不同，==mv不会将原始文件保留在“lab1”目录中==。**

  ```bash
  mv lab1/original lab1/newname
  ```

  **This command does not move the file but rather renames it from `original` to `newname`.此命令不会移动文件，而是将其从“原始”重命名为“新名称”。**

  

  There are some other useful tricks when navigating on a command line:

- Your shell can complete file names and directory names for you with *tab completion*. When you have an incomplete name (for something that already exists), try pressing the `tab` key for autocomplete or a list of possible names.

  ==shell可以使用*制表符完成*来完成文件名和目录名。如果您的名称不完整（对于已经存在的名称），请尝试按“tab”键以自动完成或列出可能的名称。==

- If you want to retype the same instruction used recently, press the `up` key on your keyboard until you see the correct instruction. This saves typing time if you are doing repetitive instructions.如果要重新键入最近使用的相同指令，请按键盘上的“向上”键，直到看到正确的指令。如果您重复执行指令，这将节省打字时间。

## Task: Terminal Test Run

Let’s ensure that everything is working.

1. First open up your terminal. Check that git is a recognized command by typing the following command:

   ```bash
   git --version
   ```

   The version number for git should be printed. If you see “git: command not found”, or similar, try opening a new terminal window, restarting your computer, or installing git again.

2. Second, let’s check that `javac` and `java` are working. `javac` and `java` allow *Command Line Compilation*, or in other words, the ability to run Java programs directly from the command line. In practice, most developers run Java programs through an IDE like IntelliJ, so we won’t be using command line compilation for much this semester other than testing your setup. Start by running the following commands at your terminal.其次，让我们检查“javac”和“java”是否正常工作==`javac`和`java`允许*命令行编译*，或者换句话说，允许直接从命令行运行java程序。==实际上，大多数开发人员都是通过IntelliJ这样的IDE运行Java程序的，**==因此本学期除了测试您的设置外，我们将不再使用命令行编译。==**首先在终端上运行以下命令。

   ```bash
   mkdir ~/temp
   cd ~/temp
   ```

   > **==`~`波浪号表示根目录==**

   1. Then, open your operating system’s file explorer in this directory. You can do this from the command line:然后，在此目录中打开操作系统的文件资源管理器。可以从命令行执行此操作：

      - Mac: `open .`
      - ==Windows: `explorer .`== **==其中explorer是文件管理器的意思, `.`是当前目录/文件的意思==**
      - Ubuntu: `gnome-open .`
      - Linux Mint: `xdg-open .` or `mate .`

   2. In this newly opened directory, create a file `HelloWorld.java` with these contents:在这个新打开的目录中，创建一个包含以下内容的文件`HelloWorld.java `：

      ```bash
      public class HelloWorld {
          public static void main(String[] args) {
              System.out.println("Hello world!");
          }
      }
      ```

      注意，除了文件查找器(file finder)或资源管理器(explorer)，**==您还可以使用终端中的“touch”创建文件==**；尝试从终端使用“touch HelloWorld.java”创建文件！然后使用您喜爱的文本编辑器打开文件并添加代码。

      <img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230118164252424.png" alt="image-20230118164252424" style="zoom:50%;" />

   3. In your terminal, enter `ls` (list the files/folders in this directory). You should see `HelloWorld.java` listed.在终端中，输入“ls”（列出此目录中的文件/文件夹）。您应该看到“HelloWorld.java”已列出。

   4. 运行`javac HelloWorld.java `。如果这会产生任何输出，那么您的设置可能有问题。尝试打开新的终端窗口或重新启动计算机。如果仍然不起作用，请参阅操作系统说明下的故障排除部分。

   5. 键入“ls”，您将看到“HelloWorld.java”和新创建的“HelloWorld.class”（“javac”命令创建了此文件）。

   6. 运行“java HelloWorld”。它应该为你打印出“hello world！”。如果没有，说明您的设置有问题！

   7. 你做完了！您还可以根据需要删除“temp”文件夹及其内容。

   下面的截图显示了我们在执行步骤4-7时所希望的内容。如果您看到类似的内容，那么您的java设置就完成了。

   <img src="https://fa22.datastructur.es/materials/lab/lab01/img/hello_world.png" alt="hello_world" style="zoom: 50%;" />



## E. Install IntelliJ

1. Download the Community Edition of IntelliJ from the [JetBrains](https://www.jetbrains.com/idea/download/) website.
2. After selecting the appropriate version for your OS, click download and wait a few minutes for the file to finish downloading.
3. Run the installer. If you have an older version of IntelliJ, you should uninstall it at this time and replace it with this newer version. You can use all of the default installation options, with one exception, if **you are on Windows**, make sure you check the box “Add launchers dir to the PATH”. If you accidentally missed it, the easiest fix is to uninstall intelliJ, and running the installer again, making sure you check that box the second time around. **The image below only applies to Windows.**

<img src="https://sp21.datastructur.es/materials/lab/lab1setup/img4/path.png" alt="Path" style="zoom: 50%;" />

## F. Installing the IntelliJ CS 61B Plugins

Begin the setup process by starting up IntelliJ. Then follow the steps below.

**Make sure you’re running IntelliJ Version 2020.3.1 or later before continuing.** Older versions may also work but we haven’t tried them ourselves.

1. In the *Welcome* window, click the **“Plugins”** button in the menu on the left.<img src="https://sp21.datastructur.es/materials/lab/lab1setup/img2/plugin_setup1.png" alt="Configure Plugin" style="zoom: 33%;" />
2. On the window that appears, click “Marketplace” and enter “CS 61B” in the search bar at the top. The CS 61B plugin entry should appear. If you click the autocomplete suggestion, a slightly different window from what is shown below may appear – this is okay.
3. Click the green **Install** button, and wait for the plugin to download and install.<img src="https://sp21.datastructur.es/materials/lab/lab1setup/img2/plugin_setup2.png" alt="Search CS 61B" style="zoom:33%;" />
4. Now, search for “Java Visualizer”, and click the green **Install** button to install the plugin.<img src="https://sp21.datastructur.es/materials/lab/lab1setup/img2/plugin_setup3.png" alt="Search Java Visualizer" style="zoom:33%;" />
5. If it appears, click the green **Restart IDE** button to finalize the installation. If you don’t see a **Restart IDE** button, just continue.

For more information on using the plugins, read [the plugin guide](https://sp21.datastructur.es/materials/guides/plugin.html#using-the-plugins). You don’t have to read this right now.

## G. Celebrate

Phew! You’re finally done setting everything up. You can now proceed to lab 1.
