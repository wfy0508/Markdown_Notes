# Spark程序部署练习-SparkPi

## 本地编译

1. 安装Intellij IDEA

2. 安装Scala：File-->Settings-->Plugins-->Scala

3. 创建Project：File-->New-->Project，选择Scala--sbt，设置Project名称，选择Scala的版本（**一定要与指定版本的spark使用的Scala一致**，可在spark官网查询得到对应的版本）

4. 导入依赖包：File-->Project--Libraries-->点击+号，选择spark安装包下的jars目录

5. 下载hadoop-common-2.2.0-bin-master.zip，并设置环境变量（Hadoop_Home和PATH），重启生效。

6. 右键Project中src/main，新建Scala Class，选择Object，输入脚本名后点击确定，将下面代码，粘贴进去：

 ```scala
   package org.apache.spark.examples
   import scala.math.random
   import org.apache.spark.sql.SparkSession
   
   /** Computes an approximation to pi */
   object SparkPi {
     def main(args: Array[String]) {
       val spark = SparkSession
         .builder
         .appName("Spark Pi")
         .getOrCreate()
       val slices = if (args.length > 0) args(0).toInt else 2
       val n = math.min(100000L * slices, Int.MaxValue).toInt // avoid overflow
       val count = spark.sparkContext.parallelize(1 until n, slices).map { i =>
         val x = random * 2 - 1
         val y = random * 2 - 1
         if (x * x + y * y <= 1) 1 else 0
       }.reduce(_ + _)
       println(s"Pi is roughly ${4.0 * count / (n - 1)}")
       spark.stop()
     }
   }
 ```

8. 点击Run-->Edit Configurations，Main class设置为`org.apache.spark.examples.SparkPi`，VM options设置为`-Dspark.master=local`

## 打包上传运行

1. 打开项目结构配置页面，可以使用快捷键 `Ctrl+Alt+Shift+S`
2. 选择Artifacts，点击+号，选择JAR-->From Modules with dependencies
3. 去除额外的lib包依赖，不将其他依赖打包到jar文件中，只保留class编译文件及META-INF文件夹，只要`SparkPi compile output`
4. 启动Hadoop和Spark
5. 编译构建 生成jar包：Build-->Build Artifacts...
6. 将JAR包上传HDFS：`hadoop fs -put sparkLearn.jar /sparkLearn`
7. `./bin/spark-submit --class SparkPi --master local hdfs://localhost:9000/sparkLearn/sparkLearn.jar`
8. 也可将JAR包放在spark目录下面，使用本地的文件运行`./bin/spark-submit --class SparkPi --master local /sparkLearn/sparkLearn.jar`

