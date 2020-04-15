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

使用Map：

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
## Spark高级API
####  DataFrame

```scala
val flightData2015 = spark
 .read
 .option("inferSchema", "true")
 .option("header", "true")
 .csv("file:///usr/local/spark/testCode/2015-summary.csv")
```



```scala
val sqlWay = spark.sql("""
select dest_country_name, count(1)
from flight_data_2015
group by dest_country_name
""")
```



```scala
val dataFrameWay =flightData2015
 .groupBy('DEST_COUNTRY_NAME)
 .count()
```



```scala
val maxSql = spark.sql("""
 select dest_country_name,sum(count)
 from flight_data_2015
 group by dest_country_name
 order by sum(count) desc
 limit 5
""")
```



```scala
flightData2015
 .groupBy("DEST_COUNTRY_NAME")
 .sum("count")
 .withColumnRenamed("sum(count)", "destination_total")
 .sort(desc("destination_total"))
 .limit(5)
 .show()
```



```scala
flightData2015
 .groupBy("DEST_COUNTRY_NAME")
 .sum("count")
 .withColumnRenamed("sum(count)", "destination_total")
 .sort(desc("destination_total"))
 .limit(5)
 .explain()
```





```scala
flightData2015
 .groupBy("DEST_COUNTRY_NAME")
 .sum("count")
 .withColumnRenamed("sum(count)", "destination_total")
 .sort(desc("destination_total"))
 .limit(5)
 .show()
```



#### DataSet

```scala
case class ValueAndDouble(value: Long, valueDoubled: Long)

spark.range(2000)
 .map(value => ValueAndDouble(value, value * 2))
 .filter(vAndD => vAndD.valueDoubled % 2 == 0)
 .where("value % 3 = 0")
 .show()
```





```scala
val DF1 = spark.read.format("CSV")
 .option("inferSchema", "true")
 .option("header", "true")
 .csv("file:///usr/local/spark/testCode/2015-summary.csv")
```



```scala
val staticDataFrame = spark.read.format("csv")
 .option("header", "true")
 .option("inferSchema", "true")
 .load("file:///usr/local/spark/testCode/by-day/2010*.csv")
```



```scala
import org.apache.spark.sql.functions.{window, column, desc, col}

staticDataFrame
 .selectExpr("CustomerId", "UnitPrice * Quantity as total_cost", "InvoiceDate")
 .groupBy(col("CustomerId"), window(col("InvoiceDate"), "1 day"))
 .sum("total_cost")
 .orderBy(desc("sum(total_cost)"))
 .take(5)

staticSchema = staticDataFrame.schema
```



```scala
val streamingDataFrame = spark.readStream
 .schema(staticSchema)
 .option("maxFilesPerTrigger", 1)
 .format("csv")
 .option("header", "true")
 .load("file:///usr/local/spark/testCode/by-day/2010*.csv")
```



```scala
val purchaseByCustomerPerHour = streamingDataFrame
 .selectExpr("CustomerID", "UnitPrice * Quantity as total_cost", "InvoiceDate")
 .groupBy($"CustomerId", window($"InvoiceDate", "1 day"))
 .sum("total_cost")
```



```scala
purchaseByCustomerPerHour.writeStream
 .format("memory") // store in-memory table
 .queryName("customer_purchase") // in-memory table name
 .outputMode("complete") //all the counts should be in the table
 .start()
```



#### MLlib

```scala
import org.apache.spark.functions.date_format

val preppedDataFrame = staticDataFrame
 .na.fill(0)
 .withColumn("day_of_week", date_format($"InvoiceDate", "EEEE"))
 .coalesce(5)

val trainDataFrame = preppedDataFrame
 .where("InvoiceDate < '2011-07-01'")

val testDataFrame = preppedDataFrame
 .where("InvoiceDate >= '2011-07-01'")
```

```scala
import org.apache.spark.ml.feature.StringIndexer

val indexer = new StringIndexer()
 .setInputCol("day_of_week")
 .setOutputCol("day_of_week_index")
```

```scala
import org.apache.spark.ml.feature.OneHotEncoder

val encoder = new OneHotEncoder()
 .setInputCol("day_of_week")
 .setOutputCol("day_of_week_encoded")

val df = spark.read.format("json").load("file:///usr/local/spark/testCode/2015-summary.json")
```

```scala
import org.apache.spark.sql.types.{StructField, StructType, StringType, LongType}
import org.apache.spark.sql.types.Metadata

val myManualSchema = StructType(
 Array(StructField("DEST_COUNTRY_NAME", StringType, true),
     StructField("ORIGIN_COUNTRY_NAME", StringType, true),
     StructField("count", LongType, false,
     Metadata.fromJson("{\"hello\":\"world\"}"))
    )
)
```

```scala
val df = spark.read.format("json").schema(myManualSchema)
 .load("file:///usr/local/spark/testCode/2015-summary.json")
```



#### Row

```scala
import org.apache.spark.sql.Row

val myRow = Row("Hello", null, 1, false)
//创建DataFrame
val df = spark.read.format("json").load("file:///usr/local/spark/testCode/2015-summary.json")
df.createOrRepalceTempView("dfTable")
```

```scala
import org.apache.spark.sql.types.{StructField, StructType, StringType, LongType}

val myManualSchema = new StructType(
 Array(new StructField("some", StringType, true),
     new StructField("col", StringType, true),
     new StructField("names", LongType, false)
    )
)

val myRows = Seq(Row("Hello", null, 1L))
val myRDD = spark.sparkContext.parallelize(myRows)
val myDf = spark.createDataFrame(myRDD, myManualSchema)
myDf.show()

//查询DataFrame
df.select("DEST_COUNTRY_NAME", "ORIGIN_COUNTRY_NAME").show(2)
```

```scala
import org.apache.spark.sql.functions.{expr, col, column}

//下面的引用有相同的效果
df.select(
    df.col("DEST_COUNTRY_NAME"),
    col("DEST_COUNTRY_NAME"),
    'DEST_COUNTRY_NAME,
    column("DEST_COUNTRY_NAME"),
    $"DEST_COUNTRY_NAME",
    expr("DEST_COUNTRY_NAME")
).show(2)

df.select(col("DEST_COUNTRY_NAME"),"DEST_COUNTRY_NAME") //第二个变量参数会被识别为String,而不是字段
df.select(expr("DEST_COUNTRY_NAME" as dest)).show(5)
df.select(expr("DEST_COUNTRY_NAME as dest").alias("DEST_COUNTRY_NAME")).show(5)

//更为便捷的方法
df.selectExpr("DEST_COUNTRY_NAME as dest", "ORIGIN_COUNTRY_NAME").show(2)
df.selectExpr("*", "DEST_COUNTRY_NAME = ORIGIN_COUNTRY_NAME as withInCountry").show(2)
```

使用聚合函数

```java
df.selectExpr("avg(count)", "count(distinct(DEST_COUNTRY_NAME))").show(2)
```

使用字面量

```scala
import org.apache.spark.sql.functions.lit

df.select(expr("*"), lit(1).as(One)).show(2)
```

添加列

```scala
df.withColumn("numberOne", lit(2)).show()
df.withColumn("withInCountry", expr("dest_country_name == origin_country_name")).show(2)
```

重命名字段

```scala
df.withColumnRenamed("dest_country_name", "dest").columns
```

保留字和关键字

```scala
val dfWithLongColName = df.withColumn("This Long Column-Name", expr("origin_country_name"))

df.selectExpr("`This Long Column-Name`", "`This Long Column-Name` as `new col`").show(2)
```

删除列

```scala
df.drop("dest_country_name").columns
```

更改列的类型

```scala
df.withColumn("count2", col("count").cast("long"))
```

过滤行

```scala
df.where(col("count") < 2).show(2)
df.filter(col("count") < 2).show(2)
df.where(col("count") < 2).where(col("dest_country_name") =!= "Croatia").show(2) 
```

获得去重后的行

```scala
df.select("dest_country_name", "origin_country_name").distinct().count()
```

随机抽样

```scala
val seed = 5
val withReplacement = false
val fraction = 0.5
df.sample(withReplacement, fraction, seed).count()
```

随机分割

```scala
val dataFrames = df.randomSplit(Array(0.25, 0.75), seed)
```

连接和追加行

```scala
val schema = df.schema
val newRows = Seq(
 Row("newCountry", "otherCountry", 5L),
 Row("newCountry 2", "otherCountry 2", 1L)
)

val parallelizedRows = spark.sparkContext.parallelize(newRows)
val newDF = spark.createDataFrame(parallelizedRows, schema)

df.union(newDF)
 .where("count = 1")
 .where($"origin_country_name" =!= "Croatia")
 .show()
```


`不相等使用=!=,相等使用===`

```scala
df.union(newDF) //where条件会同时银应用于df和newDF
 .where("count = 1")
 .where("dest_country_name" === "Singapore")
 .show()

//排序
df.sort().show(5)
df.orderBy("count", "DEST_COUNTRY_NAME").show(5)
```



```scala
import org.apache.spark.sql.functions.{asc, desc}



df.orderBy(expr("count desc")).show(5)

df.orderBy(desc("count"), asc("dest_country_name")).show(5)



//limit

df.limit(5).show()



//获取分区数

df.rdd.getNumPartitions

//重分区

df.repartition(col("dest_country_name"))

df.repartition(5, col("dest_country_name"))



//驱动器获取行

val collectDF = df.limit(10) 

collectDF.collect() //collect比较消耗资源

collectDF.toLocalIterator() //使用toLocalIterator替代，进行串行读取
```

#### 处理不同的数据类型

