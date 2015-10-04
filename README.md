简介
=======

本人用R语言编程有十来年了，其中有幸能够花些时间搞清楚R语言的内核和工作原理。在此将我多年所学整理成此书，希望能帮你快速成为一名高效的R程序员。此书将会帮助你避免我在R编程中常犯的一些错误和程序死结问题，教你一些有用的工具，技巧和常用方法来帮助你解决大部分类型的问题。刚接触R语言可能会被一些怪异的语法弄的沮丧，但我希望通过本书让你认识到R语言本质上是一门高雅而又华丽的语言，尤其适合用来做数据分析和统计学研究。

如果你是一个R语言新手，你可能会好奇为什么值得学习这门略显奇怪的语言。在我看来，和其他众多的编程语言相比，R语言有如下一些过人之处：

* R是免费的，在绝大多数平台上都可以运行。这样的好处是，如果你用R做分析，别人可以很容易重复你的结果。

* R拥有众多的工具包，涵盖统计模型，机器学习，数据可视化，数据倒入与倒出以及数据整理各个方面。无论你想用什么模型或则图表，你都可以直接使用别人开发的工具包。花最少的时间从别人那里学到你想要的东西。

* R提供最前卫的分析工具。统计和机器学习领域的研究学者通常在发表的文献中附上他们开发的相应的R工具包。这意味着你能快速使用上那些最新的算法和统计方法。

* R是一门为数据分析高度定制的语言，包括缺失值，数据框，取子集等一系列特性。

