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

Unless a directory is given as its first argument, `ls` will print the
contents of the current directory. Most commands accept flags and
options (flags with values) that start with `-` to modify their
behavior. Usually, running a program with the `-h` or `--help` flag
(`/?` on Windows) will print some help text that tells you what flags
and options are available. For example, `ls --help` tells us:

```
  -l                         use a long listing format
```

```console
missing:~$ ls -l /home
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
```

This gives us a bunch more information about each file or directory
present. First, the `d` at the beginning of the line tells us that
`missing` is a directory. Then follow three groups of three characters
(`rwx`). These indicate what permissions the owner of the file
(`missing`), the owning group (`users`), and everyone else respectively
have on the relevant item. A `-` indicates that the given principal does
not have the given permission. Above, only the owner is allowed to
modify (`w`) the `missing` directory (i.e., add/remove files in it). To
enter a directory, a user must have "search" (represented by "execute":
`x`) permissions on that directory (and its parents). To list its
contents, a user must have read (`r`) permissions on that directory. For
files, the permissions are as you would expect. Notice that nearly all
the files in `/bin` have the `x` permission set for the last group,
"everyone else", so that anyone can execute those programs.

Some other handy programs to know about at this point are `mv` (to
rename/move a file), `cp` (to copy a file), and `mkdir` (to make a new
directory).

If you ever want _more_ information about a program's arguments, inputs,
outputs, or how it works in general, give the `man` program a try. It
takes as an argument the name of a program, and shows you its _manual
page_. Press `q` to exit.

```console
missing:~$ man ls
```

## Connecting programs

In the shell, programs have two primary "streams" associated with them:
their input stream and their output stream. When the program tries to
read input, it reads from the input stream, and when it prints
something, it prints to its output stream. Normally, a program's input
and output are both your terminal. That is, your keyboard as input and
your screen as output. However, we can also rewire those streams!

The simplest form of redirection is `< file` and `> file`. These let you
rewire the input and output streams of a program to a file respectively:

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

You can also use `>>` to append to a file. Where this kind of
input/output redirection really shines is in the use of _pipes_. The `|`
operator lets you "chain" programs such that the output of one is the
input of another:

```console
missing:~$ ls -l / | tail -n1
drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
missing:~$ curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
219
```

We will go into a lot more detail about how to take advantage of pipes
in the lecture on data wrangling.

## A versatile and powerful tool

On most Unix-like systems, one user is special: the "root" user. You may
have seen it in the file listings above. The root user is above (almost)
all access restrictions, and can create, read, update, and delete any
file in the system. You will not usually log into your system as the
root user though, since it's too easy to accidentally break something.
Instead, you will be using the `sudo` command. As its name implies, it
lets you "do" something "as su" (short for "super user", or "root").
When you get permission denied errors, it is usually because you need to
do something as root. Though make sure you first double-check that you
really wanted to do it that way!

One thing you need to be root in order to do is writing to the `sysfs`
file system mounted under `/sys`. `sysfs` exposes a number of kernel
parameters as files, so that you can easily reconfigure the kernel on
the fly without specialized tools. For example, the brightness of your
laptop's screen is exposed through a file called `brightness` under

```
/sys/class/backlight
```

By writing a value into that file, we can change the screen brightness.
Your first instinct might be to do something like:

```console
$ sudo find -L /sys/class/backlight -maxdepth 2 -name '*brightness*'
/sys/class/backlight/thinkpad_screen/brightness
$ cd /sys/class/backlight/thinkpad_screen
$ sudo echo 3 > brightness
An error occurred while redirecting file 'brightness'
open: Permission denied
```

This error may come as a surprise. After all, we ran the command with
`sudo`! This is an important thing to know about the shell. Operations
like `|`, `>`, and `<` are done _by the shell_, not by the individual
program. `echo` and friends do not "know" about `|`. They just read from
their input and write to their output, whatever it may be. In the case
above, the _shell_ (which is authenticated just as your user) tries to
open the brightness file for writing, before setting that as `sudo
echo`'s output, but is prevented from doing so since the shell does not
run as root. Using this knowledge, we can work around this:

```console
$ echo 3 | sudo tee brightness
```

Since the `tee` program is the one to open the `/sys` file for writing,
and _it_ is running as `root`, the permissions all work out. You can
control all sorts of fun and useful things through `/sys`, such as the
state of various system LEDs (your path might be different):

```console
$ echo 1 | sudo tee /sys/class/leds/input6::scrolllock/brightness
```

# Next steps

At this point you know your way around a shell enough to accomplish
basic tasks. You should be able to navigate around to find files of
interest and use the basic functionality of most programs. In the next
lecture, we will talk about how to perform and automate more complex
tasks using the shell and the many handy command-line programs out
there.

# Exercises

 1. Create a new directory called `missing` under `/tmp`.
 2. Look up the `touch` program. The `man` program is your friend.
 3. Use `touch` to create a new file called `semester` in `missing`.
 4. Write the following into that file, one line at a time:
    ```
    #!/bin/sh
    curl --head --silent https://missing.csail.mit.edu
    ```
    The first line might be tricky to get working. It's helpful to know that
    `#` starts a comment in Bash, and `!` has a special meaning even within
    double-quoted (`"`) strings. Bash treats single-quoted strings (`'`)
    differently: they will do the trick in this case. See the Bash
    [quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)
    manual page for more information.
 5. Try to execute the file. Investigate why it doesn't work with `ls`.
 6. Look up the `chmod` program.
 7. Use `chmod` to make it possible to run the command `./semester`.
 8. Use `|` and `>` to write the "last modified" date output by
    `semester` into a file called `last-modified.txt` in your home
    directory.
 9. Write a command that reads out your laptop battery's power level or your
    desktop machine's CPU temperature from `/sys`. Note: if you're a macOS
    user, your OS doesn't have sysfs, so you can skip this exercise.