```scala
val df = spark.read.format("csv")
 .option("header", "true")
 .option("inferSchema", "true")
 .load("file:///usr/local/spark/testCode/2010-12-01.csv")

df.createOrRepalceTempView("dfTable")

//转换为Spark类型
df.select(lit("five"), lit(5), lit(5.0)) //使用lit将原始类型转化为Spark类型

//处理布尔类型
df.where(col("InvoiceNo").equleTo(536365))
 .select("InvoiceNo", "Description")
 .show(2, false)

//字符串形式的谓词表达式(更简洁)
df.where("Invoice = 536365").show(2, false)

//使用and or
val priceFilter=col("UnitPrice") > 600
val descriptionFilter = col("Description").contains("POSTAGE")
df.where(col("StockCode").isin("DOT")).where(priceFilter.or(descriptionFilter))

//过滤器不一定非要使用 Boolean 表达式，要过滤 DataFrame，也可以删掉指定一个 Boolean 布尔类型的列
val DOTCodeFilter = col("StockCode") === "DOT"
val priceFilter = col("UnitPrice") > 600
val descriptionFilter = col("Description").contains("POSTAGE")

df.withColumn("isExpensive", DOTCodeFilter.and(priceFilter.or(descriptionFilter)))
 .where("isExpensive")
 .select("UnitPrice", "isExpensive")
 .show(5)
```

##### 处理数值类型

```scala
import org.apache.spark.sql.functions.{expr, pow}

val fabricateQuantity = pow(col("Quantity") * col("UnitPrice"), 2) + 5
df.select(expr("CustomerId"), fabricateQuantity.alias("realQuantity")).show(2)
```

##### 四舍五入

```scala
val fabricateQuantity = round(pow(col("Quantity") * col("UnitPrice"), 2) + 5， 2) 
val fabricateQuantity = round(pow(col("Quantity") * col("UnitPrice"), 2) + 5) //向上取整
val fabricateQuantity = bround(pow(col("Quantity") * col("UnitPrice"), 2) + 5) //向下取整
```

##### 描述数据的分布

```scala
df.describe().show()
```

##### Pearson相关系数

```scala
import org.apache.spark.sql.functions.{corr}

df.stat.corr("Quantity", "UnitPrice")

df.select(corr("Quantity", "UnitPrice")).show()
```

##### 处理字符类型

```scala
import org.apache.spark.sql.functions.{InitCap} //每个单词首字母大写
df.select(col("description"), InitCap(col("Description"))).show(2, false)
```

```scala
import org.apache.spark.sql.functions.{lower, upper} //转大小写

df.select(col("description"), lower(col("Description"))).show(2, false)
df.select(col("description"), upper(col("Description"))).show(2, false)
```

##### 删除空格，添加空格

```scala
import org.apache.spark.sql.functions.{ltrim, rtrim, lpad, rpad, trim}

df.select(
 trim(lit(" HELLO ")).as("trim"),
 ltrim(lit(" HELLO ")).as("ltrim"),
 rtrim(lit(" HELLO ")).as("rtrim"),
 lpad(lit("HELLO"), 3, " ").as("lp"),
 rpad(lit("HELLO"), 10, " ").as("rp")
).show(2)
```

##### 正则表达式

```scala
import org.apache.spark.sql.functions.{regexp_replace, regexp_extract}

val simpleColors = Seq("white", "red", "green", "black", "blue")
val regexString = simpleColors.map(_.toUpperCase).mkString("|")

df.select(

 regexp_replace(
  col("Description"), 
  regexString, "COLOR").alias("color_clean"), 
  col("Description")
).show(2)
```

##### 替换指定的字符translate

```scala
import org.apache.spark.sql.functions.translate
df.select(translate(col("description"), "LEET", "1337"), col("Description")).show(2)
```

##### 取出第一个被提到的颜色

```scala
val regexString = simpleColors.map(_.toUpperCase).mkString("(","|",")")
df.select(regexp_extract(col("Description"), regexString, 1).alias("colorClean"), col("Description")).show(5)
```

##### 检查元素时候存在

```scala
val containBlack = col("Description").contains("BLACK")
val containWhite = col("Description").contains("WHITE")

df.withColumn("hasSimpleColor", containBlack.or(containWhite))
 .where("hasSimpleColor")
 .select("Description")
 .show(3, false)
```

##### 接受不定数量的参数

```scala
val selectedColumns = simpleColors.map(
 color => {col("description").contains(color.toUpperCase).alias(s"is_$color")}
):+expr("*")
```

```scala
df.select(selectedColumns:_*).where(col("is_white").or(col("is_red")))
 .select(col("description")).show(3, false)
```

##### 处理日期和时间类型

```scala
import org.apache.spark.sql.functions.{current_date, current_timestamp}

val dateDF = spark.range(10)
 .withColumn("today", current_date())
 .withColumn("now", current_timestamp())
 dateDF.createOrRepalceTempView("dateTable")
```

##### 增加或减去天数

```scala
import org.apache.spark.sql.functions.{date_add, date_sub}
dateDF.select(date_sub(col("today"), 5),date_add(col("now"), 5)).show(1)
```

##### 两个日期之间的时间间隔

```scala
import org.apache.spark.sql.functions.{month_between, to_date, datediff}

dateDF.withColumn("week_ago", date_sub(col("today"), 7)).select(datediff(col("week_ago"), col("today"))).show(1)

dateDF.select(
 to_date(lit("2016-01-01")).alias("start_date"),
 to_date(lit("2017-01-01")).alias("end_date")
)
.select(months_between(col("end_date"), col("start_date")).alias(months)).show(1)
```

`如果 Spark 无法解析日期，它不会抛出错误，而只是返回 null`

#### 处理数据中的空值

合并

```scala
import org.apache.spark.sql.functions.{coalesce}

df.select(coalesce(col("description"), col("customerId"))).show()

ifnull(null, 'return_value')
nullif('value', 'value')
nvl(null, 'return_value')
nvl2('not_null', 'return_value', "else_value") 
df.na.drop() 
df.na.drop("any")
```

指定列进行空值删除

```scala
df.na.drop("all"， Seq("StockCode"， "InvoiceNo"))
```

#### 处理复杂类型

结构struct

```scala
val complexDF = df.selectExpr("(Description, InvoiceNo) as complex", "*")
val complexDF = df.selectExpr("struct(Description, InvoiceNo) as complex", "*")
```

有一个包含 complex 列的 DataFrame，我们可以像查询另一个DataFrame一样查询它，唯一的区别是使用“.”来访问或列方法 getField 来实现

```scala
complexDF.select("complex.description")
complexDF.select(col("complex").getField("description"))
```

##### 数组-split

```scala
import org.apache.spark.sql.functions.split

df.select(split(col("description"), " ")).show(2)
df.select(split(col("description"), " ").alias("split_col")).selectExpr("split_col[0]").show(2) //引用split之后的数据元素

df.select(size(split(col("description"), " "))).show(2) //数组长度
df.select(array_contains(split(col("description"), " "), "WHITE")).show(2) //是否包含某元素

//explode 将包含数组的列中每个元素创建一行
import org.apache.spark.sql.functions.explode

df.withColumn("splitted", split(col("description"), " "))
 .withColumn("exploded", explode(col("splitted")))
 .select("description", "InvoiceNo", "exploded").show(10)
```

##### Map

```scala
import org.apache.spark.sql.functions.map

df.select(map(col("description"), col("InvoiceNo"))).alias("complex_map").show(2)

df.select(map(col("description"), col("InvoiceNo")).alias("complex_map"))

 .selectExpr("complex_map['WHITE METAL LANTERN']").show(2)
```

##### 处理JSON对象

```scala
val jsonDF = spark.range(10).selectExpr("""'{"myJSONKey":{"myJSONValue":[1, 2, 3]}}' as jsonString""")
```

无论是字典还是数组，均可以使用get_json_object直接查询JSON对象。如果此查询的JSON 对象仅有一层嵌套，则可使用json_tuple

```scala
import org.apache.spark.sql.functions.{get_json_object, json_tuple}

jsonDF.select(get_json_object(col("jsonString"), "$.myJSONKey.myJSONValue[1]") as "column", json_tuple(col("jsonString"), "myJSONKey")).show(2)
```

以使用to_json函数将StructType转换为JSON字符串

```scala
import org.apache.spark.sql.functions.to_json

df.selectExpr("(description, invoiceNo) as myStruct").select(to_json(col("myStruct"))).show(2)
df.selectExpr("(description, invoiceNo) as myStruct")
 .select(to_json(col("myStruct")).alias("myStructJson"))
 .select(json_tuple(col("myStructJson"),"description")).show(2)
```

使用from_json函数将JSON 数据解析出来

```scala
import org.apache.spark.sql.functions.from_json
import org.apache.spark.sql.types._

val praseSchema = new StructType(
 Array(
  new StructField("InvoiceNo", StringType, true),
  new StructField("Description", StringType, true)
 )
)

df.selectExpr("(InvoiceNo, Description) as MyStruct")
 .select(to_json(col("myStruct")).alias("newJson"))
 .select(from_json(col("newJson"), praseSchema), col("newJson"))
 .show(2)
```

#### 用户自定义函数（UDF）

```scala
val udfSimpleDF = spark.range(5).toDF("num")
def power3(number: Double): Double = number * number * number
power3(2.0)
```

用户自定义的函数需要注册后，才能在所有的机器上使用需要注册该函数，以便其可用作DataFrame函数

```scala
import org.apache.spark.sql.functions.udf
val power3df = udf(power3(_: Double): Double)

//就可以像其他函数一样使用
udfSimpleDF.select(col("num"), power3df(col("num")).alias("power3")).show()
```

####  聚合操作

```scala
val df = spark.read.format("csv")
 .option("header", true)
 .option("inferSchema", true)
 .load("file:///usr/local/spark/testCode/online-retail-dataset.csv")
 .coalesce(5)

df.cache()
df.createOrRepalceTempView("dfTable")

//count
import org.apache.spark.sql.functions.count
df.select(count("StockCode")).show()

//countDistinct
import org.apache.spark.sql.functions.countDistinct
df.select(countDistinct("StockCode")).show()

//approx_count_distinct
import org.apache.spark.sql.functions.approx_count_distinct
df.select(approx_count_distinct("StockCode", 0.1)).show() //0.1该参数指定可容忍的最大误差,比countDistinct节省执行耗时

//first last
import org.apache.spark.sql.functions.{first, last}
df.select(first("StockCode"), last("StockCode")).show()

//min max
import org.apache.spark.sql.functions.{min, max}
df.select(min("StockCode"), max("StockCode")).show()

//sum sumDistinct
import org.apache.spark.sql.functions.{sum, sumDistinct}
df.select(sum("StockCode"), sumDistinct("StockCode")).show()

//avg
import org.apache.spark.sql.functions.{avg}
df.select(avg("StockCode")).show()
```

