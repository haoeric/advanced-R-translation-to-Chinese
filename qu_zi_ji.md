# 取子集{#subsetting}

R的取子集操作非常快捷灵活。掌握R中的取子集能让你用简洁的方式对数据进行复杂的操作，这是其他语言所望成莫及的。R的取子集不是那么容易学习，这之前你需要先了解几个相关的概念：

* 三个取子集操作符。

* 六种取子集的索引方法。

* 对不同数据类型（比如向量，列表，因子，矩阵和数据框）取子集结果的不同。

* 取子集和任务分派的结合使用。

这章将帮助你一步一步掌握R的取子集操作。首先我们从最简单的取子集（即使用`[`对原子向量取子集）开始讲解，然后慢慢展开，学习对较复杂的数据结构比如数组和列表取子集以及使用其他取子集操作符`[[`和`$`。接下来会讲解如何结合取子集和任务分派来修改对象的内容。最后我们来看看一些有用的取子集应用实例。

取子集是`str()`函数的补充。`str()`函数帮助你了解对象的数据结构，取子集让你从对象中提取感兴趣的数据片段。

##### 测试

做做这个简单的测试看看你是否需要阅读本章内容。如果你能很快地得到答案，你可以轻松地跳过本章。本章最后提供[参考答案](#subsetting-answers)。

1.  使用正数索引，负数索引，逻辑向量索引以及字符串向量索引取子集分别有什么不同？

1.  对列表使用`[`，`[[`或则`$`有什么不同？

1.  什么时候需要使用`drop = FALSE`？

1.  如果`x`是一个矩阵，`x[] <- 0`会得到什么结果，这和`x <- 0`的结果有什么不同？

1.  如何使用有名字的向量来重新标记分类变量？

##### 概要

* [数据结构](#data-types) 首先介绍如何使用`[`以及六种对原子向量取子集的索引方法。然后讲解如何将这六种索引方法应用到列表，矩阵，数据框和S3对象。 
  
* [取子集操作符](#subsetting-operators) 介绍另外两种取子集操作符`[[`和`$`，着重介绍简化与保留的原则。
  
* In [取子集和任务分派](#subassignment) you'll learn the
  art of subassignment, combining subsetting and assignment to modify parts 
  of an object.
  
* [实例运用](#applications) leads you through eight important, but
  not obvious, applications of subsetting to solve problems that you
  often encounter in a data analysis.

## 索引类型 {#data-types}

学习原子向量的取子集是最简单的，原子向量的取子集操作可以很容易地被引申运用到高维和其他更复杂的数据结构。这里我们将从最常用的取子集操作符`[`开始讲解。后面的[取子集操作符](#subsetting-operators)一节会介绍另外两种操作符，`[[`和`$`。

### 原子向量

以下用一个简单的向量`x`来讲解不同的取子集方式。

```r
x <- c(2.1, 4.2, 3.3, 5.4) ＃注意：小数点后面的数实际标明了向量中元素的位置。
```

你可以用如下六种索引方式对一个向量进行取子集操作：

*   __正整数索引__ 返回向量中特定位置的元素： 
    ```r
    x[c(3, 1)]
    x[order(x)]

    # 重复的索引返回重复的值
    x[c(1, 1)]

    # 实数默认被去尾为整数
    x[c(2.1, 2.9)]
    ```

*   __负整数索引__ 去除向量中特定位置的元素：

    ```r
    x[-c(3, 1)]
    ```

    正整数和负整数可以在同一个取子集操作中结合使用：
    ```r
    x[c(-1, 2)]
    ```

*   __逻辑向量索引__ 选择对应值为`TRUE`的元素。这可能是最有用的取子集操作，因为你在代码中常常得到逻辑向量。

    ```r
    x[c(TRUE, TRUE, FALSE, FALSE)]
    x[x > 3]
    ```

    如果实用的逻辑向量的长度比被取子集的向量长度短，逻辑向量会被_循环_到与该向量相同的长度。

    ```r
    x[c(TRUE, FALSE)]
    # 等同于
    x[c(TRUE, FALSE, TRUE, FALSE)]
    ```

    索引中如果出现缺失值，结果中也会对应返回缺失值：

    ```r
    x[c(TRUE, TRUE, NA, FALSE)]
    ```

*   __空索引__ 返回原子向量。这对向量取子集没有什么用处，可是对于矩阵，数据框和数组却非常有用。并且还可以和任务分派联合使用。

    ```r
    x[]
    ```

*   __零索引__ 返回一个长度为零的向量。这个不常用，但是可以用来生成测试数据。

    ```r
    x[0]
    ```

*   __字符串向量索引__ 如果向量有名字，你也可以使用字符串向量索引返回与名字相同的元素：

    ```r
    (y <- setNames(x, letters[1:4]))
    y[c("d", "c", "a")]

    # 和整数索引一样，你也可以使用重复字符串
    y[c("a", "a", "a")]

    # 使用[取子集时，名字必须是完全匹配的
    z <- c(abc = 1, def = 2)
    z[c("a", "d")]
    ```

### 列表

对列表取子集与对原子向量原理相同。使用`[`将会始终返回一个向量；后面要讲解的`[[`和`$`则会提取一个向量中的元素。

### 矩阵和数组 {#matrix-subsetting}

可以使用如下三种方法对高维数据取子集：

* 多向量
* 单向量
* 矩阵

最常用的对矩阵和数组取子集就是对一维向量取子集的简单衍生：对每一个维度提供一个用逗号彼此隔开的索引。空索引这时就有用处了，它意味着保留所有行，或者所有列。

```r
a <- matrix(1:9, nrow = 3)
colnames(a) <- c("A", "B", "C")
a[1:2, ]
a[c(T, F, T), c("B", "A")]
a[0, -2]
```

默认情况下，使用`[`会对结果进行简化和降维。查看[简化与保留](#simplify-preserve)一节来学习如何避免这种情况。

因为矩阵和数组是由带特殊属性的向量构建成的，这也就意味着你也可以使用一个简单的向量来对它们进行取子集。这样它们会被视为数组。在R中数组是按列优先顺序排列存储的：

```r
(vals <- outer(1:5, 1:5, FUN = "paste", sep = ","))
vals[c(4, 15)]
```

你也可以使用整形矩阵来对高维数据进行取子集（如果高维数据有名字属性，也可以使用字符串类矩阵）。矩阵中的每一行标明一个元素在高维数据中的坐标，每一列则对应着该高维数据的一个维度。也就是说，你要使用一个两列的矩阵来对一个矩阵取子集，一个三列的矩阵来对一个三维数组取子集，依此类推。它们输出的结果是一个向量：

```r
vals <- outer(1:5, 1:5, FUN = "paste", sep = ",")
select <- matrix(ncol = 2, byrow = TRUE, c(
  1, 1,
  3, 1,
  2, 4
))
vals[select]
```

### 数据框 {#df-subsetting}

数据框同时拥有列表和矩阵的特性：如果你用单个向量来取子集，那么数据框就表现为列表；如果使用两个向量，数据框则表现为矩阵。

```r
df <- data.frame(x = 1:3, y = 3:1, z = letters[1:3])

df[df$x == 2, ]
df[c(1, 3), ]

# 有两种方法对一个数据框的列取子集
# 同列表一样
df[c("x", "z")]
# 同矩阵一样
df[, c("x", "z")]

# 如果紧取数据框的一列：使用同矩阵一样的方法则返回值会被简化## 为向量，但是使用同列表一样方法则不会简化。
str(df["x"])
str(df[, "x"])
```

### S3对象

S3对象是由原子向量，数组和列表构成的，因此你可以使用上面介绍的方法以及`str()`的帮助来对S3对象取子集。

### S4对象

对于S4对象有另外的两种取子集的操作符：`@`(等同于`$`)和`slot()`(等同于`[[`)。`@`相对于`$`更严谨，如果对应所取位置不存在则会报错。这在[面相对象指南](#s4)一章中会详细介绍。

### 练习

1.  找出并修改如下代码中的错误：

    ```r
    mtcars[mtcars$cyl = 4, ]
    mtcars[-1:4, ]
    mtcars[mtcars$cyl <= 5]
    mtcars[mtcars$cyl == 4 | 6, ]
    ```

1.  为什么`x <- 1:5; x[NA]`会返回五个缺失值？（提示：这和`x[NA_real_]`有什么不同)

1.  `upper.tri()`返回什么值？使用它对矩阵取子集是如何操作的？我们需要其他的取子集原则来描述它么？

    ```r
    x <- outer(1:5, 1:5, FUN = "*")
    x[upper.tri(x)]
    ```

1.  为什么`mtcars[1:20]`会报错，它和`mtcars[1:20, ]`有什么不同？

1.  自己编写一个对矩阵取对角元素的函数（要和对矩阵`x`使用`diag(x)`的返回值相同）。

1.  `df[is.na(df)] <- 0`做了什么操作，怎么解释这个代码？


## 取子集操作符 {#subsetting-operators}

另外两种取子集操作符分别是`[[`和`$`。`[[`与`[`相似，使用`[[`可以提取列表中的元素，但是每次只能返回单个元素。`$`可以看做是`[[`的简化，同时它还能结合字符串取子集。

对列表使用`[`返回值始终是一个列表，然而使用`[[`则返回列表中的元素。因此，提取列表中的元素时要使用`[[`:

> “如果列表`x`是一个满载货物的火车，`x[[5]]`表示在第五节车厢
> 中的货物，而`x[4:6]`则表示由四，五，六号车厢组成的小火车。”
>                                               --- @RLangTip

因为使用`[[`只能返回单个值，所以使用的索引必须是正整数或者字符串。

```r
a <- list(a = 1, b = 2)
a[[1]]
a[["a"]]

# 如果[[里是一个向量则会迭代索引
b <- list(a = list(b = list(c = list(d = 1))))
b[[c("a", "b", "c", "d")]]
# 等同于
b[["a"]][["b"]][["c"]][["d"]]
```

因为数据框本质上是由多个列向量构成的列表，所以你也可以使用`[[`来提取数据框中的某一列，比如`mtcars[[1]]`，`mtcars[["cyl"]]`。

使用`[`或`[[`对S3和S4对象进行操作时，他们的结果会因受对象的重写而不同。关键的不同在于简化与保留。所以知道什么是默认操作很重要。


### 简化与保留 {#simplify-preserve}

理解简化与保留的不同非常重要。对结果进行简化会将输出信息转化为最简单的数据结构。简化有时候很有用，因为很多时候简化后的返回值会恰好是你想要的结构。对结构进行保留则会保证输出与输入的数据结构类型一致，这对提高程序的稳定性非常重要。在对矩阵和数据框取子集时忽略`drop = FALSE`是导致程序出错的一种常见原因。（可能在你的测试数据中不会有错误，当别人输入单列的数据框时则会出现错误）

如何切换简化或则保留因数据类型的差异而不同。具体的操作概括如下表：

|         |           简化            |      保留   |
|---------|---------------------------|---------------|
| 向量    | `x[[1]]`                  | `x[1]`        |
| 列表    | `x[[1]]`                  | `x[1]`        |
| 因子    | `x[1:4, drop = T]`        | `x[1:4]`      |
| 数组    | `x[1, ]` __or__ `x[, 1]`  | `x[1, , drop = F]` __or__ `x[, 1, drop = F]` |
| 数据框  | `x[, 1]` __or__ `x[[1]]`  | `x[, 1, drop = F]` __or__ `x[1]`             |

保留操作对于所有数据类型都是一样的：你得到和输入同样类型的输出。简化操作则对不同的数据类型会有些不同：

*   __原向量__：去除名字。

    ```r
    x <- c(a = 1, b = 2)
    x[1]
    x[[1]]
    ```

*   __列表__：返回列表中的元素而不是单个元素的列表。

    ```r
    y <- list(a = 1, b = 2)
    str(y[1])
    str(y[[1]])
    ```

*   __因子__：去掉多余的水平。

    ```r
    z <- factor(c("a", "b"))
    z[1]
    z[1, drop = TRUE]
    ```

*   __矩阵__或__数组__：去掉长度为一的维度。

    ```r
    a <- matrix(1:4, nrow = 2)
    a[1, , drop = FALSE]
    a[1, ]
    ```

*   __数据框__：若返回值是单列，则返回一个向量而不是数据框。 

    ```r
    df <- data.frame(a = 1:2, b = 1:2)
    str(df[1])
    str(df[[1]])
    str(df[, "a", drop = FALSE])
    str(df[, "a"])
    ```

### `$`

`$`是一个简化操作符，`x$y`等同于`x[["y", exact = FALSE]]`。多用于对数据框取子集，比如`mtcars$cyl`和`diamonds$carat`。

使用`$`的一个常用错误是使用一个变量替代某一列的名字：

```r
var <- "cyl"
# mtcars$var等同于mtcars[["var"]]，这样返回null
mtcars$var

# 换用[[
mtcars[[var]]
```

`$`和`[[`使用上最大的不同是，`$`采用不完整配对：

```r
x <- list(abc = 1)
x$a
x[["a"]]
```

你可以修改全域设置，将`warnPartialMatchDollar`设为`TRUE`来避免这种操作。但是小心这样设置给其他导入代码（比如其他包中的代码）带来的影响。

### 缺失索引与出界索引

当使用的索引超出范围(OOB)时，使用`[`和`[[`会表现的有所不同。比如，你试图提取一个长度为四的向量的第五个元素，或则使用`NA`或`NULL`作为索引：

```r
x <- 1:4
str(x[5])
str(x[NA_real_])
str(x[NULL])
```

下面的表格归纳了在对向量或列表使用`[`和`[[`时，当出现出界索引(OOB)或缺失索引时结果的差异：

| 操作符   |    索引     |   原子向量  |     列表      |
|----------|-------------|-------------|---------------|
| `[`      | OOB         | `NA`        | `list(NULL)`  |
| `[`      | `NA_real_`  | `NA`        | `list(NULL)`  |
| `[`      | `NULL`      | `x[0]`      | `list(NULL)`  |
| `[[`     | OOB         | Error       | Error         |
| `[[`     | `NA_real_`  | Error       | `NULL`        |
| `[[`     | `NULL`      | Error       | Error         |

如果输入向量有名字，那么出界索引(OOB)或缺失索引的名字为`"<NA>"`。

```r
numeric()[1]
numeric()[NA_real_]
numeric()[NULL]
numeric()[[1]]
numeric()[[NA_real_]]
numeric()[[NULL]]

list()[1]
list()[NA_real_]
list()[NULL]
list()[[1]]
list()[[NA_real_]]
list()[[NULL]]
```

### 练习

1.  比如一个线性模型`mod <- lm(mpg ~ wt, data = mtcars)`，如何对它提取模型中的残余自由度，如何提取`summary(mod)`中的R平方值。

<!-- FIXME: more examples -->

## 取子集与任务分派 {#subassignment}

所有的取子集操作都可以和任务分派结合起来对输入的向量进行选择性地修改。

```r
x <- 1:5
x[c(1, 2)] <- 2:3
x

# LHS的长度必须和RHS一致
x[-1] <- 4:1
x

# 注意：重复的索引不会被除掉，会覆盖前面的赋值
x[c(1, 1)] <- 2:3
x

# 整型索引不能和NA一同使用
x[c(1, NA)] <- c(1, 2)
# 但是NA可以和逻辑索引一同使用 (这时，NA会被视为false)
x[c(T, F, NA)] <- 1
x

# 这对修改向量中修改符合某种条件的元素很有用处
df <- data.frame(a = c(1, 10, NA))
df$a[df$a < 5] <- 0
df$a
```

使用空索引取子集搭配任务分派能保有原对象的类型和结构。比较如下两行代码。第一行中`mtcars`将保持原类型为数据框，而第二行中`mtcars`将成为一个列表。

```r
mtcars[] <- lapply(mtcars, as.integer)
mtcars <- lapply(mtcars, as.integer)
```

对于列表，可以使用取子集＋任务分派＋`NULL`来去除向量中的某个特定元素。如果要添加一个`NULL`到一个列表，则可以使用`[`和`list(NULL)`：

```r
x <- list(a = 1, b = 2)
x[["b"]] <- NULL
str(x)

y <- list(a = 1)
y["b"] <- list(NULL)
str(y)
```

## 实例运用 {#applications}

上面介绍的取子集的基础知识能够被应用到很多的场景中。以下我们会介绍其中最重要的几个运用。有些特定的运用虽然有对应的专门的函数（比如，`subset()`, `merge()`, `plyr::arrange()`），但是了解这些函数是如何通过基础取子集操作来实现的对我们非常有帮助。这让我们能够应对那些没有专门函数来处理的新环境。

### 查寻表 (字符串取子集) {#lookup-tables}

字符匹配为制作查询表提供了一个强大的机制。比如你想转换一些缩写：

```r
x <- c("m", "f", "u", "f", "f", "m", "m")
lookup <- c(m = "Male", f = "Female", u = NA)
lookup[x]
unname(lookup[x])

# 或者更简单的输出
c(m = "Known", f = "Known", u = "Unknown")[x]
```

如果不想在结果汇总出现名字，你可以使用`unname()`来把它们去掉。


### 手动匹配和融合 (整形取子集) {#matching-merging}

你可能有一个更复杂的多列的查询表。比如我们有一个表示成绩的向量，和一个描述它的特性表：

```r
grades <- c(1, 2, 2, 3, 1)

info <- data.frame(
  grade = 3:1,
  desc = c("Excellent", "Good", "Poor"),
  fail = c(F, F, T)
)
```

我们想要得到每个成绩在特性表中对应的信息。我们有两种途径来获得，一种是使用`match()`做整形取子集，另外一种是使用`rownames()`做字符串取子集： 

```r
# 使用 match
id <- match(grades, info$grade)
info[id, ]

# 使用 rownames
rownames(info) <- info$grade
info[as.character(grades), ]
```

如果你有多列需要匹配，那么你需要先使用`interaction()`，`paste()`或者`plyr::id()`将它们转换成单列。你也可以使用`merge()`或`plyr::join()`来做同样的事。请查看对应函数的源代码来学习如何实现。

### 随机取样/自助法 (整型取子集)

你可以使用整形索引来对一个向量或者数据框进行随机取样和自助取样。首先使用`sample()`函数生成一个随机索引向量，然后对对象取子集。 

```r
df <- data.frame(x = rep(1:3, each = 2), y = 6:1, z = letters[1:6])

# 为可重复性操作设置种子
set.seed(10)

# 随机重排
df[sample(nrow(df)), ]
# 随机取3排
df[sample(nrow(df), 3), ]
# 取6个自助样本
df[sample(nrow(df), 6, rep = T), ]
```
设置`sample()`函数的参数来调整取样的个数，以及是否重复取样。

### 排序 (整形取子集)

`order()`函数的输入是一个向量，返回一个存储该向量排列顺序的整型向量。

```r
x <- c("b", "c", "a")
order(x)
x[order(x)]
```

可以给`order()`函数提供额外参数来重排并列值的顺序。可以使用`decreasing = TRUE`将返回结果变成降序排列。默认情况下，缺失值会被排在最后；可以使用`na.last = NA`来去除它们，或者使用`na.last = FALSE`将它们放在最前面。

当目标对象是二维或更高维时，可以使用`order()`和整形索引来简单地对行或者列排序：

```r
# 随机重排df
df2 <- df[sample(nrow(df)), 3:1]
df2

df2[order(df2$x), ]
df2[, order(names(df2))]
```

使用`sort()`可以对向量进行排序，`plyr::arrange()`则可以对数据框排序。

### 展开汇总计数 (整型取子集)

有时候你的数据框中的重复行可能被汇总为一行，同时添加一列来标记重复的次数。可以使用`rep()`生成有重复的行整形索引来展开汇总计数：

```r
df <- data.frame(x = c(2, 4, 1), y = c(9, 11, 6), n = c(3, 5, 1))
rep(1:nrow(df), df$n)
df[rep(1:nrow(df), df$n), ]
```

### 去除数据框中的某列 (字符串取子集)

有两种方法来去除数据框中的某列。一种是将该列设为NULL：

```r
df <- data.frame(x = 1:3, y = 3:1, z = letters[1:3])
df$z <- NULL
```

另外一种是生成只包含你想要的列的新数据框：


```r
df <- data.frame(x = 1:3, y = 3:1, z = letters[1:3])
df[c("x", "y")]
```
如果你知道你不想要的列信息，使用`setdiff`筛选出你想要保留的列：

```r
df[setdiff(names(df), "z")]
```

### 有条件的行筛选 (逻辑型取子集)

因为我们能很容易地整合多列的条件判断，所以逻辑型取子集应该是对数据框进行行筛选的最常用的方法。

```r
mtcars[mtcars$gear == 5, ]
mtcars[mtcars$gear == 5 & mtcars$cyl == 4, ]
```

注意使用向量型逻辑运算符`&`和`|`， 而不是缩短的标量型逻辑运算符`&&`和`||`，`&&`和`||`在`if`条件判断中比较有用。灵活运用[德摩根定律][demorgans]可以大大简化否定的逻辑操作。

* `!(X & Y)` 等同于 `!X | !Y`
* `!(X | Y)` 等同于 `!X & !Y`

比如 `!(X & !(Y | Z))` 可以简化成 `!X | !!(Y|Z)`，更进一步成`!X | Y | Z`。

`subset()`是专门用来对数据框取子集的速记函数。使用`subset()`可以免掉重复输入数据框的名字从而节省代码。在[非标准评估](#nse)一章，你会学习`subset()的工作原理。

```{r}
subset(mtcars, gear == 5)
subset(mtcars, gear == 5 & cyl == 4)
```

### 逻辑运算 vs. 集合运算 (逻辑型 & 整形取子集)

认识逻辑运算（逻辑型取子集）和集合运算（整型取子集）本质上的相同点非常有用，而使用集合运算更为高效：

* 你想知道第一个（或最后一个）`TRUE`。
* 你有很多的`FALSE`却比较少的`TRUE`；使用集合运算更快更节省内存。

`which()`可以帮助你将逻辑型转换为整形表示。在基础R中没有`which()`的逆操作，但是我们可以很容易的编写一个：

```r
x <- sample(10) < 4
which(x)

unwhich <- function(x, n) {
  out <- rep_len(FALSE, n)
  out[x] <- TRUE
  out
}
unwhich(which(x), 10)
```

我们创建两个逻辑型向量和对应的整型向量来探索一下逻辑运算和集合运算之间的关系。

```r
(x1 <- 1:10 %% 2 == 0)
(x2 <- which(x1))
(y1 <- 1:10 %% 5 == 0)
(y2 <- which(y1))

# X & Y <-> intersect(x, y)
x1 & y1
intersect(x2, y2)

# X | Y <-> union(x, y)
x1 | y1
union(x2, y2)

# X & !Y <-> setdiff(x, y)
x1 & !y1
setdiff(x2, y2)

# xor(X, Y) <-> setdiff(union(x, y), intersect(x, y))
xor(x1, y1)
setdiff(union(x2, y2), intersect(x2, y2))
```

刚开始学习取子集的一个常见错误是使用`x[which(y)]`而不是`x[y]`。这里的`which()`没有什么意义：它将逻辑型转换为整形索引，可是结果确实完全一样的。同时注意`x[-which(y)]`__不__等同于`x[!y]`：当`y`全是FALSE时，`which(y)`会返回`integer(0)`，那么`-integer(0)`依然是`integer(0)`，因此你会得到空值而不是所有的值。因此，除非你确实需要（比如提取第一个或最后一个`TRUE`值），尽量避免将逻辑型取子集转换为整型取子集。

### 练习

1.  你如何随机的打乱一个数据框的列？（这在随机深林方法中是非常重要的一步）你又如何同时将数据框的行和列打乱？ 

1.  How would you select a random sample of `m` rows from a data frame? 
    What if the sample had to be contiguous (i.e., with an initial row, a 
    final row, and every row in between)?
    
1.  How could you put the columns in a data frame in alphabetical order?

## 参考答案 {#subsetting-answers}

1.  Positive integers select elements at specific positions, negative integers
    drop elements; logical vectors keep elements at positions corresponding to
    `TRUE`; character vectors select elements with matching names.
   
1.  `[` selects sub-lists. It always returns a list; if you use it with a
    single positive integer, it returns a list of length one. `[[` selects 
    an element within a list. `$` is a convenient shorthand: `x$y` is
    equivalent to `x[["y"]]`.

1.  Use `drop = FALSE` if you are subsetting a matrix, array, or data frame 
    and you want to preserve the original dimensions. You should almost 
    always use it when subsetting inside a function.
   
1.  If `x` is a matrix, `x[] <- 0` will replace every element with 0, 
    keeping the same number of rows and columns. `x <- 0` completely 
    replaces the matrix with the value 0.
    
1.  A named character vector can act as a simple lookup table: 
    `c(x = 1, y = 2, z = 3)[c("y", "z", "x")]`

[demorgans]: http://en.wikipedia.org/wiki/De_Morgan's_laws
