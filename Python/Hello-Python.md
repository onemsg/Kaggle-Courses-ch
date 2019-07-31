# 介绍

本课程包含了您需要的关键Python技能，通过学习您可以使用Python进行数据科学。该课程非常适合具有一些编程经验的人，那些希望将Python添加到编程生涯中或提升基本Python技能的人。（如果您是初学编程者，可查看[这些“适用于非程序猿的Python”学习资源](https://wiki.python.org/moin/BeginnersGuide/NonProgrammers)）

首先，我们将简要概述Python语法、变量赋值和算术运算符。如果您以前有Python编程经验，可以直接[跳到动手练习](https://www.kaggle.com/scratchpad/kernel7649d4a35f/edit)。

## Hello, Python

Python是以英国喜剧团体[Monty Python](https://en.wikipedia.org/wiki/Monty_Python)的名字命名的，所以我们第一个Python程序将向他们的《垃圾邮件》短剧致敬？

开个玩笑。尝试阅读下面的代码并预测运行结果。（如果你不知道，没关系！）

然后单击“输出”按钮以查看我们程序的结果。

``` python
    spam_amount = 0
    print(spam_amount)

    # Ordering Spam, egg, Spam, Spam, bacon and Spam (4 more servings of Spam)
    spam_amount = spam_amount + 4

    if spam_amount > 0:
        print("But I don't want ANY spam!")

    viking_song = "Spam " * spam_amount
        print(viking_song)
```

输出：

``` text
    0
    But I don't want ANY spam!
    Spam Spam Spam Spam
```

在这里打开很多东西！这个傻傻的程序演示了Python代码的许多重要方面以及它的工作原理。我们从上到下查看代码。

``` python
    spam_amount = 0
```

 **变量赋值（Variable assignment）**：这里我们创建一个名为 spam_amount 的变量，并使用赋值运算符"="将其赋值为0。

>旁白：如果您已经使用某些其他语言（如Java或C ++）编程，您可能会注意到Python不需要我们在这里做的一些事情：
>
> + 我们不需要在赋值之前“声明”spam_amount
> + 我们不需要告诉Python spam_amount 将引用什么类型的数据。实际上，我们甚至可以重新分配 spam_amount 来引用不同的数据类型，比如字符串或布尔值。

``` python
    print(spam_amount)
```

输出：`0`

**函数调用（Function calls）**："print"是一个Python函数，显示在屏幕上传递给它的值。我们通过在括号后添加括号来调用函数，并将输入（或参数）放在这些括号中的函数中。

``` python
    # Ordering Spam, egg, Spam, Spam, bacon and Spam (4 more servings of Spam)
    spam_amount = spam_amount + 4
```

上面的第一行是**注释（comment）**。在Python中，注释以"＃"符号开头。

接下来我们看一个重新赋值的例子。重新赋值现有变量的值与创建变量的方式相同——它仍然使用"="赋值运算符。

在这种情况下，我们分配给 spam_amount 的值涉及对其先前值的一些简单算术。当遇到这一行时，Python会在"="的右侧计算表达式（0 + 4 = 4），然后将该值赋给左侧的变量。

``` python
    if spam_amount > 0:
        print("But I don't want ANY spam!")

    viking_song = "Spam Spam Spam"
        print(viking_song)
```

输出：

``` text
    But I don't want ANY spam!
    Spam Spam Spam Spam
```

后面的课程我们才会讨论“条件语句”，但是，即使你以前从未编码过，你也可以猜到它的作用。Python因其可读性和简单性而备受推崇。

注意哪些代码属于if。 "But I don't want ANY spam!"只有当 spam_amount 为正数时才显示。而我们（Python）是如何知道何时执行后面的代码（比如 print（viking_song） ）的呢？

if行末尾的冒号（:)表示新的“代码块”开始。随后的缩进行是该代码块的一部分。其他一些语言使用{花括号}来标记代码块的开头和结尾。Python对有意义的空白的使用令习惯于其他语言的程序员感到吃惊，实际上它比不强制执行代码块缩进的语言更有序和可读。

执行 viking_song 的后续行没有缩进额外的4个空格，因此它们不是if的代码块的一部分。稍后在定义函数和使用循环时，我们将看到更多缩进代码块的示例。

这段代码片段也是我们第一次看到Python中的**字符串（string ）**：

``` python
    "But I don't want ANY spam!"
```

字符串可以用双引号或单引号标记。 （但是因为这个特定的字符串包含单引号字符，所以我们试图用单引号括起来时Python可能会混淆，除非我们小心。）

``` python
    viking_song = "Spam " * spam_amount
    print(viking_song)
```

输出：

``` text
    Spam Spam Spam Spam
```

`*` 运算符可以用来乘以两个数字（`3 * 3`的计算结果为`9`），但有趣的是，我们还可以将一个字符串乘以一个数字，以获得重复的字符串。Python提供了许多厚颜无耻的节省时间的技巧，其中"`*`"和"`+`"等运算符具有不同的含义，具体取决于它们应用于何种情况下。 （技术术语是[运算符重载](https://en.wikipedia.org/wiki/Operator_overloading)）

## Python中的数（Number）和运算

上面我们已经见到一个赋予了数值（Number）的变量的例子：

``` python
    spam_amount = 0
```

“数（Number）”是一个很好的非正式名称，但如果我们想要更具技术性，我们可以问问Python是如何描述 spam_amount 的类型：

``` python
    type(spam_amount)
```

输出：

``` text
    int
```

`int`是`integer`的缩写，表明它是整型。我们在Python中经常会遇到另一种数：

``` python
    type(19.95)
```

输出：

``` text
    float
```

`float`是一种带有小数位的数 - 非常适用于表示重量或比例之类的。

`type()`是我们见过的第二个内置函数（在`print()`之后），它是另一个值得记住的好函数。能够问Python“这是什么类型？”非常实用。

数是用来运算的。我们已经看到"+"运算符用于加法，"*"运算符用于乘法（其中一种用法）。 Python还为我们提供了计算器上其余基本按钮的功能：

|  运算  |          名称          |          描述          |
| :----: | :--------------------: | :--------------------: |
| a + b  |          加法          |        a与b的和        |
| a - b  |          减法          |        a与b的差        |
| a * b  |          乘法          |        a与b的积        |
| a / b  | 除法（True division）  |        a和b的商        |
| a // b | 除法（Floor division） | a和b的商，删除小数部分 |
| a % b  |          求余          |   除以a后的整数余数    |
| a ** b |         幂运算         |        a的b次幂        |
|   -a   |           负           |        a的负值         |

有趣的是，虽然你的计算器可能只有一个按钮用于除法，但Python可以做两种。“除法（True division）”基本上是你的计算器所做的：

``` python
    print(5 / 2)
    print(6 / 2)
```

输出：

``` text
    2.5
    3.0
```

总是得到一个浮点数。  
“//”运算符将会舍去小数部分（向下取整）。

``` python
    print(5 // 2)
    print(6 // 2)

    输出>>>：
    2
    3
```

这有什么用呢？您马上将看到编码挑战中的一个例子。

## 运算符顺序

我们在小学学的运算是有先后顺序的。  
Python遵循类似的规则来首先执行哪些计算。他们大多非常直观。

``` python
    8 - 3 + 2
    -3 + 4 * 2

    输出>>>：
    7
    5
```

有时默认的操作顺序不是我们想要的：

``` python
    hat_height_cm = 25
    my_height_cm = 190
    # How tall am I, in meters, when wearing my hat?
    total_height_meters = hat_height_cm + my_height_cm / 100
    print("Height in meters =", total_height_meters, "?")

    输出>>>：
    Height in meters = 26.9 ?
```

括号在这里很有用。它可以改变Python原有的运算顺序。  

``` python
    total_height_meters = (hat_height_cm + my_height_cm) / 100
    print("Height in meters =", total_height_meters)

    输出>>>：
    Height in meters = 2.15
```

## 处理数的内置函数

min和max分别返回其参数的最小值和最大值...

``` python
    print(min(1, 2, 3))
    print(max(1, 2, 3))

    输出>>>：
    1
    3
```

abs返回它参数的绝对值：

``` python
    print(abs(32))
    print(abs(-32))

    输出：
    32
    32
```

除了作为Python的两个主要数字类型的名称之外，int和float也可以作为将其参数转换为相应类型的函数：

``` python
    print(float(10))
    print(int(3.33))
    # They can even be called on strings!
    print(int('807') + 1)

    输出：
    10.0
    3
    808
```

## 该你了

现在，轮到你了！尝试你的[第一个Python编程练习](https://www.kaggle.com/scratchpad/kernel4b59e810f7/edit)
