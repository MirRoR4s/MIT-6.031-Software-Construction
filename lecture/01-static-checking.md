<!-- # Reading 1: Static Checking -->
# 静态检查

<!-- ## Objectives -->

## 目标

<!-- Today’s class has two topics: -->
本篇文章包括以下两个主题：

<!-- - static typing  -->
- 静态类型

<!-- - the big three properties of good software -->
- 好的软件应具有的三大性质

---

<!-- ## Hailstone sequence -->
## 冰雹序列

<!-- As a running example, we’re going to explore the hailstone sequence, which is defined as follows. Starting with a number *n*, the next number in the sequence is *n/2* if *n* is even, or *3n+1* if *n* is odd. The sequence ends when it reaches 1. Here are some examples: -->
冰雹序列：从一个数字 **n** 开始，如果 n 是偶数，那么序列的下一项是 **n/2**，如果 n 是奇数，那么序列的下一项是 **3n + 1**，当 n 到达 1 时序列结束。以下是一些例子：

```java
2, 1
3, 10, 5, 16, 8, 4, 2, 1
4, 2, 1
2n, 2n-1 , … , 4, 2, 1
5, 16, 8, 4, 2, 1
7, 22, 11, 34, 17, 52, 26, 13, 40, …? (where does this stop?)
```

