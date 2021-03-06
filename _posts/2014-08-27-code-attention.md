---
layout: post
category : 随意记录
title: '再看优质代码十诫'
tagline: ""
tags : [代码, 思考]
---

原文[《优质代码十诫》](http://coolshell.cn/articles/1007.html)

12年的时候第一次看优质代码十诫，偶然再看，引用原作者的告诫，附带自己的一些想法。

### 1、DRY: Don’t repeat yourself

意味着，当我们在两个或多个地方的时候发现一些相似的代码的时候，我们需要把他们的共性抽象出来形一个唯一的新方法，并且改变现有的地方的代码让他们以一些合适的参数调用这个新的方法。

最近中做了不少类似的工作，把复制粘贴的代码抽取出来改用新的方法调用。在同事的建议下，一些更通用的方法可以写工具类，如`jsonClass`。新建`CommonModule`，继承父级调用公用的方法。

<!--break-->

### 2、短小的方法

- 代码会变得更容易阅读

- 代码会变得更容易重用（短方法可以减少代码间的耦合程度）

- 代码会变得更容易测试

一个函数方法动则上百行，会大大增加理解代码的难度和代码复杂度。

写代码的时候想一下，真的需要这么长？

这个数据处理应该能用一个处理方法调用？

这个调用的方法能够一目了然地知道是做了什么的？

而且长方法给重构的人增加难度，导致不断拆分测试。

### 3、良好的命名规范

使用不错的统一的命名规范可以让你的程序变得更容易阅读和维护

花时间将代码的目录名、文件名、类名、方法名统一规则，重新整理。即使不是你造成文件分布混乱、命名无规则的，你也有能力和有责任将这些整理好，相信之后这可以有效减少工作量。比如花时间找文件和理解方法上。同时记住良好的命名可以做到`望文生义`。

### 4、赋予每个类正确的职责

一个类，一个职责，这类规则可以参考一下类的[SOLID 法则](http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)。但我们这里强调的不是一种单一的职责，而是一个`正确的职责`。

|简写   |             全称                   |          备注
|-------|------------------------------------|------------------
|SRP	|The Single Responsibility Principle |A class should have one, and only one, reason to change.
|OCP	|The Open Closed Principle	         |You should be able to extend a classes behavior, without modifying it.
|LSP	|The Liskov Substitution Principle	 |Derived classes must be substitutable for their base classes.
|ISP	|The Interface Segregation Principle |Make fine grained interfaces that are client specific.
|DIP	|The Dependency Inversion Principle	 |Depend on abstractions, not on concretions.

除了类，也要赋予方法正确的职责，最好是单一的职责。

### 5、把代码组织起来

- 物理层组织：无论你使用什么样的目录，包(package)或名字空间(namespace)等的结构。类用一种标准的方法组织起来。

- 逻辑层组织：把两个不同功能的类或方法通过某种规范联系和组织起来。

不管怎样，整理出一套规范，不管是不是最好，写成文档，并告知每个维护代码的人。

### 6、创建大量的单元测试

单元测试是最接近BUG的地方，也是修改BUG成本最低的地方，同样也是决定整个软件质量好坏的成败的地方。

我相信单元测试在某些情况下很有效，但实际上单元测试的编写也会消耗一定量的时间，而需求的变动也容易导致单元测试的变动，也许该根据实际来决定是否需要单元测试。

### 7、经常重构你的代码

- 有大量的单元测试来测试。

- 每次重构都不要大，用点点滴滴的小的重构来代替那种大型的重构。有太多的时候，当我们一开始计划重构2000行代码，而在3个小时后，我们就放弃这个计划并把代码恢复到原始的版本。所以，我们推荐的是，重构最好是从点点滴滴积累起来的。

重构是为下一次重构做准备工作。

### 8、程序注释是邪恶的

大多数时候，我们都不读注释而直接读代码，因为实际程序中写出来的注释却是很糟糕的

实际上，我看到的注释，当然还有没有注释的（需要根据理解补上，避免其他维护的人遭遇同样的困扰），大部分都没写变量类型，几乎全是unknown type，我不理解是什么原因。代码中的吐槽实际上是毫无意义的，当然，可以无视。

重写注释，事实上，随着业务的变化，注释也要更新。代码更新而注释没更新会产生误解，比如我见过`status=98`代表什么什么，但事实上，我根据找不到有这个status的存在。

### 9、注重接口，而不是实现

接口注重的是——`What`是抽象，实现注重的是——`How`是细节。我们的程序员总是注重于实现细节，所以他们局部的代码写的非常不错，但软件整体却设计得相对较差。这点需要我们多多注意。

DAO（数据访问对象），ORM（对象关系映射）有了这个东西，就可以写from(table)->where(con)->select(field)，而不必重复写sql语句了。有了通用渲染就可以减少大量的数据渲染页面的编写了。

### 10、代码审查机制

- 审查者的能力一定要大于或等于代码作者的能力，不然，代码审查就成了一种对新手的training

- 而且，为了让审查者真正负责起来，而不是在敷衍审查工作，我们需要让审查者对审查过的代码负主要责任，而不是代码的作者

- 好的代码审查应该不是当代码完成的时候，而是在代码编写的过程中，不断地迭代代码审查。好的实践的，无论代码是否完成，代码审核需要几天一次地不断地进行。

代码作者本身要review，至少在代码提交之前要重复看一遍，不过自己写的东西有问题发现的概率也较低。