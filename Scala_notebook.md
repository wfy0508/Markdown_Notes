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
val oneTwoThree = 1:: oneTwo
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
                   ┌----------------┐
                   |scala.collection|
                   |      Set       |
                   |   <<trait>>    |
                   └----------------┘
                            ▲
                            |
               ┌------------------------┐
               |                        |
┌--------------------------┐ ┌-------------------------┐
|scala.collection.immutable| |scala.collection.mutable |
|         HashSet          | |         HashSet         |
|        <<trait>>         | |        <<trait>>        |
└--------------------------┘ └-------------------------┘
             ▲                           ▲
             |                           |
┌--------------------------┐ ┌-------------------------┐
|scala.collection.immutable| |scala.collection.mutable||
|         HashSet          | |         HashSet         |
└--------------------------┘ └-------------------------┘
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

这将编译源文件，但是在编译完成之前可能会有明显的延迟。原因是每次编译器启动时，它都要花时间扫描jar文件的内容，甚至在查看您提交给它的新源文件之前就进行其他初始工作。出于这个原因，Scala发行版还包括一个名为fsc的Scala编译器守护进程(用于快速Scala编译器)。

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
| -------- | --------------------------- |
| Byte     | 8bit ($-2^{7}, 2^{7}-1$)    |
| Short    | 16bit ($-2^{15}, 2^{15}-1$) |
| Int      | 32bit ($-2^{31}, 2^{31}-1$) |
| Long     | 64bit ($-2^{63}, 2^{63}-1$) |
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

您可以将任何表达式放在经过处理的字符串文字中的美元符号($)之后，Scala将把第一个非标识符字符之前的所有字符解释为表达式

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

| 操作符优先级       |
| ------------------ |
| （所有其他操作符） |
| * / %              |
| + -                |
| :                  |
| = !                |
| < >                |
| &                  |
| ^                  |
| \|                 |
| (所有字母)         |
| (所有赋值操作符)   |

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

scala>
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

以这种方式使用下划线时，您是在编写一个部分应用函数。在Scala中，当调用一个函数，传入任何需要的参数时，将该函数应用于传入的参数上。

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

另一种考虑这种表达式的方法是，用下划线来表示整个参数列表，这是一种将def转换为函数值的方法。例如，如果您有一个局部函数，如sum(a: Int, b: Int, c: Int): Int，您可以将它“包装”在一个函数值中，该函数值的apply方法具有相同的参数列表和结果类型。当你把这个函数值应用到一些参数上时，会将sumAbc应用到参数列表中，然后返回函数的返回值。

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

当您将这些闭包应用于参数(在本例中，只有一个参数x，必须传入)时，返回的结果取决于创建闭包时如何定义更多的参数:

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

Scala允许您为函数参数指定默认值。在函数调用中可以选择省略此类参数的参数，在这种情况下，相应的参数将使用默认值填充。

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

这一次，您只看到`bang`的一个堆栈帧。您可能认为`bang`在调用自己之前就崩溃了，但事实并非如此。如果您认为在查看堆栈跟踪时可能会被尾部调用优化弄糊涂，那么您可以通过向`scala shell`或`scalac`编译器提供以下参数来关闭它们。

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
 ┌--------------------┐
 |       scala        |
 |       AnyRef       |
 |<<java.lang.Object>>|
 └--------------------┘
           ▲
           |
           |
┌----------------------┐
|       Element        |
|     <<abstract>>     |
└----------------------┘
           ▲
           |
           |
┌----------------------┐    ┌---------------┐
|     ArrayElement     |<>--| Array[String] |
└----------------------┘    └---------------┘
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
 ┌--------------------┐
 |       AnyRef       |
 |<<java.lang.Object>>|
 └--------------------┘
           ▲
           |
┌----------------------┐
|       Element        |
|     <<abstract>>     |
└----------------------┘
           ▲
           |
┌----------------------┐    ┌---------------┐
|     ArrayElement     |<>--| Array[String] |
└----------------------┘    └---------------┘
           ▲
           |
┌----------------------┐
|     LineElement      |
└----------------------┘
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
          ┌----------------------┐
          |       Element        |
          |     <<abstract>>     |
          └----------------------┘
                      ▲
                      |
             ┌----------------┐
             |                |
┌------------------┐    ┌----------------┐
|   ArrayElement   |    | UniformElement |
└------------------┘    └----------------┘
           ▲
           |
┌----------------------┐
|     LineElement      |
└----------------------┘
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
            ┌----------------------┐
            | Element <<abstract>> |
            └----------------------┘
                       ▲
                       |
      ┌--------------------------------┐
      |                |               |
┌------------┐ ┌---------------┐ ┌------------┐
|ArrayElement| |UniformElement | |LineElement|
└------------┘ └---------------┘ └------------┘
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
Animal --> AnyRef --> Any
```

第2个是混入的特质Furry：

```plaintxt
Furry --> Animal --> AnyRef --> Any
```

第3个是混入的特质Fourlegged：

```plaintxt
Fourlegged --> HasLegs --> Furry --> Animal --> AnyRef --> Any
```

最后线性化的是Cat自己：

```plaintxt
Cat --> Fourlegged --> HasLegs --> Furry --> Animal --> AnyRef --> Any
```

所以Cat类的继承关系和实例化如下

|类型|线性化|
|----|----|
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

Scala提供了一个名为`_root_`的包，它位于用户可以编写的任何包之外。换句话说，您可以编写的每个**顶级包**都被视为`_root_`包的成员。例如，上例中的`launch和bobsrockets都是package的成员_root_`。结果是_root_.launch提供了顶级lanuch包和_root_.launch。Booster3指定了最外层的booster类。

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

`showFruit`中`fruit`是`Fruit`类型，直接���������`fruit`的全部成员，可以直接访问`name`和`color`，与`fruit.name`和`fruit.color`代表的含义相同。

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

有访问权限的修饰符为您提供了非常细粒度的控制。

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

到目前为止，看到添加到包中的代码是类、特质和单例对象。这些是放在包的顶层的最常见的定义。但是Scala并没有限制你只是这样做，在类中的任何类型的定义都可以放在包的顶层。如果有一些帮助器方法，您想要在整个包的范围内，那么就将它放在包的顶层。

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

在这里，编译器甚至不允许您添加默认情况。由于pi是一个变量，所以它将匹配所有输入，因此不可能找到它后面的情况（unreachable）。

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

序列类型允许您处理按顺序排列的数据组。因为元素是有序的，你可以按下标访问第1个元素，第1个元素，103号元素，以此类推。

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
|nums - 3|移除元素3，返回Set(1, 2)|
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
```

#### 17.2.3 默认的集和映射

#### 17.2.4 排序号的集和映射

### 17.3 在可变和不可变集合类之间选择

### 17.4 初始化集合

#### 17.4.1 转换成数组或列表

#### 17.4.2 在可变与不可变集及映射间转换

### 17.5 元组	