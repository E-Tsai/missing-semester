---
layout: lecture
title: "课程简介 + Shell"
date: 2019-01-13
ready: true
video:
  aspect: 56.25
  id: Z56Jmr9Z34Q
---

{% comment %}
[Reddit Discussion](https://www.reddit.com/r/hackertools/comments/anic30/course_overview_iap_2019/)
{% endcomment %}

# 动机

作为计算机科学家，我们知道计算机擅长于帮助人们执行重复性任务。但是，我们经常忘记这种便捷不仅能应用于程序所做的计算，也对计算机的*使用*同样有效。我们拥有许多触手可及的工具，它们能够提高效率，能使我们解决与计算机相关的更复杂的问题。然而，我们中的许多人仅使用了这些工具的一小部分；死记硬背足够多的神奇咒语；陷入困境时盲目地从互联网上复制粘贴命令。

这门课尝试解决以上的问题。

我们想教会你如何充分利用已知的工具，展示可以被加入到你工具箱中的新工具，并希望在你探索（甚至是构造）新工具的过程中给你带来兴奋感。这是我们认为大多数计算机教育中都缺少的一学期课程。

# 课程结构

该课程包括11个1小时的讲座，每个讲座都围绕一个[特定主题](/2020/)。这些讲座在很大程度上是独立的，但随着课程的进行，我们将假设你已熟悉之前讲座的内容。我们提供了在线讲义，但是会有很多课堂上的内容（例如举的例子）可能不在讲义中。我们将录制讲座并在线发布。

我们尝试在仅11个1小时的时间里覆盖很多领域，所以内容相当密集。为了让你有时间用自己的节奏熟悉内容，每个讲座都包含一组练习来指导重点。每次讲座结束后，我们都会在办公时间回答你遇到的任何问题。如果你是在线上课 ，那么可以向[missing-semester@mit.edu](mailto:missing-semester@mit.edu)发送问题。

由于时间有限，我们无法在相同的详细程度下涵盖所有工具。在可能的情况下，我们会附上资源，以便你进一步挖掘学习，但是如果您特别喜欢某些东西，请不要犹豫，随时与我们联系并寻求指导！

# 主题1: Shell

## 什么是shell?

如今的计算机有着各种各样的命令接口；精美的图形用户界面（GUI），语音界面，甚至AR / VR也随处可见。这些涵盖了80％的用例，但是从根本上讲，它们通常会限制允许的操作——你不能按下不存在的按钮，或是给出一个没有被预编程的语音命令。想要充分利用电脑提供的工具，我们必须放下身段，使用老套的文字界面：shell.

几乎所有你可以接触到的平台都有某种形式的shell，很多平台具有多种shell供你选择。 尽管它们的细节有所不同，但核心大致相同：它们使用一种半结构化方式允许您运行程序，提供输入，检查输出。

在本讲座中，我们将重点介绍Bourne Again SHell，即“bash”。这是使用最广泛的shell之一，其语法与其他许多shell类似。你首先需要一个*终端（terminal）*，以打开*命令行界面（shell promt）*（您可以在其中键入命令）。您的设备可能已预先安装了一个，安装一个也相当容易。

## 使用shell

当你启动终端时，你会看到一个形如下方的*命令行提示符 (prompt)*：

```console
missing:~$ 
```

这就是shell的主文字界面了。它包含的信息是：你正在机器`missing`上运行它，你的“当前工作目录”，或者你当前的位置，是`~`（“home”的简写）。 `$` 符号意味着你不是根用户（root user）（我们之后会涵盖这个）。在这个提示符后你可以键入*命令*，shell会自行翻译理解。最基本的命令是执行某个程序：

```console
missing:~$ date
Fri 10 Jan 2020 11:49:31 AM EST
missing:~$ 
```

在这里，我们执行了`date`程序，它的作用（当然）是打印当前日期和时间。然后shell等待我们另键入下一个命令。我们还可以使用_参数（arguments）_执行命令：

```console
missing:~$ echo hello
hello
```

在这个例子里，我们告诉shell执行程序`echo`，参数是`hello`。 `echo`程序的作用是简单地打印出其参数。 Shell通过按空格将命令拆分来解析，然后运行第一个单词所指的程序，并后续的每个单词都视作程序可以访问的参数。如果您想提供包含空格或其他特殊字符的参数（例如一个名为`My Photos`的目录），则可以用单引号或双引号括起来（`”My Photos“`），或使用`\`转义相关字符 （`My\ Photos`）。

但是shell如何找到`date`或`echo`程序？shell是一个编程环境，就像Python或Ruby，因此它具有变量，条件，循环和函数（下一个演讲会讲到！）。当你在shell中运行命令时，你实际上是在写一个
一小段代码shell能理解的代码。如果shell被要求执行与其编程关键字之一不匹配的命令，它会查阅一个名为`$ PATH`的环境变量，其中列出了当你向shell输入命令时，shell应该搜索程序的目录：


```console
missing:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
missing:~$ which echo
/bin/echo
missing:~$ /bin/echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

当我们运行 `echo` 命令时，shell看见了它应当执行程序 `echo`, 然后它开始搜索在`$PATH`所给出的列表中被 `:`分割的目录，以期望找到一个同名的程序。当它找到时，它便运行这个程序 （假设文件是_可执行的_；稍后我们会介绍这个概念）。我们使用`which`命令找出给定程序名称被执行的文件。我们还可以通过输入执行文件的路径来绕过使用`$PATH`变量。

## shell导航

Shell上的路径是由`/`符号（Linux和macOS）或`\`（Windows）划定的目录列表。在Linux和macOS上，`/`是文件系统的“根”，在该目录有所有目录和文件；而在Windows上，每个磁盘分区都有一个根目录（例如，`C：\`）。我们将假设您使用的是Linux文件系统。以`/`开头的路径称为_绝对（absolute）_路径，任何其他路径都是_相对（relative）_路径。相对路径是相对于当前工作目录而言的，我们可以通过`pwd`命令看到当前路径，可以用`cd`命令更改当前路径。在路径中，`.`代表当前目录，`..`代表当前目录的父目录：

```console
missing:~$ pwd
/home/missing
missing:~$ cd /home
missing:/home$ pwd
/home
missing:/home$ cd ..
missing:/$ pwd
/
missing:/$ cd ./home
missing:/home$ pwd
/home
missing:/home$ cd missing
missing:~$ pwd
/home/missing
missing:~$ ../../bin/echo hello
hello
```

要注意，shell随时都在告知我们当前工作目录是什么，你可以配置自己的命令行提示符，展示你想要看到的内容，我们将在之后的讲座中介绍。

通常来说，让我们运行程序时，除非我们特意指定，它会在当前目录里运行。例如，程序通常在当前目录里搜索文件，或是创建新文件。

`ls`命令可以查看当前目录里有什么：

```console
missing:~$ ls
missing:~$ cd ..
missing:/home$ ls
missing
missing:/home$ cd ..
missing:/$ ls
bin
boot
dev
etc
home
...
```

除非给出目录作为其第一个参数，否则`ls`将输出当前目录的内容。大多数命令接受标志（flag）和
以`-`开头的选项（option）（即带有值的标志）以修改其行为。通常，使用`-h`或`--help`标志运行程序
（相当于Windows上的`/？`）将打印一些帮助文本，告诉你哪些标志和选项可用。例如，`ls --help`告诉我们：

```
  -l                         use a long listing format
```

```console
missing:~$ ls -l /home
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
```

这为我们提供了有关每个文件或目录的更多信息。首先，该行开头的`d`告诉我们 `missing`是一个目录。然后跟随三组三个字符构成的串 （`rwx`）。这些分别是文件（`missing`）所有者的权限 ，所属组（`user`）和其他所有人的权限。 `-`表示对应的主体没有给定的权限。在上面的例子里，仅有`missing`目录所有者被允许修改（`w`）它（即在其中添加/删除文件）。想要进入一个目录，用户必须具有“搜索”（以“ execute”表示： `x`）该目录（及其父目录）的权限。想要列出其内容，则用户必须对该目录具有读取读取（`r`）权限。对于文件，它们的权限规则与你的直觉是一样的。请注意，几乎所有 `/bin`中的文件都为最后一组设置了`x`权限， “其他任何人”，以便任何人都可以执行那些程序。

现在你需要了解的其他便捷程序是`mv`（重命名/移动文件），`cp`（复制文件）和`mkdir`（创建新目录）。

如果你想要得到*更多*关于某个程序参数，输入，输出以及它如何工作的信息，可以试一试`man`命令，它将把另外的程序名当做参数，并向你展示该程序的*手册* （manual）。按`q`键退出。

```console
missing:~$ man ls
```

## 连接程序

在Shell中，程序具有两个与之关联的主要“流”：输入流和输出流。当程序尝试读取输入时，它会从输入流中读取，当它打印时，它会打印到其输出流。通常，程序的输入和输出都是终端。也就是说，你的键盘是输入，你屏幕是输出。但是，我们也可以重新连接这些流！

最简单的重定向方法是 `< file` 和 `> file`。这两种方法让你可以将程序的输入输出重新连接到文件： 

```console
missing:~$ echo hello > hello.txt
missing:~$ cat hello.txt
hello
missing:~$ cat < hello.txt
hello
missing:~$ cat < hello.txt > hello2.txt
missing:~$ cat hello2.txt
hello
```

您也可以将文件名附加到`>>`符号后面。这种输入/输出重定向的真正魅力在于使用_管道（pipes）_时。 `|` 运算符可让您“链接”程序，以使一个程序的输出成为另一个的输入：

```console
missing:~$ ls -l / | tail -n1
drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
missing:~$ curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
219
```

我们会在数据预处理章节讲到更多关于管道的知识。

## 多功能且强大的工具

在大多数类Unix系统上，有一个特殊用户：“ root”用户。你可能在上面的文件列表中已经看到了。 root用户的权限位于（几乎）所有访问限制之上，并且可以在系统中创建，读取，更新和删除任何
文件。您通常不会以root用户身份登录系统，因为这样的话很容易意外破坏某些内容。相反，你将使用`sudo`命令。顾名思义，它可让你以“ su”（“super user”或“ root”的缩写）的权限去做（“do”）操作。当您遇到权限被拒绝的错误时，通常是因为你需要以root身份做某事。首先，请先确认你真的很想那样做！

需要成为root用户才能做的事有很多，其中一件是向挂载在`/sys`下的文件系统`sysfs`写入东西。 ` sysfs`中包含了许多文件形式的内核参数，以便您可以轻松即时地重新配置内核，而无需专业工具。例如，你的笔记本电脑的屏幕亮度通过名为`brightness`的参数决定，它在这个目录下：

```
/sys/class/backlight
```

通过向这个文件写入值，我们就能改变屏幕的亮度了。

你的直觉可能会让你做下面的事情：

```console
$ sudo find -L /sys/class/backlight -maxdepth 2 -name '*brightness*'
/sys/class/backlight/thinkpad_screen/brightness
$ cd /sys/class/backlight/thinkpad_screen
$ sudo echo 3 > brightness
An error occurred while redirecting file 'brightness'
open: Permission denied
```

这个错误可能令你惊讶。毕竟，我们使用了`sudo`来运行这个程序！这是有关shell的重要知识：像`|`，`>`和`<`这样的操作符是由*shell*完成的，而不是由某个程序。 `echo`和它的朋友们不“认识”`|`。他们只是读入它们的输入，再写到它们的输出，并不在意输入输出是什么。在上面的例子中，*shell*（它通过你的用户身份验证）尝试打开亮度文件，并将它作为`sudo echo`的输出。但是由于shell没有以root身份运行，所以出现了权限错误。利用这些知识，我们可以这样解决此问题：

```console
$ echo 3 | sudo tee brightness
```

由于`tee`程序是打开`/sys`文件并进行写入的程序，并且_它_以`root`身份运行，所有权限都没有问题。你可以通过`/sys`控制各种有趣和有用的事情，例如各种系统LED的状态（您的路径可能不同）：

```console
$ echo 1 | sudo tee /sys/class/leds/input6::scrolllock/brightness
```

# 下一步

至此，你已经了解了使用shell来解决基本问题的方法。你应该能够在文件目录中穿梭，以查找感兴趣的文件，并使用大多数程序的基本功能。在下一个讲中，我们将讨论如何使用shell和许多趁手的命令行程序，来执行和自动化更复杂的任务。

# Exercises

 1. 在`/tmp`目录下创建一个 `missing` 目录。

 2. 了解`touch`程序。 `man`程序永远是你的朋友。

 3. 在`missing`目录下使用`touch`程序创建一个叫做 `semester` 的文件。

 4. 将以下内容一行一行地写入该文件中：
    ```
    #!/bin/sh
    curl --head --silent https://missing.csail.mit.edu
    ```
    第一行可能很难理解。`＃`在bash中意味着注释，`！`即使在双引号（`“`）字符串内部也具有特殊含义。Bash用不同的方式来处理单引号的字符串（`'`）：在这个例子中，他们可以解决问题。见[引用](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)以得到更多信息。
    
 5. 尝试运行`semester`程序。使用 `ls`程序来调查为何它无法执行。

 6. 了解`chmod`程序。

 7. 使用`chmod`，使得`./semester`命令可以运行。

 8. 使用 `|` 和 `>`操作读，将`semester`的“最后修改”的输出数据 写入在主目录（home directory）下的`last-modified.txt`中。
    
 9. 编写一条读出从`/sys`笔记本电池的电量或台式机的CPU温度的命令。注意：如果你是macOS
    用户，你的操作系统没有sysfs，因此可以跳过此练习。
    
    
