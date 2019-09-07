# Hello，Python

## 简介

这个课程覆盖了python的核心技能，因此你可以使用python开始你的数据科学学习之旅。这个课程非常适合有编程经验的人，他们希望将Python添加到自己的技能库中，或者提高自己基本的Python技能(如果你之前没接触过编程，你可能需要查看这些"**[Python for Non-Programmers](https://wiki.python.org/moin/BeginnersGuide/NonProgrammers)**"学习资源。)

我们将从Python语法的简要概述，变量赋值以及算法运算符开始讲起。如果你有Python的基础，你可以直接跳过去**[着手练习](https://www.kaggle.com/kernels/fork/1275163)**。

## 你好，Python!

Python是以英国喜剧剧团"**[巨蟒剧团](https://en.wikipedia.org/wiki/Monty_Python)**"命名的，因此我们使用第一次Python程序向他们表示敬意。

为了好玩，你阅读一下下面的代码并预测一下运行时会发生什么。(如果你想不出来，没关系！)

点击输出按钮查看程序输出结果。

### In [1]

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

这个程序演示了Python许多重要的东西，以及它是怎样工作的。让我们从上到下来检查代码。

### In [2]

``` text
spam_amount = 0
```

**变量赋值**：我们创建一个变量`spam_amout`并用`=`将其赋值为0，这个过程叫做赋值运算。

> 旁白：如果你使用其它编程语言(java或C++)，你可能需要注意一些Python不需要在这里做的东西：
>
>- 在给变量赋值之前不需要声明变量
>- 你不需要告诉Python将要引进的是哪种类型的值`spam_amout`。事实，我们甚至可以重回分配`spam_amount`来引用其它类型，比如字符串或布尔值。

### In [3]

``` python
print(spam_amount)
```

### Out [3]

```text
0
```

**函数调用**：```print```是一个Python函数，它在屏幕上显示传递给它的值。我们通过在函数名后面加上括号来调用该函数，把输入(参数)放在括号里。

### In [4]

``` python
# Ordering Spam, egg, Spam, Spam, bacon and Spam (4 more servings of Spam)
spam_amount = spam_amount + 4
```

上面第一行是注释，注释以```#```开始。

接下来我们看一个重新分配的例子。重新分配现有变量的值和创建变量看起来是一样的，它也是用```=```作为赋值运算符。

在这种情况下，我们分配给```spam_amount```的值涉及到对其先前值的一些简单算数。当运行到这一行，Python计算```=```(0+4=4)右边的表达式，然后赋值给左边变量。

### In [5]

``` python
if spam_amount > 0:
    print("But I don't want ANY spam!")

viking_song = "Spam Spam Spam"
print(viking_song)
```

### Out [5]

``` text
But I don't want ANY spam!
Spam Spam Spam
```

我们将在稍后讨论'条件语句'，即使你以前没有写过条件语句，但是很可能猜到是怎样回事。Python因其可读性和简单性而被赞誉。

注意，我们如何指出哪些代码属于```if```。```But I don't want ANY spam!```仅在`span_amount`为正数的条件下被输出。但是后面的代码(像```print(viking_song)```)在任何条件下都会被输出。我们(和Python)是怎样知道的？

在```if```后面的```:```表示一个'新的代码块'正在运行。后面缩进的行也是代码的一部分。一些其它语言用```{}```标记代码块的开始和结束。对于习惯于其它语言的程序员来说，Python使用有意义的空格可能使他们感到惊讶。实际上，与不强制代码缩进的语言相比，它可以更加一致、可读性更强。

后来对待```viking_song```不需要缩进4个空格，因此它不属于```if```的代码块。当我们定义函数并使用循环时，代码缩进是很常见的。

这段代码是我们第一次在Python中看到字符串。

``` text
"But I don't want ANY spam!"
```

字符串可以用双引号或单引号表示。但是这是一个特别的字符串，因为它包含了一个单引号符号，如果我们不小心，我们会用单引号包围Python，因此而混淆。

### In [6]

``` python
viking_song = "Spam " * spam_amount
print(viking_song)
```

### Out [6]

``` text
Spam Spam Spam Spam
```

这个```*```运算符表示两个数字相乘（```3*3```等于9），但是有趣的是，我们也可以用一个数字乘以字符串，得到很多重复次的字符串。Python提供了一些节省时间的小技巧，就像```*```和```+```根据它们应用于什么类型的对象而有不同含义。(术语为操作符重载)

## Python中数字和算数

我们在上面已经看到了包含数字变量的例子。

### In [7]

``` python
spam_amount = 0
```

```Number```一个非正式的名称，它表示一类事情。但是如果想要变的更专业，我们可以通过Python来查看```spam_amount```变量是什么类型。

### In [8]

``` python
type(spam_amount)
```

### Out [8]

``` text
int
```

`int`是整形的缩写。在Python中我们经常会遇到另一种类型的数字。

### In [9]

``` python
type(19.25)
```

### Out [9]

``` text
float
```

浮点数带有小数点，对于表示权重或比例之类的东西非常有用。

```type()```是我们看到的第二个内置函数(第一个```print()```),这又是一个值得记住的例子。它经常被用于查看变量的数据类型。

数字常做的事就是参与算数。我们已经学过```+```运算符和```*```乘法运算符。Python也提供了其它可以在计算器上实现的算数的运算符。

<table  style="width:100%">
	<tr>
		<td>
			Operator
		</td>
		<td>
			Name
		</td>
		<td>
			Description
		</td>
	</tr>
	<tr>
		<td>
			a + b
		</td>
		<td>
			Addition
		</td>
		<td>
			Sum of a and b
		</td>
	</tr>
	<tr>
		<td>
			a - b
		</td>
		<td>
			Subtraction
		</td>
		<td>
			Difference of a and b
		</td>
	</tr>
	<tr>
		<td>
			a * b
		</td>
		<td>
			Multiplication
		</td>
		<td>
			Product of a and b
		</td>
	</tr>
	<tr>
		<td>
			a / b
		</td>
		<td>
			True division
		</td>
		<td>
			Quotient of a and b
		</td>
	</tr>
	<tr>
		<td>
			a // b
		</td>
		<td>
			Floor division
		</td>
		<td>
			Quotient of a and b, removing fractional parts
		</td>
	</tr>
	<tr>
		<td>
			a % b
		</td>
		<td>
			Modulus
		</td>
		<td>
				Integer remainder after division of a by b
		</td>
	</tr>
	<tr>
		<td>
			a ** b
		</td>
		<td>
			Exponentiation
		</td>
		<td>
				a raised to the power of b
		</td>
	</tr>
	<tr>
		<td>
			-a
		</td>
		<td>
			Negation
		</td>
		<td>
				The negative of a
		</td>
	</tr>
</table>

这里有一个有趣的发现，虽然计算器可能只有一个除法按钮，但Python可以做两种。“真正的除法”基本上就是你的计算器所做的。

### In [10]

``` python
print(5 / 2)
print(6 / 2)
```

### Out [10]

``` text
2.5
3.0
```

总是得到浮点数。

用```//```运算符得到不大于结果的一个整数。

### In [11]

``` python
print(5 // 2)
print(6 // 2)
```

### Out [11]

``` text
2
3
```

你能想到它的用处在哪吗？你很快将会在编程挑战中看到一个实例。

**运算顺序**

我们在小学学的算术有计算运算顺序的惯例。有些人用彭达斯(PEMDAS)——括号、指数、乘法/除法、加法/减法等助记符来记住这些。

Python也遵循类似的规则，即先执行哪些计算。大多很直观。

### In [12]

``` python
8 - 3 + 2
```

### Out [12]

```text
7
```

### In [13]

``` python
-3 + 4 * 2
```

### Out [13]

```text
5
```

有时运算顺序不是我们想象中的那样。

### In [14]

``` python
hat_height_cm = 25
my_height_cm = 190
# How tall am I, in meters, when wearing my hat?
total_height_meters = hat_height_cm + my_height_cm / 100
print("Height in meters =", total_height_meters, "?")
```

### Out [14]

``` text
Height in meters = 26.9 ?
```

圆括号在这里有很多用处。您可以将它们添加到强制Python以您想要的任何顺序计算子表达式。

### In [15]

``` python
total_height_meters = (hat_height_cm + my_height_cm) / 100
print("Height in meters =", total_height_meters)
```

### Out [15]

``` text
Height in meters = 2.15
```

**有关处理数字的内置函数**

min和max分别返回它们的参数的最小值和最大值。

### In [16]

``` python
print(min(1, 2, 3))
print(max(1, 2, 3))
```

### Out [16]

```text
1
3
```

```abs```返回参数的绝对值

### In [17]

``` python
print(abs(32))
print(abs(-32))
```

### Out [17]

``` text
32
32
```

除了是Python的两种主要数值类型的名称之外，```int```和```float```作为函数用于把参数转化为对应的类型。

### In [18]

``` python
print(float(10))
print(int(3.33))
# They can even be called on strings!
print(int('807') + 1)
```

### Out [18]

``` text
10.0
3
808
```

## You Turn

现在机会交给你。开始你的第一个[Python编程练习](https://www.kaggle.com/scratchpad/kernel70c341272e/edit)。
