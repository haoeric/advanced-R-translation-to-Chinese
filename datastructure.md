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

缺失值用`NA`来标明，实际上`NA`是一个长度为1的逻辑型向量。在 `c()`函数里，`NA`会始终被转换成正确的类型。你可以使用`NA_real_` (一个数值型向量)，`NA_integer_`和`NA_character_`来创建特殊类型的`NA`.

#### 类型和测试

你可以使用`typeof()`来确定一个向量的类型, 或则使用"is"函数（`is.character()`，`is.double()`，`is.integer()`，`is.logical()`）来测试是否为某个具体的类型，或者使用`is.atomic()`做一般测试。 \indexc{typeof()}

```{r}
int_var <- c(1L, 6L, 10L)
typeof(int_var)
is.integer(int_var)
is.atomic(int_var)

dbl_var <- c(1, 2.5, 4.5)
typeof(dbl_var)
is.double(dbl_var)
is.atomic(dbl_var)
```

NB: `is.numeric()` is a general test for the "numberliness" of a vector and returns `TRUE` for both integer and double vectors. It is not a specific test for double vectors, which are often called numeric. \indexc{is.numeric()}

```{r}
is.numeric(int_var)
is.numeric(dbl_var)
```

#### Coercion

All elements of an atomic vector must be the same type, so when you attempt to combine different types they will be __coerced__ to the most flexible type. Types from least to most flexible are: logical, integer, double, and character. \index{coercion}

For example, combining a character and an integer yields a character:

```{r}
str(c("a", 1))
```

When a logical vector is coerced to an integer or double, `TRUE` becomes 1 and `FALSE` becomes 0. This is very useful in conjunction with `sum()` and `mean()`

```{r}
x <- c(FALSE, FALSE, TRUE)
as.numeric(x)

# Total number of TRUEs
sum(x)

# Proportion that are TRUE
mean(x)
```

Coercion often happens automatically. Most mathematical functions (`+`, `log`, `abs`, etc.) will coerce to a double or integer, and most logical operations (`&`, `|`, `any`, etc) will coerce to a logical. You will usually get a warning message if the coercion might lose information. If confusion is likely, explicitly coerce with `as.character()`, `as.double()`, `as.integer()`, or `as.logical()`. 

### Lists

Lists are different from atomic vectors because their elements can be of any type, including lists. You construct lists by using `list()` instead of `c()`: \index{lists} \index{vectors!lists|see{lists}}

```{r}
x <- list(1:3, "a", c(TRUE, FALSE, TRUE), c(2.3, 5.9))
str(x)
```

Lists are sometimes called __recursive__ vectors, because a list can contain other lists. This makes them fundamentally different from atomic vectors.

```{r}
x <- list(list(list(list())))
str(x)
is.recursive(x)
```

`c()` will combine several lists into one. If given a combination of atomic vectors and lists, `c()` will coerce the vectors to lists before combining them. Compare the results of `list()` and `c()`:

```{r}
x <- list(list(1, 2), c(3, 4))
y <- c(list(1, 2), c(3, 4))
str(x)
str(y)
```

The `typeof()` a list is `list`. You can test for a list with `is.list()` and coerce to a list with `as.list()`. You can turn a list into an atomic vector with `unlist()`. If the elements of a list have different types, `unlist()` uses the same coercion rules as `c()`.

