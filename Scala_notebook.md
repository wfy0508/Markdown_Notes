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

```
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

一个仅因其副作用而执行的方法称为过程(procedure)。类CheckSumAccumulator中的add方法，就是需要其执行之后的产生副作用。

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