方差是各数据样本与均值之间差的平方的平均值，标准差是方差的平方根。

Spark 既支持统计样本标准差也支持统计总体标准差，它们两个在统计学上是完全不同的概念，一定要区分它们。如果使用variance 函数和 stddev 函数，默认是计算样本标准差或样本方差的

```scala
import org.apache.spark.sql.functions.{var_pop, stddev_pop}
import org.apache.spark.sql.functions.{var_samp, stddev_samp}

df.select(var_pop("Quantity"), var_samp("Quantity"), stddev_pop("Quantity"), stddev_samp("Quantity")).show()
```

偏度系数(skewness)和峰度系数(kurtosis)都是对数据集中的极端数据点的衡量指标。偏度系数衡量数据相对于平均值的不对称程度，而峰度系数衡量数据分布形态陡缓程度。在将数据建模为随机变量的概率分布时，它们都很重要。

```scala
import org.apache.spark.sql.functions.{skewness, kurtosis} 

df.select(skewness("Quantity"), kurtosis("Quantity")).show()
```

cov和corr，它们分别用于计算协方差和相关性。相关性采用 Pearson 相关系数来衡量，范围是-1到+1。协方差的范围由数据中的输入决定。

##### 聚合输出复杂类型

在Spark中，不仅可以在数值型上执行聚合操作，还能在复杂类型上执行聚合操作。例如，可以收集某列上的值到一个list列表里，或者将unique唯一值收集到一个set集合里。

```scala
import org.apache.spark.sql.functions.{collect_set, collect_list}

df.agg(collect_set("Country"), collect_list("Country")).show()
```

###### 使用表达式分组

```scala
import org.apache.spark.sql.functions.count

df.groupBy("InvoiceNo").agg(count("Quantity").alias("quan"), expr("count(Quantity)")).show()
df.groupBy("InvoiceNo").agg(count("Quantity").alias("quan")).orderBy(desc("quan")).show()
```

##### 使用Map进行分组

```scala
df.groupBy("InvoiceNo").agg("Quantity" -> "avg", "Quantity" -> "stddev_pop").show()
```

##### Window函数

```scala
import org.apache.spark.sql.functions.{col, to_date}

val dfWithDate = df.withColumn("date", to_date("col("InvoiceNo")"))

import org.apache.spark.sql.expressions.Window

val windowSpec = Window
 .partitionBy("CustomerId", "date")
 .orderBy(col("Quantity").desc)
 .rowsBetween(Window.unboundedPreceding, Window.currentRow)
```

使用聚合函数来了解有关每个特定客户的更多信息

```scala
import org.apache.spark.sql.functions.max

val maxPurchaseQuantity = max(col("Quantity")).over(windowSpec)
```

排名

```scala
import org.apache.spark.sql.functions.{dense_rank, rank}

val purchaseDenseRank = dense_rank().over(windowSpec)
val purchaseRank = rank().over(windowSpec)
```

查看窗口中的值

```scala
dfWithDate
 .where("CustomerId is not null")
 .orderBy("CustomerId")
 .select(col("CustomerId"), 
      col("date"), 
      col("Quantity"),
      purchaseRank.alias("quantityRank"),
      purchaseDenseRank.alias("quantityDenseRank"),
      maxPurchaseQuantity.alias("maxPurchaseQuantity")
      )
 .show()
```

透视转换

```scala
val pivoted = dfWithDate.groupBy("date").pivot("Country").sum()

pivoted.where("date > '2011-12-05'").select("date", "`USA_sum(Quantity)`").show()
```

用户自定义聚合函数（UDAF）：用户定义的聚合函数（UDAF）是用户根据自定义公式或业务逻辑定义自己的聚合函数的一种方法。可以使用UDAF来计算输入数据组(与单行相对)的自定义计算。Spark维护单个AggregationBuffer，它用于存储每组输入数据的中间结果。若要创建UDAF，必须继承UserDefinedAggregateFunction基类并实现以下方法:

-  inputSchema 用户指定输入参数，输入的参数类型为StructType


-  bufferSchema 用户指定UADF的中间结果，中间结果类型为StructType


-  datatype 用户指定返回结果，返回结果的类型为DataType


-  deterministic 是一个布尔值，用户指定某个输入是否会返回相同的结果


-  initialize 初始化聚合缓冲区的初始值


-  update 描述如何根据给定行更新内部缓冲区


-  merge 描述应如何合并两个聚合缓冲区


-  evaluate 将聚合生成最终结果


```scala
import org.apache.spark.sql.expressions.MutableAggregationBuffer
import org.apache.spark.sql.expressions.UserDefinedAggregateFunction
import org.apache.spark.sql.Row
import org.apache.spark.sql.types._

class BoolAnd extends UserDefinedAggregateFunction {
    def inputSchema: org.apache.spark.sql.types.StructType =
        StructType(StructField("value", BooleanType) :: Nil)
    def bufferSchema: StructType = StructType(StructField("result", BooleanType) :: Nil)

    def dataType: DataType = BooleanType

    def deterministic: Boolean = true

    def initialize(buffer: MutableAggregationBuffer): Unit = {
        buffer(0) = true
    }

    def update(buffer: MutableAggregationBuffer, input: Row): Unit = {
        buffer(0) = buffer.getAs[Boolean](0) && input.getAs[Boolean](0)
    }

    def merge(buffer1: MutableAggregationBuffer, buffer2: Row): Unit = {
        buffer1(0) = buffer1.getAs[Boolean](0) && buffer2.getAs[Boolean](0)
    }
    
    def evaluate(buffer: Row): Any = {
        buffer(0)
    }
} 

val ba = new BoolAnd
spark.udf.register("booland", ba)
    
import org.apache.spark.sql.functions._

spark.range(1)
 .selectExpr("explode(array(TRUE, TRUE, TRUE)) as t")
 .selectExpr("explode(array(TRUE, FALSE, TRUE)) as f", "t")
 .select(ba(col("t")), expr("booland(f)"))
 .show()连接操作
```

```scala
val person = Seq(
    (0, "Bill Chambers", 0, Seq(100)),
    (1, "Matei Zaharia", 1, Seq(500, 250, 100)),
    (2, "Michael Armbrust", 1, Seq(250, 100))
).toDF("id", "name", "graduate_program", "spark_status")

val graduateProgram = Seq(
    (0, "Masters", "School of Information", "UC Berkeley"),
    (2, "Masters", "EECS", "UC Berkeley"),
    (1, "Ph.D.", "EECS", "UC Berkeley")
).toDF("id", "degree", "department", "school")

val sparkStatus = Seq(
    (500, "Vice President"),
    (250, "PMC Member"),
    (100, "Contributor")
).toDF("id", "status")
```

内连接

```scala
val joinExpression = person.col("graduate_program") === graduate_program.col("id")
person.join(graduateProgram, joinExpression).show()
//select a.*,b.*
//from person a
//inner join graduateProgram b on a.graduate_program = b.id;
```

指定连接类型:内连接

```scala
var joinType = "inner"
person.join(graduateProgram, joinExpression, joinType).show()
```

指定连接类型:外连接

```scala
var joinType = "outer"
person.join(graduateProgram, joinExpression, joinType).show()
```

左外连接:检查两个DataFrame或表中的键，并包括左侧DataFrame中的所有行以及右侧DataFrame中与左侧DataFrame有匹配项的行。如果在右侧DataFrame中没有对应的行，则 Spark 将插入null

```scala
var joinType = "left_outer"
graduateProgram.join(person, joinExpression, joinType).show()
```

右外连接:与左外连接相反

```scala
var joinType = "right_outer"
person.join(graduateProgram, joinExpression, joinType).show()
```

左半连接:与其他连接操作有点区别，它实际上并不包括右侧DataFrame中的任何值，它只是查看左侧DataFrame的值是否存在于右侧DataFrame里，如果存在则在连接结果中保留，即使左侧DataFrame中存在重复键，对应行也将保留在结果中。可以将左半连接（Left Semi Join）看作DataFrame上的过滤器，而不是常规连接函数.

```scala
var joinType = "left_semi"

graduateProgram.join(person, joinExpression, joinType).show()
```

左反连接:与左半连接相反

```scala
var joinType = "left_anti"
graduateProgram.join(person, joinExpression, joinType).show()
```

交叉连接（笛卡尔连接）

```scala
var joinType = "cross"
graduateProgram.join(person, joinExpression, joinType).show()
```

对复杂类型的连接操作

```scala
import org.apache.spark.sql.functions.expr
person.withColumnRenamed("id", "personId").join(sparkStatus, expr("array_contains(spark_status,id)")).show()
```

#### 数据源

数据读取：Spark数据读取使用DataFrameReader，通过SparkSession的read属性得到spark.read还需要指定以下几个值：

-  format


-  schema


-  read模式


-  一系列option选项


读取模式：默认 permissive ,如果遇到错误格式的记录，将所有字段设置为null并将所有的格式记录放在名为_corrupt_record字符串列中。

写数据的结构:

```scala
DataFrameWriter.format(...).option(...).partitionBy(...).bucketBy(...).orderBy(...).save
```

将使用此格式向所有数据源写入数据。format 是可选的，默认情况下Spark将使用Parquet格式，option仍用于配置写出数据的方法，PartitionBy，bucketBy和sortBy仅适用于基于文件的数据源，你可以使用这些方法来控制写出目标文件的具体结构。



##### 保存模式

-  append 追加


-  overwrite 覆盖写入


-  errorIfExists 如果目标路径已经存在数据或者文件，则抛出错误并返回写入操作失败


-  ignore 如果路径已存在数据或者文件，则不执行任何操作


读取CSV文件