* R拥有一个相当活跃的社区文化。你很容易直接通过[R-help](https://stat.ethz.ch/mailman/listinfo/r-help)邮箱列表，[stackoverflow](http://stackoverflow.com/questions/tagged/r)或者一些有特定主题的邮箱列表例如[R-SIG-mixed-models](https://stat.ethz.ch/mailman/listinfo/r-sig-mixed-models) 和 [ggplot2](https://groups.google.com/forum/#!forum/ggplot2)得到那些R专家的帮助。你也可以和其他R使用者通过[twitter](https://twitter.com/search?q=%23rstats)，[linkedin](http://www.linkedin.com/groups/R-Project-Statistical-Computing-77616)或者当地的[用户群](http://blog.revolutionanalytics.com/local-r-groups.html)建立社交关系。

* R让你能很方便的和别人交流你的分析结果。R包让你很轻松的将你的分析结果输出成html或者pdf的[报告](http://yihui.name/knitr/)，甚至生成[动态的交互网页](http://www.rstudio.com/shiny/)。

* R语言的设计基于函数式编程理论。函数式编程的思想适合解决绝大部分的数据分析问题。R提供强大而又灵活的工具让你编写精确而有高可读性的代码。

* R拥有一个专门为交互数据分析和统计编程而打造的[IDE](http://www.rstudio.com/ide/).

* R拥有强大的元编成架构。R不紧紧是一门编程语言，也是一个交互数据分析的环境。R的元编成能力让你能编写魔法般地简洁明了的函数，同时提供一个设计专用领域语言的完美环境。

* R在设计上对像C，Fortran，和C++一类的高性能语言开放接口。

当然，R语言也并不是完美的。对于使用R语言的用户来说，他们其中大多数并不是程序员背景。这意味着：

* 很多你在网络上搜寻到的R代码可能是为某个紧迫的问题而匆忙写成的。这样的代码不够完美，也没有做优化，并且可能很难理解。很多的用户在写成代码后并不会去回头解决以上的问题。

* 和其他语言相比，R社区讨论的问题更多的集中在问题的结果上而不是注重处理问题的过程。关于软件工程方面的的相关实践应用算只能算是凑合：比如很多的R程序员都不会用源代码控制和自动测试。

* 元编程是一把双刃剑。很多的R函数用一些技巧来减少代码量，然而牺牲了可读性，并且可能导致非预期的结果。

* R语言有20多年的进化历史，在发布的不同的R工具包甚至base R中都普遍存在着变量或函数名前后不一致性的情况。这使得学习R语言需要记住不同情况下的不同函数调用方法，从而加大了学习难度。

* R并不是一门注重运行速度的语言，糟糕的R代码会使得其运行速度极其缓慢。另外R语言占用内存也是很严重的。

在我个人看来，R语言当前所遇到的这些挑战对于有经验的程序员来说是一个很大的机会，他们大可以施展拳脚来推动R语言和R社区向更加成熟和完善的方向发展。R用户都很在乎写出高质量的代码，尤其是在做可重复性研究的时候，可是他们缺少相关的知识。我希望本书不仅能帮助更多的R用户提高能力成为高效的R程序员，同时也希望鼓励更多的使用其他语言的专业程序员来为R语言的发展做出贡献。


## 这本书适合谁
这本书适合以下两类读者：

* 希望更深入探索R并且了解解决不同问题的新方法的R中级程序员。

* 正在学习R并且想要知道R工作原理的其他编程语言的程序员。

为了更好的使用这本书，你需要一定的R语言或其他语言的编程经历。你不需要了解所有R的知识，但你需要知道函数是如何在R中运行的，尽管你可能感觉熟练地驾驭R函数还有些吃力。同时你也应该熟悉apply家族函数的使用方法，比如`apply()`和`lapply()`.


##你会从本书学到什么

这本书包括了我认为一个高阶的R程序员需要掌握的技能：能写出在不同环境下运行的高质量代码。

读完本书，你将：

* 熟悉R的基本知识。你会理解复杂的数据类型并且知道处理这些数据类型相应的最佳方法。你将对R函数是如何运行的有更深入的理解，并且能够理解和使用R的四种面向对象体系。

* 理解什么是函数式编程，以及它为什么适合于数据分析。你将会快速的掌握如何运用已有的工具，并且能够编写你需要的新工具。

* 了解元编程的双刃剑特性。你将会在规则范围内编写非标准评估的函数来节省代码，并且写出能处理复杂操作的华丽代码。你也会明白运用元编程的风险，并且知道为什么要小心的使用元编程。

* 能对一切运行缓慢或耗用大量内存的R操作建立直觉的认知。你会知道如何剖析运行过程来准确定位运行代码中的瓶颈之处，并且学习够用的C++知识来将缓慢的R代码转换成快速的C++代码。

* 能够轻松的读懂大部分的R代码。你会学到R的一些称用术语（尽管你可能不会用到）并且能够评价他人的代码。

##元技能

有两种对提高R程序员能力极其有用的元技能：**读源代码**和**科研的心态**。

读源代码很重要。因为它能帮助你写出更好的代码。发展这个技能最佳的方法是查看你常用的函数和工具包的源代码。你会发现一些值得在你的代码中模仿的技巧并且对好的代码培养欣赏能力。你也会发现一些不好的代码，比如代码的效率不明显，或则违反了你的直觉认识。然而阅读这些不好的代码也是有用的，它会帮助提练你辨别好代码和坏代码的能力。

有一个科研的心态对学习R非常you帮助。如果你不明白一个东西是怎么工作对，提出假设，设计一个实验，运行并且记录报告。这种练习是非常有用的。因为如果你不能够明白某个问题并且需要帮助，你能够很轻松的告诉别人你做了哪些尝试。同时当你理解正确答案的时候，你会在潜意识里更新你的认知。当我仔细的向别人描述我的问题的时候（做可重复性[案例](http://stackoverflow.com/questions/5963269)的艺术），我经常在描述的过程中自己找到了问题的答案。

##推荐阅读

R仍然是一个比较年轻的语言，帮助你学习R的资源还在不断完善中。在我个人学习R的过程中，我发现关于其他编程语言的资源对于学习R特别有帮助。R语言同时拥有函数式编程和面向对象编程的特性。学习这些特性在R语言中的体现方式能帮助你巩固其他语言相关的知识，同时帮助你发现哪些地方你还有进步的空间。

我发现Harold Abelson和Gerald Jay Sussman写的[《The Structure and Interpretation of Computer Programs》](http://mitpress.mit.edu/sicp/full-text/book/book.html)(SICP) 这本书对于理解R的面相对象系统非常有帮助。读完这本书，我第一次发现我也可以设计自己的一套面相对象系统。这本书是我学习R中面向对象编程的第一本指导书。SICP帮助我理解了面相对象的优缺点。同时这本书也介绍了很多关于函数式编程知识，以及如何将简单的函数组合其他实现复杂的功能。

为了明白R相对于其他编程语言所作出的改变和妥协， Peter van Roy和Sef Haridi 写的[《Concepts, Techniques and Models of Computer Programming》](http://amzn.com/0262220695?tag=devtools-20) 这本书让我获益匪浅。这本书让我领会到R中修改再拷贝的语法极大地提高了R语言的直观性和可读性。尽管R语言在这一点目前还不算成熟，但是这在今后一定会被完善。

如果你向学习成为一个更优秀的程序员，你一定要读Andrew Hunt和David Thomas所写的[《The Pragmatic Programmer》](http://amzn.com/020161622X?tag=devtools-20)。这是一本语言不可被认知论的书，对于如何成为更优秀的程序员提供了相当多的宝贵建议。

##如何获得帮助

当你在学习和运用R语言的过程中卡住或者找不出问题的原因，目前有两种主要的获得帮助的途径：[stackoverflow](http://stackoverflow.com/)和R-help邮箱列表。这两种途径各具特色，两者你都能从中得到极有效的帮助。但是在提出问题前，最好先了解一下各平台的背景和要求。

一点建议：

* 确保你使用的R环境和R工具包是最新版本。因为有可能你遇到的问题已经在新的版本中得到了解决。

* 花点时间创建一个[可重复性的案例](http://stackoverflow.com/questions/5963269)。这将会是非常有帮助的，有可能你在创建的过程中就找出了问题所在。

* 在提问前先搜索一下相关的问题。如果你能找到别人提出的同样的问题并且这个问题已经被解答了，那使用以解决的答案对你无疑是最快速的。


##致谢

我要谢谢R-help那些不厌其烦的贡献者以及[stackoverflow](http://stackoverflow.com/questions/tagged/r)。有很多需要感谢的不能一一列举，但是在这其中我要特别谢谢Luke Tierney，John Chambers，Dirk Eddelbuettel，JJ Allaire和Brian Ripley花费的宝贵时间来帮助更正我不计其数的错误理解。

这本书是完全开源完成的，每一章节都是在[twitter](https://twitter.com/hadleywickham)上公告后完成的。这本书是整个社区的成果：很多人参与草稿阅读，纠正拼写错误，提出改进建议，并且提供新的内容。没有这些帮助，这本书不会像现在这样完善，我由衷的感谢这些帮助。特别谢谢Peter Li，他一页一页的度过了所有的内容，并且纠正了很多的内容。其他特别贡献者包括Aaron Schumacher，@crtahlin，Lingbing Feng，@juancentro和 @johnbaums。

按照字母顺序列下所有需要感谢的人： Aaron Schumacher, @aaronwolen, Aaron Wolen, @absolutelyNoWarranty, Adam Hunt, @agrabovsky, @ajdm, @alexbbrown, @alko989, @allegretto, @AmeliaMN, @andrewla, Andy Teucher, Anthony Damico, Anton Antonov, @aranlunzer, @arilamstein, @avilella, @baptiste, @blindjesse, @blmoore, @bnjmn, Brandon Hurr, @BrianDiggs, @Bryce, @Carson, @cdrv, Ching Boon, @chiphogg, Christopher Brown, @christophergandrud, C. Jason Liang, Clay Ford, @cornelius1729, @cplouffe, Craig Citro, @crossfitAL, @crowding, @crtahlin, Crt Ahlin, @cscheid, @csgillespie, @cusanovich, @cwarden, @cwickham, Daniel Lee, @darrkj, @Dasonk, David Hajage, David LeBauer, @dchudz, dennis feehan, @dfeehan, Dirk Eddelbuettel, @dkahle, @dlebauer, @dlschweizer, @dmontaner, @dougmitarotonda, @dpatschke, @duncandonutz, @EdFineOKL, @EDiLD, @eipi10, @elegrand, @EmilRehnberg, Eric C. Anderson, @etb, @fabian-s, Facundo Muñoz, @flammy0530, @fpepin, Frank Farach, @freezby, @fyears, @garrettgman, Garrett Grolemund, @gavinsimpson, @gggtest, Gökçen Eraslan, Gregg Whitworth, @gregorp, @gsee, @gsk3, @gthb, @hassaad85, @i, Iain Dillingham, @ijlyttle, Ilan Man, @imanuelcostigan, @initdch, Jason Asher, @jasondavies, Jason Knight, @jastingo, @jcborras, Jeff Allen, @jeharmse, @jentjr, @JestonBlu, @JimInNashville, @jinlong25, JJ Allaire, Jochen Van de Velde, Johann Hibschman, @johnbaums, John Blischak, @johnjosephhorton, John Verzani, Joris Muller, Joseph Casillas, @juancentro, @kdauria, @kenahoo, @kent37, Kevin Markham, Kevin Ushey, @kforner, Kirill Müller, Kun Ren, Laurent Gatto, @Lawrence-Liu, @ldfmrails, @lgatto, @liangcj, Lingbing Feng, @lynaghk, Maarten Kruijver, Mamoun Benghezal, @mannyishere, @mattbaggott, Matthew Grogan, @mattmalin, Matt Pettis, @michaelbach, Michael Kane, @mjsduncan, @Mullefa, @myqlarson, Nacho Caballero, Nick Carchedi, @nstjhp, @ogennadi, Oliver Keyes, @otepoti, Parker Abercrombie, @patperu, Patrick Miller, @pdb61, @pengyu, Peter F Schulam, Peter Lindbrook, Peter Meilstrup, @philchalmers, @picasa, @piccolbo, @pierreroudier, @pooryorick, @ramnathv, Ramnath Vaidyanathan, @Rappster, Ricardo Pietrobon, Richard Cotton, @richardreeve, R. Mark Sharp, @rmflight, @rmsharp, Robert M Flight, @RobertZK, @robiRagan, Romain François, @rrunner, @rubenfcasal, @sailingwave, @sarunasmerkliopas, @sbgraves237, @scottko, @scottl, Scott Ritchie, Sean Anderson, Sean Carmody, Sean Wilkinson, @sebastian-c, Sebastien Vigneau, @shabbychef, Shannon Rush, Simon O’Hanlon, Simon Potter, @SplashDance, @ste-fan, Stefan Widgren, @stephens999, Steven Pav, @strongh, @stuttungur, @surmann, @swnydick, @taekyunk, @talgalili, Tal Galili, @tdenes, @Thomas, @thomasherbig, @thomaszumbrunn, Tim Cole, @tjmahr, Tom Buckley, Tom Crockett, @ttriche, @twjacobs, @tyhenkaline, @tylerritchie, @ulrichatz, @varun729, @victorkryukov, @vijaybarve, @vzemlys, @wchi144, @wibeasley, @WilCrofter, William Doane, Winston Chang, @wmc3, @wordnerd, Yoni Ben-Meshulam, @zackham, @zerokarmaleft, Zhongpeng Lin.

##本书的一些惯例

这本书中`f()`表示函数，`g`表示变量和函数的参数。`h/`表示路径。

大的代码段包括输入和输出。输出是被特别注释的。如果你用的是本书的电子版比如`http://adv-r.had.co.nz`，你能轻松的复制粘贴代码到R中运行。输出是用`#>`特别注释的，以此和常用注释分别开来。

##题记

这本书是在[Rstudio](http://www.rstudio.com/ide/)中用[Rmarkdown](http://rmarkdown.rstudio.com/)写成，然后用[knitr](http://yihui.name/knitr/)和[pandoc](http://johnmacfarlane.net/pandoc/) 转换成html和pdf格式。这个[网页](http://adv-r.had.co.nz/)
是采用[bootstrap](http://getbootstrap.com/)
模板使用[jekyll](http://jekyllrb.com/)搭建, 并且用[travis-ci](https://travis-ci.org/)自动上传到到Amazon的 S3[
](http://aws.amazon.com/s3/)。本书完整的文本在[github](https://github.com/hadley/adv-r)
上开源。

代码格式为[inconsolata](http://levien.com/type/myfonts/inconsolata.html).

