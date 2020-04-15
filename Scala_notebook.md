# Scala和Spark学习笔记

## Scala编程

### 一种可伸缩语言

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

### 2 Scala入门

#### 2.1 定义变量

Scala的变量分为两种：var和val，val跟Java的final变量类似，初始化后就不能被重新赋值；而var则不同，类似于Java的非final变量，在整个生命周期之内都可以被重新赋值。

```scala
val msg = "Hello World!"
msg: String = Hello World!
```

定义变量不用指定类型，Scala中的类型推断 (type inference) 能够推断出那些不显式执行的类型。

#### 2.2 定义函数

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

#### 2.3 编写Scala脚本

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

#### 2.4 while循环，if判断

```scala
var i = 0
while (i < args.length) { //while或if中的boolean表达式必须在圆括号内
    println(args(i))
    i += 1 //注意Java中的++i和i++在Scala中不工作
}
```

#### 2.5 用foreach和for遍历

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

### 3 Scala入门（续）

#### 3.1 用类型参数化数组

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

#### 3.3 使用列表

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

#### 3.4 使用元组

另一个容易对象是元组（tuple），元组也是不可变的，跟List不同之处在于，元组中的元素类型可以不同。

```scala
val pair = (99, "Hello")
println(pair._1) //访问字段_1，产出第1个元素。
println(pair._2) //访问字段_2，产出第2个元素。
```

Scala会推断出pair的类型为`Tuple2[Int, String]`，这里的“.”跟用于访问字段或调用方法时使用的方式相同。`元组的实际类型取决于包含的元素以及元素的类型`。pair的类型为Tuple2[Int, String]，那么元组('u', 't', 'the', 1, 4)的类型为Tuple5[Char, Char, String, Int, Int]。

我们为什么不能使用pair(0)来访问元素？这是因为列表的apply方法永远只返回同一种类型，但元组中的类型可以不同，_1可以是一种类型，_2可能是另一种类型，_N表示的字段名从1开始而不是0。

#### 3.5 使用集合和映射

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

#### 3.6 认识函数式编程

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

#### 3.7 从文件读取记录行

```scala
import scala.io.Source

if (args.length > 0){
    for (line <- Source.fromFile(args).getLines())
      println(line.length + " " + line)
}
else
  Console.err.println("Please enter fileName")
```

### 4 类和对象

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

#### 4.1 定义私有变量

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

#### 4.2 分号推断

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

#### 4.4 Scala应用

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

#### 4.5 应用特质

Scala提供了一种trait，可以节省部分代码的书写，如下

```scala
import CheckSumAccmumlator.calculate

object FallWinterSpringSummer extends App{
    for (season <- List("fall", "winter", "spring"))
    println(season + " " + calculate(season))
}
```

使用extends App可以替代main方法，你把原来放入main方法中的代码，放置其中即可。