```scala
spark.read.format("csv")
 .option("header", "true")
 .option("mode", "FAILFAST")
 .option("inferSchema", "true")
 .load("file:///path/*.csv")
```

自定义读取数据格式

```scala
import org.apache.spark.sql.types.{StructField, StructType, StringType, LongType}

val myManualSchema = new StructType(
 Array(
     new StructField("DEST_COUNTRY_NAME", StringType, true),
     new StructField("ORIGIN_COUNTRY_NAME", StringType, true),
     new StructField("count", LongType, false)
 )
)

spark.read.format("csv")
 .option("header", "true")
 .option("mode", "FAILFAST")
 .schema(myManualSchema)
 .load("file:///path/*.csv")
 .show(5)
```

写CSV文件

```scala
val csvFile = spark.read.format("csv")
 .opton("header", "true")
 .option("mode", "FAILFAST")
 .schema(myManualSchema)
 .load("file:///usr/local/spark/testCode/2010-summary.csv")

csvFile.write.format("csv").mode("overwrite").option("sep", ",").save("/tmp/my-tsv-file.csv")
```

读JSON文件

```scala
val jsonFile = spark.read.format("json")
 .option("mode", "FAILFAST")
 .schema(myManualSchema)
 .load("file:///usr/local/spark/testCode/2010-summary.json")
 .show(5)
```

写JSON文件

```scala
jsonFile.write.format("json").mode("overwrite").save("file:///usr/local/spark/testCode/test.json")
```

读取Parquet文件(Spark默认的文件格式)

```scala
spark.read.format("parquet")
```

写Parquet文件

```scala
csvFile.write.format("parquet").mode("overwrite").save("/tmp/my_parquet_file.parquet")
```

读Orc文件（与Parquet的区别是，Parquet针对Spark进行了优化， Orc针对Hive进行了优化）

```scala
spark.read.format("orc").load(...)
```

写Orc文件

```scala
csvFile.write.format("orc").mode("overwrite").save(...)
```

从SQL数据库中读取数据

```scala
val driver = "org.sqlite.JDBC" 
val path = "/data/flight-data/jdbc/my-sqlite.db" 
val url = s"jdbc:sqlite:/${path}" 
val tablename = "flight_info" 
```

为了能够读写Postgresql数据库，可能需要运行如下代码：

```scala
./bin/spark-shell --driver-class-path postgresql-9.4.1207.jar --jars postgresql-9.4.1207.jar
```

从数据库中读取数据，现制定格式再加载数据

```scala
val driver = "org.sqlite.JDBC"
val path = "/data/flight-data/jdbc/my-sqlite.db"
val url = s"jdbc:sqlite:/{path}"
val tablename = "flight_info"
```

测试连接

```scala
import org.apache.spark.DriverManager
val connection = DriverManager.getConnection(url)
connection.isClosed()
connection.close()
```

读取文本文件

```scala
spark.read.testFile("file:///usr/local/spark/testCode/2015-summary.csv")
 .selectExpr("split(value, ',') as row")
 .show(5)
```

写文本文件

```scala
csvFile.limit.select("DEST_COUNTRY_NAME", "count")
 .write.partitionBy("count")
 .text("file:///usr/local/spark/testCode/test-2015-summary.csv")
```

##### 高级IO

并行写入数据

```scala
csvFile.repartition(5).write.format("csv").save("file:///usr/local/spark/testCode/test-2015-summary.csv")
```

数据划分

```scala
csvFile.limit(10)
 .write.mode("overwrite")
 .partitionBy("DEST_COUNTRY_NAME")
 .save("file:///usr/local/spark/testCode/test-2015-summary.parquet")
```

#### 数据分桶

数据分桶是另一种文件组织方法，你可以使用该方法控制写入每个文件的数据。具有相同桶ID（哈希分桶的ID）的数据将放置到一个物理分区中，这样就可以避免在稍后读取数据时进行shuffle（洗牌）。根据你之后希望如何使用该数据来对数据进行预分区，就可以避免连接或聚合操作时执行代价很大的shuffle操作。

```scala
val numberBuckets = 10
val columnToBucketBy = "count"
csvFile.write.format("parquet").mode("overwrite")
 .bucketBy(numberBuckets, columnToBucketBy)
 .saveAsTable("file:///usr/local/spark/testCode/bucketedFiles")
```

#### 执行Spark-SQL查询

在Python或Scala中执行如下语句

```scala
spark.sql("select 1+1").show()

spark.sql("""SELECT user_id, 
             department, 
             first_name
         FROM professors 
         WHERE department in (SELECT name FROM department WHERE create_date >= '2016-01-01')
      """)
```

SQL与DataFrame之间可以实现互操作

##### 创建DataFrame

```scala
val some_sql_view = spark.read.format("json")
 .load("file:///usr/local/spark/testCode/2015-summary.json")
 .createOrRepalceTempView("some_sql_view")
```

```scala
spark.sql("select dest_country_name, sum(count) from some_sql_view group by dest_country_name").show()

spark.sql("select dest_country_name, sum(count) from some_sql_view group by dest_country_name")
 .where("dest_country_name like 'S%' ")
 .where("`sum(count)` > 10 ")
 .count()
```

##### 创建表

```sql
create table flights(
 DEST_COUNTRY_NAME string, 
 ORIGIN_COUNTRY_NAME string, 
 count LONG
) USING JSON OPTIONS (path 'file:///usr/local/spark/testCode/2015-summary.json')
```

执行此建表语句

```scala
spark.sql("""create table flights(
 DEST_COUNTRY_NAME string, 
 ORIGIN_COUNTRY_NAME string, 
 count LONG
) USING JSON OPTIONS (path 'file:///usr/local/spark/testCode/2015-summary.json')""")
```

还可以添加注释

```sql
create table flights(
 DEST_COUNTRY_NAME string, 
 ORIGIN_COUNTRY_NAME string comment "remember, the US will be most prevalent", 
 count LONG
) USING csv OPTIONS (header true 'file:///usr/local/spark/testCode/2015-summary.csv')
```

可以直接从查询结果创建表

```sql
create table flights_from_select using parquet as select * from flights
```

创建与Hive兼容的表

```sql
create table if not exists flights_from_select as select * from flights
```

分区数据

```sql
create table partitioned_flights using parquet partitioned by(dest_country_name) as 
select DEST_COUNTRY_NAME, ORIGIN_COUNTRY_NAME, count 
from flights limit 5 
```

#### 高级主题

##### 结构体（Struct）

括号中包含一组表达式

```sql
create table if not exists nested_data as
select (DEST_COUNTRY_NAME, ORIGIN_COUNTRY_NAME) as country ,count from flights

spark.sql("""create table if not exists nested_data as
         select (DEST_COUNTRY_NAME, ORIGIN_COUNTRY_NAME) as country ,count 
         from flights""")
```

列表 collect_list(可以包含重复值)， collect_set(不包含重复值)

```sql
select DEST_COUNTRY_NAME as new_name, 
     collect_list(count) as flights_count,
     collect_set(ORIGIN_COUNTRY_NAME) as origin_set
from flights
group by DEST_COUNTRY_NAME
```

可以通过设定值来创建一列

```scala
select dest_country_name, array(1,2,3) from flights --array(1,2,3)就是一个常量
```

或者通过类Python array的方式，按位置查询列表

```scala
select dest_country_name as new_name, collect_list(count)[0] from flights group by dest_country_name
```

##### 创建Dataset（Java编码器）

```java
import org.apache.spark.sql.Encoders;

public class Flight implements Serializable{
    String DEST_COUNTRY_NAME;
    String ORIGIN_COUNTRY_NAME;
    Long count;
}

Dataset<Flight> flights = spark.read
 .parquet("/data/flight-data/parquet/2010-summary.parquet/")
 .as(Encoders.bean(Flight.class))
```

##### Scala:Case类

Case类的特征：

- 1、不可变

- 2、通过模式匹配可分解，来获取类属性

- 3、允许基于结构的比较而不是基于引用的比较

- 4、易于使用和操作

这些特点对于数据分析很有用，因为case类很容易解析。而最重要的特性应该是case类的不变性，可以通过内容结构而不是引用进行比较。

**Scala文档描述**：不变性是你无需跟踪对象变化，时间和位置按值比较允许直接比较实例的值，就像他们是基本类型值（primitive value）一样，这样就避免了混淆类实例基于值比较或是基于引用比较所带来的的不确定性。模式匹配简化了分支逻辑，从而会引起更少的bug和带来可读性更好的代码

```scala
case class Flight(DEST_COUNTRY_NAME: String, ORIGIN_COUNTRY_NAME: String, count: BigInt)

//定义了case class，表示数据中的单个记录，简单的说，它使我们现在有个包含Flight类型的Dataset，只有模式没有方法
//当读取数据时，我们得到一个DataFrame，使用as方法将其强制转换为指定的行类型
val flightDF = spark.read.parquet("/data/flight-data/parquet/2010-summary.parquet/")
val flights = flightDF.as[Flight]
```

##### Action操作

```scala
flights.show()
```

当我们访问这些case class时，不需要任何强制类型转换，只需要指定case class的属性名并返回，返回包含预期值和预期类型

```scala
flight.first.DEST_COUNTRY_NAME
```

Transform操作：Dataset的转换操作和DataFrame操作是相同的。Dataset还允许我们指定比DataFrame转换更复杂的和强类型的转换操作，因为我们可以操作底层的Java虚拟机(JVM)类型。

过滤：

```scala
 def originIsDestination(flight_row: Flight): Boolean = {
     return flight_row.ORIGIN_COUNTRY_NAME == flight_row.DEST_COUNTRY_NAME
 }

 flights.filter(flight_row => originIsDestination(flight_row)).first()
```

##### Map

```scala
val destinations = flights.map(f => f.dest_country_name) //返回的为String类型的Dataset

 //可以在驱动器上collect
 val localDestinations = destinations.take(5)
```

##### Join

Dataset连接操作和DataFrame一样，但也提供了更复杂的方法joinWith。 joinWith大致等同于一个co-group(RDD术语)，将两个Dataset整合成一个，每一列都表示一个Dataset，并可以相应地进行操作当你需要在连接操作中维护更多信息，或执行一些更复杂的操作(如高级映射或筛选)时，这可能很有用。