Lists are used to build up many of the more complicated data structures in R. For example, both data frames (described in [data frames](#data-frames)) and linear models objects (as produced by `lm()`) are lists:

```{r}
is.list(mtcars)

mod <- lm(mpg ~ wt, data = mtcars)
is.list(mod)
```

### Exercises

1. What are the six types of atomic vector? How does a list differ from an
   atomic vector?

1. What makes `is.vector()` and `is.numeric()` fundamentally different to
   `is.list()` and `is.character()`?

1. Test your knowledge of vector coercion rules by predicting the output of
   the following uses of `c()`:

    ```{r, eval=FALSE}
    c(1, FALSE)
    c("a", 1)
    c(list(1), "a")
    c(TRUE, 1L)
    ```

1.  Why do you need to use `unlist()` to convert a list to an 
    atomic vector? Why doesn't `as.vector()` work? 

1. Why is `1 == "1"` true? Why is `-1 < FALSE` true? Why is `"one" < 2` false?

1. Why is the default missing value, `NA`, a logical vector? What's special
   about logical vectors? (Hint: think about `c(FALSE, NA_character_)`.)

## Attributes {#attributes}

All objects can have arbitrary additional attributes, used to store metadata about the object. Attributes can be thought of as a named list (with unique names). Attributes can be accessed individually with `attr()` or all at once (as a list) with `attributes()`. \index{attributes}

```{r}
y <- 1:10
attr(y, "my_attribute") <- "This is a vector"
attr(y, "my_attribute")
str(attributes(y))
```

The `structure()` function returns a new object with modified attributes: \indexc{structure()}

```{r}
structure(1:10, my_attribute = "This is a vector")
```

By default, most attributes are lost when modifying a vector:

```{r}
attributes(y[1])
attributes(sum(y))
```

The only attributes not lost are the three most important:

* Names, a character vector giving each element a name, described in 
  [names](#vector-names). 

* Dimensions, used to turn vectors into matrices and arrays, 
  described in [matrices and arrays](#matrices-and-arrays).

* Class, used to implement the S3 object system, described in [S3](#s3).
 
Each of these attributes has a specific accessor function to get and set values. When working with these attributes, use `names(x)`, `dim(x)`, and `class(x)`, not `attr(x, "names")`, `attr(x, "dim")`, and `attr(x, "class")`.

#### Names {#vector-names}

You can name a vector in three ways: \index{attributes|names}

* When creating it: `x <- c(a = 1, b = 2, c = 3)`.

* By modifying an existing vector in place: 
  `x <- 1:3; names(x) <- c("a", "b", "c")`. \indexc{names()}

* By creating a modified copy of a vector: 
  `x <- setNames(1:3, c("a", "b", "c"))`. \indexc{setNames()}

Names don't have to be unique. However, character subsetting, described in [subsetting](#lookup-tables), is the most important reason to use names and it is most useful when the names are unique.

Not all elements of a vector need to have a name. If some names are missing, `names()` will return an empty string for those elements. If all names are missing, `names()` will return `NULL`.

```{r}
y <- c(a = 1, 2, 3)
names(y)

z <- c(1, 2, 3)
names(z)
```

You can create a new vector without names using `unname(x)`, or remove names in place with `names(x) <- NULL`.

### Factors

One important use of attributes is to define factors. A factor is a vector that can contain only predefined values, and is used to store categorical data. Factors are built on top of integer vectors using two attributes: the `class()`, "factor", which makes them behave differently from regular integer vectors, and the `levels()`, which defines the set of allowed values. \index{factors|(}

```{r}
x <- factor(c("a", "b", "b", "a"))
x
class(x)
levels(x)

# You can't use values that are not in the levels
x[2] <- "c"
x

# NB: you can't combine factors
c(factor("a"), factor("b"))
```

Factors are useful when you know the possible values a variable may take, even if you don't see all values in a given dataset. Using a factor instead of a character vector makes it obvious when some groups contain no observations:

```{r}
sex_char <- c("m", "m", "m")
sex_factor <- factor(sex_char, levels = c("m", "f"))

table(sex_char)
table(sex_factor)
```

Sometimes when a data frame is read directly from a file, a column you'd thought would produce a numeric vector instead produces a factor. This is caused by a non-numeric value in the column, often a missing value encoded in a special way like `.` or `-`. To remedy the situation, coerce the vector from a factor to a character vector, and then from a character to a double vector. (Be sure to check for missing values after this process.) Of course, a much better plan is to discover what caused the problem in the first place and fix that; using the `na.strings` argument to `read.csv()` is often a good place to start.

```{r}
# Reading in "text" instead of from a file here:
z <- read.csv(text = "value\n12\n1\n.\n9")
typeof(z$value)
as.double(z$value)
# Oops, that's not right: 3 2 1 4 are the levels of a factor, 
# not the values we read in!
class(z$value)
# We can fix it now:
as.double(as.character(z$value))
# Or change how we read it in:
z <- read.csv(text = "value\n12\n1\n.\n9", na.strings=".")
typeof(z$value)
class(z$value)
z$value
# Perfect! :)
```

Unfortunately, most data loading functions in R automatically convert character vectors to factors. This is suboptimal, because there's no way for those functions to know the set of all possible levels or their optimal order. Instead, use the argument `stringsAsFactors = FALSE` to suppress this behaviour, and then manually convert character vectors to factors using your knowledge of the data. A global option, `options(stringsAsFactors = FALSE)`, is available to control this behaviour, but I don't recommend using it. Changing a global option may have unexpected consequences when combined with other code (either from packages, or code that you're `source()`ing), and global options make code harder to understand because they increase the number of lines you need to read to understand how a single line of code will behave.  \indexc{stringsAsFactors}

While factors look (and often behave) like character vectors, they are actually integers. Be careful when treating them like strings. Some string methods (like `gsub()` and `grepl()`) will coerce factors to strings, while others (like `nchar()`) will throw an error, and still others (like `c()`) will use the underlying integer values. For this reason, it's usually best to explicitly convert factors to character vectors if you need string-like behaviour. In early versions of R, there was a memory advantage to using factors instead of character vectors, but this is no longer the case. \index{factors|)}

### Exercises

1.  An early draft used this code to illustrate `structure()`:

    ```{r}
    structure(1:5, comment = "my attribute")
    ```

    But when you print that object you don't see the comment attribute.
    Why? Is the attribute missing, or is there something else special about
    it? (Hint: try using help.) \index{attributes!comment}

1.  What happens to a factor when you modify its levels? 
    
    ```{r, results = "none"}
    f1 <- factor(letters)
    levels(f1) <- rev(levels(f1))
    ```

1.  What does this code do? How do `f2` and `f3` differ from `f1`?

    ```{r, results = "none"}
    f2 <- rev(factor(letters))

    f3 <- factor(letters, levels = rev(letters))
    ```

## Matrices and arrays {#matrices-and-arrays}

Adding a `dim()` attribute to an atomic vector allows it to behave like a multi-dimensional __array__. A special case of the array is the __matrix__, which has two dimensions. Matrices are used commonly as part of the mathematical machinery of statistics. Arrays are much rarer, but worth being aware of. \index{arrays|(} \index{matrices|see{arrays}}

Matrices and arrays are created with `matrix()` and `array()`, or by using the assignment form of `dim()`:

```{r}
# Two scalar arguments to specify rows and columns
a <- matrix(1:6, ncol = 3, nrow = 2)
# One vector argument to describe all dimensions
b <- array(1:12, c(2, 3, 2))

# You can also modify an object in place by setting dim()
c <- 1:6
dim(c) <- c(3, 2)
c
dim(c) <- c(2, 3)
c
```

`length()` and `names()` have high-dimensional generalisations:

* `length()` generalises to `nrow()` and `ncol()` for matrices, and `dim()`
  for arrays. \indexc{nrow()} \indexc{ncol()} \indexc{dim()}

* `names()` generalises to `rownames()` and `colnames()` for matrices, and
  `dimnames()`, a list of character vectors, for arrays. \indexc{rownames()}
  \indexc{colnames()} \indexc{dimnames()}

```{r}
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

`c()` generalises to `cbind()` and `rbind()` for matrices, and to `abind()` (provided by the `abind` package) for arrays. You can transpose a matrix with `t()`; the generalised equivalent for arrays is `aperm()`. \indexc{cbind()} \indexc{rbind()} \indexc{abind()} \indexc{aperm()}

You can test if an object is a matrix or array using `is.matrix()` and `is.array()`, or by looking at the length of the `dim()`. `as.matrix()` and `as.array()` make it easy to turn an existing vector into a matrix or array.

Vectors are not the only 1-dimensional data structure. You can have matrices with a single row or single column, or arrays with a single dimension. They may print similarly, but will behave differently. The differences aren't too important, but it's useful to know they exist in case you get strange output from a function (`tapply()` is a frequent offender). As always, use `str()` to reveal the differences. \index{arrays!1d}

```{r}
str(1:3)                   # 1d vector
str(matrix(1:3, ncol = 1)) # column vector
str(matrix(1:3, nrow = 1)) # row vector
str(array(1:3, 3))         # "array" vector
```

While atomic vectors are most commonly turned into matrices, the dimension attribute can also be set on lists to make list-matrices or list-arrays: \index{arrays!list-arrays} \index{list-arrays}

```{r}
l <- list(1:3, "a", TRUE, 1.0)
dim(l) <- c(2, 2)
l
```

These are relatively esoteric data structures, but can be useful if you want to arrange objects into a grid-like structure. For example, if you're running models on a spatio-temporal grid, it might be natural to preserve the grid structure by storing the models in a 3d array. \index{arrays|)}

### Exercises

1.  What does `dim()` return when applied to a vector?

1.  If `is.matrix(x)` is `TRUE`, what will `is.array(x)` return?

1.  How would you describe the following three objects? What makes them
    different to `1:5`?

    ```{r}
    x1 <- array(1:5, c(1, 1, 5))
    x2 <- array(1:5, c(1, 5, 1))
    x3 <- array(1:5, c(5, 1, 1))
    ```

## Data frames {#data-frames}

A data frame is the most common way of storing data in R, and if [used systematically](http://vita.had.co.nz/papers/tidy-data.pdf) makes data analysis easier. Under the hood, a data frame is a list of equal-length vectors. This makes it a 2-dimensional structure, so it shares properties of both the matrix and the list.  This means that a data frame has `names()`, `colnames()`, and `rownames()`, although `names()` and `colnames()` are the same thing. The `length()` of a data frame is the length of the underlying list and so is the same as `ncol()`; `nrow()` gives the number of rows. \index{data frames|(}

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