<!-- Because of the odd-number rule, the sequence may bounce up and down before decreasing to 1. It’s conjectured that all hailstones eventually fall to the ground – i.e., the hailstone sequence reaches 1 for all starting *n* – but that’s still an [open question](https://en.wikipedia.org/wiki/Collatz_conjecture). Why is it called a hailstone sequence? Because hailstones form in clouds by bouncing up and down, until they eventually build enough weight to fall to earth. -->
由于奇数规则，所以 n 在到达 1 之前可能会忽大忽小。对所有从 n 开始的冰雹序列来说，n 最终是否一定会到达 1 是一种假设，这是一个开放性的[问题](https://en.wikipedia.org/wiki/Collatz_conjecture)。

为什么将该序列称为冰雹序列呢？因为冰雹是在云层中忽上忽下而形成的，当它们到达了足够的重量，就会跌落到地面，这和当前的序列有异曲同工之妙，所以就称为冰雹序列了。

<!-- ## Computing hailstones -->
## 计算冰雹序列

<!-- Here’s some code for computing and printing the hailstone sequence for some starting *n*. We’ll write Java and Python side by side for comparison: -->

以下 Python 和 Java 代码用于计算和打印从 n 开始的冰雹序列。

```java
// Java
int n = 3;
while (n != 1) {
    System.out.println(n);
    if (n % 2 == 0) {
        n = n / 2;
    } else {
        n = 3 * n + 1;
    }
}
System.out.println(n);
```

---

```python
# Python
n = 3
while n != 1:
    print(n)
    if n % 2 == 0:
        n = n / 2
    else:
        n = 3 * n + 1


print(n)
```

<!-- A few things are worth noting here: -->

此处有一些要点值得指出：

<!-- - The basic semantics of expressions and statements in Java are very similar to Python: `while` and `if` behave the same, for example. -->
- Java 表达式和语句的基本语义与 Python 非常相似，比如 `while` 和 `if` 语句都具有相同的行为。

<!-- - Java requires semicolons at the ends of statements. -->
- Java 要求在每个语句的结尾添加分号。

<!-- - Java requires parentheses around the conditions of the `if` and `while`. -->
- Java 要求用括号包裹 `if` 和 `while` 的条件语句。

<!-- - Java uses curly braces around blocks, instead of indentation. You should always indent the block, even though Java won’t pay any attention to your extra spaces. Programming is a form of communication, and you’re communicating not only to the compiler, but to human beings. Humans need that indentation. We’ll come back to this later. -->
- Java 使用花括号而非缩进包裹代码块。但即使 Java 不关心额外的空格，我们还是应对代码块进行缩进。因为程序是一种交流的形式，我们不仅在和编译器交流，还在和人类交流，而人类是需要缩进的！

<!-- ## Types -->
## 类型

<!-- The most important semantic difference between the Python and Java code above is the declaration of the variable `n`, which specifies its type: `int`. -->

上述 Python和 Java 代码之间最重要的语义差别在于变量 `n` 的声明。Java 在声明变量 `n` 时指定了其类型：`int`。

<!-- A **type** is a set of values, along with operations that can be performed on those values. -->

**类型**是一些值以及可对这些值执行的操作组成的集合。

<!-- Java has several **primitive types**, among them: -->
Java 有一些原始数据类型，包括：

<!-- - `int` (for integers like 5 and -200, but limited to the range about ± 231, or roughly ± 2 billion) -->

- `int`：表示 5 和 200 这样的整数，但是其范围**大概**在正负 $2^{31}$ 之间

<!-- - `long` (for larger integers up to about ± 263) -->

- `long`：表示更大的整数，范围大概在正负 $2^{63}$ 之间

<!-- - `boolean` (for true or false) -->

- `boolean`：表示 true 或 false，即真和假

<!-- - `double` (for floating-point numbers, which represent a subset of the real numbers) -->

- `double`：表示浮点数，是实数的一个子集

<!-- - `char` (for single characters like `'A'` and `'$'`) -->

- `char`：表示 `'A'` 或 `'$'` 这样的单个字符

<!-- Java also has **object types**, for example: -->
Java 也有**对象类型**，比如：

<!-- - `String` represents a sequence of characters, like a Python string. -->
`string` 表示一个字符序列，类似于 python 的字符串。

<!-- - `BigInteger` represents an integer of arbitrary size, so it acts like a Python integer. -->
`BigInteger` 表示一个任意大小的整数，类似于 python 的整数。

<!-- By Java convention, primitive types are lowercase, while object types start with a capital letter. -->
根据 Java 惯例，原始类型小写，但对象类型首字母大写。

<!-- **Operations** are functions that take inputs and produce outputs (and sometimes change the values themselves). The syntax for operations varies, but we still think of them as functions no matter how they’re written. Here are three different syntaxes for an operation in Python or Java: -->

**操作**实际上是一种函数——接收输入然后产生输出（有时也会改变输入本身）。操作可能会有不同的语法形式，但无论它们的编写方式如何，我们都可将它们视为函数。以下是 Python 或 Java 中某个操作的三种不同语法形式：

<!-- - *As an operator.* For example, `a + b` invokes the operation `+ : int × int → int`. -->

- 作为运算符。举个例子，`a + b` 执行了操作 `+ : int x int -> int`。<!-- (In this notation: `+` is the name of the operation, `int × int` before the arrow describes the two inputs, and `int` after the arrow describes the output.) -->（在该操作中，`+` 是操作的名称，箭头前的 `int x int` 描述了操作的两个输入，箭头后的 int 描述了操作的输出。）

<!-- - *As a method of an object.* For example, `bigint1.add(bigint2)` calls the operation `add: BigInteger × BigInteger → BigInteger`. -->
- 作为对象的方法。举个例子，`bigint1.add(bigint2)` 调用了操作 `add: BigInteger x BigInteger -> BigInteger`。

<!-- - *As a function.* For example, `Math.sin(theta)` calls the operation `sin: double → double`. Here, `Math` is not an object. It’s the class that contains the `sin` function. -->

- 作为函数。举个例子，`Math.sin(theta)` 调用了操作 `sin: double -> double`。此处的 `Math` 并不是一个对象，而是一个包含了 `sin` 函数的类。

<!-- Contrast Java’s `str.length()` with Python’s `len(str)`. It’s the same operation in both languages – a function that takes a string and returns its length – but it just uses different syntax. -->
对比一下 Java 的 `str.length()` 和 python 的 `len(str)`，它们实际上对应相同的操作——一个接收字符串并返回其长度的函数，但有不同的语法形式。

<!-- Some operations are **overloaded** in the sense that the same operation name is used for different types. The arithmetic operators `+`, `-`, `*`, `/` are heavily overloaded for the numeric primitive types in Java. Methods can also be overloaded. Most programming languages have some degree of overloading. -->
可以对操作进行重载，在这种情况下，不同的类型使用相同的操作名。在 Java 中，原始数字类型大量重载了 `+`，`-`，`*`，`/` 。（比如 int 和 double 都用 `*` 表示乘法运算）除了可以重载操作之外，还可以重载方法。注意到大部分的编程语言都有一定程度的重载。

<!-- ## Static typing -->
## 静态类型

<!-- Java is a **statically-typed language**. The types of all variables are known at compile time (before the program runs), and the compiler can therefore deduce the types of all expressions as well. If `a` and `b` are declared as `int`, then the compiler concludes that `a+b` is also an `int`. The Eclipse environment does this while you’re writing the code, in fact, so you find out about many errors while you’re still typing. -->
Java 是一种**静态类型**的语言，所有变量的类型在编译时（在程序运行之前）就已知晓，所以编译器可以推断出所有表达式的类型。如果 `a`、`b` 都被声明为 `int`，那么编译器就可以推断出 `a+b` 也是 `int。`

<!-- In **dynamically-typed languages** like Python, this kind of checking is deferred until runtime (while the program is running). -->
在像 python 这样的**动态类型语言**中，这种检查被推迟到运行时进行。（即程序运行的时候）

<!-- Static typing is a particular kind of **static checking**, which means checking for bugs at compile time. Bugs are the bane of programming. Many of the ideas in this course are aimed at eliminating bugs from your code, and static checking is the first idea that we’ve seen for this. Static typing prevents a large class of bugs from infecting your program: to be precise, bugs caused by applying an operation to the wrong types of arguments. If you write a broken line of code like: -->

静态类型是一种特殊的**静态检查**，会在编译时就检查 bug 的存在。静态类型可以预防一类 bug 干扰你的程序：更准确地说，是预防在执行某种操作时由于参数类型错误而产生的 bug。如果你编写了一个如下的代码：

```java
"5" * "6"
```

<!-- that tries to multiply two strings, then static typing will catch this error while you’re still programming, rather than waiting until the line is reached during execution. -->
该代码尝试将两个字符串相乘，当你还在编程时，静态类型就会捕获到该错误，而无需等到执行该代码时。

<!-- ### Support for static typing in dynamically-typed languages -->

### 在动态类型语言中添加静态类型支持

<!-- Even though Python is dynamically-typed, Python 3.5 and later allow you to declare [type hints](https://www.python.org/dev/peps/pep-0484/) in the code, e.g.: -->
尽管 python 是一种动态类型的语言，但在 python 3.5 之后的版本，我们可以在代码中声明[类型提示](https://www.python.org/dev/peps/pep-0484/)以此指定变量和返回值的类型，具体如下所示：

```python
# Python function declared with type hints
def hello(name:str)->str:
    return 'Hi, ' + name
```

<!-- The declared types can be used by a checker like [Mypy](http://mypy-lang.org/) to find type errors statically without having to run the code. -->
类似于 [Mypy](http://mypy-lang.org/) 这样的检查工具可以在不运行代码的情况下利用声明的类型来静态地寻找类型错误。

<!-- Other dynamically-typed languages have similar extensions. For example, JavaScript has been extended with static typing to create the language [TypeScript](https://www.typescriptlang.org/), e.g.: -->
其他动态类型的语言也有类似的扩展。比如为 JavaScript 语言添加静态类型扩展就创造了 [TypeScript](https://www.typescriptlang.org/)语言，具体如下所示：

```javascript
// TypeScript function
function hello(name:string):string {
    return 'Hi, ' + name;
}
```

<!-- The addition of static types to these dynamically-typed languages reflects a widespread belief among software engineers that the use of static types is essential to building and maintaining a large software system. The rest of this reading, and in fact this entire course, will show reasons for this belief. Compared to a language like Java that is statically-typed from the outset, adding static types to a dynamically-typed language enables a programming approach called [gradual typing](https://en.wikipedia.org/wiki/Gradual_typing), in which some parts of the code have static type declarations and other parts omit them. Gradual typing can provide a smoother path for a small experimental prototype to grow into a large, stable, maintainable system. -->
为这些动态类型语言添加静态类型支持，反映了一种在软件工程师中广泛传播的理念即静态类型的使用对于构建和维护大型软件系统来说是很重要的。对比于像 Java 这样的静态类型语言，为一种动态类型语言添加静态类型支持创造了一种名为[渐进式类型化](https://en.wikipedia.org/wiki/Gradual_typing)的编程方法，该方法在代码的某些部分添加静态类型声明，而其他部分则略去。

渐进式类型化可为一个小的试验原型逐步演变为为一个大型、稳定、可维护的系统提供更光滑的路径。

<!-- ## Static checking, dynamic checking, no checking -->
## 静态检查，动态检查，无检查

<!-- It’s useful to think about three kinds of automatic checking that a language can provide: -->
可将编程语言提供的自动化检查分为如下三类：

<!-- - **Static checking**: the bug is found automatically before the program even runs. -->

- 静态检查：在程序运行之前自动发现 bug。

<!-- - **Dynamic checking**: the bug is found automatically when the code is executed. -->

- 动态检查：当程序在运行时自动发现 bug。

<!-- - **No checking**: the language doesn’t help you find the error at all. You have to watch for it yourself, or end up with wrong answers. -->

- 无检查：编程语言完全不帮你寻找错误。你必须自己寻找 bug 或是最终得到错误的结果。

<!-- Needless to say, catching a bug statically is better than catching it dynamically, and catching it dynamically is better than not catching it at all. -->
毋庸置疑，静态捕获 bug 比动态捕获 bug 要好，而动态捕获 bug 又比完全不捕获 bug 要好。

<!-- Here are some rules of thumb for what errors you can expect to be caught at each of these times. -->
以下是一些规则，梳理了在这三种检查中能够捕获到的错误。

<!-- **Static checking** can catch: -->
**静态检查**可以捕获：

<!-- - syntax errors, like extra punctuation or spurious words. Even dynamically-typed languages like Python do this kind of static checking. If you have an indentation error in your Python program, you’ll find out before the program starts running. -->

- 语法错误，例如多余的标点符号或关键字用法不当。即使是像 Python 这样的动态类型语言也会进行此类静态检查。如果你在 Python 程序中有一个缩进错误，那么在你程序开始运行之前就可以发现它。

<!-- - misspelled names, like `Math.sine(2)`. (The correct spelling is `sin`.) -->
- 拼写错误，例如 `Math.sine(2)`，正确的写法是 `sin`。

<!-- - wrong number of arguments, like `Math.sin(30, 20)`. -->

- 参数个数错误，例如 `Math.sin(30, 20)`。

<!-- - wrong argument types, like `Math.sin("30")`. -->

- 参数类型错误，例如 `Math.sin("30")`。

<!-- - wrong return types, like `return "30";` from a function that’s declared to return an `int`. -->
- 返回类型错误，比如一个声明了返回类型为 `int` 的函数却返回了 `"30"`。

<!-- **Dynamic checking** can catch: -->
**动态检查**可以捕获：

<!-- - illegal argument values. For example, the integer expression `x/y` is only erroneous when `y` is actually zero; for other values of `y`, its value is well-defined. So in this expression, divide-by-zero is not a static error, but a dynamic error. -->

- 非法参数值。举个例子，整数表达式 `x/y` 仅当 `y` 为零时才会产生错误；对于其他的 `y` 值，表达式的值是有明确定义的，所以在这个表达式中，除数为零不是一个静态错误，而是一个动态错误。

<!-- - illegal conversions, i.e., when the specific value can’t be converted to or represented in the target type. For example, `Integer.valueOf("hello")` is a dynamic error, because the string `"hello"` cannot be parsed as a decimal integer. So is `Integer.valueOf("8000000000")`, because 8 billion is outside the legal range of `int` values in Java. -->

- 非法转换，即当特定值无法被转换或表示成目标值时。举个例子，`Integer.valueOf("hello")` 是一个动态错误，因为字符串 `"hello"` 无法被解析为一个十进制整数。对 `Integer.valueOf("800000000")` 来说也是如此，因为在 Java 中 8 亿超出了 `int` 能够表示的范围。

<!-- - out-of-range indices, e.g., using a negative or too-large index on a string. -->

- 索引越界，例如使用了负数或是一个超大的数来索引字符串。

<!-- - calling a method on a `null` object reference (`null` is like Python’s `None`). -->

- 在 `null` 对象引用上调用一个方法（`null` 类似于 Python 的 `None`）

<!-- Static checking can detect errors related to the **type** of a variable -- the set of values it is allowed to have -- but is generally unable to find errors related to a specific value from that type. Static typing guarantees that a variable will have **some** value from its type, but we don’t know until runtime exactly which value it has. So if the error would be caused only by certain values, like divide-by-zero or index-out-of-range, then the compiler won’t raise a static error about it. -->
静态检查可以检测到和变量的类型相关的错误，但通常情况下无法发现和该类型相关的特定值的错误。静态类型保证了一个变量必定具有来自其类型的某个值，但我们只有在运行的时候才可以确切地知道这个值是什么，所以如果错误是由特定值引起的，比如除数为零或是索引越界，那么编译器将不会抛出一个关于它的静态错误。

<!-- Dynamic checking, by contrast, tends to be about errors caused by specific values. -->
相反地，动态检查往往能够发现和特定值相关的错误。

---

<!-- ### Surprise: primitive types are not true numbers -->

### 原始类型并不是真正的数字

<!-- One trap in Java -- and many other programming languages -- is that its primitive numeric types have corner cases that do not behave like the integers and real numbers we’re used to. As a result, some errors that really should be dynamically checked are not checked at all. Here are the traps: -->
在 Java 和其他许多编程语言中都有这样一个问题：原始数值类型的边界情况与我们日常使用的整数和实数具有不同的行为，这导致的后果是一些本应被动态检查到的错误没有被检查到。如下是一些具体示例：

<!-- - **Integer division**. `5/2` does not return a fraction, it returns a truncated integer. So this is an example of where what we might have hoped would be a dynamic error (because a fraction isn’t representable as an integer) frequently produces the wrong answer instead. -->

- **整数除法**。`5/2` 不会返回一个分数，而是返回一个截断了小数部分的整数。这是一个我们期望产生动态错误的例子（因为分数不可以被表示成整数），而不是产生一个错误的结果。

<!-- - **Integer overflow**. The `int` and `long` types are actually finite sets of integers, with maximum and minimum values. What happens when you do a computation whose answer is too positive or too negative to fit in that finite range? The computation quietly **overflows** (wraps around), and returns an integer from somewhere in the legal range but not the right answer. -->

- **整数溢出**。`int` 和 `long` 这两种类型实际上是一个有限的整数集合，其有最大值和最小值。所以当我们进行了某种计算，并且结果的大小超出了集合的范围，那么在这种情况下计算就会静默地溢出，然后从集合的有效范围中返回一个整数值，注意这通常不是正确的结果。

<!-- - **Special values in floating-point types**. Floating-point types like `double` have several special values that aren’t real numbers: `NaN` (which stands for “Not a Number”), `POSITIVE_INFINITY`, and `NEGATIVE_INFINITY`. So when you apply certain operations to a `double` that you’d expect to produce dynamic errors, like dividing by zero or taking the square root of a negative number, you will get one of these special values instead. If you keep computing with it, you’ll end up with a bad final answer. -->

- **浮点类型中的特殊值**。像 `double` 这样的浮点类型有一些不为实数的特殊值：`NaN`（表示 "Not a Number"），`POSITIVE_INFINITY`，`NEGATIVE_INFINITY`。当对一个 double 变量执行了某种操作，但该操作的除数为零或是求取了负数的平方根，那么你就会得到上述的特殊值。（这就是问题所在，本应产生动态错误的值却产生了上述的特殊值）如果说你继续用得到的结果进行后续的计算，那么你最终会得到一个错误的答案。

### 阅读练习

阅读以下存在 bug 的代码并查看它们在 Java 中的行为。这些 bug 可以被静态、动态地捕获到吗？还是说完全无法被捕获。

#### 1

```java
int n = -5;
if (n) {
    System.out.println("n is a positive integer");
}
```

- [x] static error
- [ ] dynamic error
- [ ] no error, wrong answer

<!-- This is a static type error in Java, because the if statement requires an expression of boolean type, but n has int type. Changing the code to if (n > 0) ... would fix both the type error and the bug. -->
注意到 if 的条件语句要求一个布尔类型的表达式，但是代码中的 n 是一个 int 类型，所以编译器会报出一个静态类型的错误。将表达式改为 n > 0 即可修复该错误。

#### 2

```java
int big = 200000; // 200,000
big = big * big; // big should be 40 billion now
```

- [ ] static error
- [ ] dynamic error
- [x] no error, wrong answer

注意到 big 是一个 int 类型的变量，40 亿已然超出了 int 可表示的范围，不过由于 Java 语言的设计缺陷，这并不会报出错误，而是返回一个错误的值。

<!-- This is an integer overflow, because an int value can’t represent a number bigger than 231 (about 2 billion). It isn’t caught statically, but unfortunately in Java it isn’t caught dynamically either. Integer overflows quietly produce the wrong answer. -->

<!-- Some newer programming languages, like Swift, have rethought this design decision and produce a dynamic error instead. Others, like Python and Ruby, avoid the problem by automatically using unlimited-precision integers where necessary. -->
一些更新的的编程语言，比如说 Swift，重新思考了相关设计决定，并针对这种情况选择产生一个动态错误。不过像 Python 和 Ruby 这样的语言通过在必要时使用无精度限制的整数避免了该问题。

#### 3

```java
double probability = 1/5;
```

<!-- If the programmer’s intent was to get 0.2, this is using the wrong operation. / is overloaded for both integer division and floating point division. But because it’s being called with integers, 1 and 5, Java chooses integer division, and it quietly truncates the fraction and produces the wrong answer: 0. -->
如果程序员的期望结果是 0.2，那么上述代码就使用了错误的操作。/ 在整数除法和浮点数除法中进行了重载。因为代码使用了 1 和 5 来调用 /，所以此处 Java 会使用整数的除法，所以 Java 会默默地把小数点部分截取然后得到一个错误答案：0。

#### 4

```java
int sum = 0;
int n = 0;
int average = sum/n;
```

除数为零，很明显的动态错误。

#### 5

```java
double sum = 7;
double n = 0;
double average = sum/n;
```

<!-- Division by zero can’t produce a real number either – but unlike real numbers, double has a special value for POSITIVE_INFINITY, so that’s what it returns when you divide a positive integer by zero. If the code is trying to compute an average value, infinity is unlikely to be a correct or useful answer. -->

Java 浮点数除法，除数为零时会得到特殊值，而不会产生错误，所以上代码会给出一个错误的结果。

---

<!-- ## Arrays and collections -->
## 数组和集合

<!-- Let’s change our hailstone computation so that it stores the sequence in a data structure, instead of just printing it out. Java has two kinds of list-like types that we could use: arrays and Lists. -->
修改前面计算冰雹序列的代码，将序列存储在一个数据结构中，而不是直接打印它。Java 有两种类似于列表的类型可供我们使用：数组（arrays） 和 列表（Lists）。

<!-- Arrays are fixed-length sequences of another type, like integers. For example, here’s how to declare an array variable and construct an array value to assign to it: -->
类似于整数，arrays（数组）是一种定长的序列类型。举个例子，以下代码演示了如何声明一个数组变量并构造一个数组值对其进行赋值：

```java
int[] a = new int[100];
```

<!-- The `int[]` array type includes all possible array values, but a particular array value, once created, can never change its length. Operations on array types include: -->
`int[]` 数组类型包括了所有可能的数组值，注意一个特定的数组值一旦被创建，其长度就再也不可以更改。对数组类型的操作包括：

<!-- - indexing: `a[2]` -->
- 索引：`a[2]`

<!-- - assignment: `a[2] = 0` -->

- 赋值：`a[2] = 0`

<!-- - length: `a.length` -->
- 获取长度：`a.length`

<!-- Note that `a.length` is different syntax from `String.length()`. Because `a.length` is an instance variable, rather than a method call, you don’t put parentheses after it. -->
注意到 `a.length` 的语法和 `String.length()` 是不同的。因为 `a.length` 是一个实例变量，而非方法调用，所以不能在它后面加上括号。

<!-- Here’s a crack at the hailstone code using an array. We start by constructing the array, and then use an index variable `i` to step through the array, storing values of the sequence as we generate them. -->
下面是使用数组完成计算冰雹序列的代码。我们首先构造一个数组，然后利用索引变量 i 来遍历这个数组，当我们生成一个序列项时就将其存放到数组中。

```java
int[] a = new int[100];  // <==== DANGER, WILL ROBINSON!
int i = 0;
int n = 3;
while (n != 1) {
    a[i] = n;
    i++;  // very common shorthand for i=i+1
    if (n % 2 == 0) {
        n = n / 2;
    } else {
        n = 3 * n + 1;
    }
}
a[i] = n;
i++;
```

<!-- Something should immediately smell wrong in this approach. What’s that magic number 100? What would happen if we tried an `n` that turned out to have a very long hailstone sequence? It wouldn’t fit in a length-100 array. We have a bug. Would Java catch the bug statically, dynamically, or not at all? Incidentally, this kind of bug is called a [buffer overflow](https://en.wikipedia.org/wiki/Buffer_overflow), because it is overflowing a fixed-length array. Fixed-length arrays are commonly used in less-safe languages like C and C++ that don’t do automatic runtime checking of array accesses. Buffer overflows have been responsible for a large number of network security breaches and internet worms. -->
上述代码存在一个问题，如果冰雹序列的长度超过了 100，那么我们声明的数组（长度为 100）将无法容纳下该序列，所以代码存在一个 bug。Java 能够捕获到该 bug 吗？

>  顺便说一下，此类 bug 被称为[缓冲区溢出](https://en.wikipedia.org/wiki/Buffer_overflow)，因为其在一个定长的数组中发生了溢出。像 C 和 C++ 这样不对数组访问自动执行运行时检查的语言就大量使用了定长数组。缓冲区溢出导致了大量的网络安全漏洞和互联网蠕虫。

<!-- Instead of a fixed-length array, let’s use the `List` type. Lists are variable-length sequences of another type. Here’s how we can declare a `List` variable and make a list value: -->
所以我们最好使用 `List`（列表） 类型，而不是定长的数组类型。列表是一种可变长度的序列类型。以下代码演示了如何声明一个 List 类型的变量 list。

```java
List<Integer> list = new ArrayList<Integer>();
```

<!-- And here are some of its operations: -->

List 类型支持的操作如下：

<!-- - indexing: `list.get(2)` -->

- 索引：`list.get(2)`

<!-- - assignment: `list.set(2, 0)` -->

- 赋值：`list.set(2,0)`

<!-- - length: `list.size()` -->

- 获取长度：`list.size()`
<!-- Why `List` on the left but `ArrayList` on the right? `List` is an interface, a type that can’t be constructed directly, but that instead just specifies the operations that a `List` must provide, like `get()` and `set()` and `size()`. `ArrayList` is a class, a concrete type that provides implementations of those operations. `ArrayList` isn’t the only implementation of the `List` type, though; `LinkedList` is another. So we prefer the `List` type when declaring variable types or return types, because it allows the code to be more general and flexible, not caring which concrete kind of list is actually being used. We will revisit this idea of interfaces and implementation classes several times throughout the course, so it’s okay if you don’t understand it deeply yet. -->
为什么代码左侧是 `List`，但右侧却是 `ArrayList` 呢？`List` 实际上是一个接口，这是一种无法直接构造的类型，但它指定了一个 `List` 必须提供的操作，比如 `get()`、`set()`、`size()` 等等。`ArrayList` 是一个类，这是一个具体的类型，提供了上述操作的具体实现。`ArrayList` 并不是 `List` 类型的唯一实现，`LinkedList` 同样也实现了 `List`。当我们在声明变量或返回值的类型时，我们倾向于使用 List，因为这会使得代码更加的通用和灵活，而无需关心具体使用了哪种类型的列表。

<!-- You can see all the operations of `List` or the details of `ArrayList` or `LinkedList` in the Java API documentation: find it with a web search for “Java 15 API.” Get to know the Java API docs, they’re your friend. (“API” means “application programming interface,” and here it refers to the methods and classes that Java provides to help you build Java applications.) -->
可以在 Java 的 API 文档中查看所有 `List` 支持的操作或是 `ArrayList` 和 `LinkedList` 的具体信息：使用 web 搜索 “Java 15 API”。最好了解一下 Java 的 API 文档，它们是你的朋友。（API 的意思是应用程序编程接口，此处指的是 Java 提供给我们的用于构建 Java 应用的类和方法）

<!-- Why `List<Integer>` and not `List<int>`? Unfortunately, we can’t write `List<int>`. Lists only know how to deal with object types, not primitive types. Each primitive type has an equivalent object type: e.g. `int` and `Integer`, `long` and `Long`, `float` and `Float`, `double` and `Double`. Java requires us to use these object type equivalents when we parameterize one type using another. But in other contexts, Java automatically converts between `int` and `Integer`, so we can write `Integer i = 5;` without any type error. -->
为什么代码是 `List<Integer>`，而不是 `List<int>`？因为 Java 不允许我们这么编写代码。Lists 只知道如何处理对象类型，而不知道如何处理原始类型。每个原始类型都有一个等价的对象类型。比如和 int 等价的对象类型是 Integer，和 long 等价的对象类型是 Long，和 float 等价的对象类型是 Float，和 double 等价的对象类型是 Double。当我们用一种类型来参数化另一种类型时，Java 要求我们使用这些等价的对象类型。但在其他的情况下，Java 会自动地进行 `int` 和 `Integer` 的转换，所以我们可以直接编写 `Integer i = 5` 而不会引发类型错误。

<!-- Here’s the hailstone code written with lists: -->
以下是用列表类型对计算冰雹序列代码的重写：

```java
List<Integer> list = new ArrayList<Integer>();
int n = 3;
while (n != 1) {
    list.add(n);
    if (n % 2 == 0) {
        n = n / 2;
    } else {
        n = 3 * n + 1;
    }
}
list.add(n);
```

<!-- Not only simpler but safer too, because the list automatically enlarges itself to fit as many numbers as you add to it (until you run out of memory, of course). -->
上述代码不仅更加简单，而且还更加安全，因为 list 列表会自动扩容以容纳我们添加的数字(当前，也不可能无限地扩容，当内存耗尽时就会停止)。

<!-- ## Iterating -->
## 迭代

<!-- A `for` loop steps through the elements of an array or a `List`, just as in Python, though the syntax looks a little different. For example: -->
与 python 一样，for 循环可以用来遍历数组或列表中的元素，不过 java 的语法稍微有些不同，具体如下：

```java
// find the maximum point of a hailstone sequence stored in list
int max = 0;
for (int x : list) {
    max = Math.max(x, max);
}
```

<!-- You can iterate through arrays as well as lists. The same code would work if the list were replaced by an array. -->
遍历数组的方式和遍历列表一样，你只需要把列表换成数组即可。

<!-- `Math.max()` is a handy function from the Java API. The `Math` class is full of useful functions like this – web search “Java 15 Math” to find its documentation. -->
Math.max() 是一个来自 Java API 的函数。Math 类有很多像这样的实用函数——web 搜索 “Java 15 Math” 可以找到相关的文档。

<!-- ## Methods -->
## 方法

<!-- In Java, statements generally have to be inside a method, and every method has to be in a class, so the simplest way to write our hailstone program looks like this: -->
在 Java 中，语句通常是在一个方法内，而每个方法都必须要在一个类内，所以编写冰雹程序最简单的方式看起来应是如下的样子：

```java
public class Hailstone {
    /**
     * Compute a hailstone sequence.
     * @param n  starting number for sequence; assumes n > 0.
     * @return hailstone sequence starting with n and ending with 1.
     */
    public static List<Integer> hailstoneSequence(int n) {
        List<Integer> list = new ArrayList<Integer>();
        while (n != 1) {
            list.add(n);
            if (n % 2 == 0) {
                n = n / 2;
            } else {
                n = 3 * n + 1;
            }
        }
        list.add(n);
        return list;
    }
}
```

<!-- Let’s explain a few of the new things here. -->
解释一下上述代码：

<!-- `public` means that any code, anywhere in your program, can refer to the class or method. Other access modifiers, like `private`, are used to get more safety in a program, and to guarantee immutability for immutable types. We’ll talk more about them in an upcoming class. -->
public 意味着程序中的任何代码都可以引用该类或方法。其他的访问修饰符，比如说 private 用于在程序中获得更高的安全性以及保证不可变类型的不可变性。

<!-- `static` means that the method is a function that doesn’t take a `self` parameter (which in Java is called `this`, and is passed implicitly, so you won’t ever see it as an explicit method parameter). Static methods aren’t called on an object. Contrast that with the `List.add()` method or the `String.length()` method, for example, which require an object to come first. Instead, the right way to call a static method uses the class name instead of an object reference: `Hailstone.hailstoneSequence(83)`. -->
static 意味着该方法是一个没有 `self` 参数的函数。（`self` 在 Java 中被称为 `this`，并且是隐式传递的，所以你不会显式地看到它作为一个方法的参数）与 `List.add()` 或 `String.length()` 这两个需要一个对象的方法不同，static 方法不是在一个对象上调用的。调用一个 static 方法的正确方式是使用类名而非对象引用：`Hailstone.hailstoneSequence(83)`。

<!-- Take note also of the blue `/** ... */` comment before the method, because it’s very important. This comment is a specification of the method, describing the inputs and outputs of the operation. The specification should be concise, clear, and precise. The comment provides information that is not already clear from the method types. It doesn’t say, for example, that `n` is an integer, because the `int n` declaration just below already says that. But it does say that `n` must be positive, which is not captured by the type declaration but is very important for the caller to know. -->
注意方法上方的蓝色注释 `/** ... */`，因为这非常重要。注释是方法的规范，描述了操作的输入和输出，规范应该要简洁、清晰、精确。

当前方法的注释提供了从方法的类型声明中无法直接知晓的信息。例如尽管它没有说明 `n` 是一个整数（因为下方的 `int n` 已经说明了这一点），但它说到 `n` 必须是一个正数，这个信息并没有在类型声明中体现出来，但对于方法的调用者来说，这却是一个必须要知道的非常重要的信息。

<!-- We’ll have a lot more to say about how to write good specifications in a few classes, but you’ll have to start reading them and using them right away. -->
> 如何编写好的规范？我还在学习中，以后会补充上的。

<!-- ## Mutating values vs. reassigning variables -->

## 可变值 vs. 可重赋值变量

<!-- Change is a necessary evil（改变是必要之恶）. But good programmers try to avoid things that change, because they may change unexpectedly. Immutability – intentionally forbidding certain things from changing at runtime – will be a major design principle in this course. -->
改变是必要之恶，但是好的程序员尝试避免变化的事物，因为事物的变化是不可预料的。不可变性——人为地禁止特定事物在运行时改变。

<!-- For example, an immutable type is a type whose values can never change once they have been created. The string type is immutable in both Python and Java. -->
举个例子，不可变类型是指那些一旦被创建后值就再也不能改变的类型。string 类型在 Python 和 Java 中都是不可变的。

<!-- Java also gives us immutable references: variables that are assigned once and never reassigned. To make a reference unreassignable, declare it with the keyword `final`: -->
Java 也为我们提供了不可变引用：一旦被赋值后就再也不可以被重新赋值的变量。为了创建一个不可重新赋值的引用，可以用 `final` 关键字来声明它，如下代码所示：

```java
final int n = 5;
```

<!-- If the Java compiler isn’t convinced that your `final` variable will only be assigned once at runtime, then it will produce a compiler error. So `final` gives you static checking for unreassignable references. -->
如果 Java 编译器不相信你的 `final` 变量在运行时仅会被赋值一次，那么编译器就会产生一个编译错误，所以 `final` 关键字可以用来对不可重新赋值的引用进行静态检查。

<!-- It’s good practice to use `final` for declaring the parameters of a method and as many local variables as possible. Like the type of the variable, these declarations are important documentation, useful to the reader of the code and statically checked by the compiler. -->
将方法的参数声明为 `final` 并尽可能地使用局部变量是一种好的实践。和变量的类型一样，这些声明是重要的记录，对代码的阅读者和编译器的静态检查来说十分有用。

---

<!-- ## Documenting assumptions -->
## 记录假设

<!-- Writing the type of a variable down documents an assumption about it: e.g., this variable will always refer to an integer. Java actually checks this assumption at compile time, and guarantees that there’s no place in your program where you violated this assumption. -->
写下一个变量的类型，并记录下有关该变量的假设，比如变量总引用一个整数。Java 实际上会在编译时检查该假设，并保证你的程序没有违反它。

<!-- Declaring a variable `final` is also a form of documentation, a claim that the variable will never be reassigned after its initial assignment. Java checks that too, statically. -->
将一个变量声明为 `final` 也是某种形式的记录。这实际上是一种宣称：变量在初始赋值后就再也不会被重新赋值，Java 也会静态地检查这种情况。

<!-- We documented another assumption that Java (unfortunately) doesn’t check automatically: that `n` must be positive. -->
我们还记录下了 Java 不会自动检查的另一个假设：`n` 必须是正数。

<!-- Why do we need to write down our assumptions? Because programming is full of them, and if we don’t write them down, we won’t remember them, and other people who need to read or change our programs later won’t know them. They’ll have to guess. -->
为什么需要写下我们的假设呢？因为整个程序都是基于这些假设，如果我们不写下它们，那么我们将很快忘记。并且以后其他需要阅读或修改我们程序的人也无法知晓这些假设，所以它们只能猜测。

<!-- Programs have to be written with two goals in mind: -->
在编写程序时需要牢记以下两个目标：

<!-- - communicating with the computer. First persuading the compiler that your program is sensible – syntactically correct and type-correct. Then getting the logic right so that it gives the right results at runtime. -->
和计算机进行交流。首先让编译器相信你的程序是合理的——语法正确以及类型正确。然后使程序在逻辑上是正确的，以便在运行时能够得到正确的结果。

<!-- - communicating with other people. Making the program easy to understand, so that when somebody has to fix it, improve it, or adapt it in the future, they can do so. -->
和其他人进行交流。使程序易于理解，以便他人在未来需要修复、改进或是采用你的代码时，他们可以做到。

<!-- ## Hacking vs. engineering

We’ve written some hacky code in this reading. Hacking is often marked by unbridled optimism:

- Bad: writing lots of code before testing any of it
- Bad: keeping all the details in your head, assuming you’ll remember them forever, instead of writing them down in your code
- Bad: assuming that bugs will be nonexistent or else easy to find and fix

But software engineering is not hacking. Engineers are pessimists:

- Good: write a little bit at a time, testing as you go. In a future class, we’ll talk about test-first programming.
- Good: document the assumptions that your code depends on
- Good: defend your code against stupidity – especially your own! Static checking helps with that. -->

---

<!-- ## Summary -->
<!-- The main idea we introduced today is **static checking**. Here’s how this idea relates to the goals of the course: -->
<!-- 
- **Safe from bugs.** Static checking helps with safety by catching type errors and other bugs before runtime.
- **Easy to understand.** It helps with understanding, because types are explicitly stated in the code.
- **Ready for change.** Static checking makes it easier to change your code by identifying other places that need to change in tandem. For example, when you change the name or type of a variable, the compiler immediately displays errors at all the places where that variable is used, reminding you to update them as well. -->