```scala
case class FlightMetaData(count: BigInt, randomData: BigInt)
```

```scala
val flightMeta = spark.range(500).map(x => (x, scala.util.Random.nextLong))
 .withColumnRenamed("_1", "count").withColumnRenamed("_2", "randomData")
 .as[FlightMetaData]

val flights2 = flights.joinWith(flightMeta, flights.col("count") === flightMeta.col("count"))

flights2.take(5)
```

输出包含一个键值对的Dataset，其中每一行表示一个Flight和Flight元数据（Metadata）。当然，我们可以查询这些Dataset或具有复杂类型的DataFrame ：

```scala
flights2.selectExpr("_1.DEST_COUNTRY_NAME").show(5)
```

我们也可以在驱动器上获取：

```scala
flights2.take(2)
```

可以将DataFrame和Dataset相互转换，下面两行代码是一致的

```scala
val flights2 = flights.join(flightMeta, Seq("count"))
val flights2 = flights.join(flightMeta.toDF(), Seq("count"))
```

##### 分组和聚合

grouping 和aggregation在DataSet上均可以使用，但返回的为DataFrame而不是DataSet，从而会丢失类型信息。

```scala
flights.groupBy("DEST_COUNTRY_NAME").count()

//返回 org.apache.spark.sql.DataFrame = [dest_country_name: string, count: bigint]
```

如果想保持类型信息，可以用其他方法，典型的例子就是groupByKey

```scala
flights.groupByKey(x => x.dest_country_name).count()
//返回 org.apache.spark.sql.Dataset[(String, Long)] = [value: string, count(1): bigint]
```

虽然这提供了灵活性，但需要权衡，因为现在我们引入了JVM类型以及无法被Spark优化的函数。这意味着会产生性能差异，当我们检查释计划时我们可以看到这一点。下面你会看到我们正在高效地追加一个新列到DataFrame(分组函数的结果)，然后在该列上执行分组

```scala
flights.groupByKey(x => x.dest_country_name).count().explain()
```

```shell
== Physical Plan ==

*(3) HashAggregate(keys=[value#118], functions=[count(1)])
+- Exchange hashpartitioning(value#118, 200)
  +- *(2) HashAggregate(keys=[value#118], functions=[partial_count(1)])
   +- *(2) Project [value#118]
    +- AppendColumns <function1>, newInstance(class $line14.$read$$iw$$iw$Flight), 
       [staticinvoke(class org.apache.spark.unsafe.types.UTF8String, StringType, 
       fromString, input[0, java.lang.String, true], true, false) AS value#118]
      +- *(1) FileScan parquet [DEST_COUNTRY_NAME#0,ORIGIN_COUNTRY_NAME#1,count#2L] 
            Batched: true, Format: Parquet, Location: InMemoryFileIndex[file:/usr/local/spark/testCode/2010-summary.parquet], PartitionFilters: [], PushedFilters: [], ReadSchema: struct<DEST_COUNTRY_NAME:string,ORIGIN_COUNTRY_NAME:string,count:bigint
```



//在Dataset上根据某key执行分组后，我们就可以在Key Value Dataset上根据我们定义的函数操作数据，该函数将分组作为原始对象操作：

```scala
def groSum(countryName: String, values: Iterator[Flight]) = {
    values.dropWhile(_.count < 5).map(x => (countryName, x))
}

flights.groupByKey(x => x.dest_country_name).flatMapGroups(grpSum).show(5)
```





## Spark低级API

RDD：无论是DataFrame还是Dataset，运行的所有Spark代码都将编译成一个RDD。

RDD是一个只读不可变的且已分块的记录集合，并可以被并行处理。

RDD与DataFrame不同，DataFrame中每个记录即是一个结构化的数据行，各字段已知且schema已知,而RDD中的记录仅仅是程序员选择的Java、Scala或Python对象。

由于只是Java，Python对象，你能完全控制RDD，但也带来很多问题，优化，定义都需要手动执行。

RDD API与Dataset类似，所不同的是 RDD 不在结构化数据引擎上进行存储或处理。然而,在RDD和Dataset之间来回转换的代价很小，因此可以同时使用两种API来取长补短。

### RDD类型

- 通用型RDD

- 提供附加函数的key-value RDD

RDD执行两种（算子）操作：惰性执行的转换（Transform）操作和立即执行的动作（Action）操作，都以分布式的方式来处理数据。RDD没有行的概念，单个记录只是简单的Java/Python/Scala对象，必须手动处理这些数据，不能使用结构化API中的函数库。创建RDD，只需要使用rdd方法。如果从Dataset[T]转换成RDD，会返回合适的本机类型T（这仅适用于Scala和Java）

```scala
spark.range(10).rdd
```

要对这些数据进行操作，需要将此Row类型的对象转换为正确的数据类型或从中取出值

```scala
spark.range(10).toDF().rdd.map(rowObject => rowObject.getLong(0)) 
//就变成了Row类型的RDD
```

可以使用类似的方法将RDD转换为DataFrame或Dataset

```java
spark.range(10).rdd.toDF()
```

从本地集合创建RDD

```scala
val myCollection = "Spark The Definitive Guide : Big Data Processing Made Sample".split(" ")
val words = spark.sparkContext.parallelize(myCollection, 2)
```

重命名RDD

```scala
words.setName("myWords")
words.name
```

从数据源创建

```scala
spark.sparkContext.testFile("/some/path/withTextFiles") //逐行读取一个文本文件
```

将每个文件读取为RDD中的一条记录（文件的名称为第一个对象，文件内容为第二个字符串对象）

```scala
spark.sparkContext.wholeTextFiles("some/path/withTextFiles") 
```

### 操作RDD

#### Transformations

大多数情况下，转换操作（transformation）与结构化API中的转换操作在功能上相同。就像使用DataFrame和Dataset一样，需要在RDD上指定转换操作来创建一个新的RDD，这也将导致新的RDD依赖于原有的RDD。

##### distinct

```scala
words.distinct().count()
```

##### filter 

等同于创建SQL中的where条件

```scala
def startsWithS(individual: String) = {
    individual.startsWith("S")
}

words.filter(word => startsWithS(word)).collect()
```

返回：

```scala
Array[String] = Array(Spark, Sample)
```

可以看到，与DatasetAPI一样，它返回的是本地数据类型。这是因为我们从不将数据强制转换为Row类型，也不在收集数据后做数据转换操作。

##### map

```scala
val words2 = words.map(word => (word, word(0), word.startsWith))
```

##### filterMap

```scala
words.flatMap(word => word.toSeq).take(5)
```

##### orderBy

```scala
word.sortBy(word => word.length() * -1).take(2)
```

##### randomSplit 

将一个RDD随机分割为若干个RDD

```scala
val fiftyFiftySplit = word.randomSplit(Array[Double](0.5, 0.5))
```

#### Actions

##### reduce

```scala
spark.sparkContext.parallelize(1 to 20).reduce(_ + _) //计算一组数字的和，返回210
```

返回最长的单词

```scala
def wordLengthReducer(leftWord: String, rightWord: String): String = {
    if (leftWord.length > rightWord.legth)
    return leftWord
    else
    return rightWord
}

words.reduce(wordLengthReducer)
```

##### count

```scala
words.count()
```

##### countApprox 

设置置信度，返回大概的计数结果

```scala
val confidence = 0.95
val timeoutMilliseconds = 400
word.countApprox(timeoutMilliseconds, confidence)
```

##### countApproxDistinct 

计算去重之后大概的个数

```scala
words.countApproxDistinct(0.5)
```

##### countByValue 

针对值的个数进行计数

```scala
words.countByValue()
```

##### countByValueApprox 

与上面功能一致，返回近似值

```scala
words.countByValueApprox(1000, 0.95)
```

##### first

```scala
words.first()
```

##### max, min

```scala
spark.sparkContext.parallelize(1 to 20).max()
spark.sparkContext.parallelize(1 to 20).min()
```

##### take 

从RDD读取一定数量的值

```scala
words.take(5)
words.takeOrdered(5)
words.top() //与takeOrdered的顺序相反
```

#### 保存文件

要将RDD保存到文本文件中，只需指定文件路径和压缩编码器（可选参数）即可

##### saveAsTextFile

```java
words.saveAsTextFile("file:///tmp/bookTitle")
```

要设置一个压缩编码器，必须从Hadoop中倒入适当的编码器

```scala
import org.apache.hadoop.io.compress.BZip2Codec
words.saveAsTextFile("file:/tmp/cookTitleCompressed", classOf[BZip2Codec])
```

##### SequenceFiles

一种由二进制键值对组成的文件，它也是Hadoop MapReduce作业常用的输入/输出格式

```scala
words.saveAsObjectFile("/tmp/my/sequenceFilePath")
```

##### 缓存

缓存RDD的原理与缓存DataFrame和Dataset 相同，你可以缓存或持久化 RDD 。

```scala
words.cache()
```

#### 从本地集合创建RDD

```scala
val myCollection = "Spark The Definitive Guide : Big Data Processing Made Sample".split(" ")

val words = spark.sparkContext.parallelize(myCollection, 2)
```

##### Key_Value基础

