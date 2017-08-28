---
title: git stash命令
date: 2016-10-11 22:57:10
tags: 
- git
- work
---
前一段时间开始开发数据库连不上，所以所有的调试都是用本地数据库，这样每次调试都要修改application文件，并且push到远程时，要保持此文件不被修改，还要讲本地的application文件和master分支上的进行checkout，很不方便,所以问了身边同事怎么解决，发现可以用git stash。
<!--more-->
首先git stash是将现在修改的文件放入堆栈中，最主要的应用是当我们在一个分支上进行开发，当我们在这个分支上的开发还没有完成时需要切换到其他的分支进行开发，所以我们需要将现在的分支暂时保存，我一般会先进行一次commit操作，所以在自己提交的分支上会有很多无意义的提交。解决上述问题：
1.在mater分支上新开一个分wy_application并切换。

2.将application文件的开发数据库设为本地的。（除此之外不要做任何修改）

3.将现在的修改放入堆栈:git  stash。但考虑到我之后的开发可能会有其他的东西继续放入堆栈，如果我的堆栈中有很多东西，到时候就不知道取那一个了，所以给这次的东西打上标签 ，执行：git stash save db

<img src="/img/git stash -1.png" >

显示我们修改的东西已经放入堆栈，并且当前分支没有任何修改。

4.执行：git stash list查看现在堆栈区的内容：

<img src="/img/git stash -2.png" >

可见现在堆栈区只有此存入，标签为db。

5.假设现在我需要在wy_growth分支上进行开发，那么切换到wy_growth分支，先用第四步的git stash list查看：

<img src="/img/git stash -3.png" >

从标签来看，stash@{1}是对应的数据库修改的内容，所以可以取出内容。取出内容有两种方法：一：git stash pop stash@{1} 指的是出栈，但一旦出栈，在栈中就不存在了。二：git stash apply stash@{1} 只是取出内容，在堆栈中还会存在。对于我的需求当然是使用第二种方法

<img src="/img/git stash -4.png" >

此时，此分支的状态

<img src="/img/git stash -5.png" >

堆栈区状态：

<img src="/img/git stash -6.png" >

可见db的修改依然在堆栈区，下次还可以继续使用，对应的分支上也有了要修改的内容。

6. 最后当分支需要提交时，可以选择此文件不add，或是使用master上的文件checkout。

总结：git stash 命令虽然方便，但如果多次调用，那么不同的修改在何时出栈，在那个分支出栈要很清楚，所以添加标签就显得非常重要。
