# Windows上使用IDEA部署Flink开发环境

## 1 配置MAVEN

### 1.1 首先下载最新的Maven安装包

下载地址为：http://maven.apache.org/download.cgi，找到bin.zip压缩包的下载链接，点击即可下载。

### 1.2 将maven安装包解压到指定目录

### 1.3 配置Maven系统环境变量

依次选择”我的电脑–属性–高级系统设置–环境变量–系统变量—新建变量”，新建一个系统变量。设置变量名：M2_HOME，变量值：E:\maven\apache-maven-3.6.3。

### 1.4 继续配置Maven系统环境变量path

在系统变量中找到Path环境变量，在变量值尾部加入”%M2_HOME%\bin”（注意，如果是Win7系统，则需要在前面加上一个分号，用来和其他路径值分隔开）。

### 1.5 检查maven的环境变量是否配置成功

打开命令行，执行以下命令

```shell
$ mvn -v

Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: D:\App\apache-maven-3.6.3\bin\..
Java version: 13.0.1, vendor: Oracle Corporation, runtime: D:\App\jdk-13.0.1
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```

如果能正确地输出maven版本信息，则说明配置正确。

## 2 修改Maven本地仓库默认位置

Maven会将下载的类库（jar包）放置到本地的一个目录下（一般默认情况下Maven默认的本地仓库路径为${user.home}/.m2/repository，其中${user.home}指的是当前用户主目录），如果想重新定义这个仓库目录的位置就需要修改Maven本地仓库的配置。

修改本地仓库默认位置的操作步骤如下：             

1. 在自己喜欢的位置创建文件夹，此处本人创建的位置是”E:\maven\repository”。             

2. 在安装Maven的目录下找到”conf/settings.xml”文件，打开编辑，更改默认的仓库位置，并指定仓库的镜像位置。编辑内容，查找localRepository，替换为Maven目录下的repository：

```xml
<localRepository>D:\App\apache-maven-3.6.3\repository</localRepository>
```

3. 要基于Maven的Archetype来快速创建一个新的Flink工程，还需要在<mirrors/>标签后，加上以下<profiles>内容和<artiveProfiles>内容。如下图所示：（*Archetype是Maven工程的模板工具包，会帮助用户创建Maven工程模板，并给用户提供生成相关工程模板版本的参数化方法*）

4. 最后，settings.xml文件的完整内容如下：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
   
     <localRepository>E:/maven/repository</localRepository>
   
     <mirrors>
       <mirror>
   		<id>maven-repository</id>
   		<mirrorOf>*</mirrorOf>
   		<name>maven repo1</name>
   		<url>https://repo.maven.apache.org/maven2/</url>		     
   	</mirror>
     </mirrors>
   
     <profiles>
       <profile>
   	  <id>archetype</id>
   	  <repositories>
   	    <repository>
   	      <id>archetype</id>
   	      <url>https://repo1.maven.org/maven2/</url>
   	      <releases>
   	        <enabled>true</enabled>
   	        <checksumPolicy>fail</checksumPolicy>
   	      </releases>
   	      <snapshots>
   	        <enabled>true</enabled>
   	        <checksumPolicy>warn</checksumPolicy>
   	      </snapshots>
   	    </repository>
   	  </repositories>
   	</profile>
     </profiles>
   
     <activeProfiles>
   	  <activeProfile>archetype</activeProfile>
     </activeProfiles>
   </settings>
   ```

5. 验证修改是否生效。打开命令行，执行以下命令：

   ```shell
   > mvn help:system
   ```

   执行完该命令之后，在”E:\maven\repository”下面会出现很多文件，这些文件就是maven从中央仓库下载到本地仓库的文件。
   
## 3 创建Flink初始模板项目

1. 在命令行中，使用如下的命令快速创建一个空白Flink项目：

```shell
> cd FlinkProjects               		// 在指定的工作目录下
> mvn archetype:generate \
      "-DarchetypeGroupId=org.apache.flink" \
      "-DarchetypeArtifactId=flink-quickstart-scala" \
      "-DarchetypeCatalog=local" \
      "-DarchetypeVersion=1.11.1"
```

这会在”FlinkProjects”目录下创建一个Flink模板项目。在创建过程中，Maven会交互式地询问groupId、artifactId和package名称。这里我分别输入以下名称：

- groupId：com.xueai8
- artifactId：FlinkScalaBlank
- package：com.xueai8

2. 工作目录中将有一个新目录，目录名称是artifactId的名称。在命令行中可以使用下面的命令查看项目结构：

   ```shell
   > tree FlinkJavaBlank
   
   D:.
   └─flinkScala
       └─src
           └─main
               ├─resources
               └─scala
                   └─com
                       └─learn
                           └─flink
   ```

   示例项目是一个Maven项目，它包含两个类：StreamingJob和BatchJob，分别是DataStream和DataSet程序的基本框架程序。类中的main方法是程序的入口点，用于内部测试/执行和适当的部署。这个基本框架程序然后可以导入到IDE中进行开发。

## 4 打包项目

如果想构建/打包FlinkScalaBlink项目以便部署，需要运行”mvn clean package”命令。请按以下步骤操作：

1、转到项目目录。在命令行中执行以下命令：

```shell
> cd E:/FlinkProjects/FlinkScalaBlank
> mvn clean package
```

2、在项目目录下，会生成一个target目录。我们打包的结果文件就放在这个目录中，可以转到相应的文件夹去查看，也可以直接在命令行下执行dir命令查看。

3、查看target文件夹，将会发现一个JAR文件，其中包含我们应用程序以及相关的依赖项（连接器和库）。