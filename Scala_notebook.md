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
               ┌────────────────────────┐
               │                        │
┌──────────────────────────┐ ┌─────────────────────────┐
│scala.collection.immutable│ │scala.collection.mutable │
│         HashSet          │ │         HashSet         │
│        <<trait>>         │ │        <<trait>>        │
└──────────────────────────┘ └─────────────────────────┘
             ▲                           ▲
             │                           │
┌──────────────────────────┐ ┌─────────────────────────┐
│scala.collection.immutable│ │scala.collection.mutable││
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

一个仅因其副作用而执行的方法称为`过程(procedure)`。类CheckSumAccumulator中的add方法，就是需要其执行之后的产生副作用。

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

**catch子句**

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

**finally子句**

```scala
import java.io.FileReader

val file = new FileReader("input.txt")
try{

} finally {
  file.close()
}
```

**交出值**

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

在函数内部，重复参数的的类型为参数声明类型的一个数组`Array[String]`。如果你有一个某种类型的数组，你试图把它作为一个重复参数传递，编译器会抛出一个错误。

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

**跟踪尾递归**

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

**尾递归的限制**

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
             ┌────────────────┐
             │                │
┌──────────────────┐    ┌────────────────┐
│   ArrayElement   │    │ UniformElement │
└──────────────────┘    └────────────────┘
           ▲
           │
┌──────────────────────┐
│     LineElement      │
└──────────────────────┘
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
      ┌─────────────────────────────────┐
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
final  def ==(that: Any): Boolean
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

可以定义自己的类型来对呢基金按的值类进行扩充，跟内建的值类一样，自定义的值类的实例通常也会编译成那种不使用包装类的Java字节码。在需要包装类的上下文里，比如泛型代码，值将被自动装箱和拆箱。

**只有特定的及各类可以定义为值类。要使得某个类成为值类，他不仅有且仅有一个参数，并且在内部除了def之外不能有任何东西**。不仅如此，也不能有其他扩展自值类，且值类不能重新定义`equals`或`hashCode`。

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

`showFruit`中`fruit`是`Fruit`类型，直接����������`fruit`的全部成员，可以直接访问`name`和`color`，与`fruit.name`和`fruit.color`代表的含义相同。

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

在Scala中，对受保护成员的访问也比在Java中有更多的限制。在Scala中，`受保护成员只能从定义成员的类的子类中访问`。在Java中，这样的访问也可以来自同一个包中的其他类。

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

模式守卫以if开头，模式守卫可以使任意的布尔表达式，通常引用到模式中的变量。如果存在模式守卫，这个匹配仅在模式守卫求值得到true时才会成功。因此，上面提到的首个case只能匹配那些两个操作员相等的二元操作。

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
|||
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

跟`::`类似，拼接列表使用`:::`，不同于`::`，`:::`接收两个列表参数为操作元：

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

### 19.1 函数时队列

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

与Java一样，在Scala中，内部类实例持有对外部类实例的引用。这允许内部类访问其外部类的成员。因此，如果不以某种方式指定外部类实例，就不能实例化内部类。一种方法是在外部类的主体中实例化内部类。在这种情况下，将使用当前的外部类实例(使用this引用)。

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

这样的设计可用，但是并不完善，因为缺少了+和*的具体定义。那么如何在类中实现这两个方法呢？可能会使用`this.amount + that.amount`来实现+方法，但是怎么将不同币种的数据转化成同一币种，然后相加呢？可能会这么实现：

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

`作用域规则：被插入的隐式转换必须是当前作用域的单个标识符，或者跟隐式转换的源类型或目标类型有关联`。Scala编译器只会考虑那些在作用域内的隐式转换。因此，必须以某种方式将隐式转换引入到当前作用域才能使用它们。不仅如此，`隐式转换在当前作用域必须是单个标识符`。编译器不会插入someVariable.convert这种形式的转换。如果想要someVariable.convert可用，必须引入它，成为单个标识符，引入后就可用。常见的做法是提供一个包含了一些有用隐式转换的Preamble对象。这样使用这个类库的代码就可以通过“import Preamble._”来访问该类库的隐式转换。

此外，单标识符有个例外，`编译器还会在隐式转换的源类型或目标类型的伴生对象中查找隐式定义`。如果尝试将Dollar对象传给Euro，可以将一个从Dollar到Euro的隐式转换打包在Dollar或Euro任何一个类的伴生对象中：

```scala
object Dollar{
  implicit def dollarToEuro(x: Dollar): Euro = ...
}
class Dollar{...}
```

作用于规则有助于模块化的推理，如果隐式转换是全局可用的，那么要理解某个代码的文件，就需要知道在程序的任何地方添加的每个隐式定义！

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

到期望类型的转换可以在预期不同类型的上下文中使用（当前已持有）某个类型。例如，有一个String但想将它传给一个要求IndexSeq[Char]的方法，选择接收端的转换让我们适配方法调用的接收端（即方法调用的对象），如果原始类型不支持这个调用，例如“abc”.exists，这段代码会被转换为stringWrapper("abc").exists，因为exists在String上不可用，但是在IndexSeq[Char]可用。

隐式参数通常用来给调用的函数提供更多关于调用者诉求的信息。隐式参数对于泛型函数尤其有用。

### 21.3 隐式转换到一个预期的类型

转换到预期的类型，就是每当编译器看见一个X而它需要一个Y的时候，他就会查找一个能将X转换为Y的隐式转换。通常一个浮点型不能被用作整数，因为这样会丢失精度：

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