```scala
class Rational(n: Int, d: Int){
  require(d != 0)
  private val g = gcd(n.abs, d.abs)
  val numer = n / g
  val denom
  def this(n: Int) = this(n,1)

  def + (that: Rational): Rational =
    new Rational(numer * that.denom + denom * that.numer, denom * that.denom)
  
  def + (i: Int): Rational = new Rational(numer + i * denom, denom)

  def - (that: Rational): Rational =
   new Rational(numer * that.denom - denom * that.numer, denom * that.denom)

  def - (i: Int): Rational = new Rational(numer - i * denom, denom)

  def * (that: Rational): Rational =
   new Rational(numer * that.numer, denom * that.denom)

  def *(i: Int): Rational = new Rational(numer * i, denom)

  def / (that: Rational): Rational =
    new Rational(numer * that.denom, denom * that.numer)

  def /(i: Int): Rational = new Rational(numer, denom * i)

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

```scala
val line = ""
do{
 line = readLine()
 println("Read: " + line)
} while(line != "")
```

```scala
val filesHere = (new java.io.File(".").listFiles

def fileLines(file: java.io.File) scala.io.Source.fromFile(file).getLines().toList

def grep(pattern: String) =
  for (
  file <- filesHere
     if file.getName.endsWith(",scala")
       line <- fileLines(file)
     if line.trim.matchs(pattern)
 ) println(file + ": " + line.trim)
grep(".*gcd.*")
```

```scala
def forLineLengths =
for {
    file <- filesHere
    if file.getName.endsWith(".scala")
    line <- fileLines(file)
    trimmed = line.trim
    if trimmed.matchs(".*for*.")
} yield trimmed.length
```

```scala
val firstArg = if (!args.isEmpty) args(0) else ""

val friend = 
 firstArg match{
     case "salt" => "pepper"
     case "chips" => "salsa"
     case "eggs" => "bacon"
     case _ => "huh?"
 }
```

```scala
var i = 0
val foundIt = false
while (i < args.length && !foundIt) {
    if(!args.startsWith("-")) { 
        if (args(i).endsWith(".scala")) foundIt = true
    }
    i = i + 1
 }
```

```scala
def searchFrom(i: Int): Int =
if (i >= args.length) -1
else if (args(i).startsWith("-")) searchFrom(i + 1)
else if (args(i).endsWith(".scala")) i
else searchFrom(i + 1)
val i = searchFrom(0)
```

```scala
def printMutable() = {
    var i = 1
    while(i <= 10) {
        var j = 1
        while (j <= 10) {
            val prod = (i * j).toString
            var k = prod.length
            while (k < 4) {
                print(" ")
                k += 1
            }
            print(prod)
            j += 1
        }
        println()
        i += 1
    }
}
```

```scala
def makeRowSeq(row: Int) = {
 for (col <- 1 to 10) yield {
  val prod = (row * col).toString
  val padding = " " * (4 - prod.length)
  padding + prod
 }
}

def makeRow(row: Int) = makeRowSeq(row).mkString

def multiTable() = {
 val tableSeq = for (row <- 1 to 10) yield makeRow(row)
 tableSeq.mkString("\n")
}

def makeRoeSeq(row: Int) =
  for (col <- 1 to 10)
  val prod = (col * row).toString
  val padding = " " * (4 - prod.length)
  padding + prod

def makeRow(row: Int) = makeRoeSeq(row).mkString

def multiTable(row: Int) =
  val tableSeq = for (row <- 1 to 10) yield makeRow(row)
  tableSeq.mkString("\n")
```

```scala
import scala.io.Source

object Longlines{
    def processFile(filename: String, width: Int) = {
        val source = Source.fromFile(filename)
        for (line <- source.getLines())
          processLine(filename, width, line)
    }

    def processLine(filename: String, width: Int, line: String) = {
        if(line.length > width) 
        println(filename + ": " + line.trim)
    }
}

object FindLongLines extends App{
    val width = args(0).toInt
    for (arg <- args.drop(1))
      Longlines.processFile(arg, width)
}

def approximate(gauss: Double): Double = {
    if (isGoodEnough(gauss)) gauss 
    else approximate(improve(gauss))
}

def approximateLoop(initialGauss: Double): Double = {
    var gauss = initialGauss
    while(!isGoodEnough(gauss))
    gauss = improve(gauss)
    gauss
}

object FileMatcher{
    private def filesHere = (new java.io.File(".")).listFiles

    def filesEnding(query: String) = 
      for (file <- filesHere
           if file.getName.endsWith(query))
    yield file
}

def filesMatching(query: String, matcher: (String, String) => Boolean) = {
 for (file <- filesHere; if matcher(file.getName, query))
 yield file
}

def filesEnding(query, String) = filesMatching(query, _.endsWith(_))

def filesContaining(query, String) = filesMatching(query, _.contains(_))

def filesRegex(query, String) = filesMatching(query, _.matchs(_))
```

文件匹配最终优化版

```scala
object FileMatcher{
    private def filesHere = (new java.io.File(".")).listFiles
    private def filesMatching (matcher: String => Boolean) =

    for (file <- filesHere; if matcher(file.getName)) 
      yield file

    def filesEnding(query: String) = filesMatching(_.endsWith(query))

    def filesContaining(query: String) = filesMatching(_.contains(query))

    def filesRegex(query: String) = filesMatching(_.matchs(query))
}

import java.io.File
import java.io.PrintWriter

def withPrintWriter(file: File, op: PrintWriter => Unit) = {
    val writer = new PrintWriter(file)
    try op(writer)
    finally writer.close()
}
```

将两个参数分开

```scala
def withPrintWriter(file: File)(op: PrintWriter => Unit) = {
    val writer = new PrintWriter(file)
    try op(writer)
    finally writer.close()
}
```

调用上述方法

```scala
val file = new File("data.txt")

withPrintWriter(file){
    writer.println(new java.util.Date)
}
```