```scala
val keyword = word.keyBy(word => word.toLowerCase.toSeq(0).toString) //keyBy

keyword.collect()

//res: Array[(String, String)] = Array((s,Spark), (t,The), (d,Definitive), (g,Guide), (:,:), (b,Big), (d,Data), (p,Processing), (m,Made), (s,Sample))

//对值进行映射
keyword.mapValues(word => word.toUpperCase).collect()

//res: Array[(String, String)] = Array((s,SPARK), (t,THE), (d,DEFINITIVE), (g,GUIDE), (:,:), (b,BIG), (d,DATA), (p,PROCESSING), (m,MADE), (s,SAMPLE))

//提取key和value
words.keys.collect()
words.values.collect()

//查找key对应的value
words.lokkup("s")

//sampleByKey
val distinctChars = word.flatMap(word => word.toLowerCase.toSeq).distinct.collect()

//distinctChars: Array[Char] = Array(d, p, t, b, h, n, f, v, :, r, l, s, e, a, i, k, u, o, g, m, c)

val sampleMap = distinctChars.map(c => (c, new Random().nextDouble())).toMap

//sampleMap: scala.collection.immutable.Map[Char,Double] = Map(e -> 0.0990014447982912, s -> 0.7882338963599441, n -> 0.13378535260500546, t -> 0.408841397647289, u -> 0.2577941609963563, f -> 0.43970604048491135, a -> 0.6219460901237468, m -> 0.44951842614799986, i -> 0.47327015981663867, v -> 0.9601719414193667, b -> 0.40652491224686127, g -> 0.7179986092554698, l -> 0.7936523752872142, p -> 0.41150247710073784, c -> 0.365471973165296, h -> 0.30438666949669646, r -> 0.245415945051202, : -> 0.9105949719001257, k -> 0.21408162244560203, o -> 0.3235687195940211, d -> 0.012299000197014687)

words.map(word => (word.toLowerCase.toSeq(0), word)).sampleByKey(true, sampleMap, 6L).collect()

//res16: Array[(Char, String)] = Array((s,Spark), (t,The), (g,Guide), (:,:))

words.map(word => (word.toLowerCase.toSeq(0), word)).sampleByKeyExact(true, sampleMap, 6L).collect()

//res25: Array[(Char, String)] = Array((s,Spark), (t,The), (d,Definitive), (d,Definitive), (g,Guide), (:,:),  (b,Big), (p,Processing), (m,Made), (s,Sample))
```

#### 聚合操作

```scala
val chars = words.flatMap(word => word.toLowerCase.toSeq)

val KVCharacters = chars.map(letter => (letter, 1))

def maxFunc(left: Int, right: Int) = math.max(left, right)

def addFunc(left: Int, right: Int) = left + right

val nums = sc.parallelize(1 to 30, 5)

KVCharacters.groupByKey().map(row => (row._1, row._2.reduce(addFunc))).collect()

//res34: Array[(Char, Int)] = Array((d,4), (p,3), (t,3), (b,1), (h,1), (n,2), (f,1), (v,1), (:,1), (r,2), (l,1), (s,4), (e,7), (a,5), (k,1), (i,6), (u,1), (o,1), (g,3), (m,2), (c,1))
```

自定义分区是使用RDD的主要原因之一，而结构化API不支持自定义数据分区，RDD包含影响任务能否成功运行的低级实现细节。自定义分区的典型示例是PageRank实现，你需要控制集群上数据的分布并避免shuffle操作。简而言之，自定义分区的唯一目标是将数据均匀地分布到整个集群中，以避免诸如数据倾斜之类的问题。

如果要使用自定义分区，则应从结构化API定义的数据降级为RDD ，应用自定义分区程序，然后再将RDD转换回DataFrame或Dataset。只有真正需要时，才会使用RDD自定义分区，这样就可以利用两方面的优势。

```scala
val df = spark.read
 .option("header", "true")
 .option("inferSchema", "true")
 .csv("file:///usr/local/spark/testCode/online-retail-dataset.csv")
val rdd = df.coalesce(10).rdd 
```

Spark有两个内置的分区器，你可以在RDD API中调用，它们是用于离散值划分的HashPartitioner（基于哈希的分区）以及RangePartitioner（根据数值范围分区），这两个分区器分别针对离散值和连续值。Spark的结构化API已经包含了它们，也可以在RDD中使用它们。

虽然哈希分区和范围分区程序都很有用，但它们是最基本的分区方法。有时，因为数据量很大并且存在严重的数据倾斜，（由于某些key对应的value项比其他key对应的value项多很多导致的数据倾斜），你将需要实现一些非常底层的分区方法。你希望尽可能多地拆分这些key以提高并行性，并在执行过程中防止OutOfMemoryError错误发生。

```scala
import org.apache.spark.HashPartitioner

rdd.map(r => r(6)).take(5).foreach(println) 
val keyedRDD = rdd.keyBy(row => row(6).asInstanceOf[Int].toDouble) 
keyedRDD.partitionBy(new HashPartitioner(10)).take(10)
```

一个典型情况是，当且仅当某个key有特定形式时，由于某个key对应的value太多，需要把这个key拆分成很多key。例如，数据集中可能对某两个客户的数据处理总是会使分析过程崩溃，我们需要对这两个客户数据进行细分，就是说比其他客户ID更细粒度地分解它们。由于这两个key倾斜的情况很严重，所以需要特别处理，而其他的key可以被集中到大组中。这虽然是一个极端的例子，但你可能会在数据中看到类似的情况:

```scala
import org.apache.spark.Partitioner

class DomainPartitioner extends Partitioner{
    def numPartitions = 3
    def getPartition(key: Any): Int = {
        val customerId = key.asInstanceOf[Double].toInt
        if (customerId == 17850.0 || customerId == 12583.0){
            return 0
        } else {
            return new java.util.Random().nextInt(2) + 1
        }
    }
}

keyedRDD.partitionBy(new DomainPartitioner)
  .map(_._1)
  .glom()
  .map(_.toSet.toSeq.length)
  .take(5)

//res14: Array[Int] = Array(2, 4300, 4302)
```

##### 定义累加器

```scala
case class Flight(DEST_COUNTRY_NAME: String, ORIGIN_COUNTRY_NAME: String, count: Long)

val flights = spark.read.parquet("file:///usr/local/spark/testCode/2010-summary.parquet").as[Flight]

import org.apache.spark.util.LongAccumulator

val accChina = new LongAccumulator
val accChina2 = spark.sparkContext.longAccumulator("China")

spark.sparkContext.register(accChina, "China")

def accChinaFunc(flight_row: Flight) = {
    val destination = flight_row.DEST_COUNTRY_NAME
    val origin = flight_row.ORIGIN_COUNTRY_NAME
    if (destination == "China"){
        accChina.add(flight_row.count.toLong)
    }
    if (origin == "China"){
        accChina.add(flight_row.count.toLong)
    }
}
```

##### 自定义累加器

```scala
import scala.collection.mutable.ArrayBuffer
import org.apache.spark.util.AccumulatorV2

val arr = ArrayBuffer[BigInt]()
class EvenAccumulator extends AccumulatorV2[BigInt, BigInt]{
    private var num: BigInt = 0
    def reset(): Unit = {
        this.num = 0
    }
    def add(intValue: BigInt): Unit = {
        if (intValue % 2 == 0){
            this.num += intValue
        }
    }
    
    def value(): BigInt = {
        this.num
    }
    
    def copy(): AccumulatorV2[BigInt, BigInt] = {
        new EvenAccumulator
    }
    
    def isZero(): Boolean = {
        this.num == 0
    }
    
    def merge(other: AccumulatorV2[BigInt, BigInt]): Unit = {
        this.num += other.value
    }
}

val acc = new EvenAccumulator
val newAcc = sc.register(acc, "evenAcc")

//使用
flights.foreach(a => acc.add(a.count))
acc.value //31390
```

## Spark概念

任何Spark应用程序的第一步都是创建一个SparkSession。在交互模式中，通常已经为你预先创建了，但在应用程序中，你必须自己创建，`SparkSession是spark 2.x以后才支持的`

```scala
import org.apache.spark.SparkSession
val spark = SparkSession.builder().appName("Databricks Spark Example")
 .config("spark.sql.warehouse.dir", "/user/hive/warehouse")
 .getOrCreate()
```

可能会发现老旧的代码会直接为结构化API创建SparkContext或者SQLContext。SparkSession中的SparkContext对象代表与Spark集群的连接，可以通过它与一些Spark的低级API（如RDD）进行通信，在较早的示例和文档中，它通常以变量名sc存储。通过SparkContext，你可以创建RDD、累加器和广播变量，并且可以在集群上运行代码。

大多数情况下，你不需要显式初始化SparkContext，你应该可以通过SparkSession来访问它。如果你想要，一般来说你可以通过getOrCreate方法来创建它：

```scala
import org.apache.spark.SparkContext

val sc = SparkContext.getOrCreate()
```

在新版本中，永远不需要使用SQLContext，尽量避免使用SparkContext，初始化SparkSession之后就可以执行一些代码了，`所有的Spark代码都编译为RDD`。

##### 逻辑指令

Spark代码基本上由转换和动作操作组成，无论是SQL查询，低级的RDD操作还是机器学习算法。了解如何使用DataFrame之类的声明式指令并将其转换为物理执行计划，是理解Spark如何在集群上运行的重要一步。

- Spark中的stage代表可以一起执行的任务组，用以在多台机器上执行相同的操作。


- 一般来说，Spark会尝试将尽可能多的工作（即作业内部尽可能多的转换操作）加入同一阶段，但引擎在shuffle操作之后启动新的stage。


- 一次shuffle操作意味着一次对数据的物理重分布，shuffle之后开始新的stage，并按照顺序执行各阶段以计算最终结果。


- partition数量的选择，一个经验法则是分区数量应该大于集群上执行器的数量，这可能取决于工作负载相关的设置。


- task：一个stage有若干个task组成，每个任务对应于一组数据和一组将单个执行器上运行的转换操作。


- task是应用于每个数据单元（分区）的计���单位，将数据划分为更多的分区意味着可以并行执行更多分区。


##### 执行细节

Spark会自动以流水线的方式一并完成连续的stage和task，例如map操作接着另一个map操作；另一个执行细节是，对于所有的shuffle操作，Spark会将数据写入持久化存储（如磁盘），并可以在多个作业中重复使用他。

Spark在写入内存或磁盘之前执行尽可能多的操作，Spark执行的关键优化之一是流水线，在RDD级别或其他级别上执行。通过流水线技术，一系列有数据依赖关系的操作，如果不需要任何节点的数据移动，就可以将这些操作合并为一个单独的任务。

例如，如果你编写一个基于RDD的程序，首先执行map操作，然后是filter操作，然后接着另一个map操作，这些操作不需要在节点间移动数据，所以就可以将它们合并为一个单一阶段的任务，即读取每个输入记录，然后经由第一个map操作，再执行filter操作，然后如果需要的话再执行map操作。