隐式转换还能应用于方法调用的接收端，也就是被调用的那个对象。这种隐式转换有两个主要用途。首先，接收端转换允许我们更平滑将心累集成到已有的类继承关系图谱中，其次它们支持在语言中编写（原生的）领域特定语言（DSL）。

如果我们写下obj.doIt，但是obj并没有doIt成员，编译器会在放弃之前尝试插入转换。这个转换需要应用到接收端，也就是obj，编译器会装作obj的预期“类型”为拥有名为doIt的成员，这个拥有名为doIt的成员类型并不是一个普通的Scala类型，从概念上讲它是不存在的，这就是为什么编译器选择在这种情况下插入一个隐式转换。

#### 21.4.1 与新类型互操作

接收端转换的一个主要用途就是让心类型和已有类型的继承更加顺滑。尤其是这些转换使得我们可以让使用方程序员像使用新类型那样使用已有的类型，以Rational为例：

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

这个“富包装类”模式再给变成怨言提供类语法(syntax-like)的扩展的类库中十分常见。只要你看见有人调用了接收类中不存在的方法，那么很可能使用了隐式转换。

#### 21.4.3 隐式类

Scala 2.10引入了隐式类来建华富包装类的编写。隐式类以implicit开头，对于这样的类，编译器会生成一个从类的构造方法参数到类本身的隐式转换。如果打算用这个类来实现富包装类模式，这个转换真是你想要的。

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

注意：`implicit不能用在样例类(case class)上，并且其构造方法必须有且仅有一个参数`。不仅如此，隐式类必须存在与另一个对象、类和特质里。在实际应用中，只要是隐式类作为富包装类来给某个已有的类添加方法，这些限制应该都不是问题。

### 21.5 隐式参数

编译器有时将someCall(a)替换为someCall(a)(b)，或者将new Some(a)替换为new Some(a)(b)，通过追加一个参数列表的方式来完成某个函数调用。隐式参数提供的是整个最后一组柯里化的参数列表，而不仅仅是最后一个参数。如果someCall缺失的最后一个参数列表接收三个参数，那么编译器会将someCall(a)替换成someCall(a)(b, c, d)，就这个用法而言，b,c,d三个参数还必须定义为implicit。

假如有一个PreferredPrompt类，封装了一个用户偏好的命令行提示字符串($或者>)：

```scala
class PreferredPrompt(val preference: String)
```

同时，假如有一个带有greet的Greeter对象，greet接收两个参数列表，第一个参数列表接收一个字符创作为用户名，而第二个参数列表接收一个PreferredPrompt：

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

这个定义更加通用，但是也比较麻烦，现在调用者必须显式地给出排序，哪怕是当T为Int或者String这样有明确的默认排序的时候。为了让新的方法哼方便使用，可以将第二个参数标记为隐式：

```scala
def maxListImpParm[T](elements: List[T])(implicit ordering: Ordering[T]): T = elements match{
  case List() => throw new IllegalArgumentException("empty list!")
  case List(x) => x
  case x :: rest =>
    val maxRest = maxListImpParm(rest)(ordering)
    if (ordering.gt(x, maxRest)) x else maxRest
}
```

maxListImpParm函数是一个隐式参数用来提供关于在更靠前的参数列表中已经显式提到的类型的更多信息的例子。确切地说，类型为Ordering[T]的隐式参数ordering提供了更多关于类型T的信息。类型T在elements参数的类型List[T]中提到过，这是一个更靠前的参数列表中的参数。由于在任何maxListImpParm调用中elements都必须显式地给出，编译器在编译时就知道T是什么，因此就可以确定类型为Ordering[T]的隐式定义是否可用。

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

不过对于使用方而言，这个版本的函数需要提供类型为(T, T) => Boolean的参数orderer，这是一个相当泛化的类型。涵盖了所有从两个T到Boolean的函数。

### 21.6 上下文界定

使用implicit时，编译器不仅会尝试给这个参数`提供`一个隐式值，还会吧这个参数当做一个可以在方法体中`使用`的隐式定义！也就是说，可以省去方法体中对ordering的第一次使用。

```scala
def maxList[T](elements: List[T])(implicit ordering: Ordering[T]): T = elements match{
  case List() => throw new IllegalArgumentException("empty list!")
  case List(x) => x
  case x :: rest =>
    val maxRest = maxList(rest) //这里会自动添加(ordering)
    if (ordering.gt(x, maxRest)) x else maxRest //依然显式给出ordering
}
```

还有一种方法可以去掉对ordering的第二次使用，者设计标准库中定义的如下方法：

```scala
def implicitly[T](implicit t: T) = t
```

调用implicitly[Foo]的作用是编译器查找一个类型为Foo的隐式定义，然后会用这个对象来调用implicitly方法。这个方法再将这个对象返回。这样就可以在想要当前作用域找到类型为Foo的隐式对象时直接写implicitly[Foo]。下面展示了implicitly[Ordering[T]]来通过其类型获取ordering参数的用法。

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

Scala 2.8以后规定，如果可用的转换当中有某个转换严格来说比其他更具体，那么编译器就会选择哪个更具体的转换。比如一个接收String，另外一个接收Any，由于String是Any的子类，且比Any更具体，编译器会选择String的版本。

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
                   ┌───────────────────┐
                   │                   │
          ┌────────────────┐ ┌─────────────────┐
          │     scala      │ │    scala        │
          │      ::T       │ │     Nil         │
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