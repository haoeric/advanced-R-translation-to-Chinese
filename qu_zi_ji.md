# 取子集{#subsetting}

R的取子集操作非常快捷灵活。掌握R中的取子集能让你用简洁的方式对数据进行复杂的操作，这是其他语言所望成莫及的。R的取子集不是那么容易学习，这之前你需要先了解几个相关的概念：

* 三个取子集操作符。

* 六种取子集的索引方法。

* 对不同数据类型（比如向量，列表，因子，矩阵和数据框）取子集结果的不同。

* 取子集和分派的结合使用。

这章将帮助你一步一步掌握R的取子集操作。首先我们从最简单的取子集（即使用`[`对原子向量取子集）开始讲解，然后慢慢展开，学习对较复杂的数据结构比如数组和列表取子集以及使用其他取子集操作符`[[`和`$`。接下来会讲解如何结合取子集和分派来修改对象的内容。最后我们来看看一些有用的取子集应用实例。

取子集是`str()`函数的补充。`str()`函数帮助你了解对象的数据结构，取子集让你从对象中提取感兴趣的数据片段。

##### 测试

做做这个简单的测试看看你是否需要阅读本章内容。如果你能很快的的到答案，你可以轻松的跳过本章。本章最后提供[参考答案](#subsetting-answers)。

1.  使用正数索引，负数索引，逻辑向量索引以及字符向量索引取子集分别有什么不同？

1.  对列表使用`[`，`[[`或则`$`有什么不同？

1.  什么时候需要使用`drop = FALSE`？

1.  如果`x`是一个矩阵，`x[] <- 0`会得到什么结果，这和`x <- 0`的结果有什么不同？

1.  如何使用有名字的向量来重新标记分类变量？

##### 概要

* [数据结构](#data-types) 首先介绍如何使用`[`以及六种对原子向量取子集的索引方法。然后讲解如何将这六种索引方法应用到列表，矩阵，数据框和S3对象。 
  
* [取子集操作符](#subsetting-operators) 介绍另外两种取子集操作符`[[`和`$`， , focussing on the important
  principles of simplifying vs. preserving.
  
* In [取子集和分派](#subassignment) you'll learn the
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

*   __空索引__ 返回原子向量。这对向量取子集没有什么用处，可是对于矩阵，数据框和数组却非常有用。并且还可以和分派联合使用。

    ```r
    x[]
    ```

*   __零索引__ 返回一个长度为零的向量。这个不常用，但是可以用来生成测试数据。

    ```r
    x[0]
    ```

*   __字符向量索引__ 如果向量有名字，你也可以使用字符向量索引返回与名字相同的元素：

    ```r
    (y <- setNames(x, letters[1:4]))
    y[c("d", "c", "a")]

    # 和整数索引一样，你也可以使用重复字符
    y[c("a", "a", "a")]

    # 使用[取子集式，名字必须是完全匹配的
    z <- c(abc = 1, def = 2)
    z[c("a", "d")]
    ```

### 列表

对列表取子集与对原子向量原理相同。使用`[`将会始终返回一个向量；后面要讲解的`[[`和`$`则会提取一个向量中的元素。

### 矩阵和数组 {#matrix-subsetting}

可以使用如下三种方法对高纬数据取子集：

* 多向量
* 单向量
* 矩阵

最常用的对矩阵和数组取子集就是对一维向量取子集的简单衍生：对每一个维度提供一个用逗号彼此隔开的索引。空索引则意味着保留该行，或则该列。

```r
a <- matrix(1:9, nrow = 3)
colnames(a) <- c("A", "B", "C")
a[1:2, ]
a[c(T, F, T), c("B", "A")]
a[0, -2]
```

默认情况下，使用`[`会简化结果到最少维度。查看[简化与保留](#simplify-preserve)一节来学习如何避免这种情况。

因为矩阵和数组是由带特殊属性的向量构建成的，这也就意味着你也可以使用一个简单的向量来对它们进行取子集。这样它们会被视为数组。在R中数组是按列优先顺序排列存储的：

```r
(vals <- outer(1:5, 1:5, FUN = "paste", sep = ","))
vals[c(4, 15)]
```

你也可以使用整形矩阵来对高维数据进行取子集（如果高维数据有名字属性，也可以使用字符类矩阵）。矩阵中的每一行标明一个元素在高维数据中的坐标，每一列则对应着该高维数据的一个维度。也就是说，你要使用一个两列的矩阵来对一个矩阵取子集，一个三列的矩阵来对一个三维数组来取子集，依此类推。输出的结果是一个向量：

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

There are two other subsetting operators: `[[` and `$`. `[[` is similar to `[`, except it can only return a single value and it allows you to pull pieces out of a list. `$` is a useful shorthand for `[[` combined with character subsetting. \indexc{[[} \indexc{\$}

You need `[[` when working with lists. This is because when `[` is applied to a list it always returns a list: it never gives you the contents of the list. To get the contents, you need `[[`:

>  "If list `x` is a train carrying objects, then `x[[5]]` is
> the object in car 5; `x[4:6]` is a train of cars 4-6." 
>
> --- @RLangTip

Because it can return only a single value, you must use `[[` with either a single positive integer or a string: \index{subsetting!lists} \index{lists!subsetting}

```r
a <- list(a = 1, b = 2)
a[[1]]
a[["a"]]

# If you do supply a vector it indexes recursively
b <- list(a = list(b = list(c = list(d = 1))))
b[[c("a", "b", "c", "d")]]
# Same as
b[["a"]][["b"]][["c"]][["d"]]
```

Because data frames are lists of columns, you can use `[[` to extract a column from data frames: `mtcars[[1]]`, `mtcars[["cyl"]]`. \index{subsetting!data frames} \index{data frames!subsetting}

S3 and S4 objects can override the standard behaviour of `[` and `[[` so they behave differently for different types of objects. The key difference is usually how you select between simplifying or preserving behaviours, and what the default is.

### 简化与保留 {#simplify-preserve}

It's important to understand the distinction between simplifying and preserving subsetting. Simplifying subsets returns the simplest possible data structure that can represent the output, and is useful interactively because it usually gives you what you want. Preserving subsetting keeps the structure of the output the same as the input, and is generally better for programming because the result will always be the same type. Omitting `drop = FALSE` when subsetting matrices and data frames is one of the most common sources of programming errors. (It will work for your test cases, but then someone will pass in a single column data frame and it will fail in an unexpected and unclear way.) \indexc{drop = FALSE} \index{subsetting!simplifying} \index{subsetting!preserving}

Unfortunately, how you switch between simplifying and preserving differs for different data types, as summarised in the table below.

|             | Simplifying               | Preserving                                   |
|-------------|---------------------------|----------------------------------------------|
| Vector      | `x[[1]]`                  | `x[1]`                                       |
| List        | `x[[1]]`                  | `x[1]`                                       |
| Factor      | `x[1:4, drop = T]`        | `x[1:4]`                                     |
| Array       | `x[1, ]` __or__ `x[, 1]`  | `x[1, , drop = F]` __or__ `x[, 1, drop = F]` |
| Data frame  | `x[, 1]` __or__ `x[[1]]`  | `x[, 1, drop = F]` __or__ `x[1]`             |

Preserving is the same for all data types: you get the same type of output as input. Simplifying behaviour varies slightly between different data types, as described below:

*   __Atomic vector__: removes names.

    ```{r}
    x <- c(a = 1, b = 2)
    x[1]
    x[[1]]
    ```

*   __List__: return the object inside the list, not a single element list.

    ```{r}
    y <- list(a = 1, b = 2)
    str(y[1])
    str(y[[1]])
    ```

*   __Factor__: drops any unused levels.

    ```{r}
    z <- factor(c("a", "b"))
    z[1]
    z[1, drop = TRUE]
    ```

*   __Matrix__ or __array__: if any of the dimensions has length 1, 
    drops that dimension.

    ```{r}
    a <- matrix(1:4, nrow = 2)
    a[1, , drop = FALSE]
    a[1, ]
    ```

*   __Data frame__: if output is a single column, returns a vector instead of 
    a data frame.

    ```{r}
    df <- data.frame(a = 1:2, b = 1:2)
    str(df[1])
    str(df[[1]])
    str(df[, "a", drop = FALSE])
    str(df[, "a"])
    ```

### `$`

`$` is a shorthand operator, where `x$y` is equivalent to `x[["y", exact = FALSE]]`.  It's often used to access variables in a data frame, as in `mtcars$cyl` or `diamonds$carat`. \indexc{\$} \indexc{[[}

One common mistake with `$` is to try and use it when you have the name of a column stored in a variable:

```{r}
var <- "cyl"
# Doesn't work - mtcars$var translated to mtcars[["var"]]
mtcars$var

# Instead use [[
mtcars[[var]]
```

There's one important difference between `$` and `[[`. `$` does partial matching:

```{r}
x <- list(abc = 1)
x$a
x[["a"]]
```

If you want to avoid this behaviour you can set the global option `warnPartialMatchDollar` to `TRUE`. Use with caution: it may affect behaviour in other code you have loaded (e.g., from a package).

### 缺失索引与出界索引

`[` and `[[` differ slightly in their behaviour when the index is out of bounds (OOB), for example, when you try to extract the fifth element of a length four vector, or subset a vector with `NA` or `NULL`: \index{subsetting!with NA \& NULL} \index{subsetting!out of bounds}

```{r, error = TRUE}
x <- 1:4
str(x[5])
str(x[NA_real_])
str(x[NULL])
```

The following table summarises the results of subsetting atomic vectors and lists with `[` and `[[` and different types of OOB value.

| Operator | Index       | Atomic      | List          |
|----------|-------------|-------------|---------------|
| `[`      | OOB         | `NA`        | `list(NULL)`  |
| `[`      | `NA_real_`  | `NA`        | `list(NULL)`  |
| `[`      | `NULL`      | `x[0]`      | `list(NULL)`  |
| `[[`     | OOB         | Error       | Error         |
| `[[`     | `NA_real_`  | Error       | `NULL`        |
| `[[`     | `NULL`      | Error       | Error         |

If the input vector is named, then the names of OOB, missing, or `NULL` components will be `"<NA>"`.

```{r, eval = FALSE, echo = FALSE}
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

### Exercises

1.  Given a linear model, e.g., `mod <- lm(mpg ~ wt, data = mtcars)`, extract
    the residual degrees of freedom. Extract the R squared from the model
    summary (`summary(mod)`)

<!-- FIXME: more examples -->

## 取子集与分派 {#subassignment}

All subsetting operators can be combined with assignment to modify selected values of the input vector. \index{subsetting!subassignment} \index{assignment!subassignment}

```r
x <- 1:5
x[c(1, 2)] <- 2:3
x

# The length of the LHS needs to match the RHS
x[-1] <- 4:1
x

# Note that there's no checking for duplicate indices
x[c(1, 1)] <- 2:3
x

# You can't combine integer indices with NA
x[c(1, NA)] <- c(1, 2)
# But you can combine logical indices with NA
# (where they're treated as false).
x[c(T, F, NA)] <- 1
x

# This is mostly useful when conditionally modifying vectors
df <- data.frame(a = c(1, 10, NA))
df$a[df$a < 5] <- 0
df$a
```

Subsetting with nothing can be useful in conjunction with assignment because it will preserve the original object class and structure. Compare the following two expressions. In the first, `mtcars` will remain as a data frame. In the second, `mtcars` will become a list.

```{r, eval = FALSE}
mtcars[] <- lapply(mtcars, as.integer)
mtcars <- lapply(mtcars, as.integer)
```

With lists, you can use subsetting + assignment + `NULL` to remove components from a list. To add a literal `NULL` to a list, use `[` and `list(NULL)`: \index{lists!removing an element}

```{r}
x <- list(a = 1, b = 2)
x[["b"]] <- NULL
str(x)

y <- list(a = 1)
y["b"] <- list(NULL)
str(y)
```

## 实例运用 {#applications}

The basic principles described above give rise to a wide variety of useful applications. Some of the most important are described below. Many of these basic techniques are wrapped up into more concise functions (e.g., `subset()`, `merge()`, `plyr::arrange()`), but it is useful to understand how they are implemented with basic subsetting. This will allow you to adapt to new situations that are not dealt with by existing functions.

### Lookup tables (character subsetting) {#lookup-tables}

Character matching provides a powerful way to make lookup tables. Say you want to convert abbreviations: \index{lookup tables}

```{r}
x <- c("m", "f", "u", "f", "f", "m", "m")
lookup <- c(m = "Male", f = "Female", u = NA)
lookup[x]
unname(lookup[x])

# Or with fewer output values
c(m = "Known", f = "Known", u = "Unknown")[x]
```

If you don't want names in the result, use `unname()` to remove them.

### Matching and merging by hand (integer subsetting) {#matching-merging}

You may have a more complicated lookup table which has multiple columns of information. Suppose we have a vector of integer grades, and a table that describes their properties: \index{matching \& merging}

```{r}
grades <- c(1, 2, 2, 3, 1)

info <- data.frame(
  grade = 3:1,
  desc = c("Excellent", "Good", "Poor"),
  fail = c(F, F, T)
)
```

We want to duplicate the info table so that we have a row for each value in `grades`. We can do this in two ways, either using `match()` and integer subsetting, or `rownames()` and character subsetting: \indexc{match()}

```{r}
grades

# Using match
id <- match(grades, info$grade)
info[id, ]

# Using rownames
rownames(info) <- info$grade
info[as.character(grades), ]
```

If you have multiple columns to match on, you'll need to first collapse them to a single column (with `interaction()`, `paste()`, or `plyr::id()`).  You can also use `merge()` or `plyr::join()`, which do the same thing for you --- read the source code to see how. \indexc{merge()}

### Random samples/bootstrap (integer subsetting)

You can use integer indices to perform random sampling or bootstrapping of a vector or data frame. `sample()` generates a vector of indices, then subsetting to access the values: \indexc{sample()} \index{sampling} \index{random sampling} \index{bootstrapping}

```{r}
df <- data.frame(x = rep(1:3, each = 2), y = 6:1, z = letters[1:6])

# Set seed for reproducibility
set.seed(10)

# Randomly reorder
df[sample(nrow(df)), ]
# Select 3 random rows
df[sample(nrow(df), 3), ]
# Select 6 bootstrap replicates
df[sample(nrow(df), 6, rep = T), ]
```

The arguments of `sample()` control the number of samples to extract, and whether sampling is performed with or without replacement.

### Ordering (integer subsetting)

`order()` takes a vector as input and returns an integer vector describing how the subsetted vector should be ordered: \indexc{order()} \index{sorting}

```{r}
x <- c("b", "c", "a")
order(x)
x[order(x)]
```

To break ties, you can supply additional variables to `order()`, and you can change from ascending to descending order using `decreasing = TRUE`.  By default, any missing values will be put at the end of the vector; however, you can remove them with `na.last = NA` or put at the front with `na.last = FALSE`.

For two or more dimensions, `order()` and integer subsetting makes it easy to order either the rows or columns of an object:

```{r}
# Randomly reorder df
df2 <- df[sample(nrow(df)), 3:1]
df2

df2[order(df2$x), ]
df2[, order(names(df2))]
```

More concise, but less flexible, functions are available for sorting vectors, `sort()`, and data frames, `plyr::arrange()`. \indexc{sort()}

### Expanding aggregated counts (integer subsetting)

Sometimes you get a data frame where identical rows have been collapsed into one and a count column has been added. `rep()` and integer subsetting make it easy to uncollapse the data by subsetting with a repeated row index:

```{r}
df <- data.frame(x = c(2, 4, 1), y = c(9, 11, 6), n = c(3, 5, 1))
rep(1:nrow(df), df$n)
df[rep(1:nrow(df), df$n), ]
```

### Removing columns from data frames (character subsetting)

There are two ways to remove columns from a data frame. You can set individual columns to NULL: \index{data frames!remove columns}

```{r}
df <- data.frame(x = 1:3, y = 3:1, z = letters[1:3])
df$z <- NULL
```

Or you can subset to return only the columns you want:

```{r}
df <- data.frame(x = 1:3, y = 3:1, z = letters[1:3])
df[c("x", "y")]
```

If you know the columns you don't want, use set operations to work out which colums to keep:

```{r}
df[setdiff(names(df), "z")]
```

### Selecting rows based on a condition (logical subsetting)

Because it allows you to easily combine conditions from multiple columns, logical subsetting is probably the most commonly used technique for extracting rows out of a data frame. \index{subsetting!with logical vectors}

```{r}
mtcars[mtcars$gear == 5, ]
mtcars[mtcars$gear == 5 & mtcars$cyl == 4, ]
```

Remember to use the vector boolean operators `&` and `|`, not the short-circuiting scalar operators `&&` and `||` which are more useful inside if statements. Don't forget [De Morgan's laws][demorgans], which can be useful to simplify negations:

* `!(X & Y)` is the same as `!X | !Y`
* `!(X | Y)` is the same as `!X & !Y`

For example, `!(X & !(Y | Z))` simplifies to `!X | !!(Y|Z)`, and then to `!X | Y | Z`.

`subset()` is a specialised shorthand function for subsetting data frames, and saves some typing because you don't need to repeat the name of the data frame. You'll learn how it works in [non-standard evaluation](#nse). \indexc{subset()}

```{r}
subset(mtcars, gear == 5)
subset(mtcars, gear == 5 & cyl == 4)
```

### Boolean algebra vs. sets (logical & integer subsetting)

It's useful to be aware of the natural equivalence between set operations (integer subsetting) and boolean algebra (logical subsetting). Using set operations is more effective when: \index{Boolean algebra} \index{set algebra}

* You want to find the first (or last) `TRUE`.

* You have very few `TRUE`s and very many `FALSE`s; a set representation 
  may be faster and require less storage.

`which()` allows you to convert a boolean representation to an integer representation. There's no reverse operation in base R but we can easily create one: \indexc{which()}

```{r}
x <- sample(10) < 4
which(x)

unwhich <- function(x, n) {
  out <- rep_len(FALSE, n)
  out[x] <- TRUE
  out
}
unwhich(which(x), 10)
```

Let's create two logical vectors and their integer equivalents and then explore the relationship between boolean and set operations.

```{r}
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

When first learning subsetting, a common mistake is to use `x[which(y)]` instead of `x[y]`.  Here the `which()` achieves nothing: it switches from logical to integer subsetting but the result will be exactly the same. Also beware that `x[-which(y)]` is __not__ equivalent to `x[!y]`: if `y` is all FALSE, `which(y)` will be `integer(0)` and `-integer(0)` is still `integer(0)`, so you'll get no values, instead of all values. In general, avoid switching from logical to integer subsetting unless you want, for example, the first or last `TRUE` value.

### Exercises

1.  How would you randomly permute the columns of a data frame? (This is an
    important technique in random forests.) Can you simultaneously permute 
    the rows and columns in one step?

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
