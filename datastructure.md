数据结构
=========

本章主要概括讲解基础R中的几种最重要的数据结构。你可能已经使用了其中的大部分数据结构，可是你可能还没深入的弄清楚他们之间的相互关系。在本章，我不会深入讲解单个的数据结构，而是帮助你理清它们之间整体的相通性。你可以查找R文档来获得单个数据结构的详细介绍。


R的基础数据结构可以通过纬度（1维，2维或n维）和同质或异质性（数据类型是否一致）来概括。包括了五种数据分析中最常用的数据结构：

|     |          同质          |          异质         |
|-----|------------------------|-----------------------|
| 1维 | Atomic vector（原向量）| List （列表）         |
| 2维 | Matrix（矩阵）         | Data frame （数据框） |
| n维 | Array（数组）          |                       |

其他所有的数据类型几乎都收建立在这些基础的数据类型之上的。在[面相对象指南](http://adv-r.had.co.nz/OO-essentials.html#oo)
一章你将会学习如何使用这些基础的数据类型来建立更复杂的数据类型。请注意R语言没有0维和标量类型。那些你会认为是标量的单个数字或者字符串，在R中实际上是一维的数组。

查看一个对象数据结构的最简单的方式是使用`str()`函数。`str()`是structure（结构）的缩写，它能对任何R数据结构提供简洁明了的描述。

#####测试

做做这个简单的测试看看你是否需要阅读本章内容。如果你能很快的的到答案，你可以轻松的跳过本章。本章最后提供[参考答案](#data-structure-answers)。

1. 数组除了它的内容，其他的三个特性是什么？
2. 四种主要的原向量类型？以及另外两种不常用的原向量类型？
3. 什么是属性？如何来查看和设置属性？
4. 列表和原向量有什么不同，矩阵和数据结构有什么不同？
5. 一个列表可以是矩阵吗？一个数据框的一列可以是矩阵吗？


#####概要

* [向量](#vectors) 介绍R的一维数据结构，原向量和列表
  
* [属性](#attributes) 简单的介绍下R灵活的元数据说明方式－属性，这里会介绍原向量设置属性中的一种重要的数据结构－因子。
  
* [矩阵和数组](#matrices-and-arrays) 介绍二维和高维的数据结构，矩阵和数组。
  
* [数据框](#data-frames) 学习R中最重要的数据结构－数据框，数据框同时包涵列表和矩阵的特性，是一种非常适合做统计分析的数据结构。

## 向量 {#vectors}

向量是R中的基础数据结构。向量有两种形式：原向量和列表。它们有如下三种共同特性：

* 类型， `typeof()`，是什么。
* 长度， `length()`，有多少个元素。
* 属性， `attributes()`, 其他任意的原数据.

他们主要的不同在于元素的类型：原向量的所有元素必须是相同的类型，但是列表的元素可以是不一样的类型。

注意：`is.vector()` 并不测试一个对象是否是向量。只有当一个对象是向量并且除了名字没有其他属性时，`is.vector()`的返回值才是`TRUE`。请使用`is.atomic(x) || is.list(x)`来测试一个对象是否为向量。

### 原向量

我将详细的介绍下R中原向量的四种常见的类型：逻辑型(logicle)，整型(integer)，数值型(double或则numeric)和字符型(character)。还有两种不常用的类型：复杂型(complex)和粗糙型(raw)，这里就不详细介绍了。

原向量通常可以用`c()`函数来创建，`c()`表示组合(combine)：

```r
dbl_var <- c(1, 2.5, 4.5)
# 用L做后缀会的到整型而不是数值型
int_var <- c(1L, 6L, 10L)
# 用TRUE和FALSE(或者T和F) 创建逻辑型向量
log_var <- c(TRUE, FALSE, T, F)
chr_var <- c("these are", "some strings")
```

原向量始终是扁平的，即使你嵌套使用多个`c()`:

```r
c(1, c(2, c(3, 4)))
# 等同于如下
c(1, 2, 3, 4)
```

缺失值用`NA`来标明，实际上`NA`是一个长度为1的逻辑型向量。在 `c()`函数里，`NA`会始终被转换成逻辑类型。你可以使用`NA_real_` (一个数值型向量)，`NA_integer_`和`NA_character_`来创建特殊类型的`NA`.

#### 类型测试

你可以使用`typeof()`来确定一个向量的类型, 或则使用"is"函数（`is.character()`，`is.double()`，`is.integer()`，`is.logical()`）来测试是否为某个具体的类型，或者使用`is.atomic()`做一般测试。 \indexc{typeof()}

```r
int_var <- c(1L, 6L, 10L)
typeof(int_var)        # "integer"  
is.integer(int_var)    # TRUE
is.atomic(int_var)     # TRUE

dbl_var <- c(1, 2.5, 4.5)
typeof(dbl_var)        # "double"
is.double(dbl_var)     # TRUE
is.atomic(dbl_var)     # TRUE
```

注意：`is.numeric()`是判断一个向量是否为数字的通用测试。也就是说不管是整型还是数值型的向量，它的返回值都是`TRUE`。所以`is.numeric()`并不是检测数值型(double或numeric)的专用函数。

```{r}
is.numeric(int_var)  # TRUE
is.numeric(dbl_var)  # TRUE
```

#### 强制类型转换

一个原向量的所有元素都必须是相同的类型。因此当你不同类型的元素组合在一起的时候，它们会被__强制__转换成最通用的类型。不同类型的通用型由低到高排列是：逻辑型(logical)，整型(integer), 数值型(double), 和字符型(character)。

例如，整形和字符型结合在一起就被转换成字符型：

```{r}
str(c("a", 1))
```

当一个逻辑型被强制转换成整型或则数值型时，`TRUE`会被转换成1，`FALSE`会被转换成0。这在与`sum()`和`mean()`搭配使用的时候非常有用。

```{r}
x <- c(FALSE, FALSE, TRUE)
as.numeric(x)

# Total number of TRUEs
sum(x)

# Proportion that are TRUE
mean(x)
```

R中很多强制类型转换是自动的。比如在使用数学运算函数(如`+`，`log`，`abs`等)时，数据类型会被强制转换成数值型(double)或则整型(integer)； 在逻辑运算比如`&`，`|`，`any`等中，数据类型会被强制转换成逻辑型(logical)。在一些强制转换中通常如果有信息丢失，你都会的到一些警告信息。如果不确定，可以使用`as.character()`，`as.double()`，`as.integer()`或`as.logical()`来进行明确的类型转换。 

### 列表

列表和原向量的不同在于列表的元素可以是任意不同的类型和数据结构，包括列表。列表由`list()`而不是`c()`来创建。

```{r}
x <- list(1:3, "a", c(TRUE, FALSE, TRUE), c(2.3, 5.9))
str(x)
```

列表有时候也被称为__迭代__向量，因为一个列表也能包括其他列表。 这是他们与原向量有本质上的不同。

```{r}
x <- list(list(list(list())))
str(x)
is.recursive(x)
```

`c()`函数可以把多个列表组合成一个列表。`c()`不仅能组合原向量也能组合列表。在组合原向量前，`c()`会先将原向量强制转换成列表然后再组合。比较一下`list()`和`c()`的不同：

```{r}
x <- list(list(1, 2), c(3, 4))
y <- c(list(1, 2), c(3, 4))
str(x)
str(y)
```

一个列表的`typeof()`返回值是`list`。你可以使用`is.list()`来检测一个对象是否为列表，也可以使用`as.list()`将一个对象强制转换成列表。如果一个列表中的元素是不同的数据类型，使用`unlist()`进行强制类型转换将和`c()`函数的强制转换规则一样。

列表在R中通常被用来创建更复杂的数据结构。比如[数据框](#data-frames)和线性模型对象(`lm()`函数的结果)都是列表：

```{r}
is.list(mtcars)  # TRUE
mod <- lm(mpg ~ wt, data = mtcars)
is.list(mod)     # TRUE
```

### 练习

1. 原向量有哪六种类型？列表和原向量有什么不同？

2. `is.vector()`和`is.numeric()`与`is.list()`和`is.character()`本质上有什么不同？

3. 预测如下代码中`c()`的结果来检测你对向量中强制类型转换规则的掌握情况：
    ```r
    c(1, FALSE)
    c("a", 1)
    c(list(1), "a")
    c(TRUE, 1L)
    ```

4. 为什么要使用`unlist()`来将一个列表转换成原向量而不是使用`as.vector()`？ 

5. 为什么`1 == "1"`的返回值是TRUE？为什么`-1 < FALSE`返回值是TRUE？为什么`"one" < 2`的返回值是FALSE？

6. 为什么表示缺失值的`NA`是一个逻辑型向量？逻辑型向量有什么特点？（提示：想一想`c(FALSE, NA_character_)`)

## 属性 {#attributes}

所有的对象都可以拥有附加的属性，用来存储关于对象的元数据。属性可以看作无重复的名字列表。一个对象的属性可以使用`attr()`来单独查看或修改，也可以使用`attributes()`列出所有属性。

```r
y <- 1:10
attr(y, "my_attribute") <- "This is a vector"
attr(y, "my_attribute")
str(attributes(y))
```

`structure()`函数可以用来修改一个对象的属性然后返回一个新的对象：

```{r}
structure(1:10, my_attribute = "This is a vector")
```
当一个向量被修改后，其大部分属性会默认地被丢失：

```{r}
attributes(y[1])
attributes(sum(y))
```

其中有三个最重要的属性是不会丢失的：

* **名字**，一个存储每一个元素名字的字符类向量。详见[名字](#vector-names)。

* **维度**，用来将向量转化成矩阵和数组，详见[矩阵和数组](#matrices-and-arrays)。

* **类**，用在S3面相对象系统，详见[S3](#s3).

以上三种属性有专门的函数来用来查看和设置其属性值，分别是 `names(x)`，`dim(x)`和`class(x)`，注意这里不是用`attr(x, "names")`，`attr(x, "dim")`和`attr(x, "class")`。

#### 名字{#vector-names}

你可以用以下三种方式来给向量命名：

* 在创建时：`x <- c(a = 1, b = 2, c = 3)`。

* 修改一个已有向量的名字属性：`x <- 1:3; names(x) <- c("a", "b", "c")`。

* 复制一个向量并添加名称属性：`x <- setNames(1:3, c("a", "b", "c"))`。

一个向量的名字可以是重复的。然而无重复的名字属性对于R中便利的[取子集](#lookup-tables)操作来说非常关键。

并不是一个向量中的所有元素都需要有名字。`names()`函数会对那些没有名字的向量元素返回一个空字符。如果所有的名字都缺失，那么`names()`函数会返回`NULL`。

```r
y <- c(a = 1, 2, 3)
names(y)

z <- c(1, 2, 3)
names(z)
```

你可以使用`unname(x)`得到一个去掉名字的新向量，或则用`names(x) <- NULL`来去掉该对象的名字。

### 因子

使用名字的一大好处是可以定义因子。因子是一个仅包含预设值的向量，用来存储分类数据。因子是由整型向量添建两种属性来创建的。这两种属性分别是：`class()`为“因子”，使他们和常规的整形向量区分开，以及`levels()`水平，用来定义向量元素的类别。

```r
x <- factor(c("a", "b", "b", "a"))
x
class(x)
levels(x)

# 不可以使用不在水平范围中的值
x[2] <- "c"
x

# 注意：因子不可以合并
c(factor("a"), factor("b"))
```

当你不知道一个数据中的所有值，但是只知道某个变量的可能值时，使用因子会非常有用。使用因子而不是字符向量使得查看某个类别中是否有观测值变得非常方便：

```r
sex_char <- c("m", "m", "m")
sex_factor <- factor(sex_char, levels = c("m", "f"))

table(sex_char)
table(sex_factor)
```
当你从一个文件中直接读取一个数据框时，你认为应该是数值型向量的某一列可能会变成了一个因子。这应该是因为该列中存在非数值的元素，通常是一些用特殊字符比如`.`或者`-`标记的缺失值。对于这种情况，你可以将这一列先强制转换成字符行向量，然后再强制转换成数值型向量（在转换后记得查看缺失值）。当然，更简单的方法是在读取数据的时候就解决这个问题。比如在使用`read.csv()`函数时设置`na.strings`参数。

```r
# 用你的文本文件名替代下面的"text"：
z <- read.csv(text = "value\n12\n1\n.\n9")
typeof(z$value)
as.double(z$value)

# 嗷嗷，这不对。3 2 1 4是这个因子的水平，不是我们读进去的数值
class(z$value)

# 我们可以这样来解决：
as.double(as.character(z$value))

# 或者我们修改我们读取文件的函数：
z <- read.csv(text = "value\n12\n1\n.\n9", na.strings=".")
typeof(z$value)
class(z$value)
z$value
# 完美！:)
```

不幸的是，R中很多的数据读取函数都会自动的将字符型向量转换成因子。这不是最优的方案，因为这些函数不可能知道所有的因子水平以及他们正确的顺序。你可以在函数中声名`stringsAsFactors = FALSE`来去除这种转换，然后根据你对数据的了解和需要，手动的将特定的字符型向量转换成因子。另外，设置全局变量`options(stringsAsFactors = FALSE)`也可以用来去除这种强制转换，然而我并不推荐使用。修改全局变量会对外部添加的代码（无论是使用`source()`或引入其他包）造成不可预知的影响，同时它也降低了代码的可读性，因为你需要知道修改的全局变量对哪些代码产生了影响。

尽管因子在使用的时候像一个字符类向量，然而实际上因子是整型向量。一些字符操作函数（比如`gsub()`和`grepl()`）会将因子强制转换成字符，也有一些（比如`nchar()`）会报错，而像`c()`则会用因子内在的整型值。因此，如果需要字符型的操作，最好先将因子强制转换成字符型向量。在R先前的版本中使用因子比字符型向量节省一些内存，现在的版本中确并不是这样。

### 练习

1.  之前有种如下的代码演示`structure()`的使用：

    ```r
    structure(1:5, comment = "my attribute")
    ```
    可是打印出来确并看不到comment属性。为什么？属性消失了吗，或者有一些其他的特别之处？
    
2.  当你修改一个因子的水平的时候会发生什么？
    
    ```r
    f1 <- factor(letters)
    levels(f1) <- rev(levels(f1))
    ```

3.  下面的代码是用来做什么的？`f2`和`f3`与`f1`有什么不同？

    ```r
    f2 <- rev(factor(letters))
    f3 <- factor(letters, levels = rev(letters))
    ```

## 矩阵和数组 {#matrices-and-arrays}

给原向量添加一个`dim()`属性会将原向量转换成多维__数组__。矩阵是一种特殊的二维数组。矩阵在统计数学中的使用非常普遍。数组确相对比较少用，但是也是一种很重要的数据结构。

矩阵和数组可以分别使用`matrix()`和`array()`来创建，或者通过使用`dim()`给原向量赋予维度来创建：

```r
# 用两个标量参数来设置行数和列数
a <- matrix(1:6, ncol = 3, nrow = 2)
# 用一个向量来设置纬度
b <- array(1:12, c(2, 3, 2))

# 也可以使用dim()将向量转换成矩阵和数组
c <- 1:8
dim(c) <- c(4, 2)
c
dim(c) <- c(2, 2, 3)
c
```

`length()`和`names()`在矩阵和数组中也有相对应的函数：

* 在矩阵中和`length()`相对应的函数是`nrow()`和`ncol()`，在数组中和`length()`相对应的函数则是`dim()`。

* 在矩阵中和`names()`相对应的函数是`rownames()`和`colnames()`信息，在数组中和`names()`相对应的函数则是`dimnames()`。

```r
length(a)
nrow(a)
ncol(a)
rownames(a) <- c("A", "B")
colnames(a) <- c("a", "b", "c")
a

length(b)
dim(b)
dimnames(b) <- list(c("one", "two"), c("a", "b", "c"), c("A", "B"))
b
```

矩阵中和`c()`相对应的函数分别是`cbind()`和`rbind()`，数组中则是`abind()`（由`abind`包提供）。你可以使用`t()`来转置一个矩阵；数组转置则用`aperm()`。

你可以使用`is.matrix()`和`is.array()`来检测一个对象是否为矩阵或则数组，使用`dim()`来检测该对象的纬度。使用`as.matrix()`或`as.array()`则可以将一个向量轻松地转换成矩阵或则数组。

不仅仅是有向量是一维的数据结构。你也可以构建只有一行或一列的矩阵，或者只是一维的数组。他们打印出来可能很相似，可是操作起来确大不相同。这些不同不是很重要，可是知道有这些不同对于使用一些函数（比如`tapply()`）会非常有用。记住，多使用`str()`来查看数据的结构。

```r
str(1:3)                   # 一维向量
str(matrix(1:3, ncol = 1)) # 一列矩阵
str(matrix(1:3, nrow = 1)) # 一行矩阵
str(array(1:3, 3))         # “数组”向量
```

尽管通常使用`dim()`来将向量转换成矩阵，使用纬度属性也可以将列表转换成**矩阵列表**或**数组列表**。

```r
l <- list(1:3, "a", TRUE, 1.0)
dim(l) <- c(2, 2)
l
```
这些是一些相对隐秘的数据结构，但是如果你想将对象存放在格子一样的数据结构中，这将会很有帮助。比如你在时空格（spatio-temporal grid）模型中运算，将数据存储在三维数组中将维持数据的三维空间结构从而极大地方便了数据操控。

### 练习

1.  对一个向量使用`dim()`将返回什么值?

2.  如果`is.matrix(x)`返回`TRUE`，那么`is.array(x)`将返回什么？

3.  如何描述如下三种对象？它们和`1:5`有什么不同？

    ```r
    x1 <- array(1:5, c(1, 1, 5))
    x2 <- array(1:5, c(1, 5, 1))
    x3 <- array(1:5, c(5, 1, 1))
    ```

## 数据框 {#data-frames}

数据框是R中存储数据最常用的数据结构。在程序中[普及使用](http://vita.had.co.nz/papers/tidy-data.pdf)数据框会使得数据分析更简单。其实隐藏在数据框中的是一个**包含多个相同长度向量的列表**。因此数据框是一个同时拥有矩阵和列表特性的二维数据结构。这意味着数据框有`names()`也有`colnames()`和`rownames()`，尽管`names()`和`colnames()`是同一回事。用`length()`查看一个数据框的返回值是数据框中潜在列表的长度，因此和`ncol()`的返回值相同； `nrow()`返回数据框的行数。


As described in [subsetting](#subsetting), you can subset a data frame like a 1d structure (where it behaves like a list), or a 2d structure (where it behaves like a matrix).

### Creation

You create a data frame using `data.frame()`, which takes named vectors as input:

```{r}
df <- data.frame(x = 1:3, y = c("a", "b", "c"))
str(df)
```

Beware `data.frame()`'s default behaviour which turns strings into factors. Use `stringAsFactors = FALSE` to suppress this behaviour: \indexc{stringsAsFactors}

```{r}
df <- data.frame(
  x = 1:3,
  y = c("a", "b", "c"),
  stringsAsFactors = FALSE)
str(df)
```

### Testing and coercion

Because a `data.frame` is an S3 class, its type reflects the underlying vector used to build it: the list. To check if an object is a data frame, use `class()` or test explicitly with `is.data.frame()`:

```{r}
typeof(df)
class(df)
is.data.frame(df)
```

You can coerce an object to a data frame with `as.data.frame()`:

* A vector will create a one-column data frame.

* A list will create one column for each element; it's an error if they're 
  not all the same length.
  
* A matrix will create a data frame with the same number of columns and rows as the matrix.

### Combining data frames

You can combine data frames using `cbind()` and `rbind()`: \indexc{cbind()} \indexc{rbind()}

```{r}
cbind(df, data.frame(z = 3:1))
rbind(df, data.frame(x = 10, y = "z"))
```

When combining column-wise, the number of rows must match, but row names are ignored. When combining row-wise, both the number and names of columns must match. Use `plyr::rbind.fill()` to combine data frames that don't have the same columns. 

It's a common mistake to try and create a data frame by `cbind()`ing vectors together. This doesn't work because `cbind()` will create a matrix unless one of the arguments is already a data frame. Instead use `data.frame()` directly:

```{r}
bad <- data.frame(cbind(a = 1:2, b = c("a", "b")))
str(bad)
good <- data.frame(a = 1:2, b = c("a", "b"),
  stringsAsFactors = FALSE)
str(good)
```

The conversion rules for `cbind()` are complicated and best avoided by ensuring all inputs are of the same type.

### Special columns

Since a data frame is a list of vectors, it is possible for a data frame to have a column that is a list: \index{data frames!list in column}

```{r}
df <- data.frame(x = 1:3)
df$y <- list(1:2, 1:3, 1:4)
df
```

However, when a list is given to `data.frame()`, it tries to put each item of the list into its own column, so this fails:

```{r, error = TRUE}
data.frame(x = 1:3, y = list(1:2, 1:3, 1:4))
```

A workaround is to use `I()`, which causes `data.frame()` to treat the list as one unit:

```{r}
dfl <- data.frame(x = 1:3, y = I(list(1:2, 1:3, 1:4)))
str(dfl)
dfl[2, "y"]
```

`I()` adds the `AsIs` class to its input, but this can usually be safely ignored. \indexc{I()}

Similarly, it's also possible to have a column of a data frame that's a matrix or array, as long as the number of rows matches the data frame: \index{data frames!array in column}

```{r}
dfm <- data.frame(x = 1:3, y = I(matrix(1:9, nrow = 3)))
str(dfm)
dfm[2, "y"]
```

Use list and array columns with caution: many functions that work with data frames assume that all columns are atomic vectors. \index{data frames|)}

### Exercises

1.  What attributes does a data frame possess?

1.  What does `as.matrix()` do when applied to a data frame with 
    columns of different types?

1.  Can you have a data frame with 0 rows? What about 0 columns?

##答案{#data-structure-answers}
The three properties of a vector are type, length, and attributes.

The four common types of atomic vector are logical, integer, double (sometimes called numeric), and character. The two rarer types are complex and raw.

Attributes allow you to associate arbitrary additional metadata to any object. You can get and set individual attributes with attr(x, "y") and attr(x, "y") <- value; or get and set all attributes at once with attributes().

The elements of a list can be any type (even a list); the elements of an atomic vector are all of the same type. Similarly, every element of a matrix must be the same type; in a data frame, the different columns can have different types.

You can make “list-array” by assuming dimensions to a list. You can make a matrix a column of a data frame with df$x <- matrix(), or using I() when creating a new data frame data.frame(x = I(matrix())).