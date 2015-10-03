简介
=======

本人用R语言编程有十来年了，其中有幸能够花些时间搞清楚R语言的内核和工作原理。在此将我多年所学整理成此书，希望能帮你快速成为一名高效的R程序员。此书将会帮助你避免我在R编程中常犯的一些错误和程序死结问题，教你一些有用的工具，技巧和常用方法来帮助你解决大部分类型的问题。刚接触R语言可能会被一些怪异的语法弄的沮丧，但我希望通过本书让你认识到R语言本质上是一门高雅而又华丽的语言，尤其适合用来做数据分析和统计学研究。

如果你是一个R语言新手，你可能会好奇为什么值得学习这们略显奇怪的语言。在我看来，和其他众多的编程语言相比，R语言有如下一些过人之处的特点：

* R是免费的，在绝大多数平台上都可以运行。这样的好处是，如果你用R做分析，别人可以很容易重复你的结果。

* R拥有众多的工具包，涵盖统计模型，机器学习，数据可视化，数据倒入与倒出以及数据整理。无论你想用什么模型和图表，你都可以从别人开发的工具包中直接使用。花最少的时间从别人那里学到你想要的。

* R提供最前卫的分析工具。统计和机器学习领域的研究学者通常在发表的文献中附上他们开发的相应的R工具包。这意味着你能快速体验到这些最新的算法和统计方法。

* R是一门为数据分析高度定制的语言，包括缺失值，数据框，取子集等一系列特性。

* R拥有一个相当活跃的社区文化。你很容易直接通过[R-help](https://stat.ethz.ch/mailman/listinfo/r-help)邮箱列表，[stackoverflow](http://stackoverflow.com/questions/tagged/r)或者一些有主题的邮箱列表例如[R-SIG-mixed-models](https://stat.ethz.ch/mailman/listinfo/r-sig-mixed-models) 和 [ggplot2](https://groups.google.com/forum/#!forum/ggplot2)得到那些R专家的帮助。你也可以和其他R使用者通过[twitter](https://twitter.com/search?q=%23rstats)，[linkedin](http://www.linkedin.com/groups/R-Project-Statistical-Computing-77616)和通过一些当地的[用户群](http://blog.revolutionanalytics.com/local-r-groups.html)建立社交关系。


* R让你能很方便的和别人交流你的分析结果。R包让你很轻松的将你的分析结果输出成html或者pdf的[报告](http://yihui.name/knitr/)，甚至生成[动态的交互网页](http://www.rstudio.com/shiny/)。

* R语言的设计基于函数编程理论。函数编程的思想适合解决绝大部分的数据分析问题。R提供强大而又灵活的工具让你编写精确而有高可读性的代码。

* R拥有一个专门为交互数据分析和统计编程而打造的[IDE](http://www.rstudio.com/ide/).

* R拥有强大的元编成架构。R不紧紧是一门编程语言，也是一个交互数据分析的环境。R的元编成能力让你编写魔法般地简洁明了的函数，同时提供一个设计专用领域语言的完美环境。

* R在设计上对像C，Fortran，和C++一类的高性能语言开放接口。

当然，R语言也不是完美的。对于使用R语言的用户来说，他们其中大多数并不是程序员背景。这意味着：

* 很多你在网络上搜寻到的R代码都是为某个紧迫的问题而匆忙写成的。这样的代码不够完美，也没有做优化，并且可能很难理解。很多的用户在写成代码后并不会去解决以上的问题。

* 和其他语言相比，R社区讨论的问题更多的集中在问题的结果上而不是注重处理问题的过程。关于软件工程方面的的实践应用算是凑合用的：比如很多的R程序员都不会用源代码控制和自动测试。

* 元编程是一把双刃剑。很多的R函数用一些技巧来减少代码量，然而牺牲了可读性，并且可能导致非预期的结果。

* R语言有20多年的进化历史，在发布的不同的R工具包甚至base R中的变量与函数名的不一致性是普遍存在的。这使得学习R语言需要记住不同情况下的不同函数调用方法，从而加大了学习难度。

* R并不是一门注重运行速度的语言，糟糕的R代码会使得运行速度极其的慢。R语言占用内存也是很严重的。

在我个人看来，R语言当前所遇到的这些挑战对于有经验的程序员来说是一个很大的机会，他们大可以施展拳脚来推动R语言和R社区向更加成熟和完善的方向发展。R用户都很在乎写出高质量的代码，尤其是在做可重复性研究的时候，可是他们缺少相关的知识。我希望本书不仅能帮助更多的R用户提高能力成为高效的R程序员，同时也希望鼓励更多的使用其他语言的专业程序员来为R语言的发展做出贡献。


## 这本书适合谁
这本书适合以下两类读者：

* 希望更深入探索R并且了解解决不同问题的新方法的R中级程序员。

* 正在学习R并且想要知道R工作原理的其他编程语言的程序员。

为了更好的使用这本书，你需要一定的R语言或其他语言的编程经历。你不需要了解所有R的知识，但你需要知道函数是如何在R中运行的，尽管你可能感觉熟练地驾驭R函数还有些吃力。同时你也应该熟悉apply家族函数的使用方法，比如`apply()`和`lapply()`.


##你会从本书学到什么

这本书包括了我认为一个高阶的R程序员需要掌握的技能：能写出在不同环境下运行的高质量代码。

读完本书，你讲学到：

* 熟悉R的基本知识。你会理解复杂的数据类型并且知道处理这些数据类型相应的最佳方法。你将对R函数是如何运行的有更深入的理解，并且能够理解和使用R的四种面向对象体系。

* 理解什么是函数式编程，以及它为什么适合于数据分析。你将会快速的掌握如何运用已有的工具，并且能够编写你需要的新工具。

* 了解元编程的双刃剑特性。你将会在规则范围内编写非标准评估的函数来节省代码，并且写出能处理复杂操作的华丽代码。你也会明白运用元编成的风险，并且知道为什么要小心的使用元编成。


Appreciate the double-edged sword of metaprogramming. You’ll be able to create functions that use non-standard evaluation in a principled way, saving typing and creating elegant code to express important operations. You’ll also understand the dangers of metaprogramming and why you should be careful about its use.

Have a good intuition for which operations in R are slow or use a lot of memory. You’ll know how to use profiling to pinpoint performance bottlenecks, and you’ll know enough C++ to convert slow R functions to fast C++ equivalents.

Be comfortable reading and understanding the majority of R code. You’ll recognise common idioms (even if you wouldn’t use them yourself) and be able to critique others’ code.


