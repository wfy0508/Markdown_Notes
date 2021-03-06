# Scala编程

## 1 一种可伸缩语言

1 定义Map类型变量

```scala
var capital = Map("US" -> "Washington", "France" -> "Pairs")
capital += ("Japan" -> "Tokyo") //增加一对KV
println(capital("Japan")) //打印Japan对应的value
返回：Tokyo
```

2 定义函数

```scala
def factorial(x: BigInt): BigInt = {
    if (x==0) 1 else factorial(x - 1) * x
}
//如果函数体内只有条件语句，可以不用{}
def factorial(x: BigInt): BigInt = if (x==0) 1 else factorial(x-1) * x

factorial(30) //调用
res8: BigInt = 265252859812191058636308480000000
```

BigInt看上去像是Scala内建的类型，因为可以使用字面量，并且可以对类型做*和-操作，但实际上BigInt是Scala标准库里定义的一个类而已。就算没有这个类，Scala也可以直接实现，比如对`java.math.BigInteger`做下包装。

```scala
import java.math.BigInteger

def facrotial(x: BigInteger): BigInteger = {
    if (x == BigInteger.ZERO) BigInteger.ONE
    else x.multiply(factorial(x.subtract(BigInteger.ONE)))
}
```

Scala是面向对象的，每个值都是对象，每个操作都是方法的调用，举例来说，如果你说1+2，实际上是调用Int类里定义的名为+的方法。
Scala是函数式的：

- 第一理念是函数是一等(first class)的值，函数的地位和整数，字符串是相同的。可以作为参数传递给其他函数，作为返回值返回它们，或者将它们保存在变量里。

- 另一个核心理念是方法不应该有`副作用`。方法只能通过接收参数和返回结果两种方式与外部环境通信，意思是对于任何特定的输入，该方法调用都可以被其他结果替换，同时不会影响程序的语义。

## 2 Scala入门

### 2.1 定义变量

Scala的变量分为两种：var和val，val跟Java的final变量类似，初始化后就不能被重新赋值；而var则不同，类似于Java的非final变量，在整个生命周期之内都可以被重新赋值。

```scala
val msg = "Hello World!"
msg: String = Hello World!
```

定义变量不用指定类型，Scala中的类型推断 (type inference) 能够推断出那些不显式执行的类型。

### 2.2 定义函数

```scala
def max(x: Int, y:Int): Int = {
    if (x > y) x else y
}
```

参数列表之后的“：Int”是函数自己的结果类型（result type）,有时Scala编译器需要你给出函数的结果类型，比如，如果函数是`递归的`，就必须显示地给出函数的结果类型，在max函数中，并不需要给出结果类型，编译器也会做出正确的推断。`如果函数体只有一条语句，也可以选择不适用花括号`。
下面是一个不接受任何参数也不返回任何有意义的结果的函数：

```scala
def greet() = println("Hello, World!")

greet: ()Unit
```

当你定义greet()函数时，解释器会以greet(): Unit作为响应。greet是函数的名称，空的圆括号代表该函数不接受任何参数，而Unit是greet返回的结果。`Unit这样的结果类型，表示该函数并不返回任何有意义的结果`。跟Java的void类型类似。

### 2.3 编写Scala脚本

将下面的代码放入`hello.scala`的文件中

```scala
println("Hello, world, from a script!")
```

然后执行：

```bash
$scala hello.scala

返回： Hello, world, from a script!
```

命令行参数可以通过名为args的Scala数组获取，Scala的数组下标从0开始，一个名为steps的数组，第一个元素为`steps(0)`，而Java的写法为`steps[0]`。

### 2.4 while循环，if判断

```scala
var i = 0
while (i < args.length) { //while或if中的boolean表达式必须在圆括号内
    println(args(i))
    i += 1 //注意Java中的++i和i++在Scala中不工作
}
```

### 2.5 用foreach和for遍历

```scala
args.foreach(arg => println(arg))
```

arg的类型会自动推断，但也可以更加明确地表达，也可以指出类型名，不过这时候需要将标量和类型放进括号内（这是函数字面量的常规语法）。

```scala
args.foreach((arg: String) => println(arg))
```

`如果函数字面量只接受单个参数，可以不必给出参数名和参数本身`:

```scala
args.foreach(println)
```

`函数字面量的语法总结`:用圆括号括起来的一组带名字的参数，一个右箭头，和函数体。

```scala
(x: Int, y: Int) => x + y
```

Scala只支持指令式for语句的函数式亲戚（`for表达式`）

```scala
for (arg <- args) //arg是val类型，只能写成arg，不能写成val arg，<-念“in”
  println(arg)
```

## 3 Scala入门（续）

### 3.1 用类型参数化数组

用new来实例化对象或类的实例。当实例化对象时，可以用值和类型对其进行参数化（parameterize）。

```scala
val big = new java.math.BigInteger("12345")
```

也可以用类型来参数一个实例，做法是在`方括号`里给出一个或者多个类型，greetStrings的类型是`Array[String]`，而不是`Array[String](3)`。

```scala
val greetStrings = new Array[String](3)

greetStrings(0) = "hello"
greetStrings(1) = "world"
greetStrings(2) = "!"

for (i <- 0 to 2) //to方法返回的并不是一个数组，而是另一种序列，包含了0,1,2
  print(greetStrings(i))
```

Scala中技术上讲并`没有操作符重载（operator overloading）`,因为实际上`并没有传统意义上的操作符`。类似+、-、*、/这样的字符都可以作为方法名。在命令行中键入1+2，实际上是调用名为+的方法，实现方式为`(1).+(2)`。

Scala为什么使用圆括号来访问数组，跟Java相比，Scala的特例更少。`数组不过是类的实例`，这一点跟其他Scala实例没有本质区别。当你用圆括号将一个或者多个值包起来，并将其应用（apply）到某个对象时，Scala将这段代码转换成对这个对象应用名为apply的方法调用。所以`greetStrings(0)会被转换为greetStrings.apply(i)`。**因此在Scala中访问数组元素就是一个简单的方法调用，跟其他方法调用一样**。

当对变量进行赋值时，相当于调用了update方法，greetingStrings(0) = "Hello"会被转换为greetingStrings.update(0, "Hello")。

**Scala在编译代码时，会尽量使用Java数组，基本类型和原生的算术指令**。

更加精简的方法来初始化数组，实际上是调用了一个名为apply的中场方法，这个方法创建并返回了新的数组。这个apply方法接受一个变长的列表，该方法定义在Array的伴生对象（companion object)中。

```scala
val numNames = Array("zero", "one", "two")
```

### 3.3 使用列表

```scala
val oneTwoThree = List(1, 2, 3) //不用写new List，因为scala.List的伴生对象上定义了一个工厂方法
```

`List是不可变的`，类似于Java的String，当操作了一个看上去会改变列表的时候，实际上会返回一个带有新值的列表，`列表中的元素类型必须全部相同`。**List有个方法叫做“:::”，用于列表拼接**。

```scala
val oneTwo = List(1, 2)
val threeFour = List(3, 4)
val oneTwoThreeFoue = oneTwo ::: threeFour
```

**List上使用最有的方法为“::”，用于列表添加元素**。

```scala
val oneTwo = List(2, 3)
val oneTwoThree = 1 :: oneTwo
```

**注意**， ::是右操作元的方法，在1::oneTwo中，::方法调用发生在oneTwo上，传入的参数是1，就像这样oneTwo.::(1)。

`标识空列表的快捷方式为Nil`。**初始化一个列表时使用::，当结尾必须有Nil**。

```scala
val oneTwoThree = 1 :: 2 :: 3 :: Nil
```

### 3.4 使用元组

另一个容易对象是元组（tuple），元组也是不可变的，跟List不同之处在于，元组中的元素类型可以不同。

```scala
val pair = (99, "Hello")
println(pair._1) //访问字段_1，产出第1个元素。
println(pair._2) //访问字段_2，产出第2个元素。
```

Scala会推断出pair的类型为`Tuple2[Int, String]`，这里的“.”跟用于访问字段或调用方法时使用的方式相同。`元组的实际类型取决于包含的元素以及元素的类型`。pair的类型为Tuple2[Int, String]，那么元组('u', 't', 'the', 1, 4)的类型为Tuple5[Char, Char, String, Int, Int]。

我们为什么不能使用pair(0)来访问元素？这是因为列表的apply方法永远只返回同一种类型，但元组中的类型可以不同，_1可以是一种类型，_2可能是另一种类型，_N表示的字段名从1开始而不是0。

### 3.5 使用集合和映射

Scala还提供了Set和Map的可变（mutable）与不可变（immutable）的不同选择。Scala的API包含了一个基础的特质（trait）来表示集，如下图，三个特质都叫做Set，但是他们完整的名称并不相同，因为他们位于不同的包。如果想要使用一个HashSet，可以根据需要选择可变或不可变的版本。

```plain txt
                   ┌────────────────┐
                   │scala.collection│
                   │      Set       │
                   │   <<trait>>    │
                   └────────────────┘
                            ▲
                            │
               ┌────────────┴───────────┐
               │                        │
┌──────────────────────────┐ ┌─────────────────────────┐
│scala.collection.immutable│ │scala.collection.mutable │
│         HashSet          │ │         HashSet         │
│        <<trait>>         │ │        <<trait>>        │
└──────────────────────────┘ └─────────────────────────┘
             ▲                           ▲
             │                           │
┌──────────────────────────┐ ┌─────────────────────────┐
│scala.collection.immutable│ │scala.collection.mutable │
│         HashSet          │ │         HashSet         │
└──────────────────────────┘ └─────────────────────────┘
```

创建Set的默认方式：

```scala
var jetSet = Set("Boeing", "Airbus")
//jetSet: scala.collection.immutable.Set[String] = Set(Boeing, Airbus) 默认定义的是一个不可变的Set

jetSet += "Lear"
println(jetSet.contains("Cessna"))

false
```

定义并初始化了一个`不可变集`，通过调用Set的伴生对象的名为apply的工厂方法，上面例子中，实际上调用了scala.collection.immutable.Set的伴生对象的apply方法，犯规一个默认的、不可变的Set对象。Scala编译器推断出jetSet的类型为不可变的Set[String]。

要向Set添加新元素，可以对Set调用+方法，传入新元素。不论是可变还是不变的Set，+方法都会创建并返回一个新的包含了新元素的Set。`可变集合提供一个实际的+=的方法，但不可变集没有提供该方法`。

如果要定义一个可变集，需要导入一个包。

```scala
import scala.collection.mutable
val movieSet = mutable.Set("Hitch", "Poltergeist")
movieSet += "Shrek"
movieSet += "Shrek"
println(movieSet)
```

使用Map:

如果在定义一个空的Map时，下例中的[Int, String]是必须的，因为此时编译器在没有数据的情况下，无法推断出变量的类型。

```scala
import scala.collection.mutable

var treasureMap = mutable.Map[Int, String]()
treasureMap += (1, "Go to island") //调用了->的方法
treasureMap += (2, "Find big X on the ground")
treasureMap += (3, "Dig.")
println(treasureMap(2))
```

默认的Map类型为`immutable`。

```scala
// Map:scala.collection.immutable.Map
val romanNumeral = Map(1 -> "I", 2 -> "II", 3 -> "III", 4 -> "IV", 5 -> "V")
println(romanNumeral(4))
```

如果直接使用数据初始化一个Map，[Int, String]不是必须的，因为此时编译器可以推断出变量的类型。

### 3.6 认识函数式编程

区别指令式编程和函数式编程的主要特征是，`代码中是否包含var变量`。函数式编程的代码中尽量或全部使用val变量。

之前的例子：

```scala
def printArgs(args: Array[String]): Unit = {
    var i = 0
    while (i <= args.length){
        println(args(i)
        i += 1
    }
}
```

其中包含var变量，可以将其改为函数式编程

```scala
def printArgs(args: Array[String]):Unit = {
    for (arg <- args){
        println(arg)
    }
}

//或者
def printArgs(args: Array[String]): Unit = {
    args.foreach(println)
}
```

可以看出，函数式编程的代码更加简洁清晰，不易出错。这就是Scala更加鼓励编写函数式风格的代码。

但是，更进一步看，上面的例子不完全是函数式编程，因为它产生了副作用，向控制台打印输出了内容。`有副作用的函数的特征是它的结果类型是Unit`。如果一个函数不返回任何有意义的值，那么结果类型就是Unit类型。一个更加函数式编程的方式是给入参做某种格式化的操作来代替输出无意义的内容。

```scala
def formatArgs(args: Array[String])=args.mkString("\n")
```

没有副作用的函数，也就不能给外界提供有意义的值。所以在设计程序是要尽量少的产生副作用，这样你的程序更容易调试。

### 3.7 从文件读取记录行

```scala
import scala.io.Source

if (args.length > 0){
    for (line <- Source.fromFile(args).getLines())
      println(line.length + " " + line)
}
else
  Console.err.println("Please enter fileName")
```

## 4 类和对象

定义一个Class

```scala
class CheckSumAccumulator {
    var sum = 0
}
```

实例化，acc和csa指向不同的对象，由于acc和csa都是val类型，所以不能给他们赋予新的对象。

```scala
val acc = CheckSumAccumulator
acc: CheckSumAccumulator = CheckSumAccumulator@7f2d681a

val csa = CheckSumAccumulator
csa: CheckSumAccumulator = CheckSumAccumulator@1c83652d

acc = new CheckSumAccumulator
<console>:14: error: reassignment to val
```

因为class中的sum是var类型，可以给其重新赋值

```scala
acc.sum = 3
```

### 4.1 定义私有变量

```scala
class CheckSumAccumulator{
    private var sum = 0
}
```

定义为private变量之后，不能从类的外部去修改，只能由内部定义的方法去修改。

```scala
val acc = new CheckSumAccumulator
acc.sum = 1

//error: variable sum in class CheckSumAccumulator cannot be accessed in CheckSumAccumulator
```

```scala
class CheckSumAccmumlator{
    private var sum = 0
    def add(b: Byte): Unit = {
        sum += b //方法中的参数b是val
    }
    def checkSum(): Int = {
        return ~(sum & 0xFF) + 1
    }
}
```

**注意**，`方法中的参数都是val类型`。

```scala
def add(b: Byte): Unit = {
    b = 1 //执行报错，因为b不能被赋值
    sum += b
}
```

上述CheckSumAccmumlator的定义还可以进一步精简。首先return是不必要的，Scala方法会自动返回方法中最后一个被计算的值。Scala推荐的编程模式是尽量避免明显的，多重的return语句。可以`将方法看作是生成值的一个表达式`，这种思想鼓励将很大的放大定义拆分成小的方法实现。而且当方法中只有一条语句时，可以省略{}。

```scala
class CheckSumAccumulator{
    private var sum = 0
    def add(b: Byte): Unit = sum+=b
    def checkSum(): Int = ~(sum & 0xFF) +1
}
```

一个仅因其副作用而执行的方法（返回值类型为Unit的函数）称为`过程(procedure)`。类CheckSumAccumulator中的add方法，就是需要其执行之后的产生副作用。

### 4.2 分号推断

在Scala中，如果一行有多个语句，需要中间有分号隔开

```scala
val s = "hello"; println(s)
```

如果一行只有一个语句，分号不是强制需要的

```scala
if (x < 2)
  println("too small")
else
  println("ok")
```

Scala会将一个语句拆分成两个部分，如果像下面的写法

```scala
x
+y
```

除非加上括号：

```scala
(x
+y)
```

或者写成：

```scala
x+
y+
z
//将操作符至于行末是Scala style。
```

### 4.3 单例对象

**`Scala的类中不能定义static成员`**，取而代之的是，可以定义`单例对象`（Singleton objects)。单例对象的定义类似类的定义，把关键词class换成object。当单例对象的名字和类的名字相同时，单例对象成为类的`伴生对象（companion object）`，类称为单例对象的`伴生类（companion class）`。在定义两者时，必须置于同一个源文件中，类和它的伴生对象可以互相访问对方的private成员。

```scala
//CheckSumAccumulator.scala

class CheckSumAccumulator{
    private var sum = 0
    def add(b: Byte): Unit = sum+=b
    def checkSum(): Int = ~(sum & 0xFF) +1
}

import scala.collection.mutable
object CheckSumAccumulator{
    private val cache = mutable.Map.empty[String, Int]
    def calculate(s: String): Int = {
        if (cache.contains(s)){
            cache(s)
        }
        else {
            val acc = new CheckSumAccumulator
            for (c <- s)
              acc.add(c.toByte)
            val cs = acc.checkSum()
            cache += (s -> cs)
            cs
        }
    }
}
```

单例对象和类的另一个区别是，`单例对象不能携带参数，但类可以，因为不能通过new来实例化一个单例对象`。每个单例对象都被实现为一个从静态变量引用的合成类的实例，它的初始化语法和Java的static是一样的，当某些代码首次访问它时，就会被初始化。

没有伴生类的单例对象，称为独立对象（standalone object）。

### 4.4 Scala应用

要运行一个.scala代码，需要定义main方法和它的参数Array[String]，Unit的返回类型。

```scala
// in file Summer.scala

import CheckSumAccumulator.calculate
object Summer{
    def main (args: Array[String]){
        for(arg <- args)
          println(arg + " " + calculate(arg))
    }
}
```

编译Scala脚本

```bash
scalac CheckSumAccumulator.scala Summer.scala
```

这将编译源文件，但是在编译完成之前可能会有明显的延迟。原因是每次编译器启动时，它都要花时间扫描jar文件的内容，甚至在查看提交给它的新源文件之前就进行其他初始工作。出于这个原因，Scala发行版还包括一个名为fsc的Scala编译器守护进程(用于快速Scala编译器)。

```bash
fsc ChecksumAccumulator.scala Summer.scala
```

第一次运行fsc时，它将创建一个附加到计算机端口的本地服务器守护进程。然后，它将通过端口将要编译的文件列表发送给守护进程，守护进程将编译这些文件。下一次运行fsc时，该守护进程将已经在运行，因此fsc将简单地将文件列表发送给该守护进程，该守护进程将立即编译文件。

运行Scala程序

```bash
scala Summer of love

# of: -213
# love: -182
```

### 4.5 应用特质

Scala提供了一种trait，可以节省部分代码的书写，如下

```scala
import CheckSumAccmumlator.calculate

object FallWinterSpringSummer extends App{
    for (season <- List("fall", "winter", "spring"))
    println(season + " " + calculate(season))
}
```

使用extends App可以替代main方法，你把原来放入main方法中的代码，放置其中即可。

## 5 基本类型和操作符

### 5.1 基本类型

| 基本类型 | 取值范围                    |
| -- | --|
| Byte     | 8bit ($─2^{7}, 2^{7}─1$)    |
| Short    | 16bit ($─2^{15}, 2^{15}─1$) |
| Int      | 32bit ($─2^{31}, 2^{31}─1$) |
| Long     | 64bit ($─2^{63}, 2^{63}-1$) |
| Char     | 16bit ($0, 2^{16}-1$)       |
| String   | 字符序列                    |
| Float    | 32bit 单精度                |
| Double   | 64bit 双精度                |
| Boolean  | true & false                |

Scala拥有与Java完全相同的类型，这使得编译器能够将Scala值类型(例如Int或Double)的实例转换为它生成的字节码中的Java基本类型。

### 5.2 字面量

#### 5.2.1 整数字面量

```scala
val hex = 0x5
hex: Int = 5

val hex2 = 0x00FF
hex2: Int = 255

val magic = 0xcafebabe
magic: Int = -889275714
```

无论你定义的是几进制的整型数据，Scala shell总是以10进制打印整型值。如果整型字面量以L或l结尾，那么它就是Long类型，否则为Int类型。

```scala
scala> val prog = 0XCAFEBABEL
prog: Long = 3405691582

scala> val tower = 35L
tower: Long = 35
```

如果将一个Int字面量赋给一个类型为Short或Byte的变量，那么只要该字面量在该类型的有效范围内，该字面量就被视为Short或Byte类型。

#### 5.2.2 浮点型字面量

```scala
scala> val big = 1.2345
big: Double = 1.2345

scala> val bigger = 1.2345e1
bigger: Double = 12.345

scala> val biggerStill = 123E45
biggerStill: Double = 1.23E47
```

如果字面量以F或f结尾，那么它就是Float类型，否则为Double类型，也可以在结尾加上D或者d，来代表Double类型。

```scala
scala> val little = 1.2345F
little: Float = 1.2345

scala> val littleBigger = 3e5f
littleBigger: Float = 300000.0

scala> val anotherDouble = 3e5
anotherDouble: Double = 300000.0

scala> val yetAnother = 3e5D
yetAnother: Double = 300000.0
```

#### 5.2.3 字符字面量

字符是由单引号之间的任何Unicode字符组成的。

```scala
scala> val a = 'A'
a: Char = A

scala> val d = '\u0041'
d: Char = A

scala> val f = '\u0044'
f: Char = D
```

Unicode可以出现在程序的任何地方，比如

```scala
scala> val B\u0041\u0044 = 1
BAD: Int = 1
```

最后，还有一些用特殊的转移序列表示的字面值

```scala
scala> val backslash = '\\'
backslash: Char = \
```

|字面量|含义|
|--|--|
|\n |换行 (\u000A)|
|\b |退格 (\u0008)|
|\t |制表符 (\u0009)|
|\f |换页 (\u000C)|
|\r |回车 (\u000D)|
|\\" |双引号 (\u0022)|
|\\' |单引号 (\u0027)|
|\\\ |反斜杠 (\u005C)|

#### 5.2.4 字符串字面量

```scala
scala> val hello = "hello"
hello: String = hello
```

因为这种语法对于包含大量转义序列或跨多行字符串的字符串来说比较笨拙，所以Scala为原始字符串提供了一种特殊的语法。

```scala
println("""Welcome to Ultamix 3000.
           Type "HELP" for help.""")

Welcome to Ultamix 3000.
      Type "HELP" for help.
```

如果想要对齐，可以在每行之前加上`|`，会自动调用stripMargin。

```scala
println("""|Welcome to Ultamix 3000.
           |Type "HELP" for help.""".stripMargin)

Welcome to Ultamix 3000.
Type "HELP" for help.
```

#### 5.2.5 符号字面量

符号字面量通常被写为 `’ident`，**ident**可以使任何的字符或数字标识。这样的字面量被映射到预定义类scala.Symbol的实例中，具体来说，字面量`'cymbal`将由编译器扩展为工厂方法调用:Symbol("cymbal")。符号字面量通常用于动态语言场景中。比如，定义一个方法去更新数据库中的记录。

```scala
def updateRecordByName(r: Symbol, value: Any) = {
   // code goes here
}
updateRecordByName: (Symbol,Any)Unit
```

该方法使用定义的两个参数，作为记录的字段名和要更新的值，在动态类型语言中，可以调用此操作，将未声明的字段标识符传递给方法。

```scala
scala> updateRecordByName(favoriteAlbum, "OK Computer")
<console>:6: error: not found: value favoriteAlbum       updateRecordByName(favoriteAlbum, "OK Computer")
ˆ
Instead, and almost as concisely, you can pass a symbol literal:
```

但Scala中不能直接使用，要使用以下方式。

```scala
updateRecordByName('favoriteAlbum, "OK Computer")
```

对于一个符号，除了获取其名字，也没啥可操作的。

```scala
scala> val s = 'aSymbol
s: Symbol = 'aSymbol

scala> s.name
res1: String = aSample
```

#### 5.2.6 布尔字面量

```scala
scala> val bool = true
bool: Boolean = true

scala> val fool = false
fool: Boolean = false
```

### 5.3 字符串插值

Scala提供了一种灵活的字符串插值机制，允许将字符串插入到文本中，它最常见的用例是提供一种简洁易读的字符串连接方法。

```scala
val name = "reader"
println(s"Hello, $name!")

Hello, reader
```

可以将任何表达式放在经过处理的字符串文字中的美元符号($)之后，Scala将把第一个非标识符字符之前的所有字符解释为表达式

```scala
scala> s"The answer is ${6 * 7}."

res0: String = The answer is 42.
```

Scala默认提供了另外两个字符串插入器，`raw`和`f`。`raw`类似于`s`，但它`不识转义字符`。

```scala
println(raw"No\\\\escape!")

//会打印4个反斜杠，而不是两个。
```

`f`允许给内嵌表达式加上`printf`风格的指令，需要将指令放在表达式之后。

```scala
f"${math.Pi}%.5f"

res5: String = 3.14159
```

在Scala中，字符串插值在编译时会自动重写代码。编译器会将任何由一个标识符紧跟着一个字符串字面量的双引号组成的表达式视为一个字符串内插表达式。s、f和raw等字符串插值是通过这种通用机制实现的。库和用户可以为了其他目的，定义其他字符串插入器。

### 5.4 操作符即方法

例如，`1+2`实际上跟`1.+(2)`是一回事。Int类中定义了一个名为+的方法，接受一个Int参数，返回Int的结果。

```scala
val sum = 1+ 2
sum: Int = 3

val sumMore = 1.+(2)
sumMore: Int = 3
```

事实上，Int包含了多个`重载（overloaded）`的+方法，分别接收不同的参数。如接收一个Long，返回一个Long。+符号是一个操作符（更确切的叫中缀操作符）

- 前缀操作符：需要将方法名放在你要调用的方法的对象前面，如”-7“中的“-”；
- 中缀操作符：接收两个操作元，一左一右；
- 后缀操作符：放在对象之后。

前缀和中缀操作符类似，都是`一元的（unary）`，只接收一个操作元。

跟中缀操作符类似，这些前缀操作符也是调用方法的一种简写。不同的是，方法名称是`“unary_”`加上操作符。

```scala
scala> -2.0
res: Double = -2.0

scala> (2.0)unary_-
res: Double = -2.0

//在使用unary_之前需要导入包：
import scala.language.postfixOps
```

**`唯一被用作前缀操作符的是+、-、！和~`**。

后缀操作符是那些不接收参数并且在调用时没有英文句点(.)圆括号的方法。在Scala中，可以将方法调用时省去空的圆括号。从约定俗成的角度讲，`如果方法有副作用的时候保留空的圆括号`，如println()；`而方法没有副作用时，则可以省略圆括号`.

```scala
scala> val s = "Hello, World"
s: String = Hello, World

scala> s.toLowerCase
res4: String = hello, world
```

`在后一种不带参数的场景（无副作用）下，可以选择去掉句点，使用后缀操作符表示法`。

```scala
scala> s toLowerCase
res4: String = hello, world
```

### 5.5 对象相等性

比较连个对象是否相等，使用`==`和与之相反的`！=`。

```scala
scala> 1 == 2
res6: Boolean = false

scala> 1 != 2
res7: Boolean = true

scala> List(1, 2, 3) == List(1, 2, 3) //列表比较
res8: Boolean = true

scala> List(1, 2, 3) == List(2, 3, 4)
res9: Boolean = false

scala> 1 == 1.0
res10: Boolean = true

scala> List(1, 2, 3) == "hello" //不同类型也可比较
res11: Boolean = false

scala> List(1, 2, 3) == null //也可与null比较，不会报错
res12: Boolean = false
```

**比较规则**：`首先检查作则是否为null，如果不为null，调用equals方法。由于equals是个方法，得到的确切比较逻辑取决于左侧的参数。`

比较逻辑对于不同的对象，`只要他们的内容一致，且equals方法的实现也是基于内容的情况下，都会给出true的答案`。

Scala与Java的不同，对于基本类型而言，==比较的是值的相等性，对于引用类型，比较的是引用的相等性（两个变量指向JVM的堆上同一个对象）。Scala也提供了比较引用的机制，即为`eq`方法。**`eq和其对应的ne只对那些直接映射到Java对象的对象有效`**。

### 5.6 操作符优先级

|操作符优先级|
|--|
|（所有其他操作符）|
|* / %|
|+ -|
|:|
|= !|
|< >|
|&|
|^|
|\ ||
|(所有字母)|
|(所有赋值操作符)|

## 6 函数式对象

### 6.1 构建Rational

一个类如果没有定义提，不需要给出空的花括号。

```scala
class Rational(n: Int, d: Int)
```

n和d成为类参数（class parameter），Scala会采集到这两个参数，并且创建一个主构造方法（primary constructor)，接收同样的两个参数。

**在Java中，类有构造方法，构造方法可以接收参数，而在Scala中，类可以直接接收参数，Scala的类更为精简（类定义体内可以直接接收参数，不需要定义字段并编写构造方法，参数赋值给字段的代码）**。

Scala编译器会将在类定义体中给出的非字段或方法定义的代码编译进主构造方法中。

```scala
class Rational(n: Int, d: Int){
    println("Created " + n + "/" + d)
}
```

这样会将println方法放在主构造方法中，每创建一个实例，就会触发println方法。

```scala
scala> new Rational(1, 2)
Created 1/2
res13: Rational = Rational@58e9e852
```

### 6.2 重新实现toString方法

在上例中，解释器打印出`Rational@58e9e852`，解释器是通过对Rational对象调用toString方法会获取这个看上去很奇怪的字符串。Rational类默认继承了`java.lang.Object`类的`toString`实现，简单地打印出类名、@符号和十六进制的数字。

`toString`主要是帮助程序员调试输出语句、日志消息、测试失败报告、以及解释器和调试器输出中给出相应的信息。

目前给出的输出结果并不是特别有帮助。可以通过`重写（override）`来实现打印出分子分母。

```scala
scala> class Rational(n: Int, d: Int){
     | override def toString = n + "/" + d
     | }
defined class Rational

scala> val x = new Rational(1, 3)
x: Rational = 1/3

scala> val y = new Rational(5, 7)
y: Rational = 5/7
```

### 6.3 检查前置条件

如果定义除法，就要首先要求分母不能为0。解决这种问题的最佳方式是对主构造方法定义一个`前置条件`，分母d不能为0。实现方式的一种使用`require`：

```scala
scala> class Rational(n: Int, d: Int){
     | require (d != 0)
     | override def toString = n + "/" + d
     | }
defined class Rational

scala> new Rational(1, 0) //如果实例化时，分母为0，就会抛出IllegalArgumentException异常
java.lang.IllegalArgumentException: requirement failed
  at scala.Predef$.require(Predef.scala:268)
  ... 29 elided
```

### 6.4 添加字段

为Rational类添加一个add方法，接收另一个Rational作为参数，为了保持Rational不可变，需要创建并返回一个新的持有这两个有理数的和的Rational对象。

```scala
class Rational(n: Int, d: Int){
    require(d != 0)
    val numer: Int = n
    val denom: Int = d
    override def toString = numer + "/" + denom
    def add(that: Rational): Rational = {
        new Rational(numer * that.denom + that.numer * denom, d * that.denom)
    }
}

scala> val oneHalf = new Rational(1, 2)
obeHalf: Rational = 1/2

scala> val twoThird = new Rational(2, 3)
twoThird: Rational = 2/3

scala> oneHalf add twoThird
res16: Rational = 7/6
```

### 6.5 自引用

关键字`this`指向当前执行方法的调用对象。当被用在构造方法的时候，指向被构造的实例。

```scala
def lessThan(that: Rational) = {
    this.numer * that.denom < this.denom * that.numer
}
```

this.numer指向执行lessThan调用的对象的分子，也可以省去this，两者时等效的。

另外是一个不能省去this的定义，加入要添加一个max方法，，返回有理数和参数之前较大的一个。

```scala
def max(that: Rational) = {
    if (this.lessThan(that)) that else this
}
```

第一个this可以省略，但第二个不可以省略，因为它代表了表达式为false时的返回值。

### 6.6 辅助构造方法

在Scala中，`主构造方法之外的构造方法为`**`辅助构造方法(auxiliary constructor)`**。例如，如果分母为0，则可以简单的写为5而不是5/1，也就是在分母为1的情况下，可以只接收一个参数，即分子。

`辅助构造方法以def this(...)开始`，辅助构造方法只是调用了一下主构造方法，透传它唯一的参数n作为分子，1作为分母。

```scala
def this(n: Int) = this(n, 1) //1为默认值
```

>**在Scala中，每个辅助构造方法都必须首先调用同一个类的另一个构造方法。换句话说，Scala辅助构造方法的第一条语句必须是这样的形式：`this(...)`**。被调用的构造方法要么是主构造方法，要么是另一个发起调用的构造方法之前的另一个辅助构造方法。这样规则的净效应是`Scala的每个构造方法最终都会用到该类的主构造方法`。**`主构造方法就是该类的唯一入口`**。

### 6.7 私有字段和方法

上述定义的类中，加入分子和分母可以化简，比如66/42可以正则化等效为11/7，就是分子和分母除以他们的`最大公约数`，我们可以定义一个私有方法。

```scala
private def gcd(a: Int, b: Int)：Int = {
    if(a == b) a else gcd(b, a % b)

//添加一个私有字段g
private val g = gcd(n.abs, d.abs)
}
```

在Rational类中，实例化时，我们只能使用 `x.add(y)`或者`x add y`，但不能使用`x + y`。这是因为我们没有定以`+`这个方法。将类中add方法改为名为+的方法，这样就可以使用`x.+(y)`或者`x+y`。

```scala
def + (that: Rational):Rational = {
    new Rational(numer * that.denom + denom * that.numer, denom * that.denom)
  }
```

### 6.8 Scala中的标识符

#### 6.8.1 字母和数字标识符

- 字母数字标识符：以字母或下划线开头，$也算作字母，但是它是系统预留给那些有Scala编译器生成的标识符。
- Scala命名遵循`驼峰命名法`，很少使用下划线。
- 字段、方法、方法参数、局部变量和函数以小写字母开头。
- 类和特质以大写字母开头。

**尽量不要在标识符末尾加上下划线**，如果你像这样来声明一个变量，`val name_: Int = 1`，会编译出错，编译器会认为你要声明的变量名称为`name_:`，要让代码编译通过，需要在下划线后面加上一个空格`val name_ : Int = 1`。

#### 6.8.2 操作标识符

指一个或多个操作符组成，Scala编译器会在内部将操作符用内嵌$的方式转成合法的Java标识符。比如`:->`这个操作符在内部会被表示为`$colon$minus$greater`。如果你打算在Java内部使用这些标识符，就需要使用这种内部形式。

#### 6.8.3 混合标识符

就是一个字母数字组合的操作符、一个下划线和一个符号操作符组成，如`unary_!`。

#### 6.8.4 字面标识符

用反引号括起来的任何字符串（\`...\`）。`可以将任何能被运行是接收的字符串放在反引号当中，作为标识符`，其结果永远是个合法的Scala标识符，甚至反引号中是Scala的保留字也生效。一个典型的例子是访问Java的Thread类的静态方法yield，不能直接写成`Thread.yield()`，因为yield是Scala的保留字，不过仍然可以在反引号中使用这个方法名，像这样`Thread`.\`yield\``()`。

### 6.9 方法重载

定义相同的方法名，不同的方法入参，最终Rational的类结构定义如下。

```scala
class Rational(n: Int, d: Int) {
  require(d != 0)
  private val g = gcd(n.abs, d.abs)
  val numer = n / g
  val denom = d / g

  def this(n: Int) = this(n, 1)

  def +(that: Rational): Rational =
    new Rational(
      numer * that.denom + that.numer * denom,
      denom * that.denom
    )

  def +(i: Int): Rational =
    new Rational(numer + i * denom, denom)

  def -(that: Rational): Rational =
    new Rational(
      numer * that.denom - that.numer * denom,
      denom * that.denom
    )

  def -(i: Int): Rational =
    new Rational(numer - i * denom, denom)

  def *(that: Rational): Rational =
    new Rational(numer * that.numer, denom * that.denom)

  def *(i: Int): Rational =
    new Rational(numer * i, denom)

  def /(that: Rational): Rational =
    new Rational(numer * that.denom, denom * that.numer)

  def /(i: Int): Rational =
    new Rational(numer, denom * i)

  override def toString = numer + "/" + denom

  private def gcd(a: Int, b: Int): Int =
    if (b == 0) a else gcd(b, a % b)
}
```

```scala
def gcdLong(x: Long, y: Long) = {
 val a = x
 val b = y
 while(a != 0){
  val temp = a
  a = b % a
  b = temp
 }
 b
}
```

```scala
def gcdLong(x: Long, y:Long) = {
 val a = x
 val b = y
 do {
  val temp = a
  a = b % a
  b = temp
 } while (a != 0)
}
```

### 6.10 隐式转换

通过定义Rational类，定义一个变量

```scala
val r = new Rational(2, 3)
```

你可以写`r * 2`，但你并不能写`2 * r`，因为2没有对应的方法。

```scala
scala> 2*rx
<console>:14: error: overloaded method value * with alternatives:
  (r: Double)Double <and>
  (r: Float)Float <and>
  (r: Long)Long <and>
  (r: Int)Int <and>
  (r: Char)Int <and>
  (r: Short)Int <and>
  (r: Byte)Int
 cannot be applied to (Rational)
       2*r
        ^
```

这里的问题是`2 * r`相当于`2.(r)`，但Int类并没有一个接收Rational参数的乘法方法，因为Scala类并不是Scala类库中的标准类。

解决办法：可以创建一个`隐式转换`，在需要时自动将整数转换为有理数，可以在解释器（不是Rational内部）中添加行。

```scala
implicit def intToRational(x: Int) = new Rational(x)
```

然后再执行`2 * r`就能正常计算了。为了让隐式转换正常工作，它需要在作用域内。如果你将隐式方法定义在Rational内部，对解释器而言并没有在作用域范围内，就目前而言，必须在解释器中直接定义这个转换。

## 7 内建的控制结构

Scala只有为数不多的几个内建的控制结构，包括if, while, for, try, match和函数调用。`Scala所有的控制结构都返回某种值作为结果`。

### 7.1 if表达式

和其他语言基本一致，首先测试某个条件，然后根据指令条件时候来满足执行两个不同代码分支当中的一个。

```scala
var filename = "default.txt"
if(!args.isEmpty) filename = args(0)
```

更精简的写法为不用`var`，而是使用`val`。

```scala
val filename = if(!args.isEmpty) args(0) else "default.txt"
```

使用`val`的好处是对`等式推理(equational reasoning)`的支持，引入的变量等于计算出它的值的表达式（假定这个表达式没有副作用）。`任何你打算写变量名的地方，都可以直接用表达式来替换`。

### 7.2 while循环

和其他语言没啥大的差别，包含一个条件检查和一个循环体。

```scala
def gcdLoop(x: Long, y: Long): Long = {
  var a = x
  var b = y
  while (a != 0){
    val temp = a
    a = b % a
    b = temp
  }
}
```

do-while循环：

```scala
var ling = ""
do {
  line = readLine()
  println("Read: " + line)
} while (line !="")
```

while和do-while这样的语法结构，称之为循环而不是表达式。因为他们的返回值类型为Unit。实际上存在一个类型为Unit的值，叫做`单元值（unit value）`，写作`()`。存在这样一个`()`值，是Scala的Unit与Java的void的不同。

```scala
scala> def greet() = {println("hello")}
greet: ()Unit

scala> () == greet()
<console>:13: warning: comparing values of types Unit and Unit using `==' will always yield true
       () == greet()
          ^
hello
res0: Boolean = true
```

`只要返回结果类型为Unit，那么永远和单元值相等`。

另一个相关的返回单元值的语法结构是对var的赋值。

```scala
val line = ""
while((line.readLine() != "") //条件永远成立
  println("Read: " + line)
```

**用`!=`对类型为Unit的值和String作比较将永远返回true**。在Java中，赋值语句的结果是被赋上的值，而`在Scala中赋值语句的结果永远是单元值()`。

由于while循环没有返回值，纯函数式编程语言通常都不支持，这些语言有表达式，而不是循环。尽管如此。Scala中还是有while循环，因为有时候指令是编程更加易读。但在Scala中，对while循环的使用要保持警惕，尽量使用其他结构来代替。

如开头定义的函数`gcdLoop`，可以有更精简不使用while循环的写法。

```scala
def gcd(x: Long, y: Long): Long = {
  if (y == 0) x else gcd(y, x % y)
}
```

### 7.3 for表达式

#### 7.3.1 遍历集合

```scala
val filesHere = (new java.io.File(".")).listFiles

for(file <- filesHere) //生成器语法，会遍历fileHere中每个文件
  println(file)
```

for表达式可以用于任何种类的集合，而不仅仅是数组。`Range(区间)`是一种特殊用法，可以用`“1 to 5”`创建一个Range，并用for来遍历它们。

```scala
for (i <- 1 to 4)
  println("Iteration: " + i)

Iteration: 1
Iteration: 2
Iteration: 3
Iteration: 4
```

如果想遍历不包含上界，可以使用`until`。

```scala
for (i <- 1 until 4)
  println("Iteration: " + i)

Iteration: 1
Iteration: 2
Iteration: 3
```

在其他语言中，可能通过下面例子遍历数组：

```scala
for (i <- 0 to filesHere.length - 1)
  println(filesHere(i))
```

但在Scala中比较少见，因为Scala可以直接遍历数组，这样可以让代码更加简短。

#### 7.3.2 过滤

在for表达式上添加过滤器，过滤器是for表达式的圆括号中的一个if子句。比如要查找文件后缀名为`.scala`的文件。

```scala
val filesHere = (new java.io.File(.)).listFiles
for(file <- filesHere if file.getName.endsWith(".scala"))
  println(file)

//也可以用下面语句
for(file <- filesHere)
  if (file.getName.endsWith(".scala"))
    println(file)
```

`可以包含更多的过滤器，只需要添加if子句就行了`。

```scala
for (file <- filesHere
     if file.isFile //只要文件
     if file.getName.endsWith(".scala")
) println(file)
```

#### 7.3.3 嵌套迭代

如果有多个`<-子句`，将得到嵌套循环。注意，在圆括号中Scala不会自动推断分号，可以使用花括号{}而不是圆括号()来包括生成器和过滤器。这样的好处是可以省去某些分号。

```scala
def fileLines(file: java.io.File) =
  scala.io.Source.fromFile(file).getLines().toList

def grep(pattern: String) =
  for ( file <- filesHere
        if file.getName.endsWith(".scala");
        line <- fileLines(file)
        if line.trim.matches(pattern)
      ) println(file + ": " + line.trim)
grep(".*gcd.*")
```

#### 7.3.4 中途(Mid-stream)绑定变量

上面例子中，line.trim重复了两遍，我们的要求是尽量只算一次。可以用`=`将表达式的结果绑定到一个新的变量上。这样后面直接引用即可。

```scala
def grep(pattern: String) = {
  for {
    file <- filesHere
    if file.getName.endsWith(".scala")
    file <- filesHere(file)
    trimmed = line.trim  //结果绑定到trimmed上
    if trimmed.matchs(pattern)
  } println(file + ": " + trimmed) //直接引用trimmed
}
grep(".*gcd.*")
```

#### 7.3.5 产出一个新的集合

目前为止，所有的迭代都是对遍历到的值进行操作，然后忘掉他们。也完全可以在每次迭代时生成一个可以记住的值。具体做法是在for表达式的代码体之前加上关键字`yield`。

```scala
def scalaFiles = {
  for{
    file <-filesHere
    if file.getName.endsWith(".scala")
  } yield file
  //**代码体**
}
```

for表达式每执行一次，都会交出一个值。执行完毕后，会交出所有的值，包含在一个集合当中，结果是`Array[file]`。

要小心`yield`的位置。for表达式语法为:

>**`for`** 子句 **`yield`** 代码体

### 7.4 用try表达式实现异常处理

#### 7.4.1 抛出异常

**throw `new` IlleaglArgumentException**

```scala
val half =
  if (n % 2 == 0)
    n / 2
  else
    throw new IlleaglArgumentException
```

从技术上讲，抛出异常的表达式类型为`Nothing`。

#### 7.4.2 捕获异常

- catch子句

```scala
import java.io.FileReader
import java.iop.FileNotFoundException
import java.io.IOException

try{
  val f = new FileReader("input.txt")
} catch{
  case ex: FileNotFoundException => //处理找不到文件的异常
  case ex: IOException => //处理I/O错误
}
```

- finally子句

```scala
import java.io.FileReader

val file = new FileReader("input.txt")
try{

} finally {
  file.close()
}
```

- 交出值

跟Scala其他控制结构一样，`try-catch-finally`结构最终返回一个值。无论怎样，`finally`最终都会执行，这样就会造成`finally`子句的返回值可能会改写任何之前的`try`代码块或某个`catch`子句中产生的值。

```scala
def f(): Int = try return 1 finally return 2
```

无论怎么样`f()`都会返回2，`finally`将`try`子句返回的1给复写掉,因为`return`会显式的返回结果。

```scala
def g(): Int = try 1 finally 2

<console>:11: warning: a pure expression does nothing in statement position
       def g():Int = try 1 finally 2

//一个纯表达式在代码段中什么都不会做。
```

通过上面例子，尽量不要在`finally`子句中返回值。

### 7.5 match表达式

Scala中的match表达式可以从若干选项中选择，就像其他语言的switch语句。

```scala
val firstArg = if(arg.length > 0) args(0) else ""

firstArg match{
  case "salt" => println("pepper")
  case "chips" => println("salsa")
  case "eggs" => println("bacon")
  case _ => println("huh?")
}
```

Scala的match与Java的switch相比，有些重要的区别。

- 任何常量、字符串都可以用作样例，而不仅限于Java的case语句支持的整型、枚举和字符串常量；

- 每个选项的最后没有`break`语句，`Scala中break语句是隐含的`，并不会出现某个可选项执行完继续执行下一个可选项的情况；

- match表达式与Java的switch最显著的不同是，**`match表达式会返回值`**。

将返回的值保存在变量friend中：

```scala
val firstArg = if(arg.length > 0) args(0) else ""

val friend = firstArg match{
  case "salt" => "pepper"
  case "chips" => "salsa"
  case "eggs" => "bacon"
  case _ => "huh?"
}

println(friend)
```

### 7.6 没有break和continue

Scala去掉了break和continue，因为它与`函数字面量`不搭。最简单的方式就是用if换掉每一个continue，用布尔值换点每一个break。

如果是检索参数列表，找到一个以.scala结尾但不以连字符(-)开头的字符串，Java可以这样写：

```java
int i = 0;
boolean foundIt = false;
while (i < args.length){
  if (args[i].startsWith("-")){
    i = i + 1;
    continue;
  }
  if (args[i].endsWith(".scala")){
    foundIt = true;
    break;
  }
  i = i + 1;
}
```

但是换做用Scala写，可以把先if再continue的写法改成用if将整个while循环体剩余的部分包括起来。为了去掉break，可以添加一个布尔值，表示是否需要继续循环。

```scala
var i = 0
val foundIt = false

while (i < args.length && !foundIt){
  if (arg(i).startsWith("-")){
    if (args(i).endsWith(".scala")){
      foundIt = true
    }
  }
  i = i + 1
}
```

如果不想要var变量，可以将循环重写为递归函数：

```scala
def serachFrom(i: Int): Int = {
  if (i >= args.length) -1
  else if (args(i).startsWith("-")) searchFrom(i + 1)
  else if (args(i).endsWith(".scala")) i
  else searchFrom(i + 1)
}

val i = searchFrom(0)
```

>Scala编译器实际上不会对上例中的代码生成递归函数，由于所有的递归调用发生在`函数尾部（tail-call position， 尾递归）`，编译器会生成和while循环类似的代码。

### 7.7 变量作用域

和Java基本类似，一点不同是，变量一但定义好，就不能在相同的作用域定义相同名字的新变量。

```scala
def printMultiTable() = {
  var i = 1
  //只有i在作用域内
  while (i <= 10){
    var j = 1
    //i和j在作用域内
    while (j <= 10){
      val prod = (i * j).toString
      //i，j和prod在作用域内
      var k = prod.length
      ////i，j，k和prod在作用域内
      while (k < 4){
        print(" ")
        k += 1
      }
      print(prod)
      j += 1
    }
    //i和j仍在作用域内，k和prod超出了作用域
    println()
    i += 1
  }
  //i仍在作用域内
}

//执行该函数
scala> printMultiTable()
   1   2   3   4   5   6   7   8   9  10
   2   4   6   8  10  12  14  16  18  20
   3   6   9  12  15  18  21  24  27  30
   4   8  12  16  20  24  28  32  36  40
   5  10  15  20  25  30  35  40  45  50
   6  12  18  24  30  36  42  48  54  60
   7  14  21  28  35  42  49  56  63  70
   8  16  24  32  40  48  56  64  72  80
   9  18  27  36  45  54  63  72  81  90
  10  20  30  40  50  60  70  80  90 100

```

### 7.8 对指令式代码进行重构

对指令式风格的代码进行重构

```scala
//以序列形式返回一行
def makeRowSeq(row: Int) = {
  for (col <- 1 to 10) yield {
    val prod = (row * col).toString
    val padding = " " * (4 - prod.length)
    padding + prod
  }
}

//以字符串形式返回一行
def makeRow(row: Int) = makeRowSeq(row).mkString

//以每行占用一个文本行的字符串形式返回表格
def multiTable() = {
  val tableSeq = for(row <- 1 to 10) yield makeRow(row)
  tableSeq.mkString("\n")
}
```

执行结果和7.7章节的例子相同。

## 8 函数和闭包

### 8.1 方法

定义函数最常用的用途是作为一个对象的成员，这时函数被称为`方法(method)`。

```scala
import scala.io.Source

object LongLines {
  def processFile(filename: String, width: Int) = {
    val source = Source.fromFile(filename)
    for(line <- source.getLines()) //getLines()返回一个迭代器
      processLine(filename, width, line)
  }
  private def processLine(filename: String, width: Int, line: String) = {
    if(ling.length > width)
      println(filename + ": " + line.trim)
  }
}
```

上例中定义了两个方法`processFile`和`processLine`。

### 8.2 局部函数

函数式编程风格是尽量将程序拆成很多小的函数实现。单个函数通常很小。这种风格的优点是它为程序员构建复杂程序提供了灵活性。这种方法的一个问题是，所有的辅助函数名都可能污染程序的命名空间。

在解释器中，这不是什么大问题，但一旦函数被打包成可重用的类和对象，最好能隐藏部分辅助函数，这时我们可以将辅助函数定义为`局部函数`。这些函数只在部分作用域内有效。

在Java中，主要方式是定义私有方法，在Scala中，可以将局部函数定义在其他函数的内部。

```scala
def processFile(filename: String, width: Int) = {
  def processLine(filename: String, width:Int, line: String) = {
    if(line.length > width){
      println(filename + ": " + line.trim)
    }
  }
  val source = Source.fromFile(filename)
  for (line <-source.getLines()){
    processLine(filename, width, line)
  }
}
```

### 8.3 一等函数

一等函数有如下特征：

- 在运行时创建
- 能赋值给变量或数据结构中的元素
- 能作为参数传给函数
- 能作为函数的返回结果

`函数字面量(function literal)`被编译成一个类，这个类在运行时实例化成一个函数值。函数字面量与函数值的区别在于函数字面量存在于源码中，函数值在运行时作为对象存在。这个区别相当于类(source code)和对象(runtime)的区别。

下面定义一个展示一个变量加1的函数字面量

```scala
(x: Int) => x + 1
```

 **`=>`** 代表将左边的内容的转换为右边的内容的符号，这个函数将任何整型变量由`x`转换为`x+1`。

由于函数值是对象，所以你可以将他们存储在一个变量中，它们也是函数，因此可以使用常用的括号函数调用符号来调用它们。

```scala
scala> var increase = (x: Int) => x + 1
increase: Int => Int = $$Lambda$1043/700837405@15cea7b0

scala> increase(10)
res0: Int = 11
```

因为increase是一个`var`，所以你可以给他赋予不同的函数值。

```scala
scala> increase = (x: Int) => x + 9999
increase: Int => Int = $$Lambda$1078/1843853990@3cad68df

scala> increase(1)
res1: Int = 10000
```

如果希望函数字面量中，有多个语句，可以使用`{}`将他们包括起来。

```scala
scala> increase = (x: Int) =>{
     | println("We")
     | println("are")
     | println("here")
     | x + 1
     | }
increase: Int => Int = $$Lambda$1081/1490735178@73e776b7

scala> increase(1)
We
are
here
res2: Int = 2
```

在函数字面量中，你可以用`foreach`迭代一个集合。

```scala
scala> val someNumbers = List(1,2,3,4,5,6)
someNumbers: List[Int] = List(1, 2, 3, 4, 5, 6)

scala> someNumbers.foreach((x: Int) => println(x))
1
2
3
4
5
6
```

还可以使用`过滤(filter)`方法：

```scala
scala> someNumbers.filter(x => x > 3)
res12: List[Int] = List(4, 5, 6)
```

### 8.4 函数字面量简短形式

一种方式是省去函数的参数类型，由于someNumbers是一个整型列表，所以编译器知道x肯定是整型。

```scala
 someNumbers.filter((x) => x > 0)
```

第二种精简方法是去掉参数周围的圆括号，编译器能推断出参数的类型，圆括号可以省略。

```scala
 someNumbers.filter(x => x > 0)
```

### 8.5 占位符

为了使函数字面量更加精简，可以使用下划线作为占位符来代表一个或多个参数，`只要每个参数在函数字面量中只出现一次`。比如，`_ > 0`表示检查一个值是不是大于0。

```scala
someNumbers.filter(_ > 0)
```

可以将`下划线（占位符）`视为`需要填写`的`空白(blank)表达式`。这个空白将在每次调用函数时用函数的参数填充。

有时使用下划线作为参数的占位符，编译器可能没有足够的信息去推断参数的类型，如当你写`_ + _`时：

```scala
scala> val f = _ + _
<console>:11: error: missing parameter type for expanded function ((x$1: <error>, x$2) => x$1.$plus(x$2))
       val f = _ + _
               ^
<console>:11: error: missing parameter type for expanded function ((x$1: <error>, x$2: <error>) => x$1.$plus(x$2))
       val f = _ + _
                   ^
```

这种情况下，可以使用冒号来指定参数的类型：

```scala
scala> val f = (_: Int) + (_: Int)
f: (Int, Int) => Int = $$Lambda$1042/1378592036@29c5ee1d

scala> f(5, 10)
res0: Int = 15
```

`_ + _`为定义带两个参数的函数字面量，每个参数在函数字面量中只能出现一次，多个下划线代表多个参数，而不能多次重复引用一个参数，也就是传入的参数和占位符要按照位置一一对应。

### 8.6 部分应用函数

虽然前面的示例用下划线替代了单个参数，但也可以用下划线替换整个参数列表，比如：

```scala
scala> val someNumbers = List(1,2,3,-8,-10,8,-34)
someNumbers: List[Int] = List(1, 2, 3, -8, -10, 8, -34)

scala> someNumbers.foreach(println _)
1
2
3
-8
-10
8
-34
```

`这时下划线不是代表单个参数，而是代表整个参数列表`。注意函数名和下划线之间要有空格隔开，不然编译器会认为引用的是另一个符号。上述写法等同于:

```scala
someNumbers.foreach(x => println(x))
```

以这种方式使用下划线时，是在编写一个部分应用函数。在Scala中，当调用一个函数，传入任何需要的参数时，将该函数应用于传入的参数上。

```scala
scala> def sumAbc(a: Int, b: Int, c: Int) = a + b + c
sumAbc: (a: Int, b: Int, c: Int)Int
```

部分应用的函数是这样一种表达式：不必提供函数所需的所有参数。例如，要创建一个包含sumAbc的部分应用的函数表达式，在该表达式中不提供所需的三个函数中的任何一个，只是在函数名之后加上一个下划线：

```scala
scala> val a = sumAbc _
a: (Int, Int, Int) => Int = $$Lambda$1267/146644232@189f3ccd
```

当对这个新函数值应用三个参数时，

```scala
scala> a(1, 2, 3)
res12: Int = 6
```

变量a引用了一个由`sumAbc实例化的函数值对象`。编译器生成的类有一个接受三个参数的apply方法，因为sumAbc中有3个缺失的参数。Scala编译器将表达式a(1,2,3)转换为函数值的apply方法的调用，并传入三个参数1、2和3。`a(1, 2, 3)是a.apply(1, 2, 3)的简短形式`。

这个apply方法是由编译器根据sumAbc的定义自动生成，然后将3个参数传入sumAbc的参数列表中。这个例子中apply调用了sumAbc(1, 2, 3)，然后返回该函数的返回值，也就是6。

另一种考虑这种表达式的方法是，用下划线来表示整个参数列表，这是一种将def转换为函数值的方法。例如，如果有一个局部函数，如sum(a: Int, b: Int, c: Int): Int，可以将它“包装”在一个函数值中，该函数值的apply方法具有相同的参数列表和结果类型。当你把这个函数值应用到一些参数上时，会将sumAbc应用到参数列表中，然后返回函数的返回值。

即使不能将方法或嵌套函数赋给变量，或将其作为参数传递给另一个函数，但如果将方法或嵌套函数包装在函数值中，并在其名称后面加上下划线，则可以实现这些操作。

尽管sumAbc确实是一个部分应用函数，但可能不太清楚为什么这么叫。它有这个名字是因为你没有把那个函数应用到它的所有参数上。如果是`sumAbc _`，就没有应用到任何参数上，但是也可以只传入部分参数来表示部分应用函数。例子如下：

```scala
scala> val b = sumAbc(1, _: Int, 3)
b: Int => Int = $$Lambda$1268/1888428943@509d999c
```

在这个例子中，提供了第一个和第三个参数，只有第二个参数为空。Scala编译器会生成一个函数类，apply方法之应用到缺失的参数上。当使用那1个参数调用时(`b(5)`)，这个生成的函数的apply方法调用sumAbc，传入第一个参数`1`、传入指定的参数`(5)`和第三个参数`3`。

```scala
scala> b(5) //自动将5映射到第二个参数上
res13: Int = 9
```

如果你写的部分应用函数，省去了全部的参数，比如写作`println _`或者`sum _`，此时的表达式可以更加的精简，可以省去下划线`_`。

```scala
someNumbers.foreach(println)
```

这种形式只允许在需要函数的地方使用，例如本例中的foreach调用。编译器知道在这种情况下需要一个函数，因为foreach要求函数作为参数传递。在不需要函数的情况下，尝试使用此表单将导致编译错误。

```scala
scala> val c = sumAbc
<console>:12: error: missing argument list for method sumAbc
Unapplied methods are only converted to functions when a function type is expected.
You can make this conversion explicit by writing `sumAbc _` or `sumAbc(_,_,_)` instead of `sumAbc`.
       val c = sumAbc
               ^

scala> val c = sumAbc _
c: (Int, Int, Int) => Int = $$Lambda$1272/1333118503@1631a614
```

### 8.7 闭包

目前为止，所有的函数字面量示例都只引用了传递的参数。比如`(x: Int) => x + 1`，在函数体中，只有一个传递参数x，是函数中定义的一个参数。但是，你还可以引用其他地方定义的变量，如：

```scala
(x: Int) => x+ more
```

目前为止，我们不知道`more`代表的是意思，`more是一个自由变量(free variable)`，因为函数字面量本身并没有给出它任何定义。对比来说，`x是一个绑定变量(bound variable)`，因为在函数上下文中知道它代表的意思，它是函数的唯一参数。

如果没有more的定义而去直接调用它，编译器会抛出错误：

```scala
scala> (x: Int) => x + more
<console>:12: error: not found: value more
       (x: Int) => x + more
                       ^
```

先定义more，然后再去调用，就不会出现问题：

```scala
scala> val more = 1
more: Int = 1

scala> val addMore = (x: Int) => x + more
addMore: Int => Int = $$Lambda$1071/1771190979@1f1ff879

scala> addMore(1)
res2: Int = 2
```

在运行时，从这个函数字面量创建的函数值(对象)称为`闭包`。这个名字来自于通过`“捕获”`自由变量的绑定来`“关闭”`函数字面量的行为。

一个没有自由变量的函数字面量称为`闭项(close term)`，比如，`(x: Int) => x +1`。因此，从严格意义上讲，在运行时从这个函数字面量上创建的函数值不是闭包，因为它本身就是闭合的。

任何有自由变量的函数字面量称为`开项(open term)`。根据定义，在运行时从`(x: Int) => x + more`创建的任何函数值都需要捕获其自由变量`more`的绑定。得到的函数值，包含了对自由变量`more`的捕获，称之为闭包是由于函数值是关闭`开项`行为的最终结果。

**如果自由变量`more`发生变化，闭包的结果也会随之发生变化**。

```scala
scala> var more = 1
more: Int = 1

scala>  val addMore = (x: Int) => x + more
addMore: Int => Int = $$Lambda$1074/1001100746@71ad959b

scala> addMore(1)
res4: Int = 2

scala> more = 10000
more: Int = 10000

scala> addMore(1)
res5: Int = 10001

```

直观地说，`Scala的闭包捕获的是变量本身，而不是变量所引用的值`。闭包对自由变量的更改在闭包外是可见的，比如：

```scala
scala> val someNumbers = List(-11, -10, -5, 0, 5, 10)
someNumbers: List[Int] = List(-11, -10, -5, 0, 5, 10)

scala> var sum = 0
sum: Int = 0

scala> someNumbers.foreach(sum += _)

scala> sum
res8: Int = -11
```

每次调用这个函数时，它都会创建一个新的闭包。每个闭包将访问在创建闭包时处于活动状态的`more`变量。

```scala
scala> def makeIncreaser(more: Int) = (x: Int) => x + more
makeIncreaser: (more: Int)Int => Int

scala> val inc1 = makeIncreaser(1)
inc1: Int => Int = $$Lambda$1223/705170339@5ea4abef

scala> val incr9999 = makeIncreaser(9999)
incr9999: Int => Int = $$Lambda$1223/705170339@4893b980
```

当调用`makeIncreaser(1)`时，将创建并返回一个闭包，该闭包捕获值1作为绑定以获取`more`内容。同理，当调用`makeIncreaser(9999)`时，将创建并返回一个闭包，该闭包捕获值9999作为绑定以获取`more`内容。

当将这些闭包应用于参数(在本例中，只有一个参数x，必须传入)时，返回的结果取决于创建闭包时如何定义更多的参数:

```scala
scala> inc1(1)
res11: Int = 2

scala> incr9999(1)
res12: Int = 10000
```

### 8.8 特殊的函数调用

函数调用是Scala编程的核心。下面介绍一些特殊的函数调用。

#### 8.8.1 重复参数

参数的数量不固定：

```scala
scala> def echo(args: String*) = for(arg <- args) println(arg)
echo: (args: String*)Unit

scala> echo("one")
one

scala> echo("two", "three")
two
three
```

在函数内部，重复参数的的类型为参数声����类型的一个数组`Array[String]`。如果你有一个某种类型的数组，你试图把它作为一个重复参数传递，编译器会抛出一个错误。

```scala
scala> val arr = Array("One", "Two", "Three")
arr: Array[String] = Array(One, Two, Three)

scala> echo(arr)
<console>:14: error: type mismatch;
 found   : Array[String]
 required: String
       echo(arr)
            ^
```

为了能正常调用，需要按照下面的写法：

```scala
scala> echo(arr: _*)
One
Two
Three
```

这种表示法告诉编译器将arr的每个元素作为它单个参数传递给echo，而不是将所有元素作为单个参数传递。

#### 8.8.2 具名参数

传入具名参数，可以在给出参数名的前提下，位置不受限制。

```scala
scala> def speed(distance: Float, time: Float): Float = {
     | if (time != 0) distance / time
     | else 0
     | }
speed: (distance: Float, time: Float)Float

scala> speed(100, 10)
res2: Float = 10.0

scala> speed(time = 10, distance = 100)
res3: Float = 10.0
```

使用指定的参数调用，可以在不改变含义的情况下反转参数。

#### 8.8.3 默认参数

Scala允许为函数参数指定默认值。在函数调用中可以选择省略此类参数的参数，在这种情况下，相应的参数将使用默认值填充。

```scala
scala> def printTime(out: java.io.PrintStream = Console.out) =
     | out.println("time = " + System.currentTimeMillis())
printTime: (out: java.io.PrintStream)Unit

scala> printTime() //不传入参数，会使用默认参数
time = 1588068428131

scala> printTime(Console.err) //使用传入的参数
time = 1588068556370
```

设置两个默认参数：

```scala
scala> def printTime(out: java.io.PrintStream = Console.out, devisor: Int = 1) =
     | out.println("time = " + System.currentTimeMillis()/devisor)
printTime: (out: java.io.PrintStream, devisor: Int)Unit

scala> printTime()
time = 1588132268000

scala> printTime(devisor = 100)
time = 15881322770

scala> printTime(Console.err)
time = 1588132302797
```

#### 8.9 尾递归

在第7章提到过，要将vars的while循环转换为只使用vals的更实用的样式，有时可能需要使用递归。下面实现一个尾递归的方法。

```scala
def approximate(guass: Double): Double = {
  if (isGoodEnough(guass)) guass
  else approximate(improve(guass))
}
```

该函数通常用于搜索问题，为了加快函数运行速度，你可能会写一个while循环来实现。

```scala
def approximateLoop(initialGuess: Double): Double = {
  var guess = initialGuess
  while (!isGoodEnough(guess))
    guess = improve(guess)
  guess
}
```

尾递归和while循环之间如何选择，从简洁性和安全性方面来考虑肯定函数式编程更好，但指令式编程是不是更加高效呢？`实际上两者的运行速度几乎相同`。这看起来可能很奇怪，因为递归调用每次都会跳回到开头，重新开始计算。

但实际上，编译器会对第一种函数式的`approximate`定义应用一项重要优化。像`approximate`这种在函数最后一步`直接调用自身`，称为`尾递归(tail recursive)`。Scala编译器检测到尾递归，并在用新的值更新函数参数后，用返回函数开头的跳转替换它，类似于循环的实现方式。

- 跟踪尾递归

尾部递归函数不会为每个调用构建新的堆栈帧，所有的调用都会在一个栈帧中执行。这可能会让程序员感到惊讶，因为他们正在检查程序失败的堆栈跟踪。

```scala
def boom(x: Int): Int = {
  if (x == 0) throw new Exception("boom!")
  else boom(x - 1) +1
  }
boom: (x: Int)Int

scala> boom(5)
java.lang.Exception: boom!
  at .boom(<console>:12)
  at .boom(<console>:12)
  at .boom(<console>:12)
  at .boom(<console>:12)
  at .boom(<console>:12)
  at .boom(<console>:12)
  ... 28 elided
```

这个函数本质上不是一个尾递归，因为在最后调用时对`boom(x - 1)`做了`+1`操作，不是`直接调用`。

```scala
scala> def bang(x: Int): Int = {
     | if (x == 0) throw new Exception("bang!")
     | else bang(x - 1)
     | }
bang: (x: Int)Int

scala> bang(5)
java.lang.Exception: bang!
  at .bang(<console>:12)
  ... 28 elided
```

这一次，只看到`bang`的一个堆栈帧。可能认为`bang`在调用自己之前就崩溃了，但事实并非如此。如果认为在查看堆栈跟踪时可能会被尾部调用优化弄糊涂，那么可以通过向`scala shell`或`scalac`编译器提供以下参数来关闭它们。

```bash
-g:notailcalls
```

- 尾递归的限制

Scala中尾递归的使用相当有限，因为JVM指令集使得实现更高级形式的尾递归非常困难。**Scala只优化直接递归回调用相同函数的调用**。如果递归是间接的，就像下面两个相互递归的函数一样，那么就不可能进行优化。

## 9 控制抽象

### 9.1 减少代码重复

所有的含糊都能被分解成每次函数调用都`一样的公共部分`和每次调用`不一样的非公共部分`。公共部分是函数体，非公共部分必须是通过实参传入。档把函数值作为入参的时候，这段算法的非公共部分又是另一个算法。

每当这样的函数被调用，都可以传入不同的函数值作为实参，被调用的函数会调用传入的函数值。这些`高阶函数`，即那些接收函数作为参数的函数，让你有额外的机会来进一步压缩和简化代码。`高阶函数的好处之一是可以用来创建减少代码重复的控制抽象`。

定义一个查找所有扩展名为`“.scala”`的文件。

```scala
object FileMatcher{
  private def filesHere = (new java.io.File(".")).listFiles
  def filesEnding(query: String){
    for (file <- filesHere; if file.getName.endsWith(query))
      yield file
  }
}
```

如果我们要再定义一个查找文件名中包含`“pub”`的文件，需要再定义一个方法：

```scala
def filesContaining(query: String) = {
  for (file <- filesHere; if file.getName.contains(query))
    yield file
}
```

这个函数与`filesEnding`运行机制并没有什么区别，如果我们又需要定义一个使用正则表达式匹配的方法：

```scala
def filesRegex(query: String) = {
  for (file <- filesHere; if file.getName.matchs(query))
   yield file
}
```

以上三个函数的运行机制非常类似，我们可以通过使用函数值来提供一种方法，`通过传递某个帮你调用方法的函数值来达到目的`。在本例中，可以给方法添加一个mathcer参数，该参数的唯一目的是检查文件名是否满足某个条件。

```scala
def filesMatching(query: String,
  matcher: (String, String) => Boolean = {
    for (file <- filesHere; if matcher(file.getName, query))
      yield file
}
```

有了`fileMatching`，就可以定义不同的搜索方法：

```scala
def fileEnding(query: String) = {
  filesMatching(query, _.endWith(_))
}

def fileContaining(query: String) = {
  filesMatching(query, _.contains(_))
}

def fileRegex(query: String) = {
  filesMatching(query, _.matchs(_))
}
```

代码中的前后两个占位符，与`filesMatching`中`matcher(file.getName, query)`顺序一致，`_.endsWith(_)`等同于

```scala
(fileName: String, query: String) => (fileName.endsWith(query))
```

这样的写法是因为，这两个参数在函数体内分别只用到一次，`fileName`先被用到，然后是第二个参数`query`。

我们注意到，这里的查询字符串被传入`filesMatching`中后，没有做任何处理，只是传入了`matcher`函数，这样的来回传递是不必要的，完全可以将`query`从`filesMathcing`和`matcher`中移除。

```scala
object FileMatcher{
  private def filesHere = (new java.io.File(".")).listFiles

  private def filesMatching(matcher: String => Boolean) = {
    for (file <- filesHere; if matcher(file.getName))
      yield file
  }

  def filesEnding(query: String) =
    filesMatching(_.endsWith(query))
  
  def filesContaining(query: String) =
    filesMatching(_.contains(query))

  def filesRegex(query: String) =
    filesMatching(_.mathcs(query))
}
```

对于`filesMatching`来说，`query`是从方法的外部定义的，是自由变量而非绑定变量，只需要在函数的外部捕获即可，不需要写入到函数内部。正因为Scala支持闭包，才能将`query`从`filesMatching`中移除掉，从而能进一步简化代码。

### 9.2 简化调用方代码

高阶函数的另一个重要用途是将高阶函数本身放在API当中来让调用方的代码更加精简。

例如，`exists`，这个方法用于判定某个集合是否包含传入的值。我们可以定义一个函数来判定转出的List是否包含负数：

```scala
def containsNeg(nums: List[Int]): Boolean = {
  var exists = false
  for (num <- nums)
    if (num < 0)
      exists = true
  exists
}
```

调用时：

```scala
scala> containsNeg(List(1, 2, -3, 4))
res0: Boolean = true
```

不过另一种经精简的定义方式是直接传入高阶函数`exists`:

```scala
scala> def containsNeg(nums: List[Int]) = nums.exists(_ < 0)
containsNeg: (nums: List[Int])Boolean

scala> containsNeg(List(1, 2, -3, 4))
res1: Boolean = true
```

如果又要编写一个List是否包含偶奇数：

```scala
scala> def containsOdd(nums: List[Int]) = nums.exists(_ % 2 == 1)
containsOdd: (nums: List[Int])Boolean

scala> containsNeg(List(1, 2, -3, 4))
res2: Boolean = true
```

这个`exists`代表了一种控制抽象，是Scala类库提供的一种特殊用途的循环结构，并不像while或for那样是语言内建的。，不过由于`exists`是Scala集合API中的公共函数，它减少的是调用方的代码重复。

### 9.3 柯里化

Scala允许创建新的控制抽象，感觉就像语言原生支持一样。尽管目前看到的都是控制抽象，但没人会认为他们是语言原生支持的，为了搞清楚如何做出那些用起来更像是语言扩展的控制抽象，首先要理解一个编程技巧，就是`柯里化(currying)`。

`经过柯里化的函数在应用时支持多个参数列表，而不是一个`。

一个没有经过柯里化的函数：

```scala
scala> def plainOldSum(x: Int, y: Int) = x + y
plainOldSum: (x: Int, y: Int)Int

scala> plainOldSum(1, 2)
res6: Int = 3}
```

柯里化之后：

```scala
scala> def curried(x: Int)(y: Int) = x + y
curried: (x: Int)(y: Int)Int

scala> curried(1)(2)
res7: Int = 3
```

第一次接收`x`，第二次接收`y`。当你调用`curriedSum`时，实际上失恋者做了两次传统的函数调用。

```scala
def curriedSum(x: Int) = (y: Int) => x + y
```

在一个部分应用函数使用curriedSum时，占位符标识第二个参数，其结果指向一个函数引用：

```scala
scala> var b = curried(1)_
b: Int => Int = $$Lambda$1328/746855108@26e588b7

scala> b(2)
res12: Int = 3
```

### 9.4 编写新的控制结构

在拥有一等函数的语言中，可以有效地制作出新的控制接口，尽管语言的语法是固定的，需要做的就是创建接受函数作为参数的方法。

重复操作两次，并返回结果：

```scala
scala> def twice(op: Double => Double, x: Double) = op(op(x))
twice: (op: Double => Double, x: Double)Double

scala> twice(_ + 1, 5)
res13: Double = 7.0
```

每当发现新的控制模式在代码中多次出现时，就应该考虑将这个模式实现为新的控制结构。一种更加常见的编码方式：打开某个资源，对它进行操作，然后关闭该资源，可以用如下方法：

```scala
def withPrintWriter(file: File, op: PrintWriter => Unit) = {
  val writer = new PrintWriter(file)
  try{
    op(writer)
  }finally{
    writer.close()
  }
}
```

有了方法之后，就可以这样使用它：

```scala
withPrintWriter(
  new File("date.txt"),
  writer => writer.println(new java.util.Date)
)
```

使用这个方法的好处是，确保文件在最后被关闭的是`withPrintWriter`，而不是用户代码。这样做的好处是不会出现使用者忘记关闭文件的情况。这个技巧被称为`贷出模式(loan pattern)`，因为是某个控制抽象函数，比如`withPrintWriter`，打开一个资源并将该资源`“贷出”`给函数，当函数完成时，它会表明自己不再不要这个`“贷入”`的资源，这时这个资源就会在`finally`中关闭，这样确保不论函数是否返回异常，资源都能被正确的关闭。

可以`使用花括号而不是圆括号来表示参数列表`，这种调用方式看上去更像是`内建的控制结构`。在Scala中，只要那种**只传入一个参数**的方法调用，都可以**使用花括号来讲入参包括起来，而不是圆括号**。

```scala
scala> println("Hello, World")
Hello, World

scala> println{"Hello, World"} //只传入了一个参数
Hello, World

scala> val g = "Hello, world!"
g: String = Hello, world!

scala> g.substring {7, 9} //传入了两个参数，就不能使用{}代替()
<console>:1: error: ';' expected but ',' found.
       g.substring {7, 9}
                     ^

scala> g.substring (7, 9)
res16: String = wo
```

Scala允许用花括号替代圆括号来传入单个参数的目的是为了让调用方在花括号中编写函数字面量。在最新版的`writePrintWriter`中，接收了两个参数，所以不能使用`{}`替代`()`，但是可以使用柯里化将两个参数分开：

```scala
def withPrintWriter(file: File)(op: PrintWriter => Unit) = {
  val writer = new PrintWriter(file)
  try{
    op(writer)
  }finally{
    writer.close()
  }
}
```

这样就可以像下面一样使用，第二个参数列表只有一个参数，可以使用`{}`替代`()`:

```scala
val file = new File("date.txt")

withPrintWriter(file){writer => writer.println(new java.util.Date)}
```

### 9.5 传名参数

withPrintWriter的第二个参数列表，需要传入一个类型为`1PrintWriter`的入参，也就是`“writer =>”`，但是如果没有只需要传入花括号中间的代码，应该怎么办？Scala提供了`传名参数`。

下面我们定义一个不使用传名函数的断言：

```scala
var assertionsEnabled = true

def myAssert(predicate: () => Boolean) = {
  if (assertionsEnabled && !predicate())
    throw new AssertionError
}
```

使用时感觉很别扭:

```scala
myAssert(() => 5 > 3)
```

大概是不想在函数字面量里`写空的圆括号和=>符号`，而是直接写：

```scala
scala> myAssert(5 > 3)
<console>:13: error: type mismatch;
 found   : Boolean(true)
 required: () => Boolean
       myAssert(5 > 3)
                  ^
```

传名参数由此而生，要让参数称为传名参数，需要给参数`一个以=>开头的类型声明`，而不是`() =>`，将`predicate`参数转化为传名参数，把类型`“() => Boolean”`改成`“=> Boolean”`:

```scala
def byNameAssert(predicate: => Boolean) = {
  if (assertionsEnabled && !predicate)
    throw new AssertionError
}

byNameAssert: (predicate: => Boolean)Unit

scala> byNameAssert(5 > 3) //不会有问题
```

**`对于传名类型而言，空的参数列表，即()，是去掉的，这样的类型只能用于参数声明，并不存在传名变量或传名字段`**。

你可能会很好奇，为什么不能像如下定义，直接省去`=>`：

```scala
def boolAssert(predicate: Boolean) = {
  if (assertionsEnabled && !predicate)
    throw new AssertionError
}

boolAssert: (predicate: Boolean)Unit
```

这样也是可以的，使用起来看上去也没啥区别：

```scala
boolAssert(5 > 3) //也不会有问题
```

但是，两种当时有个显著的区别。由于`boolAssert`的参数类型为`Boolean`，在`boolAssert(5 > 3)`圆括号中的表达式将`“先于”`对`boolAssert`的调用被求值，而`byNameAssert`中的类型为`=> Boolean`，`byNameAssert(5 > 3)`圆括号中的表达式在调用`byNameAssert`之前是不会被求值，而是会有一个函数值被创建出来，这个函数值的`apply方法`将会对`5 > 3`求值，传入`byNameAssert`的是这个函数值。

因此，两种方式的区别在于，如果断言被禁用，你将能观察到`boolAssert`的圆括号当中表达式的副作用，而`byNameAssert`则不会，如果断言被禁用，那么我们的断言是`“x / 0 == 0”`的话，`boolAssert`就会抛出异常：

```scala
scala> boolAssert(x / 0 == 0)
java.lang.ArithmeticException: / by zero
  ... 28 elided

scala> byNameAssert(x / 0 == 0)
```

## 10 组合与继承

组合：一个类可以包含对另一个类的引用，利用这个被引用的来帮助完成任务；继承：超类/子类的关系。

### 10.1 一个二维的布局类库

创建一个用于构建和渲染二维布局元素的类库，以此作为本章的示例。每个元素标识一个文本填充的长方形。类库将提供名为`“elem”`的工厂方法，从传入的数据构造新的元素。例如用下面这个签名的工厂方法创建一个包含字符串的布局元素：

```scala
elem(s: String): Element
```

用一个名为`Element`的类型对元素建模。可以对一个元素调用above或beside，传入另一个元素，来获取一个将两个元素结合在一起的新元素。下面将创建一个有两列组成的更大的元素。每列高度都为2：

```scala
val column1 = elem("hello") above ("***")
val column2 = elem("***") above("world")
column1 beside column2

hello ***
*** world
```

布局元素很好地展示了这样一个系统，在这个系统中，对象可以通过组合操作符的帮助由简单的部件构建出来。本章将定义那些可以从数组、线和矩形构造出元素对象的类。

### 10.2 抽象类

首先定义一个`Element`类型，用来表示元素。由于元素是一个有字符组成的二维矩形，用一个成员`contents`来表示某个布局元素的内容是合情合理的。内容可以用字符串的数组表示，每个字符串代表一行，因此`contents`返回的结果类型将会是`Array[String]`。

```scala
abstract class Element{
  def contents: Array[String]
}
```

`contents`被声明为没有实现的方法，这是`Element`的抽象成员。`一个包含抽象成员的类本身也要是抽象的`。`abstract`修饰符表名该类可以拥有那些没有实现的抽象成员。因此，`Element不能直接实例化为一个抽象类`。

```scala
scala> new Element
<console>:13: error: class Element is abstract; cannot be instantiated
       new Element
       ^
```

**注意**，`Element`类中的`contents`定义前面没有加上`abstract`修饰符。`一个没有实现方法（即没有等号或方法体），那么他就是抽象的`。与Java不同，我们不需要加上`abstract`修饰符，那些给出了实现的方法被称为`具体(concrete)方法`。

### 10.3 定义无参方法

```scala
abstract class Element{
  def contents: Array[String]
  def height: Int = contents.length
  def width: Int = if (height == 0) 0 else contents(0).length
}
```

以上3个方法都没有参数列表，这样的`无参方法(parameterless method)`在Scala中很常见。与此对应，那些定义为空的圆括号定义的方法，比如`def height(): Int`被称为`空圆括号方法(empty-paren method)`。

**推荐的做法事对于没有参数且只通过读取所在对象字段的方式访问可变状态（确切地说不改变状态）的情况下尽量使用无参方法**。这样做法被称为`统一访问原则`：使用方代码不应受到某个属性是用字段还是方法实现的影响。

完全可以把上述height和width实现为字段，就把def换成val即可。

```scala
abstract class Element{
  def contents: Array[String]
  val height: Int = contents.length
  val width: Int = if (height == 0) 0 else contents(0).length
}
```

从使用方来说，两者完全等价。字段可能比方法稍微更快，因为字段在类初始化时就已经计算好了，而不是每次方法调用时都要重新计算。`核心点在于Element类的使用方不应该被内部实现的变化所影响`。

Scala对于混用无参方法和空括号方法处理比较灵活，两个可以相互重写。从原理上讲，可以对所有无参函数的调用去掉空括号。但是，Scala鼓励我们将那些`不接收参数也没有副作用的方法定义为无参方法`，如果`有副作用的方法，不应该省略空括号`。如下：

```scala
"hello".length //没有()，因为没有副作用
println() // 最好别省去()
```

换句话讲，如果你调用的这个函数执行了某个操作就加上空括号，如果仅仅访问该函数的某个属性，则可以省去空括号。

### 10.4 扩展类

前面定义的`Element`，`new Element`是不能使用的，因为`Element`是抽象类。因此要实例化一个元素，需要创建一个扩展自`Element`的子类，并实现`contents`这个抽象方法。

```scala
class ArrayElement(conts: Array[String]) extends Element{
  def contents: Array[String] = conts
}
```

`extends`子句有两个作用：

- 它使得`ArrayElement`类从`Element`类继承所有非私有的成员；
- 它也让`ArrayElement`的类型称为`Element`的子类型。

ArrayElement是Element的`子类`，反过来Element是ArrayElement的`超类`。如果不加`extends`，类的定义默认继承自`scala.AnyRef`，这对应于`java.lang.Object`。

继承关系如下图：

```plain txt
 ┌────────────────────┐
 │       scala        │
 │       AnyRef       │
 │<<java.lang.Object>>│
 └────────────────────┘
           ▲
           │
           │
┌──────────────────────┐
│       Element        │
│     <<abstract>>     │
└──────────────────────┘
           ▲
           │
           │
┌──────────────────────┐    ┌───────────────┐
│     ArrayElement     │<>──│ Array[String] │
└──────────────────────┘    └───────────────┘
```

继承的意思是超类的所有成员也是子类的成员，但是有以下例外：

- 超类的私有成员并不会被子类成员继承；
- 子类里面已经实现的相同名称和参数的成员也不会继承，称为`复写(override)`了超类成员；
- 如果`子类的成员是具体`的而`超类的成员是抽象`的，称为`具体的成员`实现(`implement`)的`那个抽象成员`。

例如，ArrayElement重写（或者实现了）Element中的抽象方法contents。ArrayElement继承了Element的hight和width两个字段。

```scala
scala> val ae = new ArrayElement(Array("hello", "world"))
ae: ArrayElement = ArrayElement@58feb6b0

scala> ae.height
res1: Int = 2

scala> ae.width
res2: Int = 5
```

`子类型(subtying)`是子类的值可以被用在任何需要超类的值的场景。

```scala
scala> val e: Element = new ArrayElement(Array("hello"))
e: Element = ArrayElement@5c25daad
```

变量`e`的类型是`Element`，因此初始化它的值也应该是`Element`类型。

继承关系图中，ArrayElement和Array[String]存在组合关系，这个关系称之为组合，因为ArrayElement是由Array[String]组合出来的，Scala编译器会在为ArrayElement生成的二进制类文件中放入一个指向传入的conts数组的字段。

### 10.5 重写方法和字段

统一访问原则只是Scala比Java在处理字段和方法上更加统一的一个方面。另一个区别是Scala中`字段和方法属于同一个命名空间`。这使得用字段重写无参方法成为可能。如在ArrayElement中直接将Element的方法contents改写字段，并不需要修改Element的字段。

```scala
class ArrayElement(conts: Array[String]) extends Element{
  val contents: Array[String] = conts
}
```

**Scala禁止在同一类中使用相同的名称命名字段和方法**，而Java可以。

```scala
class WontCompile{
  private var f = 0 //不能编译，以为字段和方法重名
  def f = 1
}

<console>:13: error: method f is defined twice;
  the conflicting variable f was defined at line 12:13
       def f = 1}
           ^
```

Scala只有两个命名空间用于定义，Java是4个（字段、方法、类型和包）。Scala的两个命名空间分别为：

- 值（字段、方法、包和单例对象）
- 类型（类和特质名）

### 10.6 定义参数化字段

```scala
class ArrayElement(conts: Array[String]) extends Element{
  val contents: Array[String] = conts
}
```

上述定义中类的参数conts唯一目的是被拷贝到contents字段上，但这种看起来有种存在不必要的冗余和重复的信号。

可以通过`将参数和字段合并成参数化字段`的定义方式来避免这种情况：

```scala
class ArrayElement(
  val contents: Array[String]
) extends Element
```

`contents`前面的`val`，同时`定义参数和同名字段的方法`。

### 10.7 调用超类构造方法

面向对象的编程让我们很容易用新的数据变种来扩展一个已有的系统，只需要添加子类即可。再定义一个扩展子ArrayElement的LineElement类：

```scala
class LineElement(s: String) extends ArrayElement(Array(s)){
  override def width = s.length
  override def height = 1
}
```

由于LineElement继承自ArrayElement，而ArrayElement的构造方法接收一个参数Array[String]，LineElement主要向超类的主构造方法传入这样一个入参。要调用超类的构造方法，只需要将你打算传入的参数放在超类名称后面的圆括号中。例如，LineElement类就是将Array(s)放在其超类ArrayElement名称后面的括号里来将其传入ArrayElement的主构造方法。

```scala
...extends ArrayElement(Array(s))...
```

继承图如下：

```plain txt
 ┌────────────────────┐
 │       AnyRef       │
 │<<java.lang.Object>>│
 └────────────────────┘
           ▲
           │
┌──────────────────────┐
│       Element        │
│     <<abstract>>     │
└──────────────────────┘
           ▲
           │
┌──────────────────────┐    ┌───────────────┐
│     ArrayElement     │<>──│ Array[String] │
└──────────────────────┘    └───────────────┘
           ▲
           │
┌──────────────────────┐
│     LineElement      │
└──────────────────────┘
```

### 10.8 使用override修饰符

Scala要求在所有`重写父类具体成员`的成员之前加上`override`修饰符。而如果`不是重写，不能使用override修饰符`。

另外还存在`“不小心出现的重写”`是所谓的`“脆弱基类(frigile base class)”`。就是你对基类添加新成员，有可能和子类中的成员名称冲突。如果在之前基类中没有`hidden方法`，但在子类定义该方法的时候并没有加上`override`标识符，但在第二版修改基类的时候，添加了`hidden方法`，就会对子类产生影响，重新编译代码时编译器就会抛出错误。

### 10.9 多态和动态绑定

在10.4中，定义了一个Element类型的变量e：

```scala
val e: Element = new ArrayElement(Array("hello"))
```

类型为Element的变量可以指向一个类型为ArrayElement的对象。这种现象为`多态(polymorphism)`。意思是“多个形状”或“多种形式”。在这个例子中，Element对象可以有许多不同的展现形式。

现在Element有ArrayElement和LineElement两种形式，还可以定义新的Element子类来创建更多形式的Element，例如：

```scala
class UniformElement(
  ch: Char,
  override val width: Int,
  override val height: Int
)extends Element{
  private val line = ch.toString * width
  def contents = Array.fill(height)(line)
}
```

继承关系图如下：

```plain txt
          ┌──────────────────────┐
          │       Element        │
          │     <<abstract>>     │
          └──────────────────────┘
                      ▲
                      │
          ┌───────────┴──────────┐
          │                      │
┌──────────────────┐    ┌────────────────┐
│   ArrayElement   │    │ UniformElement │
└──────────────────┘    └────────────────┘
          ▲
          │
┌──────────────────┐
│   LineElement    │
└──────────────────┘
```

有了这种继承关系，Scala会接收如下所有赋值，因为赋值表达式会满足定义变量的类型要求：

```scala
scala> val e1: Element = new ArrayElement(Array("hello", "world"))
e1: Element = ArrayElement@52bba91a

scala> val ae: ArrayElement = new lineElement(("hello"))
ae: ArrayElement = lineElement@59c862af

scala> val ae: ArrayElement = new lineElement("hello")
ae: ArrayElement = lineElement@5ddf5118

scala> val e2: Element = ae
e2: Element = lineElement@5ddf5118

scala> val e3: Element = new UniformElement('x', 2, 3)
e3: Element = UniformElement@53202b06

```

如果检查这个继承关系，你会发现这4个val定义当中的每一个而言，`等号右边`的表达式是类型都`位于等号左边`被初始化的val的`类型的下方`。

这也说明对变量和表达式是`动态绑定`。实际`被调用的方法实现`是在`运行时基于对象的类型来决定的，而不是变量或表达式的类型决定的`。为了展现这种行为，将Element中的左右成员去掉，添加一个demo方法，并在ArrayElement和LineElement中复写demo，但在UniformElement中不复写demo方法。

```scala
abstract class Element{
  def demo() = {
    println("Element's implementation invoked")
  }
}

class ArrayElement extends Element{
  override def demo() = {
    println("ArrayElement's implementation invoked")
  }
}

class LineElement extends ArrayElement{
  override def demo() = {
    println("LineElement's impelentation invoked")
  }
}

class UniformElement extends Element
```

当把上述4个class的代码录入解释器，在定义一个方法，然后按照下述实例化：

```scala
scala> def invokeDemo(e: Element) = {e.demo()}
invokeDemo: (e: Element)Unit

scala> invokeDemo(new ArrayElement)
ArrayElement's implementation invoked

scala> invokeDemo(new LineElement)
LineElement's impelentation invoked

scala> invokeDemo(new UniformElement)
Element's implementation invoked
```

变量e的类型和表达式在运行时对应实例化的对象。UniformElement没有复写Element中的demo方法，运行时实例化的对象来自Element类的demo实现。

### 10.10 声明final成员

如果基类中定义的`成员不能被子类继承`，可以在成员前面加上`final修饰符`来实现。

```scala
class ArrayElement extends Element{
  final override def demo() = {
    println("ArrayElement's implementation invoked")
  }
}
```

再用相同的代码去定义LineElement时，就会抛出错误，提示不能复写一个final成员：

```scala
<pastie>:13: error: overriding method demo in class ArrayElement of type ()Unit;
 method demo cannot override final member
  override def demo() = {
               ^
```

如果还想定义的ArrayElement没有子类，可以在ArrayElement定义前加上final标识符：

```scala
final class ArrayElement extends Element{
  override def demo() = {
    println("ArrayElement's implementation invoked")
  }
}
```

使用相同的代码定义LineElement，就会抛出不能继承一个类型为final的类：

```scala
<pastie>:12: error: illegal inheritance from final class ArrayElement
class LineElement extends ArrayElement{
                          ^
```

### 10.11 使用组合和继承

如果主要追求的是代码复用，一般来说应当`优先使用组合而不是继承`。`只有继承才会受到弱基类问题的影响`，会在修改积累代码的时候不小心破坏了子类的代码。

也就是LineElement不要继承自ArrayElement而是继承自Element。修改后的继承关系如下：

```plain txt
            ┌──────────────────────┐
            │ Element <<abstract>> │
            └──────────────────────┘
                       ▲
                       │
      ┌────────────────┼────────────────┐
      │                │                │
┌────────────┐  ┌──────────────┐  ┌───────────┐
│ArrayElement│  │UniformElement│  │LineElement│
└────────────┘  └──────────────┘  └───────────┘
```

### 10.12 实现above、beside和toString

首先实现Element的above方法：

```scala
def above(that: Element): Element = {
  new ArrayElement(this.contents ++ that.contents)
}
```

其中`++`操作是将两个数组拼接在一起。Scala的数组由Java的数组表示的，不过Scala数组也可以被转换成`scala.Seq`。

再来实现beside：

```scala
def beside(that: Element): Element = {
  val contents = new Array[String](this.contents.length)
  for (i <- 0 until this.contents.length)
    contents(i) = this.contents(i) + that.contents(i)
  new ArrayElement(contents)
}
```

上面代码为指令式编写，可以换一种更为简洁的方法：

```scala
new ArrayElement(
  for ((line1, line2) <-this.contents zip that.contents))
    yield line1 + line2
)
```

zip将this.contents和that.contents这两个数组转换为对偶（即Tuple2）数组，选取两个数组中对应位置上的值，组装成一个对偶(pair)数组。

```scala
scala> Array(1, 2, 3) zip Array(4, 5, 6)
res7: Array[(Int, Int)] = Array((1,4), (2,5), (3,6))

scala> Array(1, 2, 3) zip Array(4, 5)
res0: Array[(Int, Int)] = Array((1,4), (2,5))
```

如果数组长度不一致，zip将会扔掉多余的元素，第二个例子中3被丢掉了。

最后实现toString方法：

```scala
override def toString = contents.mkString "\n"
```

到目前为止，Element类实现如下：

```scala
abstract class Element{
  def contents: Array[String]

  def width: Int = if (height == 0) 0 else contents.length

  def height: Int = contents.length

  def above(that: Element): Element = {
    new ArrayElement(that.contents ++ this.contents)
  }

  def beside(that: Element): Element = {
    new ArrayElemnt(
      for (
        (line1, line2) <- that.contents zip this.contents
      ) yield line1 + line2
    )
  }

  override def toString = contents.mkString "\n"
}
```

### 10.13 定义工厂对象

现在已经拥有一组用于布局元素的类，可以“原样”展现给使用方，不过你可能想把继承关系藏在一个工厂对象背后。

工厂对象包含创建其他对象的方法。使用这些工厂方法来构造对象，而不是直接用new构建对象。这种做法的好处是对象创建逻辑可以被集中起来，而对象是如何用具体的类表示的可以被隐藏起来。这样既可以让你的类库更容易被对方理解，因为暴露的更少，同时还提供了更多的机会让你在未来在不破坏使用方代码的前提下改变类库的实现。

为布局元素构建工厂方法，直接的方案是常见一个`Element类的伴生对象`，作为布局元素的工厂对象。这样你只需要暴露Element这组类/对象给使用方，并将ArrayElement、LineElement和UniformElement隐藏起来。

```scala
object Element{
  def elem(contents: Array[String]): Element = {
    new ArrayElement(contents)
  }

  def elem(chr: Char, width: Int, height: Int): Element = {
    new UniformElement(chr, width, height)
  }

  def elem(ling:String): Element = {
    new LineElement(line)
  }
}
```

创建这些工厂方法之后，我们要为Element的定义做出一些改变，让它用工厂方法而不是直接显式地创建ArrayElement。为了在调用工厂方法时不显式地给出Element这个单例对象名称的限定词，在源码头处引入Element.elem，也就是在Element中不再用Element.elem来调用工厂方法，而是引入Element.elem后直接使用elem。

```scala
import Element.elem

abstract class Element{
  def contents: Array[String]

  def width: Int = if (height == 0) 0 else contents.length

  def height: Int = contents.length

  def above(that: Element): Element = {
    elem(that.contents ++ this.contents)
  }

  def beside(that: Element): Element = {
    elem(
      for (
        (line1, line2) <- that.contents zip this.contents
      ) yield line1 + line2
    )
  }

  override def toString = contents.mkString "\n"
}
```

有了工厂方法之后，ArrayElement、LineElement和UniformElement可以变为私有的，因为不再被使用方直接调用了。在Scala中，可以在其他类或者单例对象中定义类和单例对象。将Element子类变为私有的方式之一是把它们放在Element单例对象中，并声明为私有。

```scala
object Element{
  private class ArrayElement(
    val contents: Array[String]
  ) extends Element

  private class LineElement(s: String) extends Element{
    val contents = Array(s)
    override def width = s.length
    override def height = 1
  }

  private class UniformElement(
    ch: Char,
    override val width: Int,
    override val height: Int
  )extends Element{
    private val line = ch.toString * width
    def contents = Array.fill(height)(line)
  }

  def elem(contents: Array[String]): Element = {
    new ArrayElement(contents)
  }

  def elem(chr: Char, width: Int, height: Int): Element = {
    new UniformElement(chr, width, height)
  }

  def elem(line: String): Element = {
    new LineElement(line)
  }
}
```

### 10.14 增高和增宽

最后还需要一个增强，因为Element并不是很够用，因为它不允许使用方将不同宽度的元素叠加在一起，或者将不同高度的元素并排放置。例如下面求值不能正常工作。

```scala
new ArrayElement(Array("hello")) above
new ArrayElement(Array("world!"))
```

我们在定义一个私有的助手方法widen和heighten，使得宽度或高度不同的元素，短的通过添加空格来补齐宽度。

```scala
import Element.elem

abstract class Element{
  def contents: Array[String]

  def width: Int = if (height == 0) 0 else contents.length

  def height: Int = contents.length

  def above(that: Element): Element = {
    val this1 = this widen that.width
    val that1 = that widen this.width
    elem(this1.contents ++ that1.contents)
  }

  def beside(that: Element): Element = {
    val this1 = this heighten that.width
    val that1 = that heighten this.width
    elem(
      for (
        (line1, line2) <- this1.contents zip that1.contents
      ) yield line1 + line2
    )
  }
  
  def widen(w: Int): Element = {
    if (w <= width) this
    else {
      val left = elem(' ', (w - width) / 2, height)
      val right = elem(' ', w - width - left.width, height)
      left beside this beside right
    }
  }

  def heighten(h: Int): Element = {
    if (h <= height) this
    else {
      val top = elem(' ', width, (h - height) / 2)
      val bot = elem(' ', width, h - height - top.height)
      top above this above bot
    }
  }

  override def toString = contents mkString "\n"
}
```

### 10.15 放在一起

练习使用布局类库的几乎所有元素的趣味方式编译一个给定的边数绘制螺旋程序。

```scala
import Element.elem

object Spiral {
  val space = elem(" ")
  val corner = elem("+")
  def spiral(nEdges: Int, direction: Int): Element = {
    if (nEdges == 1) elem("+")
    else {
      val sp = spiral(nEdges - 1, (direction + 3) % 4)
      def verticalBar = elem('|', 1, sp.height)
      def horizontalBar = elem('-', sp.width, 1)
      if (direction == 0)
        (corner beside horizontalBar) above (sp beside space)
      else if (direction == 1)
             (sp above space) beside (corner above verticalBar)
      else if (direction == 2)
                (space beside sp) above (horizontalBar beside corner)
      else (verticalBar above corner) beside (space above sp)
    }
  }
  def main(args: Array[String]) = {
    val nSides = args(0).toInt
    println(spiral(nSides, 0))
  }
}
```

## 11 Scala层级结构

在Scala中，每个类都继承自一个名为`Any`的公共超类。任何类都是`Any`的子类，任何对象都可以调用在Any中定义的方法。Scala还在底层定义了一些有趣的类，`Null`和`Nothing`，来充当公共子类。

### 11.1 Scala类的层次结构

顶层的Any类包含了如下定义：

```scala
final def ==(that: Any): Boolean
final def !=(that: Any): Boolean
def equals(that: Any): Boolean
def ##: Int
def hashCode: Int
def toString: String
```

由于任何类都是继承于Any，所以Any中定义的这些方法都可以在子类中使用。`==`和`!=`为`final`类型，所以不能在子类中复写。`==`方法本质上与`equals`相同，并且`!=`总是与`equals`相反。

根类Any有两个子类：`AnyVal`和`AnyRef`。`AnyVal`是Scala中value classes的父类。value classes共有9个内置类型：`Byte, Short, Char, Int, Long, Float, Double, Boolean, Unit`，其中的前八个对应于Java的基本类型，它们的值在运行时表示为Java的基本值，在Scala中，这些类的实例会被表示为字面量。另外是，`不能使用new创建这些类的实例`，value classes都定义为既是抽象(abstract)的又是最终(final)的。

```scala
scala> new Int
<console>:12: error: class Int is abstract; cannot be instantiated
       new Int
       ^
```

内置类型之前互不为子类，类型之间的转换通过`隐式转换`完成。

### 11.2 基本类型的实现机制

像加法或乘法这样的标准操作被实现为基本操作。当需要将一个整数需要被看做Java对象时，Scala会使用一个`backup`类`java.lang.Integer`做转换，例如，当对一个整数调用toString方法时，或者当将一个整数赋给一个Any类型的变量时，就会发生这种情况。这种情况类似于Java的`自动装箱和拆箱`，但在Scala中，这种方式更加隐蔽。

```java
boolean isEqual(int x, int y){
  return x == y
}

System.out.println(isEqual(421, 421));
```

返回`true`;

```java
boolean isEqual(Integer x, Integer y){
  return x == y
}

System.out.println(isEqual(421, 421));
```

会发现得到结果为`false`。发生的情况是数字421被装箱两次，因此x和y是两个不同的对象。因为`==表示引用类型上的引用相等性`，而Integer是引用类型，所以结果为`false`。

这方面说明Java不是一种纯粹的面向对象语言。基元类型和引用类型之间的差异可以清楚地看到。

再使用Scala尝试：

```scala
scala> def isEqual(x: Int, y: Int) = x == y
isEqual: (x: Int, y: Int)Boolean

scala> 421 == 421
res5: Boolean = true

scala> def isEqual(x: Any, y: Any) = x == y
isEqual: (x: Any, y: Any)Boolean

scala> 421 == 421
res6: Boolean = true
```

Scala中的相等操作==被设计为与类型表示相关的透明操作。对于值类型，它是自然的(数字或布尔)相等性。对于Java装箱的数值类型以外的引用类型==，被视为从对象继承的equals方法的别名。

如果要比较引用相等性，使用`AnyRef`定义的`eq`方法:

```scala
scala> val x = new String("abc")
x: String = abc

scala> val y = new String("abc")
y: String = abc

scala> x == y
res7: Boolean = true

scala> x eq y
res8: Boolean = false

scala> x ne y
res9: Boolean = true
```

### 11.3 底类型(bottom types)

在继承关系的底部，有两个类`scala.Null`和`scala.Nothing`，它们是Scala面向对象的类型系统用于统一处理某些“极端情况”的特殊类型。

`Null`是`null`引用的类型，它是每个应用类的子类。但Null并不兼容于值类型，比如并不能将null赋值给一个整型变量：

```scala
scala> val i: Int = null
<console>:11: error: an expression of type Null is ineligible for implicit conversion
       val i: Int = null
                    ^
```

Nothing也是其他类型的子类，不过`并不存在这个类型的任何值`。**`Nothing的用途之一是给出非正常终止的信号`**。Scala标准库中的Predef对象有一个error方法，其定义如下：

```scala
def error(message: String): Nothing = {
  throw new RuntimeExpection(message)
}
```

error的返回类型为Nothing，这是告诉使用该方法并不会正常返回（会抛出异常）。由于Nothing是每个类型的子类，可以用非常灵活的方式来使用error方法：

```scala
def divide(x: Int, y: Int): Int = {
  if (y != 0) x / y
  else error("can't divide by zero")
}
```

### 11.4 定义自己的值类型

可以定义自己的类型来对已有的值类进行扩充，跟内建的值类一样，自定义的值类的实例通常也会编译成那种不使用包装类的Java字节码。在需要包装类的上下文里，比如泛型代码，值将被自动装箱和拆箱。

**只有特定的几个类可以定义为值类。要使得某个类成为值类，它不仅有且仅有一个参数，并且在内部除了def之外不能有任何东西**。不仅如此，也不能有其他扩展自值类，且值类不能重新定义`equals`或`hashCode`。

要定义值类，需要将它处理成`AnyVal`的子类，并在它唯一的参数前加上`val`：

```scala
class Dollars(val amount: Int) extends AnyVal{
  override def toString() = "$" + amount
}
```

参数amount前面的val，可以让amount参数可以作为字段被外界访问。

```scala
scala> val money = new Dollars(10000)
money: Dollars = $10000

scala> money.amount
res0: Int = 10000
```

## 12 特质

特质是Scala代码复用的基础单元。特质将方法和字段定义封装起来，然通过将他们混入类的方式来实现复用。它不同于类继承，类继承要求每个子类有明确的的超类，而类可以同时混入任意数量的特质。

### 12.1 特质如何工作

特质的定义和类的定义很像：

```scala
scala> trait Philosophical{
     | def philosophize() = {
     | println("I consume money, therefore I am!")
     | }
     | }
defined trait Philosophical
```

该特质并没有声明一个超类，因此跟类一样，有一个`默认超类AnyVal`。一旦特质定义好，就可以用`extends`是或`with`关键字将它混入类中。`Scala程序混入(mix in)特质，而不是从特质继承`，因为混入特质跟其他许多编程语言中的多重继承有重要的区别。下面展示一个用混入Philosophical特质的类：

```scala
scala> class Frog extends Philosophical{
     | override def toString = "green"
     | }
defined class Frog
```

可以用extends关键字来混入特质，在这种情况下隐式地继承了特质的超类。从特质继承的方法跟从超类继承的方法用起来是一样的。

```scala
scala> val frog = new Frog
frog: Frog = green

scala> frog.philosophize
I consume money, therefore I am!
```

特质同时也定义了一个类型，以下是Philosophical被用作类型的例子：

```scala
scala> val phil: Philosophical = frog
phil: Philosophical = green

scala> phil.philosophize()
I consume money, therefore I am!
```

phil的类型为Philosophical，这是一个特质，因此变量phil可以由任何混入了Philosophical的类的对象初始化。

如果想要将特质混入一个显式继承自某个超类的类，可以用extends来给出超类，用with来混入特质，如果想混入更多特质，可以用with子句进行添加：

```scala
scala> class Animal
defined class Animal

scala> class Frog extends Animal with Philosophical{
     | override def toString = "green"
     | }
defined class Frog

scala> trait HasLegs
defined trait HasLegs

scala> class Frog extends Animal with Philosophical with HasLegs{
     | override def toString = "green"
     | }
defined class Frog
```

Frog类从Philososphical继承了philosophical的实现，Frog也可以重写philosophical，重写语法跟重写超类中声明的方法看上去一样：

```scala
scala> class Frog extends Animal with Philosophical{
     | override def toString = "green"
     | override def philosophize() = {
     | println("It ain't easy being" + toString + "!")
     | }
     | }
defined class Frog
```

由于Frog复写了philosophical，再调用时将得到新的行为：

```scala
scala> val phil: Philosophical = new Frog
phil: Philosophical = green

scala> phil.philosophize
It ain't easy being green!
```

`特质可以声明字段并保持状态，事实上，在特质定义中可以做任何在类定义中做的事，语法也完全相同`。但除了以下两种情况：

- 特质中不能有任何“类”参数（即那些传入类的主构造方法的参数），可以定义这样一个类`class Point(x: Int, y: Int)`，但是不能这样定义一个特质`trait Point(x: Int, y: Int)`，这样会无法编译；

- 另一个是类和特质的区别，在于`类中的super调用时静态绑定`的，而`特质中super是动态绑定的`。类中编写`super.toString()`可以确切地知道实际调用的是哪一个实现的，但每次该特质混入到某个具体的类时，每次调用都要重新判断。

### 12.2 瘦接口和富接口

特质的一个主要用途是自动给类添加基于已有方法的新方法，可以丰富一个瘦接口，让它成为一个富接口。

瘦接口方法少，实现起来简单；富接口方法多，调用方便但实现相对麻烦。

Java的接口一般都是瘦接口，如CharSequence，是一个对所有包含一系列字符的类似字符串的类的通用瘦接口，在Scala中如下定义：

```scala
scala> trait CharSequence{
     | def charAt(index: Int):Char
     | def length:Int
     | def subSequence(start:Int, end: Int): CharSequence
     | def toString(): String
     | }
defined trait CharSequence
```

给特质添加方法让瘦接口和富接口之间的取舍变得严重倾向于富接口。不同于Java，`给Scala特质添加具体方法是一次性投入，只需要在特质中实现这些方法一次，而不需要在每个混入特质的类中重新实现一遍`。因此，拥有特质的Scala语言，让富接口实现的代价更小。

用特质来丰富某个接口，只需`定义一个拥有为数不多的抽象方法`（瘦接口）和`一个拥有数量很多的具体方法的特质`，然后将该特质混入到某个类中。

### 12.3 实例：矩形对象

在不使用特质的前提下，定义点和矩形：

```scala
class Point(x: Int, y: Int)

class Rectangle(val topLeft: Point, val bottomRight: Point){
  def left = topLeft.x
  def right = bottomRight.x
  def width = right - left
}
```

还有要定义一个图形组件：

```scala
abstract class Component{
  def topLeft: Point
  def bottomRight: Point

  def left = topLeft.x
  def right = bottomRight.x
  def width = right - left
  // more methods
}
```

可以看出Rectangle和Component两者的定义方法有很多相似之处，此时可以通过增值特质来消除重复的代码：

```scala
trait Rectangler{
  def topLeft: Point
  def bottomRight: Point

  def left = topLeft.x
  def right = bottomRight.x
  def width = right - left
  // more methods
}
```

然后Rectangle和Component可以混入特质来减少代码数量：

```scala
abstract class Component extends Rectangler{
  //other methods
}

class Rectangle(val topLeft: Point, val bottomRight: Point) extends Rectangler{
  //other methods
}
```

创建一个实例，并查询：

```scala
scala> val rect = new Rectangle(new Point(1,1), new Point(10, 10))
rect: Rectangle = Rectangle@66a99584

scala> rect.left
res1: Int = 1

scala> rect.right
res2: Int = 10

scala> rect.width
res3: Int = 9
```

### 12.4 Ordered

Scala提供一种特质用于比较两个对象，这个特质叫`Ordered`，使用的方式是将所有单独的比较方法替换成compare方法，Ordered特质提供了<、>、<=和<=，这些方法都是基于提供的compare方法来实现，如果没有使用Ordered特质，那么定义两个对象比较的方法为：

```scala
class Rational(n: Int, d: Int) {
  // ...
  def < (that: Rational) ={
    this.numer * that.denom < that.numer * this.denom
  }
  def > (that: Rational) = that < this
  def <= (that: Rational) = (this < that) || (this == that)
  def >= (that: Rational) = (this > that) || (this == that)
}
```

使用`Ordered`特质时，只需要定义一个`compare`方法，然后混入到具体类中即可：

```scala
class Rational(n: Int, d: Int) extends Ordered[Rational]{
  // ...
  def compare(that: Rational) = {
    (this.numer * that.demon) - (that.numer * this.demon)
  }
}
```

有几个需要注意的地方：

- 与其他混入时不同，Ordered要求在混入时，必须传入一个`类型参数`(type paramter)，即`Ordered[C]`，其中C是要比较元素的类型。
- 第二件事就是定义一个用来比较两个对象的compare方法，该方法应该比较接收者，即this，和作为参数传入该方法的对象。
- Ordered并不会帮助你定义equals方法，因为它做不到。这当中的问题在于用compare来实现equals需要检查传入对象的类型，对于(Java的)类型擦除机制，Ordered特质自己无法完成这项检查，因此需要自己定义equals方法。

### 12.5 作为可叠加修改的特质

特质的一个重要用途是`将瘦接口转化为富接口`，另外还有一个用途是`为类提供可叠加的修改`。特质让你修改类的方法，而它们的实现方式允许你将这些修改叠加起来。

对某个整数队列叠加修改，队列有两个操作：put和get。给定一个实现了这样一个队列的类，可以定义一些特质来执行以下修改：

- Doubling：将放入队列的元素翻倍；
- Incrementing：将放入队列的元素+1；
- Filtering：从队列去除负数。

首先，定义一个抽象类：

```scala
abstract class IntQueue{
  def get(): Int
  def put(x: Int)
}
```

再定义一个使用ArrayBuffer的IntQueue的基本实现：

```scala
import scala.collection.mutable.ArrayBuffer

class BasicIntQueue extends IntQueue{
  private val buf = new ArrayBuffer[Int] //数组缓冲
  def get() = buf.remove(0)
  def put(x: Int) = buf += x
}

scala> val queue = new BasicIntQueue
queue: BasicIntQueue = BasicIntQueue@2419fe6f

scala> queue.put(10)

scala> queue.put(20)

scala> queue.get()
res2: Int = 10
```

如何用特质修改这个行为？给每个放入的元素加倍：

```scala
trait Doubling extends IntQueue{
  abstract override def put(x: Int) = (super.put(2 * x))
}
```

该特质首先`声明一个超类IntQueue`，这个声明意味着这个特质`只能被混入同样继承自IntQueue的类`。因此可以混入BasicIntQueue，但不能混入Rational。

另外一个需要注意的是，在特质中做了一个super调用，对于普通的类，这样做是非法的，运行时会失败。但在特质中是可以成功的，由于`特质中的super调用时动态绑定的，只要在给出了该方法的具体定义的特质或类之后混入`，Doubling中的super调用就可以正常工作。

为了实现可叠加的特质，且为了告诉编译器是特意这样做的，必须将这样的方法标记为abstract override，该修饰符组合`只允许用在特质的成员上，不允许用在类的成员上`，它的含义是该特质`必须混入某个拥有该方法的具体定义的类中`。

```scala
scala> class MyQueue extends BasicIntQueue with Doubling
defined class MyQueue

scala> val queue = new MyQueue
queue: MyQueue = MyQueue@105e8710

scala> queue.put(10)

scala> queue.get() //由于特质Doubling的混入，放入的10被翻倍了
res6: Int = 20
```

由于MyQueue没有定义新的代码，这时也可直接实例化BasicIntQueue with Doubling：

```scala
scala> val queue = new BasicIntQueue with Doubling
queue: BasicIntQueue with Doubling = $anon$1@2411cf8b

scala> queue.put(100)

scala> queue.get()
res8: Int = 200
```

为了搞清楚叠加修改，我们再定义Incrementing和Filtering两个特质：

```scala
trait Incrementing extends IntQueue{
  abstract override def put(x: Int) = {super.put(x + 1)}
}

trait Filtering extends IntQueue{
  abstract override def put(x: Int) = {if(x >= 0) super.put(x)}
}
```

将这俩这特混入类中：

```scala
scala> val queue = new BasicIntQueue with Incrementing with Filtering
queue: BasicIntQueue with Incrementing with Filtering = $anon$1@6141c271

scala> queue.put(-1)

scala> queue.put(1)

scala> queue.get()
res11: Int = 2 //由于Incrementing特质的混入，给1做了加1操作
//  且由于Filtering的因素，-1没有被放到队列中
```

混入特质的顺序很重要：**`越靠右出现的特质最先起作用`**。这就是在输入值为-1时，先调用了Filtering将-1直接滤除了，没有放入队列中。如果将两个特质的顺序调换，就会出现如下结果：

```scala
scala> val queue = new BasicIntQueue with Filtering with Incrementing
queue: BasicIntQueue with Filtering with Incrementing = $anon$1@64c25a62

scala> queue.put(-1); queue.put(0); queue.put(1)

scala> queue.get()
res1: Int = 0  //先执行Incrementing，再执行Filtering，-1不会被滤除

scala> queue.get()
res2: Int = 1

scala> queue.get()
res3: Int = 2
```

### 12.6 为什么不使用多重继承

特质与其他语言中的多重继承有这重大区别，有一点特别重要：`对super的解读`。在`多重继承中，super的调用在调用发生的地方就已经确定了`。而`特质中super的调用的方法取决于类和混入该类特质的`**线性化**。正是这样，让前述的叠加修改成为可能。

前述的叠加修改，如果不是特质而是使用多重继承：

```scala
val q = new BasicIntQueue with Incrementing with Doubling
q.put(42) //应该会调用哪个put
```

这次调用执行的是Increamenting和Doubling两个中的哪个put？也许Doubling中的put被执行，输入的数据翻倍，调用了`super.put`，然后结束，不会再执行Increamenting中的`super.put`。

也许还可以让程序员自己指定调用super是到底使用哪个超类的方法：

```scala
trait MyQueue extends BasicIntQueue with Incrementing with Doubling{
  def put(x: Int) = {
    Increamenting.super.put(x) //并非真正的代码
    Doubling.super.put()
  }
}
```

这样做的可能是put方法被调用两次，不过两次都不是用加1或翻倍后的值调用的。总之来说，多重继承没有很好的解决方案。

相较而言，Scala特质的解决方案直截了当，只需将特质简单的混入类中即可。与多重继承相比，特质的区别在于`线性化`。Scala会将类及他所有继承的类和特质都拿出来，将它们线性的排列在一起。然后，当在某一个类中调用super时，被调用的方法是这个链条中向上最近的那个。如果出最后一个方法外，所有的方法都以调用了super，那么最终的结果就是叠加在一起的行为。

> 在任何线性化中，类总是位于所有它的超类和混入特质之前，因此，当你写下`调用super的方法`时，那个`方法绝对在修改超类和混入特质的行为`，而不是反过来。

下面我们举例，有一个Cat类，继承自超类Animal，又混入Furry和Fourlegged两个特质，Fourlegged又扩展自HasLegs：

```scala
class Animal
trait Furry extends Animal
trait HasLegs extends Animal
trait Fourlegged extends HasLegs
class Cat extends Animal with Furry with Fourlegged
```

Cat的线性化从后往前计算过程如下。Cat线性化的最后一部分是其Animal的线性化：

```plaintxt
Animal ➜ AnyRef ➜ Any
```

第2个是混入的特质Furry：

```plaintxt
Furry ➜ Animal ➜ AnyRef ➜ Any
```

第3个是混入的特质Fourlegged：

```plaintxt
Fourlegged ➜ HasLegs ➜ Furry ➜ Animal ➜ AnyRef ➜ Any
```

最后线性化的是Cat自己：

```plaintxt
Cat ➜ Fourlegged ➜ HasLegs ➜ Furry ➜ Animal ➜ AnyRef ➜ Any
```

所以Cat类的继承关系和实例化如下

|类型|线性化|
|--|--|
|Animal|Animal、AnyRef、Any|
|Furry|Furry、Animal、AnyRef、Any|
|Fourlegged|Fourlegged、HasLegs、Animal、AnyRef、Any|
|HasLegs|HasLegs、Animal、AnyRef、Any|
|Cat|Cat、Fourlegged、HadLegs、Furry、Animal、AnyRef、Any|

当这些类和特质中的任何一个通过super调用某个方法时，被调用是在线性化链条中出现在其右侧的首个实现。

### 12.7 要特质还是不要特质

- 如果某个行为不会被复用，用具体类。
- 如果某个行为可能被用于多个互不相关的类，用特质。只有特质才能被混入类继承关系中位于不同组成部分的类。
- 如果想要从Java代码中继承某个行为，用抽象类。从Java类继承特质比较别扭，且带有实现的特质并没有与之贴近的Java类比。
- 如果计划将某个行为以编译好的形式分发，且预期会有外部的组织编写继承自它的类，可能更倾向于使用抽象类。

## 13 包和引入

减少耦合，对构建大型程序是很重要的。低耦合能减少程序某个局部的某个看似乌海的改动对其他部分造成严重后的风险。减少耦合的一宗方式是以模块化的风格编写代码。

### 13.1 将代码放进包里

将类navigator放入bobsrockets.navigation包中。

```scala
package bobsrockets.navigation
class navigator
```

由于Scala代码是Java生态的一部分，建议遵循Java将域名倒过来作为包名的习惯。如com.bobsrockets.navigation。

另一种方式是将代码用{}包括起来。这个语法称为`打包(packaging)`。

```scala
package bobsrockets.navigation {
  class navigator
}
```

这样的写法，可以让我们在一个文件里包含多个包的内容，举例来说，可以把某个类的测试代码跟源代码放在同一个文件里，不过分成不同的包：

```scala
package bobsrockets {
  package navigation {
    class Navigator
  }
  
  package tests {
    class NavigatorSuite
  }
}
```

### 13.2 对相关代码的精简访问

当代码被划分为包层次结构时，它不仅仅帮助人们浏览代码。它还告诉编译器代码相同包中的代码，某种程度上是相关的。

```scala
package bobsrockets{
  package navigation{
    class Navigator{
      val map = new StartMap //和StartMap同在一个package中，能直接访问
    }
    class StartMap
  }

  class Ship{
    val nav = new navigation.Navigator
    //和navigation同在bobsrockets中，不用写为bobsrockets.navigation.navigator
  }

  package fleets{
    class Fleet{
      def addShip = {
        new Ship //可直接使用，不用写作bobsrockets.Ship
      }
    }
  }
}
```

另外一种情况，分成两个独立的包：

```scala
package bobsrockets{
  class Ship
}

package bobsrockets.fleets{
  class Fleet{
    def addShip() = {
      new Ship //不能编译，跨包引用，得写明全称
    }
  }
}
```

在定义包时，`bobsrockets和bobsrockets.fleets没有直接的归属关系，是不同的两个包`。

```scala
package lanuch{
  class Booster3
}

package bobsrockets{
  package navigation{
    package lanuch{
      class Booster1
    }

    class MissionControl{
      val booster1 = new lanuch.Booster1
      val booster2 = new bobsrockets.navigation.lanuch.Booster2
      val booster3 = new _root_.lanuch.Booster3
    }
  }

  package lanuch{
    class Booster2
  }
}
```

Scala提供了一个名为`_root_`的包，它位于用户可以编写的任何包之外。换句话说，可以编写的每个**顶级包**都被视为`_root_`包的成员。例如，上例中的`launch和bobsrockets都是package的成员_root_`。结果是_root_.launch提供了顶级lanuch包和_root_.launch。Booster3指定了最外层的booster类。

### 13.3 导入(import)

与Java类似，导入子句使包或对象的成员仅凭其名称即可访问，而不需要以包或对象名称作为前缀。这里有一些简单的例子：

```scala
import bobsdelights.Fruit

import bobsdelights._ //导入bobsdelights下全部成员

import bobsdelights.Fruit._ //导入bobsdelights.Fruit下全部成员

package bobsdelights
abstract class Fruit(
  val name: String,
  val color: String
)

object Fruit{
  object Apple extends Fruit("apple", "red"),
  object Orange extends Fruit("orange", "orange")
  object Pear extends Fruit("pear", "yellowish")
  val menu = List(Apple, Orange, Pear)
}

def showFruit(fruit: Fruit) = {
  import fruit._
  println(name + "s are " + color)
}
```

`showFruit`中`fruit`是`Fruit`类型，直接引入了其参数fruit（类型为Fruit）`的全部成员`，可以直接访问`name`和`color`，与`fruit.name`和`fruit.color`代表的含义相同。

Scala灵活导入包的方式：

```scala
import java.util.regex

class AStarB{
  val pat = regex.Pattern.compile("a*b")
  //导入后可直接使用regex，而不用全称java.util.regex
}

import Fruits.{Apple, Orange} //只导入Apple, orange
import Fruits.{Apple => McIntosh, Orange} //将导入的Apple重命名为McIntosh
import Fruits.{_} //导入全部成员
import Fruits.{Apple => McIntosh, _} //导入全部成员，但Apple重命名为McIntosh
import Fruits.{Pear => _, _} //导入除了Pear之外的全部成员
//<original-name> => _：代表意思为将某个成员隐藏，不导入
```

### 13.4 隐式导入

```scala
import java.lang._ // Scala脚本中，默认导入
import scala._ // Scala脚本中，默认导入
import Predef._ // Scala脚本中，默认导入
```

### 13.5 访问修饰符

包、类或对象的成员可以用访问修饰符`private`和`protected`标记。这些修饰符将对成员的访问限制在代码的某些区域。Scala对访问修饰符的处理大致遵循Java的方法，但是有一些重要的区别，本节将对此进行解释。

#### 13.5.1 Private成员

与Java类似，但是Scala的private成员只能在定义的内部使用，不允许通过`类型.方法名`访问：

```scala
class Outer{
  class Inner{
    private def f() = {println("f")}

    class InnerMost{
      f() //在Inner内部，可以访问
    }
  }
  (new Inner).f() //不能访问，不在Inner内部
}
```

#### 13.5.2 Protected成员

在Scala中，对受保护成员的访问也比在Java中有更多的限制。在Scala中，`受保护成员只能从定义成员的类的子类中访问，同一个包中其他类不能调用`。在Java中，这样的访问也可以来自同一个包中的其他类。

```scala
package p{
  class Super{
    protected def f() = {println("f")}
  }

  class Sub extends Super{
    f() //可以访问，Sub是Super的子类
  }

  class Other{
    (new Super).f() //不能访问，Other不是Super的子类
  }
}
```

### 13.5.3 保护范围

有访问权限的修饰符为提供了非常细粒度的控制。

```scala
package bobsrockets
package navigation{
  private[bobsrockets] class Navigator{
    protected[navigation] def useStarChart() = {}
    class LegOfJourney{
      private[Navigator] val distance = 100
    }
    private[this] val speed = 200
  }
}

package lanuch{
  import navigation._
  object Vehicle{
    private[lanuch] val guide = new Navigator
  }
}
```

- `private[bobsrockets] class Navigator`表示`Navigator`在`bobsrockets`的所有类中均可见。
- C类中的`protected[X]`允许C中所有子类以及封装的包、类或对象X访问。
- `protected[navigation] def useStarChart()`允许Navigator的所有子类及nagivation包中的所有代码访问。

最后，Scala还有一个访问修饰符，它的限制比private还要严格。 标记为`private[this]`的定义只能从包含该定义的`同一对象`中访问。这种定义被称为`对象私有(object-private)`。Navigator中的speed就被定义为这种类型。意味着不仅只能在Navigator中使用，还必须是`同一个实例对象访问`。在Navigator中可以使用`“speed”`或者`“this.speed”`访问，其他形式的访问都是非法的。

```scala
val other = new Navigator
other.speed // 编译报错
```

#### 可见性和伴生对象

涉及到private或protected成员访问时，Scala的访问规则为对象和类提供了特权。`一个类与它的伴生对象共享所有访问权限`，反之亦然。

特别地，一个对象可以访问它的伴生类的所有私有成员，就像一个类可以访问它的伴生对象的所有私有成员一样。

如下面定义，Rocket类和其伴生对象，Rocket类可以访问其伴生对象私有fuel方法，反过来Rocket对象可以访问其伴生类Rocket的私有canGoHomeAgain方法。

```scala
class Rocket{
  import Rocket.fuel
  private def canGoHomeAgain = fuel > 20
}

object Rocket{
  private def fuel = 10
  def chooseStragegy(rocket: Rocket) = {
    if(rocket.canGoHomeAgain)
      goHome()
    else
      pickAStar()
  }
  def goHome() = {}
  def pickAStar() = {}
}
```

对于Scala的伴生对象而言，`protected`的成员没有意义，因为`单例对象没有子类`。

### 13.6 包对象

到目前为止，看到添加到包中的代码是类、特质和单例对象。这些是放在包的顶层的最常见的定义。但是Scala并没有限制你只是这样做，在类中的任何类型的定义都可以放在包的顶层。如果有一些帮助器方法，想要在整个包的范围内，那么就将它放在包的顶层。

为了这样做，可以将其放进`包对象(package object)`中，每个包允许有一个包对象。放在包对象中的任何定义都被认为是包本身的成员。

```scala
//-------------- bobsdelights/package.scala中代码
package object bobsdelights{
  def showFruit(fruit: Fruit) = {
    import fruit._
    println(name + "s are " + color)
  }
}

//--------------PrintMenu.scala中代码
package printmenu
import bobsdelights.Fruits
import bobsdelights.showFruit

object PrintMenu{
  def main(args: Array[String]) = {
    for (fruit <- Fruit.menu){
      showFruit(fruit)
    }
  }
}
```

从上述代码中，我们看到在package.scala代码中，定义了一个包对象，与包定义唯一不同的是多了个object限定词。有了这个定义，任何包中的任何其他代码都可以像导入类一样导入这个方法。PrintMenu可以以导入类Fruit的相同方式导入showFruit。

包对象经常用于保存包范围的类型别名(第20章)和隐式转换(第21章)。`顶级scala包有一个包对象，所有scala代码都可以使用它`。

包对象会被编译成名为`Package.class`的类文件，该文件位于包的目录中。对源文件保持相同的约定是很有用的。上面例子中的包对象bobs的源文件会放入名为`package.scala`的文件中，放在`bobsdelights`的目录下。

## 14 断言和测试

断言和测试是检查软件行为是否符合预期的重要手段。

### 14.1 断言

断言的写法是对预定义方法assert的调用，assert方法定义在Predef的单例对象中。如果`condition`不满足，表达式`assert(condition)`将抛出`AssertionError`。

另一个版本是：`assert(condition, expalanation)`，如果condition条件不满足，就抛出给定explanation的AssertionError，explanation的类型为`Any`，因此`可以传入任何对象`。assert方法将调用expalanation的toString方法来获取一个字符串的解释放入AssertError中。

```scala
def above(that: Element): Element = {
  val this1 = this widen that.width
  val that1 = that widen this.width
  assert(this1.width == that.width) //增加断言，保证两个元素的宽度一致
  elem(this1.contents ++ that1.contents)
}

```

另一种更加精简的定义是使用`Predef`中的`ensuring`方法：

```scala
private def widen(w: Int): Element = {
  if (w <= width) this
  else {
    val left = elem(' ', (w - width) / 2, height)
    val right = elem(' ', w - width - left.width, height)
    left beside this beside right
  }
} ensuring (w <= _.width) // _为widen方法的结果，一个Element
```

ensuring这个方法可以被用于任何结果类型（得益于隐式转换）。如果前提条件返回true，那么ensuring就正常返回，否则抛出AssertionError。

assert可以用JVM命令行参数`-ea`和`-da`来打开和关闭。

### 14.3 用Scala写测试

常用工具有ScalaTest、specs2和ScalaCheck。ScalaTest是最灵活的Scala测试框架：可以很容易地定制它来解决不同的问题。ScalaTest的灵活性意味着团队可以使用任何满足他们需要的测试风格，例如使用JUnit的团对，FunSuite风格是最舒适和熟悉的：

```scala
import org.scalatest.FunSuite
import Element.elem
class ElementSuite extends FunSuite {
  test("elem result should have passed width"){
    val ele = elem('x', 2, 3) //宽度为2，高度为3的元素
    assert(ele.width == 2)
  }
}
```

### 14.4 详实的失败报告

如果断言失败，失败报告就会包括文件名和该断言所在的行号，以及一条失败的详细信息。

```scala
scala> val width = 3
width: Int = 3

scala> assert(width == 2)
java.lang.AssertionError: assertion failed
  at scala.Predef$.assert(Predef.scala:208)
  ... 28 elided
```

为了在断言失败时提供给描述性的信息，ScalaTest会在编译时分析每次传入assert调用的表达式。如果想要更详细的信息，可以使用ScalaTest的DiagrammedAssertions，其错误消息会显示传入assert的表达式的一张图：

```scala
scala> assert(List(1, 2, 3).contains(4))
java.lang.AssertionError: assertion failed
  at scala.Predef$.assert(Predef.scala:208)
  ... 28 elided
```

`assert`仅仅提示左侧和右侧操作元不相等，或者在示意图显示出表达式的值。如果想要强调表达式实际和预期的差别，可以使用`assertResult`方法：

```scala
assertResult(2){  
  ele.width
}
```

括号内的`2`为预期值，如果为3，则会返回“excepted 2, but got 3”。

如果想要代码抛出某个预期的异常，使用`assertThrows`方法：

```scala
assertThrows[IlleagalArgumentException]{
  elem('x', 2, 3)
}
```

如果没有抛出异常，或者抛出了跟预期不同的异常，`assertThrows`将以`TestFailedException`异常终止。如果抛出了预期的异常，assertThrows将正常退出。如果想要进一步检视预期的异常，可以使用intercept而不是assertThrows。intercept方法跟assertThrows的运行机制相同，不过当异常被抛出时，intercept将抛出一下异常：

```scala
cal caught = intercept[ArithmeticException]{1 / 0}

assert(caught.getMessage = "/ by zero")
```

...................................

## 15 样例类和模式匹配

样例类(case class)和模式匹配(pattern matching)，为我们编写规则的，未封装的数据结构提供支持。对于表达树形的递归数据尤其有用。

样例类是Scala用来对对象进行模式匹配而不需要大量的样板代码的方式。如果要做模式匹配，只要在类上加case关键字即可。

### 15.1 一个简单的例子

编写一个操作算术表达式的类库，要先定义输入数据，将注意力集中在由变量，数以及一元和二元操作符组成的算术表达式上。用Scala的层次结构来表达：

```scala
abstract class Expr
case class Var(name: String) extends Expr
case class Number(num: Double) extends Expr
case class UnOp(operator: String, arg: Expr) extends Expr
case class BinOp(operator: String, left: Expr, right: Expr) extends Expr
```

一个抽象基类和四个子类。

#### 15.1.1 样例类

上面的4个子类前都有`case修饰符`，带有这种修饰符的称为`样例类(case class)`。用上这个修饰符，Scala编译器会对这种类添加一些语法上的便利。

首先，会`添加一个同名的工厂方法`，意味着可以用`Var('x')`来构造一个Var对象，而不用`new Var('x')`

```scala
scala> val v = Var("x")

v: Var = Var(x)
```

当需要嵌套时，工厂方法尤为有用，由于代码中不再满处都是new修饰符，一眼就能看明白表达式结构。

```scala
scala> val op = BinOp("+", Number(1), v)

op: BinOp = BinOp(+,Number(1.0),Var(x))
```

其次，第二个语法上的便利是`参数列表中的参数都隐式地获得了一个val前缀`，因此它们会被当做字段处理：

```scala
scala> v.name
res0: String = x

scala> op.left
res1: Expr = Number(1.0)
```

再次，编译器会帮助我们以“自然”的方式实现toString， hashCode和equals方法。

```scala
scala> println(op)
BinOp(+,Number(1.0),Var(x))

scala> op.right == Var("x")
res3: Boolean = true
```

最后，编译器还会`添加一个copy方法用于制作修改过的拷贝`。可以用于制作除了一两个属性不同之外，其余完全相同的该类的新实例，相当于带名字的参数和缺省参数，只修改其中的一个，不会影响其他参数。

```scala
scala> op.copy(operator = "-")
res4: BinOp = BinOp(-,Number(1.0),Var(x))
```

所以这些带来的大量的便利，代价却很小，编译器会自动添加某些方法。样例类最大的好处是他们支持模式匹配。

#### 15.1.2 模式匹配

加入要简化前面的定义，可以这样做：

```scala
UpOn("-", UpOn("-", e)) => e
BinOp("+", e, Number(0)) => e
BinOp("-", e, Number(0)) => e
```

用模式匹配的话，这些规则可以看成是一个Scala编写简化函数的核心：

```scala
def simplify(expr: Expr): Expr = expr match {
  case UnOp("-", UnOp("-", e)) => e
  case BinOp("+", e, Number(0)) => e
  case BinOp("-", e, Number(0)) => e
  case _ => expr
}

scala> simplify(UnOp("-", UnOp("-", Var("x"))))
res5: Expr = Var(x)
```

`simplify`的右侧由一个`match表达式`组成，match表达式对应Java的`switch`，不过`match关键字出现在选择器表达式后面`，写成：

```plain txt
选择器 match { 可选分支 }
```

而不是

```plain txt
switch ( 选择器 ) { 可选分支 }
```

模式匹配包含一系列以case关键字打头的可选分支，每一个可选分支都包含一个模式以及一个或多个表达式，如果模式匹配了，这些表达式就会被求值，箭头符号`=>`用于将模式和表达式分开。

#### 15.1.3 对比match和switch

match表达式可以被看做Java风格的switch的广义化。switch可以很自然的使用match表达式表达，其中每个模式都是长亮且最后一个模式是一个通配模式（代表switch中默认case）。

区别如下：

- Scala的match是一个表达式，也就是说它总能得到一个值；
- Scala的可选分支不会贯穿到下一个case；
- 如果一个模式都没有匹配上，会抛出名为MatchError的异常，意味着要保证所有的case都被覆盖到，哪怕添加一个什么都不做的缺省case。

```scala
expr match{
  case BinOp(op, left, right) => println(expr + " is a binary operation")
  case _ =>   //这个case是必要的，因为第一个case并不能覆盖全部情况
}

```

### 15.2 模式类型

模式的语法比较简单，比如Var(x)匹配任何表达式，把x绑定到变量的名称。

#### 15.2.1 通配符模式

通配符(`_`)可以匹配任何对象:

```scala
expr match {
  case BinOp(op, left, right) => println(expr + " is a binary operation")
  case _ => // handle the default case
}
```

通配符还可以用于忽略不关心的对象部分。例如，前面的例子实际上并不关心二元操作符的元素是什么，它只是检查它是否是一个二元操作符。

```scala
expr match{
  case BinOp(_, _, _) => println(expr + " is a binary operation")
  case _ => println("It's something else")
}
```

#### 15.2.2 常量模式

`常量模式只匹配自己本身，任何字面量都可以被用作常量`，并且`任何val和单例对象都可以被用作常量`。

```scala
def describe(x: Any) = x match{
  case 5 => "five"
  case true => "truth"
  case "hello" => "Hi!"
  case Nil => "the empty list"
  case _ => "something else"
}

scala> describe(5)
res0: String = five

scala> describe("hello")
res1: String = Hi!

scala> describe(12)
res2: String = something else
```

#### 15.2.3 变量模式

变量模式匹配任何对象，就像通配符一样。但与通配符不同的是，`Scala将变量绑定到对象`,然后可以使用此变量对对象进行进一步操作。

```scala
expr match{
  case 0 => "zero"
  case somethingElse => "not zero: " + somethingElse
}
```

使用常量还是变量？

常量可以有符号名，比如可以使用`Nil，E，Pi`：

```scala
import math.{Pi, E}

E match{
  case Pi => "strange math! Pi = " + Pi
  case _ => "OK"
}

res0: String = OK!
```

Scala怎么知道Pi是一个常量还是被定义的变量？Scala使用了一个简单的词法规则来消除歧义：一个以小写字母开头的简单名称被认为是一个模式变量；所有其他引用都被认为是常量。要查看区别，为pi创建一个小写别名并尝试使用它：

```scala
val pi = 3.1415926

E match{
  case pi => "strange math! Pi = " + Pi
  case _ => "OK"  //编译器会抛出一个告警， warning: unreachable code
}

res3: String = strange math! Pi = 2.718281828459045
```

在这里，编译器甚至不允许添加默认情况。由于pi是一个变量，所以它将匹配所有输入，因此不可能找到它后面的情况（unreachable）。

对于常量模式，也是可以使用小写：

- 首先，如果常量是一个对象的成员，像`pi`是一个变量模式，但是`this.pi`或者`obj.pi`就代表一个常量；

- 如果这不起作用(因为pi是一个局部变量)，可以`将变量名用反引号括起来`。例如，\`pi\` 将再次被解释为一个常数，而不是一个变量。

```scala
scala> E match {
  case `pi` => "strange math? Pi = " + pi
  case _ => "OK"  //不会抛出告警
}

res1: String = OK
```

#### 15.2.4 构造器模式

构造函数使模式匹配变得非常强大。一个构造函数模式看起来像`BinOp(“+”，e, Number(0))`。它由一个名称`BinOp`和括号内的一些模式组成:`“+”`、`e`和`number(0)`。假设名称指定了一个case类，这种模式意味着首先检查对象是否是命名的case类的成员，然后检查对象的构造函数参数是否与提供的其他模式匹配。

这些额外的模式意味着Scala模式支持深度匹配。不仅可以检查提供的顶级对象，还可以检查对象的其他模式。由于额外的模式本身可以是构造函数模式，所以可以使用它们来任意深入地检查对象。

```scala
expr match{
  case BinOp("+", e, Number(0)) => println("a deep match")
  case =>
}
```

上面所示的模式检查顶级对象是一个`BinOp`，它的第三个构造函数参数是一个数字，该数字的值字段是`0`。这个模式只有一行，但是检查了三层。

#### 15.2.5 序列模式

可以匹配序列类型，如`List`和`Array`，就像匹配case类一样。和之前的语法相同，但是要指定元素的数量：

```scala
expr match{
  case List(0, _, _) => println("found it")
  case _ =>
}
```

如果不指定长度来匹配任意数量的元素，使用`_*`：

```scala
expr match{
  case List(0, _*) => println("found it")
  case _ =>
}
```

#### 15.2.5 元组模式

```scala
def tupleDemo(expr: Any) = {
  expr match{
    case (a, b, c) => println("matched " + a + b + c)
    case _=>
  }
}

scala> tupleDemo("a ", 3, "-tuple")
matched a 3-tuple
```

#### 15.2.6 类型化模式(Typed pattern)

可以使用类型化模式方便地替换类型测试和类型转换。

```scala
def generalSize(x: Any) = x match{
  case s: String => s.length
  case m: Map[_, _] => m.size
  case _ => -1
}

scala> generalSize("helloworld")
res4: Int = 10

scala> generalSize(Map(1 -> 'a', 2 -> 'b'))
res5: Int = 2

scala> generalSize(math.Pi)
res6: Int = -1
```

测试表达式expr的类型：

```scala
expr.isInstanceOf[String]
```

转换类型：

```scala
expr.asInstanceOf[String]
```

#### 15.2.7 类型擦除

对上述例子中的`Map[_, _]`，指定键和值的类型`Map[Int, Int]`：

```scala
def isIntIntMap(x: Any) = x match {
  case m: Map[Int, Int] => m.size
  case _ => false
}

Warning:(39, 11) non-variable type argument Int in type pattern scala.collection.immutable.Map[Int,Int] (the underlying of Map[Int,Int]) is unchecked since it is eliminated by erasure
case m: Map[Int, Int] => m.size
```

Scala使用泛型的擦除模型，就像Java一样。这意味着在运行时不会维护关于类型参数的信息。在运行时,没有办法确定给定的Map是用两个Int类型参数创建的，还是其他类型创建的。只是判断某个值是不确定类型参数的Map。

```scala
scala> isIntIntMap(Map(1 -> 1))
res2: Boolean = true

scala> isIntIntMap(Map("abc"-> "abc"))
res3: Boolean = true
```

但是类型擦除规则唯一例外的是数组，数组的元素类型是跟数据一起保存的：

```scala
def isStringArray(x: Any) = x match{
  case a: Array[String] => "yes"
  case _ => "no"
}

scala> val as = Array("abc")
as: Array[String] = Array(abc)

scala> isStringArray(as)
res4: String = yes

scala> val ai = Array(1, 2, 3)
ai: Array[Int] = Array(1, 2, 3)

scala> isStringArray(ai)
res5: String = no
```

#### 15.2.8 变量绑定

变量名，一个@符号和模式本身，就得到一个变量绑定模式。意味着这个模式将跟平常一阳指型模式匹配，如果匹配成功，就将匹配的对象赋值给这个变量，就像简单的变量模式一样。

```scala
expr match{
  case UnOp("abs", e @ UnOp("abs", _)) => e
  case _ =>
}
```

包含了一个以`e为变量`，`UnOp("abs", _)`为模式的变量绑定模式。如果匹配成功，那么匹配了`UnOp("abs", _)`的部分就被复制给变量`e`。这个case的结果就是e，这是因为e跟expr的值相同，但是少了一次求绝对值的操作。

### 15.3 模式守卫

加入我们要公式化一个简化规则：`(e * 2)`替代`(e + e)`，在表示Expr数的语言中：

`BinOp("+", Var("x"), Var("x")) => BinOp("*", Var("x"), Number(2))`

可能写如下规则：

```scala
def simplifyAdd(e: Expr) = e match{
  case BinOp("+", x, x) => BinOp("*", x, Number(2))
  case _ => e
}

<pastie>:17: error: x is already defined as value x
  case BinOp("+", x, x) => BinOp("*", x, Number(2))
                     ^
```

编译器会抛出一个错误，因为Scala要求模式都是线性的，`同一个模式变量在模式中只能出现一次`。不过可以用一个模式守卫来重新定义这个匹配逻辑：

```scala
def simplifyAdd(e: Expr) = e match{
  case BinOp("+", x, y) if x == y => BinOp("*", x, Number(2))
  case _ => e
}

simplifyAdd: (e: Expr)Expr
```

模式守卫以if开头，模式守卫可以使任意的布尔表达式，通常引用到模式中的变量。`如果存在模式守卫，这个匹配仅在模式守卫求值得到true时才会成功`。因此，上面提到的首个case只能匹配那些两个操作员相等的二元操作。

其他模式守卫实例：

```scala
case n: if n > 0 => ...
case s: String if s(0) == 'a' => ...
```

### 15.4 模式重叠

```scala
def simplifyAll(expr: Expr): Expr = expr match{
  case UnOp("-", UnOp("-", e)) => simplifyAll(e)
  case BinOp("+", e, Number(0)) => simplifyAll(e)
  case BinOp("*", e, Number(1)) => simplifyAll(e)
  case UnOp(op, e) => UnOp(op, simplifyAll(e))
  case BinOp(op, l, r) => BinOp(op, simplifyAll(l), simplifyAll(r))
  case _ =>expr
}

scala> val x = Number(-123)
x: Number = Number(-123.0)

scala> val y = Var("abc")
y: Var = Var(abc)

scala> simplifyAll(UnOp("-", UnOp("-", y)))  //匹配第一个case
res11: Expr = Var(abc)

scala> simplifyAll(BinOp("+", x, Number(1))) //没有匹配到，执行最后一个case
res12: Expr = BinOp(+,Number(-123.0),Number(1.0))

scala> simplifyAll(BinOp("*", x, Number(1)))
res13: Expr = Number(-123.0)

scala> simplifyAll(UnOp("+", x))
res14: Expr = UnOp(+,Number(-123.0))

scala> simplifyAll(BinOp("*", x, x))
res16: Expr = BinOp(*,Number(-123.0),Number(-123.0))
```

### 15.5 密封类

当模式匹配无法考虑到所有可能的case时，可以添加一个缺省case来做到，但有些场合可能不够合适，因此可以寻求编译器的帮助，会自动检测出match表达式中缺失的模式组合。

将样例类的超类标记为密封(sealed)的。`密封类除了在同一个文件中定义的子类之外，不能添加新的子类`。

定义密封类，只需要在继承关系的顶部的那个`类名前加上sealed关键字`。

```scala
sealed abstract class Expr //声明为密封类
case class Var(name: String) extends Expr
case class Number(num: Double) extends Expr
case class UnOp(operator: String, arg: Expr) extends Expr
case class BinOp(operator: String, left: Expr, right: Expr) extends Expr

def descirbe(e: Expr): String = e match{
  case Number(_) => "a number"
  case Var(_) => "a variable"
}

Warning:(7, 33) match may not be exhaustive.It would fail on the following inputs:
  BinOp(_, _, _),
  UnOp(_, _)
def descirbe(e: Expr): String = e match{
```

这样的警告表示，代码存在产生`MatchError`异常的风险，因为某些可能的模式（UnOp， BinOp）并没有被处理。

但是有的时候我们定义case类，明确的使用场景只有Number和Var，很清楚不会发生`MatchError`错误。这时可以定义一个缺省case，来捕获所有的case：

```scala
def descirbe(e: Expr): String = e match{
  case Number(_) => "a number"
  case Var(_) => "a variable"
  case _ => throw new RuntimeException //加上之后不会再发生告警，但这个case永远不会执行
}
```

有时候这样做也不够理想，因为被迫添加了永远不会被执行的代码，而只是让编译器不再提示告警，一个更为简单的做法是给match选择器部分添加一个@unchecked注解：

```scala
def descirbe(e: Expr): String = (e: @unchecked) match{
  case Number(_) => "a number"
  case Var(_) => "a variable"
}
```

如果match表达式的选择器带上了这个注解，那么编译器对后续的模式分支的覆盖完整型检查就会被压制。

### 15.6 Option类型

在Scala中，一个名为Option的标准类型来表示可选值，这样的只可以有两种形式：`Some(x)`，其中x是实际的值，或者`None`对象，代表没有值。

Scala集合类的某些标准操作会返回可选值。比如Map中有一个get方法，当输入的键有对应的值时，会返回`Some(value)`，当传入的键没有对应的值时，会返回`None`：

```scala
scala> val capital = Map("France" -> "Pairs", "Japan" -> "Tokyo")
capital: scala.collection.immutable.Map[String,String] = Map(France -> Pairs, Japan -> Tokyo)

scala> capital get "France"
res17: Option[String] = Some(Pairs)

scala> capital get "America"
res18: Option[String] = None
```

### 15.7 到处都是模式

当定义一个val或者var时，都可以用模式而不是简单的标识符，例如：

```scala
scala> val myTuple = (123, "abc")
myTuple: (Int, String) = (123,abc)

scala> val (number, string) = myTuple
number: Int = 123
string: String = abc

scala> number
res19: Int = 123

scala> string
res20: String = abc
```

将对应位置的值赋值给变量。也可以用`模式析构`：

```scala
scala> val exp = new BinOp("*", Number(5), Number(1))
exp: BinOp = BinOp(*,Number(5.0),Number(1.0))

scala> val BinOp(op, left, right) = exp
op: String = *
left: Expr = Number(5.0)
right: Expr = Number(1.0)
```

#### 15.7.1 作为偏函数的序列

本质上一个case序列就是一个函数字面量，用花括号包起来的一系列case，可以用在任何出现函数字面量的地方。不像普通函数那样，只有一个入口和参数列表，case序列可以有多个入口，而该入口的参数列表用模式来指定。每个入口的逻辑主体是case右边的部分。

```scala
scala> val withDefault: Option[Int] => Int ={
     | case Some(x) => x
     | case None => 0
     | }
withDefault: Option[Int] => Int = $$Lambda$1636/374559056@1b08681b

scala> withDefault(Some(10))
res22: Int = 10

scala> withDefault(None)
res23: Int = 0
```

用它来定义Akka中actor的receive方法：

```scala
var sum = 0
def receive = {
  case Data(byte) => sum += byte
  case GetCheckSum(requester) =>
    val checksum = ~(sum & 0xFF) + 1
    requester ! checksum
}
```

通过case序列得到的就是一个偏函数。如果将这样一个函数应用到它不支持的值上，它会产生一个运行时异常，例如有一个但会整数列表整第二个元素的偏函数：

```scala
val second: List[Int] => Int = {
  case x :: y :: _ => y
}

warning: match may not be exhaustive.
It would fail on the following inputs: List(_), Nil
val second: List[Int] => Int = {
                               ^
second: List[Int] => Int = $$Lambda$1665/627852710@477b324e
```

编译器警告这个函数的匹配不全面，如果传入一个三元素列表，函数会执行成功，如果传入一个空列表，就会抛出一个错误：  

```scala
scala> second(List(1, 2, 3))
res24: Int = 2

scala> second(List())
scala.MatchError: List() (of class scala.collection.immutable.Nil$)
  at .$anonfun$second$1(<pastie>:12)
  at .$anonfun$second$1$adapted(<pastie>:12)
  ... 28 elided
```

如果想要检查某个偏函数是否对某个入参有定义，必须首先告诉编译器你知道你要处理的是偏函数。`List[Int] => Int`这个类型涵盖了所有从整数列表到整数的函数，不论这个函数是偏函数还是全函数。仅涵盖从整数列表到整数的偏函数的类型写作`PartialFunction[List[Int], Int]`，要重写这个函数：

```scala
val second: PartialFunction[List[Int], Int] = {
  case x :: y :: _ => y
}
```

偏函数定义了一个`isDefinedAt`方法，可以用来检查该函数时候支持某个特定的值定义。

```scala
scala> second.isDefinedAt(List(5, 6, 7))
res26: Boolean = true

scala> second.isDefinedAt(List())
res27: Boolean = false
```

`偏函数的典型用例是模式匹配函数字面量`。事实上，这样的表达式会被Scala编译器翻译成偏函数，这样的翻译发生了两次：`一次是实现真正的函数，另一次是测试这个函数事后对指定的值有定义`。

函数字面量`{case x :: y :: _ => y}`将翻译成如下的偏函数值：

```scala
new PartialFunction[List[Int], Int] {
  def apply(xs: List[Int}]) = xs match{
    case x :: y :: _ => y
  }
  def isDefinedAt(xs: List[Int]) = xs match {
    case x :: y :: _ => true
    case _ => false
  }
}
```

#### 15.7.2 for表达式中的模式

在for表达式中也可以使用模式：

```scala
for((country, city) <- capitals)
  println("The capital of " + country + " is " + city)

The capital of France is Pairs
The capital of Japan is Tokyo
```

这个称为对偶模式，很特别的地方是这个匹配永远不会失败。

### 15.8 一个复杂的例子

学习了模式的各种形式之后，编写一个表达式格式化类，以二维布局来显示一个算术表达式，诸如`x / (x + 1)`除法应该纵向打印：

```plain txt
   x
-------
 x + 1
```

另一个例子是，表达式`((a / (b * c) + 1/ n)/ 3)`：

```plain txt
  a      1
----- + ---
b * c    n
-----------
     3
```

/*-----------待补充----------------*/

## 16 使用列表

### 16.1 List字面量

```scala
val fruit = List("apples", "oranges", "pears")
val nums = List(1, 2, 3, 4)
val diag3 = List(
  List(1, 0, 0),
  List(0, 1, 0),
  List(0, 0, 1)
)
val empty = List()
```

列表跟数组很像，但有两个重要区别：

- 列表是不可变的，列表的元素不能通过赋值改变；
- 列表的结构是递归的，即链表，而数组是平的。

### 16.2 List类型

`同一列表的所有元素都必须是相同的元素`。元素类型为`T`的类表类型写作`List[T]`。

Scala的列表是协变(covariant)的，意思是对每一组类型`S`和`T`，如果`S`是`T`的子类型，那么`List[S]`就是`List[T]`的子类型。例如，`List[String]`是`List[Object]`的子类型。

注意，空列表的类型为List[Nothing]是所有List[T]的子类：

```scala
val xs: List[String] = List() //List()也是List[String]类型的
```

### 16.3 构建列表

所有的列表都构建自两个基础构建单元：`Nil`和`::`。`Nil表示空列表`，`中缀操作符::`表示在列表前面追加元素。

```scala
val fruit = "apples" :: ("oranges" :: ("pears" :: Nil))
val nums = 1 :: (2 :: (3 :: (4 :: Nil)))
val diags = (1 :: (0 :: (0 :: Nil))) ::
            (0 :: (1 :: (0 :: Nil))) ::
            (0 :: (0 :: (1 :: Nil))) :: Nil
val empty = Nil
```

### 16.4 列表的基本操作

对列表的所有操作都可以用下面的这三项来表示：
|操作|操作含义|
|--|--|
|`head`|返回列表的第一个元素|
|`tail`|返回列表中出第一个元素之外的所有元素|
|`isEmpty`|返回列表是否为空列表|
|||

`head`和`tail`只对`非空列表`有定义。

对列表进行插入排序：

```scala
def isort(xs: List[Int]): List[Int] = {
  if (xs.isEmpty) Nil
  else insert(xs.head, isort(xs.tail))
}

def insert(x: Int, xs: List[Int]): List[Int] = {
  if (xs.isEmpty || x <= xs.head) x :: xs
  else xs.head :: insert(x, xs, xs.tail)
}
```

### 16.5 列表模式

列表也可以使用模式匹配解开。列表模式可以逐一对应到列表表达式。

```scala
scala> val fruit = "apples" :: ("oranges" :: ("pears" :: Nil))
fruit: List[String] = List(apples, oranges, pears)

scala> val List(a, b, c) = fruit
a: String = apples
b: String = oranges
c: String = pears
```

如果实现不知道列表中元素的个数，可以使用如下方法来匹配：

```scala
scala> val a :: b :: c :: Nil = fruit
a: String = apples
b: String = oranges
c: String = pears
```

> `x :: xs` 表达式相当于`::(x, xs)`

实际上有一个名为`::`的类，它的名字叫做`scala.::`，就是用来构建非空列表的。因此`::`在Scala中出现了两次，以此作为scala包中一个类的名字，一次是在List类的方法名。`::`方法的作用是产生一个`scala.::`类的实例。

用模式匹配再次实现插入排序：

```scala
def isort(xs: List[Int]): List[Int] = xs match{
  case List() => List()
  case x :: xs1 => insert(x, isort(xs1))
}

def insert(x: Int, xs: List[Int]): List[Int] = xs match{
  case List() => List()
  case y :: ys => if (x <= y) x :: xs else y :: insert(x, ys)
}
```

### 16.6 List类的初阶方法

#### 16.6.1 拼接列表

跟`::`类似，拼接列表使用`:::`，不同于`::`，`:::`接收两个列表参数为操作元，并将左边列表中的每个元素逐个放入右边列表中：

```scala
scala> List(1, 2) ::: List(3, 4, 5)
res0: List[Int] = List(1, 2, 3, 4, 5)

scala> List() ::: List(1, 2, 3)
res1: List[Int] = List(1, 2, 3)

scala> List(1, 2, 3) ::: List(4)
res2: List[Int] = List(1, 2, 3, 4)
```

列表拼接也是右结合的，像这样一个表达式

```scala
xs ::: ys ::: zs
```

被解读为：

```scala
xs ::: (ys ::: zs)
```

#### 16.6.2 分治原则

拼接(`:::`)是作为List类的一个方法实现的。也可以通过列表进行模式匹配来实现“`手工`”拼接。`手工拼接`展示了用列表实现算法的常用方式。先假定一个拼方法签名`append`，接收两个待拼接的列表的作为参数，两个列表的元素类型必须一致：

```scala
def append[T](xs: List[T], ys: List[T]): List[T]
```

要设计这样一个`append`方法，需要回顾一下对于列表这样的递归数据结构的“`分而治之`”的程序设计原则。许多对列表的算法都首先会模式匹配将待输入的列表切分成更小的样例。这是设计原则中的“`分`”的部分。然后对每个样例构建对应的结果，如果结果是一个非空列表，那么这个列表的局部可以通过递归地调用同一个算法来构建，这个设计原则中的“`治`”的部分。

要实现append方法的第一个问题是：`匹配哪一个列表`，由分而治之中“`治`”的部分可知，需要同时包含两个输入列表的所有元素。由于列表是从右开始构建的，所以ys可以不用动，而xs需要先解开然后追加到ys的前面。这样一来，xs就是模式匹配的来源。

匹配列表最常见的模式是区分空列表和非空列表，以下是append方法的初步实现：

```scala
def append[T](xs: List[T], ys: List[T]): List[T] = xs match{
  case List() => ???
  case x :: xs1 => ???
}
```

第一部分，当来源是空列表时：

```scala
case List() => ys
```

第二部分，当输入列表xs由某个头`x`和尾`xs1`组成时的可选分支时，这个`case`中的结果也是一个非空列表。我们知道第一个元素是`x`，而余下的元素可以通过将第二个列表`ys`拼接在第一个列表的剩余部分即`xs1`之后

```scala
def append[T](xs: List[T], ys: List[T]): List[T] = xs match{
  case List() => ys
  case x :: xs1 => x :: append(xs1, ys) //体现了治的原则，调用的都是同一个算法append
}
```

#### 16.6.3 获取列表长度

```scala
scala> List(1, 2, 3).length
res3: Int = 3
```

相比于数组，列表获取长度length操作相对更加耗时，找到一个列表的末端，需要遍历整个列表，因此耗时与列表的元素数量成正比，这就是`xs.isEmpty`这样的测试换成`xs.length == 0`并不是什么好主意，并没有什么区别，且第二种更加耗时。

#### 16.6.4 访问列表的末端

```scala
scala> val abcde = List('a', 'b', 'c', 'd', 'e')
abcde: List[Char] = List(a, b, c, d, e)

scala> abcde.last //返回最后一个元素
res4: Char = e

scala> abcde.init //返回除最后一个元素之外的所有元素
res5: List[Char] = List(a, b, c, d)
```

与head和tail类似，init和last的定义是针对非空列表的：

```scala
scala> List().last
java.util.NoSuchElementException
  at scala.collection.LinearSeqOptimized.last
  ...

scala> List().last
java.util.NoSuchElementException
  at scala.collection.LinearSeqOptimized.last
  ...
```

#### 16.6.5 反转列表

```scala
scala> abcde.reverse
res8: List[Char] = List(e, d, c, b, a)

scala> abcde.reverse.reverse
res9: List[Char] = List(a, b, c, d, e)
```

调用reverse并不会对原有列表进行修改，而是创建新的列表。

```scala
scala> abcde.reverse.init == abcde.tail.reverse
res10: Boolean = true

scala> abcde.reverse.tail == abcde.init.reverse
res11: Boolean = true

scala> abcde.reverse.head == abcde.last
res12: Boolean = true

scala> abcde.reverse.last == abcde.head
res13: Boolean = true
```

#### 16.6.6 前缀和后缀

drop和take是对tail和init的一般化，它们返回的是列表任意长度的前缀和后缀，表达式`xs take n`，表示返回xs的前n个元素，如果n大于列表的长度，则返回整个列表的元素，表达式`xs drop n`表示返回xs除了前n个元素之外的所有元素，如果n大于列表长度，则返回空列表：

```scala
scala> abcde take 2
res14: List[Char] = List(a, b)

scala> abcde drop 3
res15: List[Char] = List(d, e)
```

splitAt操作 将列表从执行位置切开，返回这两个列表组成的对偶：

```scala
xs splitAt n 等于 (xs take n, xs drop n)

scala> abcde splitAt 3
res17: (List[Char], List[Char]) = (List(a, b, c),List(d, e))
```

#### 16.6.7 元素选择

apply支持从任意位置选取元素，相比于数组，这个操作在列表的并不是很常用：

```scala
scala> abcde apply 2
res18: Char = c

//apply是通过drop和head定义的：
xs apply n 等于(xs drop n).head

scala> abcde(2) //在scala中很少见，因为比较耗时
res19: Char = c

scala> abcde.indices
res20: scala.collection.immutable.Range = Range 0 until 5
```

#### 16.6.8 扁平化列表的列表

```scala
scala> List(List(1, 2), List(3), List(), List(4, 5)).flatten
res21: List[Int] = List(1, 2, 3, 4, 5)

scala> val fruit = List("apples", "oranges", "pears")
fruit: List[String] = List(apples, oranges, pears)

scala> fruit.map(_.toCharArray).flatten
res22: List[Char] = List(a, p, p, l, e, s, o, r, a, n, g, e, s, p, e, a, r, s)
```

flatten方法只能作用于那些所有元素都是列表的列表，针对单个列表使用，编译器会抛出一个错误：

```scala
scala> List(1, 2, 3).flatten
<console>:12: error: No implicit view available from Int => scala.collection.GenTraversableOnce[B].
       List(1, 2, 3).flatten
                     ^
```

#### 16.6.9 zip方法

zip方法接收两个列表，返回一个对偶组成的列表：

```scala
scala> abcde.indices zip abcde
res24: scala.collection.immutable.IndexedSeq[(Int, Char)] = Vector((0,a), (1,b), (2,c), (3,d), (4,e))

scala> abcde zip abcde
res25: List[(Char, Char)] = List((a,a), (b,b), (c,c), (d,d), (e,e))
```

如果列表的元素个数不同，则会抛弃多余的元素：

```scala
scala> abcde zip List(1, 2, 3) //列表abcde中的d，e会被丢弃
res26: List[(Char, Int)] = List((a,1), (b,2), (c,3))
```

一个有用的特例是将列表的元素和对应的下标zip起来：

```scala
scala> abcde.zipWithIndex
res27: List[(Char, Int)] = List((a,0), (b,1), (c,2), (d,3), (e,4))
```

也可以通过unzip还原列表：

```scala
scala> abcde.zipWithIndex.unzip
res28: (List[Char], List[Int]) = (List(a, b, c, d, e),List(0, 1, 2, 3, 4))
```

#### 16.6.10 显示列表

toString操作返回列表的标准字符串表现形式：

```scala
scala> abcde.toString
res29: String = List(a, b, c, d, e) //转为换String类型

scala> abcde
res30: List[Char] = List(a, b, c, d, e)
```

如果需要不同的表现形式，可以使用mkString方法：

```scala
xs mkString (pre, sep, post)
```

涉及4个操作元：要显示的列表`xs`、列表最前面的前缀字符串`pre`、在元素间显式的分隔符字符串`sep`和出现在最后的后缀字符串`post`。这个操作的结果是如下字符串：

`pre + xs(0) + sep + ... + sep + xs(xs.length - 1) + post`

mkString有两个重载的变种，第一个变种是只接受一个分割字符串：

`xs mkString sep` 等于 `xs mkString ("", sep, "")`

第二种是什么都不填：

`xs mkString` 等于 `xs mkString ""`

例子如下：

```scala
scala> abcde mkString("[", ",", "]")
res31: String = [a,b,c,d,e]

scala> abcde mkString ""
res33: String = abcde

scala> abcde.mkString
res34: String = abcde

scala> abcde mkString ("List(",", ",")")
res35: String = List(a, b, c, d, e)
```

mkString还有别的变种，如addString，这个方法侯建出来的字符串追加到一个StringBuilder对象，而不是作为结果返回：

```scala
scala> val buf = new StringBuilder
buf: StringBuilder =

scala> abcde addString (buf, "(", ";", ")")
res36: StringBuilder = (a;b;c;d;e)
```

#### 16.6.11 转换列表

数组与列表之间的转化可以使用List类的`toArray`和Array类的`toList`方法:

```scala
scala> val arr = abcde.toArray
arr: Array[Char] = Array(a, b, c, d, e)

scala> arr.toList
res37: List[Char] = List(a, b, c, d, e)
```

还有一个copyToArray方法可以将列表中的元素依次赋值到目标数组的指定位置：

`xs copyToArray (arr, start)`

要保证数组足够大，能容纳整个列表：

```scala
scala> val arr2 = new Array[Int](10)
arr2: Array[Int] = Array(0, 0, 0, 0, 0, 0, 0, 0, 0, 0)

scala> List(1, 2, 3) copyToArray (arr2, 3)

scala> arr2
res39: Array[Int] = Array(0, 0, 0, 1, 2, 3, 0, 0, 0, 0)
```

如果要通过迭代器去访问列表元素，可以iterator方法：

```scala
scala> val it = abcde.iterator
it: Iterator[Char] = <iterator>

scala> it.next
res40: Char = a

scala> it.next
res41: Char = b
```

### 16.7 List类的高阶方法

#### 16.7.1 对列表进行映射

map操作：

```scala
scala> List(1, 2, 3) map (_ +1)
res42: List[Int] = List(2, 3, 4)

scala> val words = List("the", "quick", "brown", "fox")
words: List[String] = List(the, quick, brown, fox)

scala> words map (_.length)
res43: List[Int] = List(3, 5, 5, 3)

scala> words map (_.toList.reverse.mkString)
res44: List[String] = List(eht, kciuq, nworb, xof)
```

flatMap和map操作类似，不过它要求右侧的操作元是一个返回元素列表的函数。它将这个函数应用到列表中每个元素上，然后将所有结果拼接起来返回：

```scala
scala> words map (_.toList)
res45: List[List[Char]] = List(List(t, h, e), List(q, u, i, c, k), List(b, r, o, w, n), List(f, o, x))

scala> words flatMap (_.toList)
res46: List[Char] = List(t, h, e, q, u, i, c, k, b, r, o, w, n, f, o, x)
```

map返回的是列表的列表，flatMap返回的是所有元素拼接起来的列表。

下面表达式构建一个满足`1 <= j < i < 5`的所有对偶`(i, j)`：

```scala
scala> List.range(1, 5)
res47: List[Int] = List(1, 2, 3, 4)

scala> List.range(1, 5) flatMap(i => List.range(1, i) map (j => (i, j)))
res49: List[(Int, Int)] = List((2,1), (3,1), (3,2), (4,1), (4,2), (4,3))
```

同样也可以使用for表达式来实现：

```scala
scala> for (i <- List.range(1, 5); j <- List.range(1, i)) yield (i, j)
res51: List[(Int, Int)] = List((2,1), (3,1), (3,2), (4,1), (4,2), (4,3))
```

foreach操作不同于map和flatMap，foreach要求有操作元是一个过程（结果类型为Unit）。只是简单的将过程应用到列表中的每个元素，整个操作本身结果类型也为Unit，并没有列表类型的结果被组装出来。

```scala
scala> var sum = 0
sum: Int = 0

scala> List(1, 2, 3, 4, 5).for
forall   foreach   formatted

scala> List(1, 2, 3, 4, 5).foreach(sum += _)

scala> sum
res53: Int = 15
```

#### 16.7.2 过滤列表

`"xs filter p"`：操作元分别是类型为`List[T]`的`xs`和类型为`T => Boolean`的前提条件函数`p`。这个操作将交出xs中所有p(x)为true的元素x:

```scala
scala> List(1, 2, 3, 4, 5) filter (_ % 2 == 0)
res54: List[Int] = List(2, 4)

scala> words filter (_.length == 3)
res55: List[String] = List(the, fox)
```

`partition`方法与filter很像，不过`返回的是一对列表`，其中一个所有前提条件为true的元素，另一个为条件为false的元素：

`xs partition p` 等于 `(xs filter p, xs filter (!p(_)))`

```scala
scala> List(1, 2, 3, 4, 5) partition (_ % 2 == 0)
res56: (List[Int], List[Int]) = (List(2, 4),List(1, 3, 5))
```

find方法返回查找到符合条件的第一个元素:

```scala
scala> List(1,2,3,4,5) find (_ % 2 == 0)
res57: Option[Int] = Some(2)
```

`takeWhile`和`dropWhile`操作符也将一个前提条件为右操作元。`xs takeWhile p`操作将返回列表xs中`连续满足p的最长前缀`，同理是`dropWhile`是`去除`。

```scala
scala> List(1,2,3,-4,5) takeWhile (_ > 0)
res58: List[Int] = List(1, 2, 3)

scala> List(1,2,3,-4,5) dropWhile (_ > 0)
res59: List[Int] = List(-4, 5)

scala> words dropWhile (_ startsWith "t")
res60: List[String] = List(quick, brown, fox)
```

`span`将`takeWhile`和`dropWhile`两个操作合二为一，就像`splitAt`将`take`和`drop`操作合二为一一样，会返回一堆列表：

`xs span p`  等于  `(xs takeWhile p, xs drop While p)`

```scala
scala> List(1,2,3,-4,5) span (_ > 0)
res61: (List[Int], List[Int]) = (List(1, 2, 3),List(-4, 5))
```

#### 16.7.3 前提条件检查

`xs forall p`：接收`一个列表xs`和`一个前提条件p`作为入参。如果列表中所有的元素都满足条件p就返回true。

`xs exists p`：只要有一个元素满足条件p就返回true。

```scala
scala> List(1,2,3,-4,5) forall (_ > 0)
res62: Boolean = false

scala> List(1,2,3,-4,5) exists (_ > 0)
res63: Boolean = true
```

#### 16.7.4 折叠列表

对列表的另一种常见操作是用某种操作符合并元素：

`sum(List(1, 2, 3))`  等于  `0 + a + b + c`

这是折叠操作的一个特例：

```scala
scala> def sum(xs: List[Int]): Int = (0 /: xs)(_ + _)
sum: (xs: List[Int])Int
```

同理：

`product(List(a, b, c))`  等于  `1 * a * b * c`

```scala
scala> def product(xs: List[Int]): Int = (0 /: xs)(_ * _)
product: (xs: List[Int])Int
```

`左折叠`操作：`(z /: xs)(op)`，设计三个对象，起始值`z`、列表`xs`和二元操作`op`。折叠的结果以z为前缀，对列表元素依次连续应用op。

`(z /: List(a, b, c))(op)`  等于  `op(op(op(z, a), b), c)`

```scala
scala> ("" /: words)(_ + " " + _)
res65: String = " the quick brown fox" //这里开始多了一个空格
```

要去除这个空格，像下面改写：

```scala
scala> (words.head /: words.tail) (_ + " " + _)
res66: String = the quick brown fox
```

`/:`操作符产生一个往左靠的数，同理`:/`(右折叠)操作符产生一个向右靠的操作树。

`(List(a, b, c) :/ z)(op)`  等于  `op(a, op(b, op(c, z)))`

#### 16.7.5 反转列表

reverse用于反转列表的开销是列表长度的平方级，现在来看一个reverse的不同实现，运行开销是线性的，原理是基于下面的机制来做左折叠：

```scala
def reverseLeft[T](xs: List[T]) = (startvalue /: xs)(operation)
```

剩下的需要补全的就是startvalue和operation部分。事实上，可以用更见的例子来推导，为了推导出startvalue正确的取值，可以用最简单的列表List()开始：

`List()` 等同于 `reverseLeft(List())` 等同于 `(起始值 /: List())(op)`

因此startvalue必须是List()，要推导出第二个操作元，可以拿仅次于List()的最小列表作为样例，可以做一下演算：

`List()` 等同于 `reverseLeft(List())` 等同于 `(List() /: List())(操作)` 等同于 `op(List(), x)`。

因此，`operation(List(), x)`等于`List()`，而`List(x)`也可以写作`x :: List()`。这样我们就发现可以基于`::`操作符把两个操作元翻转一下来得到`operation`，于是得到reverseLeft如下实现：

```scala
def reverseLeft[T](xs: List[T]) = {
  (List[T]() /: xs){
    (ys, y) => y :: ys
  }
}
```

同样，为了让类型推断程序正常工作，这里的类型注解`List[T]()`是必须的。如果我们分析reverseLeft的实现复杂度，会发现它执行这个常量时间操作（即，“snoc”）n次，因此reverseLeft的时间复杂度是线性的。

#### 16.7.6 列表排序

`xs sortWith before`这个操作对列表xs中的元素进行排序，其中xs是列表，而before是一个用来比较两个元素的函数：

```scala
scala> List(1, -3, 4, 2, 6) sortWith (_ < _)
res67: List[Int] = List(-3, 1, 2, 4, 6)

scala> words sortWith (_.length > _.length)
res68: List[String] = List(quick, brown, the, fox)
```

### 16.8 List对象的方法

以上定义的所有操作都是List类的方法，因此我们其实是在每个具体的列表对象上调用它们。还有一些定义在全局可访问对象`scala.List`上的，这是`List类的伴生对象`。某些操作是用于创建列表的工厂方法，另一些是对特定形状的列表进行操作。

#### 16.8.1 从元素创建列表

```scala
scala> List.apply(1, 2, 3)
res69: List[Int] = List(1, 2, 3)
```

#### 16.8.2 创建数值区间

```scala
scala> List.range(1, 5)
res70: List[Int] = List(1, 2, 3, 4)

scala> List.range(1, 9, 2)  //(start, end, step)
res71: List[Int] = List(1, 3, 5, 7)

scala> List.range(9, 1, -1) //(start, end, step)
res72: List[Int] = List(9, 8, 7, 6, 5, 4, 3, 2)
```

#### 16.8.3 创建相同元素的列表

```scala
scala> List.fill(5)('a')
res73: List[Char] = List(a, a, a, a, a)

scala> List.fill(3)("hello")
res74: List[String] = List(hello, hello, hello)
```

如果给fill的参数多于1个，那么它会自动创建多维列表。也就是说，它将创建出列表的列表，列表的列表的列表。多出来的这些参数要放在第一个参数列表中。

```scala
scala> List.fill(2, 3)('b')
res75: List[List[Char]] = List(List(b, b, b), List(b, b, b))
```

#### 16.8.4 表格化一个函数

`tabulate`方法创建的是一个根据给定的函数计算的元素的列表，其入参和`List.fill`一样：第一个参数列表给出要创建列表的维度，而第二个参数列表描述列表的元素，唯一的区别是，元素值不再是固定的，而是从函数计算得来：

```scala
scala> val squares = List.tabulate(5)(n => n * n)
squares: List[Int] = List(0, 1, 4, 9, 16)

scala> val multiplication = List.tabulate(5, 5)(_ * _)
multiplication: List[List[Int]] =
  List(List(0, 0, 0, 0, 0),
       List(0, 1, 2, 3, 4),
       List(0, 2, 4, 6, 8),
       List(0, 3, 6, 9, 12),
       List(0, 4, 8, 12, 16))
```

#### 16.8.5 拼接列表

```scala
scala> List.concat(List('a', 'b'), List('c', 'd'))
res76: List[Char] = List(a, b, c, d)

scala> List.concat(List(), List('a'), List('b'))
res77: List[Char] = List(a, b)

scala> List.concat()
res78: List[Nothing] = List()
```

### 16.9 同时处理多个列表

zipped可以同时处理多个列表，其中一个通用的操作是map，对两个zip在一起的列表调用map的效果是对元素一组一组地做映射，而不是单个元素。

```scala
scala> (List(10, 20), List(3, 4, 5)).zipped.map(_ * _)
res79: List[Int] = List(30, 80)
```

同理，exists和forall也有zip起来的版本，但操作的是多个列表：

```scala
scala> (List("abc", "de"), List(3, 2)).zipped.forall(_.length == _)
res80: Boolean = true

scala> (List("abc", "de"), List(3, 2)).zipped.exists(_.length != _)
res81: Boolean = false
```

## 17 使用其他集合类

### 17.1 序列

序列类型允许处理按顺序排列的数据组。因为元素是有序的，你可以按下标访问第1个元素，第1个元素，103号元素，以此类推。

#### 17.1.1 列表

由前几章介绍可知，List是不可变链表，支持在头部快速添加和删除元素，但是并不支持按照下标快速访问其中的元素，因为这需要随下标线性的遍历列表。

这些特征的组合听起来可能有点奇怪，但它们正好适合许多算法。初始元素的快速添加和删除意味着模式匹配工作良好。

#### 17.1.2 数组

数组允许保存一个元素序列，并使用从零开始的索引有效地访问任意位置的元素，以获取或更新元素。

```scala
scala> val fiveInts = new Array[Int](5)
fiveInts: Array[Int] = Array(0, 0, 0, 0, 0)

scala> val fiveToOne = new Array[Int](5)
fiveToOne: Array[Int] = Array(0, 0, 0, 0, 0)

scala> fiveToOne(4) = 1

scala> fiveToOne
res3: Array[Int] = Array(0, 0, 0, 0, 1)
```

#### 17.1.3 列表缓冲

如果要在列表的尾部添加一个元素，首先需要将列表使用reverse方法，将列表反转过来，在头部添加元素后，再将列表反转。

另一种不使用reverse的方法是调用`ListBuffer`方法。ListBuffer是一个可变对象，位于`scala.collection.mutable`包中，在对列表添加元素时更加高效，提供常量时间的尾部添加(`append`)和头部添加(`prepend`)元素操作。`append`元素使用`+=`操作符，`prepend`使用`+=:`操作，操作完成后调用`toList`方法，将`ListBuffer`转换为`List`。

```scala
scala> import scala.collection.mutable.ListBuffer
import scala.collection.mutable.ListBuffer

scala> val buf = new ListBuffer[Int]
buf: scala.collection.mutable.ListBuffer[Int] = ListBuffer()

scala> buf += 1
res4: buf.type = ListBuffer(1)

scala> buf += 2
res5: buf.type = ListBuffer(1, 2)

scala> 3 +=: buf
res7: buf.type = ListBuffer(3, 1, 2)
```

使用ListBuffer的另一个原因是，防止潜在的栈溢出风险，后面将在22.2章详细介绍。

#### 17.1.4 数组缓冲

`ArrayBuffer`类似于数组，此外还可以在序列的开头和结尾添加和删除元素。所有数组操作都是可用的，不过由于实现中有一层包装，它们的速度会慢一些。新的添加和删除操作的平均时间是常数，但偶尔需要线性时间，因为实现需要分配一个新的数组来保存缓冲区的内容。`ArrayBuffer`位于`scala.collection.mutable包`中。

```scala
scala> import scala.collection.mutable.ArrayBuffer
import scala.collection.mutable.ArrayBuffer

scala> val buf = new ArrayBuffer[Int]() //可以不指定长度
buf: scala.collection.mutable.ArrayBuffer[Int] = ArrayBuffer()

scala> buf += 12
res8: buf.type = ArrayBuffer(12)

scala> buf += 15
res9: buf.type = ArrayBuffer(12, 15)

scala> buf
res10: scala.collection.mutable.ArrayBuffer[Int] = ArrayBuffer(12, 15)

scala> buf.length
res11: Int = 2

scala> buf(0)
res12: Int = 12
```

#### 17.1.2 字符串（通过StringOps）

另一个需要注意的序列是`StringOps`，它实现了许多序列方法。因为`Predef`具有从`字符串到字符串的隐式转换`，所以可以将任何字符串视为序列。

```scala
scala> def hasUpperCase(s: String) = s.exists(_.isUpper)
hasUpperCase: (s: String)Boolean

scala> hasUpperCase("Scala.Programming")
res14: Boolean = true

scala> hasUpperCase("scala.programming")
res15: Boolean = false
```

String类本身没有exists方法，编译器会将String隐式转换为StringOps，StringOps有exists方法，exists会将string视为字符序列( sequence of characters)。

### 17.2 集和映射

前面已经介绍了Set和Map的用法，默认情况下，使用的都是不可变对象，如果想使用可变对象，需要显式的从scala.collection.mutable包中导入。另外这些对象都是Predef中定义，会自动导入到每一个Scala源文件中，下面是Predef的部分定义：

```scala
object Predef {
  type Map[A, +B] = collection.immutable.Map[A, B] //type用于定义等号左边是等号右边的别名
  type Set[A] = collection.immutable.Set(A)
  val Map = collection.immutable.Map
  val Set = collection.immutable.Set
  //...
}
```

#### 17.2.1 使用集

集合的关键特征是，它们将确保在任何时候最多只包含一个相同的元素。

```scala
scala> val text = "See Spot run. Run, Spot. Run!"
text: String = See Spot run. Run, Spot. Run!

scala> val wordsArray = text.split("[ !,.]+")
wordsArray: Array[String] = Array(See, Spot, run, Run, Spot, Run)

scala> import scala.collection.mutable.Set
import scala.collection.mutable.Set

scala> val words = scala.collection.mutable.Set.empty[String]
words: scala.collection.mutable.Set[String] = Set()

scala> for (word <- wordsArray) words += word.toLowerCase

scala> words
res5: scala.collection.mutable.Set[String] = Set(see, run, spot)
```

Set常规操作：

|操作|含义|
|--|--|
|val nums = Set(1, 2, 3)|创建一个不可变集合，nums.toString返回Set(1, 2, 3)|
|nums + 5|添加元素5，返回Set(1, 2, 3, 5)|
|nums -- 3|移除元素3，返回Set(1, 2)|
|nums ++ List(5, 6)|添加多个元素，返回Set(1, 2, 3, 5, 6)|
|nums -- List(1, 2)|移除多个元素，返回Set(3)|
|nums & Set(1, 3, 5, 7)|集合交集，返回Set(1, 3)|
|nums.size|返回集的大小，返回3|
|nums.contains(3)|是否包含3，返回true|
|import scala.collection.mutable|导入可变集|
|val words = mutable.Set.empty[String]|声明一个可变的空集合|
|words += "the"|添加元素the，返回Set(the)|
|words -= "the"|移除元素the，返回Set()|
|words ++= List("do", "re", "mi")|添加多个元素，返回Set(do, re, mi)|
|words --= List("do", "re")|移除do, re，返回Set(mi)|
|words.clear|移除所有元素，返回Set()|

#### 17.2.2 使用映射

map使用起来类似于数组，只不过map的下标不是从0开始，可以使用任何类型的键：

```scala
scala> import scala.collection.mutable.Map
import scala.collection.mutable.Map

scala> val map = scala.collection.mutable.Map.empty[String, Int]
map: scala.collection.mutable.Map[String,Int] = Map()

scala> map("hello") = 1

scala> map("world") = 2

scala> map
res2: scala.collection.mutable.Map[String,Int] = Map(world -> 2, hello -> 1)

scala> map("hello")
res3: Int = 1
```

将所有这些放在一起，这里有一个方法可以计算每个单词在字符串中出现的次数:

```scala
def countWords(text: String) = {
  val counts = scala.collection.mutable.Map.empty[String, Int]
  for (rawWord <- text.split("[ !,.]+")){
    val word = rawWord.toLowerCase
    val oldCount = if (counts.contains(word)) counts(word) else 0
    counts += (word -> (oldCount + 1))
  }
  counts
}

scala> countWords("See Spot run! Run, Spot. Run!")
res0: scala.collection.mutable.Map[String,Int] = Map(spot -> 2, see -> 1, run -> 3)
```

Map的常用方法：

|操作|含义|
|--|--|
|val nums = Map("i" -> 1, "ii" -> 2)|创建一个immutable Map|
|nums +("vi" -> 6)|添加一个条目|
|nums - "ii"|移除一个条目|
|nums ++ List("iii" -> 3, "v" -> 5)|添加多个条目|
|nums -- List("i", "ii")|移除两个条目|
|nums.size|返回nums的大小|
|nums.contains("ii")|是否包含指定键|
|nums("ii")|返回键对应的值|
|nums.keys|返回所有的键|
|nums.keySet|返回所有键组成的集合|
|nums.values|返回所有的值|
|nums.isEmpty|判断nums是否为空|
|import scala.collection.mutable|导入可变集合|
|val words = mutable.Map.empty[String, Int]|建立一个空Map|
|words += ("one" -> 1)|添加条目|
|words -= "one"|移除条目|
|words ++= List("one" -> 1,"two" -> 2, "three" -> 3)|添加多个条目|
|words --= List("one", "two")|移除多个条目|

#### 17.2.3 默认的集和映射

对于大多数情况，Set()、scala.collections.mutable.map()等工厂方法提供的可变和不可变集和映射的实现可能就足够了。这些工厂提供的实现使用快速查找算法，通常涉及到散列表，因此它们可以快速确定对象是否在集合中。

例如，scala.collections.mutable.set()工厂方法，使用的是内部hash表，返回一个scala.collection.mutable.HashSet，同样地，scala.collections.mutable.collection.mutable.Map()工厂将返回一个scala.collection.mutable.HashMap。

不可变Set和Map就要复杂一些，对于不可变Set：

|||
|--|--|
|元素个数|实现方法|
|0|scala.collection.immutable.EmptySet|
|1|scala.collection.immutable.Set1|
|2|scala.collection.immutable.Set2|
|3|scala.collection.immutable.Set3|
|4|scala.collection.immutable.Set4|
|5 或者更多|scala.collection.immutable.HashSet|

对于不可变Map：

|||
|--|--|
|元素个数|实现方法|
|0|scala.collection.immutable.EmptyMap|
|1|scala.collection.immutable.Map1|
|2|scala.collection.immutable.Map2|
|3|scala.collection.immutable.Map3|
|4|scala.collection.immutable.Map4|
|5 或者更多|scala.collection.immutable.HashMap|

#### 17.2.4 有序集和映射

有时，可能需要一个集合或映射，其迭代器以特定顺序返回元素。为此，Scala集合库提供了SortedSet和SortedMap两种特质。

这些特征是由TreeSet和TreeMap类实现，它们使用红黑树来保持元素(在TreeSet的情况下)或键(在TreeMap的情况下)的顺序。顺序由Ordered特质决定，集合的元素类型或映射的键类型必须混合或隐式转换为该特质。这些类只有不可变变量。

```scala
scala> import scala.collection.immutable.TreeSet
import scala.collection.immutable.TreeSet

scala> val ts = TreeSet(9, 3, 1, 8, 0, 2, 7, 4, 6, 5)
ts: scala.collection.immutable.TreeSet[Int] = TreeSet(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)

scala> val cs = TreeSet('f', 'u', 'n')
cs: scala.collection.immutable.TreeSet[Char] = TreeSet(f, n, u)

scala> import scala.collection.immutable.TreeSet
import scala.collection.immutable.TreeSet

scala> var tm = TreeSet(3 -> 'x', 1 -> 'x', 4 -> 'x')
tm: scala.collection.immutable.TreeSet[(Int, Char)] = TreeSet((1,x), (3,x), (4,x))

scala> tm += (2 -> 'x')

scala> tm
res12: scala.collection.immutable.TreeSet[(Int, Char)] = TreeSet((1,x), (2,x), (3,x), (4,x))
```

### 17.3 在可变和不可变集合类之间选择

编程中，当抉择不定的时候，优先选择使用不可变集合类，因为这样更容易推断。特别是，如果发现自己担心在适当的位置创建可变集合的副本，或者经常考虑谁“拥有”或“包含”可变集合，那么请考虑将某些集合切换为其不可变对象。

除了可能更容易推理之外，如果集合中存储的元素数量很少，那么不可变集合通常可以比可变集合存储得更紧凑。例如，一个空的、默认表示为HashMap的可变映射大约占用80个字节，每增加一个条目，就增加16个字节。一个空的不可变Map，在所有引用之间共享的单一对象，因此对它的引用实际上只需要一个指针字段。

此外，Scala集合库目前在单个对象中存储最多4个条目的不可变映射和集合，这通常占用16到40个字节，具体取决于集合中存储的条目数量。所以对于小型Map和Set，不可变版本比可变版本更紧凑。考虑到许多集合都很小，将它们切换���不可变可以带来重要的空间节省和性能优势。

为了更容易从不可变集合切换到可变集合，反之亦然，Scala提供了一些语法糖。尽管不可变Set和Map不支持真正的`+=`方法，但Scala提供了一种有用的替代解释`+=.`。每当编写`a += b`，而a不支持名为`+=`的方法时，Scala将尝试将其解释为`a = a + b`。

```scala
scala> val people = Set("Nancy", "Jane")
people: scala.collection.immutable.Set[String] = Set(Nancy, Jane)

scala>  people += "Bob"
<console>:16: error: value += is not a member of scala.collection.immutable.Set[String]
  Expression does not convert to assignment because receiver is not assignable.
        people += "Bob"
               ^

scala> var people = Set("Nancy", "Jane")
people: scala.collection.immutable.Set[String] = Set(Nancy, Jane)

scala>  people += "Bob"

scala> people
res15: scala.collection.immutable.Set[String] = Set(Nancy, Jane, Bob)

scala> people -= "Jane"

scala>  people ++= List("Tom", "Harry")

scala>  people
res18: scala.collection.immutable.Set[String] = Set(Nancy, Bob, Tom, Harry)
```

### 17.4 初始化集合

如前所述，创建和初始化集合的最常见方法是将初始元素传递给所选集合的同伴对象上的工厂方法。只需将元素放在伴随对象名称后的括号中，Scala编译器就会将其转换为对伴生对象的apply方法的调用

```scala
scala> List(1, 2, 3)
res19: List[Int] = List(1, 2, 3)

scala> Set(1, 2, 3)
res20: scala.collection.immutable.Set[Int] = Set(1, 2, 3)

scala> import scala.collection.mutable
import scala.collection.mutable

scala> mutable.Map("hi" -> 1, "there" -> 2)
res21: scala.collection.mutable.Map[String,Int] = Map(hi -> 1, there -> 2)

scala> Array(1.0, 2.0, 3.0)
res22: Array[Double] = Array(1.0, 2.0, 3.0)

scala> val stuff = mutable.Set(42)
stuff: scala.collection.mutable.Set[Int] = Set(42)

scala> val stuff = mutable.Set[Any](42)
stuff: scala.collection.mutable.Set[Any] = Set(42)

scala> stuff += "abcd"
res23: stuff.type = Set(abcd, 42)

scala> val colors = List("blue", "red", "yellow", "green")
colors: List[String] = List(blue, red, yellow, green)

scala> import scala.collection.immutable.TreeSet
import scala.collection.immutable.TreeSet

scala> val treeSet = TreeSet(colors)
<console>:18: error: No implicit Ordering defined for List[String].
       val treeSet = TreeSet(colors) //不能将列表传给TreeSet
                            ^

scala> val treeSet = TreeSet[String]() ++ colors //先创建TreeSet，然后使用++操作符，将列表元素添加进TreeSet中
treeSet: scala.collection.immutable.TreeSet[String] = TreeSet(blue, green, red, yellow)
```

#### 17.4.1 转换成数组或列表

```scala
scala> treeSet.toList
res24: List[String] = List(blue, green, red, yellow)

scala> treeSet.toArray
res25: Array[String] = Array(blue, green, red, yellow)
```

但是转换到列表或数组通常需要复制集合的所有元素，因此对于大型集合可能很慢。但是，由于现有的API，有时需要这样做。由于许多集合无论如何都只有几个元素，在这种情况下，速度损失很小。

#### 17.4.2 在可变与不可变集及映射间转换

有时需要将可变集转换或映射为不可变集，反之亦然。要实现这一点，可以使用上面所示的技术，用列表的元素初始化TreeSet。使用empty方法创建新类型的集合，然后使用++或++=(根据目标集合类型选择)添加新元素。下面是将不可变TreeSet从上一个示例转换为可变集，然后再转换回不可变集的方法：

```scala
scala> treeSet
res26: scala.collection.immutable.TreeSet[String] = TreeSet(blue, green, red, yellow)

scala> val mutaSet = mutable.Set.empty ++= treeSet
mutaSet: scala.collection.mutable.Set[String] = Set(red, blue, green, yellow)

scala> val immutaSet = Set.empty ++ mutaSet
immutaSet: scala.collection.immutable.Set[String] = Set(red, blue, green, yellow)
```

转换可变Map和不可变Map使用类似的方法：

```scala
scala> val muta = mutable.Map("i" -> 1, "ii" -> 2)
muta: scala.collection.mutable.Map[String,Int] = Map(ii -> 2, i -> 1)

scala>  val immu = Map.empty ++ muta
immu: scala.collection.immutable.Map[String,Int] = Map(ii -> 2, i -> 1)
```

### 17.5 元组

与Array和List不同的是，元组Tuple中可以拥有不同类型的元素：

```scala
(1, "hello", Console)
```

元组省去了定义简单数据量大的类的繁琐工作。元组节省了为类选择名称、选择定义类的作用域以及为类成员选择名称的工作。

因为元组可以组合不同类型的对象，所以`元组不能被遍历`。如果想要精确地分组一个整数和一个字符串，那么需要一个元组，而不是列表或数组。`元组的一个常见应用是从一个方法返回多个值`。例如，下面的方法可以查找集合中最长的单词，并返回其索引：

```scala
def longestWord(words: Array[String]) = {
  var word = words(0)
  var idx = 0
  for (i <- 1 until words.length){
    if (words(i).length > word.length)
      word = words(i)
      idx = i
  }
  (word, idx)
}

scala> val longest = longestWord("the quick brown fox".split(" "))
longest: (String, Int) = (quick,3)
```

获取元组中的元素，使用_1取出第一个元素，_2取出第二个元素，等等：

```scala
scala> longest._1
res27: String = quick

scala> longest._2
res28: Int = 3
```

此外，可以将元组的每个元素分配给它自己定义的变量：

```scala
scala> val (word, idx) = longest
word: String = quick
idx: Int = 3
```

## 18 可变对象

在前面的章节中，主要介绍了函数式不可变的对象上。这是因为没有任何可变状态的对象这个理念值得人们更多的关注。不过，在Scala中定义带有可变状态的对象也完全可行。当想要对真实世界中那些随着时间变换的对象进行建模时，自然而然就会想到这样的可变对象。

### 18.1 什么样的对象是可变的

对于不可变对象，比如字符列表：

```scala
val cs = List('a', 'b', 'c', 'd')
```

对`cs`调用`cs.head`总是返回`'a'`，无论在`cs.head`调用之前发生了任意数量，也不会改变。

另一方面，对于可变对象而言，方法调用或字段访问的结果取决于之前这个对象被执行了哪些操作，可变对象的不错例子是银行账户：

```scala
class BankAccount{
  private var bal: Int = 0
  def balance: Int = bal

  def deposit(amount: Int) = {
    require(amount > 0)
    bal += amount
  }

  def withdraw(amount: Int) = {
    if (amount > bal) false
    else {
      bal -= amount
      true
    }
  }
}

scala> val account = new BankAccount
account: BankAccount = BankAccount@295bf2a

scala> account deposit 100

scala> account withdraw 80
res1: Boolean = true

scala> account withdraw 80
res2: Boolean = false
```

一个类可能并没有定义或继承任何var变量，但它依然是可变的，因为它将方法调用转发到其他带有可变状态的对象上。反过来也是有可能的：一个类可能包含了var但却是纯函数式的。例如，某个类可能为了优化性能将开销巨大的操作结果缓存到字段中，如下，一个没有经过优化的Keyed类，其computeKey操作开销很大：

```scala
class Keyed{
  def computeKey: Int = ...//这需要些时间
}
```

假设computeKey既不读也不写任何var，可以通过添加缓存来让Keyed变得更加高效：

```scala
class MemoKeyed extends Keyed {
  private var keyCache: Option[Int] = None
  override def computeKey: Int = {
    if (!keyCache.isDefined) keyCache = Some(super.computeKey)
    keyCache.get
  }
}
```

使用MemoKeyed而不是Keyed可以提速，因为computeKey操作第二次被请求时，可以直接返回保存在KeyCache字段中的值，而不是再次运行computeKey。不过除了性能提升，Keyed和MemoKeyed类的行为完全一致。因此，如果说Keyed是纯函数式的，那么MemoKeyed同样也是，尽管它有一个可被重新复制的变量。

## 18.2 可被中心赋值的变量和属性

可以对可被重新赋值的变量做两种操作：

- 获取它的值
- 将它设为新值

在像`JavaBean`的类库中，这些操作通常被包装秤单独的`getter`和`setter`方法，需要显式的定义这些方法。

但是，在Scala中，每个`非私有的var成员`都隐式的定义了getter和setter方法，不过这些方法和Java的有些不同，`var x`的`getter方法`只是简单的命名为`“x”`，而他的`setter方法`命名为`"x_="`。

例如：

```scala
var hour = 12
```

除了定义字段以外，还生成了`名为“hour”的getter`和`名为“hour_=”的setter方法`。其中的字段总是被标记为`private[this]`，意味着这个字段只能总包含它的对象中访问。

而getter和setter则拥有跟原来的var相同的可见性。如果原先var定义为公有的，那么它的setter和getter也是公有的，如果是protected，那么setter和getter也是protected。

举例如下：

```scala
class Time{
  var hour = 12
  var minute = 0
}
```

与下面的定义完全一致：

```scala

​```scala
class Time{
  private[this] var h = 12
  private[this] var m = 0

  def hour: Int = h
  def hour_=(x: Int)={h = x}
  
  def minute: Int = m
  def minute_=(x: Int)={m = x}
}
```

这样的展开定义我们可以按照自己的意愿来解释变量访问和赋值。

```scala
class Time {
  private[this] var h = 12
  private[this] var m = 0
  def hour: Int = h
  def hour_= (x: Int) = {
    require(0 <= x && x < 24)
    h = x
    }
  def minute = m
  def minute_= (x: Int) = {
    require(0 <= x && x < 60)
    m = x
    }
}
```

某些语言对于这些类似变量的值有特殊的语法表示，他们不同于普通常量的地方在于getter和setter可以被重新定义。

有时，定义不跟任何字段关联的getter和setter也是有用的。

```scala
class Thermometer{
  var celsius: Float = _ //下划线为缺省0值，具体取决于类型，Int为0，布尔值为false，不能简单写作var celsius: Float，只是定义一个抽象变量，而不是一个未赋值的变量。

  def fahrenheit = celsius * 9 / 5 + 32 //getter方法
  def fahrenheit_= (f: Float)={
    celsius = (f - 32) * 5 / 9 //setter方法
  }
  override def toString = fahrenheit + "F/" + celsius + "C"
}
```

```scala
scala> var t = new Thermometer
t: Thermometer = 32.0F/0.0C

scala> t.celsius
res6: Float = 0.0

scala> t.celsius = 100
t.celsius: Float = 100.0

scala> t.celsius = -40
t.celsius: Float = -40.0

scala> t
res7: Thermometer = -40.0F/-40.0C

scala> t.fahrenheit = 100
t.fahrenheit: Float = 100.0

scala> t
res8: Thermometer = 100.0F/37.77778C
```

## 18.3 案例分析：离散事件模拟

......

## 19 类型参数化

设计一个纯函数式类，来呈现类型参数化和信息隐藏，因为信息隐藏可以被用于更通用的类型参数化变注解。

类型参数化让我们能够编写泛型和注解。与Java不同，`Scala并不允许原生类型，Scala要求我们给出类型参数`，**型变定义了参数化类型的继承关系**，以Set[String]为例，型变定义了它是不是Set[AnyRef]的子类型。

### 19.1 函数式队列

函数式队列是一种数据结构，它支持三种操作：

- head 返回队列的第一个元素
- tail 返回队列的最后一个元素
- enqueue 返回一个将给定元素追加到队尾的新队列

跟可变队列不同，函数式队列在新元素被追加时，其内容不会被修改，而是返回一个新的队列。

本节主要实现的队列能够实现下列操作：

```scala
scala> val q = Queue(1, 2, 3)
q: Queue[Int] = Queue(1, 2, 3)

scala> val q1 = q enqueue 4
q1: Queue[Int] = Queue(1, 2, 3, 4)

scala> q
res0: Queue[Int] = Queue(1, 2, 3)
```

纯函数式队列还跟列表有一些相似：它们都被称为**完全持久化**(fully persistent)的数据结构，在经过扩展或修改后，老版本将继续可用。它们都支持head和tail操作，不同的是**列表添加元素是在列表头部扩展**，使用`::`操作；而**队列是在尾部扩展**，用的`euqueue`方法。

要实现高效的操作，使head、tail和enqueue操作都以常量时间完成。实现函数式队列的一种简单方式使用列表作为表现类型。这样一来head和tail都只是简单地翻译为列表中相同的操作，而enqueue则通过列表的拼接实现。

```scala
class SlowAppendQueue[T](elems: List[T]){
  def head = elems.head
  def tail = new SlowAppendQueue(elems.tail)
  def enqueue(x: T) = new SlowAppendQueue(elems ::: List(x))
}
```

enqueue的操作时间跟队列中存放元素的数量成正比（:::操作，从后往前拼接）。如果想常量时间实现enqueue操作，可以`尝试将底层列表中的元素顺序反转过来，这样最后追加的元素出现在列表的头部`。

```scala
class SlowAppendQueue[T](smele: List[T]){
  //smele是elems反转过来的意思
  def head = smele.last //此操作与元素数量成正比
  def tail = new SlowAppendQueue(smele.init) //此操作与元素数量成正比
  def enqueue(x: T) = new SlowAppendQueue(x :: smele)
}
```

从上面的两个例子，看上去无法将3种操作都做到常量时间。但有一种方法，将两种操作结合到一起，可以非常接近这个目标。背后的理念是leading和trailing两个列表来表示队列，leading列表包含队列中靠前的元素。trailing列表包含队列中靠后的元素，***按倒序排列***。整个队列在任何时刻的内容都等于`leading ::: trailing.reverse`。

要追加一个元素，只需要用::操作将它追加到trailing，这样一来enqueue操作就是常量时间。这意味着，当开始为空的队列通过接连的enqueue操作初始化时，trailing列表会增长而leading列表保持为空。接下来在head和tail被执行到空的leading列表之前，整个trailing列表被复制到leading，同时元素的顺序被反转，这是通过mirror操作完成：

```scala
class Queue[T](
  private val leading: List[T],
  private val trailing: List[T]
  ){
    private def mirror = {
      if (leading.isEmpty)
        new Queue(trailing.reverse, Nil)
      else
        this
    }
    def head = mirror.leading.head
    def tail = {
      val q = mirror
      new Queue(q.leading.tail, q.trailing)
    }
    def enqueue(x: T) = {
      new Queue(leading, x :: trailing)
    }
  }
```

mirror操作耗时，与队列元素的数量成正比，但仅当leading为空时在发生。leading非空时，直接返回。由于head和tail调用了mirror，它的复杂度与队列的长度也成了线性关系，不过，顺着队列变长，mirror被调用的频率也会变低。当head、tail和enqueue三者的操作频次相对均衡时，这样的摊销复杂度是的整体操作耗时趋近于常量，否则这个论点不成立。

### 19.2 信息隐藏

上节例子中的操作效率很高，但是也有部分缺点，就是暴露了不必要的实现细节。如全局可访问的Queue构造方法接收列表列表作为参数，其中一个顺序还是反的：很难说是一个直观的对队列的表示，需要用代码将这个构造方法隐藏。

#### 19.2.1 私有构造方法和工厂方法

加上`private`来隐藏构造方法：

```scala
class Queue[T] private(
  private val leading: List[T],
  private cal trailing: List[T]
)
```

类名和参数之前的`private`修饰符表示这个构造方法是私有的：`它只能从类本身及其伴生对象访问`，类名依然是公有的，因此可以把它当做类型来使用，但是不能调用其构造方法。

既然Queue类的主构造方法不能从使用方代码调用，就需要定义别的方式来创建队列，一种可能的方式是添加一个辅助构造方法：

```scala
def this() = this(Nil, Nil)

//或者
def this(elems: T*) = this(elems.toList, Nil) //T*用来表示重复的参数
```

另一种但是是创建其伴生对象：

```scala
object Queue{
  def apply[T](xs: T*) = new Queue(xs.toList, Nil)
}
```

因为Queue是对象而不是一个函数，Queue使用起来就像全局定义的工厂方法一样，实际上Scala并没有全局课件的方法，每个方法都必须被包含在某个对象或某个类中。不过，通过在全局对象中使用apply方法，可以支持看上去像是全局方法的使用模式。

#### 19.2.2 备选方案：私有类

私有构造方法和私有成员只是隐藏类的初始化和内部表现形式的一种方法。另一种更加激进的方式是隐藏整个类，并且只暴露一个反映类的公有接口的特质。

```scala
trait Queue[T]{
  def head: T
  def tail: Queue[T]
  def enqueue(x: T): Queue[T]
}

object Queue{
  def apply[T](xs: T*): Queue[T] = {
    new QueueTmp1[T](xs.toList, Nil)
  }
  private class QueueTmp1[T]( //直接隐藏整个类
    private val leading: List[T],
    private val trailing: List[T]
  ) extends Queue[T]{
    def mirror = {
      if(leading.isEmpty)
        new QueueTmp1(trailing.reverse, Nil)
      else
        this
    }
    def head: T = mirror.leading.head
    def tail: QueueTmp1[T] = {
      val q = mirror
      new QueueTmp1(q.leading.tail, q.trailing)
    }
    def enqueue(x: T) = {
      new QueueTmp1(leading, x :: trailing)
    }
  }
}
```

### 19.3 型变注解

Queue被定义为一个trait，因为它接受一个类型参数，如下定义无法编译：

```scala
scala> def doesNotComplie(q: Queue) = {} //Queue需要有类型参数
<console>:13: error: trait Queue takes type parameters
       def doesNotComplie(q: Queue) = {}
                             ^
```

特质可以让我们执行参数化的类型，比如，Queue[AnyRef]、Queue[String]、Queue[Int]等。

```scala
scala> def doesComplie(q: Queue[AnyRef]) = {}
doesNotComplie: (q: Queue[AnyRef])Unit
```

Queue是一个特质，Queue[String]是一个类型。Queue也被称为类型构造方法(type constructor)，因为可以通过执行类型参数来构造一个类型。类型构造方法Queue能够“生成”成组的类型，包括Queue[Int]、Queue[String]、Queue[AnyRef]等。

也可以将Queue说成是一个**泛型**的特质，可以通过泛化的类或特质来定义许许多多具体的类型，在上述例子中，Queue特质就是定义了一个泛型的队列，Queue[Int]和Queue[String]就是哪些具体的类型。

类型参数和子类型这两个概念放在一起，就会有一些有趣的问题，例如通过Queue[T]生成的具体类型之间有没有特殊的子类型关系，更确切的说，`Queue[String]是不是Queue[AnyRef]的子类型?`

`如果S是T的子类型，那么Queue[S]是不是Queue[T]子类型`？如果是，可以说Queue特质在类型参数T上是**协变(convariant)**的。`由于Queue只有一个参数，也可以简单的说Queue是协变的`。协变的Queue意味着传入一个Queue[String]到前面doesCompile方法，这个方法接收的类型为Queue[AnyRef]的值参数。

直观地讲，所有这些看起来都是没有问题的，因为一个String的队列看上去就像是AnyRef的队列的特例。不过，在Scala中，`泛型类型默认的子类型规则是不变(noconvariant)的`。也就是说像上例Queue定义，`不同元素类型的队列之间永远不会存在子类型关系`。`Queue[String]不能当做Queue[AnyRef]的子类型使用`。

不过可以修改定义来要求队列的子类型关系是协变得：

```scala
trait Queue[+T]{...}
```

在类型参数T前面，加上`"+"表示在参数上是协变的`，这样Queue[String]就能当做Queue[AnyRef]的子类型使用。

此外，还有逆变(contravariant)的子类型关系：

```scala
trait Queue[-T]{...}
```

那么，`如果S是T的子类型，则Queue[T]是Queue[S]的子类型`。**类型参数协变的、逆变的、还是不变的，都被称为类型参数的型变(variance)**。可以放在类型参数旁边的"+"和"-"被称为`型变注解`。

如定义一下例子：

```scala
class Cell[T](init: T){
  private[this] var current = init
  def get = current
  def set(x: T) = {current = x}
}
```

然后定义：

```scala
val c1 = new Cell[String]("abc")
val c2: Cell[Any] = c1
c2.set(1)
val s: String = c1.get
```

单看上述4行定义都没有问题，但是放在一块就出现了问题，Cell被声明为非协变的，c2的定义就会抛出type mismatch的错误。最后一行讲一个Int类型的1赋值给了String类型的s，这显然有悖于类型约束。

第二行用到了协变的子类型关系，但是String的Cell并不同时是Any的Cell，因为有些我们对Any的Cell做的事并不能对String的Cell做。举例来说，并不能对String的Cell使用参数为Int的set。

```scala
scala> val c1 = new Cell[String]("abc") //Cell为
c1: Cell[String] = Cell@3e230171

scala> val c2: Cell[Any] = c1
<console>:13: error: type mismatch;
 found   : Cell[String]
 required: Cell[Any]
Note: String <: Any, but class Cell is invariant in type T.
You may wish to define T as +T instead. (SLS 4.5)
       val c2: Cell[Any] = c1
                           ^
```

假如将Cell的类型参数声明为协变的，在调用时，还会得到一个错误：

```scala
scala> class Cell[+T](init: T){
     | private[this] var current = init
     | def get = current
     | def set(x: T) = {current = x}
     | }
<console>:14: error: covariant type T occurs in contravariant position in type T of value x
       def set(x: T) = {current = x}
               ^
```

#### 19.3.1 型变和数组

这个行为和Java比较会很有趣，从原理上讲，数组和单元格很像，只不过数组可以有多于一个元素。尽管如此，在Java中数据被当做协变来处理的。看如下Java代码：

```java
String[] a1 = {"abc"};
Object[] a2 = a1;
a2[0] =  new Integer(17);
String s = a1[0];
```

如果执行代码，发现是能通过编译的，不过在运行时，当a2[0]被赋值一个Integer，程序就会抛出ArrayStoreException。

发生了什么？Java在运行时会保存数组的元素类型，每当数组元素被更新时，都会检查新元素是否满足保存下来的类型要求。如果新元素不是这个类型的实例，就会抛出ArrayStoreException。

Java这样的设计，需要协变的数组，才能让任意引用类型的数组得以传入这个方法。当然随着Java泛型的引入，不再需要协变的数组。不过由于兼容性的原因，还保留了这样的做法。

Scala在这一点上比Java做的更加纯粹，它并不把数组当做协变的。如果尝试把代码的前两行翻译为Scala语言，就像这样：

```scala
scala> val a1 = Array("abc")
a1: Array[String] = Array(abc)

scala> val a2: Array[Any] = a1
<console>:12: error: type mismatch;
 found   : Array[String]
 required: Array[Any]
Note: String <: Any, but class Array is invariant in type T.
You may wish to investigate a wildcard type such as `_ <: Any`. (SLS 3.2.10)
       val a2: Array[Any] = a1
                            ^
```

发现报错了，这是因为Scala将数组处理为不变（刻板的），因此Array[String]并不能当做是Array[Any]处理。

但是，Scala允许将元素类型为T的数组类型，转换为T的任意超类的数组：

```scala
scala> val a2: Array[Object] = a1.asInstanceOf[Array[Object]]
a2: Array[Object] = Array(abc)
```

编译时永远合法，且运行时也会永远成功，因为JVM的底层运行时，把数组的处理当做协变的。不过，跟Java一样，可能在这之后，会得到一个ArrayStoreException。

### 19.4 检查型变注解

所有对类型可靠性的违背都设计可被重新赋值的字段或数组元素。与此相对应，纯函数式实现的队列看上去是协变不错的候选人。不过通过如下的例子你会看到，即便没有可被重新赋值的字段，还是有办法能“刻意地作出”不可靠的情况。

假定在19.2.2中定义的Queue是协变的，然后创建一个特别针对元素类型Int的队列子类，重写enqueue方法：

```scala
class StrangeIntQueue extends Queue[Int]{
  override def enqueue(x: Int) = {
    println(math.sqrt(x))
    super.enqueue(x)
  }
}
```

StrangeIntQueue的enqueue方法会先打印出入参的平方根，然后再做追加操作。现在，做出一个反例：

```scala
val x: Queue[Any] = new StrangeIntQueue
x.enqueue("abc")
```

对一个字符串求平方根，这完全讲不通。显然，`并不仅仅是可变字段能让协变类型变得不可靠`。还有更深层次的问题。`一旦泛型参数类型作为方法参数类型，包含这个泛型参数的类或特质就不能以那个类型做协变`。

对于队列而言，enqueue方法违背了这个条件：

```scala
class Queue[+T]{  //类型参数协变
  def enqueue(x: T){ //方法参数并没有协变
    x
  }
}

error: covariant type T occurs in contravariant position in type T of value x
  def enqueue(x: T){
              ^
```

> ☆☆☆☆☆ 可被重新赋值的字段是如下规则的特例：`用+注解的类型参数不允许用于方法参数的类型`。正如在18.2节中提到的，一个被重新赋值的字段“var x: T”在Scala中被当做getter方法“def x: T”和setter方法“def x_=(y: T)”。我们看到setter方法有一个参数，其类型为字段类型T，因此这个类型不能是协变的。

此外，最重要的一点，`Scala编译器会检查你加在类型参数上任何型变注解`。例如，`如果你尝试生命一个类型为协变的（添加一个+），但是有可能引发潜在的运行时错误，你的程序将无法通过编译`。

### 19.5 下界

之前队列类Queue中的类型参数T不能定义为协变的，因为T在enqueue方法中作为参数类型出现，这里是一个逆变点。

幸运的是，有一种方式可以改变这种场景，使用多态来泛化enqueue，为其参数类型执行一个下界(lower bound)：

```scala
class Queue[+T](
  private[this] var leading: List[T],
  private[this] var trailing: List[T]
){
  def enqueue[U >: T](xs: U) = {
    new Queue[U](leading, xs :: trailing)
  }
}
```

为enqueue方法定义了一个新的参数U，语法为U >: T，代表T为U的下界，也就是U必须是T的超类型（超类型和子类型关系是自反的，这意味着类型既是它自己的超类型又是它自己的子类型。），即使T是U的下界，你仍然可以传递一个T给方法enqueue。

例如，有一个名为Fruit的类和它的两个子类Apple和Orange，如果定义了一个新类Queue，就可以将Orange添加到Queue[Apple]，其结果将是Queue[Fruit]。

可以说，队列的新定义比原来定义更好，因为它更通用。与旧版本不同，新定义允许附加队列元素类型T的任意超类型U，结果是Queue[U]。由于Queue的类型参数也定义为协变的，这就为创建不同元素类型的队列提供了适当的灵活性。

这表明协变声明和下界组合在一起可以很好地发挥作用。它们是`类型驱动设计`的好例子，其中接口的类型指导其详细设计和实现。

对于队列，可能没有想到使用一个下界来改进enqueue的实现。在此之前，你可能决定让队列协变，在这种情况下，编译器会指出队列的协变错误。通过增加一个下界来修正方差误差可以使队列更通用，队列作为一个整体也更有用。

> ※※※ 与Java不同的是，Scala选择在`声明点协变`而不是定义在`用户调用点协变`
。在Java中，定义为用户调用点协变，需要自己设计一个类。需要使用通配符的将是类的入口，如果使用时遇到错误，后续一些重要的实例方法将不再适用。协变就是一个棘手的问题，用户通常会犯错误，他们会认为通配符和泛型过于复杂。通过`定义端协变`，可以向编译器表达意图，编译器会检查想要可用的方法是否确实可用。

### 19.6 逆变

前面讨论了协变和非协变，有时逆变也是常用的。

```scala
trait OutputChannel[-T]{
  def write(x: T)
}
```

OutputChannel中的类型参数T定义为逆变。举个例子，AnyRef的output channel是String output channel的子类型，虽然看起来有违直觉，但它确实能讲得通的。OutputChannel[String]仅支持String类型写入操作。写入String类型的操作同样可以应用到OutputChannel[AnyRef]。所以使用OutputChannel[AnyRef]替换OutputChannel[String]是没有什么问题的，但是反过来就不可行，因为OutputChannel[String]不能写入String类型以外的值。

这个推理指向类型系统设计中的一个通用原则：`如果可以在需要类型U的地方替换类型T的值，那么可以安全地假设类型T是类型U的子类型`，这种原则称为**里氏替换原则**(Liskov Substitution Principle)。

如果T支持与U相同的操作，并且T的所有操作都需要更少而比U提供更多的相关操作。在这种情况下，OutputChannel[AnyRef]可以是OutputChannel[String]的子类型，因为这两个操作支持相同的写操作，并且该操作在OutputChannel[AnyRef]中比在OutputChannel[String]中要求的条件更少。“更少”意味着参数在第一种情况下只需要入参是AnyRef，而后者要求入参是String。

有时协变和逆变混合在同一类型中。一个典型的例子是Scala的函数特质。例如，当编写函数类型`A => B`时，Scala将其展开成`Function1[A, B]`。

```scala
trait Function1[-S, +T]{
  def apply(x: S): T
}
```

标准库中Function1的定义使用了协变和逆变：Function1特质`在入参类型S上是逆变，在结果类型T上协变`，如下所示。这满足了里氏替换原则，因为函数入参是对外的要求，结果是函数向外提供的返回值。

举例如下：

```scala
class Publication(var title: String)
class Book(title: String) extends Publication(title)

object Library {
  val books: Set[Book] = {
    Set(
      new Book("Programming in Scala"),
      new Book("Walden")
    )
  }
  def printBookList(info: Book => AnyRef) = {
    for (book <- books) println(info(book))
  }
}

object Customer extends App {
  def getTitle(p: Publication): String = p.title
  Library.printBookList(getTitle)
}
```

最后一行，调用了Library的printBookList，并将getTitle作为参数传入：

```scala
Library.printBookList(getTitle)
```

这行可以通过编译器的检查，尽管函数的结果类型String是printBookList的info参数的结果类型AnyRef的子类型。这段代码能够通过编译是因为函数的结果类型被声明为协变的。

printBookList方法会遍历Book列表，并对每本书调用传入的函数，它将info返回的AnyRef结果传入println，由它调用并打印出结果，这个动作对String和AnyRef的任何子类都可行，这就是函数的结果类型协变的意义。

printBookList方法的函数的参数类型声明为Book，传入给getTitle方法的参数却是Publication，它是Book的子类型。这样之所以可行，背后的原因是：`由于printBookList的参数类型是Book，printBookList的方法体只能将Book传入，而由于getTitle的参数类型是Publication，这个函数的函数体只能对其参数p访问Publication类中声明的成员。由于Publication中声明的所有方法都在其子类Book中可用，一切都应该可以工作，这就是函数参数类型逆变的意义。`

```plain txt
 ┌──────┐     ┌─────────────┐
 │ Book │ ──> │ Publication │
 └──────┘     └─────────────┘

 ┌────────────────┐     ┌───────────────────────┐
 │ Book => AnyRef │ <── │ Publication => String │
 └────────────────┘     └───────────────────────┘

 ┌────────┐     ┌────────┐
 │ AnyRef │ <── │ String │
 └────────┘     └────────┘
```

上面的代码能通过编译，是由于`Publication => String`是`Book => AnyRef`的子类型。由于Function1的结果类型定义为协变，上图中下部的显示两个结果类型的继承关系跟中部函数的继承关系方向是相同的，而由于Function入参类型定义为逆变，图中上部显示的两个参数类型的继承关系和函数的继承关系方向是相反的。

### 19.7 对象私有数据

前面定义的Queue类有一个问题，就是leading一直为空，如果连续调用mirror操作会反复地从trailing拷贝元素到leading。这种无畏的拷贝可以通过添加副作用的方式避免，下面的下代码，对连续的head操作最多执行一次trailing到leading的拷贝，但每次拷贝都会有两个副作用，而不是返回新的队列，这个副作用纯粹是Queue内部的，对使用方不可见，仍然是纯函数式对象。

```scala
class Queue[+T] private(
  private[this] var leading: List[T],
  private[this] var trailing: List[T]
){
  private def mirror() = {
    if (leading.isEmpty) {
      while (!trailing.isEmpty){
        leading = trailing.head :: leading
        trailing = trailing.tail
      }
    }
  }
  def head: T = {
    mirror()
    leading.head
  }
  def tail: Queue[T] = {
    mirror()
    new Queue (leading.tail, trailing)
  }
  def enqueue[U >: T](x: U) = {
    new Queue[U](leading, x :: trailing)
  }
}
```

这样的定义是怎么通过编译器的检查的，毕竟队列现在包含两个协变的参数类型T的可被从新赋值的字段，这不是违背了型变规则？的确有这个嫌疑，不过leading和trailing带上了private[this]的修饰符，因而对象是私有的。只能从对象的内部访问，从而`从定义变量的同一个对象访问这些变量`并不会造成型变的问题。

只管的理解是，**如果我们要构造一个型变会引发类型错误的场景，需要引入一个从静态类型上比定义该对象更弱的对象，而访问对象私有值的情况，这是不可能出现的**。

Scala的型变检查对于对象私有定义有一个特殊规则，`在检查带有+或-的类型个参数只应出现在相同的型变归类的位点时，会忽略掉对象私有的定义`。但是如果漏掉这两个private修饰符的[this]限定词，我们将看到如下错误：

```scala
13: error: covariant type T occurs in contravariant position in type List[T] of value leading_=
  private var leading: List[T],
              ^
14: error: covariant type T occurs in contravariant position in type List[T] of value trailing_=
  private var trailing: List[T]
              ^
```

### 19.8 上界

如前面16章中的例子，展示了一个接收比较函数作为第一个入参，以及一个要排序的列表作为第二个（柯里化）入参的归并排序函数。也可以用另一种方式来组是这样一个排序函数，那就是要求列表的类型是混入了Ordered特质的，就像12.4节提到的，通过混入Ordered并实现Ordered特质的首相方法compare，可以让类的使用方代码<、>、<=和>=来比较实例，例如：

```scala
class Person(val firstName: String, val lastName: String) extends Ordered[Person]{
  def compare(that: Person) = {
    val lastNameComparison = lastName.compareToIgnoreCase(that.lastName)
    if (lastNameComparison != 0)
      lastNameComparison
    else
      firstName.compareToIgnoreCase(that.firstName)
  }
  override def toString = firstName + " " + lastName
}

scala> val robet = new Person("Robert", "Jones")
robet: Person = Robert Jones

scala> val sally = new Person("Sally", "Smith")
sally: Person = Sally Smith

scala> robet > sally
res0: Boolean = false

scala> robet < sally
res1: Boolean = true
```

为了确保传入到这个新的排序函数的列表类型混入了Ordered，需要使用上界(upper bound)。上界的指定方式跟下界类似，用符号`<:`

```scala
def orderedMergeSort[T <: Ordered[T]](xs: List[T]): List[T] = {
  def merge(xs: List[T], ys: List[T]): List[T] = (xs, ys) match {
    case (Nil, _) => ys
    case (_, Nil) => ys
    case (x :: xs1, y :: ys1) =>
      if (x < y) x :: merge(xs1, ys)
      else y :: merge(xs, ys1)
  }
  val n = xs.length / 2
  if (n == 0) xs
  else {
    val (ys, zs) = xs splitAt n
    merge(orderedMergeSort(ys), orderedMergeSort(zs))
  }
}

orderedMergeSort: [T <: Ordered[T]](xs: List[T])List[T]
```

通过“`T <: Ordered[T`]”这样的语法，告诉我们编译器`类型参数T有一个上界Order[T]`。这意味着`orderedMergeSort的列表元素类型必须是Ordered的子类型`。可以将List[Person]传给orderedMergeSort，因为Person混入了Ordered。

```scala
val people = List(
  new Person("Larry", "Wall"),
  new Person("Anders", "Hejlsberg"),
  new Person("Guido", "van Rossum"),
  new Person("Alan", "Kay"),
  new Person("Yukihiro", "Matsumoto")
)

people: List[Person] = List(Larry Wall, Anders Hejlsberg, Guido van Rossum, Alan Kay, Yukihiro Matsumoto)
```

由于这个列表的元素类型Person混入了Ordered[Person]（也就是说它是Ordered[Person]的子类型），可以将这个列表传入orderedMergeSort：

```scala
val sortedPeople = orderedMergeSort(people)

sortedPeople: List[Person] = List(Anders Hejlsberg, Alan Kay, Yukihiro Matsumoto)
```

它实际上并不是Scala中利用Ordered特质设计排序函数的最通用的方式。举例来说，我们并不能用orderedMergeSort来对整数列表进行排序，因为Int类并不是Ordered[Int]的子类型：

```scala
scala> val wontCompile = orderedMergeSort(List(3, 2, 1))
<console>:12: error: inferred type arguments [Int] do not conform to method orderedMergeSort's type parameter bounds [T <: Ordered[T]]
       val wontCompile = orderedMergeSort(List(3, 2, 1))
                         ^
<console>:12: error: type mismatch;
 found   : List[Int]
 required: List[T]
       val wontCompile = orderedMergeSort(List(3, 2, 1))
                                              ^
```

## 20 抽象成员

如果类或特质的成员在类中没有完整的定义，则该成员是抽象的。类中声明的抽象成员在它的子类中实现。

在本章中，将描述所有四种抽象成员：vals、var、方法和类型。在此过程中，我们将讨论`预初始化的字段`、`惰性vals`、`路径依赖类型`和`枚举`。

### 20.1 快速浏览轴向成员

定义抽象成员：

```scala
trait Abatract{
  type T
  def transform(x: T): T
  val initial: T
  var current: T
}
```

抽象的具体实现需要为其每个抽象成员实现定义：

```scala
class Concrete extends Abstract{
  type T = String
  def transform (x: T) = x + x
  val initial = "hi"
  var current = initial
}
```

### 20.2 类型成员

从上一节的示例中可以看到，Scala中抽象类型意味着使用type关键字声明一个类型，而没有指定定义。类本身可能是抽象的，特质在定义上也是抽象的，但这两种类型在Scala中都不是抽象类型。

在Scala中，`抽象类型总是类或特质的成员`，就像T是Abstract的类型成员。

可以将非抽象(或“具体”)类型成员(例如类Concrete中的类型T)视为为类型定义新名称或别名的方法。在Concrete中，将String赋给了别名为T的类型，在类中的任何位置，T都是String。

`使用类型成员的一个原因是为类型定义一个简短的描述性别名`，它的真实名称比别名更冗长，或者含义更不明显，类型成员可以帮助你简洁地阐明类或特质的代码。类型成员的其他主要用途是声明必须在子类中定义的抽象类型。

### 20.3 抽象vals

抽象vals的定义类似于：

```scala
val initial: String
```

对一个val定义了类型，但并没有给出值，具体的实现在Concrete中实现：

```scala
val initial = "hi"
```

抽象vals的声明，类似于一个无参方法的定义：

```scala
def initial: String
```

从使用方来看，两者的定义没有任何区别。但是，如果initial是一个抽象val，使用方调用`obj.initial`每次都会返回相同的值；而如果initial是一个抽象方法，就未必是这样。作为抽象方法，在子类中实现时，可以在每次调用时都返回不同的值。

换句话说，`抽象val的执行规则`有限制：`任何实现都必须为一个val的定义，不能使用var或者def`。另一方面，抽象方法声明可以通过具体的方法定义和具体的val定义来实现。下面例子中，抽象类Fruit，子类Apple是合法的，但BadApple则不行：

```scala
abstract class Fruit{
  val v: String
  def m: String
}
abstract class Apple extends Fruit{
  val v: String
  val m: String
}
abstract class BadApple extends Fruit{
  def v: String //编译器会报错，不能使用def来实现一个val
  def m: String
}

<console>:13: error: overriding value v in class Fruit of type String;
 method v needs to be a stable, immutable value
       def v: String
           ^
```

### 20.4 抽象vars

和抽象vals定义类似，抽象vars的定义：

```scala
trait AbstractTime{
  var hour: Int
  var minute: Int
}
```

声明为类成员的vars，编译器自动定义了getter和setter方法，与下面定义一致：

```scala
trait AbstractTime{
  var hour: Int
  var hour_=(x: Int)
  var minute: Int
  var minute_=(x: Int)
}
```

### 20.5 初始化抽象vars

抽象的vals有时起到类似于超类参数的作用：`允许在子类中实现父类缺少的详细信息`。这对于特质来说特别重要，`因为特质没有可以传递参数的构造函数`。因此，参数化特征的通常概念是通过抽象vals，在子类中实现的。

现在重写在第6章中实现的Rational：

```scala
trait RationalTrait{
  val numerArg: Int
  val denomArg: Int
}
```

在第六章中，Rational有两个参数，n和d，这里定义了两个抽象成员numerArg和denomArg，然后实例化：

```scala
new RationalTrait{
  val numerArg = 1
  val denomArg = 2
}
```

这个表达式生成一个匿名类的实例，该匿名类混合了特质，这个特定的匿名类实例化的效果类似于创建新的实例`new Rational(1,2)`。

然而，这个类比并不完美。在表达式初始化的顺序上有一个细微的差别，当你写：

```scala
new Rational(expr1, expr2)
```

在Rational实例化之前，会检查expr1和expr2两个参数，是否在Rational中可用。

对于特质，则恰好相反：

```scala
new RationalTrait{
  val numerArg = expr1
  val denomArg = expr2
}
```

表达式、expr1和expr2作为匿名类初始化的一部分进行计算，但是匿名类在初始化RationalTrait之后进行初始化。因此，在`RationalTrait的初始化过程中，numerArg和denomArg的值是不可用的`(更精确的来说，使用任何一个值都会返回一个默认Int类型的0)。对于前面RationalTrait的代码定义，这并不是一个问题，因为特质的初始化不会用到numerArg和denomArg的值。

但是对于下面的例子来说，就会有问题：

```scala
trait RationalTrait{
  val numerArg: Int
  val denomArg: Int
  require (denomArg != 0) //在初始化时，denomArg的值为0，require调用失败
  private val g = gcd(numerArg, denomArg)
  val numer = numerArg / g
  val denom = denomArg / g
  private def gcd(a: Int, b: Int): Int = {
    if (b == 0) a else gcd(b, a % b)
  }
  override def toString = numer + "/" + denom
}

scala> val x = 2
x: Int = 2

scala> new RationalTrait{
     | val numerArg = 1 * x
     | val denomArg = 2 * x //初始化时，denomArg是0
     | }
java.lang.IllegalArgumentException: requirement failed
  at scala.Predef$.require(Predef.scala:268)
  at RationalTrait.$init$(<pastie>:14)
  ... 32 elided
```

如果尝试用一些不是简单的文字的分子和分母表达式实例化这个特质，将得到一个异常。这个示例中编译器会抛出异常，因为当类RationalTrait初始化时，denomArg的默认值仍然是0，这会导致require调用失败。

这个示例，展示了类参数和抽象字段的初始化顺序不同。`类参数`在`传递给类构造函数之前就已经被计算`(除非参数是按名称的)。与此不同的是，`子类中的val实现定义只有在超类被初始化之后才计算`。

现在已经知道了抽象vals和类参数的不同，后面介绍两种不同的初始化方式来优化这种场景，分别为`预初始化字段(pre-initialized fields)`和`惰性vals(lazy vals)`。

#### 20.5.1 预初始化字段

第一个解决方案是预初始化字段，它允许`在调用超类之前初始化子类的字段`。为此，只需在超类构造函数调用之前将字段定义放在大括号中。

```scala
new {
  val numerArg = 1 * x
  val denomArg = 2 * x
} with RationalTrait
```

预初始化的字段不限于匿名类，它们也可以在对象或具名子类中使用。

```scala
object twoThirds extends{
  val numerArg = 2
  val denomArg = 3
} with RationalTrait


class RationalClass(n: Int, d: Int) extends{
  val numerArg = n
  val denomArg = d
}with RationalTrait{
  def + (that: RationalClass) = new RationalTrait(
    numerArg * that.denomArg + that.numerArg * denomArg,
    denomArg * that.denomArg
  )
}
```

因为预初始化的字段是在调用超类构造器之前初始化的，它们的初始化器不能引用正在构造的对象。因此，如果初始化器引用`this`，那么指向包含正在构造的类或对象的对象，而不是被构造的对象本身。

```scala
scala> new {
     | val numerArg = 1
     | val denomArg = this.
     | val denomArg = this.numerArg * 2
     | } with RationalTrait
<console>:15: error: value numerArg is not a member of object $iw
       val denomArg = this.numerArg * 2
                           ^
```

该示例没有通过编译，因为引用了`this.numerArg`，这个操作会`在包含new对象`(在本例中是名为$iw的合成对象，解释器将用户输入行放入其中)`的对象中寻找numerArg字段。`再者，预初始化的字段在这方面的行为类似于类构造函数参数。

#### 20.5.2 惰性vals

可以使用预先初始化的字段精确地模拟类构造函数参数的初始化行为。但是，有时可能更愿意让系统自己来确定应该如何初始化。可以通过在`val之前加上lazy标识符`来实现，等式右边的表达式会在第一次引用val变量时计算。

下面举例说明：

```scala
scala> object Demo{
     | val x = {println("initialing x"); "done"}
     | }
defined object Demo

scala> Demo
initialing x
res1: Demo.type = Demo$@182e7eda

scala> Demo.x
res2: String = done
```

在使用Demo时，它的x字段就被初始化了，x的初始化时Demo初始化的一部分。如果加上`lazy`标识符：

```scala
scala> object Demo{
     | lazy val x = {println("initialing x"); "done"}
     | }
defined object Demo

scala> Demo //x没有被初始化
res3: Demo.type = Demo$@25109608

scala> Demo.x //在调用时，被初始化
initialing x
res4: String = done
```

初始化Demo并不涉及初始化x，x的初始化将被推迟到第一次使用x时。类似于使用def将x定义为无参数方法的情况，但是与def不同的是，lazy val永远不会被计算超过一次。实际上，在惰性val的第一次求值之后，将存储求值的结果，以便在随后使用相同的val时重用。

从这个例子可以看出，像lazy val的定义，可以只有在使用时才被初始化。实际上，`对象定义可以看作是使用描述对象内容的匿名类的惰性val定义的简写`。

使用lazy vals可以将RationalTrait修改成如下：

```scala
trait LazyRationalTrait{
  val numerArg: Int
  val denomArg: Int
  lazy val numer = numerArg / g
  lazy val denom = denomArg / g
  override def toString = numer + "/" + denom
  private lazy val g = {
    require(denomArg != 0)
    gcd(numerArg, denomArg)
  }
  private def gcd(a: Int, b: Int): Int = {
    if (b == 0) a else gcd(b, a % b)
  }
}

scala> val x = 2
x: Int = 2

scala> new LazyRationalTrait{
     | val numerArg = 1 * x
     | val denomArg = 2 * x
     | }
res6: LazyRationalTrait = 1/2
```

不需要预初始化。跟踪导致上面代码中输出的字符串1/2的初始化序列很有启发意义:

1. 创建一个新的LazyRationalTrait实例，并运行LazyRationalTrait的初始化代码。该初始化代码为空；LazyRationalTrait的字段还没有初始化。

2. 执行新表达式定义的匿名子类的主构造函数。这涉及到用2初始化numerArg，用4初始化denomArg。

3. 解释器在构造的对象上调用toString方法，以便输出结果值。

4. 特质LazyRationalTrait中的toString方法第一次访问numer字段，因此计算它的初始化器。

5. numer的初始化器访问私有字段g，因此接下来对g进行计算。这个计算访问在步骤2中定义的数字和分母。

6. toString方法访问denom的值，这会对denom的求值。denom的求值访问的是denomArg和g的值。g字段的初始化器不会被重新求值，因为它已经在步骤5中求值了。

7. 最后，构造并输出结果字符串“1/2”。

注意，在LazyRationalTrait类中，g的定义在文本上出现在numer和denom的定义之后。然而，因为这三个值都是惰性的，所以g在numer和denom的初始化完成之前就被初始化了。

这显示了惰性vals的一个重要属性：`其定义的文本顺序并不重要，因为值是按需初始化的`。因此，lazy vals可以让程序员不必费力地思考如何安排val定义，以确保在需要时定义所有内容。

然而，`只有初始化惰性vals既不产生副作用，也不依赖于它们，则该属性才会生效`。在存在副作用的情况下，初始化顺序影响实际使用。然后就很难跟踪初始化代码的运行顺序，正如前面的示例所演示的那样。所以`lazy vals是对函数对象的理想补充，此处初始化的顺序并不重要，只要最终所有东西都被初始化`。它们不太适合指令式编程。

### 20.6 抽象类型

在本章的开头，你看到了type T，一个抽象的类型描述。本章的其余部分将讨论这种抽象类型的表述的含义以及它的好处。与所有其他抽象声明一样，抽象类型声明是一个占位符，用于在子类中具体定义的内容。在本例中，type T是一个将在类继承层次结构中进一步定义的类型。因此，上面的T指的是在声明它时还未知的类型。不同的子类可以实现不同的T类型。

下面来举例说明，模拟动物的饮食行为，先定义Food类和Animal类及其方法eat：

```scala
class Food
abstract class Animal{
  def eat(food: Food)
}
```

然后再定义Cow和Grass类，然后编写如下代码：

```scala
class Grass extends Food
class Cow extends Animal{
  override def eat(food: Grass) = {}
}
```

但在编译时，会抛出一个错误：

```scala
<console>:14: error: method eat overrides nothing.
Note: the super classes of class Cow contain the following, non final members named eat:
def eat(food: Food): Unit
       override def eat(food: Grass) = {}
                    ^
```

Cow类中的eat方法并没有复写Animal中的eat方法，因为两者的eat方法类型不一致。

有些人认为，类型系统在拒绝这些类方面太严格，认为在子类中指定一个参数类型是合理的，但是，如果按编写的方式允许使用这些类，可能很快就会陷入不安全的情况。

```scala
class Food
abstract class Animal{
  def eat(food: Food)
}

class Grass extends Food
class Cow extends Animal{
  override def eat(food: Grass) = {} //不能编译，但如果可以编译的话
}

class Fish extends Food
val bessy: Animal = new Cow
bessy eat (new Fish)
```

因为定义牛的类(Cow)是Animal的子类，所以Cow中的eat方法会接受Food的子类作为入参，但这样就会出现牛吃鱼的情况，这是不符合常理的。

相反，需要应用一些更精确的建模。动物确实吃食物，但是每种动物(Animal)吃什么食物取决于它们(Animal)的定义，这样可以使用抽象类型巧妙的定义：

```scala
class Food
abstract class Animal{
  type SuitableFood <: Food
  def eat(food: SuitableFood)
}
```

新的定义类型中使用上界，动物只能吃适合吃的东西，所有在子类中具体实现的方法参数SuitableFood都必须是Food的子类。在下面的定义中，Cow在编译时就没什么问题:

```scala
class Grass extends Food
class Cow extends Animal{
  type SuitableFood = Grass
  override def eat(food: Grass) = {}
}
```

然后再定义Fish类，然后将Fish传入Cow的eat方法中：

```scala
scala> class Fish extends Food
defined class Fish

scala> val bessy: Animal = new Cow
bessy: Animal = Cow@36463b09

scala> bessy eat (new Fish) //由于Cow中的SuittableFood已经定义为Grass，所以不能传入Grass意外的食物
<console>:14: error: type mismatch;
 found   : Fish
 required: bessy.SuitableFood
       bessy eat (new Fish)
                  ^
```

### 20.7 路径依赖类型

上节例子最后的错误中显示`bessy.SuitableFood`，其中包含一个对象引用(bessy)，和它的字段SuitableFood。这表明`Scala中的对象可以将类型作为成员`。bessy.SuitableFood是作为bessy引用的对象成员的SuitableFood类型，或者，也可以是适合bessy的food类型。

像bessy.SuitableFood这种类型称为`路径依赖类型`。正如术语“路径依赖类型”所隐含的，`类型依赖于路径`;通常，不同的路径会产生不同的类型。例如，假设像这样定义了类DogFood和Dog:

```scala
class DogFood extends Food
class Dog extends Animal{
  type SuitableFood = DogFood
  override def eat(food: DogFood) = {}
}
```

如果试图用适合牛的食物喂狗，的代码将无法编译：

```scala
scala> val bessy = new Cow
bessy: Cow = Cow@29892a77

scala> val lassie = new Dog
lassie: Dog = Dog@40ac0a56

scala> lassie eat (new bessy.SuitableFood)
<console>:14: error: type mismatch;
 found   : Grass
 required: DogFood
       lassie eat (new bessy.SuitableFood)
                   ^
```

如果新定义一个Dog的实例，再做操作，则没有问题：

```scala
scala> val bootsie = new Dog
bootsie: Dog = Dog@312819ae

scala> lassie eat (new bootsie.SuitableFood)
```

Scala的路径依赖类型和Java的内部类类型相似，但不同的是：`路径依赖类型为外部对象命名，而内部类类型为外部类命名`。Java风格的内部类类型也可以用Scala表示，但是它们的编写方式不同。

```scala
class Outer{
  class Inner
}
```

在Scala中，使用表达式`Outer#inner`来处理内部类，而不是Java的`Outer.inner`。我们先实例化两个Outer类：

```scala
val o1 = new Outer
val o2 = new Outer
```

o1.Inner和o2.Inner是两个不同的路径依赖类型。

与Java一样，在Scala中，`内部类实例持有对外部类实例的引用`。这允许内部类访问其外部类的成员。因此，如果不以某种方式指定外部类实例，就不能实例化内部类。一种方法是`在外部类的主体中实例化内部类`。在这种情况下，将使用当前的外部类实例(`使用this引用`)。

```scala
class ScalaOuterClass {
  val name = "scott"
  private val salary: Double = 10000.90

  class ScalaInnerClass {
    def info(): Unit = {
      //内部类如果想要访问外部类的属性，可以通过外部类对象访问，方法为：外部类名.this.属性名
      println("name" + ScalaOuterClass.this.name + "sal = " + ScalaOuterClass.this.salary)
    }
  }
}
```

另一种方法是使用路径依赖类型。例如，因为类型为o1.Inner，命名一个特定的外部对象，你可以实例化它:

```scala
new o1.Inner
```

内部对象将包含外部对象的引用（从o1引用的对象）。相反，因为类型Outer#Inner没有指明Outer的任何特定实例，所以不能创建它的实例

```scala
scala> new Outer#Inner
<console>:13: error: Outer is not a legal prefix for a constructor
       new Outer#Inner
                 ^
```

### 20.8 改良类型

当一个类从另一个类继承时，将前者称为另一个的`名义(nominal)子类型`。之所以是名义子类型，是因为每个类型都有一个名称，而这些名称被显式地声明为存在子类型关系。除此之外Scala还额外支持`结构(struct)子类型`，即`只要两个类型有兼容的成员，就可以说他们之间存在子类型关系`。Scala实现结构子类型的方式成为`改良类型(refinement type)`。

名义子类型通常更方便，在新的设计中通常优先尝试名义子类型。名称是单个简短的标识符，因此比显式地列出成员的类型要更精简。但结构子类型通常更加灵活，不同的控件之间可能有相同的名字，比如画画draw()，一个西部牛仔拔枪draw()，两个名字相同，但并不能互相替代，当你不小心用错的时候，通常更加希望得到一个编译错误。

尽管如此，结构子类型也有自身优势，有时候某个类型除了成员之外并没有更多的信息。假如定义一个可以包含食草动物的Pasture类，一种选择是定义一个AnimalThatEatsGrass特质在适用的类上混入。不过这样代码很啰嗦，Cow类已经声明了它是动物，并且食草，现在它还需要声明它是一个“食草动物”。

除了定义AnimalThatEatsGrass外，还可以使用改良类型。只需要写下基类型Animal，然后加上一系列用花括号包括起来的成员即可。花括号中的成员进一步指定了基类中的成员类型。

```scala
Animal {type SuitableFood = Grass}
```

有了这个类型声明，就可以像这样来编写Pasture类：

```scala
class Pasture {
  val animals: List[Animal {type SuitableFood = Grass}] = Nil
}
```

### 20.9 枚举

在Scala的枚举支持端口中发现了一个关于路径依赖类型的有趣应用。其他一些语言，包括Java和c#，使用枚举作为内置的语言构造来定义新类型。Scala对枚举不需要特殊的语法。相反，在它的标准库中有一个类`scala.Enumeration`。

要创建新的枚举，需要定义一个扩展该类的对象，如下面的示例所示，该对象定义了Colors的新枚举:

```scala
object Colors extends scala.Enumeration{
  val Red = Value
  val Green = Value
  val Bule = Value
}
```

也可以使用较为简洁的方法：

```scala
object Colors extends scala.Enumeration{
  val Red, Green, Bule = Value
}
```

使用import导入定义好的类和枚举值：

```scala
import Color._
```

枚举定义了一个名为Value的内部类，同名的无参数值方法返回该类的一个新实例。换句话说，一个值，比如Colors.Red是Color.Value的类型。Colors.Value是所有在Colors中定义枚举值的类型。它是一个路径依赖类型，Colors是路径，Value是依赖类型。重要的是，这是一个全新的类型，不同于所有其他类型。

Scala的枚举类还提供了其他语言的枚举设计中可以找到的许多其他特性。通过使用Value方法的另一个重载变体，可以将名称与枚举值关联起来:

```scala
object Direction extends Enumeration {
  val North = Value("North")
  val East = Value("East")
  val South = Value("South")
  val West = Value("West")
}

//可以通过枚举的values方法返回的集合迭代枚举的值:
scala> for (d <- Direction.values) print(d + " ")
North East South West
```

枚举值从0开始编号，通过id方法可以找到枚举值的个数:

```scala
scala> Direction.North.id
res15: Int = 0
```

也可以采用另一种方式，从非负整数到枚举中以该数字作为id的值:

```scala
scala> Direction(0)
res16: Direction.Value = North
```

这里展示了部分枚举类型实现，可以在scala.Enumeration中找到更多的使用方法。

### 20.10 案例：货币

本章的剩余部分将通过案例研究来解释如何在Scala中使用抽象类型。这里定义一个货币类`Currency`，一个典型的货币类表示以美元、欧元、日元或其他货币表示的金额。还会通过一些算法来计算两种货币通过汇率互相转换。

```scala
abstract class Currency{
  val amount: Long //货币的金额
  def designation: String //货币符号 USD Yen...
  override def toString = amount + " " + designation
  def + (that: Currency): Currency = ...
  def * (x: Double): Currency = ...
}
```

可以通过提供具体的数量和指定值来创建具体的货币值:

```scala
new Currency{
  val amount = 79L
  def designation = "USD"
}
```

如果我们想要建模的只是单一货币，比如美元或欧元，那么这个设计是可行的。但如果我们需要处理几种货币，它就会失败。假设将美元和欧元建模为类Currency的两个子类:

```scala
abstract class Dollar extends Currency{
  def designation = "USD"
}

abstract class Euro extends Currency{
  def designation = "Euro"
}
```

这样看起来可行，但是这样会允许美元和欧元直接相加，这是不符合常理的。相反，需要将+方法定义为通用的版本。当实现Dollar类时，它应该接收Dollar类型的数据并且交出Dollar类型的结果，作用于Euro时同样如此。所以+方法的类型依赖于所在的类，尽管如此，希望只编写一次加法方法，而不是每次定义新货币时都编写。

在Scala中，有一种简单的技术可以处理这样的情况。如果在定义类时有些东西还不知道具体怎么来实现，那么将其抽象到类中。这种方法适用于值和类型。对于Currency，+方法的确切参数和结果类型是未知的，将其定义为抽象类型是个很好的选择。

```scala
abstract class AbstractCurrency{
  type Currency <: AbstractCurrency //定义了一个抽象类型
  val amount: Long
  def designation: String
  override def toString = amount + " " + designation
  def + (that: Currency): Currency = ...
  def * (x: Double): Currency = ...
}
```

然后再定义新的Dollar类：

```scala
abstract class Dollar extends AbstractCurrency{
  type Currency = Dollor
  def designation = "USD"
}
```

这样的设计可用，但是并不完善，因为缺少了+和*的具体定义。那么如何在类中实���������个方法呢？可能会使用`this.amount + that.amount`来实现+方法，但是怎么将不同币种的数据转化成同一币种，然后相加呢？可能会这么实现：

```scala
def +(that: Currency): Currency = new Currency{
  val amount = that.amount + this.amount
}
```

但是这样并不能编译：

```scala
error: class type required
def + (that: Currency): Currency = new Currency {
                                       ˆ
```

Scala处理抽象类型的一个限制是，`既不能创建抽象类型的实例，也不能将抽象类型作为另一个类的超类型`。因此，编译器会拒绝试图实例化抽象类型Currency的代码。

但是，可以使用工厂方法解决此限制。不是直接创建抽象类型的实例，而是声明一个执行该操作的抽象方法。然后，当抽象类型被固定为某种具体类型时，还需要给出工厂方法的具体实现。对于类AbstractCurrency，如下所示：

```scala
abstract class AbstractCurrency{
  type Currency <: AbstractCurrency //抽象类型
  def make(amount: Long): Currency //工厂方法
  ...
}
```

这样的设计是可行的，但它看起来很可疑。为什么要把工厂方法放在类AbstractCurrency中？这看起来可疑，至少有两个原因。首先，如果你有一定数量的货币(比如，1美元)，你还拥有制造更多相同货币的能力，像这样：

```scala
myDollar.make(100) //违法行为
```

这段代码的第二个问题是，如果已经有了对Currency对象的引用，则可以创建更多Currency对象。但是，如何获得给定Currency的第一个对象呢？需要另一个创建方法，它的工作与make基本相同。

当然，解决方案是将抽象类型和工厂方法移到类AbstractCurrency之外。需要创建另一个包含AbstractCurrency类、Currency类型和工厂方法make。这个类名为CurrencyZone:

```scala
abstract class CurrencZone{
  type Currency <: AbstractCurrency
  def make(x: Long): Currency
  abstract class AbstractCurrency{
    val amount: Long
    def designation: String
    override def toString = amount + " " + designation
    def + (that: Currency): Currency = make(this.amount + that.amount)
    def * (x: Double): Currency = make((this.amount * x).toLong)
  }
}
```

一个具体的例子是US，可以定义为:

```scala
object US extends CurrencyZone{
  abstract class Dollar extends AbstractCurrency{
    def designation = "USD"
  }
  type Currency = Dollar
  def make(x: Long) = new Dollar{val amount = x}
}
```

在US中money的类型为US.Dollar，US对象还将类型Currency固定为Dollar的别名，它给出了一个make工厂方法的实现，以返回美元数量。

到目前为止，每一种货币都以单一的单位计量：美元、欧元或日元。但是大多数货币都有子类，比如美分(cents)。最直接的方式在US中定义美分字段。这个字段代表美分而不是美元。要把它转换回美元，在类CurrencyZone中引入一个字段CurrencyUnit是很有用的，它包含了该货币的一个标准单位的金额:

```scala
class CurrencyZone{
  ...
  val CurrencyUint: Currency
}
```

可以定义完整的US类：

```scala
object US extends CurrencyZone{
  abstract class Dollar extends AbstractCurrency{
    def designation = "USD"
  }
  type Currency = Dollar
  def make(cents: Long) = new Dollar{
    val amount = cents
  }
  val Cents = make(1)
  val Dollar = make(100)
  val CurrencyUnit = Dollar
}
```

和前面定义的US类似，另外增加了三个字段Cents, Dollar和CurrencyUnit。字段Cents表示1 US.Currency，相当于一美分的硬币之类的。字段Dollar表示100 US.Currency。

- `type Dollar`(由名为Dollar的抽象内部类定义)表示在对象US中Currency的通用名称；
- `val Dollar`是US中定义个一个val变量，与类型Dollar是不同的；
- `CurrencyUnit`就是US中标准货币单位，就是`val Dollar`，而不是`type Dollar`。

Currency类中的toString方法也需要调整以考虑子类，如美分。例如，10美元23美分的总和应该打印成一个小数：`10.23 USD`。为此，可以实现toString方法，如下所示：

```scala
override def toString =
  ((amount.toDouble / CurrencyUnit.amount.toDouble)
   formatted ("%." + decimals(CurrencyUnit.amount) + "f")
   + " " + designation)
```

formatted方法返回格式化原始字符串后产生的字符串，该字符串是根据作为格式化方法的右操作数传递的格式化字符串调用的，与Java的String.format的方法一样。在toString中使用的格式化字符串是对CurrencyUnit.amount调用decimals方法，返回一个数值的十进制位数减1，decimals(10)就返回1，decimals(100)就返回2，以此类推。decimals的定义如下：

```scala
private def decimal(n: Long): Int = {
  if (n == 1) 0 else 1 + decimals(n / 100)
}
```

下面定义了其他币种：

```scala
object Europe extends CurrencyZone{
  abstract class Euro extends AbstractCurrency{
    def designation = "EUR"
  }
  type Currency = Euro
  def make(cents: Long) = new Euro{
    val amount = cents
  }
  val Cent = make(1)
  val Euro = make(100)
  val CurrencyUnit = Euro
}

object Japan extends CurrencyZone{
  abstract class Yen extends AbstractCurrency{
    def designation = "JPY"
  }
  type Currency = Yen
  def make(yen: Long) = new Yen{
    val amount = yen
  }
  val Yen = make(1)
  val CurrencyUnit = Yen
}
```

要实现货币转换，首先编写转换器Converter：

```scala
object Converter {
  var exchangeRate = Map(
    "USD" -> Map("USD" -> 1.0, "EUR" -> 0.7596,
                 "JPY" -> 1.211, "CHF" -> 1.223),
    "EUR" -> Map("USD" -> 1.316, "EUR" -> 1.0,
                 "JPY" -> 1.594, "CHF" -> 1.623),
    "JPY" -> Map("USD" -> 0.8257, "EUR" -> 0.6272,
                 "JPY" -> 1.0, "CHF" -> 1.018),
    "CHF" -> Map("USD" -> 0.8108, "EUR" -> 0.6160,
                 "JPY" -> 0.982, "CHF" -> 1.0 )
  )
}
```

然后，可以为Currency添加一个转换方法from，该方法将给定的源货币转换为当前的Currency对象：

```scala
def from(other: CurrencyZone#AbstractCurrency): Currency = {
  make(math.round(other.amount.toDouble * Converter.exchangeRate(other.designation)(this.designation)))
}
```

from方法可以输入任意的货币，这是通过它的形参类型来表示的，`CurrencyZone#AbstractCurrency`，这表明输入的参数other必须是某个任意未知CurrencyZone的AbstractCurrency类型。

最终CurrencyZone的定义为：

```scala
abstract class CurrencyZone{
  type Currency <: AbstractCurrency
  def make(x: Long): Currency

  abstract class AbstractCurrency{
    val amount: Long
    def designation: String
    def + (that: Currency): Currency = make(this.amount + that.amount)
    def * (x: Double): Currency = make((this.amount * x).toLong)
    def - (that: Currency): Currency = make(this.amount - that.amount)
    def / (that: Double) = make((this.amount / that).toLong)
    def / (that: Currency) = this.amount.toDouble / that.amount
    def from (other: CurrencyZone#AbstractCurrency): Currency = {
      make(math.round(other.amount.toDouble * Converter.exchangeRate(other.designation)(this.designation)))
    }
    private def decimals(n: Long): Int = {
      if (n == 1) 0 else decimals(n / 10) + 1
    }
    override def toString = {
      ((amount.toDouble / CurrencyUnit.amount.toDouble) formatted ("%." + decimals(CurrencyUnit.amount) + "f") + " " + designation)
    }
  }
  val CurrencyUnit: Currency
}
```

测试如下：

```scala
scala> Japan.Yen from US.Dollar * 100
res3: Japan.Currency = 12110 JPY

scala> Europe.Euro from res3
res4: Europe.Currency = 75.95 EUR

scala> US.Dollar from res4
res5: US.Currency = 99.95 USD

scala> US.Dollar * 100 + res5
res6: US.Currency = 199.95 USD

scala> US.Dollar + Europe.Euro
<console>:14: error: type mismatch;
 found   : Europe.Euro
 required: US.Currency
    (which expands to)  US.Dollar
       US.Dollar + Europe.Euro
                          ^
```

## 21 隐式转换和隐式参数

### 21.1 隐式转换

隐式转换通常在处理两个在开发是完全不知道对方存在的软件或类库时非常有用。他们各自都有自己的方式来描述某个概念，而这个概念在本质上是同一件事情。`隐式转换可以减少以从一个类型显式转换成另一个类型的需要`。

Java包含一个名为Swing的类库来实现跨平台的用户界面，主要是处理操作系统的事件，将他们转换为平台独立的对象，并将这些事件传给事件监听器的应用代码。对于监听器，接口是ActionListener。如果没有隐式转换，使用到Swing的Scala程序就必须像Java那样使用内部类。

这里创建一个按钮并挂上一个动作监听器的例子，每当按钮按下，这个动作监听器就会被调用：

```scala
val button = New JButton
button.addActionListener(
  new ActionListener{
    def actionPerformed(event: ActionEvent) = {
      println("pressed")
    }
  }
)
```

代码中有大量不增加有用信息的样板代码，这里唯一有用的是println语句。对Scala更友好的版本应该接受函数作为入参，大幅地减少样板代码：

```scala
button.addActionListener( //类型不匹配
  (_: ActionEvent) => println("pressed!")
)
```

但是这个代码并不能正常工作（Scala 2.13中可以），addActionListener方法想要的是一个动作监听器，而我们给他的是一个函数，而通过隐式转换，这段代码是可行的。

第一步：编写两个类型之前的隐式转换函数，这里是一个从函数到动作监听器的隐式转换：

```scala
implicit def function2ActionListener(f: ActionEvent => Unit) = {
  new ActionListener{
    def actionPerformed(event: ActionEvent) = f(event)
  }
}
```

定义了一个单参数方法接收一个函数并返回一个动作监听器，可以被直接调用：

```scala
button.addActionListener(
  function2ActionListener(
    (_: AvtionEvent) => println("pressed!")
  )
)
```

相比于内部类，那些样板代码被一个函数字面量和方法调用替换掉了，不过用隐式转换，还能做得更好。`由于function2ActionListener被标记为隐式的，可以不写出这个调用，编译器会自动插入`：

```scala
button.addActionListener(
  (_: ActionEvent) => println("pressed!")
)
```

这段代码会按照原样编译，不过会遇到一个类型错误，编译器在放弃之前，会查找一个能修复该问题的隐式转换，在本例中，编译器找到了function2ActionListener，它会尝试这个隐式转换，发现可行，就继续下去。

### 21.2 隐式规则

`隐式定义是指那些我们允许编译器插入程序已解决类型错误的定义`。例如，如果x + y不能通过编译，那么编译器可能会把它改成convert(x) + y，其中convert可能就是某种隐式转换。如果convert将x改成某种支持+方法的对象，那么这个改动就能修复这个程序。

隐式转换受如下规则的约束：

`标记规则：只有标记为implicit的定义才可用`。关键字implicit用来标记那些声明可变编译器用作隐式定义。可以标记任何变量、函数或对象定义：

```scala
implicit def intToString(x: Int) = x.toString
```

编译器只会在convert被标记为implicit时，才将x + y修改成convert(x) + y。

`作用域规则：被插入的隐式转换必须是当前作用域的单个标识符，或者跟隐式转换的源类型或目标类型有关联`。Scala编译器只会考虑那些在作用域内的隐式转换。因此，必须以某种方式将隐式转换引入到当前作用域才能使用它们。不仅如此，`隐式转换在当前作用域必须是单个标识符`。编译器不会插入`someVariable.convert`这种形式的转换。如果想要`someVariable.convert`可用，必须引入它，成为单个标识符，引入后就可用。常见的做法是提供一个包含了一些有用隐式转换的Preamble对象。这样使用这个类库的代码就可以通过`import Preamble._`来访问该类库的隐式转换。

此外，单标识符有个例外，`编译器还会在隐式转换的源类型或目标类型的伴生对象中查找隐式定义`。如果尝试将Dollar对象传给Euro，可以将一个从Dollar到Euro的隐式转换打包在Dollar或Euro任何一个类的伴生对象中：

```scala
object Dollar{
  implicit def dollarToEuro(x: Dollar): Euro = ...
}
class Dollar{...}
```

**作用域规则有助于模块化的推理**，如果隐式转换是全局可用的，那么要理解某个代码的文件，就需要知道在程序的任何地方添加的每个隐式定义！

`每一次规则：每次只能有一个隐式定义被插入`。编译器绝对不会将x + y重写为convert1(convert2(x)) + y。`如果编译器已经尝试某个隐式转换的过程当中，它不会在尝试另一个隐式转换的`。不过可以让隐式转换定义包含隐式参数的方式绕过这个限制。

`显式优先原则：只要代码按编写的样子能通过类型检查，就不会尝试隐式定义`。我们总是可以将隐式转换标识符替换成显式的，代码会更长但歧义更少。

#### 21.2.1 命名一个隐式转换

隐式转换可以使用任何名称，隐式转换的名称只在两种情况下重要：

- 当你想在方法引用中显式地写出来；
- 为了决定在程序中的某个位置都有哪些隐式转换可用。

下面定义一个包含两个隐式转换的对象：

```scala
object MyConversions{
  implicit def stringWrapper(s: String): IndexedSeq[Char] = ...
  implicit def intToString(x: Int): String = ...
}
````

如果只需要其中一个隐式转换，可以只导入一个：

```scala
import MyConversions.stringWrapper
```

#### 21.2.2 在哪些地方会尝试隐式转换

Scala总共有三个地方会用到隐式转换：

- 转换到一个预期的类型；
- 对某个（成员）选择接收端（即方法、方法调用等）的转换；
- 隐式参数。

到期望类型的转换可以在预期不同类型的上下文中使用（当前已持有）某个类型。例如，有一个String但想将它传给一个要求IndexSeq[Char]的方法，选择接收端的转换让我们适配方法调用的接收端（即方法调用的对象），如果原始类型不支持这个调用，例如`"abc".exists`，这段代码会被转换为`stringWrapper("abc").exists`，因为exists在String上不可用，但是在IndexSeq[Char]可用。

隐式参数通常用来给调用的函数提供更多关于调用者诉求的信息。隐式参数对于泛型函数尤其有用。

### 21.3 隐式转换到一个预期的类型

转换到预期的类型，就是每当编译器看见一个X而它需要一个Y的时候，它就会查找一个能将X转换为Y的隐式转换。通常一个浮点型不能被用作整数，因为这样会丢失精度：

```scala
scala> val i: Int = 3.5
<console>:11: error: type mismatch;
 found   : Double(3.5)
 required: Int
       val i: Int = 3.5
                    ^
```

但是定义一个隐式转换就可以，但是会抛出一个告警：

```scala
scala> implicit def doubleToInt(x: Double) = x.toInt
<console>:11: warning: implicit conversion method doubleToInt should be enabled by making the implicit value scala.language.implicitConversions visible.
This can be achieved by adding the import clause 'import scala.language.implicitConversions' or by setting the compiler option -language: implicitConversions.
See the Scaladoc for value scala.language.implicitConversions for a discussion
why the feature should be explicitly enabled.
       implicit def doubleToInt(x: Double) = x.toInt
                    ^
doubleToInt: (x: Double)Int

scala> val i: Int = 3.5
i: Int = 3
```

代码在幕后变成了：

```scala
val i: Int = doubleToInt(3.5)
```

这是一个隐式转换，因为你并没有显式地要求这样一个转换，而是通过将doubleToInt作为单个标识符纳入到当前作用域来，并它标记为可用的隐式转换，这样编译器就会在需要将Double转换成Int自动使用它。

由Double转为Int这其实不符合常理，因为会丢失精度。但是Scala中的scala.Predef这个每个Scala程序都隐式引入的对象定义了那些从“更小”的数值类型向“更大”的数值类型的隐式转换，比如在Predef中可以找到这样的定义：

```scala
implicit def int2double(x: Int): Double = x.Double
```

这就是为什么Scala的Int值可以保存到类型为Double的变量中。类型系统当中并没有特殊的规则，这只不过是一个被（编译器）应用的隐式转换而已。

### 21.4 转换接收端

隐式转换还能应用于方法调用的接收端，也就是被调用的那个对象。这种隐式转换有两个主要用途。

- 首先，接收端转换允许我们更平滑将心累集成到已有的类继承关系图谱中；
- 其次，它们支持在语言中编写（原生的）领域特定语言（DSL）。

如果我们写下`obj.doIt`，但是obj并没有doIt成员，编译器会在放弃之前尝试插入转换。这个转换需要应用到接收端，也就是obj，编译器会装作obj的预期“类型”为拥有名为doIt的成员，这个拥有名为doIt的成员类型并不是一个普通的Scala类型，从概念上讲它是不存在的，这就是为什么编译器选择在这种情况下插入一个隐式转换。

#### 21.4.1 与新类型互操作

接收端转换的一个主要用途就是**让新类型和已有类型的继承更加顺滑**。尤其是这些转换使得我们可以让使用方程序员像使用新类型那样使用已有的类型，以Rational为例：

```scala
class Rational(n: Int, d: Int){
  def + (that: Rational): Rational = ...
  def + (that: Int): Rational = ...
}
```

可以实现的操作有两个有理数加法，或者一个有理数和一个整数相加：

```scala
val oneHalf = new Rational(1, 2)

oneHalf + oneHalf
oneHalf + 1
```

其中并没定义像实现`1 + oneHalf`这样的方法。为了实现这样的操作，需要定义一个从Int到Rational的隐式转换：

```scala
implicit def intToRational(x: Int) = new Rational(x, 2)
```

这样就可以实现`1 + oneHalf`的操作，在实现时，编译器会交出如下代码：

```scala
intToRational(1) + oneHalf
```

#### 21.4.2 模拟新的语法

隐式转换的另一个用途是模拟新的语法，比如我们之前定义了一个Map：

```scala
Map(1 -> "One", 2 -> "Two", 3 -> "Three")
```

有没有想过Scala是如何实现`->`操作的，这并不是语法特性！`->`是ArrowAssoc类的方法，ArrowAssoc是一个定义在scala.Predef对象这一个Scala标准前导代码(preamble)中的类，当你卸下`1 -> "One"`时，编译器会插入一个从1到ArrowAssoc的转换，以便`->`能被找到：

```scala
package scala
object Predef{
  class ArrowAssoc[A](x: A){
    def -> [B](y: B): Tuple2[A, B] = Tuple2(x, y)
  }
  implicit def any2ArrowAssoc[A](x: A): ArrowAssoc[A] = {
    new ArrowAssoc(x)
  }
}
```

这个“富包装类”模式再提供类语法(syntax-like)的扩展的类库中十分常见。只要你看见有人调用了接收类中不存在的方法，那么很可能使用了隐式转换。

#### 21.4.3 隐式类

Scala 2.10引入了隐式类来简化富包装类的编写。隐式类以implicit开头，对于这样的类，编译器会生成一个从类的构造方法参数到类本身的隐式转换。如果打算用这个类来实现富包装类模式，那么它正是你所需要的。

如果有一个名为Rectangle的类用来表示屏幕上一个长方形的宽和高：

```scala
case class Rectangle(width: Int, height: Int)
```

用富包装类来简化工作：

```scala
implicit class RectangleMaker(width: Int){
  def x(height: Int) = Rectangle(width, height)
}
```

会伴随上述代码自动生成：

```scala
implicit def Rectangle(width: Int) = {
  new RectangleMaker(width)
}
```

当使用`x`来创建点时：

```scala
scala> val myRectangle = 3 x 4
myRectangle: Rectangle = Rectangle(3,4)
```

因为Int类型并没有名为`x`的方法，编译器会查找一个从Int到某个有这个方法的类型的隐式转换。它将找到自动生成的这个RectangleMaker的转换。

**注意**：`implicit不能用在样例类(case class)上，并且其构造方法必须有且仅有一个参数`。不仅如此，隐式类必须存在于另一个对象、类和特质里。在实际应用中，只要是隐式类作为富包装类来给某个已有的类添加方法，这些限制应该都不是问题。

### 21.5 隐式参数

编译器有时将`someCall(a)`替换为`someCall(a)(b)`，或者将`new Some(a)`替换为`new Some(a)(b)`，通过追加一个参数列表的方式来完成某个函数调用。隐式参数提供的是整个最后一组柯里化的参数列表，而不仅仅是最后一个参数。如果someCall缺失的最后一个参数列表接收三个参数，那么编译器会将`someCall(a)`替换成`someCall(a)(b, c, d)`，就这个用法而言，**b,c,d三个参数还必须定义为implicit**。

假如有一个PreferredPrompt类，封装了一个用户偏好的命令行提示字符串($或者>)：

```scala
class PreferredPrompt(val preference: String)
```

同时，假如有一个带有greet方法的Greeter对象，greet接收两个参数列表，第一个参数列表接收一个字符创作为用户名，而第二个参数列表接收一个PreferredPrompt：

```scala
object Greeter{
  def greet(name: String)(implicit prompt: PreferredPrompt) = {
    println("Welcome, " + name + ". The system is ready.")
    println(prompt.preference)
  }
}
```

显式的给出prompt：

```scala
scala> val bobsPrompt = new PreferredPrompt("relax> ")
bobsPrompt: PreferredPrompt = PreferredPrompt@8ce3f27

scala> Greeter.greet("Bob")(bobsPrompt)
Welcome, Bob. The system is ready.
relax>
```

要让编译器帮你填充prompt，必须首先定义这样一个符合预期类型的变量：

```scala
object JoesPrefs{
  implicit val prompt = new PreferredPrompt("Yes, master> ")
}
```

然后直接使用，发现编译器会抛出一个错误：

```scala
scala> Greeter.greet("Joe")
<console>:15: error: could not find implicit value for parameter prompt: PreferredPrompt
       Greeter.greet("Joe")
                    ^
```

需要先将它带到作用域内：

```scala
scala> import JoesPrefs._
import JoesPrefs._

scala> Greeter.greet("Joe")
Welcome, Joe. The system is ready.
Yes, master>
```

注意：`implicit关键字是应用到整个参数列表，而不是单个参数的`。例如：

```scala
class PreferredPrompt(val preference: String)
class PreferredDrink(val preference: String)

object Greeter{
  // prompt和drink都是隐式参数
  def greet(name: String)(implicit prompt: PreferredPrompt, drink: PreferredDrink) = {
    println("Welcome, " + name + ". The system is ready.")
    println("But while you work, ")
    println("why not enjoy a cup of " + drink.preference + "?")
    println(prompt.preference)
  }
}

object JoesPrefs{
  implicit val prompt = new PreferredPrompt("Yes, master> ")
  implicit val drink = new PreferredDrink("tea")
}
```

定义完之后如果不引入，同样不能使用：

```scala
scala> Greeter.greet("Joe")
<console>:19: error: could not find implicit value for parameter prompt: PreferredPrompt
       Greeter.greet("Joe")
                    ^

scala> import JoesPrefs._
import JoesPrefs._
```

引入后，可以显式给出隐式参数，也可以省略：

```scala
scala> Greeter.greet("Joe")
Welcome, Joe. The system is ready.
But while you work,
why not enjoy a cup of tea?
Yes, master>

scala> Greeter.greet("Joe")(prompt, drink)
Welcome, Joe. The system is ready.
But while you work,
why not enjoy a cup of tea?
Yes, master>
```

例子中的关键点在于：我们并没有用String作为prompt和drink的类型，尽管它们最终都是通过preference字段提供了这样的String。由于编译器在选择隐式参数时是通过作用域内的值的类型做参数类型匹配，隐式参数通常都采用那些“足够”、“稀有”或者“特别“的类型，当值以外的匹配。

关于隐式参数的问题是，它们可能最常使用的场景是提供关于`更靠前`的参数列表中已经“显式”地提到的类型的信息。

看下面一个例子：

```scala
def maxListOrdering[T](elements: List[T])(ordering: Ordering[T]): T = elements match{
  case List() => throw new IllegalArgumentException("empty list!")
  case List(x) => x
  case x :: rest =>
    val maxRest = maxListOrdering(rest)(ordering)
    if (ordering.gt(x, maxRest)) x else maxRest
}
```

maxListOrdering除了接收一个List[T]作为入参，还接收一个额外的类型为Ordering[T]的入参，这个额外的参数是给出在比较T类型元素是应该使用的顺序。

这个定义更加通用，但是也比较麻烦，现在调用者必须显式地给出排序，哪怕是当T为Int或者String这样有明确的默认排序的时候。为了让新的方法更方便使用，可以将第二个参数标记为隐式：

```scala
def maxListImpParm[T](elements: List[T])(implicit ordering: Ordering[T]): T = elements match{
  case List() => throw new IllegalArgumentException("empty list!")
  case List(x) => x
  case x :: rest =>
    val maxRest = maxListImpParm(rest)(ordering)
    if (ordering.gt(x, maxRest)) x else maxRest
}
```

maxListImpParm函数是一个隐式参数，用来提供关于在更靠前的参数列表中已经显式提到的类型的更多信息的例子。确切地说，类型为Ordering[T]的隐式参数ordering提供了更多关于类型T的信息。类型T在elements参数的类型List[T]中提到过，这是一个更靠前的参数列表中的参数。在任何maxListImpParm调用中elements都必须显式地给出，编译器在编译时就知道T是什么，因此就可以确定类型为Ordering[T]的隐式定义是否可用。

实际使用如下：

```scala
scala> maxListImpParm(List(1, 5, 10, 3))
res6: Int = 10

scala> maxListImpParm(List(1.5, 5.2, 10.7, 3.1415))
res7: Double = 10.7

scala> maxListImpParm(List("One", "Two", "Three"))
res8: String = Two
```

#### 21.5.1 隐式参数的代码风格规则

从代码风格而言，`最好是对隐式参数使用定制名称的类型`。如前面的ptompt和drink用的并不是String，而分别是PreferredPrompt和PreferredDrink。在此maxListImpParm函数也可以用下面的类型签名来写：

```scala
def maxListPoorStyle[T](elements: List[T])(implicit orderer: (T, T) => Boolean): T
```

不过对于使用方而言，这个版本的函数需要提供类型为`(T, T) => Boolean`的参数orderer，这是一个相当泛化的类型。涵盖了所有从两个T到Boolean的函数。

### 21.6 上下文界定

使用implicit时，编译器不仅会尝试给这个参数`提供`一个隐式值，还会把这个参数当做一个可以在方法体中`使用`的隐式定义！也就是说，可以省去方法体中对ordering的第一次使用。

```scala
def maxList[T](elements: List[T])(implicit ordering: Ordering[T]): T = elements match{
  case List() => throw new IllegalArgumentException("empty list!")
  case List(x) => x
  case x :: rest =>
    val maxRest = maxList(rest) //这里会自动添加(ordering)
    if (ordering.gt(x, maxRest)) x else maxRest //依然显式给出ordering
}
```

还有一种方法可以去掉对ordering的第二次使用，这涉及标准库中定义的如下方法：

```scala
def implicitly[T](implicit t: T) = t
```

调用`implicitly[Foo]`的作用，是编译器查找一个类型为Foo的隐式定义，然后会用这个对象来调用implicitly方法。这个方法再将这个对象返回。这样就可以在想要当前作用域找到类型为Foo的隐式对象时直接写implicitly[Foo]。下面展示了implicitly[Ordering[T]]来通过其类型获取ordering参数的用法。

```scala
def maxList[T](elements: List[T])(implicit ordering: Ordering[T]): T = elements match{
  case List() => throw new IllegalArgumentException("empty list!")
  case List(x) => x
  case x :: rest =>
    val maxRest = maxList(rest)
    if (implicitly[Ordering[T]].gt(x, maxRest)) x else maxRest
}
```

通过这个定义，参数列表中的ordering可以换成其他名字。

由于这个模式很常用，Scala允许我们省掉这个参数的名称并使用`上下文界定(context bound)`来缩短方法签名。通过上下文界定，`[T: Ordering]`这样的语法是一个上下文界定，它做了两件事：首先，`像平常一样引入类型参数T`；其次，`它添加了类型为Ordering[T]的隐式参数`。

```scala
def maxList[T: Ordering[T]](elements: List[T]): T = elements match{
  case List() => throw new IllegalArgumentException("empty list!")
  case List(x) => x
  case x :: rest =>
    val maxRest = maxList(rest)
    if (implicitly[Ordering[T]].gt(x, maxRest)) x
    else maxRest
}
```

如果写下`[T <: Ordering[T]]`，实际上说T是一个Ordering[T]，相对而言，如果写的是`[T: Ordering]`，那么并没有说T是什么，而是说T带有某种形式的排序，从这个角度出发，上下文界定很有用。它允许我们的代码“要求”某个类型支持排序，但并不要哪个类型的定义。

### 21.7 当有多个转换可选时

当当前作用域中有多个隐式转换满足要求，大部分场合编译器都会拒绝插入转换。隐式转换在这个转换显而易见且纯粹是样板代码的时候最好用。如果有多个隐式转换可选，选哪一个就不那么明显。

这里一个简单的例子：

```scala
scala> def printLength(seq: Seq[Int]) = println(seq.length)
printLength: (seq: Seq[Int])Unit

scala> implicit def intToRange(i: Int) = 1 to i
intToRange: (i: Int)scala.collection.immutable.Range.Inclusive

scala> implicit def intToDigits(i: Int) = i.toString.toList.map(_.toInt)
intToDigits: (i: Int)List[Int]

scala> printLength(12)
<console>:25: error: type mismatch;
 found   : Int(12)
 required: Seq[Int]
Note that implicit conversions are not applicable because they are ambiguous:
 both method intToRange of type (i: Int)scala.collection.immutable.Range.Inclusive
 and method intToDigits of type (i: Int)List[Int]
 are possible conversion functions from Int(12) to Seq[Int]
       printLength(12)
                   ^
```

这里有两个隐式转换，编译器拒绝插入转换。

Scala 2.8以后规定，**如果可用的转换当中有某个转换严格来说比其他的转换更具体，那么编译器就会选择哪个更具体的转换**。比如一个接收String，另外一个接收Any，由于String是Any的子类，且比Any更具体，编译器会选择String的版本。

更确切地说，当满足下面任意一条时，就说一条比另一条具体：

- 前者的入参类型是后者入参类型的子类型；
- 两者都是方法，而前者所在的类扩展自后者所在的类。

### 21.8 调试

隐式定义是Scala的一项很强大的功能，不过有时也很难做对，这里有些小技巧。

有时会好奇为什么编译器没有找到你认为应该可以使用的隐式转换。这是将转换显式地写出来有助于解决问题。如果显式地写出来还是报错，你就知道为什么编译器不能应用你想要的隐式转换。

举例来说，假如你错误地将wrapString当做一个从String到Lists而不是IndexedSeq，你就会奇怪为什么如下代码不能工作：

```scala
scala> val chars: List[Char] = "xyz"
<console>:23: error: type mismatch;
 found   : String("xyz")
 required: List[Char]
       val chars: List[Char] = "xyz"
                               ^
```

这时，将wrapString显式地写出来有助于搞清楚是哪里错了：

```scala
scala> val chars: List[Char] = wrapString("xyz")
<console>:23: error: type mismatch;
 found   : scala.collection.immutable.WrappedString
 required: List[Char]
       val chars: List[Char] = wrapString("xyz")
                                         ^
```

当你调试一个程序是，看到编译器插入的隐式转换有时会有帮助，可以用`-Xprint:typer`这个编译器选项。如果你用这个选项运行scalac，编译器会告诉你类型检查其添加了所有隐式转换后你的代码是什么样子。

```scala
object Mocha extends App{
  class PreferredDrink(val preference: String)
  implicit val perf = new PreferredDrink("mocha")
  def enjoy(name: String)(implicit drink: PreferredDrink) = {
    print("Welcome, " + name)
    print(". Enjoy a ")
    print(drink.preference)
    println("!")
  }
  enjoy("reader")
}
```

使用`scalac -Xprint:typer mocha.scala`命令后，返回：

```scala
[[syntax trees at end of typer]] // mocha.scala
package <empty> {
  object Mocha extends AnyRef with App {
    def <init>(): Mocha.type = {
      Mocha.super.<init>();
      ()
    };
    class PreferredDrink extends scala.AnyRef {
      <paramaccessor> private[this] val preference: String = _;
      <stable> <accessor> <paramaccessor> def preference: String = PreferredDrink.this.preference;
      def <init>(preference: String): Mocha.PreferredDrink = {
        PreferredDrink.super.<init>();
        ()
        }
    };
    private[this] val perf: Mocha.PreferredDrink = new Mocha.this.PreferredDrink("mocha");
    implicit <stable> <accessor> def perf: Mocha.PreferredDrink = Mocha.this.perf;
    def enjoy(name: String)(implicit drink: Mocha.PreferredDrink): Unit = {
      scala.Predef.print("Welcome, ".+(name));
      scala.Predef.print(". Enjoy a ");
      scala.Predef.print(drink.preference);
      scala.Predef.println("!")
    };
    Mocha.this.enjoy("reader")(Mocha.this.perf)
  }
}
```

## 22 实现列表

### 22.1 List原理

List并不是Scala内建的语法结构，它们是由scala包里的周祥磊List定义的，这个抽象类有两个子类，`::`和`Nil`。本节对List的介绍跟Scala标准库中真正的实现相比有所简化：

```scala
package scala
abstract class List[+T]{
  ...
}
```

List是一个抽象类，因此不能通过调用空的List构造方法来定义元素：

```scala
scala> new List
<console>:12: error: class List is abstract; cannot be instantiated
       new List
       ^
```

这个类有一个类型参数T，声明为协变的。正因为这个特性，可以将类型为List[Int]的值赋值给类型为List[Any]的变量：

```scala
scala> val xs = List(1, 2, 3)
xs: List[Int] = List(1, 2, 3)

scala> var ys: List[Any] = xs
ys: List[Any] = List(1, 2, 3)
```

所有的列表操作都可以通过三个基本的方法来定义：

```scala
def isEmpty: Boolean
def head: T
def tail: List[T]
```

这些方法在List类中都是抽象的，它们的具体定义出现在`子对象Nil`和`子类::`中。继承关系如下：

```plain txt
         ┌───────────────────┐
         │      scala        │
         │     List[+T]      │
         │<<sealed abstract>>│
         └───────────────────┘
                   ▲
                   │
         ┌─────────┴─────────┐
         │                   │
┌────────────────┐ ┌─────────────────┐
│     scala      │ │      scala      │
│      ::T       │ │       Nil       │
│ <<final case>> │ │ <<case object>> │
└────────────────┘ └─────────────────┘
```

#### 22.1.1 Nil对象

Nil定义了一个空列表，Nil对象继承自类型List[Nothing]，因为是协变的的原因，这意味着Nil跟List类型的每个实力都兼容。

```scala
case object Nil extends List[Nothing]{
  override def isEmpty = true
  def head: Nothing = {
    throw new NoSuchElementException("head of empty list")
  }
  def tail: List[Nothing] = {
    throw new NoSuchElementException("tail of empty list")
  }
}
```

Nil对象中，isEmpty方法返回true，head和tail都返回异常。这里抛出的异常是合理的，而且实际上是head实现的唯一选择：`Nil是Nothing的List，head的结果类型也必须是Nothing`。由于没有任何值是这个类型的，head没法返回一个正常的值，它只能抛出异常的方式非正常的返回。

#### 22.1.2 ::类

`::类`表示空列表，之所以这样命名，是为了支持用中缀操作符实现模式匹配。之前在16.5中曾提到，模式匹配中的每个中缀操作符都被当做是用入参调用该中缀操作符对应的构造方法处理。因此`x :: xs`被处理为`xs.::(x)`，其中::是一个样例类：

```scala
case class ::[T](hd: T, tl: List[T]) extends List[T]{
  def head = hd
  def tail = tl
  override def isEmpty: Boolean = false
}
```

`::类`的实现很直接，接收两个参数hd和tl，分别表示要构建列表的头和尾。两个参数都是简单的返回对应的参数。事实上，这个模式可以用构造方法的参数直接实现超类的head和tail方法，进一步简化定义，就像下面这个等效但更简短`::类`定义：

```scala
final case ::[T](head: T, tail: List[T]) extends List[T] {
  override def isEmpty: Boolean = false
}
```

这样之所以可行，`因为样例类的每个参数都隐式地同时是这个类的字段`。

#### 22.1.3 更多方法

List的其他方法都可以用这三个基本方法编写。例如：

```scala
def length: Int = if (isEmpty) 0 else 1 + tail.length

def drop(x: Int): List[T] = {
  if (isEmpty) Nil
  else if (n <=0) this
  else tail.drop(n - 1)
}

def map[U](f: T => U): List[T] = {
  if (isEmpty) Nil
  else f(head) :: tail.map(f)
}
```

### 22.2 List的构造

列表的构造方法::和:::是特别的，因为它们都是以冒号结尾，它们会被绑定到右操作元上。

::方法应该接收一个元素值并交出一个新的列表。那么元素的类型是不是必须和列表元素类型一致？这样的要求事实上跟实际需要相比也许过于严格了。例如。考虑以下例子：

```scala
abstract class Fruit
class Apple extends Fruit
class Orange extends Fruit

scala> val apples = new Apple :: Nil
apples: List[Apple] = List(Apple@2ead6ba4)

scala> val fruits = new Orange :: apples
fruits: List[Fruit] = List(Orange@31723307, Apple@2ead6ba4)
```

apples值被当做Apple的List处理，不过，fruits的定义告诉我们仍然可以添加不同类型的元素到列表中。结果列表的类型是Fruit，这是原始列表元素（即Apple）和要添加的元素类型（即Orange）的最具体的公共超类型。

这种操作的成立，归功于::方法的定义：

```scala
def ::[U >: T](x: U): List[U] = new scala.::(x, this)
```

这个方法本身是多态的（接收一个名为U的类型参数）。不仅如此，`元素参数U受[U >: T]的约束，它必须是列表元素类型T的超类型，要添加的元素必须是类型U的值且结果是List[U]`。

上例中::的类型参数被实例化为Fruit。U的下界是满足的，因为apples这个列表的类型为List[Apple]而Fruit是Apple的超类。::的入参是new Orange满足类型为Fruit的要求，因此这个方法调用的正确结果类型为List[Fruit]。

```plain txt
    ┌──────┐
    │ head │──> Orange    ┌──────┐
    │ tail │─────────────>│ head │──> Apple
    └──────┘              │ tail │───────────> О
     fruits               └──────┘            Nil
                           apples
```

事实上，::多态定义中的下界T不仅为了方便，对于List类的类型正确而言是必要的，这是因为从定义上讲List是协变的。

假设有下面的定义：

```scala
def ::(x: T): List[T] = new scala.::(x, this)
```

19章讲过，方法参数会被当做逆变点，因此在上面的定义中，列表元素T位于逆变点，这样一来List就不能声明T是协变的。下界的定义实际上达到了两个目的：消除一个类型问题，同时让::方法用起来更灵活。列表拼接方法:::的定义跟::类似。

```scala
def :::[U >: T](prefix: List[U]): List[U] =
  if (prefix.isEmpty) this
  else prefix.head :: prefix.tail ::: this
```

列表拼接方法也是多态的，::和:::的方法可以展开为一下等效方法：

```scala
prefic.head :: prefix.tail ::: this
//等效于
prefix.head :: (prefix.tail ::: this)
//等效于
(prefix.tail ::: this).::(prefix.head)
//等效于
this.:::(prefix.tail).::(prefix.head)
```

### 22.2 ListBuffer类

对列表的典型访问模式是递归的。例如，要对某个列表的每个元素递增而不是用map：

```scala
def incAll(xs: List[Int]): List[Int] = xs match{
  case List() => List()
  case x :: xs1 => x + 1 :: incAll(xs1)
}
```

这个例子的缺陷是，它并不是尾递归的，每次递归调用都需要一个新的栈帧，在List较大的时候，可能会发生栈溢出的情况，如何编写一个支持任意大小的列表？一种方式是使用循环：

```scala
var result = List[Int]()
for (x <- xs) result = result ::: List(x + 1)
result
```

但这样写效率很低，:::的耗时跟首个操作元的长度成正比，整个操作的耗时跟列表的长度平方成正比。

更好的方案是使用`列表缓冲ListBuffer`，列表缓冲允许对列表的元素做累加，可以用诸如“buf += elem”的操作在列表缓冲buf尾部追加elem元素。可以用toList操作将缓冲转换成列表。

ListBuffer是scala.collection.mutable包里的一个类。使用之前需要导入：

```scala
import scala.collection.mutable.LustBuffer
```

通过ListBuffer，incAll的方法可以写成如下代码：

```scala
val buf = new ListBuffer[Int]
for (x <- xs) buf += x + 1
buf.toList
```

使用ListBuffer实现组织做到了追加操作（+=）和toList的操作都只消耗常量的时间。

### 22.3 List类的实践

List类大多数方法的实现并没有使用递归，而是通过循环和列表缓冲。例如List类的map方法的实现：

```scala
final override def map[U](f: T => U): List[U] = {
  val b = new ListBuffer[U]
  var these = this
  while (!these.isEmpty){
    b += f(these.head)
    these = these.tail
  }
  b.toList
}
```

尾递归的实现可以同样高效，但是一个普通的递归实现会更慢且伸缩性较差。toList方法的调用只用常量的时间，跟列表的长度无关。

首先，先看::类的实现，这个类构造非空列表：

```scala
final case class ::[U](hd: U, private[scala] val tl: List[U]) extends List[U]{
  def head = hd
  def tl = tl
  override def isEmpty: Boolean = false
}
```

入参tl被声明为var，这意味着列表创建之后，列表的尾部是可以被修改的，且有private[scala]修饰符，它只能在scala这个内部被访问，在这个包之外的代码既不能写也不能读这个变量。

由于ListBuffer是在scala.collection.mutable包中。ListBuffer可以访问列表单元格中的tl字段。事实上列表缓冲的元素就是用列表来表示的，而对列表缓冲追加元素，会涉及对列表最后一个::单元格的tl字段的修改，一下是ListBuffer类定义的开头部分：

```scala
package scala.collection.immutable
final class ListBuffer[T] extends Buffer[T]{
  private var start: List[T] = Nil
  private var last0: ::[T] = _
  private var exported: Boolean = false
  ...
}
```

ListBuffer有三个私有字段：

- start: 指向缓冲中保存的所有元素的列表
- last0: 指向该列表最后一个::单元格
- exported: 表示该缓冲是否已经通过toList转成了列表

toList操作非常简单：

```scala
override def toList: List[T] = {
  exported = !start.isEmpty
  start
}
```

它返回由start执行元素列表，并且（如果列表是非空的）将exported置为true。toList非常高效，因为它并不会对保存在ListBuffer中的列表进行拷贝，不过如果在toList操作之后继续对它进行变更会发生什么？不过一旦变为列表，它就必须是不可变的。

对last0的追加操作会修改start指向的列表。为了保持列表缓冲操作的正确性，需要一个全新的列表。实现方式是+=操作的第一行：

```scala
override def += (x: T) = {
  if (exported) copy()
  if (start.isEmpty){
    last0 = new scala.::(x, Nil)
    start = last0
  }else {
    val last1 = last0
    last0 = new scala.::(x, Nil)
    last1.tl = last0
  }
}
```

如果想要对列表尾部追加，就需要拷贝。不过，ListBuffer的实现方式确保了只有当列表缓冲被转成列表后还需要进一步扩展时，拷贝才是必要的。这在实际中很少见，裂变缓冲的大部分用例是逐个添加元素然后再做最后一次toList操作，这种情况不需要拷贝。

### 22.4 外部可见的函数式

列表从“外面”看是纯函数式的，而它的实现从“里面”看是指令式的。这个Scala的一个典型策略：`通过小心翼翼地界定非纯操作的作用试讲纯粹性和效率结合起来`。

为什么坚持纯粹性？如果将List中的head或者tail作为可变的，这样的做法的弊端是让程序更加脆弱。需要注意的是`当用::构造列表时，会复用构建出来的列表的尾部`:

```scala
val ys = 1 :: xs
val zs = 2 :: xs
```

列表ys和zs的尾部是共享的：它们指向相同的数据结构。这对于效率而言非常重要，如果每次添加新的元素都拷贝到列表xs，就会慢很多。由于到处都是共享的，如果允许改变列表元素，事情就会变得非常危险。

如果通过下面的代码将列表ys截断成前两个元素：

```scala
ys.drop(2).tail = Nil //在Scala中不能这样做
```

作为副作用，会同时截断zs和xs。显然，要跟中所有变更很困难，这就是为什么`Scala在列表的实现上尽量采纳了共享和不可变的原则`。

Scala中的List和ListBuffer的设计，跟Java中的String和StringBuffer类很相似，都想在保持纯的不可变的数据结构的同时提供一种高效的、渐进式的构造方式。

对Scala列表来说，要么选择使用::来在列表头部添加元素，要么使用ListBuffer在末尾添加元素，至于选择哪个要看具体的场景。

## 23 重访for表达式

第16章演示了高阶函数，如map、flatMap和filter，提供了处理列表的强大结构。但有时这些函数所要求的抽象级别会使程序有点难以理解。

下面定义了一个Person类和其中的字段：姓名，性别，子女：

```scala
case class Person(
  name: String,
  isMale: Boolean,
  children: Person*
)
```

然后给出一部分样例：

```scala
val lara = Person("Lara", false)
val bob = Person("Bob", true)
val julie = Person("Julie", false, lara, bob)
val persons = List(lara, bob, julie)

lara: Person = Person(Lara,false,WrappedArray())
bob: Person = Person(Bob,true,WrappedArray())
julie: Person = Person(Julie,false,WrappedArray(Person(Lara,false,WrappedArray()), Person(Bob,true,WrappedArray())))
persons: List[Person] = List(Person(Lara,false,WrappedArray()), Person(Bob,true,WrappedArray()), Person(Julie,false,WrappedArray(Person(Lara,false,WrappedArray()), Person(Bob,true,WrappedArray()))))
```

现在，想要找出所有的母亲和孩子，使用map、flatMap和filter，可能会这样写：

```scala
scala> persons filter (p => !p.isMale) flatMap(p => (p.children map (c => (p.name, c.name))))

res2: List[(String, String)] = List((Julie,Lara), (Julie,Bob))
```

可以使用withFilter代替filter来优化这段代码，可以避免为过滤女性创建的中间数据结构：

```scala
scala> persons withFilter (p => !p.isMale) flatMap(p => (p.children map(c => (p.name, c.name))))

res3: List[(String, String)] = List((Julie,Lara), (Julie,Bob))
```

这样写可以解决问题，但是这段代码并不那么容易读写，如果改用for表达式则会简单易于理解：

```scala
for (p <- persons; if !p.isMale; c <- p.children)
  yield (p.name, c.name)

res4: List[(String, String)] = List((Julie,Lara), (Julie,Bob))
```

两段代码的结果是一样的。代码的大多数读者可能会发现for表达式比前面的查询更清晰，前面的查询使用了更高阶函数map、flatMap和withFilter。

但是，两个查询并没有看起来那么不同。事实上，Scala编译器会将第二个查询转换为第一个查询。更一般地，`编译器会将所有for表达式和yield生成结果的表达式转换为高阶方法map、flatMap和withFilter的调用组合`。**所有的for循环无输出输出被转换成一个更小的高阶函数集：只有filter和foreach**。

如果写成花括号，则可以省去分号：

```scala
for {p <- persons
     if !p.isMale
     c <- p.children
} yield (p.name, c.name)

res5: List[(String, String)] = List((Julie,Lara), (Julie,Bob))
```

### 23.1 for表达式

通常来说，for表达式是这样的：

```scala
for (seq) yield expr
```

*`seq`* 是一个序列生成器、定义或者过滤器，表达式之前使用分号隔开，下面是一个例子：

```scala
for (p <- persons; n = p.name; if (n startWiths "To"))
  yield n
```

也可以写成：

```scala
for {p <- persons
     n = p.name;
     if (n startWiths "To")
} yield n
```

`每个for表达式都以生成器开始`。如果for表达式中有多个生成器，则后面的生成器比前面的生成器变化得更快。您可以通过下面的简单测试轻松地验证这一点：

```scala
scala> for (x <- List(1, 2); y<- List("One", "Two")) yield (x, y)

res6: List[(Int, String)] = List((1,One), (1,Two), (2,One), (2,Two))
```

### 23.2 N皇后问题

for表达式的一个特别适合的应用领域是组合谜题。这类谜题的一个例子是8个王后问题：给定一个标准棋盘，放置8个王后，使任何其他的王后都不受制约(王后可以吃掉另一王后，如果另一个王后在同一列、同一行或同一对角线上)。要找到这个问题的解决方案，实际上更简单的方法是将其推广到任意大小的棋盘上。因此，问题是如何定位棋盘上N×N个方格，N的大小是任意的。

我们将从1开始编号单元格，所以N×N块黑板的左上角单元格坐标为(1,1)，右下角单元格坐标为(N,N)。

要解决N皇后问题，请注意需要在每行中放置一个皇后。因此，可以将皇后放在连续的行中，每次检查新放置的皇后是否被已经放置的其他皇后吃掉。在这个搜索过程中，可能会出现这样的情况：需要放在第k行中的皇后会在第1行到k−1行的所有字段中被吃掉。在这种情况下，需要中止这部分搜索，以便在列1到k−1中继续使用皇后组的不同配置。

解决这个问题可以`使用递归算法`。假设已经生成了将k个皇后放到一个大小为N×N的板子上的所有解，其中k小于N。每个这样的解可以由长度为k的坐标列表表示(行、列)，其中行号和列号的范围都是1到N。

将这些部分解决方案列表视为堆栈很方便，其中k行中的皇后的坐标在列表中排在第一位，随后是k−1行中的q皇后，以此类推。堆栈的底部是放置在棋盘第一行的皇后的坐标。所有的解决方案一起被表示为一个列表的列表，列表中的每个元素都是一种解决方法。

现在，`为了在第k+1行中放置下一个皇后，需要为之前的每个解生成所有可能的位置，再增加一个皇后（按照一行一行进行构建）`。这会产生另一个解决方案列表，这次的长度是k+1。继续这个过程，直到你得到棋盘大小N的所有解。

实现代码如下：

```scala
//检查q1和q2是否在同一列，同一行或者对角线上：
def inCheck(q1: (Int, Int), q2: (Int, Int)) = {
  q1._1 == q2._1 ||
  q1._2 == q2._2 ||
  (q1._1 - q2._1).abs == (q1._2 - q2._2).abs
}

//检查新放置的皇后，和已有的皇后时候有被吃掉的可能：
def isSafe(queen: (Int, Int), queens: List[(Int, Int)]) = {
  queens forall (q => !inCheck(queen, q))
}

//定义一个方法返回所有的解，结果类型为列表的列表
def queens(n: Int): List[List[(Int, Int)]] = {
  def placeQueens(k: Int): List[List[(Int, Int)]] = {
    if (k == 0) List(List())
    else
      for{
        queens <-placeQueens(k - 1)
        column <- 1 to n
        queen = (k, column)
        if isSafe(queen, queens)
      } yield queen :: queens
  }
  placeQueens(n)
}
```

当n=8时，共有92个解，其中的部分解如下：

```scala
List(
    List((8,4), (7,2), (6,7), (5,3), (4,6), (3,8), (2,5), (1,1)),
    List((8,5), (7,2), (6,4), (5,7), (4,3), (3,8), (2,6), (1,1)),
    List((8,3), (7,5), (6,2), (5,8), (4,6), (3,4), (2,7), (1,1)),
    List((8,3), (7,6), (6,4), (5,2), (4,8), (3,5), (2,7), (1,1)),
    List((8,5), (7,7), (6,1), (5,3), (4,8), (3,6), (2,4), (1,2))
)
```

### 23.3 使用for表达式查询

for符号在本质上等同于数据库查询语言的通用操作。下面定义一个Book类和一个books的列表：

```scala
case class Book(title: String, anthors: String*)

val books: List[Book] =
  List(
    Book(
      "Structure and Interpretation of Computer Programs",
      "Abelson, Harold", "Sussman, Gerald J."
    ),
    Book(
      "Principles of Compiler Design",
      "Aho, Alfred", "Ullman, Jeffrey"
    ),
    Book(
      "Programming in Modula-2",
      "Wirth, Niklaus"
    ),
    Book(
      "Elements of ML Programming",
      "Ullman, Jeffrey"
    ),
    Book(
      "The Java Language Specification", "Gosling, James",
      "Joy, Bill", "Steele, Guy", "Bracha, Gilad"
    )
)

books: List[Book] = List(Book(Structure and Interpretation of Computer Programs,WrappedArray(Abelson, Harold, Sussman, Gerald J.)), Book(Principles of Compiler Design,WrappedArray(Aho, Alfred, Ullman, Jeffrey)), Book(Programming in Modula-2,WrappedArray(Wirth, Niklaus)), Book(Elements of ML Programming,WrappedArray(Ullman, Jeffrey)), Book(The Java Language Specification,WrappedArray(Gosling, James, Joy, Bill, Steele, Guy, Bracha, Gilad)))
```

查询并返回作者中以“Gosling”开头的所有书籍的名字：

```scala
scala> for {
  b <- books
  a <- b.authors
  if a startsWith "Gosling"
} yield b.title

res1: List[String] = List(The Java Language Specification)
```

或者查询包含“Program”的书籍：

```scala
scala> for {
  b <- books
  if (b.title indexOf "Program") >= 0
} yield b.title

res2: List[String] = List(Structure and Interpretation of Computer Programs, Programming in Modula-2, Elements of ML Programming)
```

或者查询一个人写了两本书：

```scala
scala> for{
  b1 <- books
  b2 <- books
  if b1 != b2
  a1 <- b1.authors
  a2 <- b2.authors
  if a1 == a2
} yield a1

res3: List[String] = List(Ullman, Jeffrey, Ullman, Jeffrey)
```

这种查询结果会返回重复的数据，可以移除重复的数据后再返回，先定义一个移除列表中重复数据的函数：

```scala
def removeDuplicates[A](xs: List[A]): List[A] = {
  if (xs.isEmpty) xs
  else
    xs.head :: removeDuplicates(
      xs.tail.filter(x => x != xs.head)
    )
}

scala> removeDuplicates(res3)

res13: List[String] = List(Ullman, Jeffrey)
```

和for表达式结合在一起：

```scala
xs.head :: removeDuplicates(
  for (x <- xs.tail if x != xs.head)
    yield x
)
```

### 23.4 翻译for表达式

每个for表达式都可以用三个高阶函数map、flatMap和withFilter来表示。本节描述Scala编译器也使用的转换方案。

#### 23.4.1 只有一个生成器

```scala
for (x <- expr1) yield expr2
```

可替换为：

```scala
exprs.map(x => expr2)
```

#### 23.4.1 只有一个生成器和一个过滤器

```scala
for (x <- expr1; if expr2) yield expr3
```

可替换为：

```scala
for (x <- expr1 withFilter (x => expr2)) yield expr3
```

此转换提供了另一个比原始表达式短一个元素的for表达式，因为在第一个生成器表达式上，if元素被转换为withFilter的应用程序。接着翻译第二个表达式，最后得到

```scala
expr1 withFilter (x => expr2) map (x => expr3)
```

这样的转换方案也适应于过滤器有更多元素的情况，如下：

```scala
for (x <- expr1 if expr2; seq) yield expr3
```

会被翻译为：

```scala
for (x <- expr1 withFilter expr2; seq) yield expr3
```

#### 23.4.2 有两个生成器

```scala
for (x <- expr1; y <- expr2; seq) yield expr3
```

同样，假设seq是生成器、定义和过滤器的任意序列。事实上，seq也可能是空的，在这种情况下，在expr2之后不会有分号。在这样的情况下，转换方案保持不变。上面的for表达式被转换为一个flatMap程序：

```scala
expr1.flatMap(x => for(y <- expr2; seq) yield expr3)
```

到目前为止，给出的三个翻译方案足以翻译仅包含生成器和过滤器以及生成器仅绑定简单变量的表达式。

下面，我们要找出所有至少写了两本书的作者：

```scala
for {b1 <- books;
     b2 <- books if b1 != b2;
     a1 <- b1.authors;
     a2 <- b2.authors if a1 == a2
} yield a1
```

此查询转换为以下map/flatMap/withFilter组合：

```scala
books flatMap(b1 =>
  books withFilter(b2 => b1 != b2) flatMap(b2 =>
    b1.authors flatMap(a1 =>
      b2.authors withFilter (a2 => a1 == a2) map(
        a2 => a1
      )
    )
  )
)
```

到目前为止，提供的转换方案还不能处理绑定整个模式而不是简单变量的生成器。它也没有涵盖定义。这两个方面将在接下来的两个小节中解释。

#### 23.4.3 翻译生成器中的模式

如果生成器的左侧不是一个简单的变量，而是一个模式：`pat`，那么转换方案就会变得更加复杂。for表达式绑定变量的元组的情况仍然相对容易处理。在这种情况下，适用于单变量的方案几乎相同。

```scala
for ((x1, x2, ..., xn) <- expr1) yield expr2
```

会翻译成：

```scala
expr1.map{case (x1, x2, ..., xn) => expr2}
```

如果生成器的左侧是一个任意的模式`pat`，而不是单个变量或元组，那么事情就会变得更加复杂。

```scala
for (pat <- expr1) yield expr2
```

会被翻译为：

```scala
expr1 withFilter {
  case pat => true
  case _ => false
} map{
  case pat => expr2
}
```

也就是说，首先过滤生成的项，并且只映射那些与pat匹配的项，这样模式匹配就不会抛出MatchError。

这里的方案只处理for表达式只有一个模式匹配生成器的情况。如果for表达式包含其他生成器、过滤器或定义，则应用类似的规则。

#### 23.4.4 翻译(for表达式中的)定义

最后一个缺失的情况是for表达式包含嵌入的定义类操作。这是一个典型的例子：

```scala
for (x <- exprs; y = expr2; seq) yield expr3
```

假定seq是生成器序列、定义或者过滤器（有可能为空），这样的表达式会被翻译为：

```scala
for ((x, y) <- for (x <- expr1) yield (x, expr2); seq) yield expr3
```

对于精通语法的人来说，结论是在不引用由前面的生成器绑定的变量的表达式中嵌入这样的定义可能不是一个好主意，因为重新计算这些表达式会造成浪费。但是，像y = expr2这样的定义最好是写在for表达式的外面，更加容易理解：

```scala
for (x <- 1 to 1000; y = expensiveComputationNotInvolvingX)
yield x * y
//改为

val y = expensiveComputationNotInvolvingX
for (x <- 1 to 1000) yield x * y
```

#### 23.4.5 翻译for循环

前面的子节展示了如何对包含yield的表达式进行翻译。如果循环只是执行一个副作用，而不返回任何东西，那该怎么办呢？与前面的翻译类似，但比表达式更简单。原则上，前面的转换方案在转换中使用map或flatMap，for循环的转换方案只使用foreach。

```scala
for (x <- expr1) body
```

会被翻译为：

```scala
expr1 foreach (x => body)
```

复杂一点的表达式：

```scala
for (x <- expr1; if expr2; y <- expr3) body
```

翻译为：

```scala
expr1 withFilter(x => expr2) foreach (x => expr3 foreach(y => body))
```

例如，下面的表达式将一个矩阵中的所有元素汇总为一个列表的列表：

```scala
var sum = 0
for (xs <- xss; x <- xs) sum += x
```

翻译为：

```scala
var sum = 0
xss foreach (xs => xs foreach (x => sum += x))
```

### 23.5 反过来

上一节演示了可以将for表达式转换为高阶函数map、flatMap和withFilter的应用。实际上，您也可以采用另一种方式:map、flatMap或filter的每个应用程序都可以表示为for表达式。

下面是这三个方法在for表达式中的实现。这些方法包含在对象演示中，以区别于对Lists的标准操作。具体地说，这三个函数都接收列表作为参数，但是转换模式也可以用于其他集合类型：

```scala
object Demon{
  def map[A, B](xs: List[A], f: A => B): List(B) = {
    for (x <- xs) yield f(x)
  }

  def flatMap[A, B](xs: List[A], f: A => B): List(B) = {
    for (x <- xs; y <- f(x)) yield y
  }

  def filter[A](xs: List[A], p: A => Boolean): List[A] = {
    for (x <- xs if p(x)) yield x
  }
}
```

### 23.6 泛化for表达式

因为for表达式的转换只依赖于方法map、flatMap和withFilter的存在，所以可以将for表示法应用于大量数据类型。

前面的例子中，将for表达式应用于列表和数组。它们之所以能够支持for表达式，是因为列表和数组定义了`map`、`flatMap`和`withFilter`等操作。由于它们还定义了`foreach`方法，所以这些数据类型也可以使用for循环。

除了列表和数组之外，Scala标准库中还有许多其他类型支持相同的4种方法，因此也允许使用for表达式。包括ranges、iterators、streams和sets的所有实现。自定义的数据类型也完全有可能通过定义必要的方法来支持for表达式。要支持完整范围的for表达式和for循环，需要将map、flatMap、withFilter和foreach定义为数据类型的方法。但是也可以定义这些方法的子集，从而支持for表达式或for循环的所有可能的子集。

下面是一些使用规则：

- 如果类型只定义map方法，则允许由单个生成器组成的表达式。
- 如果定义了flatMap和map，那么for表达式允许使用多个生成器。
- 如果定义了foreach，它就允许循环(具有单个和多个生成器)。
- 如果定义withFilter，则允许在for表达式中以if开头的过滤器表达式。

`for表达式的转换发生在类型检查之前`。这样可以获得最大的灵活性，因为唯一的要求是展开for表达式类型检查的结果。Scala没有为for表达式本身定义类型规则，也不要求方法map、flatMap、withFilter或foreach具有任何特定的类型签名。

不过，有一种典型的设置可以捕获用于for表达式转换的高阶方法的最常见意图。假设您有一个参数化的类C，它通常代表某种集合。就会很自然地为map、flatMap、withFilter和foreach选择以下类型签名：

```scala
abstract class C[A]{
  def map[B](f: A => B): C[B]
  def flatMap[B](f: A => C[B]): C[B]
  def withFilter(p: A => Boolean): C[A]
  def foreach(b: A => Unit): Unit
}
```

## 24 深入集合类

Scala自带一个强大而优雅的集合类库，尽管这些集合API看上去没什么，它们对你的编程风格的影响可谓巨大。通常，就好比把整个集合而不是集合中的元素当做构建单元来组织上层逻辑，这种编程风格需要适应，但是Scala也有很过不错的特性，它们精简、易用、安全、快速而且通行。

### 24.1 可变与不可变集合

所有的集合类都可以在scala.collection包或它的子包mutable、immutable和generic中找到。大多数使用的集合类都分为三个变种。这三个变种分别位于scala.collection包、scala.collection.immutable，以及scala.collection.mutable中。

- scala.collection.immutable包中的集合对所有人都是不可变的，无论怎么访问都会交出相同元素的集合。

- scala.collection.mutable包中的集合有一些操作可以当场修改集合，这些操作允许来编写改变集合的代码。

- scala.collection包中的集合既可以是可变的，也可以是不可变的。例如，scala.collection.IndexSeq[T]是scala.collection.immutable.IndexedSeq[T]和scala.collection.mutable.IndexedSeq[T]的超类型。一般而言，scala.collection包中的根(root)集合定义了跟不变集合相同的接口，而通常，scala.collection.mutable包中的可变集合会在上述不可变接口的基础上添加一些有副作用的修改操作。

根集合和不可变集合的区别是，`根集合的使用方只知道自己不能修改这个集合，而不可变集合的使用方确认没人可以修改这个集合`。尽管根集合的静态类型没有提供修改集合的操作，它运行时类型仍有可能是一个可变集合，能够被使用方修改。

`Scala默认选择不可变集合`。如果需要使用可变集合，需要显式地导入。

集合类继承关系中最后一个包是collection.generic。这个包包含了那些用于实现集合的构建单元。通常，集合类会选择将部分操作交给generic中的类的实现来完成。集合框架的日常使用中并不需要引入generic包中的类。

### 24.2 集合一致性

下面给出了那些最重要的集合类，大部分类都有三个版本：`根、可变的和不可变的`，唯一例外的是`Buffer特质，它只在可变集合中出现`：

```scala
Traversable
  Iterable
    Seq
     IndexedSeq
       Vector
       ResizableArray
       GenericArray
    LinearSeq
       MutableList
       List
       Stream
    Buffer
       ListBuffer
       ArrayBuffer
  Set
    SortedSet
      TreeSet
    HashSet (mutable)
    LinkedHashSet
    HashSet (immutable)
    BitSet
    EmptySet, Set1, Set2, Set3, Set4
  Map
    SortedMap
       TreeMap
    HashMap (mutable)
    LinkedHashMap (mutable)
    HashMap (immutable)
    EmptyMap, Map1, Map2, Map3, Map4
```

每一种集合都可以使用相同的一致语法来创建：

```scala
Traversable(1, 2, 3)
Iterable("x", "y", "z")
Map("x" -> 24, "y" -> 25, "z" -> 26)
Set(Color.Red, Color.Green, Color.Blue)
SortedSet("hello", "world")
Buffer(x, y, z)
IndexedSeq(1.0, 2.0)
LinearSeq(a, b, c)
```

同样的原则也适用于特定的集合实现：

```scala
List(1, 2, 3)
HashMap("x" -> 24, "y" -> 25, "z" -> 26)
```

所有集合的toString方法也会产生相似的输出，类型名加上圆括号括起来的元素。`所有的集合都支持由Traversable提供的API`。不过它们的方法都返回自己的类而不是根类Traversable，这样返回的类型更加精确。

```scala
scala> List(1, 2, 3) map (_ + 1)
res0: List[Int] = List(2, 3, 4)

scala> Set(1, 2, 3) map(_ * 2)
res1: scala.collection.immutable.Set[Int] = Set(2, 4, 6)
```

### 24.3 Traversable特质

在继承类的顶端是Traversable特质，它唯一的抽象操作是foreach：

```scala
def foreach[U](f: Elem => U)
```

实现Traversable的集合类只需要定义这个方法即可，其他方法都可以从Traversable继承。

Traversable定义了很多具体方法：

- **添加++：** 可以将两个Traversable加在一起，或将某个迭代器的所有元素添加到Traversable。
- **映射操作：** map, flatMap和collect，通过对集合的元素应用某个函数来产生一个新的集合。

- **转换：** toIndexedSeq、toIterable、toStream、toArray、toList、toSeq、toSet和toMap，将一个Traversable集合转换为更具体的集合。如果原集合已经匹配了所需要的集合类型，所有的这些转换就会直接返回原集合。例如，对List使用toList操作，就会返回List本身。
- **拷贝操作：** copyToBuffer和copyToArray。
- **大小操作：** isEmpty、nonEmpty、size和hasDefiniteSize（表示集合是否有限大小）。
- **元素获取：** head、last、headOption、lastOption和find。
- **子集合获取操作：** takeWhile、tail、init、slice、take、drop、filter、dropWhile、filterNot和withFilter。
- **细分：** splitAt、span、partition和groupBy。
- **元素测试：** exists、forall和count。
- **折叠：** foldLeft、foldRight、/:、:\、reduceLeft和reduceRight，对连续的元素应用某个二元操作。
- **特殊折叠：** sum、product、min和max。
- **字符串操作：** mkString、addString和stringPrefix。
- **视图操作：** 由两个重载的view方法组成，视图是一个惰性求值的集合，将在后面介绍。

### 24.4 Iterable特质

该特质的所有方都是通过抽象方法iterator来定义的。这个抽象方法的作用是逐个交出集合的元素。从Traversable继承下来的foreach方法在Iterable中的定义就用到了iterator：

```scala
def foreach[U](f: Elem => U): Unit = {
  val it = iterator
  while (it.hadNext) f(it.next())
}
```

很多Iterable的子类都重写了这个在Iterable中的foreach标准实现，因为可以提供更高效的表现。

`Iterable还有两个方法迭代器：grouped和sliding。它们并不返回单个元素，而是原始集合的整个子序列`。grouped将元素分段，sliding交出的是对元素的一个滑动窗口：

```scala
scala> val xs = List(1, 2, 3, 4, 5)
xs: List[Int] = List(1, 2, 3, 4, 5)

scala> val git = xs grouped 3
git: Iterator[List[Int]] = <iterator>

scala> git.next()
res2: List[Int] = List(1, 2, 3)

scala> git .next()
res3: List[Int] = List(4, 5)

scala> val sit = xs sliding 3
sit: Iterator[List[Int]] = <iterator>

scala> sit.next()
res4: List[Int] = List(1, 2, 3)

scala> sit.next()
res5: List[Int] = List(2, 3, 4)

scala> sit.next()
res6: List[Int] = List(3, 4, 5)
```

Iterable还对Traversable添加了一些其他方法，这个方法只有在有迭代器存在的情况下才能得以高效地实现：

|操作|操作含义|
|--|--|
|**抽象方法：**||
|xs.iterator|按照与foreach遍历元素的顺序交出xs每个元素的迭代器|
|**其他迭代器:**||
|xs grouped size|交出固定大小“段”的迭代器|
|xs sliding size|交出固定大小滑动窗口的元素的迭代器|
|**子集合：**||
|xs takeRight n|包含xs后n个元素的集合（如果没有定义顺序，就是任意的n个元素）|
|xs dropRight n|集合除去xs takeRight n外的部分|
|**拉链：**||
|xs zip ys|去两个对应元素的对偶组成的Iterable|
|xs zipAll ys|用较短的序列用xs或者ys的元素值延展成相同的长度|
|xs.zipWithIndex|由xs中的元素及其下标的对偶组成的Iterable|
|**比较：**||
|xs sameElements ys|测试是否xs和ys包含顺序相同元素|

#### 24.4.1 为什么要同时有Traversable和Iterable

为什么要在Iterable上多加一个TRaversable特质，还要额外增加一层foreach而不是用iterator来定义其方法的抽象特质？`增加Traversable的原因之一是有时候提供foreach比提供iterator的实现更加容易`。

假如要定义一个二叉树的类继承关系，其中叶子节点的元素是整数类型的，可能会这样编写：

```scala
sealed abstract class Tree
case class Branch(left: Tree, right: Tree) extends Tree
case class Node(elem: Int) extends Tree
```

现在要遍历这些二叉树，怎么做呢？可以让Tree继承自Traversable[Int]，然后像这样定义一个foreach方法：

```scala
sealed abstract class Tree extends Traversable[Int]{
  def foreach[U](f: Int => U) = this match{
    case Node(elem) => f(elem)
    case Branch(1, r) => l foreach f; r foreach f
  }
}
```

`遍历平衡树的耗时跟树中元素的数量成正比`，对于`N`个叶子的平衡树，会有`N-1`个Branch类的内部节点，因此遍历整棵树的步数为`N + N - 1`。

下面把树做成Iterable做对比，让Tree继承自Iterable[Int]，然后定义一个iterator方法：

```scala
sealed abstract class Tree extends Iterable[Int]{
  def iterator: Iterator[Int] = this match{
    case Node(elem) => Iterator.single(elem)
    case Branch(l, r) => l.iterator ++ r.iterator
  }
}
```

看上去和foreach类似，但这里的++操作有一个运行效率的问题，像l.iterator ++ r.iterator这样的拼接起来的迭代器，每交出一个元素，都需要多一层计算来判断使用的哪一个迭代器（l.iterator还是r.iterator）。总体而言，对于N个叶子的平衡树而言，需要log(N)次计算。因此访问某个数所有元素的成本从foreach遍历的大约`2N`次增加到iterator的`N*log(N)`。如果树的元素有100万个，那就意味着foreach大约需要200万步，而iterator需要将近2000万步，因此foreach更具优势。

#### 24.4.2 Iterable子类目

Iterable之下有三个特质：Seq、Set和Map。这些特质的共同特点是都实现了PartialFunction特质，只不过实现的方式略有不同。

对于Seq而言，apply是位置下标，元素的下标总是从0开始，例如：

```scala
scala> Seq(1, 2, 3)(1)
res0: Int = 2
```

对于Set而言，apply是成员测试，

```scala
scala> Set('a', 'b', 'c')('b')
res1: Boolean = true

scala> Set()('b')
res2: Boolean = false
```

对于Map而言，apply是选择：

```scala
scala> Map('a' -> 1, 'b' -> 2, 'c' -> 3)('b')
res3: Int = 2
```

### 24.5 序列型特质Seq, IndexedSeq, and LinearSeq

Seq特质代表序列，序列是一种有长度（length）且元素都有固定的从0开始的下表位置的迭代：

|操作|操作含义|
|--|--|
|**下边和长度:**||
|xs(i)|xs中下标为i的元素|
|xs isDefinedAt i|测试i是否包含在xs.indices中|
|xs.length|xs的长度|
|xs.lengthCompare ys|如果xs的长度大于ys则返回1，小于返回-1，等于返回0|
|xs.indices|xs索引的范围，从0到xs.length - 1|
|**下标检索:**||
|xs indexOf x|xs中首个等于x的下标|
|xs lastIndexOf x|xs中最后一个等于x的下标|
|xs indexOfSlice ys|xs中首个满足自该元素起的连续元素能够构成ys序列的下标|
|xs lastIndexOfSlice ys|xs中最后一个满足自该元素起的连续元素能够构成ys序列的下标|
|xs indexWhere p|xs中首个满足p的元素的下标|
|xs segmentLength (p, i)|xs中自xs(i)开始最长满足p的片段的长度|
|xs prefixLength p|xs中最长的连续满足跑的前缀的长度|
|**添加:**||
|x +: xs |将x添加到xs的头部|
|xs :+ x |将x添加到xs的尾部|
|xs padTo (len, x)|将x追加到xs知道长度达到len后得到的序列|
|**更新:**||
|xs patch (i, ys, r)|将xs中从下标i开始的r个元素替换成ye得到的序列|
|xs updated (i, x) |下标i的元素被替换成x的对xs的拷贝|
|xs(i) = x|将下标i的元素更新为x|
|**排序:**||
|xs.sorted|用xs元素的标准类型对xs排序|
|xs sortWith lessThan |以lessThan为比较操作对xs进行排序|
|xs sortBy f|通过对元素应用f，然后比较其顺序|
|**反转:**||
|xs.reverse |颠倒xs的顺序|
|xs.reverseIterator|以颠倒的顺序交出xs所有元素的迭代器|
|xs reverseMap f|以颠倒的顺序对xs的元素映射f后得到的序列|
|**比较:**||
|xs startsWith ys |测试xs是否以ys开始|
|xs endsWith ys |测试xs是否已ys结束|
|xs contains x|xs是否包含x|
|xs containsSlice ys|xs时候包含与ys相等的连续子序列|
|**多重Set操作**||
|xs intersect ys|交集，保持xs中的顺序|
|xs diff ys|差集，保持xs中的顺序|
|xs union ys|并集，等同于xs ++ ys|
|xs.distinct|不包含重复元素的xs的子序列|

Seq有两个子特质：LinearSeq和IndexedSeq。这两个特质并没有添加任何新的操作，不过它们各自拥有不同的性能特征。

- LinearSeq拥有高效的head和tail操作
- IndexedSeq拥有高效的apply、length和update操作（如果是可变的）。

`List`是一种常用的线性序列，`Stream`也是，而Array和ArrayBuffer是两种常用的`经过下标索引的序列`。`Vector类提供了介于索引和现行访问之间有趣的妥协，它即拥有从效果上讲常量时间的索引开销，也拥有时间线性的访问开销`。由于这个特点，向量(Vector)是混用两种访问模式（索引的和线性的）的一个好的基础。

#### 24.5.1 缓冲

可变列表的一种重要子类目是缓冲。缓冲不仅允许对已有元素的更新，同时还允许元素插入，移除和在缓冲末尾高效地添加新元素，主要支持的方法如下：

|操作|操作含义|
|--|--|
|**添加：**||
|buf += x|将元素追加到buf中，并返回buf本身|
|buf += (x, y, z)|将指定元素添加到buf|
|buf ++= xs|将xs的所有元素添加到缓冲|
|x +=: buf|将x添加到buf的头部|
|xs ++=: buf|将xs中的所有元素添加到buf头部|
|buf insert (i, x)|在buf的下标i位置插入x|
|buf insertAll (i, xs)|将xs的全部元素插入到下标i位置|
|**移除：**||
|buf -= x|移除buf中的x|
|buf remove i|移除buf中下标为i的元素|
|buf remove (i, n)|移除buf中从下标i开始的n个元素|
|buf trimStart n|移除buf的前n个元素|
|buf trimEnd n|移除buf后n和元素|
|buf.clear()|移除buf中所有元素|
|**克隆：**||
|buf.clone|生成一个和buf相同的缓冲|

 两个常用的Buffer实现为：`ListBuffer`和`ArrayBuffer`。

- ListBuffer背后是List，支持List的高效转换
- ArrayBuffer背后是数组，可以被快速地转换成数组

### 24.6 集(Sets)

`Set是没有重复元素的Iterable`，主要操作如下：

- **测试**：contains，apply和subsetOf，Set的contains等同于apply。
- **添加**：+和++。将一个元素或者多个元素添加到Set，交出新的Set。
- **移除**：-和--。移除一个或多个元素，交出新的Set。
- **集操作**：交集、差集和并集。这种操作有两种形式：字母的和符号的。字母的有intersect、union和diff。而符号的有&、|和&~。Set从Traversable继承的++可以被看成是union或|的另一个别名。只不过++接收Traversable的入参，而union和|的入参是Set。

+=和-=这样的方法名意味着对于可变和不可变集，可以使用非常类似的代码来处理。参考下面这段用到不可变集s的解释器会话：

```scala
scala> var s = Set(1, 2, 3)
s: scala.collection.immutable.Set[Int] = Set(1, 2, 3)

scala> s += 4

scala> s -= 2

scala> s
res6: scala.collection.immutable.Set[Int] = Set(1, 3, 4)
```

s += 4的语句是是s = s + 4的简写。因此这段代码`调用集s的+方法`，然后将结果赋值给变量s。

对于可变集的处理：

```scala
scala> val s = scala.collection.mutable.Set(1, 2, 3)
s: scala.collection.mutable.Set[Int] = Set(1, 2, 3)

scala> s += 4
res7: s.type = Set(1, 2, 3, 4)

scala> s
res8: scala.collection.mutable.Set[Int] = Set(1, 2, 3, 4)

scala> s -= 2
res9: s.type = Set(1, 3, 4)
```

这个s += 4是`调用可变集的+=方法`，当场修改了集的内容，同理，这次s -= 2`调用的是同一个集上的-=方法`。

比较这个例子，可以得出一个重要原则：通常可以用一个保存为`var的不可变集合`来`替换`一个保存为`val的可变集合`，或者反过来。只要是没有执行这些集合的别名让你可以观测到它到底是`当场修改`还是`返回了新的集合`，这样做就是可行的。

可变集还提供了add和remove作为+=和-=的变种。区别在于add和remove返回的是表示该操作是否让集发生了改变布尔值结果。

目前**可变集的默认实现使用了`哈希表`来保存集的元素**。**不可变集的默认实现是使用了一种可以`跟集的元素数量相适配的底层表示（EmptyMap, Map1, Map2, Map3, Map4, 4个以上使用哈希字典树HashMap）`**。

对于4个元素以内的不可变集比可变集更加紧凑，也更加高效。因此如果你逾期用到的集比较小，尽量用不可变集。

### 24.7 Maps

Map是键值对类型的Iterable。Scala中的Predef类定义了一个隐式转换，允许你写`key -> value`等同于写`(key, value)`也就是`Map("x" -> 24, "y" -> 25, "z" -> 26)`和`Map(("x", 24), ("y", 25), ("z", 26))`是一样的。

支持的主要操作如下：

- **查找**： apply，get，getOrElse，contains和isDefinedAt. 这些操作将映射转换为部分应用函数从键映射到值上。基本的查找值的方法为：

```scala
def get(key): Option[Value]
```

`m get key`操作返回Map中与key对应的值，并放置于`Some`中:

```scala
scala> val a = Map('a' -> 1, 'b' -> 2, 'c' -> 3)
a: scala.collection.immutable.Map[Char,Int] = Map(a -> 1, b -> 2, c -> 3)

scala> a get 'a'  //有对应的键，就返回对应的值
res0: Option[Int] = Some(1)

scala> a get 'd' //没有对应的键，就返回None
res1: Option[Int] = None
```

Map是也定义了apply方法，会直接返回对应的值，而不是放置于`Some`中，如果查找不到，则抛出key not found错误。

```scala
scala> a('d')
java.util.NoSuchElementException: key not found: d
  at scala.collection.immutable.Map$Map3.apply(Map.scala:170)
  ... 28 elided

scala> a('a')
res3: Int = 1
```

- **添加与更新**： +，++ 和updated。
- **移除**： -和--，移除对应元素。
- **生成子集**： keys，keySet，keysIterator，valuesIterator，
和values。它以各种形式分别返回Map的键和值。
- **转换**： filterKeys和mapValues。通过过滤和转换现有Map的绑定来生成一个新的Map。

在`Map特质`上的操作：
|操作|操作含义|
|--|:--|
|**查找：**||
|ms get k|获取键为k对应的值，匹配不到的返回None|
|ms(k)|同上，找不到的键会抛出ket not found错误|
|ms getOrElse (k, d)|如果k存在就返回k，否则返回默认值d|
|ms contains k|ms是否包含k|
|ms isDefinedAt k|同上|
|**添加与更新**：||
|ms + (k -> v)|返回ms所有的键值对和+号右边的键值对|
|ms + (k -> v, l -> w)|同上|
|ms ++ kvs|返回ms和新增键值对左右的元素|
|ms updated (k, v)|同ms + (k -> v)|
|**移除**：||
|ms - k|返回移除键K的剩余元素|
|ms - (k, l, m)|返回不包含k, l, m的剩余元素|
|ms -- ks|移除多个键|
|**子集**：||
|ms.keys|返回Iterable类型的键的集合|
|ms.keySet|返回所有键的集合|
|ms.keysIterator|返回一个迭代器|
|ms.values|返回Iterable类型的值的集合|
|ms.valuesIterator|返回一个迭代器|
|**转换**：||
|ms filterKeys p|一个Map视图，只包含ms中键满足p的那些map|
|ms mapValues f|将函数f应用于ms中与键相关联的每个值所产生的Map视图|

在`可变Map特质`上的操作：
|操作|操作含义|
|--|:--|
|**添加与更新**：||
|ms(k) = v|添加从键k到值v的映射，作为一个副作用映射ms，覆盖k之前的任何映射|
|ms += (k -> v)|增加从键k到值v的映射，将ms映射为一个副作用，并返回ms本身|
|ms += (k -> v, l -> w)|将给定的映射作为副作用添加到ms，并返回ms本身|
|ms ++= kvs|将kvs中的所有映射添加到ms中作为副作用，并返回ms本身|
|ms put (k, v)|添加从键k到值v到ms的映射，并返回之前与k相关的任何值作为可选项|
|ms getOrElseUpdate (k, d)|如果键k在ms中定义，返回它的相关值。否则，用映射k -> d更新ms并返回d|
|**移除**：||
|ms -= k|从ms中移除键k的映射，并返回ms本身|
|ms -= (k, l, m)|从ms中删除带有给定键的映射，并返回ms本身|
|ms --= ks|从ms中删除ks中的所有键作为副作用，并返回ms本身|
|ms remove k|从ms中删除键为k的任何映射，并返回之前与k关联的任何值作为可选项|
|ms retain p|在ms中只保留那些键满足谓词p的映射。|
|ms.clear()|删除ms中的所有映射|
|**转换与克隆**：||
|ms transform f|将map ms中的所有相关值与函数f进行转换|
|ms.clone|返回一个新的可变映射，其映射与ms相同|

Map的添加和删除操作与集合的添加和删除操作是一致的。对于集合，可变映射还支持非破坏性的添加操作+、-和updated，但是它们的使用频率较低，因为它们涉及到可变映射的复制。相反，可变map m的更新通常会被“当场修改”，使用`m(key) = value`或`m += (key ->value)`。还有一个变体`m put (key, value)`，它返回一个Option值，其中包含先前与key关联的值，如果key在以前的映射中不存在，则返回None。

getOrElseUpdate对于访问充当缓存的映射非常有用。如果调用函数f触发一个“昂贵”的计算：

```scala
def f(x: String) = {
    println("taking my time."); Thread.sleep(100)
    x.reverse
}
f: (x: String)String

```

进一步假设f没有副作用，所以用相同的参数再次调用它将总是产生相同的结果。在这种情况下，可以通过将以前计算的参数绑定和f的结果存储在映射中来节省时间，并且只有在没有找到参数的结果时才计算f的结果。可以说map是f计算的缓存。

```scala
scala> def f(x: String) = {
     | println("taking my time."); Thread.sleep(100)
     | x.reverse}
f: (x: String)String

scala> val cache = scala.collection.mutable.Map[String, String]()
cache: scala.collection.mutable.Map[String,String] = Map()

scala> def cachedF(s: String) = cache.getOrElseUpdate(s, f(s))
cachedF: (s: String)String

scala> cachedF("abc")
taking my time.
res0: String = cba

scala> cachedF("abc")
res1: String = cba
```

注意到，getOrElseUpdate的第二个参数只有一个键名，因此上面f(“abc”)的计算只在getOrElseUpdate需要第二个参数的值时执行，而这恰恰是在它的第一个参数没有在缓存映射中找到的情况下执行。您也可以直接实现cachedF，只使用基本的映射操作，但是这样做需要写更多的代码：

```scala
def cachedF(arg: String) = cache get arg match {
  case Some(result) => result
  case None =>
    val result = f(arg)
    cache(arg) = result
    result
}
```

### 24.8 具体的不可变集合类

Scala提供了许多具体的不可变集合类。它们实现的特征(映射、集合、序列)不同，它们是否可以无限，以及各种操作的速度不同。从回顾最常见的不可变集合类型开始。

#### 24.8.1 列表(Lists)

列表是有限不可变序列。它们提供对第一个元素以及列表的其余部分的常量时间访问，并且它们有常量时间的连接操作用于将新元素添加到列表的前面。

#### 24.8.2 流(Streams)

流类似于列表，不同之处在于它的元素是惰性计算的。因此，一条流可以无限长。只有那些被请求的元素才会被计算。否则，流具有与列表相同的性能特征。

列表是用::操作符构造的，而流是用类似的#::构造的：

```scala
scala> val str = 1 #:: 2 #:: 3 #:: Stream.empty
str: scala.collection.immutable.Stream[Int] = Stream(1, ?)
```

这个流的头是1， 尾部包含2和3，这里尾部没有被打印出来，因为还没有计算它们。流需要惰性地进行计算，流的toString方法小心翼翼地不强制进行任何额外计算。

下面是一个更复杂的示例。它计算包含从给定的两个数字开始的斐波那契序列的流。斐波那契序列中的每个元素都是该序列中前两个元素的和：

```scala
scala> def fibFrom(a: Int, b: Int): Stream[Int] = {
  a #:: fibFrom(b, a + b)
}
fibFrom: (a: Int, b: Int)Stream[Int]
```

这个函数看起来很简单。序列的第一个元素显然是a，序列的其余部分是斐波那契序列，从b开始，然后是a + b。棘手的部分是计算这个序列而不导致无限递归。如果函数使用::而不是#::，那么每次调用该函数都会导致另一次调用，从而导致无限递归。使用#::操作符右边的部分只有在被请求的时候才会参与计算。

```scala
scala> val fibs = fibFrom(1, 1).take(7)
fibs: scala.collection.immutable.Stream[Int] = Stream(1, ?)

scala> fibs.toList
res11: List[Int] = List(1, 1, 2, 3, 5, 8, 13)
```

当处理列表的算法小心地只处理它们的头部时，列表是非常高效的。访问、添加和删除列表的头只需要常量时间，而稍后访问或修改列表中的元素所花费的时间与列表长度成线性关系。

#### 24.8.3 向量(Vectors)

向量是一种集合类型，可以有效地访问头部以外的元素。访问向量的任何元素只需要“有效常数时间”，如下所示。它比访问列表头或读取数组元素的常数要大，但它仍然是一个常数。因此，`使用向量的算法在访问序列的头部时不必非常小心。它们可以访问和修改任意位置的元素`，因此编写它们更加方便。

```scala
scala> val vec = scala.collection.immutable.Vector.empty
vec: scala.collection.immutable.Vector[Nothing] = Vector()

scala> val vec2 = vec :+ 1 :+ 2
vec2: scala.collection.immutable.Vector[Int] = Vector(1, 2)

scala> val vec3 = 100 +: vec2
vec3: scala.collection.immutable.Vector[Int] = Vector(100, 1, 2)

scala> vec3(0)
res12: Int = 100
```

向量用宽而浅的树表示，每个树节点最多包含32个向量元素或者32个其他树节点。小于等于32个元素的向量可以在单个节点中表示。小于等于32*32=1024个元素的向量可以用单词额外的间接性来做到。如果允许从根部到最终的元素节点间有两跳(hop)，就可以表示多大2\^15个元素的向量，允许三跳可达2\^20个元素的向量等等。

因此，对于所有正常大小的向量，元素选择最多涉及5个基本数组选择。这就是我们写元素访问是`“从实效上讲的常量时间”`。

`向量是不可变的`，因此不能“当场修改”向量的元素。但是，使用updated方法可以创建一个新向量，只有一个元素与原有向量不同。

```scala
scala> val vec = Vector(1, 2, 3)
vec: scala.collection.immutable.Vector[Int] = Vector(1, 2, 3)

scala> vec updated (2, 4) //将第二个值更新为4，并返回一个新的向量
res13: scala.collection.immutable.Vector[Int] = Vector(1, 2, 4)

scala> vec //vec并没有改变
res14: scala.collection.immutable.Vector[Int] = Vector(1, 2, 3)
```

如上面的最后一行所示，对updated的调用对原始向量vec没有影响。与选择一样，功能向量更新也是“有效的常量时间”。可以通过从树的根开始复制包含该元素的节点和指向该元素的每个节点来更新向量中间的元素。这意味着一个功能性更新将在1到5个节点之间创建，每个节点最多包含32个元素或子树。这当然比在可变数组中就地更新要“昂贵”，但仍然比复制整个向量“便宜”得多。

因为向量在快速随机选择和快速随机函数更新之间取得了很好的平衡，所以它们目前是不可变索引序列的默认实现：

```scala
scala> collection.immutable.IndexedSeq(1, 2, 3)
res15: scala.collection.immutable.IndexedSeq[Int] = Vector(1, 2, 3)
```

#### 24.8.4 不可变栈(Stacks)

如果需要一个先进后出的序列，可以使用Stack，可以使用push压入一个元素，使用pop来弹出一个元素，以及使用top来查看栈顶的元素，这些操作都是常量时间。

```scala
scala> val stack = scala.collection.immutable.Stack.empty
stack: scala.collection.immutable.Stack[Nothing] = Stack()

scala> val hasOne = stack.push(1)
hasOne: scala.collection.immutable.Stack[Int] = Stack(1)

scala> stack
res0: scala.collection.immutable.Stack[Nothing] = Stack()

scala> hasOne.pop
res1: scala.collection.immutable.Stack[Int] = Stack()

scala> hasOne.top
res2: Int = 1
```

不可变的栈很少使用，因为它的功能被列表替代了，对于不可变栈的push与列表的::操作相同，而对于栈的pop操作等同于列表的tail。

#### 24.8.5 不可变队列(Queues)

队列和栈的实现比较类似，但队列是先进先出的结构：

```scala
scala> val empty = scala.collection.immutable.Queue[Int]()
empty: scala.collection.immutable.Queue[Int] = Queue()

scala> val has1 = empty.enqueue(1) //添加一个元素
has1: scala.collection.immutable.Queue[Int] = Queue(1)

scala> val has123 = has1.enqueue(List(2, 3)) //添加多个元素
has123: scala.collection.immutable.Queue[Int] = Queue(1, 2, 3)

scala> val (elem, has23) = has123.dequeue  //移除元素
elem: Int = 1
has23: scala.collection.immutable.Queue[Int] = Queue(2, 3)
```

`dequeue返回的是一组包含被移除元素以及队列剩余元素的对偶`。

#### 24.8.6 区间(Ranges)

区间是一组有序的整数序列，整数之间有相同的间隔。用Scala创建Range的方式是使用预定义的方法to和by：

```scala
scala> 1 to 3
res3: scala.collection.immutable.Range.Inclusive = Range 1 to 3

scala> 5 to 14 by 3 //间隔为3
res4: scala.collection.immutable.Range = Range 5 to 14 by 3

scala> 1 until 13
res5: scala.collection.immutable.Range = Range 1 until 13
```

`区间的内部表示占据常量的空间`，因为它可以使用三个数表示：`起始值、终值和步长`。因此，大多数区间操作都很快。

#### 24.8.7 哈希字典树

哈希字典树是实现高效的不可变映射和不可变集的标准方式。它们的内部表现形式和向量类似。它们也是每个节点都有32个元素或者32个子树的树，不过`元素的选择是基于哈希码`的。例如，要找出映射中给定的键，首先用哈希码的最低5位来找到第1颗子树，用接下来的5位找到第2颗子树，以此类推，当某个节点所有元素的哈希码（已用到的部分）各不相同时，这个选择过程就停止了，因此并不是必须用到哈希码的所有位。

哈希字典树在比较快的查找和比较高效的函数式插入（+）和删除（-）之间找到了一个平衡，这就是Scala对不可变映射和不可变集的默认实现的基础。

#### 24.8.9 红黑树

红黑树是一种平衡的二叉树，某些节点被标记为“红”，而其他节点被标记为“黑”的，跟其他平衡二叉树一样，对它们的操作可以可靠地与树规模相关的对数时间内完成。

Scala提供了内部使用的红黑树的集和映射的实现，可以用TreeSet和TreeMap来访问它们：

```scala
scala> val set = scala.collection.immutable.TreeSet.empty[Int]
set: scala.collection.immutable.TreeSet[Int] = TreeSet()

scala> set + 1 + 3 + 3
res10: scala.collection.immutable.TreeSet[Int] = TreeSet(1, 3)
```

`红黑树是Scala中SortedSet的标准实现`，因为它们提供了按顺序返回集的所有元素的一个高效迭代器。

#### 24.8.10 不可变位组

位组用来表示某个更大整数的为的小整数的集合。例如，包含3、2和0的位组可以用二进制的整数1101表示，转换成十进制就是13。

从内部讲，位组使用的是一个64位Long的数组，数组中的第一个Long表示0到63的整数，第2个Long表示64到127的整数，以此类推。因此只要位组中最大的整数小于数百这个规模，位组都会非常紧凑。

对位组的操作非常快，测试某个位组师傅包含某个值只需要常量的时间。往位组中添加条目需要的时间跟位组的Long数组长度整整比，这通常是一个非常小的值：

```scala
scala> val bits = scala.collection.immutable.BitSet.empty
bits: scala.collection.immutable.BitSet = BitSet()

scala> val moreBits = bits + 3 + 4 + 4
moreBits: scala.collection.immutable.BitSet = BitSet(3, 4)

scala> moreBits(3)
res11: Boolean = true

scala> moreBits(0)
res12: Boolean = false
```

#### 24.8.11 列表映射

列表映射将映射表示为一个由键值对组成的链表。一般而言，对列表映射的操作需要遍历整个列表。因此，对列表映射的操作耗时跟映射的规模成正比。

事实上，`Scala很少使用映射列表`，因为`标准的不可变映射几乎总是比列表映射更快`，唯一可能有区别的场景是当映射应为某种原因需要`经常访问列表中的首个元素`时，频率远高于访问其他元素。

```scala
scala> val map = collection.immutable.ListMap(1 -> "one", 2 -> "two")
map: scala.collection.immutable.ListMap[Int,String] = ListMap(1 -> one, 2 -> two)

scala> map(2)
res13: String = two
```

### 24.9 具体的可变集合类

#### 24.9.1 数组缓冲

数组缓冲包含一个数组和一个大小。对数组缓冲的大部分操作都跟数组的速度一样。这些操作只是简单地访问和修改底层数组。数组缓冲可以高效的添加元素，对数组缓冲追加元素需要的时间为平摊的常量时间。数组缓冲对于那些通过往喂不添加新元素来高效构建大集合的场景而言非常有用。

#### 24.9.2 列表缓冲

和数组缓冲类似，只不过内部使用的是链表而不是数组，如果打算在构建完成后将缓冲转换为列表，就使用列表缓冲。

#### 24.9.3 字符串构建器

正如数组缓冲有助于构建数组，列表缓冲有助于构建列表，字符串构建器有助于构建字符串。由于字符串构建器非常有用，它们已经被引入到默认的命名空间当中。只需要简单的使用`new StringBuilder`来创建即可：

```scala
scala> val buf = new StringBuilder
buf: StringBuilder =

scala> buf += 'a'
res14: buf.type = a

scala> buf += 'b'
res15: buf.type = ab
```

#### 24.9.4 链表

`链表是由用next指针链接起来的节点组成的可变序列`。在大多数语言中，null会被用于空链表，但是在Scala中行不通，因为即便是空的序列也需要支持所有的序列方法。尤其是LinkedList.empty.isEmpty应该返回true而不是抛出NullPointerException。`空链表因此也是特殊处理的：它们的next字段指向节点自己`。

跟它的不可变版本一样，链表支持的最佳的操作是顺序操作。不仅如此，在链表中插入元素或其他链表十分容易。

#### 24.9.5 双向链表

DoubleLinkedList比链表多了一个前向指针prev，指向当前节点的前一个元素，这个额外的链接的主要好处是让它移除元素的操作非常快。

#### 24.9.6 可变列表

MutableList由一个单向链表和一个执行该列表末端的空节点组成。这使得往列表尾部的追加操作是一个常量时间的，因为它免除了遍历列表来找到末端的需要。MutableList目前是Scala的mutable.LinearSeq的标准实现。

#### 24.9.7 队列

与不可变队列类似，不过不是用enqueue而是用+=和++=来追加元素。另外对于可变队列而言，dequeue方法只会简单滴移除头部的元素并返回。参考下面例子：

```scala
scala> val queue = new scala.collection.mutable.Queue[String]
queue: scala.collection.mutable.Queue[String] = Queue()

scala> queue += "a"
res17: queue.type = Queue(a)

scala> queue ++= List("b", "c")
res18: queue.type = Queue(a, b, c)

scala> queue.dequeue
dequeue   dequeueAll   dequeueFirst

scala> queue.dequeue
res19: String = a

scala> queue
res20: scala.collection.mutable.Queue[String] = Queue(b, c)
```

#### 24.9.8 数组序列

数组序列是固定大小的，内部使用Array[AnyRef]来存放其元素的可变序列。Scala中的实现是ArraySeq类。

#### 24.9.9 栈

与不可变栈类似，可变栈的修改是当场发生的，在Scala 2.12中被弃用。

```scala
scala> val stack = new scala.collection.mutable.Stack[Int]
<console>:11: warning: class Stack in package mutable is deprecated (since 2.12.0): Stack is an inelegant and potentially poorly-performing wrapper around List. Use a List assigned to a var instead.
       val stack = new scala.collection.mutable.Stack[Int]
                                                ^
stack: scala.collection.mutable.Stack[Int] = Stack()
```

#### 24.9.10 数组栈

ArrayStack是可变栈的另一种实现，内部是一个Array，在需要时重新改变大小。他提供了快速的下标索引，一般而言对于大多数操作都比可变栈更快。

#### 24.9.11 哈希表

哈希表底层用数组存放其元素，元素的存放位置取决于该元素的哈希码。往哈希表添加元素只需要常量时间，只要数组中没有其他元素拥有相同的哈希码。因此，只要哈希表中的对象能够按哈希码分布的足够均匀，哈希操作就会非常快。正因为如此，Scala中默认的可变映射和可变集的实现都是基于哈希码表的。

哈希集和哈希映射用起来和其他集或映射一样：

```scala
scala> val map = collection.mutable.HashMap.empty[Int, String]
map: scala.collection.mutable.HashMap[Int,String] = Map()

scala> map += (1 -> "make a web site")
res21: map.type = Map(1 -> make a web site)

scala> map += (3 -> "profit!")
res22: map.type = Map(1 -> make a web site, 3 -> profit!)

scala> map(1)
res23: String = make a web site

scala> map contains 2
res24: Boolean = false
```

对哈希表的便利并不保证按照某个特定的顺序，遍历只不过是简单地遍历底层的数组。如果想要保证迭代顺序，可以用链式的哈希映射或哈希集，而不是常见的哈希映射或哈希集。

#### 24.9.12 弱哈希映射

弱哈希映射是一种特殊的哈希映射，对这种哈希映射，垃圾收集器并不会跟踪映射到其中的键的链接。这意味着`如果没有其他引用指向某个键，那么该键到它的关联就会从映射中消失`。

弱哈希映射对于类似缓存这样的任务而言十分有用，即使你想要重用某个耗时计算的函数结果的场景。如果这些代表入参的键和函数结果是保存在常规的哈希映射当中的，这个映射就会无限增长，所有的键都不会被当做垃圾处理。使用弱哈希映射就会避免这个问题。一旦某个键对象不再可及，该条目就会从弱哈希表中移除。Scala中弱哈希的实现是对底层Java实现java.util.WeakhashMap的包装。

#### 24.9.13 并发映射

并发映射可以被多个线程同时访问，除了常见的Map操作外，还提供如下原子操作：

|操作|操作含义|
|--|--|
|m putIfAbsent (k, v)|如果k不存在，则添加(k, v)|
|m remove (k, v)|移除该条目|
|m replace (k, old, new)|如果k原先绑定old，则将关联更新为new|
|m replace (k, v)|将k的关联更新为v|

ConcurrentHashMap是Scala标准类库中的一个特质。目前它的唯一实现是Java的java.util.concurrentHashMap，通过标准的Java/Scala集合转换，可以自动转换成Scala映射。

#### 24.9.14 可变位组

可变位组和不可变位组类似，只不过它可以当场被修改。可变位组在更新方法比不可变位组稍微高效一点，因为它们不需要将那些没有改变的Long来回复制。参考下面例子：

```scala
scala> val bits = scala.collection.mutable.BitSet.empty
bits: scala.collection.mutable.BitSet = BitSet()

scala> bits += 1
res25: bits.type = BitSet(1)

scala> bits += 3
res26: bits.type = BitSet(1, 3)

scala> bits
res27: scala.collection.mutable.BitSet = BitSet(1, 3)
```

### 24.10 数组

数组在Scala中是一种特殊的集合。一方面Scala的数组跟Java的数组一一对应，`Array[int]`对应于`int[]`，`Array[String]`对应于`String[]`等等，另一方面，Scala数组提供了比Java更多的功能。首先，`Scala数组支持泛型`其次，Scala数组跟Scala的序列兼容（可以在要求`Seq[T]`的地方传入`Array[T]`），最后，Scala数组还支持所有的序列操作：

```scala
scala> val a1 = Array(1, 2, 3)
a1: Array[Int] = Array(1, 2, 3)

scala> val a2 = a1 map(_ * 3)
a2: Array[Int] = Array(3, 6, 9)

scala> val a3 = a2 filter (_ % 2 != 0)
a3: Array[Int] = Array(3, 9)

scala> a3.reverse
res28: Array[Int] = Array(9, 3)
```

Scala的数组使用Java的数组来表示的，Scala数组是怎么支持这些功能的？答案是`隐式转换`，数组并不能假装是序列，因为原生数组的数据类型表示并不是序列的子类型，每当使用数组被用作序列，都会被隐式地转换为Seq的子类。这个子类的名称为`scala.collection.mutable.WrappedArray`，如下：

```scala
scala> val seq: Seq[Int] = a1
seq: Seq[Int] = WrappedArray(1, 2, 3)

scala> val a4: Array[Int] = seq.toArray
a4: Array[Int] = Array(1, 2, 3)

scala> a1 eq a4
res29: Boolean = true
```

从上面看出，有一个从Array到WrappedArray的隐式转换，如果要反过来，从WrappedArray转换为Array，可以用Traverable中定义的toArray方法，经过两个转换之后的数组跟一开始是相同的数组。

可以被应用到数组的还有另一个隐式转换，这个转换只是`简单地将所有序列的方法“添加”到数组`，但并不将数组本身变成序列。“添加”意味着数组被包装成另一个类型为ArrayOps的对象，这个对象支持所有的序列方法，通常这个ArrayOps对象的生命周期很短：`它通常在调用序列方法之后就不再被访问了`，因此其存储空间可以被回收。现代的VM会完全避免创建这个对象。

这两种隐式转换的区别可以通过下面例子展现出来：

```scala
scala> val seq: Seq[Int] = a1
seq: Seq[Int] = WrappedArray(1, 2, 3)

scala> seq.reverse
res30: Seq[Int] = WrappedArray(3, 2, 1)

scala> val ops: collection.mutable.ArrayOps[Int] = a1
ops: scala.collection.mutable.ArrayOps[Int] = [I(1, 2, 3)

scala> ops.reverse
res31: Array[Int] = Array(3, 2, 1)
```

对Seq调用reverse返回的类型是WrappedArray，并没有发生变化，这合乎逻辑，因为被包装的数组是Seq，而对任何Seq调用reverse都会返回Seq，而对ArrayOps调用reverse返回的是Array而不是Seq。

上例仅仅展示了Array和WrappedArray的关系，通常从来都不用定义一个ArrayOps，只需要对数组调用Seq方法即可：

```scala
scala> a1.reverse
res32: Array[Int] = Array(3, 2, 1)
```

`隐式转换会自动插入ArrayOps对象`，因此上面这行代码和下面的代码是等效的，其中intArrayOps就是那个被自动插入的隐式转换：

```scala
scala> intArrayOps(a1).reverse
res33: Array[Int] = Array(3, 2, 1)
```

这就带来一个问题，编译器是如何选中了intArrayOps而不是另一个到WrappedArray的隐式转换呢？毕竟，这两个隐式转换都可以将数组映射成一个支持reverse方法的类型。答案就是：`这两个隐式转换之前存在优先级`，ArrayOps转换的优先级要高于WrappedArray转换，前者定义在Predef对象中，而后者定义在scala.LowPriorityImplicits类中，这个类时Predef的超类。`子类和子对象中的隐式转换比基类的隐式转换优先级更高`，因此如果两个隐式转换同时可用，编译器会选择Predef中的那一个。

数组与序列是兼容的，支持所有的序列操作，不过泛型呢？在Java中你没法写出`T[]`，那么Scala的`Array[T]`有时如何表示的呢？事实上，像`Array[T]`这样的泛型数组在运行时可以使任何Java支持的8中基本类型的数组`byte[], short[], char[], int[], long[], float[], double[], boolean[]`，也可以是对象的数组，`唯一能跨越这些类型的公共运行期类型是AnyRef`，因此这就是Scala将`Array[T]`映射到的类型。

在运行时，档类型为Array[T]的数组的元素被访问或者被更新时，有一系列的类型检查来决定实际的数组类型，然后才是对Java数组的正确操作。类型检查比较消耗时间，在满足性能要求前提下，尽量使用类型确定的数组，而不是泛型数组。

仅仅能够表示泛型数组的类型还不够，还需要以某种方式来创建泛型数组，这个问题更加困难，首先尝试创建数组：

```scala
def evenElems[T](xs: Vector[T]): Array[T] = {
  val arr = new Array[T]((xs.length + 1) / 2)
  for (i <- 0 until xs.length by 2)
    arr(i / 2) = xs(i)
  arr
}

<pastie>:12: error: cannot find class tag for element type T
  val arr = new Array[T]((xs.length + 1) / 2)
            ^
```

编译器抛出错误信息，对于T类型找不到tag类。evenElems定义了返回类型与入参的类型一致，基于参数类型T的实际类型，可能为Array[Int]也可能为Array[Boolean]，这些类型在运行时表现各不相同，Scala不能确定T的实际类型，因为与类型参数T相对应的实际类型在运行时被擦除了，这就是编译器为什么会抛出一个错误信息。

在许多情况下，编译器都可以自行生成类标签，对于具体类型Int或String就是如此，对于某些泛型类型比如List[T]也是如此，有足够多的信息已知，可以预测被擦除的类型，在本例中这个被擦除的类型是List。

实际上编译器需要提供关于evenElems实际的参数类型是什么的运行时线索，这个线索的表现形式是类型为scala.reflact.ClassTag的`类标签(class tag)`。类标签描述是给定类型**被擦除的类型**，这也是构造该类型的数组需要的全部信息。

```scala
import scala.reflact.ClassTag
def evenElems[T: ClassTag](xs: Vector[T]): Array[T] = {
  val arr = new Array[T]((xs.length + 1) / 2)
  for (i <- 0 until xs.length by 2)
    arr(i / 2) = xs(i)
  arr
}

evenElems: [T](xs: Vector[T])(implicit evidence$1: scala.reflect.ClassTag[T])Array[T]
```

在新定义当中，当Array[T]被创建时，编译器会查找类型参数T的类标签，也就是说，它会查找一个类型为ClassTag[T]的隐式值。如果找到这样的值，类标签就被用于构造正确类型的数组，不然就会报错。

```scala
scala> evenElems(Vector(1, 2, 3, 4, 5))
res34: Array[Int] = Array(1, 3, 5)

scala> evenElems(Vector("this", "is", "a", "test", "run"))
res35: Array[String] = Array(this, a, run)
```

在这两种情况下，Scala编译器都自动为元素类型构建出类标签（首先是Int然后是String）并将它传入evenElems的隐式参数。对于所有具体类型，编译器都可以帮我们完成，但是如果入参本身是另一个参数类型而不带类标签，编译器就无能为力了，比如下面这段代码：

```scala
scala> def wrap[U](xs: Vector[U]) = evenElems(xs)
<console>:14: error: No ClassTag available for U
       def wrap[U](xs: Vector[U]) = evenElems(xs)
                                             ^

scala> def wrap[U: ClassTag](xs: Vector[U]) = evenElems(xs)
wrap: [U](xs: Vector[U])(implicit evidence$1: scala.reflect.ClassTag[U])Array[U]
```

第一个定义报错的原因是evenElems要求类型参数U的类标签，但是没有找到。解决方案就是第二个要求针对U的隐式类标签。`U定义中的上下文界定只不过是此处名为evidence$1，类型为ClassTag[U]的隐式参数的简写`。

### 24.11 字符串

跟数组一样，字符串也不直接是序列，但是它们可以转换为序列，因而支持序列的所有操作。

```scala
scala> val str = "hello"
str: String = hello

scala> str.reverse
res0: String = olleh

scala> str.map(_.toUpper)
res1: String = HELLO

scala> str drop 3
res2: String = lo

scala> str slice (1, 4)
res3: String = ell

scala> val s: Seq[Char] = str
s: Seq[Char] = hello
```

### 24.12 性能特征

正如前面的解释所示，不同的集合类型具有不同的性能特征。这通常是选择一种集合类型而不是另一种集合类型的主要原因。您可以看到集合上一些常见操作的性能特征，总结在两个表中：

序列类型的性能特征:

||head|tail|apply|update|prepend|append|insert|
|--|--|--|--|--|--|--|--|
|**immutable**|
|List|C|C|L|L|C|L|-|
|Stream|C|C|L|L|C|L|-|
|Vector|eC|eC|eC|eC|eC|eC|-|
|Stack|C|C|L|L|C|L|-|
|Queue|aC|aC|L|L|L|C|-|
|Range|C|C|C|-|-|-|-|
|String|C|L|C|L|L|L|-|
|**mutable**||||||||
|ArrayBuffer|C|L|C|C|L|aC|L|
|ListBuffer|C|L|L|L|C|C|L|
|StringBuilder|C|L|C|C|L|aC|L|
|MutableList|C|L|L|L|C|C|L|
|Queue|C|L|L|L|C|C|L|
|ArraySeq|C|L|C|C|-|-|-|
|Stack|C|L|L|L|C|L|L|
|ArrayStack|C|L|C|C|aC|L|L|
|Array|C|L|C|C|-|-|-|

集合和映射类型的性能特征:

||lookup|add|remove|min|
|--|--|--|--|--|
|**immutable**|||||
|HashSet/HashMap|eC|eC|eC|L|
|TreeSet/TreeMap|Log|Log|Log|Log|
|BitSet|C|L|L|eC^a^|
|ListMap|L|L|L|L|
|**mutable**|||||
|HashSet/HashMap|eC|eC|eC|L|
|WeakHashMap|eC|eC|eC|L|
|BitSet|C|aC|C|eC^a^|

其中：

- `C`：操作消耗常量时间
- `eC`：该操作需要的时间实际上是常数，但这可能取决于一些假设，如向量的最大长度或散列键的分布
- `aC`：这个操作`平摊常数时间`。一些操作的调用可能会花费更长的时间，但是如果许多操作平均执行，每个操作只花费常数时间
- `Log`：操作消耗对数时间
- `L`：操作消耗时间与大小正正比
- `-`：不支持该操作

### 24.13 相等性

集合库对于相等和散列有统一的方法。首先，将集合划分为集合、映射和序列。`不同的集合类别之间总是不相等的`，比如Set(1, 2, 3)不等同于List(1, 2, 3)，即使它们包含相同的元素。另一方面，`在同一类别中，如果当且仅当集合具有相同的元素(对于序列:相同顺序的相同元素)，则集合是相等的`，例如List(1, 2, 3) == Vector(1, 2, 3)和HashSet(1, 2) == TreeSet(2, 1)。

对于相等性检查来说，集合是可变的还是不可变的并不重要。对于可变集合，相等性仅仅取决于执行相等性测试时的当前元素。这意味着可变集合可能等于不同时间的不同集合，这取决于添加或删除了什么元素。当使用可变集合作为散列映射中的键时，这是一个潜在的陷阱。

```scala
scala> import scala.collection.mutable.{HashMap, ArrayBuffer}
import scala.collection.mutable.{HashMap, ArrayBuffer}

scala> val buf = ArrayBuffer(1, 2, 3)
buf: scala.collection.mutable.ArrayBuffer[Int] = ArrayBuffer(1, 2, 3)

scala> val map = HashMap(buf -> 3)
map: scala.collection.mutable.HashMap[scala.collection.mutable.ArrayBuffer[Int],Int] = Map(ArrayBuffer(1, 2, 3) -> 3)

scala> map(buf)
res4: Int = 3

scala> buf(0) += 1

scala> map(buf)
java.util.NoSuchElementException: key not found: ArrayBuffer(2, 2, 3)
  at scala.collection.MapLike.default(MapLike.scala:235)
  at scala.collection.MapLike.default$(MapLike.scala:234)
  at scala.collection.AbstractMap.default(Map.scala:63)
  at scala.collection.mutable.HashMap.apply(HashMap.scala:69)
  ... 28 elided
```

在本例中，最后一行的选择很可能会失败，因为数组xs的哈希码在倒数第二行发生了更改。因此，基于哈希代码的查找将查看与存储xs的位置不同的位置。

### 24.14 视图

集合有很多构造新集合的方法。例如map、filter和++。我们称这种方法为转换器，因为它们至少接受一个集合作为接收对象，并在其结果中生成另一个集合。

转换器的实现方式主要有两种：`严格模式`和`惰性模式`。严格模式的转换器使用它的所有元素构造一个新的集合。惰性转换器仅为结果集合构造一个代理，其元素是按需构造的。

作为一个非严格转换器的示例，请考虑一个延迟映射操作的简单策略：

```scala
def lazyMap[T, U](coll: Iterable[T], f: T => U) = {
  new Iterable[U]{
    def iterator = coll.iterator map f
  }
}

lazyMap: [T, U](coll: Iterable[T], f: T => U)Iterable[U]
```

注意，lazyMap构造了一个新的迭代器，而没有遍历给定集合coll的所有元素。相反，给定的函数f会在新集合迭代器的元素需要时才被调用。

默认情况下，Scala集合在其所有转换器中都是严格的，Stream除外，后者延迟地实现其所有转换器方法。但是，有一种系统的方法可以将每个集合变为惰性集合，或者反过来也可行，就是基于视图。视图是一种特殊的集合，它能表示一些基本集合，但是延迟地实现它的所有转换器。

要从集合转到它的视图，可以使用集合上的`view`方法。如果xs是某个集合，那么`xs.view`是相同的集合，但是所有转换器都是惰性实现的。要从视图返回到严格模式集合，可以使用`force`方法。

举个例子，假设你有一个Ints向量，想在这个向量上连续映射两个函数:

```scala
scala> val v = Vector(1 to 10: _*)
v: scala.collection.immutable.Vector[Int] = Vector(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

scala> v map(_ + 1) map(_ * 2)
res7: scala.collection.immutable.Vector[Int] = Vector(4, 6, 8, 10, 12, 14, 16, 18, 20, 22)
```

在最后一条语句中，表达式`v map(_ + 1)`构造了一个新的向量，然后在第二次调用`map(_ * 2)`时将其转换为第三个向量。在许多情况下，从第一次映射调用构造中间结果有点浪费。在这个伪示例中，使用两个函数`(_ + 1)`和`(_ * 2)`组成一个映射会更快。

如果这两个函数在同一位置可用，则可以手动完成此操作。但通常，数据结构的连续转换是在不同的程序模块中完成的。融合这些转换将破坏模块化。避免中间结果的一种更一般的方法是首先将向量转换为视图，对视图应用所有转换，最后将视图强制转换为向量:

```scala
scala> (v.view map(_ + 1) map(_ * 2)).force
res12: Seq[Int] = Vector(4, 6, 8, 10, 12, 14, 16, 18, 20, 22)

scala> val vv = v.view
vv: scala.collection.SeqView[Int,scala.collection.immutable.Vector[Int]] = SeqView(...)
```

v.view操作会返回一个SeqView，一个延迟计算的Seq。SeqView有两个类型参数，第一个是Int，代表元素的类型；第二个是scala.collection.immutable.Vector[Int]，代表视图转回向量的类型构造器。

```scala
scala> vv map(_ + 1)
res13: scala.collection.SeqView[Int,Seq[_]] = SeqViewM(...)
```

返回SeqViewM，这本质上是一个记录了一个带有函数`(_ + 1)`的map操作需要被应用到向量v的包装器，不过它并不会在视图被强转之前应用这个map操作。名称中SeqView后面的“M”表示该视图封装了一个map操作，还有其他字母用于标识其他延迟的操作。例如，“S”标识一个延迟的slice操作，而“R”标识一个reverse。

```scala
scala> res13 map(_ * 2)
res14: scala.collection.SeqView[Int,Seq[_]] = SeqViewMM(...)
```

经过这个操作后，打印出来两个“M”：SeqViewMM，这得到的是一个包含了两次map操作的SeqView。

```scala
scala> res14.force
res15: Seq[Int] = Vector(4, 6, 8, 10, 12, 14, 16, 18, 20, 22)
```

作为force操作的一部分，两个被保存的函数得以应用，新的向量被构造出来，通过这种方式，并不需要中间的数据结构。

需要注意的是，最后的结果类型是一个Seq，而不是Vector。通过追踪类型变化可以看到，当第一次延迟的mao被应用是，结果的静态类型就是`SeqView[Int, Seq[_]]`，也就是说，类型系统对于视图被应用到具体的序列类型Vector这件事的“认知”丢失了。对于任何特定的视图的实现都要求大量的代码。因此，Scala集合类库几乎只对一般化的集合类型而不是具体的实现提供视图支持（数组例外，对数组的延迟操作会得到静态为Array的结果）。

采用视图有两个原因，`首先是性能`，使用视图避免了中间结果的产生，节约开销，`其次是针对可变序列`，这类视图的许多变化函数提供了对原始序列的一个窗口，可以用来可选择地对该序列的某些元素进行更新：

```scala
val arr = (0 to 9).toArray
val subarr = arr.view.slice(3, 6)

def negate(xs: collection.mutable.Seq[Int]) = {
  for (i <- 0 until xs.length) xs(i) = -xs(i)
}

negate(substr) //只更新了位置3到位置5的元素
}
```

看过视图的使用过后，可能好奇为什么还要求严格求值得集合呢？原因之一是性能的比较并非总是偏爱惰性求值的集合，`对于小型集合而言，组织视图和应用闭包的额外开销通常打过免去中间数据结构的收益`。`可能更重要的原因是如果延迟操作有副作用`，对视图的求值可能会变得给常令人困惑。

比如，在Scala2.8之前，Range类型是惰性的，因为其行为从效果上跟视图很像，当创建这样一个actors时：

```scala
val actors = for (0 <- 1 to 10) yield actor{...}
```

在这之后actor并没有被执行，因为Range是惰性的。所以为了避免这样的问题，Scala从2.8开始除了流之外的所有几何都是严格求值的。`从严格求值的集合到惰性求值的唯一方式是通过view方法，反过来唯一的方式是force方法`。

总的来说，视图是一个调和效率和模块化之间矛盾的强大工具。使用视图应该局限于两个场景：

- 在集合变换没有副作用的纯函数式的代码中应用视图；
- 对所有的修改都是显式执行的可变集合使用视图。

最好`避免既在创建新的集合又有副作用的场景下混用视图和各种集合操作`。

### 24.15 迭代器

迭代器并不是集合，而是逐个访问集合元素的一种方式。迭代器it的两个基本操作是`next`个`hasNext`。`it.next()`的调用返回迭代器的下一个元素并将迭代器的状态往前推进一步，如果没有更多的元素可以返回，那么对next操作会抛出`NoSuchElementException`。可以用Iterator的hasNext方法开货值时候还有更多的元素可以返回。

遍历迭代器的所有元素最直接的方法是通过循环：

```scala
while(it.next())
  println(it.next())
```

Scala的迭代器还提供了Traversable、Iterable和Seq特质中的大部分方法，比如，提供了foreach，上述过程可以简写为：

```scala
it foreach println
```

还可以使用for表达式：

```scala
for (elem <- it) println(elem)
```

迭代器的foreach和可遍历集合(Traverable)的同名方法有一个重要的区别是：`对迭代器调用foreach，它执行完后会将迭代器留在末端`。因此对相同的迭代器再次调用next会抛NoSuchElementException。而`对集合调用foreach，它会保持集合中的元素数量不变`（除非传入的函数会添加或移除元素，不过并不鼓励这样做，因为可能会带来令人意外的结果）。

```scala
scala> val it = Iterator("This", "is", "a", "Scala", "script")
it: Iterator[String] = <iterator>

scala> it.map(_.length)
res0: Iterator[Int] = <iterator>

scala> res0 foreach println
4
2
1
5
6

scala> it.next //调用完上一步后，已经指向it的尾部
java.util.NoSuchElementException: next on empty iterator
  at scala.collection.Iterator$$anon$2.next(Iterator.scala:41)
  at scala.collection.Iterator$$anon$2.next(Iterator.scala:39)
  at scala.collection.IndexedSeqLike$Elements.next(IndexedSeqLike.scala:63)
  ... 28 elided

scala> it.hasNext
res3: Boolean = false
```

另一个例子是dropWhile方法，可以用来找到迭代器中首个满足某种条件的`首个元素`：

```scala
scala> val it = Iterator("a", "number", "of", "words")
it: Iterator[String] = <iterator>

scala> it dropWhile (_.length < 2)
res17: Iterator[String] = <iterator>

scala> res17.next() //如果执行的是it.next()，就会返回a，it并没在第二步被修改
res18: String = number

//与上述代码分开，单独执行
scala> val it = Iterator("a", "number", "of", "words")
it: Iterator[String] = <iterator>

scala> it dropWhile (_.length < 2)
res19: Iterator[String] = <iterator>

scala> it.next()
res20: String = a
```

只有一个标准操作duplicate允许重用同一个迭代器：

```scala
scala> val it = Iterator("a", "number", "of", "words")
it: Iterator[String] = <iterator>

scala> val (it1, it2) = it.duplicate
it1: Iterator[String] = <iterator>
it2: Iterator[String] = <iterator>
```

it1和it2相互独立，且都和it拥有相同的元素，it并没有被修改掉，还是原始的it:

```scala
scala> it.next()
res21: String = a

scala> it.next()
res22: String = number
```

但是it的next操作会影响it1和it2：

```scala
scala> val it = Iterator("a", "number", "of", "words")
it: Iterator[String] = <iterator>

scala> val (it1, it2) = it.duplicate
it1: Iterator[String] = <iterator>
it2: Iterator[String] = <iterator>

scala> it.next()
res23: String = a

scala> it.next()
res24: String = number

scala> it1.next()
res25: String = of

scala> it2.next()
res26: String = of

scala> it.next()
res27: String = words

scala> it1.next()
java.util.NoSuchElementException: next on empty iterator
  at scala.collection.Iterator$$anon$2.next(Iterator.scala:41)
  at scala.collection.Iterator$$anon$2.next(Iterator.scala:39)
  at scala.collection.IndexedSeqLike$Elements.next(IndexedSeqLike.scala:63)
  at scala.collection.Iterator$Partner$1.next(Iterator.scala:1308)
  ... 28 elided
```

总的来说，迭代器的行为跟集合很像，如果你在调用了迭代器的方法后就不再访问它。Scala几个类库将这个性质显式地表示为一个名为TraversableOnce的抽象，这是Traversable和Iterator的公共超特质。正如其名称所示，TraversableOnce对象可以用foreach来遍历，不过在遍历后该对象的状态并没有规定。如果是一个Iterator，遍历后将位于它的末端，而如果是Traversable，遍历过后保持原样。`TraversableOnce的一个常见用例是作为即可以接收迭代器也可以接收可遍历集合的方法的入参类型声明`。比如，Traversable特质的++方法，它接收一个TraversableOnce参数，因此可以追加来自迭代器或者可遍历集合的元素。

#### 24.15.1 带缓冲的迭代器

有时需要一个可以“向前看”的迭代器，这样就可以检查下一个要返回的元素但并不往前推进，例如，需要从一个返回字符串序列的迭代器中跳过前面的空字符串，尝试这样实现：

```scala
def skipEmptyWordsNOT(it: Iterator[String]) = {
  while (it.next().isEmpty){it.next()}
}
```

这样的实现有一个明显的问题是，会跳过第一个非空的字符串！这个问题的解决方案是使用待缓冲的迭代器，即BufferedIterator特质的实例。BufferedIterator是Iterator的子特质，提供了一个额外的方法：`head`，调用它会返回迭代器的第一个元素，不过并不会将迭代器推进到下一步。用待缓冲的迭代器跳过空字符可以这样写：

```scala
def skipEmptyWords(it: BufferedIterator[String]) = {
  while (it.next().isEmpty){it.next()}
}
```

每个迭代器都可以转换为待缓冲的迭代器，方法是调用其buffered方法：

```scala
scala> val it = Iterator(1, 2, 3, 4)
it: Iterator[Int] = <iterator>

scala> val bit = it.buffered
bit: scala.collection.BufferedIterator[Int] = <iterator>

scala> bit.head //调用head并不会推进迭代器到下一步
res38: Int = 1

scala> bit.next() //与head返回的元素一致
res39: Int = 1

scala> bit.next()
res40: Int = 2
```

### 24.16 从头创建集合

Scala集合的一个通行功能，可以挑选任何一个集合名，然后用圆括号给出元素的列表来创建一个新的集合：

```scala
Traversable()           // 一个空的可遍历的对象
List()                  // 空列表
List(1.0, 2.0)          // 有两个元素的列表
Vector(1.0, 2.0)        // 有两个元素的向量
Iterator(1, 2, 3)       // 三个元素的迭代器
Set(dog, cat, bird)     // 动物的集合
HashSet(dog, cat, bird) // 同样动物组成的哈希集
Map('a' -> 7, 'b' -> 0) // 从字符到整数的映射
```

这样实现的背后，都是调用了某个对象的apply方法，第三行代码展开后就是：

```scala
List.apply(1.0, 2.0)
```

因此这是一个对List类的伴生对象的apply方法的调用。Scala类库中每个集合类都有一个带有这样的apply方法的伴生对象。无论具体的集合类，调用apply将会产出该特质的某种默认实现：

```scala
scala> List(1, 2, 3)
res41: List[Int] = List(1, 2, 3)

scala> Traversable(1, 2, 3)
res42: Traversable[Int] = List(1, 2, 3)

scala> scala.collection.mutable.Traversable(1, 2, 3)
res43: scala.collection.mutable.Traversable[Int] = ArrayBuffer(1, 2, 3)
```

另外，Scala中Seq特质的后代还通过伴生对象提供了��他工厂方法操作：

|操作|操作含义|
|--|--|
|S.empty|空序列|
|S(x, y, z)|由x, y, z组成的序列|
|S.concat(xs, ys, zs)|连接操作|
|S.fill(n)(e)|长度为n的序列，其中每个元素由表达式e计算|
|S.fill(m, n)(e)|大小为m×n的序列，其中每个元素由表达式e计算|
|S.tabulate(n)(f)|长度为n的序列，其中对下标i对应的元素有f(i)计算得出|
|S.tabulate(m, n)(f)|长度为m×n的序列，其中对下标i对应的元素有f(i)计算得出|
|S.range(start, end)|整数序列，从start到end，默认步长为1|
|S.range(start, end, step)|整数序列，从start到end，步长为step|
|S.iterate(x, n)(f)|长度为n的序列，元素值为x, f(x), f(f(x)), ...|

### 24.17 Java和Scala集合互转

跟Scala，Java也有很多类库，它们之间有很多相似之处，两个集合类库都有迭代器、集、映射和序列等等。当Scala更加强调了不可变集合，并提供了更多将集合变换为新集合的操作。

有时候可能需要从一个集合框架转换到另一个集合框架。例如，可能想要访问某个已有的Java集合，把它当做是Scala集合，又或者想要将某个Scala集合传递给某个预期Java集合的方法。这些都很容易做到，因为Scala在JavaConversions对象中提供了所有主要集合类型之间的隐式转化，具体来说能找到下列类型之间的双向转换：

```scala
Iterator       ⇔ java.util.Iterator
Iterator       ⇔ java.util.Enumeration
Iterable       ⇔ java.lang.Iterable
Iterable       ⇔ java.util.Collection
mutable.Buffer ⇔ java.util.List
mutable.Set    ⇔ java.util.Set
mutable.Map    ⇔ java.util.Map
```

要允许这些转换，只需要像这样做一次引入：

```scala
import scala.collection.JavaConversions._
```

现在就有了在Scala集合和对应的Java集合之间的相互转换能力：

```scala
scala> import collection.mutable._
import collection.mutable._

scala> val ju1: java.util.List[Int] = ArrayBuffer(1, 2, 3)

ju1: java.util.List[Int] = [1, 2, 3]

scala> val buf: Seq[Int] = ju1

buf: scala.collection.mutable.Seq[Int] = ArrayBuffer(1, 2, 3)

scala> val m: java.util.Map[String, Int] = HashMap("abc" -> 1, "hello" -> 2)

m: java.util.Map[String,Int] = {abc=1, hello=2}
```

在内部，这个转换是通过设置一个“包装”对象并将所有操作转发到底层集合对象来实现的。因此集合在Java和Scala席间转换时，并不会做拷贝。一个有趣的性质是，如果你完成一次往返的转换，比如将Java类型转换成对应的Scala的类型，并再转换回Java的类型，你得到的还是最开始的那个集合对象。

还有以下其他常用的Scala集合可以被转换为Java类型，不过并没有另一个方向的转化与之对应：

```scala
Seq         ⇒ java.util.List
mutable.Seq ⇒ java.util.List
Set         ⇒ java.util.Set
Map         ⇒ java.util.Map
```

由于Java并不在类型上区分可变集合和不可变集合。从collection.immutable.List转成java.util.List后，如果尝试对它进行变更操作，将会抛出UnsupportedOperationException：

```scala
scala> val ju1: java.util.List[Int] = List(1, 2, 3)
ju1: java.util.List[Int] = [1, 2, 3]

scala> ju1.add(4)
java.lang.UnsupportedOperationException
  at java.util.AbstractList.add(AbstractList.java:148)
  at java.util.AbstractList.add(AbstractList.java:108)
  ... 28 elided
```

## 25 Scala集合架构

本章详细描述了Scala集合框架的架构。继续第24章的主题，你会发现更多关于框架内部工作的信息。您还将了解此体系结构如何帮助您在重用框架中集合功能的绝大部分的同时，用几行代码定义自己的集合。

第24章列举了大量的集合操作，这些操作一致地存在于许多不同的集合实现中。为每种集合类型重新实现每种集合操作将导致大量重复代码，其中大部分代码将从其他地方复制。随着时间的推移，当操作在集合库的一部分中添加或修改而不在其他部分中添加或修改时，这种代码重复可能会导致不一致。新的集合框架的主要设计目标是避免任何重复，在尽可能少的地方定义每个操作。

设计方法是在集合中实现大多数操作可以灵活地从单个基类和实现继承的“模板”。在本章中，将研究这些模板，以及构成框架“构建块”的其他类和特征，以及它们所支持的构造原则。

### 25.1 构建器

几乎所有的集合操作都是通过遍历和构建器实现的。遍历可以由Traverable的foreach方法实现，构造一个新集合使用Builder类的实例。下面给出Builder的简要定义：

```scala
package scala.collection.generic

class Builder[-Elem, +To]{
  def +=(elem: Elem): this.type
  def result(): To
  def clear()
  def mapResult[NewTo](f: To => NewTo): Builder[Elem, NewTo] = {
    ......
  }
}
```

您可以使用`b += x`将元素x添加到构建器b中。还有一种语法可以一次性添加多个元素:例如，`b += (x, y)`和`b ++= xs`作为缓冲。(实际上，缓冲区是构建器的一个富包装版本)，result()方法从构建器返回一个集合。result()方法从构建器中返回一个集合，但可以使用clear()将其重置为新的空状态。构建器在元素类型Elem和它们返回的集合的To类型方面都是通用的。

通常，构建器可以引用其他构建器来组装集合的元素，但随后又希望转换其他构建器的结果。例如，为其提供一个不同的类型。这个任务通过类Builder中的mapResult方法得到简化。假设你有一个数组缓冲buf。数组缓冲本身就是构建器，因此获取数组缓冲的result()将返回它自己。如果你想使用这个缓冲产生一个生成器，构建数组，你可以使用mapResult:

```scala
scala> import scala.collection.mutable.ArrayBuffer
import scala.collection.mutable.ArrayBuffer

scala> val buf = new ArrayBuffer[Int]
buf: scala.collection.mutable.ArrayBuffer[Int] = ArrayBuffer()

scala> val bldr = buf mapResult (_.toArray)
bldr: scala.collection.mutable.Builder[Int,Array[Int]] = ArrayBuffer()
```

### 25.2 抽取公共操作☆

重新定义集合类型的主要设计目标是同时拥有自然的类型，以及在最大程度上共享实现代码。需要特别指出的是Scala集合遵循“相同结果类型”的原则：`之哟啊可能，对集合的变换操作将交出相同类型的集合`。举例来说，filter操作应该对所有集合类型交出相同集合类型的实例。对List应用filter应该得到List，对Map应用filter应该得到Map，以此类推。

Scala集合类库是通过使用所谓的`实现特质`中的`泛型的构建器`和`遍历`来避免代码重复达成“相同结果类型”原则的。这些特质的命名中都带有Like后缀：例如IndexedSeqLike是IndexSeq的实现特质，TraversableLike是Traversable的实现特质。像Traversable或IndexSeq这样的集合具体方法的实现都是从这些特质继承下来的。实现特质不同于一般的集合，它们有两个参数类型，不仅在`集合元素的类型`上是参数化的，在集合的`表现类型（representation type, 也就是底层的集合）`上也是参数化的，比如Seq[T]或List[T]。

举例来说，一下是TraversableLike特质的头部：

```scala
trait TraversableLike[+Elem, +Repr]{...}
```

类型参数Elem表示可遍历集合的元素类型，而类型参数Repr表示它的表现类型。对于Repr是什么并没有限制，Repr可以被实例化成不是Traversable的子类型。这样一来，位于集合继承关系之外的类，比如String和Array，也可以利用集合实现特质中定义的所有操作。

```scala
package scala.collection

trait TraversableLike[+Elem, +Repr]{
  def newBuilder: Builder[Elem, Repr]
  def foreach[U](f: Elem => U)
  ...
  def filter(p: Elem => Boolean): Repr = {
    val b = newBuilder
    foreach {elem => if(p(elem)) b += elem}
    b.result
  }
}
```

在特质Traversable中，filter只定义一次，但对所有的集合类都可用，对所有使用这些方法的集合的实现方式是一致的。首先newBuilder构造出一个新的元素表现类型为Repr的构建器，然后用foreach遍历当前集合的所有元素。

集合的map操作要更复杂一些，如果f是一个从String到Int的函数，而xs是一个List[String]，那么xs map f就应该得到List[Int]，反之亦然。不过如何在不重复定义列表和数组的map方法前提下做到这一点呢？

上例代码中的newBuilder和foreach不足以完成这个操作，因为它只允许创建相同集合类型的实例，但map需要一个相同的集合类型的构建器的实例，氮元素的类型可能不同，不仅如此，像map这样的函数的类型构造器可能还在很大程度上取决于其他入参类型，如下示例：

```scala
scala> import collection.immutable.BitSet
import collection.immutable.BitSet

scala> val bits = BitSet(1, 2, 3)
bits: scala.collection.immutable.BitSet = BitSet(1, 2, 3)

scala> bits map (_ * 2)
res1: scala.collection.immutable.BitSet = BitSet(2, 4, 6)

scala> bits map (_.toFloat)
res3: scala.collection.immutable.SortedSet[Float] = TreeSet(1.0, 2.0, 3.0)
```

如果将乘以2的函数应用到位组，将返回一个位组，但是如果将toFloat应用到位组，返回的确实一个通用的TreeSet[Float]，这样的结果当然不可能是位组，因为位组包含的是Int而不是Float。

注意，`map的结果了类型取决于传入函数的类型`。如果对于位组传入的是Int，那么map的结果就是BiSet，如果传入的是其他类型，那么map的结果就只是个Set。

BitSet的这个问题并非个案。再看下面一个例子：

```scala
scala> Map("a" -> 1, "b" -> 2) map {case (x, y) => (y, x)}
res4: scala.collection.immutable.Map[Int,String] = Map(1 -> a, 2 -> b)

scala> Map("a" -> 1, "b" -> 2) map {case (x, y) => y}
res5: scala.collection.immutable.Iterable[Int] = List(1, 2)
```

第一个例子将键值对的映射方向反转，事实上，第一个表达式交出的是原始映射的反转，前提是它是可以反转的。不过第二个将键值对映射成整数，也就是键值对中值的部分。这里并不能从它的结果做出一个Map，不过仍然可以做出一个Iterable，这个Map的超特质。

为什么不限制map只能返回一种集合？对于位组，map只接收Int到Int的函数，对于映射只接收对偶到对偶的函数。这样的做法是违背里氏替换原则：`Map是Iterable。因此任何在Iterable上的合法操作，也必须在Map上合法。`

Scala解决这个问题的方式是重载：不是Java采用的那种简单的重载，因为那样不够灵活，而是隐式参数提供的更系统化的重载。

```scala
def map[B, That](f: Elem => B)(implicit bf: CanBuildFrom[Repr, B, That]): That = {
  val b = bf(this)
  for (x <- this) b += f(x)
  b.result
}
```

这是TraversableLike的map实现，跟filter实现很像，主要区别在于filter用的是TraversableLike抽象方法newBuilder，而map用的是一个额外的隐式参数的形式传入的类型为CanBuildFrom的**构建器工厂(builder factory)**。

```scala
package scala.collection.generic

trait CanBuildFrom[-From, -Elem, To]{
  //创建新的构建器
  def apply（from: From): Builder[Elem, To]
}
```

该特质代表了构建器工厂，它有三个类型参数：`Elem`代表要构建的集合的元素类型，`To`表示要构建的集合的类型，而`From`表示要应用到该构建器工厂的类型。通过定义正确的构建器工厂的隐式定义，可以按需定制正确的类型行为。

以BitSet为例，它的伴生对象可以包含一个类型为CanBuildFrom[BitSet, Int, BitSet]的构建器工厂。这意味着当操作一个BitSet时，可以构造出另一个BitSet，只要要构建的集合的元素类型为Int。如果不是这样，总是可以退而求其次，采用另一个隐式构建器工厂，一个在mutable。Set的伴生对象中实现的隐式构建器工厂。这个更通用的构建器工厂定义为（其中A为泛型的类型参数）：

```scala
CanBuildFrom[Set[_], A, Set[A]]
```

这意味着，当操作一个以Set[_]通配类型表示的任意类型的Set时，仍然可以构建出一个Set，而不论元素类型A是什么。有了这两个CanBuildFrom的隐式实例，就可以依赖Scala的隐式解析规则来选取合适的并且是最具体的那个构建器工厂了。

隐式解析对那那些比较麻烦的操作，比如map，提供了正确的静态类型，不过，动态类型会怎么样？确切地说，如果你有一个列表的值，其静态类型为Iterable，然后你对这个值map了某个函数：

```scala
scala> val xs: Iterable[Int] = List(1, 2, 3)
xs: Iterable[Int] = List(1, 2, 3)

scala> val ys = xs map (x => x * x)
ys: Iterable[Int] = List(1, 4, 9)
```

ys的静态类型为Iterable，不过它的动态类型为List！做到这一层，需要额外的处理机制。CanBuildFrom的apply方法接收原集合为入参传入。泛型可遍历集合的大多数构建器工厂将这个调用转发到集合的genericBuilder方法。这个genericBuilder方法进而调用属于该集合的构建器。也就是说，`Scala用静态的隐式解析规则来解决map类型约束，用虚拟分发来选择与这些约束相对应的最佳动态（运行时）类型`。

### 25.3 集成新的集合

如果想集成一个新的集合类，让它能够以正确的类型利用所有预定义的操作，需要怎么做？

#### 25.3.1 集成序列

假设要创建一个新的序列类型来表示RNA链，一个由4中碱基组成的序列：A（腺嘌呤，adenine）、T（胸腺嘧啶，thymine)、G（鸟嘌呤，guanine）和U（尿嘧啶，uracil）。碱基的定义很容易：

```scala
abstract class Base
case object A extends Base
case object T extends Base
case object G extends Base
case object U extends Base

object Base{
  val fromInt: Int => Base = Array(A, T, G, U)
  val toInt: Base => Int = Map(A -> 0, T -> 1, G -> 2, U -> 3)
}
```

每个碱基都定义为一个继承自公共抽象类Base的样例对象。Base类有一个伴生对象，该伴生对象定义了两个函数。

接下来定义RNA链，从概念上讲RNA链就是一个Seq[Base]。不过RNA链会很长，可以做出一个紧凑的表现形式。由于只有4中碱基，每个碱基可以用2个比特位来标识。下面是第一版实现：

```scala
import scala.collection.IndexedSeqLike
import scala.collection.mutable.{Builder, ArrayBuffer}
import scala.collection.generic.CanBuildFrom

final class RNA1 private (val groups: Array[Int], val length: Int) extends IndexedSeq[Base]{
  import RNA1._
  def apply(idx: Int): Base = {
    if (idx < 0 || length <= idx)
      throw new IndexOutOfBoundsException
    Base.fromInt(groups(idx / N) >> (idx % N * S) & M)
  }
}

object RNA1{
  private val S = 2
  private val N = 32 / S
  private val M = (1 << S) - 1
  def fromSeq(buf: Seq[Base]): RNA1 = {
    val groups = new Array[Int]((buf.length + N - 1) / N)
    for (i <- 0 until buf.length)
      groups(i / N) |= Base.toInt(buf(i)) << (i % N * S)
    new RNA1(groups, buf.length)
  }
  def apply(bases: Base*) = fromSeq(bases)
}
```

- 第一个入参接收Int数组为首个入参的构造方法，包含了紧凑格式的RNA数据，每个元素包含16个碱基，除了最后一个元素外，这个元素可能是部分填充的。
- 第二个参数length表示数组中碱基的数量。
- RNA1扩展自IndexedSeq[Base]，该特质定义了两个抽象方法：length和apply。
- RNA1类通过定义同名的参数化字段自动实现了length。还用上例给出的代码实现了索引方法apply。本质上讲，apply首先从groups数组中提取出一个整数值，然后用位右移(>>)和位与(&)从这个整数值中提取出正确的两比特位表示的数。
- 私有常量S、N和M来自RNA1的伴生对象。S为每个包的大小(2);N为每个整数代表的两比特的包的个数；M是从整数中分理出最低位的S包的掩码。

RNA1的构造方法是私有的，这样的设计对RNA序列的接口和实现做了很好的解耦。这样没法用new构造RNA序列，但是可以使用RNA1伴生对象提供两种可选的创建RNA序列的方式。

- 第一种是fromSeq方法，将给定的碱基的序列（即类型为Seq[Base]的值）转换为RNA1类的实例。fromSeq方法将入参序列包含的所有碱基打包成数组，然后用这个数组和原始序列的长度作为土蚕调用RNA1的私有构造方法。这个做法利用了类的构造方法对伴生对象可见这一事实。
- 第二种是通过RNA1的apply方法。这个方法接收可变数量的Base入参，然后简单地将它们作为序列转发给fromSeq。

这两种创建机制实际运行效果：

```scala
scala> val xs = List(A, G, T, A)
xs: List[Product with Serializable with Base] = List(A, G, T, A)

scala> RNA1.fromSeq(xs)
res7: RNA1 = read.RNA1(A, G, T, A)

scala> val rna1 = RNA1(A, U, G, G, T)
rna1: RNA1 = read.RNA1(A, U, G, G, T)
```

#### 25.3.2 适应RNA方法的结果类型

以下是更多基于RNA1的交互：

```scala
scala> rna1.length
res8: Int = 5

scala> rna1.last
res10: Base = T

scala> rna1.take(3)
res11: IndexedSeq[Base] = Vector(A, U, G)
```

前两个结果是符合预期的，但是第三个就不一定了，看到的静态类型是IndexedSeq[Base]，而动态类型为`Vector的结果值`，你可能预期看到是`RNA1的值`。不过这是不可能的！因为定义RNA1时，是从IndexedSeq扩展。而IndexedSeq类是有一个返回IndexedSeq的take方法，而这个方法是基于IndexedSeq的默认实现，也就是Vector。

要改变这个行为，就是重写take方法，像这样：

```scala
def take(count: Int): RNA1 = RNA1.fromSeq(super.take(count))
```

这样对于take操作是够了，但是对于drop、filter和init这样的方法，不可能每个都要重写。幸好有一种很简单的方式可以达到同样的效果，RNA不仅需要继承IndexedSeq，还需要继承它的实现特质IndexedSeqLike。

```scala
final class RNA2 private (
  val groups: Array[Int],
  val length: Int
) extends IndexedSeq[Base] with IndexedSeqLike[Base,RNA2]{
  import RNA2._
  override def newBuilder: Builder[Base, RNA2] = {
    new ArrayBuffer[Base] mapResult fromSeq
  }
  def apply(idx: Int): Base = {
    if (idx < 0 || length <= idx)
      throw new IndexOutOfBoundsException
    Base.fromInt(groups(idx / N) >> (idx % N * S) & M)
  }
}

object RNA2{
  private val S = 2
  private val N = 32 / S
  private val M = (1 << S) - 1
  def fromSeq(buf: Seq[Base]): RNA2 = {
    val groups = new Array[Int]((buf.length + N - 1) / N)
    for (i <- 0 until buf.length)
      groups(i / N) |= Base.toInt(buf(i)) << (i % N * S)
    new RNA2(groups, buf.length)
  }
  def apply(bases: Base*) = fromSeq(bases)
}
```

如果漏掉newBuilder的定义，可能会得到如下这样的错误：

```scala
error: overriding method newBuilder in trait TraversableLike of type => scala.collection.mutable.Builder[Base,RNA2];
 method newBuilder in trait GenericTraversableTemplate of type => scala.collection.mutable.Builder[Base,IndexedSeq[Base]] has incompatible type
final class RNA2 private (
            ^
```

提示这里需要的是一个结果类型为Builder[Base, RNA2]的newBuilder方法，但编译器找到是一个结果类型为Builder[Base, IndexedSeq[Base]]的方法。后者并不会重写前者。

第一个方法，也就是结果类型为Builder[Base, RNA2]的，是RNA2定义中，通过RNA2这个类型参数传给IndexedSeqLike后得到以实例化的抽象方法。第二个方法，也就是结果类型为Builder[Base, IndexedSeq[Base]]的，是被继承的IndexedSeq类提供的。换句话说，RNA2类如果没有钱一个结果类型的newBuilder定义，它就是非法的。

改良后的RNA2看起来就符合预期了：

```scala
scala> val rna2 = RNA2(A, U, G, G, T)
rna2: RNA2 = read.RNA2(A, U, G, G, T)

scala> rna2 take 3
res13: RNA2 = read.RNA2(A, U, G)

scala> rna2 filter (_ != U)
res14: RNA2 = read.RNA2(A, G, G, T)
```

#### 25.3.3 处理map等方法

集合中还有一类方法没有处理，这些方法并不总是返回确定的集合类型，他们可能返回同一个集合，但是是不同的元素类型。经典的就是map方法。如果s是一个Seq[Int]，而f是一个从Int到String的函数，那么s.map(f)将返回Seq[String]。这样一来，在调用方接受的结果之间，元素的类型变了，但集合的种类保持不变。

还有一些比如追加方法++，向Int列表中追加String列表的结果是一个Any列表。那么针对RNA链，希望对RNA链执行碱基到碱基的映射应该仍然交出RNA链：

```scala
scala> val rna = RNA(A, U, G, G, T)
rna: RNA = RNA(A, U, G, G, T)

scala> rna map{case A => T case b =>b}
res16: RNA = RNA(A, U, G, G, T)

scala> rna ++ rna
res17: RNA = RNA(A, U, G, G, T)(A, U, G, G, T, A, U, G, G, T)
```

另一方面，对RNA链执行碱基到其他类型的映射没法交出另一个RNA链，因为新元素的类型不对，只能交出一个序列。同理，将类型不是Base的元素追加到RNA链可以交出一个通用的序列，但是一定不是RNA链。

```scala
scala> rna map Base.toInt
res20: IndexedSeq[Int] = Vector(0, 3, 2, 2, 1)

scala> rna ++ List("missing", "data")
res21: IndexedSeq[Object] = Vector(A, U, G, G, T, missing, data)
```

但是，针对目前RNA的定义是得不到期望的结果：

```scala
scala> rna map{case A => T case b =>b}
res16: IndexedSeq[Base] = Vector(T, U, G, G, T)

scala> rna ++ rna
res17: IndexedSeq[Base] = Vector(A, U, G, G, T, A, U, G, G, T)
```

所以map和++的记过无论如何都不会是RNA链，哪怕生成的集合元素类型都是Base。为了做的很好，可以仔细看下一些map方法的签名（++也有类似的签名）。map方法最开始是在scala.collection.TraversableLike类中定义的，签名如下：

```scala
def map[B, That](f: Elem => B)(implicit cbf: CanBuildFrom[Repr, B, That]): That
```

Elem是集合的元素类型，Repr是集合本身的类型，也就是传入TraversableLike和IndexedSeqLike这些实现类的第二个类型参数。map方法额外接收两个类型的参数，B和That。参数B表示映射函数的结果类型，这也是新集合元素的类型，参数That出现在map的结果类型上，因此它代表了新创建的集合的类型。

That类型如何确定？它通过类型为CanBuildFrom[Repr, B, That]的隐式参数cbf跟其他类型链接起来。这些CanBuildFrom的隐式值由集合类个自定义。从本质上讲，类型为`CanBuildFrom[From, Elem, To]`的隐式值表达的意思是：`有一种方式，将给定一个类型为From的集合，可以用类型为Elem的元素构建出一个类型为To的集合`。

前面的代码没有得到期望的结果，是因为没有创建RNA2序列的CanBuildFrom实例，因此编译器能找到的次佳选择就是RNA2继承的IndexedSeq的伴生对象中的CanBuildFrom了。那个CanBuildFrom隐式值创建的是IndexedSeq，这也是对rna2应用map时看到的结果类型。

要实现期望的返回结果，需要在RNA类的伴生对象中定义一个CanBuildFrom的隐式实例，这个实例的类应该是`CanBuildFrom[RNA, Base, RNA]`。这个实例就是要`从给定的RNA链和一个新元素类型Base构建出另一个RNA链`。

跟RNA2的实现有两点不同。

- 首先，NewBuilder的实现从RNA类移到了伴生对象中。RNA类中的newBuilder方法只是简单地将调用转发过去。
- 其次，RNA对象中现在有一个CanBuildFrom的隐式值，要创建这样一个对象，需要定义CanBuildFrom的两个apply方法。

这两个方法都会创造RNA集合的构建器，不过参数列表不同。

- `apply()`只是简单地创建出正确类型的构建器。
- `apply(from)`方法将原始的集合作为入参。这样做有助于将构建器的返回类型的动态（运行时）类型适配成接收方（被调用方）的动态（运行时）类型。

对RNA而言，这种静态类型和动态类型不一致的情况并不会发生，因为RNA类时final的，因此任何静态类型为RNA的接收方，其动态类型也一定是RNA。这就是为什么apply(from)也是简单地调用newBuilder，直接忽略掉入参。

```scala
final class RNA private (
  val groups: Array[Int],
  val length: Int
) extends IndexedSeq[Base] with IndexedSeqLike[Base,RNA]{
  import RNA._

  override protected[this] def newBuilder: Builder[Base, RNA] = {
    RNA.newBuilder
  }

  def apply(idx: Int): Base = {
    if (idx < 0 || length <= idx)
      throw new IndexOutOfBoundsException
    Base.fromInt(groups(idx / N) >> (idx % N * S) & M)
  }

  override def foreach[U](f: Base => U): Unit = {
    var i = 0
    var b = 0
    while (i < length){
      b = if(i % N == 0) groups(i / N) else b >>> S
      f(Base.fromInt(b & M))
      i += 1
    }
  }
}

object RNA{
  private val S = 2 //单组的比特数
  private val N = 32 / S //用于分离出单个组的掩码
  private val M = (1 << S) - 1 //一个Int可以容纳的组的个数
  
  def fromSeq(buf: Seq[Base]): RNA = {
    val groups = new Array[Int]((buf.length + N - 1) / N)
    for (i <- 0 until buf.length)
      groups(i / N) |= Base.toInt(buf(i)) << (i % N * S)
    new RNA(groups, buf.length)
  }
  
  def apply(bases: Base*) = fromSeq(bases)

  def newBuilder: Builder[Base, RNA] = {
    new ArrayBuffer mapResult fromSeq
  }

  implicit def canBuildFrom: CanBuildFrom[RNA, Base, RNA] = {
    new CanBuildFrom[RNA, Base, RNA]{
      def apply(): Builder[Base, RNA] = newBuilder
      def apply(from: RNA): Builder[Base, RNA] = newBuilder
    }
  }
}
```

这样完善定义之后，返回的结果就是我们期望的类型：

```scala
scala> val rna = RNA(A, U, G, G, T)
rna: RNA = read.RNA(A, U, G, G, T)

scala> rna map {case A => T case b => b}
res29: RNA = read.RNA(T, U, G, G, T)

scala> rna ++ rna
res30: RNA = read.RNA(A, U, G, G, T, A, U, G, G, T)
```

在上述优化定义时，重写了foreach方法。IndexedSeq中foreach的标准实现只是简单地用apply选择集合中的第i个元素，其中i的取值范围是0到集合的长度减1。因此这个标准实现在选择RNA链中每个元素是都会从数组选择一个元素并从中解包出一个碱基。RNA类重写的foreach更加聪明，每当它选中数组中的一个元素，都会立即对钙元素包含的所有碱基应用给定的函数。这样就大大减轻了从数组选择和按位解包的负担。

#### 25.3.4 总结

如果想要完整地将一个新的集合集成到框架中，需要注意如下几点：

1. 决定该集合是可变的还是不变的。
2. 选择合适的特质作为集合的基础。
3. 从合适的实现特质继承来实现大多数集合操作。
4. 如果你要想map和类似操作返回你的集合类型，在你的伴生对象中提供一个隐式的CanBuildFrom。

## 26 提取器

Scala中，习惯使用模式匹配精确地拆解和分析数据的方式，本章将这个概念进一步泛化。到目前为止，构造方法模式都跟样例类有关。例如，Some(x)是一个合法的模式，因为Some是一个样例类。有时候想写这样的模式，但并不需要创建关联的样例类。这样的模式，提取器是一种实现手段。

### 26.1 提取电子邮件地址

对于给定的字符串，要判断它是不是电子邮件，如果是提取用户部分和域名部分。传统方式是用三个助手函数：

```scala
def isEMail(s: String): Boolean
def domain(s: String): String
def user(s: String): String
```

这样就可以解析给定的字符串：

```scala
if (isEMail) println(user(s) + " AT " + domain(s))
else println("not an email address")
```

但是这样做不够灵活，如果想要找到列表中连续两个字符都是同一个用户的邮件地址，可以用前面定义的访问函数来看看到底有多麻烦。

假定可以使用模式匹配字符串：

```scala
EMail(user, domain)
```

然后通过内嵌的“@”符号，来匹配用户和域名：

```scala
s match{
  case EMail(user, domain) => println(user + " AT " + domain)
  case _ => println("not an email address")
}
```

更复杂的连续出现的同一个用户的两个电子邮件地址的问题，可以翻译如下：

```scala
ss match{
  case EMail(u1, d1) :: EMail(u2, d2) :: _ if (u1 == u2) => ...
}
```

不过这里有个问题，就是字符串并不是样例类，它们没有符合Email(user, domain)的表现形式。这就需要提取器来实现，`有了提取器，模式并不需要遵循从类型的内部表现形式`。

### 26.2 提取器

提取器拥有名为unapply的成员方法的对象，对某个值做匹配并将它拆开；apply是构建器（可选的），是unapply反方向操作。

```scala
object EMail{
  def apply(user: String, domain: String) = user + "@" + domain
  def unapply(str: String): Option[(String, String)] = {
    val parts = str split "@"
    if (parts.length == 2) Some(parts(0), parts(1)) else None
  }
}
```

如果想更明显地表明意图，还可以让EMail继承Scala的函数类型：

```scala
object EMail extends ((String, String) => String){...}
```

如果str不是电子邮件，就会返回None。

```scala
unapply("John@example.com") equals Some("John", "example.com")
unapply("John Doe") equals None
```

每当模式匹配遇到引用提取器对象的模式时，会调用提取器的unapply方法：

```scala
selectorString match{case EMail(user, domain) => ...}
```

会引发如下调用：

```scala
EMail.unapply(selectorString)
```

selectorString的类型String满足unapply的参数类型，但是这并不是必须的，可以在匹配之前进行检查：

```scala
val x: Any = ...
x match {case EMail(user, domain) => ...}
```

如果x不满足，立即失败，如果满足，继续进行匹配。

在对象Email中，apply方法被称为`注入`，unapply被称为`提取`。如果在对象总只定义了提取逻辑，则这个对象被称为`提取器`。

如果包含了“注入”方法，那么应该是”提取“方法的对偶(dual)。举例来说：

```scala
EMail.unapply(Email.apply(user, domain))
```

应该返回：

```scala
Some(user, domain)
```

也就是Some包起来的同一个入参序列，反方向意味着首先执行unapply再执行apply：

```scala
EMail.unapply(obj) match{
  case Some(u, d) => Email.apply(u, d)
}
```

这段代码中，如果obj的匹配成功了，应该用apply取回的是同一个对象。这就是apply和unapply的对偶性设计。

### 26.3 提取0个或1个变量的模式

前面的unapply方法如果匹配成功，返回的是一对元素值。这很容易泛化成多个变量的模式。如果要绑定N个变量，**unapply可以返回一个以`Some`包起来的N个元素的元组**。

不过当模式只绑定一个变量时，处理逻辑是不同的，Scala并没有单个元素的元组。为了只返回单个模式元素，unapply方法只是简单地将元素本身放在Some里，下面给出的提取器对象顶一个针对那些由连续两个出现的相同字符串组成的字符串的apply和unapply方法：

```scala
object Twice{
  def apply(s: String): String = s + s
  def unapply(s: String): Option[String] = {
    val length = s.length / 2
    val half = s.substring(0, length)
    if (half == s.substring(length)) Some(half) else None
  }
}
```

也有可能某个提取其模式并不绑定任何变量，这是unapply就会返回布尔值，如只返回全部为大写字母的s：

```scala
object UpperCase {
  def unapply(s: String): Boolean = s.toUpperCase == s
}
```

下面的useTwiceUpper函数在它的欧式匹配代码中同时应用了前面定义的所有提取器：

```scala
def useTwiceUpper(s: String) = s match {
  case EMail(Twice(x @ UpperCase()), domain) =>
    "match: " + x + " in domain " + domain
  case _ => "no match"
}
```

该函数的第一个模式匹配的是所有连续两个大写用户名的同一个字符串的电子邮件地址的字符串(x @ UpperCase是将x和UpperCase匹配的模式关联起来，详见15.2章节)：

```scala
scala> useTwiceUpper("DIDI@hotmail.com")
res0: String = match: DI in domain hotmail.com

scala> useTwiceUpper("DIDO@hotmail.com")
res1: String = no match

scala> useTwiceUpper("didi@hotmail.com")
res2: String = no match
```

### 26.4 提取可变长参数的模式

之前定义的unapply提取方法，返回的元素数量是固定的，有时候不够灵活，有时想返回域名中的每个部分，可以使用下面模式：

```scala
dom match{
  case Domain("org", "acm") => println("acm.org")
  case Domain("com", "sun", "java") => println("java.sun.com")
  case Domain("net", _*) => println("a .net domain")
}
```

参数列表的最后序列通配模式_*会匹配序列中任何剩余的元素。提取器支持前面例子中的**变长参数匹配**，unapply无法完成这个任务，但是有一个方法专门处理变长参数匹配：`unapplySeq`。

```scala
object Domain{
  def apply(parts: String*): String = {
    parts.reverse.mkString(".")
  }

  def unapplySeq(whole: String): Option[Seq[String]] = {
    Some(whole.split("\\.").reverse)
  }
}
```

unapplySeq的结果类型必须符合Option[Set[T]]的要求，其中元素类型T可以是任意类型。

可以用Domain提取器获取电子邮件地址字符串的更详细的信息。例如，要查找某个“.com”域名下的某个名为”tom“的电子邮件地址：

```scala
def isTomDotCom(s: String): Boolean = s match{
  case EMail("tom", Domain("com", _*)) => true
  case _ => false
}
```

给出了似乎是预期的结果：

```scala
scala> isTomDotCom("tom@sun.com")
res3: Boolean = true

scala> isTomDotCom("peter@sum.com")
res4: Boolean = false

scala> isTomDotCom("tom@acm.org")
res5: Boolean = false
```

从unapplySeq返回某些固定的元素再加上可变的部分也是可行的。这是通过将所有的元素放在元组里返回来实现的，其中可变部分出现在元素的最后一位，就像往常一样，将域名部分展开成序列：

```scala
object ExpandedEMail{
  def unapplySeq(email: String): Option[(String, Seq[String])] = {
    val parts = email split "@"
    if (parts.length == 2)
      Some(parts(0), parts(1).split("\\.").reverse)
    else
     None
  }
}
```

ExpandedEMail的unapplySeq方法返回一个类型为Tuple2的可选值。其中第一个元素为用户名部分，第二个元素表示域名的序列，可以向往常一样用它来做模式匹配：

```scala
scala> val s = "tom@support.com.cn"
s: String = tom@support.com.cn

scala> val ExpandedEMail(name, topdom, subdoms @ _*) = s
name: String = tom
topdom: String = cn
subdoms: Seq[String] = WrappedArray(com, support)
```

### 26.5 提取器和序列模式

在15.2章节，可以使用序列模式来访问列表或数组的元素：

```scala
List()
List(x, y, _*)
Array(x, 0, 0, _)
```

事实上，Scala标准库中额这些序列模式都是用提取器实现的。举例来说，形如List(...)这样的模式之所以可行，是因为scala.List的伴生对象是一个定义了unapplySeq方法的提取器，如下：

```scala
package scala
object List{
  def apply[T](elem: T*) = elems.toList
  def unapplySeq[T](x: List[T]): Option[Seq[T]] = Some(x)
  ...
}
```

List对象包含一个接收可变数量的入参的apply方法。正是这个方法让你可以编写这样的表达式：

```scala
List()
List(1, 2, 3)
```

对于数组操作，也能找到非常类似的定义，这些定义支持针对数组的“注入”和“提取”。

### 26.2 提取器和样例类的对比

样例类非常有用，但是有一个缺点：`它们将数据的具体表现类型暴露给了使用方`。这意味着构造方法模式中使用的类名跟选择器对象的具体类型相关。如果如下模式的匹配：

```scala
case C(...)
```

成功了，你就知道选择器表达式是C这个类的实例。

提取器支持的模式跟被选择的对象的数据类型没有任何关系，这性质被称为**表现独立**。表现独立在开发大型项目时非常重要，因为它允许我们修改某些组件的实现类型，同时又不影响这些组件的使用方。

样例类和提取器的区别：

- 如果你的组件定义并输出了样例类，你就没法修改这些样例类，因为会影响使用方的代码。
- 提取器没有这个问题，更改某个类型的表现形式不会影响使用方。
- 样例类设置和定义比提取器简单很多，并且能带来更加高效的模式匹配，因为编译器能够对使用样例类的模式做更好的优化。
- 提取器的unapply或unapplySeq几乎可以做任何事。
- 如果样例类继承自一个sealed的基类，编译器会自动检查模式匹配是否全面，而对提取器不会有这样的检查。

两者之间如何选择？刚开始构建的项目，总是可以从样例类开始i，随着需求的出现，再改成提取器。由于用提取器的模式和用样例类的模式在Scala中看上去完全相同，使用方中的代码看上去完全相同，使用方代码中的模式匹配仍然可以继续工作。

### 26.7 正则表达式

提取器一个尤其有用的应用场景是正则表达式。

#### 26.7.1 组织正则表达式

Scala从Java继承了正则表达式的语法。Scala的正则表达式类位于scala.util.matching包中。

```scala
import scala.util.matching.Regex
```

新的正则表达式值是通过将一个字符串传给Regex构造方法创建的。

```scala
scala> import scala.util.matching.Regex
import scala.util.matching.Regex

scala> val Decimal = new Regex("(-)?(\\d+)(\\.\\d*)?")
Decimal: scala.util.matching.Regex = (-)?(\d+)(\.\d*)?
```

\\表示转义\，返回一个\。在代码中过多的转义字符可能不那么易读，可以用一下代码替换：

```scala
scala> val Decimal = new Regex("""(-)?(\d+)(\.\d*)?""")
Decimal: scala.util.matching.Regex = (-)?(\d+)(\.\d*)?
```

一种更短的表达式为：

```scala
scala> val Decimal = """(-)?(\d+)(\.\d*)?""".r
Decimal: scala.util.matching.Regex = (-)?(\d+)(\.\d*)?
```

换句话说，只要在字符串后面追加一个`.r`就能得到一个正则表达式。这样之所以可行，是因为StringOps类里有一个名为r的方法，将字符串转换为正则表达式。

```scala
package scala.runtime
import scala.util.matching.Regex

class StringOps(self: String) ... {
  ...
  def r = new Regex(self)
}
```

#### 26.7.2 查找正则表达式

可以用一下几种不同的操作符在字符串中查找正则表达式：

- `regex findFirstIn str`：在字符串str中查找第一个正则表达式regex，以Option类型返回结果。
- `regex findAllIn str`：在字符串str中查找正则表达式regex，以Iterator类型返回结果。
- `regex findPrefixOf str`：在字符串str的一开始查找正则表达式regex，以Iterator类型返回结果。

```scala
scala> val input = "for -1.0 to 99 by 3"
input: String = for -1.0 to 99 by 3

scala> for (s <- Decimal findAllIn input) println(s)
-1.0
99
3

scala> Decimal findFirstIn input
res8: Option[String] = Some(-1.0)

scala> Decimal findPrefixOf input
res10: Option[String] = None
```

#### 26.7.3 用正则表达式提取信息

在Scala中每个正则表达式都定义一个提取器。该提取器用来识别正则表达式中的组匹配的子字符串，可以像下面拆解一个十进制的字符串：

```scala
scala> val Decimal(sign, integerpart, decimalpart) = "-1.23"
sign: String = -
integerpart: String = 1
decimalpart: String = .23
```

正则表达式使用(-)?(\d+)(\.\d*)?中的三个组相对应的不跟就被作为模式的元素返回，进而被模式变量sign, integerpart, decimalpart匹配，吐过某个组缺失了，对应的元素就被设为null。

```scala
scala> val Decimal(sign, integerpart, decimalpart) = "-123"
sign: String = -
integerpart: String = 123
decimalpart: String = null
```

也可以在for表达式中混用提取器和正则表达式的查找，例如对十进制的字符串做拆解：

```scala
for (Decimal(s, i, d) <- Decimal findAllIn input)
  println("sign: " + s + ", integer: " + i + ", decimal: " + d)
sign: -, integer: 1, decimal: .0
sign: null, integer: 99, decimal: null
sign: null, integer: 3, decimal: null
```

## 27 注解

注解是添加到程序代码中的结构化信息。跟注释一样，他们可以出现在程序的任何位置，被附加到任意变量、方法、表达式或其他程序元素上。跟注释不同，它们有结构，因而更易于机器处理。

### 27.1 为什么要有注解

程序除了被编译和运行外，还可以做很多事情，比如：

1. 自动生成文档，就像Scaladoc那样。
2. 格式化代码，让它符合你喜欢的风格。
3. 检查常见的代码问题，比如打开了文件但是在某些控制路径上忘记关闭它。
4. 实验性的类型检查，比如管理副作用或确保所有者性质。

这样的工具被称为**元编程工具**，因为它们是`把其他程序当做输入的程序`。注解通过`让程序员在他们的源代码中添加指令的方式来支持这些工具`。这些指令让工具（相对于没有用户输入是）更加高效。例如，注解可以在吐下方面改进前面列出的工具。

1. 文档生成器可以被告知某个方法已过时。
2. 代码格式化工具可以被告知跳过某些手工格式化的部分。
3. 未关闭文件的检查器可以被告知忽略某个特定的经过手动验证已关闭的文件。
4. 副作用检查器可以被告知需要验证某个方法是没有副作用的。

所有这些情况，从理论上讲编程语言都有可能提供插入额外信息的方式。事实上，几乎所有的这些提到的功能都有编程语言是直接支持的。不过，工具很多，单个编程语言很难全部直接支持，所有这些信息都完全被编译器忽略，毕竟编译器只想让代码跑起来。

Scala处理这类问题的哲学是在核心语言总包含最小的、正交的支持，让大量各式各样的元编程工具可以被写出来。这里的最小支持就是另一个注解的系统。编译器只需要理解一个功能特性，那就是注解，但是并不是每个注解附加任何含义。这样每个元编程工具就可以定义它们自己的特定的注解。

### 27.2 注解的语法

一个典型的注解用法如下：

```scala
@deprecated def bigMistake() = //...
```

@deprecated是注解，对bigMistake方法有效。这个方法被标记成bigMistake的作者不希望你是用的东西，也许bigMistake在将来某个版本移除。

注解还可以用在各种生命或定义上，包括val, var, def, class, object, trait和type。注解对于跟在它后面的整个生命或定义有效。

```scala
@deprecated class QuickDirty {  //用于class
  ...
}

(e: @unchecked) match{ //用于表达式
  ...
}
```

还有更加丰富的一般形式：

```scala
@annot(exp1, exp2, ...)
```

其中，`annot部分是注解名。所有的注解都必须包含这个`，*exp*的部分是给注解的入参。对于`@deprecated`这样的注解而言，它们并不需要任何入参，通常可以省去圆括号，不过也可以写为`@deprecated()`。对于那些确实需要注解入参的，需要将入参放在圆括号中，例如@serial(1234)。

提供给注解的入参的具体形式取决于特定的注解类，大多数注解处理器只允许你提供直接就是常量的值，比如123或hello，不过编译器本身支持任意表达式，只要它们能通过类型检查。某些注解类可以利用这一点，比如引用作用域内其他变量：

```scala
@cool val normal = "Hello"
@coolerThan(normal) val fonzy = "Heeyyy"
```

Scala内部`将注解表示为仅仅是对某个注解类的构造方法的调用`（如果将@替换为new，就能得到一个合法的创建实例的表达式）。这意味着`编译器可以很自然地支持注解带名参数和默认参数`，因为Scala已经支持方法和构造方法调用的带名参数和默认参数。

一个比较麻烦的点是关于那些从概念上讲`接收其他注解作为入参的注解`，某些框架需要这个。不能直接把注解当做另一个注解的入参，因为注解并不是合法的表达式，在这种情况下，必须用new而不是@，参考如下例子：

```scala
scala> import scala.annotation._
import scala.annotation._

scala> class strategy(arg: Annotation) extends Annotation
defined class strategy

scala> class delayed extends Annotation
defined class delayed

scala> @strategy(@delayed) def f() = {}
<console>:1: error: illegal start of simple expression
       @strategy(@delayed) def f() = {}
                 ^

scala> @strategy(new delayed) def f() = {}
f: ()Unit
```

### 27.3 标准注解

#### 27.3.1 过时(deprecation)

有时候写过的类或方法，过段时间以后看起来有问题，但是其他人可能有调用到，因此不能简单地删掉这个昂发，因为这会让别的代码也不能正常编译。可以简单地在方法写上@deprecated来标记为过时，例如：

```scala
@deprecated def bigMistake() = //...
```

如果以入参的形式给@deprecated提供一个自渡船，那么这个字符串就会随着过时警告一起提示出来：

```scala
@deprecated("use newShinyMethod() instead")
def bigMistake() = //...
```

#### 27.3.2 易失(volatile)字段

并发编程跟共享可变状态并不是很合得来。因此，Scala并发编程的重点是消息传递和尽量少的共享可变状态。

尽管如此，有时候还是想在并发编程中使用可变状态，@volatile注解就非常有帮助。它告诉编译器，这个变量会被多个线程时候用，这样的变量实现的效果使读写更慢，但是从多个线程访问是的行为更可预期。

@volatile关键字在不同平台上有不同的保证。不过在Java平台上，得到的行为就跟用Java代码的volatile修饰符一样。

#### 27.3.3 二进制序列化

序列化框架帮助我们将对象转换成字节流，或者将字节流还原对象。Scala并没有自己的序列化框架，应该使用底层平台提供的框架。Scala能做的是提供三个不同框架使用的注解。针对Java平台的Scala编译器会以Java的方式解释这些注解。

1. 第一个注解`表示某个类是否支持序列化`，默认情况下被认为可序列化的，应该给所有应该被序列化的类加上`@serializable`注解。
2. 第二个注解帮助`处理随着时间推移会发生变化的可序列化的类`。可以添加`@SerialVersionUID(1234)`这样的注解来对某个类的当前版本带上一个序列号(1234)。序列化框架应该将这个序列号保存在生成的字节流中。当时稍后从改字节流重新载入并尝试转换成对象时，框架可以检查对应的类的当前版本是否跟字节流里的版本号一致。当你想对你的类做一个序列化不兼容的修改，你可以修改这个版本号，框架就会自动拒绝载入老版本的实例。
3. Scala还提供了一个`@transient`注解，用来标记那些完全不应该被序列化的字段。

#### 27.3.4 自动的get和set方法

Scala代码通常不需要显式地给出字段的get和set方法，因为Scala混合了字段访问和方法调用的语法。不过有一些平台特定的框架会预期get和set方法。为此，Scala提供了`scala.reflect.BeanProperty`注解。如果你对某个字段添加了该注解，编译器会自动为你生成get和set方法。如果添加该注解的字段名为crazy，那么get方法将会命名为getCrazy，而set方法将会命名为setCrazy

生成的get和set方法仅在编译之后可用因此，你不能从那些跟注解的字段一起编译的代码中调用这些get和set方法。这在实际当中应该不是问题，因为用Scala代码时你可以直接访问这些字段。这个功能的本意是支持那些预期常规get和set方法的框架，而通常你并不会同时编译框架和框架使用的代码。

#### 27.3.5 尾递归（tailrec）

通常你会给需要尾递归的方法添加`@tailrec`注解，比方说你预期如果不是尾递归，这个方法会递归得很深。为了确保Scala编译器确实对该方法执行了尾递归优化（参考8.9节），可以在方法定义之前添加＠tailrec注解。如果尾递归优化（因为某种原因）不能被执行，那么你会得到一个警告，以及一个关于为什么不能做尾递归优化的解释。

#### 27.3.6 不检查（unchecked）

编译器在处理模式匹配时会看到@unchecked注解。这个注解告诉编译器`不需要担心match表达式可能看上去漏了某些case`。更多内容参考15.5节。

#### 27.3.7 本地（native）方法

 @native注解告诉编译器`某个方法的实现是由运行时而非Scala代码提的`。编译器会在输出中开启合适的标记，将由开发者自己利用诸如Java本地接口（JNI）的机制提供实现。

当使用@native注解时，必须提供方法体，不过这个方法体并不会被包含在输出当中。例如，以下是如何声明一个由运行时提供的beginCountdown方法：

```scala
@native
def beginCountdown() = {}
```

## 28 使用XML

...

## 29 用对象实现模块化编程

主要介绍如何把简单地额单例对象用作模块，然后再说明如何把特质和类用作对模块的抽象。这种抽象可以在不同的模块中配置，甚至是在同一个程序中多次配置。最后展示一种用特质把模块分拆到多个文件中的技术。

### 29.1 问题描述

随着程序规模的增大，以模块化的方式对其加以组织变得尤为重要。首先，如果能通过编译不同的模块来分别建造系统，可以帮助不同的小组互不干扰地工作。另外，如果允许把模块的某个实现拔掉换成另一个实现，这种插拔和替换是有益的，因为这使得系统的不同配置得以应用于不同的环境，例如开发者电脑上的单元测试、集成测试、预发准备以及线上部署等。

举例来说，假定你需要实现一个使用数据库和消息服务的应用程序，在编写代码时，你或许希望能在你的桌面电脑上运行mock掉的数据库和消息服务的单元测试，它们模拟的这些服务足以应付测试而不需要与共享资源进行网络通信。集成测试中，你或许希望使用mock的消息服务但却要用现实版的开发者数据库。而在预发准备和线上部署的过程中你的组织可能会希望使用线上真实版本的数据库和消息服务。

任何致力于达成这种模块化目标的技巧都需要满足一些最基本的要求。首先，应该有一个能够很好地分离接口和实现的模块结构。其次，应该有方式可以替换具有相同接口的模块，而不需要改变或重新编译依赖该模块的其他模块。最后，应该有方式可以把模块连接在一起这种连接的任务可以被认为是在配置（ configuring）该系统。

解决这个问题的一种方式是依赖注入（dependency injection），这是一种由框架（比如企业Java社区较为流行的Spring或uice）支持的构建在Java平台之上的技术。拿Spring来说，它本质上让你可以用Java接口来表示模块的接口，并用Java类来实现。可以通过外部XM配置文件指定模块之间的依赖关系并最终将应用程序“连接”起来。尽管你也可以在Scala里使用Spring，从而以Spring的方式让你的Scala程序做到系统级的模块化，用Scala的我们还有别的选择。本章后续部分将展示如何把对象当成模块来使用，以此来达到我们想要的“大规模”的模块化，而无须用到任何外部框架。

### 29.2 食谱应用程序

假如正在设计一个食谱Web应用，想要把软件划分为不同的层次，包括领域层和应用层。在领域层，定义领域对象，用来保存业务概念和规则并封装将被持久化到外部关系数据库的状态。在应用层，将给出已提供给客户的服务的形式组织的API。应用层将通过协调任务以及派发工作给领域对象的方式来实现这些服务。

需要让每一层都可以插入某些对象真实的或者mock的版本，这样就可以更容易地为应用程序编写单元测试。为了这个目的，可以将想要mock对象当做模块。

既然代表关系型数据库的对象是在领域层内想要mock的东西之一，那么就应该把它做成模块。

对于食谱应用程序，第一件事是对食物和食谱进行建模：

```scala
package org.stairwaybook.recipe
abstract class Food(val name: String){
  override def toString = name
}

class Recipe(
  val name: String,
  val ingredients: List[Food],
  val instructions: String
){
  override def toString = name
}
```

Food和Recipe代表了将要被持久化到数据库的实体，下面定义这两个类的一些单例对象，可以编写测试代码时使用：

```scala
package org.stairwaybook.recipe

object Apple extends Food("Apple")
object Orange extends Food("Orange")
object Cream extends Food("Cream")
object Sugar extends Food("Sugar")

object FruitSalad extends Recipe(
  "fruit salad",
  List(Apple, Orange, Cream, Sugar),
  "Stir it all togather."
)
```

Scala使用对象来表示模块，所以可以从创建测试期间用mock数据库和浏览器模块的两个单例对象开始模块化你的程序。因为是mock，数据库模块只用简单的内存列表来支撑即可。这些对象的实现如下：

```scala
package org.stairwaybook.recipe
object SimpleDatabase{
  def allFoods = List(Apple, Orange, Cream, Sugar)
  def foodNamed(name: String): Option[Food] = {
    allFoods.find(_.name == name)
  }
  def allRecipes: List[Recipe] = List(FruitSalad)
}

object SimpleBrowser{
  def recipesUsing(food: Food) = {
    SimpleDatabase.allRecipes.filter(recipe => recipe.ingredients.contains(food))
  }
}
```

使用方式如下：

```scala
scala> val apple = SimpleDatabase.foodNamed("Apple").get
applt: Food = Apple

scala> SimpleBrowser.recipesUsing(apple)
res0: List[Recipe] = List(fruit salad)
```

假定再添加一个对食物分类的类FoodCategory，来列出数据库中所有的类目：

```scala
package org.stairwaybook.recipe
object SimpleDatabase{
  def allFoods = List(Apple, Orange, Cream, Sugar)
  
  def foodNamed(name: String): Option[Food] = {
    allFoods.find(_.name == name)
  }
  
  def allRecipes: List[Recipe] = List(FruitSalad)

  case class FoodCategory(name: String, foods: List[Food])

  private var categories = List(
    FoodCategory("fruits", List(Apple, Orange)),
    FoodCategory("misc", List(Cream, Sugar))
  )

  def allCategories = categories

}

object SimpleBrowser{
  def recipesUsing(food: Food) = {
    SimpleDatabase.allRecipes.filter(recipe => recipe.ingredients.contains(food))
  }

  def displayCategory(category: SimpleDatabases.FoodCategory) = {
    println(category)
  }
}
```

到这一步，后续还可以添加更多的类和方法。程序可以被切分为但力度向，可以认为它们是模块，这并不是什么新的内容，但是当考虑抽象概念时，就会变得非常有用。

### 29.3 抽象

尽管目前为止看到的例子的确把程序划分为不同的数据库和浏览器模块，尽管这个设计不够“模块化”。问题在于浏览器模块实质上是“硬链接”到数据库模块的：

```scala
SimpleDatabase.allRecipes.filter(recipe =>...
```

所以并不能在不修改和重新编译浏览器模块的情况下插入数据库模块的不同实现。而且，尽管SimpleDatabase模块没有指向SimpleBrowser模块的硬链接，目前也没有什么清晰的方式能够把应用层配置成使用不同的浏览器模块实现。

不过，当把这些模块模块变得更加可插拔的时候，很重要的一点就是避免了代码重复，因为可能有大量的代码可以在相同模块的不同实现之间共享。`如果模块是对象，那么模块的就是类`。就好比类描述了所有实例的公共部分一样，类也可以描述模块中它所有可能的配置中的公共部分。

这样一来，浏览器定义变成了类，不再是对象。所用的数据库被指定为类的抽象成员。数据库也变成了类，包括尽量多的横跨所有数据库的公共逻辑，并声明了那些缺失的，必须有具体的数据库实现给出定义的部分。在本例中，所有的数据库模块都必须定义allFoods、allRecipes和allCategories，不过由于它们可能用任意需要的方式定义，因为这些方法必须在Database类中保持抽象。与此相反，foodNamed方法则可以在抽象Database类里实现。

```scala
abstract class Browser{
  val database: Database

  def recipesUsing(food: Food) = {
    database.allRecipes.filter(recipe => recipe.ingredients.contains(food))
  }

  def displayCategory(category: database.FoodCategory) = {
    println(category)
  }
}

abstract class Database{
  def allFoods: List[Food]
  def allRecipes: List[Recipe]
  def foodNamed(name: String) = allFoods.find(f => f.name == name)
  case class FoodCategory(name: String, foods: List[Food])
  def allCategories: List[FoodCategory]
}

object SimpleDatabase extends Database{
  def allFoods = List(Apple, Orange, Cream, Sugar)
  def allRecipes: List[Recipe] = List(FruitSalad)
  private var categories = List(
    FoodCategory("fruits", List(Apple, Orange)),
    FoodCategory("misc", List(Cream, Sugar))
  )
  def allCategories = categories
}
```

仍然可以像之前使用这些可插拔模块：

```scala
scala> val apple = SimpleDatabase.foodNamed("Apple").get
apple: Food = Apple

scala> object SimpleBrowser extends Browser{
     | val database = SimpleDatabase
     | }
defined object SimpleBrowser

scala> SimpleBrowser.recipesUsing(apple)
res2: List[Recipe] = List(fruit salad)
```

现在可以创建另一个新的mock数据库，并在同一个浏览器中使用它：

```scala
object StudentDatabase extends Database{
  object FrozenFood extends Food("FrozenFood")

  object HeatItUp extends Recipe(
    "heat it up",
    List(FrozenFood),
    "Microwave the 'food' 10 minutes."
  )

  def allFoods = List(FrozenFood)
  def allRecipes = List(HeatItUp)
  def allCategories = List(
    FoodCategory("edible", List(FrozenFood))
  )
}

object StudentBrowser extends Browser{
  val database = StudentDatabase
}
```

### 29.4 将模块拆分成特质

模块通常比较大，因此不是个放在单个文件中。如果发生这种情况，可以使用特质把模块拆分为多个文件。例如，想要把执行分类操作的代码移到Database之外成为独立文件，可以为这段代码创建一个特质：

```scala
trait FoodCategories{
  case class FoodCategory(name: String, foods: List[Food])
  def allCategoried: List[FoodCategory]
}
```

然后将这个特质混入Database中，而不用再定义FoodCategory和allCategories了：

```scala
abstract class Database extends FoodCategories{
  def allFoods: List[Food]
  def allRecipes: List[Recipe]
  def foodNamed(name: String) = allFoods.find(f => f.name == name)
}
```

可以尝试将SimpleDatabase划分为两个特质，一个是食物，一个是食谱，这样就可以如下定义SimpleDatabase：

```scala
object SimpleDatabase extends Database with SimpleFoods with SimpleRecipes
```

SimpleFoods特质的定义是没有问题的：

```scala
trait SimpleFoods{
  object Pear extends Food("Pear")
  def allFoods = List(Apple, Pear)
  def allCategories = Nil
}
```

但是如果想如下定义SimpleRecipes，就会有问题：

```scala
trait SimpleRecipes{
  object FruitSalad extends Recipe(
    "fruit salad",
    List(Apple, Pear),  //Pear没有处于使用它的特质中，超出了作用域范围
    "Mix is all togather."
  )
  def allRecipes = List(FruitSalad)
}

error: not found: value Pear
    List(Apple, Pear),
                ^
```

Pear没有处于使用它的特质中，超出了作用域范围。编译器并不知道SimpleReciples只能与SimpleFoods混搭在一起。

不过有一种方式可以告诉编译器这个要求。Scala专门提供了自身类型(selftype)来应对这种情况。`从技术上讲，自身类型中提到this时，对于this的假定类型。从实用角度上讲，自身类型制定了对于特质能够混入具体类的要求`。如果你的特质仅能用于混入另一个或几个特定的特质，那么你可以指定那些特质作为自身类型。在这个例子中，指定一个SimpleFoods作为自身类型就已经足够了:

```scala
trait SimpleRecipes{
  this: SimpleFoods =>
  object FruitSalad extends Recipe(
    "fruit salad",
    List(Apple, Pear),
    "Mix is all togather."
  )
  def allRecipes = List(FruitSalad)
}
```

有了新的自身类型，Pear就可以使用了。Pear的引用被隐含地认为是this.Pear。这是安全的，因为任何混入了SimpleRecipes的具体类型都必须同时是SimpleFoods的子类型，也就是说Pear一定是它的成员，抽象子类和特质不用必须遵循这个限制，但因为它们不能使用new实例化，所以并不存在this.Pear引用失败的风险。

### 29.5 运行时链接

Scala模块可以在运行时被链接在一起，并且还可以根据运行时的计算决定哪些模块将被链接起来。下面展示了一个可以运行时选择数据库实现并打印出所有苹果食谱的小程序。

```scala
object GotApples{
  def main(args: Array[String]){
    val db: Database =
      if (args(0) == "student")
        StudentDatabase
      else
        SimpleDatabase
  }

  object browser extends Browser{
    val database = db
  }

  val apple = SimpleDatabase.foodNamed("Apple").get
  for (recipe <- browser.recipesUsing(apple))
    println(recipe)
}
```

如果使用SimpleDatabase，你将会找到一个salad的食谱。而如果使用学生数据库，你会发现根本没有找到Apple的是食谱。

### 29.6 跟踪模块实例

尽管使用的是相同的代码，上一节创建的不同浏览器仍然是分离的模块，每个模块都有自己的内容。SimpleDatabase中的FoodCategory和StudentDatabase中的FoodCategory是不同类：

```scala
scala> val category = StudentDatabase.allCategories.head
category: StudentDatabase.FoodCategory = FoodCategory(edible,List(FrozenFood))

scala> SimpleBrowser.displayCategory(category)
<console>:14: error: type mismatch;
 found   : StudentDatabase.FoodCategory
 required: SimpleBrowser.database.FoodCategory
       SimpleBrowser.displayCategory(category)
                                     ^
```

如果想让两者个FoodCategory是同一个，可以把FoodCategory定义移到类或者特质之外。有的时候两个类型相同，但是编译器却不能鉴别出来，你会看到编译器抛出的错误信息提示两个类型不同，虽然你知道它们是完全一致的。

在这种情况下，使用**单例类型**来解决问题。例如在GoApples程序中，类型检查不知道db和browser.database是同一个类型，因此当你尝试在两个对象之间传递类目消息时：

```scala
object GoApples{
  //一些定义
  for (category <- db.allCategories)
    browser.displayCategory(category)
  ///...
}

GotApples2.scala:14: error: type mismatch;
found : db.FoodCategory
required: browser.database.FoodCategory
browser.displayCategory(category)
ˆ
one error found
```

要避免这个错误，需要告诉类型检查器它们是同一个对象。可以通过下面代码实现：

```scala
object browser extends Browser{
  val database: db.type = db
}
```

这个定义中database的类型很古怪，db.type。结尾的“.type”表示它是单例类型。单例类型及其明确，它只保存一个对象，在本例中就是db指向的那个对象。通常这样的类型实在太过明确，以至于没有什么用处，这也是为什么编译器通常都不自动插入单例类型。但是在本例中，单例类型可以让编译器知道db和browser.database是同一个对象。

## 30 对象相等性

### 30.1 Scala中的相等性

Scala对相等性的定义和Java不同，Java有两种相等性比较：

- **==操作符**：`对值而言`这自然是相等性，对`引用类型`而言则是对象一致性；
- **equals方法**：是（用户定义的）`引用类型`的规约相等性。

在Java编程中，对于初学者而言是以常见的陷进，是在该用equals的地方使用==来比较对象。举例来说，即使在x，y拥有完全相等的字符和顺序，`x == y`得到false也不奇怪。

Scala也有一个相等性判断方法用来表示对象一致性，不过用的并不多。此类相等性的判断，写作“`x eq y`”，当x和y引用同一个对象时为true。在Scala中：

- **==**：用来表示每个类型“自然的”相等性。对于`值类型`而言，是对值的比较，和Java一样。对于`引用类型`，相当于equals。

**可以重写新类型的equals方法从而重新定义==的行为**。这个方法总是会从Any类继承下来，除非重写，默认是想Java那样判断对象是否一致。因此，`equals`方法（以及==）默认和eq是一样的。不过可以通过在定义的类中重写equals方法的方式来改变其行为。

- 没有办法直接重写==方法，因为它在Any中定义为final方法：

```scala
final def ==(that: Any): Boolean =
  if (null eq this) {null eq that} else {this equals that}
```

### 30.2 编写相等性方法

在面向对象的语言中，正确编写相等性方法是十分困难的。正如2007年的一篇论文(*Declarative Object Identify Using Ration Types*)指出：几乎所有的equals方法实现都有问题。这个问题很严重，因为很多其他代码逻辑都以相等性判断为基础。比如，如果一个类型C的相等性方法有问题，可能意味着你无法很有把握地将一个类型C的对象放到集合中。

可能有两个相等的类型C的元素elem1和elem2，即`elem1 equals elem2`会得到true。尽管如此，由于经常会遇到equals方法实现有问题的情况，可能还会碰到如下问题：

```scala
val hashSet: Set[C] = new scala.collection.immutable.HashSet
hashSet += elem1
hashSet contains elem2 //返回false
```

重写equals方法时有四种常见的陷进，可能造成不一致：

- **定义equals方法采用了错误的方法签名**；
- **修改了equals方法当并没有同时修改hashCode**；
- **用可变字段定义了equals方法**；
- **未能按同等关系equals方法**。

#### 30.2.1 以错误的签名定义equals方法

为Point添加一个相等性判断方法：

```scala
class Point(val x: Int, val y: Int){
  def equals(other: Point): Boolean =
    this.x == other.x && this.y == other.y
}
```

看上去没有什么问题：

```scala
scala> val p1, p2 = new Point(1, 2)
p1: Point = Point@2862ca5
p2: Point = Point@32967892

scala> val q = new Point(21, 3)
q: Point = Point@1f2616d3

scala> p1 equals p2
res13: Boolean = true

scala> p1 equals q
res14: Boolean = false
```

但是将放入集合，麻烦就来了：

```scala
scala> import scala.collection.mutable
import scala.collection.mutable

scala> val col1 = mutable.HashSet(p1)
col1: scala.collection.mutable.HashSet[Point] = Set(Point@2862ca5)

scala> col1 contains p2
res16: Boolean = false
```

p1等于p2，为什么col1不包含p2呢？下面我们重新定义一个p2的副本：

```scala
scala> val p2a: Any = p2
p2a: Any = Point@32967892

scala> p1 equals p2a
res17: Boolean = false
```

定义新的p2a，和p1是不相同的，可以看到p2a的类型为Any而不是Point。事实上，之前给出的equals方法并没有重写标准的equals，因为它们的类型不同。根类Any中定义个equals方法所用的类型是：

```scala
def equals(other: Any): Boolean
```

Point类的equals的参数类型是Point而不是Any，它`并没有重写Any类中的equals方法，而是一个重载的备选方法`。目前，Scala和Java中的重载都是根据参数的静态类型，而不是运行时的类型来解析的。因此只要参数的静态类型是Point，则调用的就是Point类中的equals方法，静态类型是Any的就调用根类中的equals方法。这就是为什么`p1 equals p2a`还是会得到出false了。这也是为什么HashSet的contains方法返回的是false。由于这个方法操作的是泛型的集合，它调用的是Obkject类的equals方法而不是Point中重载的变种。

重新定义Point的equals方法：

```scala
override def equals(other: Any) = other match{
  case that: Point => this.x == other.x && this.y == other.y
  case _ => false
}
```

但是这样并不是完美的方法，首先它检测other对象的类型是否是Point，如果是就比较两个坐标，否则就返回false。

另一种陷进是试图重写==方法，这个方法在Any中定义为final，因此它是不能被重写的。有时候想重写，但是用了错误的签名：

```scala
def ==(other: Point): Boolean = ...
```

这样编译器不会报错，但并不是重写了`==`方法，**定义的==方法只是被当做`Any类中同名方法的重载`的变种**，容易代码不必要的麻烦。

#### 30.2.2 修改equals但没有同时修改hashCode

应用修改过后的equals方法，再来比较p1和p2a将会得到true。但是用hashSet.contains检测时，还是会返回false：

```scala
scala> val p1, p2 = new Point(1, 2)
p1: Point = Point@359a9bf1
p2: Point = Point@73f90886

scala> val p2a: Any = p2
p2a: Any = Point@73f90886

scala> p1 equals p2a
res18: Boolean = true

scala> collection.mutable.HashSet(p1) contains p2
res19: Boolean = false
```

这是因为重写了equals，但没有重写hashCode。

注意上例中使用的HashSet。这意味着这个集合类中的元素会依据它们的哈希码被放进**哈希桶**中。contains检测首先决定要找的桶，然后将他们给定的元素同该桶中的所有元素进行比较。现在的情况是，最后这个版本的重写equals时并没有同时重写hashCode。因此hashCode仍然是AnyRef中的定义：**已分配对象的某种转换**。

p1和p2的哈希码几乎肯定是不同的，尽管这两个点的值是相同的。不同的哈希码意味着它们几乎不可能被放进同一个桶中，所以在p1的那个桶中几乎永远也找不到p2。问题在于Point最后这个实现违背了Any类中定义hashCode方法的契约：**如果两个对象根据equals方法是相等的，那么对它们每一个调用hashCode方法都必须产出相同的整型结果**。

hashCode和equals要一起重定义，hashCode只能依赖equals方法依赖的字段，下面是hashCode的定义：

```scala
class Point(val x: Int, val y: Int){
  override def hashCode = (x, y).##  //##为计算哈希码的简写
  override def equals(other: Any) = other match{
    case that: Point => this.x == that.x && this.y == that.y
    case _ => false
  }
}
```

#### 30.2.3 用可变字段定义equals

如果使用var代替val定义x和y，像如下定义：

```scala
class Point(var x: Int, var y: Int){
  override def hashCode = (x, y).##  //##为计算哈希码的简写
  override def equals(other: Any) = other match{
    case that: Point => this.x == that.x && this.y == that.y
    case _ => false
  }
}
```

就会带来奇怪的效果：

```scala
scala> val p = new Point(1, 2)
p: Point = Point@5428bd62

scala> val col1 = mutable.HashSet(p)
col1: scala.collection.mutable.HashSet[Point] = Set(Point@5428bd62)

scala> col1 contains p
res20: Boolean = true
```

这看上去是没有什么问题，但是当修改p的x坐标时，麻烦就来了：

```scala
scala> p.x += 1

scala> col1 contains p
res22: Boolean = false
```

p去哪了？但是当时用迭代器检查时结果会更奇怪：

```scala
scala> col1.iterator contains p
res24: Boolean = true
```

这是个不包含p的集合，但是p又在集合中！事实上，当x被修改之后，点对象p相当于被放到col1集合当中错误的哈希桶里。换句话说，原来那个哈希桶不再对应到这个点对象新的哈希值。某种意义上讲，p这个点对象被“挤出”了col1集合的“视野”，虽然它仍然是集合的一员。

这个问题说明，如果equals和hashCode依赖于可变状态，对于潜在的用户带来问题。如果将这样的对象放入集合中，必须要给常小心不去修改被依赖的状态，而这并不容易做到。如果需要的比较牵扯对象当前的状态，通常应该取别的名字（类似equalsContent），而不是equals。

#### 30.2.4 未能按等同关系定义equals方法

根据scala.Any中的equals方法的契约约定，equals方法必须对非null对象实现对等关系：

- 它是自反射的：对任何非空值x，表达式x.equals(x)应该返回true.
- 它是对称的：对任何非空值x和y，x.equals(y)当且仅当y.equals(x)返回true时返回true。
- 它是可传递的，对任何非空值x、y和z。如果x.equals(y)返回true且y.equals(z)返回true，则x.equals(z)应返回true。
- 它是一致的。对任何非空值x和y，多次调用x.equals(y)返回值应该相同，只要勇于对象的equals比较的信息没有被修改过。
- 对任何非空值x，x.equals(null)应该返回false。

到目前为止，开发的Point类没有什么问题，是满足契约的。不过，当开始考虑子类时，事情就变得复杂了。如果给Point增加一个ColoredPoint子类，并添加了一个类型为Color的字段color：

```scala
object Color extends Enumeration{
  val Red, Yellow, Green, Black = Value
}

class ColoredPoint(x: Int, y: Int, val color: Color.Value) extends Point(x, y){
  override def equals(other: Any) = other match{ //问题：equals是不对称的
    case that: ColoredPoint => this.color == that.color && super.equals(that)
    case _ => false
  }
}
```

本例中，ColoredPoint并不需要重写hashCode，因为ColoredPoint的新的equals定义比Point中被重写的定义更加严格。也就是hashCode的契约依然合法。如果有两个带颜色相等，它们必须有相同的坐标，因此它们的hashCode也一定会相等的。

如果只考虑ColoredPoint自身，equals方法用起来是没有问题的，但是一旦点和带颜色的点混在一起，equals契约就实效了。

```scala
scala> val p = new Point(1, 2)
p: Point = Point@5428bd62

scala> val cp = new ColoredPoint(1, 2, Color.Red)
cp: ColoredPoint = ColoredPoint@5428bd62

scala> p equals cp
res25: Boolean = true

scala> cp equals p
res26: Boolean = false
```

这明显违背了对称性的契约。其实p和zp调用了两个不同的equals方法，对于集合类而言，失去对称性会带来无法预料的后果。

```scala
scala> mutable.HashSet[Point](p) contains cp
res27: Boolean = true

scala> mutable.HashSet[Point](cp) contains p
res28: Boolean = false
```

针对这个问题修改为如下代码：

```scala
class ColoredPoint(x: Int, y: Int, val color: Color.Value) extends Point(x, y){
  override def equals(other: Any) = other match{ //问题：equals不是可传递的
    case that: ColoredPoint => this.color == that.color && super.equals(that)
    case that: Point => that equals this
    case _ => false
  }
}
```

然后测试一下：

```scala
scala> val redp = new ColoredPoint(1, 2, Color.Red)
redp: ColoredPoint = ColoredPoint@5428bd62

scala> val bluep = new ColoredPoint(1, 2 ,Color.Green)
bluep: ColoredPoint = ColoredPoint@5428bd62

scala> redp == p
res29: Boolean = true

scala> p == bluep
res30: Boolean = true

scala> redp == bluep
res31: Boolean = false
```

符合对称性的契约，但是最后一行代码显式，它并不满足可传递性的契约。这里应该让equals方法更加严格，方式是总是将不同类型的对象当做是不同的。这可以通过修改Point类和ColoredPoint类的equals方法来实现。在Point中，可以添加一个额外的比较检查是否是另一个Point的运行时确切是Point类。

```scala
class Point(val x: Int, val y: Int) {
  override def hashCode = (x, y).##
  override def equals(other: Any) = other match {
    case that: Point => this.x == that.x && this.y == that.y && this.getClass == that.getClass
    case _ => false
  }
}
```

然后将ColoredPoint回退到首次定义的版本（这个版本违反了对称性要求）。

新的定义既要满足对称性又要满足可传递性，因为现在不同类型的对象间的标胶判断总是返回false。因此一个带颜色的点永远不可能和一个点相等，这可能太过严格。考虑一下情况，以变通的方式定义一个新的点：

```scala
scala> val pAnon = new Point(1, 1) { override val y = 2 }
pAnon: Point = $anon$1@5428bd62
```

p和pAnon是不相等的。因为p和pAnon相关联的java.lang.Class对象不同。p是Point，而pAnon是匿名的Point类的（子）类。但是很清楚的是p和pAnon指向的是同一个点，这样似乎不和情理。

有一种方式可以重新定义类继承关系中若干级别上的相等性，同时不违背契约。就是在定义equals和hashCode之外，再多定义一个方法。它应该同时明确指出该类的对象不与任何定义了不同相等性方法的超类的对象相等，这是通过给每个重定义equals方法的类添加一个canEqual方法来实现：

```scala
def canEqual(other: Any): Boolean
```

如果other对象是重定义了canEqual方法的类的实例，则该方法应该返回true，否则返回false。equals方法中调用canEqual来确保对象可以进行双向比较：

```scala
class Point(val x: Int, val y: Int) {
  override def hashCode = (x, y).##
  override def equals(other: Any) = other match {
    case that: Point => this.x == that.x && this.y == that.y && (that canEqual this)
    case _ => false
  }

  def canEqual(other: Any) = other.isInstanceOf[Point]
}

class ColoredPoint(x: Int, y: Int, val color: Color.Value) extends Point(x, y){
  override def equals(other: Any) = other match{ //问题：equals不是可传递的
    case that: ColoredPoint => this.color == that.color && super.equals(that) && (that canEqual this)
    case _ => false
  }

  override def canEqual(other: Any) = other.isInstanceOf[ColoredPoint]
}
```

根据新的定义就是另外一个对象要能够与这个对象相等，取决于canEqual方法。根据Point的canEqual方法的实现，所有的实力都可以相等。

两个类中定义的equals方法符合契约规则，相等性是对称性的，也是可传递的。

```scala
scala> val p = new Point(1, 2)
p: Point = Point@5428bd62

scala> val cp = new ColoredPoint(1, 2, Color.Red)
cp: ColoredPoint = ColoredPoint@5428bd62

scala> val pAnon = new Point(1, 1) {override val y = 2}
pAnon: Point = $anon$1@5428bd62

scala> val col1 = List(p)
col1: List[Point] = List(Point@5428bd62)

scala> col1 contains cp
res32: Boolean = false

scala> col1 contains pAnon
res33: Boolean = true

scala> p equals pAnon
res34: Boolean = true
```

这些例子展示，度过超类的equals实现定义并调用了canEqual，则实现子类的程序员可以决定它们的子类是否可以与超类的实例相等。由于子类ColoredPoint重写了Point的canEqual方法，所以一个带颜色的点和一个普通的点是不可能相同的。但pAnon引用的匿名子类并没有重写canEqual方法，它的实例可以与Point的实例相等。

### 30.3 定义参数化类型的相等性

前面示例中的equals方法都以模式匹配开始，该模式匹配测试操作数的类型是否符合包含equals方法的类的类型。当类被参数化时，需要对该方案进行一些调整。

以二叉树为例，定义一个抽象Tree类和两个可选实现EmptyTree对象和代表非空树的Branch类：

```scala
trait Tree[+T]{
  def elem: T
  def left: Tree[T]
  def right: Tree[T]
}

object EmptyTree extends Tree[Nothing]{
  def elem = throw new NoSuchElementException("EmptyTree.elem")
  def left = throw new NoSuchElementException("EmptyTree.left")
  def right = throw new NoSuchElementException("EmptyTree.right")
}

class Branch[+T](
  val elem: T,
  val left； Tree[T],
  val right: Tree[T]
) extends Tree[T]
```

现在我们将向这些类添加equals和hashCode方法。为Tree树本身不需要做什么，因为我们假设这些方法是为抽象类的每个实现单独实现的。对于EmptyTree，仍然没有什么要做的，因为EmptyTree从AnyRef继承的equals和hashCode的默认实现工作得很好，毕竟空树EmptyTree只等于它自己。

但是为Branch添加equals和hashCode需要定义一些操作，因为一个Branch要等于另一个Branch，需要有相同的元素和左右节点：

```scala
class Branch[T](
  val elem: T,
  val left: Tree[T],
  val right: Tree[T]
) extends Tree[T]{
  override def equals(other: Any) = other match{
    case that: Branch[T] => this.elem == that.elem &&
                            this.left == that.left &&
                            this.right == that.right
    case _ => false
  }
}
```

编译这段代码，编译器会抛出“unchecked”告警，可以在编译时加上-unchecked选项打印出原因：

```scala
$ fsc unchecked
Tree.scala
Tree.scala:14: warning: non variable typeargument
T in type
pattern is unchecked since it is eliminated by erasure
    case that: Branch[T] => this.elem == that.elem &&
               ˆ
```

定义equals时，对Branch[T]使用了模式匹配，然而系统只检查other引用(某种形式的)Branch，它不能检查Tree的元素类型是否为T。就如19章所述：类型参数的元素类型在编译器编译阶段被擦除了，在运行时时是不可用的。

幸运的是，在比较两个Branch时，并不需要检查它们是否具有相同的元素类型。具有不同元素类型的两个分支很可能是相等的，只要它们的字段是相同的。一个简单的例子是由一个Nil元素和两个空子树组成的分支。可以认为任意两个这样的分支是相等的，无论它们具有什么静态类型：

```scala
scala> val b1 = new Branch[List[String]](Nil, EmptyTree, EmptyTree)
b1: Branch[List[String]] = Branch@794eeaf8

scala> val b2 = new Branch[List[Int]](Nil, EmptyTree, EmptyTree)
b2: Branch[List[Int]] = Branch@59e082f8

scala> b1 == b2
res0: Boolean = true
```

上面的例子说明，编译器并不会检查Branch的类型参数T，否则`b1 == b2`应该返回false。

在这两种可能的比较结果中，哪一种更自然，可能意见不一。最后，这取决于表示类的心智模型。类型参数只在编译阶段出现的模型中，很自然地认为两个分支值b1和b2是相等的。在类型参数是对象值的一部分的模型中，同样很自然地认为它们是不同的。由于Scala采用类型擦除模型，类型参数在运行时不被保留，因此b1和b2自然被认为是相等的。

如果想让编译器在编译阶段不再抛出“unchecked”告警，可将Branch的类型参数T改为t：

```scala
case that: Branch[t] => this.elem == that.elem &&
                        this.left == that.left &&
                        this.right == that.right
```

`t`代表类型参数的类型是未知的。这个模式匹配中`case that: Branch[t] =>`对于任何类型的编译都会成功。`t`还可以使用`_`替代：

```scala
case that: Branch[_] =>
```

下面就是给Branch定义hashCode和CanEqual方法：

```scala
class Branch[T](
  val elem: T,
  val left: Tree[T],
  val right: Tree[T]
) extends Tree[T]{
  override def equals(other: Any) = other match{
    case that: Branch[_] => this.elem == that.elem &&
                            this.left == that.left &&
                            this.right == that.right
    case _ => false
  }

  override def hashCode: Int = (elem, left, right).##

  def canEqual(other: Any) = other match{
    case that: Branch[_] => true
    case _ => false
  }
}
```

canEqual实现使用了模式匹配，也可以用下面定义：

```scala
def canEqual(other: Any) = other.isInstanceOf[Branch[_]]
```

可能想知道上面类型中的下划线表示什么。毕竟，`Branch[_]`技术上是方法的类型参数，而不是类型模式。那怎么可能让它的某些部分没有定义呢?

这个问题的答案在下一章讨论。Branch[_]是所谓的通配符类型的简写，粗略地说，通配符类型中有一些未知的部分。因此，即使从技术上讲，下划线在模式匹配和方法调用的类型参数中代表两个不同的东西，但本质上，其含义是相同的:`它允许标记未知的东西`。

### 30.4 重写equals和hashCode方法

在本节中，我们将逐步提供创建equals和hashCode方法的规则，这些方法应该能够满足大多数情况。作为说明，将使用Rational类的方法。此次创建的Rational类删除了数学运算的方法，并增强了toString的功能，并修了分子分母的表现形式（将1/(-2)转换为(-1)/2，为负数是将`-`符号加在分子上）。

#### 30.4.1 重写equals

下面是重写equals的方法：

- 要在非final类中重写equals，要创建一个canEqual方法。如果继承的equals定义来自AnyRef(也就是说，equals没有在类层次结构的更高层重新定义)，canEqual将是新定义的方法，否则就是重写了之前的同名方法。这个要求的唯一例外是，`对于重新定义从AnyRef继承的equals方法的final类`。这样来说，不可能出现第30.2节中描述的子类异常（final class没有子类）；因此，他们不需要定义canEqual。传递给canEqual的对象类型应该是Any：

```scala
def canEqual(other: Any): Boolean = ...
```

- 如果参数对象是当前类的实例，则canEqual方法应该返回true，否则返回false：

```scala
other.isInstanceOf[Rational]
```

- 在equals方法中，确保传递对象的类型是Any:

```scala
override def equals(other: Any): Boolean = ...
```

- 将equals方法的主体编写为单个匹配表达式：

```scala
other match {
// ...
}
```

- 匹配表达式应该有两个分支，第一分支应该为定义equals方法的类的类型声明一个类型化模式：

```scala
case that: Rational =>
```

- 编写一个表达式，将必须为真才能使两个对象相等的单个表达式逻辑相加。如果要重写的equals方法不是AnyRef的方法，那么很可能要包含超类的equals方法：`super.equals(that) &&`；如果正在为第一次引入canEqual的类定义equals，那么您应该在equals方法的参数上调用canEqual，并将其作为参数传递：

```scala
(that canEqual this) &&
```

重写equals的重定义还应该包括canEqual调用，除非它们包含对super.equals的调用。在后一种情况下，canEqual测试已经由超类调用完成。最后，对于与相等性相关的每个字段，验证此对象中的字段是否等于所传递对象中的相应字段：

```scala
numer == that.numer && denom == that.denom
```

- 匹配表达式的另一个分支如下：

```scala
case _ => false
```

如果遵循相等的原则，那么相等就保证是一个等价关系，这是相等契约所要求的。

#### 30.4.2 重写hashCode

对于hashCode，如果使用以下方法，通常可以获得满意的结果，这类似于为*Effective Java*中的Java类推荐的方法：

```scala
override def hashCode: Int = (a, b, c, d, e).##
```

如果equals方法在其计算中调用了`super.equals(that)`，那么应该通过调用`super.hashCode`计算hashCode。比如，Rational中equals中调用`super.equals(that)`，那么hashCode应该这样写：

```scala
override def hashCode: Int = (super.hashCode, numer, denom).##
```

重写之后的Rational：

```scala
class Rational(n: Int, d: Int){
  require(d != 0)
  
  private val g = gcd(n.abs, d.abs)
  val numer = (if (d < 0) -n else n) / g
  val denom = d.abs / g

  private def gcd(a: Int, b: Int): Int =
    if (b == 0) a else gcd(b, a % b)

  override def equals(other: Any): Boolean = other match{
    case Rational => that canEqual this &&
                     numer == that.numer &&
                     denom =- that.denom
    case _ => false
  }

  def canEqual(other: Any): Boolean = other.isInstanceOf[Rational]

  override def hashCode: Int = (numer, denom).##

  override def toString =
    if (denom == 1) numer.toString else numer + "/" + denom
}
```

在使用这种方法编写hashCode方法时需要记住的一点，就是hashCode好坏要看你构建它用到的哈希码的好坏，也就是说通过调用对象中相关字段的hashCode得到的值。有时可能除了在字段上调用hashCode之外还需要做些额外的事情才能得到该字段有用的哈希码。举例来说，如果某个字段是集合，或许希望这个字段的哈希码是基于集合类中的所有元素的。如果字段是Vector、List、Set、Map或Tuple，可以简单地调用字段的hashCode，因为这些类的equals和hashCode方法被重写过，会考虑包含的元素。但是对于Array而言并不是这样，它们在计算哈希码时并不会考虑元素。因此，对数组而言，应该将每个元素当做是对象的字段，主动调用每个元素的hashCode，或者将数组传递给单例对象java.util.Arrays的某一个hashCode方法。

最后，如果发现一个特定的哈希码计算影响到程序的性能，也可以考虑将哈希码缓存起来，如果对象是不可变的，可以在对象创建时计算哈希码并保存到一个字段中。可以简单地通过val而不是def重写hashCode来做到：

```scala
override val hashCode: Int = (numer, denom).##
```

这种方法是用内存换取计算时间，因为每个不可变的实例将会多出一个字段来保存缓存的哈希码值。

## 31 结合Scala和Java

### 31.1 从Java使用Scala

大多数时候，从源码层面考虑Scala就可以了。不过如果知道一些Scala到Java的翻译细节，会对系统运行有更全面的理解。进一步说，如果从Java调用Scala代码，将会知道从Java的角度看Scala代码长什么样。

#### 31.1.1 一般原则

Scala的实现方式是将代码翻译为标准的Java字节码。Scala的特性尽可能地直接映射为相对等的Java特性。举例来说，Scala的类、方法、字符串和异常等和它们在Java中的对应概念一样编译成Java字节码。

为了实现这一点，在设计Scala过程中，有时候需要作出艰难的抉择。例如，在运行期使用运行期类型解析确定重载方法，而不是在编译期决定，也许是个不做的方法，但是这样的设计会破坏Java的重载解析，使得混用Java和Scala变得更加难以应对。在这个问题上，Scala和Java的重载解析保持一致，因而Scala的方法和方法调用可以直接与Java的方法和方法的调用相对应。

另外，Scala也有一些其他特性，如特质在Java中没有与之相当的对应。同样地，Scala和Java都有泛型，但是两者的实现细节是不同的。对于冲突的语言特性，Scala代码无法直接映射为Jaa语法结构，因此必须结合Java现有的特性来进行编码。

对于不能直接映射的特性，编码并不是固定的。目前进行的工作是尽量使这个翻译尽可能简单。可以使用javap这样的工具查看.class文件来获取当前Scala编译器使用的翻译。

#### 31.1.2 值类型

类似Int这样的值类型翻译成Java有两种方式。只要可能，编译器会将Scala的Int翻译为Java的int以获取更好的性能。但有时做不到，因为编译器不确定它在翻译的是一个Int还是另外某种数据类型。比如List[Any]中可能只有Int型的元素，单边一起没有办法确认这一点。

对于这样的类型，编译器不确定某个对象是不是值类型，而是会使用对象并依赖相应的包装类。如java.lang.Integer这样的包装类允许一个值类型被包装在Java对象中，由需要对象的代码操作。

#### 31.1.3 单例对象

Java中并没有单例对象，但是有静态方法。`Scala对单例对象的翻译采用了静态和实例方法相结合的方式`。每一个Scala单例对象，编译器都会为这个对象创建一个同名并加美元符号的Java类。比如定义了一个App的单例对象，编译器产出一个名为`App$`的java类，这个类拥有Scala单例对象的所有方法和字段，这个Java类同时还有一个名为`MODULES$`的静态对象，保存该类在运行期创建的一个实例。

```scala
object App{
  def main(args: Array[String]) = {
    println("Hello, world!")
  }
}
```

Scala将会生成一个Java类App$，具体如下：

```java
$ javap App$
public final class App$ extends java.lang.Object implements scala.ScalaObject{
  public static final App$ MODULES$;
  public static {};
  public App$();
  public void main(java.lang.String[]);
  public int $tag();
}
```

如果只是一个单例对象，而没有与之对应的同名的类，编译器创建一个名为App的Java类，这个类对于每个Scala单例对象的方法都有一个静态的转发方法与之对应：

```java
$ javap App
Complied from "App.scala"
public final class App extends java.lang.Object{
  public static final int $tag();
  public static final void main(java.lang.Strng[]);
}
```

相反，如果确实有一个名为App的类，Scala会创建一个对应的Java类App来保存这个类的成员。这种情况下，就不会添加任何转发到同名单例对象的方法，Java代码必须通过`MODULES$`字段来访问这个单例对象。

#### 31.1.4 作为特质的接口

编译任何特质都会创建一个同名的Java接口。这个接口可以作为Java类型使用，可以通过这个类型的变量来调用Scala对象的方法。

在Java中实现一个特质则完全是另一回事。`如果定义的Scala特质只包含抽象方法，这个特质会直接翻译成Java接口，不需要任何其他代码`。

### 31.2 注解

#### 31.2.1 标准注解的额外效果

对于有一些注解，编译器在针对Java平台编译时会产生额外的信息。当编译器看到这样的注解时，会首先根据一般Scala原则去处理，然后针对Java做一些额外的工作。

- **@deprecated**：编译器会为产生的代码添加Java自己对应的注解。
- **@Volatile**：Scala与Java的处理机制完全一样。
- **@serializable**：会加上Java的Serialzable接口，@SerialVersionUID(1234L)会被转换为Java的字段定义`private final static long SerialVersionUID = 1234L`。任何标记为**@trainsient**的变量会被加上Java的**transient**修饰符。

#### 31.2.2 抛出的异常

Scala并不检查抛出的异常是否被捕获。也就是Scala中并没有throws声明相对应的定义，`所有Scala方法都被翻译成没有声明任何抛出异常的Java方法`。

声明抛出异常这个特性之所以被Scala排除在外，是因为Java中人们对他的体验并不是很全面。由于用throws语句注解方法是个沉重的负担，因此许多开发者都编写吃掉并丢弃异常的代码，仅仅是为了在不增加所有这些throws语句的情况下让代码编译通过。

有时候与Java对接，可能需要编写对Java友好的注解，用于描述某个方法可能抛出的异常。比如每个RMI远程接口中的方法都需要throws子句中提到的java.io.RemoteException。因此希望带有抽象方法顶一个额Scala特质编写RMI接口，需要在这些方法的throws子句中列出RemoteException。为了达成这个目的，就要用@throw注解定义的方法：

```scala
import java.io._
class Reader(fname: String){
  private val in = new BufferReader(new FileReader(fname)
  )

  @throws(classOf[IOException])
  def read() = in.read()
}
```

从Java来看就是：

```java
$ javap Reader
Comlied from "Reader.scala"
public class Reader extends java.lang.Object impliments scala.ScalaObject{
  public Reader(java.lang.String);
  public int read() throws java.io.Exception;
  public int $tag();
}
```

#### 31.2.3 Java注解

Java框架中的胡姐可以直接在Scala代码中使用。任何Java框架都会看到你编写额注解，就好像使用Java编写的一样。

例如，编写一个Test注解：

```scala
import org.junit.Test
import org.junit.Assert.assertEquals

class SetTest{
  @Test
  def testMultiAdd = {
    val set = Set() + 1 + 2 +3 + 1 + 3
    assertEquals(2, set.size)
  }
}
```

通过命令行来执行：

```scala
$ scala -cp junit-4.3.1.jar:. org.junit.runner.JUnitCore SetTest

JUnit version 4.3.1
.
OK (1 test)
```

#### 31.2.4 编写自定义注解

为了让注解对Java反射可见，必须用Java的语法编写并用javac编译。对于这样的用力而言，用Scala来编写注解看上去没有什么帮助，因此标准的编译器不支持这样做，这背后的原因是Scala的支持将不可避免地会无法实现Java注解的全部功能，而且Scala可能会在某类拥有自己的反射，而你可能像使用Scala反射来访问Scala的注解。

```java
import java.lang.annoation.*
@Retention(RetentionPolicy.RUNTIMEE)
@Target(ElementType.METHOD)
public @interface Ignore{}
```

使用javac编译上述代码之后，可以像下面一样来使用该注解：

```scala
object Tests{
  @Ignore
  def testData = List(0, 1, -1, 5, -5)
  def test1 = {
    assert(testData == (testData.head :: testData.tail))
  }
  def test2 = {
    assert(testData.contains(testData.head))
  }
}
```

test1和test2都是测试方法，近端testData以test开头，它实际上应该被忽略。

为了查看这些注解是否被用到，可以用Java反射API。下面代码示例展示它是如何工作的：

```scala
object FindTests{
  def main(args: Array[String]){
    for {
      method <- Tests.getClass.getMethods
      if method.getName.startsWith("test")
      if method.getAnnotation(classOf[Ignore]) == null
    }{
      println("found a test method: " + method)
    }
  }
}
```

getClass和getMethods都是普通的反射方法。。与注解特定的部分是对getAnnotation方法的使用，许多反射对象都有一个getAnnotation方法来查找特定类型的注解。本例中查找的是新的类型Ignore类型的注解。由于这时Java API，是否成功取决于结果是null还是实际的注解对象。

代码运行后的结果如下：

```bash
$ src scalac -d /usr/local/src Tests.scala FindTests.scala Ignore.java
$ src javac -cp /usr/local/src -d /usr/local/src Ignore.java
$ src scala -cp /usr/local/src FindTests
found a test method: public void Tests$.test1()
found a test method: public void Tests$.test2()
```

注意，以Java反射的视角来看这些方法位于`Test$`类而非Test类中。如本章一开始讲的那样，Scala的单例对象的实现位于一个名称后增加美元符号的Java类中，对本例而言，Tests的实现位于类`Tests$`中。

在使用Java注解时必须遵循它们所规定的的限制。比如，**在注解的参数中，只能使用常量，而不能使用表达式**。能使用`@serial(1234)`，但是不能使用`@serial(x * 2)`，因为`x * 2`不是常量，而是表达式。

### 31.3 通配类型

所有Java类型在Scala中都有对等的概念。这是必要的，因为只有这样的Scala代码才能访问任何合法的Java类。大多数时候翻译都很直接了当，比如Java中的`Iterator<Component>`翻译为Scala就是`Iterator[Component]`。但是某些情况下Scala类型不一定满足要求，比如`Iterator<?>`或者`Iterator<? extends Component>`的怎么翻译？对于类型参数的原始类型Iterator又能做什么？对于通配类型和原始类型，Scala使用一种额外的叫作**通配类型(wildcard type)**来表示。

通配类型的编写方法就是通过*占位符语法*，如`(_ + 1)`等效于`(x => x + 1)`，`Iterator[_]`代表元素类型未知的Iterator。

也可以在使用占位符是插入上界和下界，例如，`Iterator[_ <: Component]`，这个类型表示的就是Iterator的元素类型必须是Component的子类型。

对于简单用例而言，可以忽略通配符，直接调用基类型的方法，如下例子：

```java
public class Wild{
  public Collection<?> contents{
    Collection<String> stuff = new Vector<String>();
    stuff.add("a");
    stuff.add("b");
    stuff.add("see");
    return stuff;
  }
}
```

如果在Scala中访问这个类，将会看到它有一个通配类型：

```scala
scala> val contents = (new Wild).contents
contents: java.util.Collection[_] = [a, b, see]
```

如果想要知道集合中有多少元素，可以简单地忽略通配部分，并像平常一样调用size方法：

```scala
scala> contents.size()
res0: Int = 3
```

由于通配符没有名称，没有办法在两个不同的地方使用它。假如想要创建一个可变的Scala集合，并使用contents的元素初始化：

```scala
import scala.collection.mutable
val iter = (new Wild).contents.iterator
val set = mutable.Set.empty[???] //应该填什么类型？
while(iter.hashMore)
  set += iter.next()
```

第三行中，没有办法给出Java集合中元素类型的名称，因此无法写下set的满足各项要求的类型，为了绕开这个问题，考虑一下两种技巧：

1. 将通配类型传入方法时，给方法分配一个类型参数来表示这个通配类型。现在就有了一个该类型的名称，想用多少次都可以。
2. 不要从方法返回通配类型，而是返回一个对每个占位符类型都定义了抽象成员的对象。

```scala
import scala.collection.mutable
import java.util.Collection

abstract class SetAndType{
  type Elem
  val set: mutable.Set[Elem]
}

def javaSet2ScalaSet[T](jset: Collection[T]): SetAndType = {
  val sset = mutable.Set.empty[T] //现在可以使红T来表示这个类型
  val iter = jset.iterator
  while(iter.hasNext)
    sset += iter.next()
  
  return new SetAntType{
    type Elem = T
    val set = sset
  }
}
```

这就是Scala通常不使用通配类型。为了用他们实现任何复杂一点的东西，就会倾向于将他们转换成使用抽象成员，既然如此，一开始就使用抽象成员也完全可以。

### 31.4 同时编译Scala和Java

当编译依赖Java的Scala代码是，首先将Java代码构建成类文件，然后再编译Scala代码，将Java代码的文件放在类路径中，不过反过来，Java无法调用Scala的代码。

为了支持这种构件场景，`Scala允许同时面对Java代码和Java类文件做编译。你需要做的就是将Java代码放在命令行中，就当做它们是Scala文件那样`。**Scala不会编译这些文件，但是会扫描它们，看看包含了什么内容**。要利用这一点，首先用Java源文件编译Scala代码，然后再利用Scala编译出的类文件编译Java代码，下面是典型的命令执行顺序：

```bash
$ scalac -d bin InventoryAnalysis.scala InventoryItem.java Inventory.java
$ javac -cp bin -d bin Inventory.java InventoryItem.java InventoryManagement.java
$ scala -cp bin InventoryManagement
Most expensive item = sprocket($4.99)
```

### 31.5 基于Scala 2.12特性的Java8集成

Java8对Java语言和字节码做了一些改进，从Scala 2.12版本开始用到了这些改进，利用Java8的这些新特性，Scala 2.12的编译器可以生成更小的类文件和jar文件，同时改善了特质的二进制兼容性。

#### 31.5.1 Lambda表达式和SAM类型

有了lambda表达式，可以改进匿名类的定义，以前定义可能像这样：

```java
JButton button = new JButton();
button.addActionListener(
  new ActionListener(){
    public void actionPerformed(ActionEvent event){
      System.out.println("pressed!");
    }
  }
);
```

使用lambda表达式改进ActionListener接口的实现：

```java
JButton button = new JButton();
button.addActionListener(
  event -> System.out.println("pressed!")
);
```

在Scala中，相同的情形下使用匿名内部类的实例，不过可能更倾向于使用函数字面量，就像这样：

```scala
val button = new JButton
button.addActionListener(
  _ => println("pressed!")
)
```

可以通过定义一个从`ActionEvent => Unit`函数类型到ActionListener的隐式转换来支持这样的编码风格。

Scala 2.12允许我们在这样的情况下直接使用函数字面量，而不需要定义隐式转换。跟Java8一样，Scala 2.12也允许任何要求某个声明了单个抽象方法(SAM)的类或特质的实例的地方使用函数类型的值。在Scala 2.12中，任何SAM都可以，比如，可以定义一个Increaser特质，这个特质带有唯一的一个抽象方法increase：

```scala
trait Increaser{
  def increase(i: Int): Int
}
```

然后可定义一个接收Increaser的方法：

```scala
def increaseOne(increaser: Increaser): Int =
  increaser.increase(1)
```

为了调用新的方法，可以传入一个Increaser特质的匿名实例，就像这样：

```scala
increaseOne(
  new Increaser{
    def increase(i: Int): Int = i + 7
  }
)
```

不过在Scala 2.12中，可以简单使用一个函数字面量，因为Increaser是一个SAM类型：

```scala
increaseOne(i => i + 7)
```

#### 31.5.2 从Scala 2.12使用Java 8的Stream

Java的Stream是一个函数式的数据结构，提供了接收java.util.function.IntUnaryOperator参数的map方法。可以从Scala调用Stream.map来对Array的每个元素加1：

```scala
scala> import java.util.function.IntUnaryOperator
import java.util.function.IntUnaryOperator

scala> import java.util.Array
ArrayDeque   ArrayList   Arrays

scala> import java.util.Arrays
import java.util.Arrays

scala> val stream = Arrays.stream(Array(1, 2, 3))
stream: java.util.stream.IntStream = java.util.stream.IntPipeline$Head@7cf8f45a

scala> stream.map(
     | new IntUnaryOperator{
     | def applyAsInt(i: Int): Int = i + 1
     | }
     | ).toArray
res0: Array[Int] = Array(2, 3, 4)
```

不过由于IntUnaryOperator是一个SAM类型，可以使用函数字面量来更加精简地调用它：

```scala
scala> val stream = Arrays.stream(Array(1, 2 ,3))
stream: java.util.stream.IntStream = java.util.stream.IntPipeline$Head@54c697c

scala> stream.map(i => i +1).toArray
res3: Array[Int] = Array(2, 3, 4)
```

注意，**只有函数字面量会被适配成SAM类型，并非任意的拥有函数类型的表达式**，如果定义下面一个表达式：

```scala
scala> val f = (i: Int) => i + 1
f: Int => Int = $$Lambda$1318/221029289@2b26d289
```

尽管f跟之前传入`stream.map`的函数字面量有着相同的类型，并不能在要求IntUnaryOperator的地方使用f:

```scala
scala> val stream = Arrays.stream(Array(1, 2, 3))
stream: java.util.stream.IntStream = java.util.stream.IntPipeline$Head@76afb9d

scala> stream.map(f).toArray
<console>:16: error: type mismatch;
 found   : Int => Int
 required: java.util.function.IntUnaryOperator
       stream.map(f).toArray
                  ^
```

要使用f，可以显式地用函数字面量来调用它，就像这样：

```scala
scala> stream.map(i => f(i)).toArray
res5: Array[Int] = Array(2, 3, 4)
```

或者在定义f的时候，将f标注为IntUnaryOperator，即stream.map预期的类型：

```scala
scala> val f: IntUnaryOperator = i => i + 1
f: java.util.function.IntUnaryOperator = $$Lambda$1339/319357437@659f5f32

scala> val stream = Arrays.stream(Array(1, 2, 3))
stream: java.util.stream.IntStream = java.util.stream.IntPipeline$Head@70b2819f

scala> stream.map(f).toArray
res6: Array[Int] = Array(2, 3, 4)
```

有了Scala 2.12和Java 8，还可以从Java调用编译后的Scala方法，用Java的Lambda表达式传入Scala函数类型的值。虽然Scala的函数类型定义为包含具体方法的特质，Scala 2.12会将特质编译成带有默认方法的Java接口。这样，在Java看来，Scala的函数类型其实跟SAM没什么两样。

## 32 Future和并发编程

Java提供了围绕共享内存和锁构建的并发支持。虽然支持是完备的，这种并发方案实际过程中缺很难做到。Scala标准库提供了另一种能够规避这些难点的选择，将程序员的精力集中在不可变状态的异步变换上：也就是Future。

虽然Java也提供了Future，它跟Scala的Future非常不同。两种Future都是用来表示某个异步计算的结果，但**Java的Future要求通过阻塞的get方法来访问这个结果**。虽然可以在调用get之前先调用isDone来判断某个Java的Future是否已经完成，从而避免阻塞，缺必须要等到Java的Future完成之后才能继续用这个结果做进一步计算。

Scala的Future则不同，不论计算是否完成，都可以指定对它的变换逻辑。每一个变换都产生新的Future来表示原始的Future经过给定的函数变换后产生的异步结果。执行计算的线程由隐式给出的执行上下文决定。这使得可以将异步的计算描述成一系列对不可变值的变换，完全不需要考虑共享内存和锁。

### 32.1 天堂里的烦恼

在Java平台上，每个对象都关联了一个逻辑监视器，可以用来控制对数据的多线程访问。使用这种模式需要由你来界定哪些数据将被多个线程共享，并将访问共享数据或控制对这些共享数据访问的代码段标记为**synchronized**。Java运行时将运用一种锁的机制来确保同一时间只有一个线程进入有同一个锁控制的同步代码段，从而让你可以协同共享数据的多线程访问。

为了兼容，Scala提供了Java并发原语的访问，可以使用Scala调用wait、notify和notifyAll等方法，它们的含义跟Java中的方法一样。从技术上讲，**Scala没有synchronized关键字，不过有一个synchorized方法**：

```scala
var counter = 0
synchorized{
  //这里每次都只有一个线程在执行
  counter = counter + 1
}
```

但是，程序员发现要使用共享数据和锁模型来有把握地构建健壮的、多线程的应用程序十分困难。这当中的问题，在程序中的没一点，都必须推断哪些正在修改或访问的数据可能会被其他线程修改或访问，以及在这一点上你握有哪些锁。每次方法调用，都必须推断出它将会尝试握有哪些锁，并说服自己它这样做不会产生死锁。而在推断中的这些锁，并不是编译器就固定下来，这让问题变得更加复杂，因为程序可以在运行时的执行过程中任意创建新的锁。

更糟糕的是，对于多线程而言，测试是不可靠的，由于线程是非确定性的，可能测试1000次都是成功的，而程序第一次在客户的机器上运行就出现问题。对共享数据和锁，必须通过推断来把程序做对，没有其他方法。

不仅如此，也无法通过过度的同步来解决问题，同步一切可能并不比什么都不同步更好。这中间额问题尽管是新的锁操作去掉了争用状况的可能，但同时增加了新的死锁的可能。一个正确的使用锁的程序既不能存在争用状况，也不能有死锁，因此不论往哪个方向做过头都是不安全的。

java.util.concurrent类库提供了并发编程额更高级别的抽象。使用并发工具包进行多线程编程比你用低级别的同步语法制作自己的抽象可能会带来的问题要少的多。尽管如此，并发工具包也是基于共享数据和锁的，因此并没有从根本上解决使用这种模型的种种困难。

### 32.2 异步执行和Try

Scala的Future提供了一种可以减少（甚至免去）对共享数据和所进行推理的方式。当你调用Scala方法是，它“在你等待的过程中”，执行某项计算并返回结果。`如果结果是一个Future，那么这个Future就表示另一个将异步执行的计算，而该计算通常是由另一个不同的线程来完成的`。因此，`对Future的许多操作都需要一个隐式的执行上下文来提供异步执行的函数的策略`。如果试着通过Future.apply工厂方法创建一个future但又不提供隐式的执行上下文，编译器将抛出一个错误：

```scala
scala> import scala.concurrent.Future
import scala.concurrent.Future

scala> val fut = Future{Thread
Thread   ThreadDeath   ThreadGroup   ThreadLocal

scala> val fut = Future{Thread.sleep(10000); 21 + 21}
<console>:12: error: Cannot find an implicit ExecutionContext. You might pass
an (implicit ec: ExecutionContext) parameter to your method.

The ExecutionContext is used to configure how and on which
thread pools Futures will run, so the specific ExecutionContext
that is selected is important.

If your application does not define an ExecutionContext elsewhere,
consider using Scala's global ExecutionContext by defining
the following:

implicit val ec: scala.concurrent.ExecutionContext = scala.concurrent.ExecutionContext.global
       val fut = Future{Thread.sleep(10000); 21 + 21}
                       ^
```

这个错误提示需要引入Scala提供的一个全局的执行上下文，对JVM而言，这个全局的执行上下文使用的是一个线程池。引入这个全局执行上下文之后，就可以正常创建Future了：

```scala
scala> import scala.concurrent.ExecutionContext.Implicits.global
import scala.concurrent.ExecutionContext.Implicits.global

scala> val fut = Future{Thread.sleep(10000); 21 + 21}
fut: scala.concurrent.Future[Int] = Future(<not completed>)
```

创建完这个线程之后，需要等待10秒才能完成。Future有两个方法可以轮询fut是否完成和访问fut的返回值：`isCompleted`和`value`。

如果是未完成：

```scala
scala> fut.isCompleted
res0: Boolean = false

scala> fut.value
res1: Option[scala.util.Try[Int]] = None
```

如果是已完成：

```scala
scala> fut.isCompleted
res2: Boolean = true

scala> fut.value
res3: Option[scala.util.Try[Int]] = Some(Success(42))
```

这个value返回的可选值包含一个`Try`，如下继承关系，一个Try要么是包含类型为T的值的Success，要么是包含一个异常（java.lang.Throwable的实例）的Failure。Try的目的是为了异步计算提供一种与同步计算中返回try表达式类似的东西：**允许你处理那些计算有可能异常终止而不是返回结果的情况**。

```plain txt
        ┌─────────────────────┐
        │     scala.util      │
        │      Try[+T]        │
        │ <<sealed abstract>> │
        └─────────────────────┘
                   ▲
        ┌──────────┴─────────┐
        │                    │
┌────────────────┐   ┌─────────────────┐
│   scala.util   │   │   scala.util    │
│   Success[T]   │   │   Failure[T]    │
│ <<final case>> │   │ <<final case>>  │
└────────────────┘   └─────────────────┘
```

对于痛不而言，可以用try/catch来确保调用某个方法的线程可以捕获并处理由该方法抛出的异常。不过对于异步计算来说，发起该计算的线程通常都去转别的任务去了。在这之后如果异步计算因为某个异常失败了，原始的线程就不再能够用catch来处理这个异常。因此，当处理表示异步活动的Future时，就要用Try来处理这种情况：该活动未能交出某个结果，而是异常终止了。下面展示了异步活动失败场景的例子：

```scala
scala> val fut = Future { Thread.sleep(10000); 21 / 0 }
fut: scala.concurrent.Future[Int] = Future(<not completed>)

scala> fut.value
res4: Option[scala.util.Try[Int]] = None

//当这个任务完成后：
scala> fut.value
res5: Option[scala.util.Try[Int]] = Some(Failure(java.lang.ArithmeticException: / by zero))
```

### 32.3 使用Future

Scala的Future让你对Future的结果指定变换后得到一个新的*future*来表示这两个异步计算的组合：**原始计算和变换**。

#### 32.3.1 用map对Future做变换

可以直接将下一个计算**map**到当前的future，而不是阻塞（等待结果）然后继续做另一个计算。这样做的结果将会是一个新的future，表示原始的异步计算结果经过传给map的函数异步变换后的结果。例如：

```scala
val fut = Future { Thread.sleep(10000); 21 + 21 }
fut: scala.concurrent.Future[Int] = Future(<not completed>)
```

会在10秒后完成，对这个future应设一个加1的函数将会交出意外一个future，这个新的future将表示由原始的加法和跟在后面的一次加1组成的计算：

```scala
scala> val result = fut.map(_ + 1)
result: scala.concurrent.Future[Int] = Future(<not completed>)

scala> result.value
res6: Option[scala.util.Try[Int]] = None
```

一旦fut完成，并且该函数被应用到其结果后，map方法返回的那个future也会完成：

```scala
scala> result.value
res7: Option[scala.util.Try[Int]] = Some(Success(43))
```

#### 32.3.2 用for表达式对Future做变换

由于Scala的Future还声明了flatMap方法，可以用for表达式来对future做变换：

```scala
scala> val fut1 = Future{ Thread.sleep(10000); 21 + 21 }
fut1: scala.concurrent.Future[Int] = Future(<not completed>)

scala> val fut2 = Future{ Thread.sleep(10000); 23 + 23 }
fut2: scala.concurrent.Future[Int] = Future(<not completed>)
```

有了这两个Future，可以得到一个新的表示它们结果的异步和新future，就像这样：

```scala
scala> for {
     |   x <- fut1
     |   y <- fut2
     | } yield x + y
res8: scala.concurrent.Future[Int] = Future(<not completed>)

scala> res8.value
res9: Option[scala.util.Try[Int]] = Some(Success(88))
```

由于for表达式会串行化它们的变换（本例中给出的表啊是会被重写成一个对fut1.flatMap的调用，传入一个fut2.map的函数：`fut1.flatMap(x +. fut2.map(y => x + y))`），**如果不在for表达式之前创建future，它们就不会并行运行**。例如前面的for表达式需要大约10秒完成，下面的表达式至少需要20秒：

```scala
scala> for{
     | x <- Future { Thread.sleep(10000); 21 + 21 }
     | y <- Future { Thread.sleep(10000); 23 + 23 }
     | } yield x + y
res10: scala.concurrent.Future[Int] = Future(<not completed>)

scala> res10.value
res11: Option[scala.util.Try[Int]] = Some(Success(88))
```

#### 32.3.3 创建Future

除了之前创建future的apply方法之外，Future伴生对象还提供了三个创建依然完成的future的工厂方法：successful、failed和fromTry。这些工厂方法不需要ExecutionContext。

**successful**方法创建一个已经完成的future：

```scala
scala> Future.successful{21 + 21}
res12: scala.concurrent.Future[Int] = Future(Success(42))
```

**failed**方法创建一个已经失败的future：

```scala
scala> Future.failed{ new Exception("bummer!") }
res13: scala.concurrent.Future[Nothing] = Future(Failure(java.lang.Exception: bummer!))
```

**fromTry**方法将从给定的Try创建一个已经完成的future：

```scala
scala> import scala.util.{Success, Failure}
import scala.util.{Success, Failure}

scala> Future.fromTry(Success {21 + 21})
res14: scala.concurrent.Future[Int] = Future(Success(42))

scala> Future.fromTry(Failure(new Exception("Bummer!")))
res15: scala.concurrent.Future[Nothing] = Future(Failure(java.lang.Exception: Bummer!))
```

创建future最一般化的方式是使用**Promise**。给定一个promise，可以得到一个有这个promise控制的future。当你完成promise时，对应的future也会完成：

```scala
scala> import scala.concurrent.Promise
import scala.concurrent.Promise

scala> val pro = Promise[Int]
pro: scala.concurrent.Promise[Int] = Future(<not completed>)

scala> val fut = pro.future
fut: scala.concurrent.Future[Int] = Future(<not completed>)

scala> fut.value
res16: Option[scala.util.Try[Int]] = None
```

可以使用success、failure和complete的方法来完成promise。Promise的这些方法跟前面介绍的构造依然完成的future的方法很像，例如，success方法将成功地完成future：

```scala
scala> pro.success(42)
res17: pro.type = Future(Success(42))

scala> fut.value
res18: Option[scala.util.Try[Int]] = Some(Success(42))
```

`failure方法`接收一个会让future，因为它是失败的异常。complete方法接收一个Try。还有一个接收future的completeWith方法，这个方法使得该promise的future的完成状态跟你传入的future保持同步。

#### 32.3.4 filter和collect

Scala的future提供了两个方法：**filter**和**collect**，来确保某个future值保持某种性质。filter方法对future结果进行校验，如果合法就原样保留，这里有一个确保Int是正数的例子：

```scala
scala> val fut = Future(42)
fut: scala.concurrent.Future[Int] = Future(<not completed>)

scala> val valid = fut.filter(res => res > 0)
valid: scala.concurrent.Future[Int] = Future(<not completed>)

scala> valid.value
res19: Option[scala.util.Try[Int]] = Some(Success(42))
```

如果future值非法，那么filter返回的这个future就会以NoSuchElementException失败：

```scala
scala> val invalid = fut.filter(res => res < 0)
invalid: scala.concurrent.Future[Int] = Future(<not completed>)

scala> invalid.value
res20: Option[scala.util.Try[Int]] = Some(Failure(java.util.NoSuchElementException: Future.filter predicate is not satisfied))
```

由于Future还提供了withFilter方法，可以用for表达式的过滤器来执行同样的操作：

```scala
scala> val valid = for(res <- fut if res > 0) yield res
valid: scala.concurrent.Future[Int] = Future(<not completed>)

scala> valid.value
res21: Option[scala.util.Try[Int]] = Some(Success(42))

scala> val invalid = for (res <- fut if res < 0) yield res
invalid: scala.concurrent.Future[Int] = Future(<not completed>)

scala> invalid.value
res22: Option[scala.util.Try[Int]] = Some(Failure(java.util.NoSuchElementException: Future.filter predicate is not satisfied))
```

Future的`collect方法`允许再一次操作中同时完成校验和变换。如果传给collect方法的偏函数对future结果有定义，那么collect返回的future就会以经过该函数变换后的值成功完成。

```scala
scala> val valid = fut collect { case res if res > 0 => res + 46 }
valid: scala.concurrent.Future[Int] = Future(<not completed>)

scala> valid.value
res23: Option[scala.util.Try[Int]] = Some(Success(88))
```

而如果偏函数对future结果没有定义，那么collect返回的这个future就将以NoSuchElementException失败：

```scala
scala> val invalid = fut collect {case res if res < 0 => res + 46}
invalid: scala.concurrent.Future[Int] = Future(<not completed>)

scala> invalid.value
res24: Option[scala.util.Try[Int]] = Some(Failure(java.util.NoSuchElementException: Future.collect partial function is not defined at: 42))
```

#### 32.3.5 处理失败

Scala的Future提供了处理失败的future的方式，包括failed、fallbackTo、recover和recoverWith。failed方法将任何类型的失败的future变换成一个成功的Future[Throwable]，带上引发失败的异常：

```scala
scala> val failure = Future{42 / 0}
failure: scala.concurrent.Future[Int] = Future(<not completed>)

scala> failure.value
res25: Option[scala.util.Try[Int]] = Some(Failure(java.lang.ArithmeticException: / by zero))

scala> val expectedFailure = failure.failed
expectedFailure: scala.concurrent.Future[Throwable] = Future(Success(java.lang.ArithmeticException: / by zero))

scala> expectedFailure.value
res26: Option[scala.util.Try[Throwable]] = Some(Success(java.lang.ArithmeticException: / by zero))
```

**如果被调用failed方法的future最终成功了，那么failed返回的这个future将NoSuchElementException失败**。因此failed方法只有在你预期某个future一定会失败的情况下才适用：

```scala
scala> val success = Future{42 / 1}
success: scala.concurrent.Future[Int] = Future(<not completed>)

scala> success.value
res27: Option[scala.util.Try[Int]] = Some(Success(42))

scala> val unexpectedSuccess = success.failed
unexpectedSuccess: scala.concurrent.Future[Throwable] = Future(Failure(java.util.NoSuchElementException: Future.failed not completed with a throwable.))

scala> unexpectedSuccess.value
res28: Option[scala.util.Try[Throwable]] = Some(Failure(java.util.NoSuchElementException: Future.failed not completed with a throwable.))
```

`fallbackTo方法`允许提供一个额外可选的future，这个future将用于在你调用fallbackTo的那个future失败的情况。以下是一个失败的future回退降级到另一个成功future的例子：

```scala
scala> val fallback = failure.fallbackTo(success)
fallback: scala.concurrent.Future[Int] = Future(Success(42))

scala> fallback.value
res29: Option[scala.util.Try[Int]] = Some(Success(42))
```

如果被调用fallbackTo方法的原始future失败了，那么传递给fallbackTo的future的失败（如果失败）会被忽略。fallbackTo返回这个future会以最初的异常失败：

```scala
scala> val failedFallback = failure.fallbackTo(Future {val res = 42; requir
e(res < 0); res})
failedFallback: scala.concurrent.Future[Int] = Future(<not completed>)

scala> failedFallback.value
res30: Option[scala.util.Try[Int]] = Some(Failure(java.lang.ArithmeticException: / by zero))
```

`recover方法`让你可以把失败的future变换成成功的future，同时将成功的future结果原样透传。例如，对于一个以ArithmeticException失败的future，可以用recover方法将它变成成功的future，就像这样：

```scala
scala> val recovered = failedFallback recover{case ex: ArithmeticException
=> -1}
recovered: scala.concurrent.Future[Int] = Future(<not completed>)

scala> recovered.value
res31: Option[scala.util.Try[Int]] = Some(Success(-1))
```

如果原始的future没有失败，那么recover返回的这个future就会以相同的值完成：

```scala
scala> val unrecovered = fallback recover{case ex: ArithmeticException => -
1}
unrecovered: scala.concurrent.Future[Int] = Future(<not completed>)

scala> unrecovered.value
res32: Option[scala.util.Try[Int]] = Some(Success(42))
```

这里有一个使用新的transform方法将失败变为成功的例子：

```scala
scala> val nonNegative = failure.transform{
  case Success(res) => Success(res.abs + 1)
  case Failure(_) => Success(0)
}
nonNegative: scala.concurrent.Future[Int] = Future(<not completed>)

scala> nonNegative.value
res33: Option[scala.util.Try[Int]] = Some(Success(0))
```

#### 32.3.6 组合Future

Future和它的伴生对象提供了用于组合多个future的方法。`zip方法`将两个成功的future变换成这两个值的元组的future：

```scala
scala> val zippedSuccess = success zip recovered
zippedSuccess: scala.concurrent.Future[(Int, Int)] = Future(Success((42,-1)))

scala> zippedSuccess.value
res34: Option[scala.util.Try[(Int, Int)]] = Some(Success((42,-1)))
```

如果有任何一个future失败了，zip返回的这个future也会以相同的异常失败：

```scala
scala> val zippedFailure = success zip failure
zippedFailure: scala.concurrent.Future[(Int, Int)] = Future(Failure(java.lang.ArithmeticException: / by zero))

scala> zippedFailure.value
res35: Option[scala.util.Try[(Int, Int)]] = Some(Failure(java.lang.ArithmeticException: / by zero))
```

如果两个future都失败了，那么最终失败的这个future将会包含头一个future的异常，也就是被调用zip方法的哪一个。

Future的伴生对象提供了一个`fold方法`，用来累计一个TraversableOnce集合中所有future的结果，并交出一个future的结果，如果集合中所有的future都成功了，那么结果的future讲义累计的结果成功完成。而**如果集合中有任何future失败了，结果的future也将失败。如果有多个future失败了，结果的future将会跟第一个失败的future**（最先出现在TraversableOnce集合中的）相同的异常失败：

```scala
scala> val folded = Future.fold(futureNums)(0){(acc, num) => acc + num}

folded: scala.concurrent.Future[Int] = Future(<not completed>)

scala> folded.value
res36: Option[scala.util.Try[Int]] = Some(Success(88))
```

`Future.reduce方法`执行跟fold一样的折叠操作，只不过`不带零值`，而是用第一个future结果作为起始值：

```scala
scala> val reduced = Future.reduce(futureNums){(acc, num) => acc + num}

reduced: scala.concurrent.Future[Int] = Future(<not completed>)

scala> reduced.value
res37: Option[scala.util.Try[Int]] = Some(Success(88))
```

如果传一个空的集合给reduce，结果的future将以NoSuchElementException失败。

`Future.sequence方法`将一个TraversableOnce的future集合变成一个由值组成的TraversableOnce的future。比如，下面用一个sequence将一个List[Future[Int]]变换成了一个Future[List[Int]]：

```scala
scala> val futureList = Future.sequence(futureNums)
futureList: scala.concurrent.Future[List[Int]] = Future(<not completed>)

scala> futureList.value
res39: Option[scala.util.Try[List[Int]]] = Some(Success(List(42, 46)))
```

`Future.traverse方法`会将任何元素类型的TraversableOnce集合变换成一个由future组成的TraversableOnce，并将它“sequence”成一个由值组成的TraversableOnce的future。例如，将`List[Int]`就被Future.traverse变换成了`Future[List[Int]]`：

```scala
scala> val traversed = Future.traverse(List(1, 2, 3)) {i => Future(i)}
traversed: scala.concurrent.Future[List[Int]] = Future(<not completed>)

scala> traversed.value
res40: Option[scala.util.Try[List[Int]]] = Some(Success(List(1, 2, 3)))
```

#### 32.3.7 执行副作用

有时候，可能想要在某个future完成后执行一个副作用。Future为此提佛那个了好几种方法。最基本的方法就是foreach，如果future完成就会执行一个副作用，future失败则不会执行：

```scala
scala> failure.foreach(ex => println(ex))

scala> success.foreach(res => println(res))
42
```

由于不带yield的for会被编译器重写成对foreach的调用，可以通过for表达式达成同样的效果：

```scala
scala> for (res <- failure) println(res)

scala> for (res <- success) println(res)
42
```

Future还提供两个方法来注册“回调（callback）”函数。`onComplete方法`最终成功或失败是会被执行。这个函数会被传入一个Try（如果future成功了就是一个包含结果的Success），如果失败就是一个包含造成失败的异常Failure。

```scala
scala> success onComplete{
     | case Success(res) => println(res)
     | case Failure(ex) => println(ex)
     | }
42

scala> failure onComplete{
     | case Success(res) => println(res)
     | case Failure(ex) => println(ex)
     | }
java.lang.ArithmeticException: / by zero
```

Future并不保证通过onComplete注册的回调函数的任何执行顺序。如果想要强制回调函数的顺序，必须使用andThen。`andThen方法`会返回一个新的对原始的被调用andThen的future做镜像（随之成功或失败）的future，不过会执行完回调再完成：

```scala
scala> val newFuture = success andThen{
     | case Success(res) => println(res)
     | case Failure(ex) => println(ex)
     | }
newFuture: scala.concurrent.Future[Int] = Future(<not completed>)
42

scala> newFuture.value
res48: Option[scala.util.Try[Int]] = Some(Success(42))
```

注意，如果传入andThen的回调函数在执行时抛出了异常，这个异常是不会被传到到后续回调或是通过结果的future报出来的。

#### 32.3.8 Scala 2.12中添加的其他方法

在Scala 2.12中添加的`flatten方法`将一个嵌套在另一个Future的Future变换成一个内嵌类型的Future。比如，flatten可以将一个`Future[Future[Int]]`变换成一个`Future[Int]`：

```scala
scala> val nestedFuture = Future{Future{42}}
nestedFuture: scala.concurrent.Future[scala.concurrent.Future[Int]] = Future(<not completed>)

scala> val flattened = nestedFuture.flatten
flattened: scala.concurrent.Future[Int] = Future(Success(42))
```

Scala 2.12中添加的`zipWith方法`本质上将两个Future zip到一起，然后对结果的元组执行map。下面的例子就是两步处理，先zip再map：

```scala
scala> val futNum = Future{21 + 21}
futNum: scala.concurrent.Future[Int] = Future(<not completed>)

scala> val futStr = Future{"ans" + "wer"}
futStr: scala.concurrent.Future[String] = Future(<not completed>)

scala> val zipped = futNum zip futStr
zipped: scala.concurrent.Future[(Int, String)] = Future(Success((42,answer)))

scala> val mapped = zipped map{
     | case (num, str) => s"$num is the $str"
     | }
mapped: scala.concurrent.Future[String] = Future(<not completed>)

scala> mapped.value
res49: Option[scala.util.Try[String]] = Some(Success(42 is the answer))
```

zipWith允许你一步完成同样的操作：

```scala
scala> val fut = futNum.zipWith(futStr){
     | case (num, str) => s"$num is the $str"
     | }
fut: scala.concurrent.Future[String] = Future(<not completed>)

scala> fut.value
res50: Option[scala.util.Try[String]] = Some(Success(42 is the answer))
```

Scala 2.12中还增加了一个`transformWith方法`，可以用一个Try到Future的函数对future进行变换。

```scala
scala> val flipped = success.transformWith{
     | case Success(res) => Future{throw new Exception(res.toString)}
     | case Failure(ex) => Future{21 + 21}
     | }
flipped: scala.concurrent.Future[Int] = Future(<not completed>)

scala> flipped.value
res51: Option[scala.util.Try[Int]] = Some(Failure(java.lang.Exception: 42))
```

这个transformWith跟Scala 2.12中新增的重载transform方法类似。不过不想transform那样要求你传入的函数交出Try，transformWith允许交出future。

### 32.4 测试Future

Scala的future的一个优势是它们能帮你避免阻塞。在大所属JVM实现中，创建数千个线程以后，在线程之间的上下文切换就会让性能变得无法接受。通过避免阻塞，可以保持一组有限数量的线程，让它们不停工作。尽管如此，Scala也允许你在需要的时候在一个future上阻塞（等待它的结果）。Scala的Await对象提供了等待结果的手段。参考下面的例子：

```scala
scala> import scala.concurrent.Await
Await   Awaitable

scala> import scala.concurrent.Await
import scala.concurrent.Await

scala> import scala.concurrent.duration._
import scala.concurrent.duration._

scala> val fut = Future{Thread.sleep(10000); 21 + 21}
fut: scala.concurrent.Future[Int] = Future(<not completed>)

scala> val x = Await.result(fut, 15.seconds)
x: Int = 42
```

Await.result接收一个Future和一个Duration。这里的Duration用于表示Await.result应该等待多长时间让Future完成，如果到时间未完成则触发超时。在本例中，给Duration执行了15秒。因此Await.result方法不应该在future完成并得到最终结果42之前超时。

阻塞被广泛接收的一个场景是对异步代码的测试。既然Await.result已经返回，就可以用这个结果来执行计算了，比如测试中会用到的断言：

```scala
scala> import org.scalatest.Matchers._

scala> s should be (42)
res0: org.scalatest.Assertion = Successed
```

或者也可以使用ScalaTest的ScalaFutures特质提供的阻塞结构。比如ScalaFuture为Future隐式添加的futureValue方法，会阻塞直到future完成，如果future失败了，futureValue就会抛出TestFailedException来描述这个问题。如果future成功了，futureValue将返回该future成功的结果，供你对这个结果执行断言：

```scala
scala> import org.scalatest.concurrent.ScalaFutures._
import org.scalatest.concurrent.ScalaFutures._

scala> val fut = Future { Thread.sleep(10000); 21 + 21 }
fut: scala.concurrent.Future[Int] = ...

scala> fut.futureValue should be (42) // futureValue blocks
res1: org.scalatest.Assertion = Succeeded
```

虽然在测试中阻塞通常没什么问题，ScalaTest 3.0添加了“异步”测试的风格，可以以不阻塞的方式测试future。拿到future以后，并不是先阻塞然后对结果执行断言，而是将断言直接map到future上然后返回Future[Assertion]给ScalaTest。下面例子总，当这个future的断言完成时，ScalaTest会异步地将时间（测试成功、测试失败等）发送给报告程序。

```scala
import org.scalatest.AsyncFunSpec
import scala.concurrent.Future

class AddSpec extends AsyncFunSpec {
  def addSoon(addends: Int*): Future[Int] = Future {
    addends.sum
  }

  describe("addSoon") {it("will eventually compute a sum of passed Ints") {
    val futureSum: Future[Int] = addSoon(1, 2)
    // 可以将断言映射到Future，然后返回
    // 得到的Future[Assertion]给ScalaTest
    futureSum map { sum => assert(sum == 3)
    }
  }
}
```

异步测试的用例展示了处理future的一般原则：一旦进入“future空间”，就尽量待在future空间里。不要对future阻塞拿到结果后再继续计算，而是通过执行一系列的变换，而每个变换都返回新的future供后续进一步变换处理来保持异步。需要从future空间拿到结果是，注册副作用，在future完成时异步执行。这种方式可以让你最大限度地利用线程。
