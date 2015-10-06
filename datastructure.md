数据结构
=========

本章主要概括讲解基础R中的几种最重要的数据结构。你可能已经使用了其中的大部分数据结构，可是你可能还没深入的弄清楚他们之间的相互关系。在本章，我不会深入讲解单个的数据结构，而是帮助你理清它们之间整体的相通性。如果你需要单个数据结构的详细介绍，你可以查找R文档。


R的基础数据结构可以通过纬度（1维，2维或n维）和同质或异质性（数据类型是否一致）来概括。包括了五种数据分析中最常用的数据结构：

|     |          同质        |          异质         |
|-----|----------------------|-----------------------|
| 1维 | Atomic vector（原向量）| List （列表）         |
| 2维 | Matrix（矩阵）       | Data frame （数据框） |
| n维 | Array（数组）        |                       |

其他所有的数据类型几乎都收建立在这些基础的数据类型之上的。在[面相对象指南](http://adv-r.had.co.nz/OO-essentials.html#oo)
一章你将会学习如何使用这些基础的数据类型来建立更复杂的数据类型。请注意R语言没有0维和标量类型。那些你会认为是标量的单个数字或者字符串，在R中实际上是一维的数组。

查看一个对象数据结构的最简单的方式是使用`str()`函数。`str()`是structure（结构）的缩写，它能对任何R数据结构提供简洁明了的描述。

####测试

做做这个简单的测试看看你是否需要阅读本章内容。如果你能很快的的到答案，你可以轻松的跳过本章。本章最后提供[参考答案](#data-structure-answers)。

1. 数组除了它的内容，其他的三个特性是什么？
2. 四种主要的原向量类型？以及另外两种不常用的原向量类型？
3. 什么是属性？如何来查看和设置属性？
4. 列表和原向量有什么不同，矩阵和数据结构有什么不同？
5. 一个列表可以是矩阵吗？一个数据框的一列可以是矩阵吗？



##答案{#data-structure-answers}
The three properties of a vector are type, length, and attributes.

The four common types of atomic vector are logical, integer, double (sometimes called numeric), and character. The two rarer types are complex and raw.

Attributes allow you to associate arbitrary additional metadata to any object. You can get and set individual attributes with attr(x, "y") and attr(x, "y") <- value; or get and set all attributes at once with attributes().

The elements of a list can be any type (even a list); the elements of an atomic vector are all of the same type. Similarly, every element of a matrix must be the same type; in a data frame, the different columns can have different types.

You can make “list-array” by assuming dimensions to a list. You can make a matrix a column of a data frame with df$x <- matrix(), or using I() when creating a new data frame data.frame(x = I(matrix())).