## Structured Streaming

读取数据：

```scala
val static = spark.read.json("/data/activity-data/")
val dataSchema = static.schema
```

所有静态的结构化API中可用的转换操作都适用于流式DataFrame，但是一个区别是结构化流处理不允许你在未明确指定它的情况下执行模式推断，读取一个具有已知可用的数据模式的文件中读取模式dataSchema对象，并将dataSchema对象从静态DataFrame应用到流式DataFrame

```scala
val streaming = spark.readStream.schema(dataSchema)
 .option("maxFilesPerTragger", 1)
 .json("/data/activity-data/")
```

流式DataFrame的创建和执行是惰性的，在最终启用之前可以执行转换操作

```scala
val activityCounts = streaming.groupBy("gt").count()
```

为了避免shuffle分区过多

```scala
spark.conf.set("spark.sql.shuffle.partitions", 5)
```

开始：

```scala
val activityQuery = activityCounts.writeStream.queryName("activity_counts")
 .format("memory")
 .outputMode("complete")
 .start()
```

结构化流处理支持所有的转换和筛选转换，对所有DATAFRAME函数和对单个列的操作也都有支持

##### 选择和筛选

```scala
import org.apache.spark.sql.functions.expr

val simpleTransform = streaming.withColumn("stairs", expr("gt like '%stairs%'"))
 .where("stairs")
 .where("gt is not null")
 .select("gt", "model","arrival_time", "creation_time")
 .writeStream
 .queryName("simple_transform")
 .format("memory")
 .outputMode("app\\end")
 .start()
```

##### 聚合

```scala
val deviceModelStats = streaming.cube("gt","model").avg()
 .drop("avg(Arrival_time)")
 .drop("avg(Creation_time)")
 .drop("avg(Index)")
 .writeStream.queryName("device_counts").format("memory").outputMode("complete")
 .start()
```

##### 连接操作

从spark 2.2开始，结构化流处理支持流式DataFrame与静态DataFrame的连接操作，spark 2.3将增加对链接多个流的支持，可以执行多列连接，并从静态数据中补充数据

```scala
val historicalAgg = static.groupBy("gt", "model").avg()
val deviceModelStats = streaming
 .drop("Arrival_Time", "Creation_Time", "Index")
 .cube("gt", "model")
 .avg()
 .join(historicalAgg, Seq("gt","model"))
 .writeStream
 .queryName("device_counts")
 .format("memory")
 .outputMode("complete")
 .start()
```

事件时间基础

```scala
spark.conf.set("spark.sql.shuffle.partitions", 5)
val static = spark.read.json("/data/activity-data/")
val streaming = spark.readStream.schema(static.schema)
 .option("maxFilesPerTriagger", 1)
 .json("/data/activity-data/")
```

### 事件时间的窗口
事件时间的分析第一不是将时间戳列转换为合适的Spark SQL时间戳类型

```scala
val withEventTime = streaming
  .selectExpr("*", "cast(cast(Creation_time as double)/1000000000 as timestamp) as event_time")
```

#### 滚动窗口(Tumbling Windows)

最简单的计算就是计算给定时间窗口中某事件发生的次数，一个事件只可能落入一个时间窗口

```scala
import org.apache.spark.sql.functions.{window, col}

withEventTime
 .groupBy(window(col("event_time"), "10 minutes"))
 .count()
 .writeStream
 .queryName("events_per_window")
 .format("memory")
 .outputMode("complete")
 .start()
```



//把数据写入内存接收器中以便于调试，我们可以在运行流处理之后使用SQL查询：

```scala
spark.sql("select * from events_per_window").show(5)
```

```scala
+---------------------------------------------+-----+
|                          window|count|
+---------------------------------------------+-----+
|[2015-02-23 18:40:00.0,2015-02-23 18:50:00.0]| 5521|
|[2015-02-24 19:50:00.0,2015-02-24 20:00:00.0]| 9356|
|[2015-02-23 20:20:00.0,2015-02-23 20:30:00.0]| 6672|
|[2015-02-23 21:40:00.0,2015-02-23 21:50:00.0]|10383|
|[2015-02-23 19:20:00.0,2015-02-23 19:30:00.0]| 4664|
+---------------------------------------------+-----+
```

时间窗口实际上是一个结构体（一个复杂的数据类型）,使用此方法可以查询结构体以获得特定时间窗口的开始和结束时间

#### 滑动窗口(Sliding Window)

多时间窗口是可以重叠的，以运行10分钟为增量的滑动窗口，但我们希望每5分钟获得一次状态

```scala
withEventTime.groupBy(window(col("event_time"), "10 minutes", "5 minutes"))
 .count()
 .writeStream
 .queryName("events_per_window")
 .format("memory")
 .outputMode("complete")
 .start()
```

```scala
+---------------------------------------------+-----+
|                          window|count|
+---------------------------------------------+-----+
|[2015-02-22 08:35:00.0,2015-02-22 08:45:00.0]|   2|
|[2015-02-22 08:40:00.0,2015-02-22 08:50:00.0]|   2|
|[2015-02-23 18:10:00.0,2015-02-23 18:20:00.0]|  682|
|[2015-02-23 18:15:00.0,2015-02-22 08:25:00.0]| 3455|
|[2015-02-23 18:20:00.0,2015-02-22 08:30:00.0]| 6167|
+---------------------------------------------+-----+
```

#### 使用watermark处理延迟数据

前面的例子有一个缺陷，我们未指定系统可以接收延迟多久的迟到数据，这意味着spark需要永久存储这些中间数据因为我们没有指定一个过期的时间，如果数据迟到超过一定的时间阈值，我们将不会处理它。这是用于所有基于事件时间的有状态处理，我们必须指定水位(watermark)，以便确定过期数据，这样就可以免于由于长时间不清理过期数据而造成的系统存储压力过大。

watermark是给定时间或事件集之后的一个时间长度，在该时间长度之后我们不希望在看到来自该时间长度之前的任何数据。

```scala
withEventTime
 .withWatermark("event_time", "5 hours") //watermark = 5 hours
 .groupBy(window(col("event_time"), "10 minutes", "5 minutes"))
 .count()
 .writeStream
 .queryName("events_per_window")
 .format("memory")
 .outputMode("complete")
 .start()
```

如果没有指定接受延迟多晚的数据，spark就会永久将这些数据保留在内存中指定watermark可以使其从内存中释放，从而使流处理能够长时间稳定的运行

删除重复数据：

```scala
import org.apache.spark.sql.functions.expr

withEventTime
 .withWatermark("event_time", "5 seconds")
 .dropDuplicates("User", "event_time")
 .groupBy("User")
 .count()
 .writeStream
 .queryName("deduplicated")
 .format("memory")
 .outputMode("complete")
 .start()
```



### 任意有状态的处理



由上述例子，当你有更复杂的窗口概念时情况会有所不同而这是任意有状态处理(Arbitrary Stateful Processing)的所长之处。执行有状态的处理可能需要执行以下操作：根据给定键的计数创建窗口如果在特定事件范围内发生多个特定事件，则发出警报，在不确定的时间内维护用户会话，保存这些回话以便稍后进行分析在一天的最后，如果执行这类处理时，你将需要执行以下两件事情： 映射数据中的组，对每组数据进行操作，并为每个组生成至多一行。这个用例的相关API是`mapGroupsWithState`。映射数据中的组，对每组数据进行操作，并为每个组生成一行或多行。这个用例的相关API是`flatMapGroupsWithState`。 

当我们说对每组数据进行“操作”时，你可以独立于其他数据组去任意更新该组，也就是说你也可以定义不符合翻滚窗口或滑动窗口的任意窗口类型。在执行这种处理方式时获得的一个重要好处是可以控制配置状态超时，使用时间窗口和watermark，这非常简单：当一个窗口开始前watermark 已经超时，你只需暂停该窗口。这不适用于任意有状态处理，因为你是基于用户定义的概念来管理状态，所以你需要适当地设置状态超时。

####  输出模式

##### mapGroupWithState

类似于用户自定义的聚合函数，它将更新数据集作为输入，然后将其间隙为一个对应值集合的键需要给出以下定义：

-  三个类定义：输入定义、状态定义以及可选的输出定义；


-  基于键、事件迭代器和先前状态的函数；


-  超时时间参数


要查找给定用户在数据集中执行某项活动的第一个和最后一个时间戳，这意味着分组操作的键是用户和活动的组合

输入定义：

```java
case class InputRow(user: String, timestamp: java.sql.Timestamp, activity: String)
```

状态定义：

```scala
case class UserState(user: String, 
  var activity: String, 
  var start: java.sql.Timestamp,
  var end: java.sql.Timestamp)
```

输出定义：

```scala
def updateUserStateWithEvent(state: UserState, input: InputRow): UserState = {
    if (Option(input.timestamp) isEmpty) {
        return state
    }
    if (state.activity == input.activity) {
        if (input.timestamp.after(state.end)) {
            state.end = input.timestamp
        }
        if (input.timestamp.before(state.start)) {
            state.start = input.timestamp
        }
    } else {
        if (input.timestamp.after(state.end)) {
            state.start = input.timestamp
            state.end = input.timestamp
            state.activity = input.activity
        }
    }
    state
}
```

现在需要通过函数来定义根据每一批次行来更新状态

```scala
import org.apache.spark.sql.streaming.{GroupStateTimeout, OutputMode, GroupState}

def updateAcrossEvents(user: String, inputs: Iterator[InputRow], oldState: GroupState[UserState]): UserState = {
    var state:UserState = if (oldState.exists) oldState.get else UserState(
        user,
        "",
        new java.sql.Timestamp(6284160000000L),
        new java.sql.Timestamp(6284160L)
    )
    for (input <- inputs) {
        state = updateUserStateWithEvent(state, input)
        oldState.update(state)
    }
    state
}
```



