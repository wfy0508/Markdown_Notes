# Scala编程

## 一种可伸缩语言

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

|基本类型|取值范围|
|--|--|
|Byte|8bit ($-2^{7}, 2^{7}-1$)|
|Short|16bit ($-2^{15}, 2^{15}-1$)|
|Int|32bit ($-2^{31}, 2^{31}-1$)|
|Long|64bit ($-2^{63}, 2^{63}-1$)|
|Char|16bit ($0, 2^{16}-1$)|
|String|字符序列|
|Float|32bit 单精度|
|Double|64bit 双精度|
|Boolean|true & false|

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

|字面量|含义
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
|\||
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

 **`=>`** 代表将左边的东西的转换为右边的东西的符号，这个函数将任何整型变量由`x`转换为`x+1`。


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