定义好之后，就可以通过传递相关信息来执行查询。在指定mapGroupWithState时，需要指定是否需要超时给定组的状态，它告诉系统如果在一段时间后没有收到更新的状态应该做什么，在这个例子中，希望无限期地维护状态，因此spark不会超时。使用update输出模式，以便获得用户活动的更新

```scala
import org.apache.spark.sql.streaming.GroupStateTimeout

withEventTime
 .selectExpr("User as user",
 "cast(Creation_time/1000000000 as timestamp) as timestamp", 
 "gt as activity")
 .as[InputRow]
 .groupByKey(_.user)
 .mapGroupsWithState(GroupStateTimeout.NoTimeout)(updateAcrossEvents)
 .writeStream
 .queryName("events_per_window")
 .format("memory")
 .outputMode("update")
 .start()
```



##### 查询结果

```scala
spark.sql("select * from events_per_window").orderBy("user", "start").show(5)
```

```
+----+--------+--------------------+----------------------+
|user|activity|        start   |             end|
+----+--------+--------------------+----------------------+
|   a|    bike|2015-02-23 21:30:...|2015-02-23 22:06:...|
|   b|    bike|2015-02-24 22:01:...|2015-02-24 22:38:...|
|   c|    bike|2015-02-23 20:40:...|2015-02-23 21:15:...|
|   d|    bike|2015-02-24 21:07:...|2015-02-24 21:42:...|
|   e|    bike|2015-02-24 22:45:...|2015-02-24 23:21:...|
+----+--------+--------------------+----------------------+
```



##### 基于计数的窗口

典型的窗口操作，是把落入开始时间和结束时间之内的所有事件都来进行计数或求和。但是有时候不是基于时间创建窗口，而是基于大量事件创建它们，而不考虑状态和事件事件，并且在该数据窗口上执行一些聚合操作。例如我们想要为接收到的500个事件计算一个值，而不管它们何时接收到。

下面是各种自描述的case类定义：

```scala
case class InputRow(device: String, timestamp: java.sql.Timestamp, x: Double)
case class DeviceState(device: String, var: values: Array[Double], var count: Int)
case class OutputRow(device: String, previousAverage: Double)
```

现在定义一个函数，根据输入的一行来更新状态：

```scala
def updateWithEvent(state: DeviceState, input: InputRow): DeviceState = {
    state.count += 1
}
```

现在定义一个基于多输入行的更新函数，，有一个特定的键、包含一系列输入的迭代器、还有旧的状态，然后随着时间推移在接收到新事件后我们持续更新这个旧状态。它根据每个设备上发生事件的计数数量，针对每个设备返回更新的输出行。   这种情况非常简单，在一定事件数量之后，更新状态并重置它，然后创建一个输出行。可以在输出表中看到此行：

```scala
import org.apache.spark.sql.streaming.{GroupStateTimeout, OutputMode, GroupState}

def updateAcrossEvents(device: String, 
                       inputs: Iterator[InputRow],
                       oldState: GroupState[DeviceState]): Iterator[OutputRow]={
    inputs.toSeq.sortBy(_.timestamp.getTime).toIterator.flatMap{
        input => val state = if (oldState.exists) oldState.get 
                             else DeviceState(device, Array(), 0)
        val newState = updateWithEvent(state, input)
        if (newState.count >= 500){
            // 某个窗口完成之后用空的DeviceState代替该状态，并输出过去500项的平均值
            oldState.update(DeviceState(device, Arrat(), 0))
            Iterator(OutputRow(device, newState.values.sum /                                              newState.values.length.toDouble))
        }
        else {
            oldState.update(newState)
            Iterator()
        }
    }
}
```

现在可以运行流处理任务了，会注意到需要显式说明输出模式，即append模式，还需要设置一个GroupStateTimeout，这个超时时间指定了窗口输出的等待时间（即使没有达到所需的计数）。在这种情况下，如果设置一个无限的超时时间，则意味着如果一个设备一直没有累积到要求的 500 计数阈值，该状态将一直保持为“不完全”并且不会输出到结果表。

```scala
import org.apache.spark.sql.streaming.GroupStateTimeout
withEventTime
  .selectExpr("Device as device",
  "cast(Creation_Time/1000000000 as timestamp) as timestamp"，"x")
  .as[InputRow]
  .groupByKey(_.device)
  .flatMapGroupsWithState(OutputMode.Append,
  GroupStateTimeout.NoTimeout)(updateAcrossEvents)
  .writeStream
  .queryName("count_based_device")
  .format("memory")
  .outputMode("append")
  .start()
```

##### flatMapGroupWithState

第二个有状态处理的示例使用 flatMapGroupsWithState ，这与 mapGroupsWithState 非常相似，不同之处在于一个键不是只对应最多一个输出，而是一个键可以对应多个输出。这可以为我们提供更多的灵活性，并且与 mapGroupsWithState 具有相同的基本结构。

- 三个类定义：   输入定义、状态定义、以及可选的输出定义；

- 一个函数，输入参数为一个键、一个多事件迭代器、和先前状态；

- 超时事件参数  ( 如超时部分所述 )

通过这些对象和定义，我们可以通过创建状态、（随着时间推移）更新状态、以及删除状态来控制任意状态。接下来我们提供一个会话化（ Sessionization ）的例子。

会话是未指定的时间窗口，其中可能发生一系列事件。通常你需要将这些不同的事件记录在数组中，以便将其与将来的其他会话进行比较。在一次会话中，用户可能会设计各种不同的逻辑来维护和更新状态，以及定义窗口何时结束（如计数）或通过简单的超时来结束。我们以前面的例子为基础，并把它更严格地定义为一个会话。  

有时候你可能有一个明确的会话 ID ，你可以在你的函数中使用它。这是很容易实现的，因为你只是执行简单的聚合，甚至可能不需要你自己定义的有状态逻辑。在下面这个例子中，将根据用户 ID 和一些时间信息即时创建会话，并且如果你在五秒内没有看到该用户的新事件，则会话将终止。你还会注意到，此代码使用超时的方式与其他示例中的不同。

可以遵循创建类的相同流程，定义我们的单个事件更新函数，然后定义多事件更新函数：

```scala
case class InputRow(uid: String, timestamp: java.sql.Timestamp, x: Double, activity: String)

case class UserSession(var uid: String,
                       var timestamp: java.sql.Timestamp,
                       var activities: Array[String],
                       var values: Array[Double])

case class UserSessionOutput(var uid: String, var activities: Array[String], var xAvg: Double)

def updateWithEvent(state: UserSession: input: InputRow): UserSession = {
  // 处理异常数据 
  if (Option(input.timestamp).isEmpty) {
    return state
  }
  state.timestamp = input.timestamp
  state.values = state.values ++ Array(input.x)
  if (!state.activities.contains(input.activity)) {
    state.activities = state.activities ++ Array(input.activity)
  }
  state
}

import org.apache.spark.sql.streaming.{GroupStateTimeout: OutputMode, GroupState}

def updateAcrossEvents(uid: String,
                       inputs: Iterator[InputRow],
                       oldState: GroupState[UserSession]): Iterator[UserSessionOutput] = {
  inputs.toSeq.sortBy(_.timestamp.getTime).toIterator.flatMap { input =>
    val state = if (oldState.exists) oldState.get else UserSession(
      uid,
      new java.sql.Timestamp(6284160000000L),
      Array(),
      Array())
    val newState = updateWithEvent(state, input)
    if (oldState.hasTimedOut) {
      val state = oldState.get
      oldState.remove()
      Iterator(UserSessionOutput(uid,
        state.activities,
        newState.values.sum /
          newState.values.length.toDouble))
    } else if (state.values.length > 1000) {
      val state = oldState.get
      oldState.remove()
      Iterator(UserSessionOutput(uid,
        state.activities,
        newState.values.sum /
          newState.values.length.toDouble))
    } else {
      oldState.update(newState)
      oldState.setTimeoutTimestamp(newState.timestamp.getTime(), "5 seconds")
      Iterator()
    }
  }
} 
```

你会看到，我们只希望看到最多延迟 5 秒的事件，我们将忽略太晚到达的事件。在这个有状态操作中，我们将使用 EventTimeTimeout 来基于事件时间设置超时：

```scala
import org.apache.spark.sql.streaming.GroupStateTimeout

withEventTime.where("x is not null") 
  .selectExpr("user as uid", 
  "cast(Creation_Time/1000000000 as timestamp) as timestamp", 
  "x"，"gt as activity") 
  .as[InputRow] 
  .withWatermark("timestamp"，"5 seconds") 
  .groupByKey(_.uid) 
  .flatMapGroupsWithState(OutputMode.Append, 
  GroupStateTimeout.EventTimeTimeout)(updateAcrossEvents) 
  .writeStream 
  .queryName("count_based_device") 
  .format("memory") 
  .start() 
```

###  生产中的结构化处理

流处理应用程序中最需要重视的问题是故障恢复。故障是不可避免的，你可能会丢失集群中的一台机器，在没有适当迁移的情况下数据模式 schema 可能发生意外更改，或者你可能需要重新启动集群或应用程序。在上面任何一种情况下，结构化流处理允许你仅通过重新启动应用程序
来恢复它。为此，**你必须将应用程序配置为使用检查点和预写日志，这两者都由引擎自动处理**。具体来说，你必须配置查询以写入可靠文件系统上的检查点位置（例如， HDFS ， S3 或其他兼容的文件系统）。然后，结构化流处理将周期性地保存所有相关进度信息（例如，一个触发操作处理的偏移量范围），以及当前中间状态值，将它们保存到检查点位置。在失败情况下，你只需重新启动应用程序，确保指向相同的检查点位置，它将自动恢复其状态并在其中断的位置重新开始处理数据。你不必手动管理此状态，结构化流处理可以自动完成这些过程。

要使用检查点，请在启动应用程序之前通过 writeStream 上的 checkpointLocation 选项指定检查点位置。

```scala
val static = spark.read.json("/data/activity-data/")
val streaming = spark
  .readStream
  .schema(static.schema)
  .option("maxFilesPerTrigger", 1)
  .json("/data/activity-data/")
  .groupBy("gt")
  .count()
val query = streaming.write
```











