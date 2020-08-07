# Java笔记

## 1 Java程序基础

1. Java入口程序规定的方法必须是静态方法，方法名必须为`main`，括号内的参数必须是String数组。

```java
/**
 * 可以用来自动创建文档的注释
 */
public class Hello { //public 表明该class是公开的，不写也可以，但这个类无法在命令行调用
    public static void main(String[] args) {
        // 向屏幕输出文本:
        System.out.println("Hello, world!");
        /* 多行注释开始
        注释内容
        注释结束 */
    }
} // class定义结束
```

2. Java的每一行语句必须以分号结束。
3. 定义变量的时候，如果加上**final**修饰符，这个变量就变成了常量，常量在定义时进行初始化后就不可再次赋值，再次赋值会导致编译错误。
4. 要特别注意，整数由于存在范围限制，如果计算结果超出了范围，就会产生溢出，而溢出*不会出错*，却会得到一个奇怪的结果。
5. 整数的除法对于除数为0时运行时将报错，但编译不会报错。
6. <<表示向左移位，>>表示向右移位，**如果对一个负数进行右移，最高位的`1`不动，结果仍然是一个负数**。
7. **不带符号的右移运算，使用`>>>`，它的特点是符号位跟着动，因此，对一个负数进行`>>>`右移，它会变成正数，原因是最高位的`1`变成了`0`**
8. 对`byte`和`short`类型进行移位时，会首先转换为`int`再进行位移。
9. 位运算是按位进行与(**&**)、或(**|**)、非(**~**)和异或(**^**)的运算。
10. 类型自动提升与强制转型：在运算过程中，如果参与运算的两个数类型不一致，那么计算结果为较大类型的整型。例如，`short`和`int`计算，结果总是`int`，原因是`short`首先自动被转型为`int` ，int类型和double或者float类型一起计算时，会提升到double或者float。
11. 强制转型使用`(类型)`，例如，将`int`强制转型为`short`

```java
int i = 12345;
short s = (short) i; // 12345
```

12. 要注意，超出范围的强制转型会得到错误的结果，原因是转型时，`int`的两个高位字节直接被扔掉，仅保留了低位的两个字节
13. 浮点数运算和整数运算相比，只能进行加减乘除这些数值计算，不能做位运算和移位运算。
14. 在计算机中，浮点数虽然表示的范围大，但是，浮点数有个非常重要的特点，就是浮点数常常无法精确表示。浮点数`0.1`在计算机中就无法精确表示，因为十进制的`0.1`换算成二进制是一个无限循环小数，很显然，无论使用`float`还是`double`，都只能存储一个`0.1`的近似值。但是，`0.5`这个浮点数又可以精确地表示。
15. 因为浮点数常常无法精确表示，因此，浮点数运算会产生误差：

```java
public class Main {
    public static void main(String[] args) {
        double x = 1.0 / 10;
        double y = 1 - 9.0 / 10;
        // 观察x和y是否相等:
        System.out.println(x); //0.1
        System.out.println(y); //0.09999999999999998
    }
}
```

16. 定义一个数组类型的变量，使用数组类型“类型[]”，例如，`int[]`，和单个基本类型变量不同，数组变量初始化必须使用`new int[5]`表示创建一个可容纳5个`int`元素的数组。
17. Java的数组有几个特点：
    - 数组所有元素初始化为默认值，整型都是`0`，浮点型是`0.0`，布尔型是`false`；
    - 数组一旦创建后，大小就不可改变。
18. 可以在定义数组时直接指定初始化的元素，这样就不必写出数组大小，而是由编译器自动推算数组大小

```java
public class Main {
    public static void main(String[] args) {
        // 5位同学的成绩:
        int[] ns = new int[] { 68, 79, 91, 85, 62 };
        int[] ns1 = { 68, 79, 91, 85, 62 }; //进一步简写
        System.out.println(ns.length); // 编译器自动推算数组大小为5
    }
}
```

19. 在Java中，判断值类型的变量是否相等，可以使用`==`运算符。但是，判断**引用类型的变量是否相等**，`==`表示“引用是否相等”，或者说，是否**指向同一个对象**。例如，下面的两个String类型，它们的内容是相同的，但是，分别指向不同的对象，用`==`判断，结果为`false`

20. 要判断引用类型的变量内容是否相等，必须使用`equals()`
21. switch循环时，一定要跟上break，且不要忘了default

```java
//Java 12之前
public class Main {
    public static void main(String[] args) {
        int option = 2;
        switch (option) {
        case 1:
            System.out.println("Selected 1");
            break;
        case 2:
            System.out.println("Selected 2");
            break; //不加break的话，case 3也会执行
        case 3:
            System.out.println("Selected 3");
            break;
        default:
            System.out.println("Not selected");
            break;
        }
    }
}

```

```java
// Java 12之后switch语句升级为更简洁的表达式语法，使用类似模式匹配（Pattern Matching）的方法，保证只有一种路径会被执行，并且不需要break语句
public class Main {
    public static void main(String[] args) {
        String fruit = "apple";
        switch (fruit) {
        case "apple" -> System.out.println("Selected apple");
        case "pear" -> System.out.println("Selected pear");
        case "mango" -> {
            System.out.println("Selected mango");
            System.out.println("Good choice!");
        }
        default -> System.out.println("No fruit selected");
        }
    }
}
```

```java
int opt;
switch (fruit) {
case "apple":
    opt = 1;
    break;
case "pear":
case "mango":
    opt = 2;
    break;
default:
    opt = 0;
    break;
}

//使用新的switch语法，不但不需要break，还可以直接返回值。把上面的代码改写如下：
public class Main {
    public static void main(String[] args) {
        String fruit = "apple";
        int opt = switch (fruit) {
            case "apple" -> 1;
            case "pear", "mango" -> 2;
            default -> 0;
        }; // 注意赋值语句要以;结束
        System.out.println("opt = " + opt);
    }
}
```

22. 大多数时候，在`switch`表达式内部，我们会返回简单的值。但是，如果需要复杂的语句，我们也可以写很多语句，放到`{...}`里，然后，用`yield`返回一个值作为`switch`语句的返回值：

```java
public class Main {
    public static void main(String[] args) {
        String fruit = "orange";
        int opt = switch (fruit) {
            case "apple" -> 1;
            case "pear", "mango" -> 2;
            default -> {
                int code = fruit.hashCode();
                yield code; // switch语句返回值
            }
        };
        System.out.println("opt = " + opt);
    }
}
```

23. Java程序的入口是`main`方法，而`main`方法可以接受一个命令行参数，它是一个`String[]`数组。

## 2 面向对象编程

### 2.1 定义class

```java
class Person {
    public String name; //类型在前，字段在后
    public int age;
}
```

1. 一个`class`可以包含多个字段（`field`），字段用来描述一个类的特征。上面的`Person`类，我们定义了两个字段，一个是`String`类型的字段，命名为`name`，一个是`int`类型的字段，命名为`age`。因此，通过`class`，把一组数据汇集到一个对象上，实现了数据封装。

2. `public`是用来修饰字段的，它表示这个字段可以被外部访问。

### 2.2 常见实例

定义了class，只是定义了对象模版，而要根据对象模版创建出真正的对象实例，必须用new操作符。  new操作符可以创建一个实例，然后，我们需要定义一个引用类型的变量来指向这个实例：

```java
Person ming = new Person();
```

上述代码创建了一个Person类型的实例，并通过变量`ming`指向它。  注意区分`Person ming`是定义`Person`类型的变量`ming`，而`new Person()`是创建`Person`实例。

有了指向这个实例的变量，我们就可以通过这个变量来操作实例。访问实例变量可以用`变量.字段`，例如：

```java
ming.name = "Xiao Ming"; // 对字段name赋值
ming.age = 12; // 对字段age赋值
System.out.println(ming.name); // 访问字段name

Person hong = new Person();
hong.name = "Xiao Hong";
hong.age = 15;
```

直接把`field`用`public`暴露给外部可能会破坏封装性。  

```java
Person ming = new Person();
ming.name = "Xiao Ming";
ming.age = -99; // age设置为负数
```

为了避免外部代码直接去访问`field`，我们可以用`private`修饰`field`，拒绝外部访问

```java
class Person {
    private String name;
    private int age;
}
```

把`field`从`public`改成`private`，外部代码不能访问这些`field`，  我们需要使用方法（`method`）来让外部代码可以间接修改`field`

```java
class Person {
    private String name;
    private int age;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return this.age;
    }

    public void setAge(int age) {
        if (age < 0 || age > 100) {
            throw new IllegalArgumentException("invalid age value");
        }
        this.age = age;
    }
}
```

虽然外部代码不能直接修改`private`字段，但是，外部代码可以调用方法`setName()`和`setAge()`来间接修改`private`字段。  `setAge()`就会检查传入的参数，参数超出了范围，直接报错。这样，外部代码就没有任何机会把`age`设置成不合理的值。  对`setName()`方法同样可以做检查，例如，不允许传入`null`和空字符串 :

```java
public void setName(String name) {
    if (name == null || name.isBlank()) {
        throw new IllegalArgumentException("invalid name");
    }
    this.name = name.strip(); // 去掉首尾空格
}
```

同样，外部代码不能直接读取`private`字段，但可以通过`getName()`和`getAge()`间接获取`private`字段的值。

所以，一个类通过定义方法，就可以给外部代码暴露一些操作的接口，同时，内部自己保证逻辑一致性。调用方法的语法是`实例变量.方法名(参数);`。一个方法调用就是一个语句，所以不要忘了在末尾加`;`。例如：`ming.setName("Xiao Ming");`。

### 2.3 定义方法

```java
修饰符 方法返回类型 方法名(方法参数列表) {
    若干方法语句;
    return 方法返回值;
}
```

**方法返回值通过`return`语句实现，如果没有返回值，返回类型设置为`void`，可以省略`return`。**

#### 2.3.1 private方法

有`public`方法，自然就有`private`方法。和`private`字段一样，`private`方法不允许外部调用，定义`private`方法的理由是内部方法是可以调用`private`方法的。

```java
class Person {
    private String name;
    private int birth;

    public void setBirth(int birth) {
        this.birth = birth;
    }

    public int getAge() {
        return calcAge(2019); // 调用private方法
    }

    // private方法:
    private int calcAge(int currentYear) {
        return currentYear - this.birth;
    }
}
```

`calcAge()`是一个`private`方法，外部代码无法调用，但是，内部方法`getAge()`可以调用它。

此外，我们还注意到，这个`Person`类只定义了`birth`字段，没有定义`age`字段，获取`age`时，通过方法`getAge()`返回的是一个实时计算的值，并非存储在某个字段的值。这说明方法可以封装一个类的对外接口，调用方不需要知道也不关心`Person`实例在内部到底有没有`age`字段。

#### 2.3.2 this变量

在方法内部，可以使用一个隐含的变量`this`，它始终指向当前实例。因此，通过`this.field`就可以访问当前实例的字段。

如果没有命名冲突，可以省略`this`。例如：

```java
class Person {
    private String name;

    public String getName() {
        return name; // 相当于this.name
    }
}
```

但是，如果有局部变量和字段重名，那么局部变量优先级更高，就必须加上`this`：

```java
class Person {
    private String name;

    public void setName(String name) {
        this.name = name; // 前面的this不可少，少了就变成局部变量name了
    }
}
```

#### 2.3.3 方法参数

方法可以包含0个或任意个参数。方法参数用于接收传递给方法的变量值。调用方法时，必须严格按照参数的定义一一传递。例如： 

```java
class Person {
    ...
    public void setNameAndAge(String name, int age) {
        ...
    }
}
```

调用这个`setNameAndAge()`方法时，必须有两个参数，且第一个参数必须为`String`，第二个参数必须为`int`： 

```java
Person ming = new Person();
ming.setNameAndAge("Xiao Ming"); // 编译错误：参数个数不对
ming.setNameAndAge(12, "Xiao Ming"); // 编译错误：参数类型不对
```

#### 2.3.4 可变参数

可变参数用`类型...`定义，可变参数相当于数组类型：

```java
class Group {
    private String[] names;

    public void setNames(String... names) {
        this.names = names;
    }
}
```

上面的`setNames()`就定义了一个可变参数。调用时，可以这么写：

```java
Group g = new Group();
g.setNames("Xiao Ming", "Xiao Hong", "Xiao Jun"); // 传入3个String
g.setNames("Xiao Ming", "Xiao Hong"); // 传入2个String
g.setNames("Xiao Ming"); // 传入1个String
g.setNames(); // 传入0个String
```

 完全可以把可变参数改写为`String[]`类型：

```java
class Group {
    private String[] names;

    public void setNames(String[] names) {
        this.names = names;
    }
}
```

但是，调用方需要自己先构造`String[]`，比较麻烦。例如：

```java
Group g = new Group();
g.setNames(new String[] {"Xiao Ming", "Xiao Hong", "Xiao Jun"}); // 传入1个String[]
```

另一个问题是，调用方可以传入`null`：

```java
Group g = new Group();
g.setNames(null);
```

而可变参数可以保证无法传入`null`，因为传入0个参数时，接收到的实际值是一个空数组而不是`null`。

#### 2.3.5 参数绑定

调用方把**参数传递**给实例方法时，调用时传递的值会按参数位置一一绑定。

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        int n = 15; // n的值为15
        p.setAge(n); // 传入n的值
        System.out.println(p.getAge()); // 15
        n = 20; // n的值改为20
        System.out.println(p.getAge()); // 15还是20?
    }
}

class Person {
    private int age;

    public int getAge() {
        return this.age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

```

结果为两次返回的结果都是15。从结果可知，修改外部的局部变量`n`，不影响实例`p`的`age`字段，原因是`setAge()`方法获得的参数，复制了`n`的值，因此，`p.age`和局部变量`n`互不影响。

**结论**：基本类型参数的传递，是调用方值的复制。双方各自的后续修改，互不影响。

再看一个**传递参数引用**的例子：

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        String[] fullname = new String[] { "Homer", "Simpson" };
        p.setName(fullname); // 传入fullname数组
        System.out.println(p.getName()); // "Homer Simpson"
        fullname[0] = "Bart"; // fullname数组的第一个元素修改为"Bart"
        System.out.println(p.getName()); // "Homer Simpson"还是"Bart Simpson"?
    }
}

class Person {
    private String[] name;

    public String getName() {
        return this.name[0] + " " + this.name[1];
    }

    public void setName(String[] name) {
        this.name = name;
    }
}
```

第一次运行为： Homer Simpson ，第二次运行为：  Bart Simpson

注意到`setName()`的参数现在是一个数组。一开始，把`fullname`数组传进去，然后，修改`fullname`数组的内容，结果发现，实例`p`的字段`p.name`也被修改了！

**结论**：引用类型参数的传递，调用方的变量，和接收方的参数变量，指向的是同一个对象。双方任意一方对这个对象的修改，都会影响对方（因为指向同一个对象嘛）。

### 2.4 构造方法

 创建实例的时候，我们经常需要同时初始化这个实例的字段，例如：

```java
Person ming = new Person();
ming.setName("小明");
ming.setAge(12);
```

初始化对象实例需要3行代码，而且，如果忘了调用`setName()`或者`setAge()`，这个实例内部的状态就是不正确的。

能否在创建对象实例时就把内部字段全部初始化为合适的值？完全可以。这时，我们就需要构造方法。创建实例的时候，实际上是通过构造方法来初始化实例的。我们先来定义一个构造方法，能在创建`Person`实例的时候，一次性传入`name`和`age`，完成初始化：

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person("Xiao Ming", 15);
        System.out.println(p.getName());
        System.out.println(p.getAge());
    }
}

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }
}

```

由于构造方法是如此特殊，所以`构造方法的名称`就是`类名`。构造方法的参数没有限制，在方法内部，也可以编写任意语句。但是，和普通方法相比，`构造方法没有返回值`（也没有`void`），调用构造方法，必须用`new`操作符。

#### 2.4.1 默认构造方法

任何`class`都有构造方法。那前面我们并没有为`Person`类编写构造方法，为什么可以调用`new Person()`？

原因是如果一个类没有定义构造方法，编译器会自动为我们生成一个默认构造方法，它没有参数，也没有执行语句，类似这样：

```java
class Person {
    public Person() {
    }
}
```

要特别注意的是，如果我们自定义了一个构造方法，那么，编译器就不再自动创建默认构造方法：

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person(); // 编译错误:找不到这个构造方法,不带参数的为默认构造方法
    }
}

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }
}

```

如果既要能使用带参数的构造方法，又想保留不带参数的构造方法，那么只能把两个构造方法都定义出来：

```java
class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }
}
```

没有在构造方法中初始化字段时，引用类型的字段默认是`null`，数值类型的字段用默认值，`int`类型默认值是`0`，布尔类型默认值是`false`：

```java
class Person {
    private String name; // 默认初始化为null
    private int age; // 默认初始化为0

    public Person() {
    }
}
```

 也可以对字段直接进行初始化：

```java
class Person {
    private String name = "Unamed";
    private int age = 10;
}
```

那么问题来了：既对字段进行初始化，又在构造方法中对字段进行初始化：

```java
class Person {
    private String name = "Unamed";
    private int age = 10;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

当我们创建对象的时候，`new Person("Xiao Ming", 12)`得到的对象实例，字段的初始值是啥？

在Java中，创建对象实例的时候，按照如下顺序进行初始化：

1. 先初始化字段，例如，`int age = 10;`表示字段初始化为`10`，`double salary;`表示字段默认初始化为`0`，`String name;`表示引用类型字段默认初始化为`null`；
2. 执行构造方法的代码进行初始化。

因此，构造方法的代码由于后运行，所以，`new Person("Xiao Ming", 12)`的字段值最终由构造方法的代码确定。

#### 2.4.2 多构造方法

可以定义多个构造方法，在通过`new`操作符调用的时候，编译器通过构造方法的参数数量、位置和类型自动区分：

```java
class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person(String name) {
        this.name = name;
        this.age = 12;
    }

    public Person() {
    }
}
```

如果调用`new Person("Xiao Ming", 20);`，会自动匹配到构造方法`public Person(String, int)`。

如果调用`new Person("Xiao Ming");`，会自动匹配到构造方法`public Person(String)`。

如果调用`new Person();`，会自动匹配到构造方法`public Person()`。

一个构造方法可以调用其他构造方法，这样做的目的是便于代码复用。调用其他构造方法的语法是`this(…)`：

```java
class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person(String name) {
        this(name, 18); // 调用另一个构造方法Person(String, int)
    }

    public Person() {
        this("Unnamed"); // 调用另一个构造方法Person(String)
    }
}
```

### 2.5 方法重载

在一个类中，我们可以定义多个方法。如果有一系列方法，它们的功能都是类似的，只有参数有所不同，那么，可以把这一组方法名做成*同名*方法。例如，在`Hello`类中，定义多个`hello()`方法： 

```java
class Hello {
    public void hello() {
        System.out.println("Hello, world!");
    }

    public void hello(String name) {
        System.out.println("Hello, " + name + "!");
    }

    public void hello(String name, int age) {
        if (age < 18) {
            System.out.println("Hi, " + name + "!");
        } else {
            System.out.println("Hello, " + name + "!");
        }
    }
}
```

这种方法名相同，但各自的参数不同，称为方法重载（`Overload`）。

方法重载的目的是，功能类似的方法使用同一名字，更容易记住，因此，调用起来更简单。

举个例子，`String`类提供了多个重载方法`indexOf()`，可以查找子串：

- `int indexOf(int ch)`：根据字符的Unicode码查找；
- `int indexOf(String str)`：根据字符串查找；
- `int indexOf(int ch, int fromIndex)`：根据字符查找，但指定起始位置；
- `int indexOf(String str, int fromIndex)`根据字符串查找，但指定起始位置。

### 2.6 继承

 在前面的章节中，我们已经定义了`Person`类：

```java
class Person {
    private String name;
    private int age;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
}
```

 现在，假设需要定义一个`Student`类，字段如下：

```java
class Student {
    private String name;
    private int age;
    private int score;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
    public int getScore() { … }
    public void setScore(int score) { … }
}
```

仔细观察，发现`Student`类包含了`Person`类已有的字段和方法，只是多出了一个`score`字段和相应的`getScore()`、`setScore()`方法。

能不能在`Student`中不要写重复的代码？

这个时候，继承就派上用场了。

继承是面向对象编程中非常强大的一种机制，它首先可以复用代码。当我们让`Student`从`Person`继承时，`Student`就获得了`Person`的所有功能，我们只需要为`Student`编写新增的功能。

Java使用`extends`关键字来实现继承：

```java
class Person {
    private String name;
    private int age;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
}

class Student extends Person {
    // 不要重复name和age字段/方法,
    // 只需要定义新增score字段/方法:
    private int score;

    public int getScore() { … }
    public void setScore(int score) { … }
}
```

可见，通过继承，`Student`只需要编写额外的功能，不再需要重复代码。

在OOP的术语中，我们把`Person`称为超类（super class），父类（parent class），基类（base class），把`Student`称为子类（subclass），扩展类（extended class）。

#### 2.6.1 继承树

注意到我们在定义`Person`的时候，没有写`extends`。在Java中，没有明确写`extends`的类，编译器会自动加上`extends Object`。所以，任何类，除了`Object`，都会继承自某个类。下图是`Person`、`Student`的继承树：

```ascii
┌───────────┐
│  Object   │
└───────────┘
      ▲
      │
┌───────────┐
│  Person   │
└───────────┘
      ▲
      │
┌───────────┐
│  Student  │
└───────────┘
```

Java只允许一个class继承自一个类，因此，一个类有且仅有一个父类。只有`Object`特殊，它没有父类。

类似的，如果我们定义一个继承自`Person`的`Teacher`，它们的继承树关系如下：

```ascii
       ┌───────────┐
       │  Object   │
       └───────────┘
             ▲
             │
       ┌───────────┐
       │  Person   │
       └───────────┘
          ▲     ▲
          │     │
          │     │
┌───────────┐ ┌───────────┐
│  Student  │ │  Teacher  │
└───────────┘ └───────────┘
```

#### 2.6.2 protected

继承有个特点，就是子类无法访问父类的`private`字段或者`private`方法。例如，`Student`类就无法访问`Person`类的`name`和`age`字段：

```java
class Person {
    private String name;
    private int age;
}

class Student extends Person {
    public String hello() {
        return "Hello, " + name; // 编译错误：无法访问name字段
    }
}
```

这使得继承的作用被削弱了。为了让子类可以访问父类的字段，我们需要把`private`改为`protected`。用`protected`修饰的字段可以被子类访问：

```java
class Person {
    protected String name;
    protected int age;
}

class Student extends Person {
    public String hello() {
        return "Hello, " + name; // OK!
    }
}
```

因此，`protected`关键字可以把字段和方法的访问权限控制在继承树内部，一个`protected`字段和方法可以被其子类，以及子类的子类所访问。

#### 2.6.3 super

`super`关键字表示父类（超类）。子类引用父类的字段时，可以用`super.fieldName`。例如：

```java
class Student extends Person {
    public String hello() {
        return "Hello, " + super.name;
    }
}
```

实际上，这里使用`super.name`，或者`this.name`，或者`name`，效果都是一样的。编译器会自动定位到父类的`name`字段。  

但是，在某些时候，就必须使用`super`。我们来看一个例子：

```java
public class Main {
    public static void main(String[] args) {
        Student s = new Student("Xiao Ming", 12, 89);
    }
}

class Person {
    protected String name;
    protected int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(name, age);
this.score = score;
    }
}

```

运行上面的代码，会得到一个编译错误，大意是在`Student`的构造方法中，无法调用`Person`的构造方法。

这是因为在Java中，任何`class`的构造方法，第一行语句必须是调用父类的构造方法。如果没有明确地调用父类的构造方法，编译器会帮我们自动加一句`super();`，所以，`Student`类的构造方法实际上是这样：

```java
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(); // 自动调用父类的构造方法
        this.score = score;
    }
}
```

但是，`Person`类并没有无参数的构造方法，因此，编译失败。

解决方法是调用`Person`类存在的某个构造方法。例如：

```java
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(name, age); // 调用父类的构造方法Person(String, int)
        this.score = score;
    }
}
```

这样就可以正常编译了！

因此我们得出结论：如果父类没有默认的构造方法，子类就必须显式调用`super()`并给出参数以便让编译器定位到父类的一个合适的构造方法。

这里还顺带引出了另一个问题：`即子类不会继承任何父类的构造方法`。子类默认的构造方法是编译器自动生成的，不是继承的。

#### 2.6.4 向上转型

如果一个引用变量的类型是`Student`，那么它可以指向一个`Student`类型的实例：

```java
Student s = new Student();
```

如果一个引用类型的变量是`Person`，那么它可以指向一个`Person`类型的实例：

```java
Person p = new Person();
```

现在问题来了：如果`Student`是从`Person`继承下来的，那么，一个引用类型为`Person`的变量，能否指向`Student`类型的实例？

```java
Person p = new Student(); //???
```

测试一下就可以发现，这种指向是允许的！

这是因为`Student`继承自`Person`，因此，它拥有`Person`的全部功能。`Person`类型的变量，如果指向`Student`类型的实例，对它进行操作，是没有问题的！

这种把一个子类类型安全地变为父类类型的赋值，被称为向上转型（upcasting）。

向上转型实际上是把一个子类型安全地变为更加抽象的父类型：

```java
Student s = new Student();
Person p = s; // upcasting, ok
Object o1 = p; // upcasting, ok
Object o2 = s; // upcasting, ok
```

注意到继承树是`Student > Person > Object`，所以，可以把`Student`类型转型为`Person`，或者更高层次的`Object`。

#### 2.6.5 向下转型

和向上转型相反，如果把一个父类类型强制转型为子类类型，就是向下转型（downcasting）。例如：

```java
Person p1 = new Student(); // upcasting, ok
Person p2 = new Person();
Student s1 = (Student) p1; // ok
Student s2 = (Student) p2; // runtime error! ClassCastException!
```

如果测试上面的代码，可以发现：

`Person`类型`p1`实际指向`Student`实例，`Person`类型变量`p2`实际指向`Person`实例。在向下转型的时候，把`p1`转型为`Student`会成功，因为`p1`确实指向`Student`实例，把`p2`转型为`Student`会失败，因为`p2`的实际类型是`Person`，不能把父类变为子类，因为子类功能比父类多，多的功能无法凭空变出来。

因此，向下转型很可能会失败。失败的时候，Java虚拟机会报`ClassCastException`。

为了避免向下转型出错，Java提供了`instanceof`操作符，可以先判断一个实例究竟是不是某种类型：

```java
Person p = new Person();
System.out.println(p instanceof Person); // true
System.out.println(p instanceof Student); // false

Student s = new Student();
System.out.println(s instanceof Person); // true
System.out.println(s instanceof Student); // true

Student n = null;
System.out.println(n instanceof Student); // false
```

`instanceof`实际上判断一个变量所指向的实例是否是指定类型，或者这个类型的子类。如果一个引用变量为`null`，那么对任何`instanceof`的判断都为`false`。

利用`instanceof`，在向下转型前可以先判断：

```java
Person p = new Student();
if (p instanceof Student) {
    // 只有判断成功才会向下转型:
    Student s = (Student) p; // 一定会成功
}
```

#### 2.6.6 区分继承和组合

在使用继承时，我们要注意逻辑一致性。

考察下面的`Book`类：

```java
class Book {
    protected String name;
    public String getName() {...}
    public void setName(String name) {...}
}
```

 这个`Book`类也有`name`字段，那么，我们能不能让`Student`继承自`Book`呢？

```java
class Student extends Book {
    protected int score;
}
```

显然，从逻辑上讲，这是不合理的，`Student`不应该从`Book`继承，而应该从`Person`继承。

究其原因，是因为`Student`是`Person`的一种，它们是is关系，而`Student`并不是`Book`。实际上`Student`和`Book`的关系是has关系。

具有has关系不应该使用继承，而是使用组合，即`Student`可以持有一个`Book`实例：

```java
class Student extends Person {
    protected Book book;
    protected int score;
}
```

 因此，继承是is关系，组合是has关系。

#### 2.6.7 小结

- 继承是面向对象编程的一种强大的代码复用方式；

- Java只允许单继承，所有类最终的根类是`Object`；

- `protected`允许子类访问父类的字段和方法；

- 子类的构造方法可以通过`super()`调用父类的构造方法；

- 可以安全地向上转型为更抽象的类型；

- 可以强制向下转型，最好借助`instanceof`判断；

- 子类和父类的关系是is，has关系不能用继承。

### 2.7 多态

在继承关系中，子类如果定义了一个与父类方法签名完全相同的方法，被称为覆写（Override）。

例如，在`Person`类中，我们定义了`run()`方法：

```java
class Person {
    public void run() {
        System.out.println("Person.run");
    }
}
```

 在子类`Student`中，覆写这个`run()`方法：

```java
class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

Override和Overload不同的是，如果方法签名如果不同，就是Overload，Overload方法是一个新方法；如果方法签名相同，并且返回值也相同，就是`Override`。

**注意：方法名相同，方法参数相同，但方法返回值不同，也是不同的方法。在Java程序中，出现这种情况，编译器会报错。**

```java
class Person {
    public void run() { … }
}

class Student extends Person {
    // 不是Override，因为参数不同:
    public void run(String s) { … }
    // 不是Override，因为返回值不同:
    public int run() { … }
}
```

加上`@Override`可以让编译器帮助检查是否进行了正确的覆写。希望进行覆写，但是不小心写错了方法签名，编译器会报错。

```java
public class Main {
    public static void main(String[] args) {
    }
}

class Person {
    public void run() {}
}

public class Student extends Person {
    @Override // Compile error!
    public void run(String s) {}  //Override错误，父类方法无参数，但子类方法有参数String s
}

```

但是`@Override`不是必需的。

在上一节中，我们已经知道，引用变量的声明类型可能与其实际类型不符，例如：

```java
Person p = new Student();
```

 现在，我们考虑一种情况，如果子类覆写了父类的方法：

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run(); // 应该打印Person.run还是Student.run?
    }
}

class Person {
    public void run() {
        System.out.println("Person.run");
    }
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

那么，一个实际类型为`Student`，引用类型为`Person`的变量，调用其`run()`方法，调用的是`Person`还是`Student`的`run()`方法？

运行一下上面的代码就可以知道，实际上调用的方法是`Student`的`run()`方法。因此可得出结论：**Java的实例方法调用是基于运行时的实际类型的动态调用，而非变量的声明类型。**

这个非常重要的特性在面向对象编程中称之为`多态`。它的英文拼写非常复杂：Polymorphic。

#### 2.7.1 多态

多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法。例如：

```java
Person p = new Student();
p.run(); // 无法确定运行时究竟调用哪个run()方法
```

有童鞋会问，从上面的代码一看就明白，肯定调用的是`Student`的`run()`方法啊。

但是，假设我们编写这样一个方法：

```java
public void runTwice(Person p) {
    p.run();
    p.run();
}
```

它传入的参数类型是`Person`，我们是无法知道传入的参数实际类型究竟是`Person`，还是`Student`，还是`Person`的其他子类，因此，也无法确定调用的是不是`Person`类定义的`run()`方法。

所以，`多态的特性就是，运行期才能动态决定调用的子类方法。`对某个类型调用某个方法，执行的实际方法可能是某个子类的覆写方法。这种不确定性的方法调用，究竟有什么作用？

假设我们定义一种收入，需要给它报税，那么先定义一个`Income`类：

```java
class Income {
    protected double income;
    public double getTax() {
        return income * 0.1; // 税率10%
    }
}
```

对于工资收入，可以减去一个基数，那么我们可以从`Income`派生出`SalaryIncome`，并覆写`getTax()`：

```java
class Salary extends Income {
    @Override
    public double getTax() {
        if (income <= 5000) {
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}
```

如果你享受国务院特殊津贴，那么按照规定，可以全部免税：

```java
class StateCouncilSpecialAllowance extends Income {
    @Override
    public double getTax() {
        return 0;
    }
}
```

现在，我们要编写一个报税的财务软件，对于一个人的所有收入进行报税，可以这么写：

```java
public double totalTax(Income... incomes) {
    double total = 0;
    for (Income income: incomes) {
        total = total + income.getTax();
    }
    return total;
}
```

来试一下：

```java
public class Main {
    public static void main(String[] args) {
        // 给一个有普通收入、工资收入和享受国务院特殊津贴的小伙伴算税:
        Income[] incomes = new Income[] {
            new Income(3000),
            new Salary(7500),
            new StateCouncilSpecialAllowance(15000)
        };
        System.out.println(totalTax(incomes));
    }

    public static double totalTax(Income... incomes) {
        double total = 0;
        for (Income income: incomes) {
            total = total + income.getTax();
        }
        return total;
    }
}

class Income {
    protected double income;

    public Income(double income) {
        this.income = income;
    }

    public double getTax() {
        return income * 0.1; // 税率10%
    }
}

class Salary extends Income {
    public Salary(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        if (income <= 5000) {
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}

class StateCouncilSpecialAllowance extends Income {
    public StateCouncilSpecialAllowance(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        return 0;
    }
}

```

观察`totalTax()`方法：利用多态，`totalTax()`方法只需要和`Income`打交道，它完全不需要知道`Salary`和`StateCouncilSpecialAllowance`的存在，就可以正确计算出总的税。如果我们要新增一种稿费收入，只需要从`Income`派生，然后正确覆写`getTax()`方法就可以。把新的类型传入`totalTax()`，不需要修改任何代码。

可见，多态具有一个非常强大的功能，就是允许添加更多类型的子类实现功能扩展，却不需要修改基于父类的代码。

#### 2.7.2 覆写Object方法

因为所有的`class`最终都继承自`Object`，而`Object`定义了几个重要的方法：

- `toString()`：把instance输出为`String`；
- `equals()`：判断两个instance是否逻辑相等；
- `hashCode()`：计算一个instance的哈希值。
在必要的情况下，我们可以覆写`Object`的这几个方法。例如：

```java
class Person {
    ...
    // 显示更有意义的字符串:
    @Override
    public String toString() {
        return "Person:name=" + name;
    }

    // 比较是否相等:
    @Override
    public boolean equals(Object o) {
        // 当且仅当o为Person类型:
        if (o instanceof Person) {
            Person p = (Person) o;
            // 并且name字段相同时，返回true:
            return this.name.equals(p.name);
        }
        return false;
    }

    // 计算hash:
    @Override
    public int hashCode() {
        return this.name.hashCode();
    }
}
```

#### 2.7.3 调用super

在子类的覆写方法中，如果要调用父类的被覆写的方法，可以通过`super`来调用。例如：

```java
class Person {
    protected String name;
    public String hello() {
        return "Hello, " + name;
    }
}

Student extends Person {
    @Override
    public String hello() {
        // 调用父类的hello()方法:
        return super.hello() + "!";
    }
}
```

#### 2.7.4 final

继承可以允许子类覆写父类的方法。如果一个父类不允许子类对它的某个方法进行覆写，可以把该方法标记为`final`。用`final`修饰的方法不能被`Override`：

```java
class Person {
    protected String name;
    public final String hello() {
        return "Hello, " + name;
    }
}

Student extends Person {
    // compile error: 不允许覆写
    @Override
    public String hello() {
    }
}
```

如果一个类不希望任何其他类继承自它，那么可以把这个类本身标记为`final`。用`final`修饰的类不能被继承：

```java
final class Person {
    protected String name;
}

// compile error: 不允许继承自Person
Student extends Person {
}
```

对于一个类的实例字段，同样可以用`final`修饰。用`final`修饰的字段在初始化后不能被修改。例如：

```java
class Person {
    public final String name = "Unamed";
}
```

 对`final`字段重新赋值会报错：

```java
Person p = new Person();
p.name = "New Name"; // compile error!
```

 可以在构造方法中初始化final字段：

```java
class Person {
    public final String name;
    public Person(String name) {
        this.name = name;
    }
}
```

这种方法更为常用，因为可以保证实例一旦创建，其`final`字段就不可修改。

#### 2.7.5 小结

- 子类可以覆写父类的方法（Override），覆写在子类中改变了父类方法的行为；
- Java的方法调用总是作用于运行期对象的实际类型，这种行为称为多态；
- `final`修饰符有多种作用：
  - `final`修饰的方法可以阻止被覆写；
  - `final`修饰的class可以阻止被继承；
  - `final`修饰的field必须在创建对象时初始化，随后不可修改。

### 2.8 抽象类

由于多态的存在，每个子类都可以覆写父类的方法，例如：

```java
class Person {
    public void run() { … }
}

class Student extends Person {
    @Override
    public void run() { … }
}

class Teacher extends Person {
    @Override
    public void run() { … }
}
```

从`Person`类派生的`Student`和`Teacher`都可以覆写`run()`方法。

如果父类`Person`的`run()`方法没有实际意义，能否去掉方法的执行语句？

```java
class Person {
    public void run(); // Compile Error!
}
```

答案是不行，会导致编译错误，因为定义方法的时候，必须实现方法的语句。

能不能去掉父类的`run()`方法？

答案还是不行，因为去掉父类的`run()`方法，就失去了多态的特性。例如，`runTwice()`就无法编译：

```java
public void runTwice(Person p) {
    p.run(); // Person没有run()方法，会导致编译错误
    p.run();
}
```

如果父类的方法本身不需要实现任何功能，仅仅是为了定义方法签名，目的是让子类去覆写它，那么，可以把父类的方法声明为抽象方法：

```java
class Person {
    public abstract void run(); //加上abstract关键字
}
```

把一个方法声明为`abstract`，表示它是一个抽象方法，本身没有实现任何方法语句。因为这个抽象方法本身是无法执行的，所以，`Person`类也无法被实例化。编译器会告诉我们，无法编译`Person`类，因为它包含抽象方法。

必须把`Person`类本身也声明为`abstract`，才能正确编译它：

```java
abstract class Person {
    public abstract void run();
}
```

#### 2.8.1 抽象类

如果一个`class`定义了方法，但没有具体执行代码，这个方法就是抽象方法，抽象方法用`abstract`修饰。

**因为无法执行抽象方法，因此这个类也必须申明为抽象类**（abstract class）。

使用`abstract`修饰的类就是抽象类。我们无法实例化一个抽象类：

```java
Person p = new Person(); // 编译错误
```

无法实例化的抽象类有什么用？

因为抽象类本身被设计成只能用于被继承，因此，抽象类可以强迫子类实现其定义的抽象方法，否则编译会报错。因此，抽象方法实际上相当于定义了“规范”。

例如，`Person`类定义了抽象方法`run()`，那么，在实现子类`Student`的时候，就必须覆写`run()`方法：

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run();
    }
}

abstract class Person {
    public abstract void run();
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}

```

#### 2.8.2 面向抽象编程

当我们定义了抽象类`Person`，以及具体的`Student`、`Teacher`子类的时候，我们可以通过抽象类`Person`类型去引用具体的子类的实例：

```java
Person s = new Student(); //尽量使用Person类型
Person t = new Teacher();//尽量使用Person类型
```

这种引用抽象类的好处在于，我们对其进行方法调用，并不关心`Person`类型变量的具体子类型：

```java
// 不关心Person变量的具体子类型:
s.run();
t.run();
```

 同样的代码，如果引用的是一个新的子类，我们仍然不关心具体类型：

```java
// 同样不关心新的子类是如何实现run()方法的：
Person e = new Employee();//尽量使用Person类型
e.run();
```

这种**尽量引用高层类型，避免引用实际子类型的方式，称之为**`面向抽象编程`。

面向抽象编程的本质就是：

- 上层代码只定义规范（例如：`abstract class Person`）；
- 不需要子类就可以实现业务逻辑（正常编译）；
- 具体的业务逻辑由不同的子类实现，调用者并不关心。

#### 2.8.3 小结

- 通过`abstract`定义的方法是抽象方法，它只有定义，没有实现。抽象方法定义了子类必须实现的接口规范；
- 定义了抽象方法的class必须被定义为抽象类，从抽象类继承的子类必须实现抽象方法；
- 如果不实现抽象方法，则该子类仍是一个抽象类；
- 面向抽象编程使得调用者只关心抽象方法的定义，不关心子类的具体实现。

### 2.9 接口

在抽象类中，**抽象方法本质上是定义接口规范**：即规定高层类的接口，从而保证所有子类都有相同的接口实现，这样，多态就能发挥出威力。

如果一个抽象类没有字段，所有方法全部都是抽象方法：

```java
abstract class Person {
    public abstract void run();
    public abstract String getName();
}
```

就可以把该抽象类改写为接口：`interface`。

在Java中，使用`interface`可以声明一个接口：

```java
interface Person {
    void run();
    String getName();
}
```

所谓`interface`，就是比抽象类还要抽象的纯抽象接口，`因为它连字段都不能有`。因为接口定义的所有方法默认都是`public abstract`的，所以这两个修饰符不需要写出来（写不写效果都一样）。

当一个具体的`class`去实现一个`interface`时，需要使用`implements`关键字。举个例子：  

```java
class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        System.out.println(this.name + " run");
    }

    @Override
    public String getName() {
        return this.name;
    }
}
```

我们知道，在Java中，一个类只能继承自另一个类，不能从多个类继承。但是，一个类可以实现多个`interface`，例如：

```java
class Student implements Person, Hello { // 实现了两个interface
    ...
}
```

#### 2.9.1 术语

注意区分术语：

Java的接口特指`interface`的定义，表示一个接口类型和一组方法签名，而编程接口泛指接口规范，如方法签名，数据格式，网络协议等。

抽象类和接口的对比如下：

|            | abstract class       | interface                   |
| :--------- | :------------------- | --------------------------- |
| 继承       | 只能extends一个class | 可以implements多个interface |
| 字段       | 可以定义实例字段     | 不能定义实例字段            |
| 抽象方法   | 可以定义抽象方法     | 可以定义抽象方法            |
| 非抽象方法 | 可以定义非抽象方法   | 可以定义default方法         |

#### 2.9.2 接口继承

一个`interface`可以继承自另一个`interface`。`interface`继承自`interface`使用`extends`，它相当于扩展了接口的方法。例如：

```java
interface Hello {
    void hello();
}

interface Person extends Hello {
    void run();
    String getName();
}
```

此时，`Person`接口继承自`Hello`接口，因此，`Person`接口现在实际上有3个抽象方法签名，其中一个来自继承的`Hello`接口。

#### 2.9.3 继承关系

合理设计`interface`和`abstract class`的继承关系，可以充分复用代码。一般来说，公共逻辑适合放在`abstract class`中，具体逻辑放到各个子类，而接口层次代表抽象程度。可以参考Java的集合类定义的一组接口、抽象类以及具体子类的继承关系： 

```ascii
┌───────────────┐
│   Iterable    │
└───────────────┘
        ▲                ┌───────────────────┐
        │                │      Object       │
┌───────────────┐        └───────────────────┘
│  Collection   │                  ▲
└───────────────┘                  │
        ▲     ▲          ┌───────────────────┐
        │     └──────────│AbstractCollection │
┌───────────────┐        └───────────────────┘
│     List      │                  ▲
└───────────────┘                  │
              ▲          ┌───────────────────┐
              └──────────│   AbstractList    │
                         └───────────────────┘
                                ▲     ▲
                                │     │
                                │     │
                     ┌────────────┐ ┌────────────┐
                     │ ArrayList  │ │ LinkedList │
                     └────────────┘ └────────────┘
```

在使用的时候，实例化的对象永远只能是某个具体的子类，但总是通过接口去引用它，因为接口比抽象类更抽象：

```java
List list = new ArrayList(); // 用List接口引用具体子类的实例
Collection coll = list; // 向上转型为Collection接口
Iterable it = coll; // 向上转型为Iterable接口
```

#### 2.9.4 default方法

在接口中，可以定义`default`方法。例如，把`Person`接口的`run()`方法改为`default`方法：

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Student("Xiao Ming");
        p.run();
    }
}

interface Person {
    String getName();
    default void run() {
        System.out.println(getName() + " run");
    }
}

class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }
}
```

实现类可以不必覆写`default`方法。`default`方法的目的是，当我们需要给接口新增一个方法时，会涉及到修改全部子类。如果新增的是`default`方法，那么子类就不必全部修改，只需要在需要覆写的地方去覆写新增方法。

`default`方法和抽象类的普通方法是有所不同的。因为`interface`没有字段，`default`方法无法访问字段，而抽象类的普通方法可以访问实例字段。

#### 2.9.5 小结

Java的接口（interface）定义了纯抽象规范，一个类可以实现多个接口；

接口也是数据类型，适用于向上转型和向下转型；

接口的所有方法都是抽象方法，接口不能定义实例字段；

接口可以定义`default`方法（JDK>=1.8）。

### 2.10 静态字段和静态方法

在一个`class`中定义的字段，我们称之为实例字段。实例字段的特点是，每个实例都有独立的字段，各个实例的同名字段互不影响。

#### 2.10.1 静态字段

还有一种字段，是用`static`修饰的字段，称为静态字段：`static field`。

实例字段在每个实例中都有自己的一个独立“空间”，但是静态字段只有一个共享“空间”，所有实例都会共享该字段。举个例子：

```java
class Person {
    public String name;
    public int age;
    // 定义静态字段number:
    public static int number;
}
```

我们来看看下面的代码：

```java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person("Xiao Ming", 12);
        Person hong = new Person("Xiao Hong", 15);
        ming.number = 88;
        System.out.println(hong.number);
        hong.number = 99;
        System.out.println(ming.number);
    }
}

class Person {
    public String name;
    public int age;

    public static int number;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

```

对于静态字段，无论修改哪个实例的静态字段，效果都是一样的：所有实例的静态字段都被修改了，原因是静态字段并不属于实例：

```ascii
        ┌──────────────────┐
ming ──>│Person instance   │
        ├──────────────────┤
        │name = "Xiao Ming"│
        │age = 12          │
        │number ───────────┼──┐    ┌─────────────┐
        └──────────────────┘  │    │Person class │
                              │    ├─────────────┤
                              ├───>│number = 99  │
        ┌──────────────────┐  │    └─────────────┘
hong ──>│Person instance   │  │
        ├──────────────────┤  │
        │name = "Xiao Hong"│  │
        │age = 15          │  │
        │number ───────────┼──┘
        └──────────────────┘
```

虽然实例可以访问静态字段，但是它们指向的其实都是`Person class`的静态字段。所以，所有实例共享一个静态字段。

因此，不推荐用`实例变量.静态字段`去访问静态字段，因为在Java程序中，实例对象并没有静态字段。在代码中，实例对象能访问静态字段只是因为编译器可以根据实例类型自动转换为`类名.静态字段`来访问静态对象。

推荐用类名来访问静态字段。可以把静态字段理解为描述`class`本身的字段（非实例字段）。对于上面的代码，更好的写法是：

```java
Person.number = 99;
System.out.println(Person.number);
```

#### 2.10.2 静态方法

有静态字段，就有静态方法。用`static`修饰的方法称为静态方法。

调用实例方法必须通过一个实例变量，而调用静态方法则不需要实例变量，通过类名就可以调用。静态方法类似其它编程语言的函数。例如：

```java
public class Main {
    public static void main(String[] args) {
        Person.setNumber(99);
        System.out.println(Person.number);
    }
}

class Person {
    public static int number;

    public static void setNumber(int value) {
        number = value;
    }
}
```

因为静态方法属于`class`而不属于实例，因此，静态方法内部，无法访问`this`变量，也无法访问实例字段，它只能访问静态字段。

通过实例变量也可以调用静态方法，但这只是编译器自动帮我们把实例改写成类名而已。

通常情况下，通过实例变量访问静态字段和静态方法，会得到一个编译警告。

静态方法经常用于工具类。例如：

- Arrays.sort()
- Math.random()

静态方法也经常用于辅助方法。注意到Java程序的入口`main()`也是静态方法。

#### 2.10.3 接口的静态字段

因为`interface`是一个纯抽象类，所以它不能定义实例字段。但是，`interface`是可以有静态字段的，并且静态字段必须为`final`类型：

```java
public interface Person {
    public static final int MALE = 1;
    public static final int FEMALE = 2;
}
```

实际上，因为`interface`的字段只能是`public static final`类型，所以我们可以把这些修饰符都去掉，上述代码可以简写为：

```java
public interface Person {
    // 编译器会自动加上public statc final:
    int MALE = 1;
    int FEMALE = 2;
}
```

编译器会自动把该字段变为`public static final`类型。

#### 2.10.4 小结

- 静态字段属于所有实例“共享”的字段，实际上是属于`class`的字段；
- 调用静态方法不需要实例，无法访问`this`，但可以访问静态字段和其他静态方法；
- 静态方法常用于工具类和辅助方法。

### 2.11 包（package）

在前面的代码中，我们把类和接口命名为`Person`、`Student`、`Hello`等简单名字。

在现实中，如果小明写了一个`Person`类，小红也写了一个`Person`类，现在，小白既想用小明的`Person`，也想用小红的`Person`，怎么办？

如果小军写了一个`Arrays`类，恰好JDK也自带了一个`Arrays`类，如何解决类名冲突？

在Java中，我们使用`package`来解决名字冲突。

Java定义了一种名字空间，称之为包：`package`。一个类总是属于某个包，类名（比如`Person`）只是一个简写，真正的完整类名是`包名.类名`。

例如：

小明的`Person`类存放在包`ming`下面，因此，完整类名是`ming.Person`；

小红的`Person`类存放在包`hong`下面，因此，完整类名是`hong.Person`；

小军的`Arrays`类存放在包`mr.jun`下面，因此，完整类名是`mr.jun.Arrays`；

JDK的`Arrays`类存放在包`java.util`下面，因此，完整类名是`java.util.Arrays`。

在定义`class`的时候，我们需要在第一行声明这个`class`属于哪个包。

小明的`Person.java`文件：

```java
package ming; // 申明包名ming

public class Person {
}
```

 小军的`Arrays.java`文件：

```java
package mr.jun; // 申明包名mr.jun

public class Arrays {
}
```

在Java虚拟机执行的时候，JVM只看完整类名，因此，只要包名不同，类就不同。

包可以是多层结构，用`.`隔开。例如：`java.util`。

**要特别注意：包没有父子关系。java.util和java.util.zip是不同的包，两者没有任何继承关系。**

没有定义包名的`class`，它使用的是默认包，非常容易引起名字冲突，因此，不推荐不写包名的做法。

我们还需要按照包结构把上面的Java文件组织起来。假设以`package_sample`作为根目录，`src`作为源码目录，那么所有文件结构就是：

```ascii
package_sample
└─ src
    ├─ hong
    │  └─ Person.java
    │  ming
    │  └─ Person.java
    └─ mr
       └─ jun
          └─ Arrays.java
```

即所有Java文件对应的目录层次要和包的层次一致。

编译后的`.class`文件也需要按照包结构存放。如果使用IDE，把编译后的`.class`文件放到`bin`目录下，那么，编译的文件结构就是：

```ascii
package_sample
└─ bin
   ├─ hong
   │  └─ Person.class
   │  ming
   │  └─ Person.class
   └─ mr
      └─ jun
         └─ Arrays.class
```

编译的命令相对比较复杂，我们需要在`src`目录下执行`javac`命令：

```shell
javac -d ../bin ming/Person.java hong/Person.java mr/jun/Arrays.java
```

在IDE中，会自动根据包结构编译所有Java源码，所以不必担心使用命令行编译的复杂命令。

#### 2.11.1 包作用域

位于同一个包的类，可以访问包作用域的字段和方法。不用`public`、`protected`、`private`修饰的字段和方法就是包作用域。例如，`Person`类定义在`hello`包下面：

```java
package hello;

public class Person {
    // 包作用域:
    void hello() {
        System.out.println("Hello!");
    }
}
```

 `Main`类也定义在`hello`包下面： 

```java
package hello;

public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        p.hello(); // 可以调用，因为Main和Person在同一个包
    }
}
```

#### 2.11.2 import

在一个`class`中，我们总会引用其他的`class`。例如，小明的`ming.Person`类，如果要引用小军的`mr.jun.Arrays`类，他有三种写法：

第一种，直接写出完整类名，例如：

```java
// Person.java
package ming;

public class Person {
    public void run() {
        mr.jun.Arrays arrays = new mr.jun.Arrays();
    }
}
```

很显然，每次写完整类名比较痛苦。

因此，第二种写法是用`import`语句，导入小军的`Arrays`，然后写简单类名：

```java
// Person.java
package ming;

// 导入完整类名:
import mr.jun.Arrays;

public class Person {
    public void run() {
        Arrays arrays = new Arrays();
    }
}
```

在写`import`的时候，可以使用`*`，表示把这个包下面的所有`class`都导入进来（但不包括子包的`class`）：

```java
// Person.java
package ming;

// 导入mr.jun包的所有class:
import mr.jun.*;

public class Person {
    public void run() {
        Arrays arrays = new Arrays();
    }
}
```

我们一般不推荐这种写法，因为在导入了多个包后，很难看出`Arrays`类属于哪个包。

还有一种`import static`的语法，它可以导入可以导入一个类的静态字段和静态方法：

```java
package main;

// 导入System类的所有静态字段和静态方法:
import static java.lang.System.*;

public class Main {
    public static void main(String[] args) {
        // 相当于调用System.out.println(…)
        out.println("Hello, world!");
    }
}
```

`import static`很少使用。

Java编译器最终编译出的`.class`文件只使用*完整类名*，因此，在代码中，当编译器遇到一个`class`名称时：

- 如果是完整类名，就直接根据完整类名查找这个`class`；
- 如果是简单类名，按下面的顺序依次查找：
  - 查找当前`package`是否存在这个`class`；
  - 查找`import`的包是否包含这个`class`；
  - 查找`java.lang`包是否包含这个`class`。

如果按照上面的规则还无法确定类名，则编译报错。

我们来看一个例子：

```java
// Main.java
package test;

import java.text.Format;

public class Main {
    public static void main(String[] args) {
        java.util.List list; // ok，使用完整类名 -> java.util.List
        Format format = null; // ok，使用import的类 -> java.text.Format
        String s = "hi"; // ok，使用java.lang包的String -> java.lang.String
        System.out.println(s); // ok，使用java.lang包的System -> java.lang.System
        MessageFormat mf = null; // 编译错误：无法找到MessageFormat: MessageFormat cannot be resolved to a type
    }
}
```

因此，编写class的时候，编译器会自动帮我们做两个import动作：

- 默认自动`import`当前`package`的其他`class`；
- 默认自动`import java.lang.*`。

**注意：自动导入的是java.lang包，但类似java.lang.reflect这些包仍需要手动导入。**

如果有两个`class`名称相同，例如，`mr.jun.Arrays`和`java.util.Arrays`，那么只能`import`其中一个，另一个必须写完整类名。

#### 2.12.2 最佳实践

为了避免名字冲突，我们需要确定唯一的包名。推荐的做法是使用倒置的域名来确保唯一性。例如：

- org.apache
- org.apache.commons.log
- com.liaoxuefeng.sample

子包就可以根据功能自行命名。

要注意不要和`java.lang`包的类重名，即自己的类不要使用这些名字：

- String
- System
- Runtime
- ...

要注意也不要和JDK常用类重名：

- java.util.List
- java.text.Format
- java.math.BigInteger
- ...

#### 2.12.3 小结

Java内建的`package`机制是为了避免`class`命名冲突；

JDK的核心类使用`java.lang`包，编译器会自动导入；

JDK的其它常用类定义在`java.util.*`，`java.math.*`，`java.text.*`，……；

包名推荐使用倒置的域名，例如`org.apache`。

### 2.13 作用域

在Java中，我们经常看到`public`、`protected`、`private`这些修饰符。在Java中，这些修饰符可以用来限定访问作用域。

#### 2.13.1 public

定义为`public`的`class`、`interface`可以被其他任何类访问：

```java
package abc;

public class Hello {
    public void hi() {
    }
}
```

上面的`Hello`是`public`，因此，可以被其他包的类访问：

```java
package xyz;

class Main {
    void foo() {
        // Main可以访问Hello
        Hello h = new Hello();
    }
}
```

定义为`public`的`field`、`method`可以被其他类访问，前提是首先有访问`class`的权限：

```java
package abc;

public class Hello {
    public void hi() {
    }
}
```

上面的`hi()`方法是`public`，可以被其他类调用，前提是首先要能访问`Hello`类：

```java
package xyz;

class Main {
    void foo() {
        Hello h = new Hello();
        h.hi();
    }
}
```

#### 2.13.2 private

定义为`private`的`field`、`method`无法被其他类访问：

```java
package abc;

public class Hello {
    // 不能被其他类调用:
    private void hi() {
    }

    public void hello() {
        this.hi();
    }
}
```

实际上，确切地说，`private`访问权限被限定在`class`的内部，而且与方法声明顺序*无关*。推荐把`private`方法放到后面，因为`public`方法定义了类对外提供的功能，阅读代码的时候，应该先关注`public`方法：

```java
package abc;

public class Hello {
    public void hello() {
        this.hi();
    }

    private void hi() {
    }
}
```

由于Java支持嵌套类，如果一个类内部还定义了嵌套类，那么，嵌套类拥有访问`private`的权限：

```java
public class Main {
    public static void main(String[] args) {
        Inner i = new Inner();
        i.hi();
    }

    // private方法:
    private static void hello() {
        System.out.println("private hello!");
    }

    // 静态内部类:
    static class Inner {
        public void hi() {
            Main.hello();
        }
    }
}
```

定义在一个`class`内部的`class`称为嵌套类（`nested class`），Java支持好几种嵌套类。

#### 2.13.3 protected

`protected`作用于继承关系。定义为`protected`的字段和方法可以被子类访问，以及子类的子类：

```java
package abc;

public class Hello {
    // protected方法:
    protected void hi() {
    }
}
```

 上面的`protected`方法可以被继承的类访问：

```java
package xyz;

class Main extends Hello {
    void foo() {
        Hello h = new Hello();
        // 可以访问protected方法:
        h.hi();
    }
}
```

#### 2.13.4 package

 最后，包作用域是指一个类允许访问同一个`package`的没有`public`、`private`修饰的`class`，以及没有`public`、`protected`、`private`修饰的字段和方法。

```java
package abc;
// package权限的类:
class Hello {
    // package权限的方法:
    void hi() {
    }
}
```

 只要在同一个包，就可以访问`package`权限的`class`、`field`和`method`：

```java
package abc;

class Main {
    void foo() {
        // 可以访问package权限的类:
        Hello h = new Hello();
        // 可以调用package权限的方法:
        h.hi();
    }
}
```

 注意，包名必须完全一致，包没有父子关系，`com.apache`和`com.apache.abc`是不同的包。

#### 2.13.5 局部变量

 在方法内部定义的变量称为局部变量，局部变量作用域从变量声明处开始到对应的块结束。方法参数也是局部变量。

```java
package abc;

public class Hello {
    void hi(String name) { // ①
        String s = name.toLowerCase(); // ②
        int len = s.length(); // ③
        if (len < 10) { // ④
            int p = 10 - len; // ⑤
            for (int i=0; i<10; i++) { // ⑥
                System.out.println(); // ⑦
            } // ⑧
        } // ⑨
    } // ⑩
}
```

我们观察上面的`hi()`方法代码：

- 方法参数name是局部变量，它的作用域是整个方法，即①～⑩；
- 变量s的作用域是定义处到方法结束，即②～⑩；
- 变量len的作用域是定义处到方法结束，即③～⑩；
- 变量p的作用域是定义处到if块结束，即⑤～⑨；
- 变量i的作用域是for循环，即⑥～⑧。

使用局部变量时，应该尽可能把局部变量的作用域缩小，尽可能延后声明局部变量。

#### 2.13.6 final

Java还提供了一个`final`修饰符。`final`与访问权限不冲突，它有很多作用。

用`final`修饰`class`可以阻止被继承：

```java
package abc;

// 无法被继承:
public final class Hello {
    private int n = 0;
    protected void hi(int t) {
        long i = t;
    }
}
```

 用`final`修饰`method`可以阻止被子类覆写：

```java
package abc;

public class Hello {
    // 无法被覆写:
    protected final void hi() {
    }
}
```

 用`final`修饰`field`可以阻止被重新赋值：

```java
package abc;

public class Hello {
    private final int n = 0;
    protected void hi() {
        this.n = 1; // error!
    }
}
```

用`final`修饰局部变量可以阻止被重新赋值：

```java
package abc;

public class Hello {
    protected void hi(final int t) {
        t = 1; // error!
    }
}
```

#### 2.13.7 最佳实践

如果不确定是否需要`public`，就不声明为`public`，即尽可能少地暴露对外的字段和方法。

把方法定义为`package`权限有助于测试，因为测试类和被测试类只要位于同一个`package`，测试代码就可以访问被测试类的`package`权限方法。

一个`.java`文件只能包含一个`public`类，但可以包含多个非`public`类。如果有`public`类，文件名必须和`public`类的名字相同。

#### 2.13.8 小结

Java内建的访问权限包括`public`、`protected`、`private`和`package`权限；

Java在方法内部定义的变量是局部变量，局部变量的作用域从变量声明开始，到一个块结束；

`final`修饰符不是访问权限，它可以修饰`class`、`field`和`method`；

一个`.java`文件只能包含一个`public`类，但可以包含多个非`public`类。

### 2.14 classpath和jar

#### 2.14.1 classpath

在Java中，我们经常听到`classpath`这个东西。网上有很多关于“如何设置classpath”的文章，但大部分设置都不靠谱。

到底什么是`classpath`？

`classpath`是JVM用到的一个环境变量，它用来指示JVM如何搜索`class`。

因为Java是编译型语言，源码文件是`.java`，而编译后的`.class`文件才是真正可以被JVM执行的字节码。因此，JVM需要知道，如果要加载一个`abc.xyz.Hello`的类，应该去哪搜索对应的`Hello.class`文件。

所以，`classpath`就是一组目录的集合，它设置的搜索路径与操作系统相关。例如，在Windows系统上，用`;`分隔，带空格的目录用`""`括起来，可能长这样：

```shell
C:\work\project1\bin;C:\shared;"D:\My Documents\project1\bin"
```

在Linux系统上，用`:`分隔，可能长这样： 

```shell
/usr/shared:/usr/local/bin:/home/liaoxuefeng/bin
```

现在我们假设`classpath`是`.;C:\work\project1\bin;C:\shared`，当JVM在加载`abc.xyz.Hello`这个类时，会依次查找：

- <当前目录>\abc\xyz\Hello.class
- C:\work\project1\bin\abc\xyz\Hello.class
- C:\shared\abc\xyz\Hello.class

注意到`.`代表当前目录。如果JVM在某个路径下找到了对应的`class`文件，就不再往后继续搜索。如果所有路径下都没有找到，就报错。

`classpath`的设定方法有两种：

在系统环境变量中设置`classpath`环境变量，不推荐；

在启动JVM时设置`classpath`变量，推荐。

我们强烈*不推荐*在系统环境变量中设置`classpath`，那样会污染整个系统环境。在启动JVM时设置`classpath`才是推荐的做法。实际上就是给`java`命令传入`-classpath`或`-cp`参数：

```shell
java -classpath .;C:\work\project1\bin;C:\shared abc.xyz.Hello
```

没有设置系统环境变量，也没有传入`-cp`参数，那么JVM默认的`classpath`为`.`，即当前目录：

```shell
java abc.xyz.Hello
```

上述命令告诉JVM只在当前目录搜索`Hello.class`。

在IDE中运行Java程序，IDE自动传入的`-cp`参数是当前工程的`bin`目录和引入的jar包。

通常，我们在自己编写的`class`中，会引用Java核心库的`class`，例如，`String`、`ArrayList`等。这些`class`应该上哪去找？

有很多“如何设置classpath”的文章会告诉你把JVM自带的`rt.jar`放入`classpath`，但事实上，根本不需要告诉JVM如何去Java核心库查找`class`，JVM怎么可能笨到连自己的核心库在哪都不知道？

**不要把任何Java核心库添加到classpath中！JVM根本不依赖classpath加载核心库！**

更好的做法是，不要设置`classpath`！默认的当前目录`.`对于绝大多数情况都够用了。

#### 2.14.2 jar包

如果有很多`.class`文件，散落在各层目录中，肯定不便于管理。如果能把目录打一个包，变成一个文件，就方便多了。

jar包就是用来干这个事的，它可以把`package`组织的目录层级，以及各个目录下的所有文件（包括`.class`文件和其他文件）都打成一个jar文件，这样一来，无论是备份，还是发给客户，就简单多了。

jar包实际上就是一个zip格式的压缩文件，而jar包相当于目录。如果我们要执行一个jar包的`class`，就可以把jar包放到`classpath`中：

```bash
java -cp ./hello.jar abc.xyz.Hello
```

这样JVM会自动在`hello.jar`文件里去搜索某个类。

那么问题来了：如何创建jar包？

因为jar包就是zip包，所以，直接在资源管理器中，找到正确的目录，点击右键，在弹出的快捷菜单中选择“发送到”，“压缩(zipped)文件夹”，就制作了一个zip文件。然后，把后缀从`.zip`改为`.jar`，一个jar包就创建成功。

假设编译输出的目录结构是这样：

```ascii
package_sample
└─ bin
   ├─ hong
   │  └─ Person.class
   │  ming
   │  └─ Person.class
   └─ mr
      └─ jun
         └─ Arrays.class
```

这里需要特别注意的是，jar包里的第一层目录，不能是`bin`，而应该是`hong`、`ming`、`mr`。如果在Windows的资源管理器中看，应该长这样：

```ascii
package_sample
└─> bin
   ├─ hello.zip
   │  └─> hong
   │  └─> xiao
   └─ mr
      └─> jun
```

 如果长这样：

```ascii
package_sample
└─> bin
└─> hello.zip
    └─> bin
        └─> hong
        └─> xiao
        └─> mr
            └─> jun
```

说明打包打得有问题，JVM仍然无法从jar包中查找正确的`class`，原因是`hong.Person`必须按`hong/Person.class`存放，而不是`bin/hong/Person.class`。

jar包还可以包含一个特殊的`/META-INF/MANIFEST.MF`文件，`MANIFEST.MF`是纯文本，可以指定`Main-Class`和其它信息。JVM会自动读取这个`MANIFEST.MF`文件，如果存在`Main-Class`，我们就不必在命令行指定启动的类名，而是用更方便的命令：

```shell
java -jar hello.jar
```

jar包还可以包含其它jar包，这个时候，就需要在`MANIFEST.MF`文件里配置`classpath`了。

在大型项目中，不可能手动编写`MANIFEST.MF`文件，再手动创建zip包。Java社区提供了大量的开源构建工具，例如[Maven](https://www.liaoxuefeng.com/wiki/1252599548343744/1255945359327200)，可以非常方便地创建jar包。

#### 2.14.3 小结

JVM通过环境变量`classpath`决定搜索`class`的路径和顺序；

不推荐设置系统环境变量`classpath`，始终建议通过`-cp`命令传入；

jar包相当于目录，可以包含很多`.class`文件，方便下载和使用；

`MANIFEST.MF`文件可以提供jar包的信息，如`Main-Class`，这样可以直接运行jar包。

### 2.15 模块

从Java 9开始，JDK又引入了模块（Module）。

什么是模块？这要从Java 9之前的版本说起。

我们知道，`.class`文件是JVM看到的最小可执行文件，而一个大型程序需要编写很多Class，并生成一堆`.class`文件，很不便于管理，所以，`jar`文件就是`class`文件的容器。

在Java 9之前，一个大型Java程序会生成自己的jar文件，同时引用依赖的第三方jar文件，而JVM自带的Java标准库，实际上也是以jar文件形式存放的，这个文件叫`rt.jar`，一共有60多M。

如果是自己开发的程序，除了一个自己的`app.jar`以外，还需要一堆第三方的jar包，运行一个Java程序，一般来说，命令行写这样：

```bash
java -cp app.jar:a.jar:b.jar:c.jar com.liaoxuefeng.sample.Main
```

**注意：JVM自带的标准库rt.jar不要写到classpath中，写了反而会干扰JVM的正常运行。**

如果漏写了某个运行时需要用到的jar，那么在运行期极有可能抛出`ClassNotFoundException`。

所以，jar只是用于存放class的容器，它并不关心class之间的依赖。

从Java 9开始引入的模块，主要是为了解决“依赖”这个问题。如果`a.jar`必须依赖另一个`b.jar`才能运行，那我们应该给`a.jar`加点说明啥的，让程序在编译和运行的时候能自动定位到`b.jar`，这种自带“依赖关系”的class容器就是模块。

为了表明Java模块化的决心，从Java 9开始，原有的Java标准库已经由一个单一巨大的`rt.jar`分拆成了几十个模块，这些模块以`.jmod`扩展名标识，可以在`$JAVA_HOME/jmods`目录下找到它们： 

- java.base.jmod
- java.compiler.jmod
- java.datatransfer.jmod
- java.desktop.jmod
- ...

这些`.jmod`文件每一个都是一个模块，模块名就是文件名。例如：模块`java.base`对应的文件就是`java.base.jmod`。模块之间的依赖关系已经被写入到模块内的`module-info.class`文件了。所有的模块都直接或间接地依赖`java.base`模块，只有`java.base`模块不依赖任何模块，它可以被看作是“根模块”，好比所有的类都是从`Object`直接或间接继承而来。

把一堆class封装为jar仅仅是一个打包的过程，而把一堆class封装为模块则不但需要打包，还需要写入依赖关系，并且还可以包含二进制代码（通常是JNI扩展）。此外，模块支持多版本，即在同一个模块中可以为不同的JVM提供不同的版本。

#### 2.15.1 编写模块

那么，我们应该如何编写模块呢？还是以具体的例子来说。首先，创建模块和原有的创建Java项目是完全一样的，以`oop-module`工程为例，它的目录结构如下：

```ascii
oop-module
├── bin
├── build.sh
└── src
    ├── com
    │   └── itranswarp
    │       └── sample
    │           ├── Greeting.java
    │           └── Main.java
    └── module-info.java
```

其中，`bin`目录存放编译后的class文件，`src`目录存放源码，按包名的目录结构存放，仅仅在`src`目录下多了一个`module-info.java`这个文件，这就是模块的描述文件。在这个模块中，它长这样：

```java
module hello.world {
    requires java.base; // 可不写，任何模块都会自动引入java.base
    requires java.xml;
}
```

其中，`module`是关键字，后面的`hello.world`是模块的名称，它的命名规范与包一致。花括号的`requires xxx;`表示这个模块需要引用的其他模块名。除了`java.base`可以被自动引入外，这里我们引入了一个`java.xml`的模块。

当我们使用模块声明了依赖关系后，才能使用引入的模块。例如，`Main.java`代码如下：

```java
package com.itranswarp.sample;

// 必须引入java.xml模块后才能使用其中的类:
import javax.xml.XMLConstants;

public class Main {
    public static void main(String[] args) {
        Greeting g = new Greeting();
        System.out.println(g.hello(XMLConstants.XML_NS_PREFIX));
        }
}
```

如果把`requires java.xml;`从`module-info.java`中去掉，编译将报错。可见，**模块的重要作用就是声明依赖关系**。

下面，我们用JDK提供的命令行工具来编译并创建模块。

首先，我们把工作目录切换到`oop-module`，在当前目录下编译所有的`.java`文件，并存放到`bin`目录下，命令如下：

```bash
> javac -d bin src/module-info.java src/com/itranswarp/sample/*.java
```

 如果编译成功，现在项目结构如下：

```ascii
oop-module
├── bin
│   ├── com
│   │   └── itranswarp
│   │       └── sample
│   │           ├── Greeting.class
│   │           └── Main.class
│   └── module-info.class
└── src
    ├── com
    │   └── itranswarp
    │       └── sample
    │           ├── Greeting.java
    │           └── Main.java
    └── module-info.java
```

注意到`src`目录下的`module-info.java`被编译到`bin`目录下的`module-info.class`。

下一步，我们需要把bin目录下的所有class文件先打包成jar，在打包的时候，注意传入`--main-class`参数，让这个jar包能自己定位`main`方法所在的类：

```bash
> jar --create --file hello.jar --main-class com.itranswarp.sample.Main -C bin .
```

现在我们就在当前目录下得到了`hello.jar`这个jar包，它和普通jar包并无区别，可以直接使用命令`java -jar hello.jar`来运行它。但是我们的目标是创建模块，所以，继续使用JDK自带的`jmod`命令把一个jar包转换成模块：

```bash
> jmod create --class-path hello.jar hello.jmod
```

于是，在当前目录下我们又得到了`hello.jmod`这个模块文件，这就是最后打包出来的传说中的模块！

#### 2.15.2 运行模块

要运行一个jar，我们使用`java -jar xxx.jar`命令。要运行一个模块，我们只需要指定模块名。试试：

```bash
> java --module-path hello.jmod --module hello.world
```

结果是一个错误：

```bash
Error occurred during initialization of boot layer
java.lang.module.FindException: JMOD format not supported at execution time: hello.jmod
```

原因是`.jmod`不能被放入`--module-path`中。换成`.jar`就没问题了：

```bash
$ java --module-path hello.jar --module hello.world
Hello, xml!
```

那我们辛辛苦苦创建的`hello.jmod`有什么用？答案是我们可以用它来打包JRE。

#### 2.15.3 打包JRE

前面讲了，为了支持模块化，Java 9首先带头把自己的一个巨大无比的`rt.jar`拆成了几十个`.jmod`模块，原因就是，运行Java程序的时候，实际上我们用到的JDK模块，并没有那么多。不需要的模块，完全可以删除。

过去发布一个Java应用程序，要运行它，必须下载一个完整的JRE，再运行jar包。而完整的JRE块头很大，有100多M。怎么给JRE瘦身呢？

现在，JRE自身的标准库已经分拆成了模块，只需要带上程序用到的模块，其他的模块就可以被裁剪掉。怎么裁剪JRE呢？并不是说把系统安装的JRE给删掉部分模块，而是“复制”一份JRE，但只带上用到的模块。为此，JDK提供了`jlink`命令来干这件事。命令如下：

```bash
> jlink --module-path hello.jmod --add-modules java.base,java.xml,hello.world --output jre/
```

我们在`--module-path`参数指定了我们自己的模块`hello.jmod`，然后，在`--add-modules`参数中指定了我们用到的3个模块`java.base`、`java.xml`和`hello.world`，用`,`分隔。最后，在`--output`参数指定输出目录。

现在，在当前目录下，我们可以找到`jre`目录，这是一个完整的并且带有我们自己`hello.jmod`模块的JRE。试试直接运行这个JRE：

```bash
$ jre/bin/java --module hello.world
Hello, xml!
```

要分发我们自己的Java应用程序，只需要把这个`jre`目录打个包给对方发过去，对方直接运行上述命令即可，既不用下载安装JDK，也不用知道如何配置我们自己的模块，极大地方便了分发和部署。

#### 2.15.4 访问权限

前面我们讲过，Java的class访问权限分为public、protected、private和默认的包访问权限。引入模块后，这些访问权限的规则就要稍微做些调整。

确切地说，class的这些访问权限只在一个模块内有效，模块和模块之间，例如，a模块要访问b模块的某个class，必要条件是b模块明确地导出了可以访问的包。

举个例子：我们编写的模块`hello.world`用到了模块`java.xml`的一个类`javax.xml.XMLConstants`，我们之所以能直接使用这个类，是因为模块`java.xml`的`module-info.java`中声明了若干导出：

```java
module java.xml {
    exports java.xml;
    exports javax.xml.catalog;
    exports javax.xml.datatype;
    ...
}
```

只有它声明的导出的包，外部代码才被允许访问。换句话说，如果外部代码想要访问我们的`hello.world`模块中的`com.itranswarp.sample.Greeting`类，我们必须将其导出：

```java
module hello.world {
    exports com.itranswarp.sample;
    requires java.base;
    requires java.xml;
}
```

因此，模块进一步隔离了代码的访问权限。

#### 2.15.5 小结

Java 9引入的模块目的是为了管理依赖；

使用模块可以按需打包JRE；

使用模块对类的访问权限有了进一步限制。

## 3 Java核心类

### 3.1 字符串和编码

#### 3.1.1 String

在Java中，`String`是一个引用类型，它本身也是一个`class`。但是，Java编译器对`String`有特殊处理，即可以直接用`"..."`来表示一个字符串：

```java
String s1 = "Hello!";
```

实际上字符串在`String`内部是通过一个`char[]`数组表示的，因此，按下面的写法也是可以的：

```java
String s2 = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});
```

因为`String`太常用了，所以Java提供了`"..."`这种字符串字面量表示方法。

Java字符串的一个重要特点就是字符串*不可变*。这种不可变性是通过内部的`private final char[]`字段，以及没有任何修改`char[]`的方法实现的。

我们来看一个例子：

```java
public class Main {
    public static void main(String[] args) {
        String s = "Hello";
        System.out.println(s);
        s = s.toUpperCase();
        System.out.println(s);
    }
}
```

输出： Hello
          HELLO

#### 3.1.2 字符串比较

 当我们想要比较两个字符串是否相同时，要特别注意，我们实际上是想比较字符串的内容是否相同。必须使用`equals()`方法而不能用`==`。

```java
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "hello";
        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}
```

从表面上看，两个字符串用`==`和`equals()`比较都为`true`，但实际上那只是Java编译器在编译期，会自动把所有相同的字符串当作一个对象放入常量池，自然`s1`和`s2`的引用就是相同的。

所以，这种`==`比较返回`true`纯属巧合。换一种写法，`==`比较就会失败：

```java
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "HELLO".toLowerCase();
        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}
```

结论：两个字符串比较，必须总是使用`equals()`方法。

要忽略大小写比较，使用`equalsIgnoreCase()`方法。

`String`类还提供了多种方法来搜索子串、提取子串。常用的方法有：

```java
// 是否包含子串:
"Hello".contains("ll"); // true
```

 注意到`contains()`方法的参数是`CharSequence`而不是`String`，因为`CharSequence`是`String`的父类。

 搜索子串的更多的例子：

```java
"Hello".indexOf("l"); // 2
"Hello".lastIndexOf("l"); // 3
"Hello".startsWith("He"); // true
"Hello".endsWith("lo"); // true
```

 提取子串的例子：

```java
"Hello".substring(2); // "llo"
"Hello".substring(2, 4); "ll"
```

 注意索引号是从`0`开始的。

#### 3.1.3 去除首尾空白字符

使用`trim()`方法可以移除字符串首尾空白字符。空白字符包括空格，`\t`，`\r`，`\n`：

```java
"  \tHello\r\n ".trim(); // "Hello"
```

注意：`trim()`并没有改变字符串的内容，而是返回了一个新字符串。

另一个`strip()`方法也可以移除字符串首尾空白字符。它和`trim()`不同的是，类似中文的空格字符`\u3000`也会被移除：

```java
"\u3000Hello\u3000".strip(); // "Hello"
" Hello ".stripLeading(); // "Hello "
" Hello ".stripTrailing(); // " Hello"
```

`String`还提供了`isEmpty()`和`isBlank()`来判断字符串是否为空和空白字符串：

```java
"".isEmpty(); // true，因为字符串长度为0
"  ".isEmpty(); // false，因为字符串长度不为0
"  \n".isBlank(); // true，因为只包含空白字符
" Hello ".isBlank(); // false，因为包含非空白字符
```

#### 3.1.4 替换子串

要在字符串中替换子串，有两种方法。一种是根据字符或字符串替换：

```java
String s = "hello";
s.replace('l', 'w'); // "hewwo"，所有字符'l'被替换为'w'
s.replace("ll", "~~"); // "he~~o"，所有子串"ll"被替换为"~~"
```

另一种是通过正则表达式替换：

```java
String s = "A,,B;C ,D";
s.replaceAll("[\\,\\;\\s]+", ","); // "A,B,C,D"
```

上面的代码通过正则表达式，把匹配的子串统一替换为`","`。关于正则表达式的用法我们会在后面详细讲解。

#### 3.1.4 分割字符串

要分割字符串，使用`split()`方法，并且传入的也是正则表达式：

```java
String s = "A,B,C,D";
String[] ss = s.split("\\,"); // {"A", "B", "C", "D"}
```

#### 3.1.5 拼接字符串

拼接字符串使用静态方法`join()`，它用指定的字符串连接字符串数组：

```java
String[] arr = {"A", "B", "C"};
String s = String.join("***", arr); // "A***B***C"
```

#### 3.1.6 类型转换

要把任意基本类型或引用类型转换为字符串，可以使用静态方法`valueOf()`。这是一个重载方法，编译器会根据参数自动选择合适的方法：

```java
String.valueOf(123); // "123"
String.valueOf(45.67); // "45.67"
String.valueOf(true); // "true"
String.valueOf(new Object()); // 类似java.lang.Object@636be97c
```

要把字符串转换为其他类型，就需要根据情况。例如，把字符串转换为`int`类型：

```java
int n1 = Integer.parseInt("123"); // 123
int n2 = Integer.parseInt("ff", 16); // 按十六进制转换，255
```

把字符串转换为`boolean`类型：

```java
boolean b1 = Boolean.parseBoolean("true"); // true
boolean b2 = Boolean.parseBoolean("FALSE"); // false
```

要特别注意，`Integer`有个`getInteger(String)`方法，它不是将字符串转换为`int`，而是把该字符串对应的系统变量转换为`Integer`：

```java
Integer.getInteger("java.version"); // 版本号，11
```

#### 3.1.7 转换为char[]

`String`和`char[]`类型可以互相转换，方法是：

```java
char[] cs = "Hello".toCharArray(); // String -> char[]
String s = new String(cs); // char[] -> String
```

如果修改了`char[]`数组，`String`并不会改变：

```java
public class Main {
    public static void main(String[] args) {
        char[] cs = "Hello".toCharArray();
        String s = new String(cs);
        System.out.println(s);
        cs[0] = 'X';
        System.out.println(s);
    }
}
```

这是因为通过`new String(char[])`创建新的`String`实例时，它并不会直接引用传入的`char[]`数组，而是会复制一份，所以，修改外部的`char[]`数组不会影响`String`实例内部的`char[]`数组，因为这是两个不同的数组。

从`String`的不变性设计可以看出，如果传入的对象有可能改变，我们需要复制而不是直接引用。

例如，下面的代码设计了一个`Score`类保存一组学生的成绩：

```java
public class Main {
    public static void main(String[] args) {
        int[] scores = new int[] { 88, 77, 51, 66 };
        Score s = new Score(scores);
        s.printScores();
        scores[2] = 99;
        s.printScores();
    }
}

class Score {
    private int[] scores;
    public Score(int[] scores) {
        this.scores = scores;
    }

    public void printScores() {
        System.out.println(Arrays.toString(scores));
    }
}
```

观察两次输出，由于`Score`内部直接引用了外部传入的`int[]`数组，这会造成外部代码对`int[]`数组的修改，影响到`Score`类的字段。如果外部代码不可信，这就会造成安全隐患。

请修复`Score`的构造方法，使得外部代码对数组的修改不影响`Score`实例的`int[]`字段。

#### 3.1.8 字符编码

在早期的计算机系统中，为了给字符编码，美国国家标准学会（American National Standard Institute：ANSI）制定了一套英文字母、数字和常用符号的编码，它占用一个字节，编码范围从`0`到`127`，最高位始终为`0`，称为`ASCII`编码。例如，字符`'A'`的编码是`0x41`，字符`'1'`的编码是`0x31`。

如果要把汉字也纳入计算机编码，很显然一个字节是不够的。`GB2312`标准使用两个字节表示一个汉字，其中第一个字节的最高位始终为`1`，以便和`ASCII`编码区分开。例如，汉字`'中'`的`GB2312`编码是`0xd6d0`。

类似的，日文有`Shift_JIS`编码，韩文有`EUC-KR`编码，这些编码因为标准不统一，同时使用，就会产生冲突。

为了统一全球所有语言的编码，全球统一码联盟发布了`Unicode`编码，它把世界上主要语言都纳入同一个编码，这样，中文、日文、韩文和其他语言就不会冲突。

`Unicode`编码需要两个或者更多字节表示，我们可以比较中英文字符在`ASCII`、`GB2312`和`Unicode`的编码：

英文字符`'A'`的`ASCII`编码和`Unicode`编码：

```ascii
         ┌────┐
ASCII:   │ 41 │
         └────┘
         ┌────┬────┐
Unicode: │ 00 │ 41 │
         └────┴────┘
```

英文字符的`Unicode`编码就是简单地在前面添加一个`00`字节。

中文字符`'中'`的`GB2312`编码和`Unicode`编码：

```ascii
         ┌────┬────┐
GB2312:  │ d6 │ d0 │
         └────┴────┘
         ┌────┬────┐
Unicode: │ 4e │ 2d │
         └────┴────┘
```

那我们经常使用的`UTF-8`又是什么编码呢？因为英文字符的`Unicode`编码高字节总是`00`，包含大量英文的文本会浪费空间，所以，出现了`UTF-8`编码，它是一种变长编码，用来把固定长度的`Unicode`编码变成1～4字节的变长编码。通过`UTF-8`编码，英文字符`'A'`的`UTF-8`编码变为`0x41`，正好和`ASCII`码一致，而中文`'中'`的`UTF-8`编码为3字节`0xe4b8ad`。

`UTF-8`编码的另一个好处是容错能力强。如果传输过程中某些字符出错，不会影响后续字符，因为`UTF-8`编码依靠高字节位来确定一个字符究竟是几个字节，它经常用来作为传输编码。

在Java中，`char`类型实际上就是两个字节的`Unicode`编码。如果我们要手动把字符串转换成其他编码，可以这样做：

```java
byte[] b1 = "Hello".getBytes(); // 按ISO8859-1编码转换，不推荐
byte[] b2 = "Hello".getBytes("UTF-8"); // 按UTF-8编码转换
byte[] b2 = "Hello".getBytes("GBK"); // 按GBK编码转换
byte[] b3 = "Hello".getBytes(StandardCharsets.UTF_8); // 按UTF-8编码转换
```

注意：转换编码后，就不再是`char`类型，而是`byte`类型表示的数组。

如果要把已知编码的`byte[]`转换为`String`，可以这样做：

```java
byte[] b = ...
String s1 = new String(b, "GBK"); // 按GBK转换
String s2 = new String(b, StandardCharsets.UTF_8); // 按UTF-8转换
```

始终牢记：Java的`String`和`char`在内存中总是以Unicode编码表示。

#### 3.1.9 延伸阅读

对于不同版本的JDK，`String`类在内存中有不同的优化方式。具体来说，早期JDK版本的`String`总是以`char[]`存储，它的定义如下：

```java
public final class String {
    private final char[] value;
    private final int offset;
    private final int count;
}
```

而较新的JDK版本的`String`则以`byte[]`存储：如果`String`仅包含ASCII字符，则每个`byte`存储一个字符，否则，每两个`byte`存储一个字符，这样做的目的是为了节省内存，因为大量的长度较短的`String`通常仅包含ASCII字符：

```java
public final class String {
    private final byte[] value;
    private final byte coder; // 0 = LATIN1, 1 = UTF16
```

对于使用者来说，`String`内部的优化不影响任何已有代码，因为它的`public`方法签名是不变的。

#### 3.1.10 小结

- Java字符串`String`是不可变对象；
- 字符串操作不改变原字符串内容，而是返回新字符串；
- 常用的字符串操作：提取子串、查找、替换、大小写转换等；
- Java使用Unicode编码表示`String`和`char`；
- 转换编码就是将`String`和`byte[]`转换，需要指定编码；
- 转换为`byte[]`时，始终优先考虑`UTF-8`编码。

### 3.2 StringBuilder

Java编译器对`String`做了特殊处理，使得我们可以直接用`+`拼接字符串。

考察下面的循环代码：

```java
String s = "";
for (int i = 0; i < 1000; i++) {
    s = s + "," + i;
}
```

虽然可以直接拼接字符串，但是，在循环中，每次循环都会创建新的字符串对象，然后扔掉旧的字符串。这样，绝大部分字符串都是临时对象，不但浪费内存，还会影响GC效率。

为了能高效拼接字符串，Java标准库提供了`StringBuilder`，它是一个可变对象，可以预分配缓冲区，这样，往`StringBuilder`中新增字符时，不会创建新的临时对象：

```java
StringBuilder sb = new StringBuilder(1024);
for (int i = 0; i < 1000; i++) {
    sb.append(',');
    sb.append(i);
}
String s = sb.toString();
```

 `StringBuilder`还可以进行链式操作：

```java
public class Main {
    public static void main(String[] args) {
        var sb = new StringBuilder(1024);
        sb.append("Mr ")
          .append("Bob")
          .append("!")
          .insert(0, "Hello, ");
        System.out.println(sb.toString());
    }
}
```

如果我们查看`StringBuilder`的源码，可以发现，进行链式操作的关键是，定义的`append()`方法会返回

`this`，这样，就可以不断调用自身的其他方法。

仿照`StringBuilder`，我们也可以设计支持链式操作的类。例如，一个可以不断增加的计数器：

```java
public class Main {
    public static void main(String[] args) {
        Adder adder = new Adder();
        adder.add(3)
             .add(5)
             .inc()
             .add(10);
        System.out.println(adder.value());
    }
}

class Adder {
    private int sum = 0;

    public Adder add(int n) {
        sum += n;
        return this;
    }

    public Adder inc() {
        sum ++;
        return this;
    }

    public int value() {
        return sum;
    }
}
```

注意：对于普通的字符串`+`操作，并不需要我们将其改写为`StringBuilder`，因为Java编译器在编译时就自动把多个连续的`+`操作编码为`StringConcatFactory`的操作。在运行期，`StringConcatFactory`会自动把字符串连接操作优化为数组复制或者`StringBuilder`操作。

你可能还听说过`StringBuffer`，这是Java早期的一个`StringBuilder`的线程安全版本，它通过同步来保证多个线程操作`StringBuffer`也是安全的，但是同步会带来执行速度的下降。

`StringBuilder`和`StringBuffer`接口完全相同，现在完全没有必要使用`StringBuffer`。

#### 3.2.1 小结

`StringBuilder`是可变对象，用来高效拼接字符串；

`StringBuilder`可以支持链式操作，实现链式操作的关键是返回实例本身；

`StringBuffer`是`StringBuilder`的线程安全版本，现在很少使用。

### 3.3 StringJoiner

要高效拼接字符串，应该使用`StringBuilder`。

很多时候，我们拼接的字符串像这样：

```java
public class Main {
    public static void main(String[] args) {
        String[] names = {"Bob", "Alice", "Grace"};
        var sb = new StringBuilder();
        sb.append("Hello ");
        for (String name : names) {
            sb.append(name).append(", ");
        }
        // 注意去掉最后的", ":
        sb.delete(sb.length() - 2, sb.length());
        sb.append("!");
        System.out.println(sb.toString());
    }
}
```

类似用分隔符拼接数组的需求很常见，所以Java标准库还提供了一个`StringJoiner`来干这个事：

```java
import java.util.StringJoiner;
public class Main {
    public static void main(String[] args) {
        String[] names = {"Bob", "Alice", "Grace"};
        var sj = new StringJoiner(", ");
        for (String name : names) {
            sj.add(name);
        }
        System.out.println(sj.toString());
    }
}
```

慢着！用`StringJoiner`的结果少了前面的`"Hello "`和结尾的`"!"`！遇到这种情况，需要给`StringJoine``r`指定“开头”和“结尾”：

```java
import java.util.StringJoiner;
public class Main {
    public static void main(String[] args) {
        String[] names = {"Bob", "Alice", "Grace"};
        var sj = new StringJoiner(", ", "Hello ", "!");
        for (String name : names) {
            sj.add(name);
        }
        System.out.println(sj.toString());
    }
} 
```

那么`StringJoiner`内部是如何拼接字符串的呢？如果查看源码，可以发现，`StringJoiner`内部实际上就是使用了`StringBuilder`，所以拼接效率和`StringBuilder`几乎是一模一样的。

#### 3.3.1 String.join()

`String`还提供了一个静态方法`join()`，这个方法在内部使用了`StringJoiner`来拼接字符串，在不需要指定“开头”和“结尾”的时候，用`String.join()`更方便：

```java
String[] names = {"Bob", "Alice", "Grace"};
var s = String.join(", ", names);
```

#### 3.3.2 小结

用指定分隔符拼接字符串数组时，使用`StringJoiner`或者`String.join()`更方便；

用`StringJoiner`拼接字符串时，还可以额外附加一个“开头”和“结尾”。

### 3.4 包装类型

我们已经知道，Java的数据类型分两种：

- 基本类型：`byte`，`short`，`int`，`long`，`boolean`，`float`，`double`，`char`
- 引用类型：所有`class`和`interface`类型

引用类型可以赋值为`null`，表示空，但基本类型不能赋值为`null`：

```java
String s = null;
int n = null; // compile error!
```

那么，如何把一个基本类型视为对象（引用类型）？

比如，想要把`int`基本类型变成一个引用类型，我们可以定义一个`Integer`类，它只包含一个实例字段`int`，这样，`Integer`类就可以视为`int`的包装类（Wrapper Class）：

```java
public class Integer {
    private int value;

    public Integer(int value) {
        this.value = value;
    }

    public int intValue() {
        return this.value;
    }
}
```

定义好了`Integer`类，我们就可以把`int`和`Integer`互相转换：

```java
Integer n = null;
Integer n2 = new Integer(99);
int n3 = n2.intValue();
```

实际上，因为包装类型非常有用，Java核心库为每种基本类型都提供了对应的包装类型：

| 基本类型 | 对应的引用类型      |
| :------- | :------------------ |
| boolean  | java.lang.Boolean   |
| byte     | java.lang.Byte      |
| short    | java.lang.Short     |
| int      | java.lang.Integer   |
| long     | java.lang.Long      |
| float    | java.lang.Float     |
| double   | java.lang.Double    |
| char     | java.lang.Character |

我们可以直接使用，并不需要自己去定义：

```java
public class Main {
    public static void main(String[] args) {
        int i = 100;
        // 通过new操作符创建Integer实例(不推荐使用,会有编译警告):
        Integer n1 = new Integer(i);
        // 通过静态方法valueOf(int)创建Integer实例:
        Integer n2 = Integer.valueOf(i);
        // 通过静态方法valueOf(String)创建Integer实例:
        Integer n3 = Integer.valueOf("100");
        System.out.println(n3.intValue());
    }
}
```

#### 3.4.1 Auto Boxing

因为`int`和`Integer`可以互相转换：

```java
int i = 100;
Integer n = Integer.valueOf(i);
int x = n.intValue();
```

 所以，Java编译器可以帮助我们自动在`int`和`Integer`之间转型：

```java
Integer n = 100; // 编译器自动使用Integer.valueOf(int)
int x = n; // 编译器自动使用Integer.intValue()
```

这种直接把`int`变为`Integer`的赋值写法，称为`自动装箱`（Auto Boxing），反过来，把`Integer`变为`int`的赋值写法，称为`自动拆箱`（Auto Unboxing）。

注意：自动装箱和自动拆箱只发生在编译阶段，目的是为了少写代码。

装箱和拆箱会影响代码的执行效率，因为编译后的`class`代码是严格区分基本类型和引用类型的。并且，自动拆箱执行时可能会报`NullPointerException`：

```java
public class Main {
    public static void main(String[] args) {
        Integer n = null;
        int i = n;
    }
}
//Exception in thread "main" java.lang.NullPointerException
//    at Main.main(Main.java:5)
```

#### 4.3.2 不变类

所有的包装类型都是不变类。我们查看`Integer`的源码可知，它的核心代码如下：

```java
public final class Integer {
    private final int value;
}
```

因此，一旦创建了`Integer`对象，该对象就是不变的。

对两个`Integer`实例进行比较要特别注意：绝对不能用`==`比较，因为`Integer`是引用类型，必须使用`equals()`比较：

```java
public class Main {
    public static void main(String[] args) {
        Integer x = 127;
        Integer y = 127;
        Integer m = 99999;
        Integer n = 99999;
        System.out.println("x == y: " + (x==y)); // true
        System.out.println("m == n: " + (m==n)); // false
        System.out.println("x.equals(y): " + x.equals(y)); // true
        System.out.println("m.equals(n): " + m.equals(n)); // true
    }
}
```

仔细观察结果的童鞋可以发现，`==`比较，较小的两个相同的`Integer`返回`true`，较大的两个相同的`Integer`返回`false`，这是因为`Integer`是不变类，编译器把`Integer x = 127;`自动变为`Integer x = Integer.valueOf(127);`，为了节省内存，`Integer.valueOf()`对于较小的数，始终返回相同的实例，因此，`==`比较“恰好”为`true`，但我们*绝不能*因为Java标准库的`Integer`内部有缓存优化就用`==`比较，必须用`equals()`方法比较两个`Integer`。

**按照语义编程，而不是针对特定的底层实现去“优化”。**

因为`Integer.valueOf()`可能始终返回同一个`Integer`实例，因此，在我们自己创建`Integer`的时候，以下两种方法：

- 方法1：`Integer n = new Integer(100);`
- 方法2：`Integer n = Integer.valueOf(100);`

方法2更好，因为方法1总是创建新的`Integer`实例，方法2把内部优化留给`Integer`的实现者去做，即使在当前版本没有优化，也有可能在下一个版本进行优化。

我们把能创建**“新”**对象的静态方法称为**静态工厂方法**。`Integer.valueOf()`就是静态工厂方法，它尽可能地返回缓存的实例以节省内存。

**创建新对象时，优先选用静态工厂方法而不是new操作符。**

如果我们考察`Byte.valueOf()`方法的源码，可以看到，标准库返回的`Byte`实例全部是缓存实例，但调用者并不关心静态工厂方法以何种方式创建新实例还是直接返回缓存的实例。

#### 3.4.3 进制转换

`Integer`类本身还提供了大量方法，例如，最常用的静态方法`parseInt()`可以把字符串解析成一个整数：

```java
int x1 = Integer.parseInt("100"); // 100
int x2 = Integer.parseInt("100", 16); // 256,因为按16进制解析
```

`Integer`还可以把整数格式化为指定进制的字符串：

```java
public class Main {
    public static void main(String[] args) {
        System.out.println(Integer.toString(100)); // "100",表示为10进制
        System.out.println(Integer.toString(100, 36)); // "2s",表示为36进制
        System.out.println(Integer.toHexString(100)); // "64",表示为16进制
        System.out.println(Integer.toOctalString(100)); // "144",表示为8进制
        System.out.println(Integer.toBinaryString(100)); // "1100100",表示为2进制
    }
}
```

注意：上述方法的输出都是`String`，在计算机内存中，只用二进制表示，不存在十进制或十六进制的表示方法。`int n = 100`在内存中总是以4字节的二进制表示：

```ascii
┌────────┬────────┬────────┬────────┐
│00000000│00000000│00000000│01100100│
└────────┴────────┴────────┴────────┘
```

我们经常使用的`System.out.println(n);`是依靠核心库自动把整数格式化为10进制输出并显示在屏幕上，使用`Integer.toHexString(n)`则通过核心库自动把整数格式化为16进制。

这里我们注意到程序设计的一个重要原则：数据的存储和显示要分离。

Java的包装类型还定义了一些有用的静态变量

```java
// boolean只有两个值true/false，其包装类型只需要引用Boolean提供的静态字段:
Boolean t = Boolean.TRUE;
Boolean f = Boolean.FALSE;
// int可表示的最大/最小值:
int max = Integer.MAX_VALUE; // 2147483647
int min = Integer.MIN_VALUE; // -2147483648
// long类型占用的bit和byte数量:
int sizeOfLong = Long.SIZE; // 64 (bits)
int bytesOfLong = Long.BYTES; // 8 (bytes)
```

最后，所有的整数和浮点数的包装类型都继承自`Number`，因此，可以非常方便地直接通过包装类型获取各种基本类型：

```java
// 向上转型为Number:
Number num = new Integer(999);
// 获取byte, int, long, float, double:
byte b = num.byteValue();
int n = num.intValue();
long ln = num.longValue();
float f = num.floatValue();
double d = num.doubleValue();
```

#### 3.4.4 处理无符号整型

在Java中，并没有无符号整型（Unsigned）的基本数据类型。`byte`、`short`、`int`和`long`都是带符号整型，最高位是符号位。而C语言则提供了CPU支持的全部数据类型，包括无符号整型。无符号整型和有符号整型的转换在Java中就需要借助包装类型的静态方法完成。

例如，byte是有符号整型，范围是`-128~+127`，但如果把`byte`看作无符号整型，它的范围就是`0`~`255`。我们把一个负的`byte`按无符号整型转换为`int`：

```java
public class Main {
    public static void main(String[] args) {
        byte x = -1;
        byte y = 127;
        System.out.println(Byte.toUnsignedInt(x)); // 255
        System.out.println(Byte.toUnsignedInt(y)); // 127
    }
}
```

因为`byte`的`-1`的二进制表示是`11111111`，以无符号整型转换后的`int`就是`255`。

类似的，可以把一个`short`按unsigned转换为`int`，把一个`int`按unsigned转换为`long`。

#### 3.4.5 小结

Java核心库提供的包装类型可以把基本类型包装为`class`；

自动装箱和自动拆箱都是在编译期完成的（JDK>=1.5）；

装箱和拆箱会影响执行效率，且拆箱时可能发生`NullPointerException`；

包装类型的比较必须使用`equals()`；

整数和浮点数的包装类型都继承自`Number`；

包装类型提供了大量实用方法。

### 3.5 JavaBean

在Java中，有很多`class`的定义都符合这样的规范：

- 若干`private`实例字段；
- 通过`public`方法来读写实例字段。

例如：

```java
public class Person {
    private String name;
    private int age;

    public String getName() { return this.name; }
    public void setName(String name) { this.name = name; }

    public int getAge() { return this.age; }
    public void setAge(int age) { this.age = age; }
}
```

 如果读写方法符合以下这种命名规范：

```java
// 读方法:
public Type getXyz()
// 写方法:
public void setXyz(Type value)
```

那么这种`class`被称为`JavaBean`.

上面的字段是`xyz`，那么读写方法名分别以`get`和`set`开头，并且后接大写字母开头的字段名`Xyz`，因此两个读写方法名分别是`getXyz()`和`setXyz()`。

`boolean`字段比较特殊，它的读方法一般命名为`isXyz()`：

```java
// 读方法:
public boolean isChild()
// 写方法:
public void setChild(boolean value)
```

我们通常把一组对应的读方法（`getter`）和写方法（`setter`）称为属性（`property`）。例如，`name`属性：

- 对应的读方法是`String getName()`
- 对应的写方法是`setName(String)`

只有`getter`的属性称为只读属性（read-only），例如，定义一个age只读属性：

- 对应的读方法是`int getAge()`
- 无对应的写方法`setAge(int)`

类似的，只有`setter`的属性称为只写属性（write-only）。

很明显，只读属性很常见，只写属性不常见。

属性只需要定义`getter`和`setter`方法，不一定需要对应的字段。例如，`child`只读属性定义如下：

```java
public class Person {
    private String name;
    private int age;

    public String getName() { return this.name; }
    public void setName(String name) { this.name = name; }

    public int getAge() { return this.age; }
    public void setAge(int age) { this.age = age; }

    public boolean isChild() {
        return age <= 6;
    }
}
```

可以看出，`getter`和`setter`也是一种数据封装的方法。

#### 3.5.1 JavaBean的作用

JavaBean主要用来传递数据，即把一组数据组合成一个JavaBean便于传输。此外，JavaBean可以方便地被IDE工具分析，生成读写属性的代码，主要用在图形界面的可视化设计中。

通过IDE，可以快速生成`getter`和`setter`。例如，在Eclipse中，先输入以下代码：

```java
public class Person {
    private String name;
    private int age;
}
```

然后，点击右键，在弹出的菜单中选择“Source”，“Generate Getters and Setters”，在弹出的对话框中选中需要生成`getter`和`setter`方法的字段，点击确定即可由IDE自动完成所有方法代码。

#### 3.5.2 枚举JavaBean属性

要枚举一个JavaBean的所有属性，可以直接使用Java核心库提供的`Introspector`：

```java
import java.beans.*;
public class Main {
    public static void main(String[] args) throws Exception {
        BeanInfo info = Introspector.getBeanInfo(Person.class);
        for (PropertyDescriptor pd : info.getPropertyDescriptors()) {
            System.out.println(pd.getName());
            System.out.println("  " + pd.getReadMethod());
            System.out.println("  " + pd.getWriteMethod());
        }
    }
}

class Person {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

 运行上述代码，可以列出所有的属性，以及对应的读写方法。注意`class`属性是从`Object`继承的`getClass()`方法带来的。

```java
age
 public int Person.getAge()
 public void Person.setAge(int)
class
 public final native java.lang.Class java.lang.Object.getClass()
 null
name
 public java.lang.String Person.getName()
 public void Person.setName(java.lang.String)
```

#### 3.5.3 小结

JavaBean是一种符合命名规范的`class`，它通过`getter`和`setter`来定义属性；

属性是一种通用的叫法，并非Java语法规定；

可以利用IDE快速生成`getter`和`setter`；

使用`Introspector.getBeanInfo()`可以获取属性列表。

### 3.6 枚举类

在Java中，我们可以通过`static final`来定义常量。例如，我们希望定义周一到周日这7个常量，可以用7个不同的`int`表示：

```java
public class Weekday {
    public static final int SUN = 0;
    public static final int MON = 1;
    public static final int TUE = 2;
    public static final int WED = 3;
    public static final int THU = 4;
    public static final int FRI = 5;
    public static final int SAT = 6;
}
```

使用常量的时候，可以这么引用：

```java
if (day == Weekday.SAT || day == Weekday.SUN) {
    // TODO: work at home
}
```

也可以把常量定义为字符串类型，例如，定义3种颜色的常量：

```java
public class Color {
    public static final String RED = "r";
    public static final String GREEN = "g";
    public static final String BLUE = "b";
}
```

使用常量的时候，可以这么引用：

```java
String color = ...
if (Color.RED.equals(color)) {
    // TODO:
}
```

无论是`int`常量还是`String`常量，使用这些常量来表示一组枚举值的时候，有一个严重的问题就是，编译器无法检查每个值的合理性。例如：

```java
if (weekday == 6 || weekday == 7) {
    if (tasks == Weekday.MON) {
        // TODO:
    }
}
```

上述代码编译和运行均不会报错，但存在两个问题：

- 注意到`Weekday`定义的常量范围是`0`~`6`，并不包含`7`，编译器无法检查不在枚举中的`int`值；
- 定义的常量仍可与其他变量比较，但其用途并非是枚举星期值。

#### 3.6.1 enum

为了让编译器能自动检查某个值在枚举的集合内，并且，不同用途的枚举需要不同的类型来标记，不能混用，我们可以使用`enum`来定义枚举类：

```java
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        if (day == Weekday.SAT || day == Weekday.SUN) {
            System.out.println("Work at home!");
        } else {
            System.out.println("Work at office!");
        }
    }
}

enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT;
}
```

注意到定义枚举类是通过关键字`enum`实现的，我们只需依次列出枚举的常量名。

和`int`定义的常量相比，使用`enum`定义枚举有如下好处：

首先，`enum`常量本身带有类型信息，即`Weekday.SUN`类型是`Weekday`，编译器会自动检查出类型错误。例如，下面的语句不可能编译通过：

```java
int day = 1;
if (day == Weekday.SUN) { // Compile error: bad operand types for binary operator '=='
}
```

其次，不可能引用到非枚举的值，因为无法通过编译。

最后，不同类型的枚举不能互相比较或者赋值，因为类型不符。例如，不能给一个`Weekday`枚举类型的变量赋值为`Color`枚举类型的值：

```java
Weekday x = Weekday.SUN; // ok!
Weekday y = Color.RED; // Compile error: incompatible types
```

这就使得编译器可以在编译期自动检查出所有可能的潜在错误。

#### 3.6.2 enum的比较

使用`enum`定义的枚举类是一种引用类型。前面我们讲到，引用类型比较，要使用`equals()`方法，如果使用`==`比较，它比较的是两个引用类型的变量是否是同一个对象。因此，引用类型比较，要始终使用`equals()`方法，但`enum`类型可以例外。

这是因为`enum`类型的每个常量在JVM中只有一个唯一实例，所以可以直接用`==`比较：

```java
if (day == Weekday.FRI) { // ok!
}
if (day.equals(Weekday.SUN)) { // ok, but more code!
}
```

#### 3.6.3 enum类型

通过`enum`定义的枚举类，和其他的`class`有什么区别？

答案是没有任何区别。`enum`定义的类型就是`class`，只不过它有以下几个特点：

- 定义的`enum`类型总是继承自`java.lang.Enum`，且无法被继承；
- 只能定义出`enum`的实例，而无法通过`new`操作符创建`enum`的实例；
- 定义的每个实例都是引用类型的唯一实例；
- 可以将`enum`类型用于`switch`语句。

例如，我们定义的`Color`枚举类：

```java
public enum Color {
    RED, GREEN, BLUE;
}
```

编译器编译出的`class`大概就像这样：

```java
public final class Color extends Enum { // 继承自Enum，标记为final class
    // 每个实例均为全局唯一:
    public static final Color RED = new Color();
    public static final Color GREEN = new Color();
    public static final Color BLUE = new Color();
    // private构造方法，确保外部无法调用new操作符:
    private Color() {}
}
```

所以，编译后的`enum`类和普通`class`并没有任何区别。但是我们自己无法按定义普通`class`那样来定义`enum`，必须使用`enum`关键字，这是Java语法规定的。

因为`enum`是一个`class`，每个枚举的值都是`class`实例，因此，这些实例有一些方法：

**name()**

返回常量名，例如：

```java
String s = Weekday.SUN.name(); // "SUN"
```

**ordinal()**

返回定义的常量的顺序，从0开始计数，例如：

```java
int n = Weekday.MON.ordinal(); // 1
```

改变枚举常量定义的顺序就会导致`ordinal()`返回值发生变化。例如：

```java
public enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT;
}
```

和

```java
public enum Weekday {
    MON, TUE, WED, THU, FRI, SAT, SUN;
}
```

的`ordinal`就是不同的。如果在代码中编写了类似`if(x.ordinal()==1)`这样的语句，就要保证`enum`的枚举顺序不能变。新增的常量必须放在最后。

有些童鞋会想，`Weekday`的枚举常量如果要和`int`转换，使用`ordinal()`不是非常方便？比如这样写：

```java
String task = Weekday.MON.ordinal() + "/ppt";
saveToFile(task);
```

但是，如果不小心修改了枚举的顺序，编译器是无法检查出这种逻辑错误的。要编写健壮的代码，就不要依靠`ordinal()`的返回值。因为`enum`本身是`class`，所以我们可以定义`private`的构造方法，并且，给每个枚举常量添加字段：

```java
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        if (day.dayValue == 6 || day.dayValue == 0) {
            System.out.println("Work at home!");
        } else {
            System.out.println("Work at office!");
        }
    }
}

enum Weekday {
    MON(1), TUE(2), WED(3), THU(4), FRI(5), SAT(6), SUN(0);

    public final int dayValue;

    private Weekday(int dayValue) {
        this.dayValue = dayValue;
    }
}
```

这样就无需担心顺序的变化，新增枚举常量时，也需要指定一个`int`值。

**注意：枚举类的字段也可以是非final类型，即可以在运行期修改，但是不推荐这样做！**

默认情况下，对枚举常量调用`toString()`会返回和`name()`一样的字符串。但是，`toString()`可以被覆写，而`name()`则不行。我们可以给`Weekday`添加`toString()`方法：

```java
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        if (day.dayValue == 6 || day.dayValue == 0) {
            System.out.println("Today is " + day + ". Work at home!");
        } else {
            System.out.println("Today is " + day + ". Work at office!");
        }
    }
}

enum Weekday {
    MON(1, "星期一"), TUE(2, "星期二"), WED(3, "星期三"), THU(4, "星期四"), FRI(5, "星期五"), SAT(6, "星期六"), SUN(0, "星期日");

    public final int dayValue;
    private final String chinese;

    private Weekday(int dayValue, String chinese) {
        this.dayValue = dayValue;
        this.chinese = chinese;
    }

    @Override
    public String toString() {
        return this.chinese;
    }
}
```

覆写`toString()`的目的是在输出时更有可读性。

**switch**

最后，枚举类可以应用在`switch`语句中。因为枚举类天生具有类型信息和有限个枚举常量，所以比`int`、`String`类型更适合用在`switch`语句中：

```java
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        switch(day) {
        case MON:
        case TUE:
        case WED:
        case THU:
        case FRI:
            System.out.println("Today is " + day + ". Work at office!");
            break;
        case SAT:
        case SUN:
            System.out.println("Today is " + day + ". Work at home!");
            break;
        default:
            throw new RuntimeException("cannot process " + day);
        }
    }
}

enum Weekday {
    MON, TUE, WED, THU, FRI, SAT, SUN;
}
```

加上`default`语句，可以在漏写某个枚举常量时自动报错，从而及时发现错误。

#### 3.6.4 小结

Java使用`enum`定义枚举类型，它被编译器编译为`final class Xxx extends Enum { … }`；

通过`name()`获取常量定义的字符串，注意不要使用`toString()`；

通过`ordinal()`返回常量定义的顺序（无实质意义）；

可以为`enum`编写构造方法、字段和方法

`enum`的构造方法要声明为`private`，字段强烈建议声明为`final`；

`enum`适合用在`switch`语句中。

### 3.7 BigInteger

在Java中，由CPU原生提供的整型最大范围是64位`long`型整数。使用`long`型整数可以直接通过CPU指令进行计算，速度非常快。

如果我们使用的整数范围超过了`long`型怎么办？这个时候，就只能用软件来模拟一个大整数。`java.math.BigInteger`就是用来表示任意大小的整数。`BigInteger`内部用一个`int[]`数组来模拟一个非常大的整数：

```java
BigInteger bi = new BigInteger("1234567890");
System.out.println(bi.pow(5)); // 2867971860299718107233761438093672048294900000
```

对`BigInteger`做运算的时候，只能使用实例方法，例如，加法运算：

```java
BigInteger i1 = new BigInteger("1234567890");
BigInteger i2 = new BigInteger("12345678901234567890");
BigInteger sum = i1.add(i2); // 12345678902469135780
```

和`long`型整数运算比，`BigInteger`不会有范围限制，但缺点是速度比较慢。

也可以把`BigInteger`转换成`long`型：

```java
BigInteger i = new BigInteger("123456789000");
System.out.println(i.longValue()); // 123456789000
System.out.println(i.multiply(i).longValueExact()); // java.lang.ArithmeticException: BigInteger out of long range
```

使用`longValueExact()`方法时，如果超出了`long`型的范围，会抛出`ArithmeticException`。

`BigInteger`和`Integer`、`Long`一样，也是不可变类，并且也继承自`Number`类。因为`Number`定义了转换为基本类型的几个方法：

- 转换为`byte`：`byteValue()`
- 转换为`short`：`shortValue()`
- 转换为`int`：`intValue()`
- 转换为`long`：`longValue()`
- 转换为`float`：`floatValue()`
- 转换为`double`：`doubleValue()`

因此，通过上述方法，可以把`BigInteger`转换成基本类型。如果`BigInteger`表示的范围超过了基本类型的范围，转换时将丢失高位信息，即结果不一定是准确的。如果需要准确地转换成基本类型，可以使用`intValueExact()`、`longValueExact()`等方法，在转换时如果超出范围，将直接抛出`ArithmeticException`异常。

如果`BigInteger`的值甚至超过了`float`的最大范围（3.4x1038），那么返回的float是什么呢？

```java
public class Main {
    public static void main(String[] args) {
        BigInteger n = new BigInteger("999999").pow(99);
        float f = n.floatValue();
        System.out.println(f);
    }
}
//返回Infinity
```

#### 3.7.1 小结

`BigInteger`用于表示任意大小的整数；

`BigInteger`是不变类，并且继承自`Number`；

将`BigInteger`转换成基本类型时可使用`longValueExact()`等方法保证结果准确。

### 3.8 BigDecimal

和`BigInteger`类似，`BigDecimal`可以表示一个任意大小且精度完全准确的浮点数。

```java
BigDecimal bd = new BigDecimal("123.4567");
System.out.println(bd.multiply(bd)); // 15241.55677489
```

`BigDecimal`用`scale()`表示小数位数，例如：

```java
BigDecimal d1 = new BigDecimal("123.45");
BigDecimal d2 = new BigDecimal("123.4500");
BigDecimal d3 = new BigDecimal("1234500");
System.out.println(d1.scale()); // 2,两位小数
System.out.println(d2.scale()); // 4
System.out.println(d3.scale()); // 0
```

通过`BigDecimal`的`stripTrailingZeros()`方法，可以将一个`BigDecimal`格式化为一个相等的，但去掉了末尾0的`BigDecimal`：

```java
BigDecimal d1 = new BigDecimal("123.4500");
BigDecimal d2 = d1.stripTrailingZeros();
System.out.println(d1.scale()); // 4
System.out.println(d2.scale()); // 2,因为去掉了00

BigDecimal d3 = new BigDecimal("1234500");
BigDecimal d4 = d3.stripTrailingZeros();
System.out.println(d3.scale()); // 0
System.out.println(d4.scale()); // -2
```

如果一个`BigDecimal`的`scale()`返回负数，例如，`-2`，表示这个数是个整数，并且末尾有2个0。

可以对一个`BigDecimal`设置它的`scale`，如果精度比原始值低，那么按照指定的方法进行四舍五入或者直接截断：

```java
import java.math.BigDecimal;
import java.math.RoundingMode;
public class Main {
    public static void main(String[] args) {
        BigDecimal d1 = new BigDecimal("123.456789");
        BigDecimal d2 = d1.setScale(4, RoundingMode.HALF_UP); // 四舍五入，123.4568
        BigDecimal d3 = d1.setScale(4, RoundingMode.DOWN); // 直接截断，123.4567
        System.out.println(d2);
        System.out.println(d3);
    }
}
```

对`BigDecimal`做加、减、乘时，精度不会丢失，但是做除法时，存在无法除尽的情况，这时，就必须指定精度以及如何进行截断：

```java
BigDecimal d1 = new BigDecimal("123.456");
BigDecimal d2 = new BigDecimal("23.456789");
BigDecimal d3 = d1.divide(d2, 10, RoundingMode.HALF_UP); // 保留10位小数并四舍五入
BigDecimal d4 = d1.divide(d2); // 报错：ArithmeticException，因为除不尽
```

还可以对`BigDecimal`做除法的同时求余数：

```java
import java.math.BigDecimal;
public class Main {
    public static void main(String[] args) {
        BigDecimal n = new BigDecimal("12.345");
        BigDecimal m = new BigDecimal("0.12");
        BigDecimal[] dr = n.divideAndRemainder(m);
        System.out.println(dr[0]); // 102
        System.out.println(dr[1]); // 0.105
    }
}
```

调用`divideAndRemainder()`方法时，返回的数组包含两个`BigDecimal`，分别是商和余数，其中商总是整数，余数不会大于除数。我们可以利用这个方法判断两个`BigDecimal`是否是整数倍数：

```java
BigDecimal n = new BigDecimal("12.75");
BigDecimal m = new BigDecimal("0.15");
BigDecimal[] dr = n.divideAndRemainder(m);
if (dr[1].signum() == 0) {
    // n是m的整数倍
}
```

#### 3.8.1 比较BigDecimal

在比较两个`BigDecimal`的值是否相等时，要特别注意，使用`equals()`方法不但要求两个`BigDecimal`的值相等，还要求它们的`scale()`相等：

```java
BigDecimal d1 = new BigDecimal("123.456");
BigDecimal d2 = new BigDecimal("123.45600");
System.out.println(d1.equals(d2)); // false,因为scale不同
System.out.println(d1.equals(d2.stripTrailingZeros())); // true,因为d2去除尾部0后scale变为2
System.out.println(d1.compareTo(d2)); // 0
```

必须使用`compareTo()`方法来比较，它根据两个值的大小分别返回负数、正数和`0`，分别表示小于、大于和等于。

**总是使用compareTo()比较两个BigDecimal的值，不要使用equals()！**

如果查看`BigDecimal`的源码，可以发现，实际上一个`BigDecimal`是通过一个`BigInteger`和一个`scale`来表示的，即`BigInteger`表示一个完整的整数，而`scale`表示小数位数：

```java
public class BigDecimal extends Number implements Comparable<BigDecimal> {
    private final BigInteger intVal;
    private final int scale;
}
```

`BigDecimal`也是从`Number`继承的，也是不可变对象。

#### 3.8.2 小结

`BigDecimal`用于表示精确的小数，常用于财务计算；

比较`BigDecimal`的值是否相等，必须使用`compareTo()`而不能使用`equals()`。

### 3.9 常用工具类

Java的核心库提供了大量的现成的类供我们使用。本节我们介绍几个常用的工具类。

#### 3.9.1 Math

顾名思义，`Math`类就是用来进行数学计算的，它提供了大量的静态方法来便于我们实现数学计算：

求绝对值：

```java
Math.abs(-100); // 100
Math.abs(-7.8); // 7.8
```

取最大或最小值：

```java
Math.max(100, 99); // 100
Math.min(1.2, 2.3); // 1.2
```

计算xy次方：

```java
Math.pow(2, 10); // 2的10次方=1024
```

计算√x：

```java
Math.sqrt(2); // 1.414...
```

计算ex次方：

```java
Math.exp(2); // 7.389...
```

计算以e为底的对数：

```java
Math.log(4); // 1.386...
```

计算以10为底的对数：

```java
Math.log10(100); // 2
```

三角函数：

```java
Math.sin(3.14); // 0.00159...
Math.cos(3.14); // -0.9999...
Math.tan(3.14); // -0.0015...
Math.asin(1.0); // 1.57079...
Math.acos(1.0); // 0.0
```

Math还提供了几个数学常量：

```java
double pi = Math.PI; // 3.14159...
double e = Math.E; // 2.7182818...
Math.sin(Math.PI / 6); // sin(π/6) = 0.5
```

生成一个随机数x，x的范围是`0 <= x < 1`：

```java
Math.random(); // 0.53907... 每次都不一样
```

如果我们要生成一个区间在`[MIN, MAX)`的随机数，可以借助`Math.random()`实现，计算如下：

```java
// 区间在[MIN, MAX)的随机数
public class Main {
    public static void main(String[] args) {
        double x = Math.random(); // x的范围是[0,1)
        double min = 10;
        double max = 50;
        double y = x * (max - min) + min; // y的范围是[10,50)
        long n = (long) y; // n的范围是[10,50)的整数
        System.out.println(y);
        System.out.println(n);
    }
}
```

有些童鞋可能注意到Java标准库还提供了一个`StrictMath`，它提供了和`Math`几乎一模一样的方法。这两个类的区别在于，由于浮点数计算存在误差，不同的平台（例如x86和ARM）计算的结果可能不一致（指误差不同），因此，`StrictMath`保证所有平台计算结果都是完全相同的，而`Math`会尽量针对平台优化计算速度，所以，绝大多数情况下，使用`Math`就足够了。

#### 3.9.2 Random

`Random`用来创建伪随机数。所谓伪随机数，是指只要给定一个初始的种子，产生的随机数序列是完全一样的。

要生成一个随机数，可以使用`nextInt()`、`nextLong()`、`nextFloat()`、`nextDouble()`：

```java
Random r = new Random();
r.nextInt(); // 2071575453,每次都不一样
r.nextInt(10); // 5,生成一个[0,10)之间的int
r.nextLong(); // 8811649292570369305,每次都不一样
r.nextFloat(); // 0.54335...生成一个[0,1)之间的float
r.nextDouble(); // 0.3716...生成一个[0,1)之间的double
```

有童鞋问，每次运行程序，生成的随机数都是不同的，没看出*伪随机数*的特性来。

这是因为我们创建`Random`实例时，如果不给定种子，就使用系统当前时间戳作为种子，因此每次运行时，种子不同，得到的伪随机数序列就不同。

如果我们在创建`Random`实例时指定一个种子，就会得到完全确定的随机数序列：

```java
import java.util.Random;
public class Main {
    public static void main(String[] args) {
        Random r = new Random(12345);
        for (int i = 0; i < 10; i++) {
            System.out.println(r.nextInt(100));
        }
        // 51, 80, 41, 28, 55...
    }
}
```

前面我们使用的`Math.random()`实际上内部调用了`Random`类，所以它也是伪随机数，只是我们无法指定种子。

#### 3.9.3 SecureRandom

有伪随机数，就有真随机数。实际上真正的真随机数只能通过量子力学原理来获取，而我们想要的是一个不可预测的安全的随机数，`SecureRandom`就是用来创建安全的随机数的：

```java
SecureRandom sr = new SecureRandom();
System.out.println(sr.nextInt(100));
```

`SecureRandom`无法指定种子，它使用RNG（random number generator）算法。JDK的`SecureRandom`实际上有多种不同的底层实现，有的使用安全随机种子加上伪随机数算法来产生安全的随机数，有的使用真正的随机数生成器。实际使用的时候，可以优先获取高强度的安全随机数生成器，如果没有提供，再使用普通等级的安全随机数生成器：

```java
import java.util.Arrays;
import java.security.SecureRandom;
import java.security.NoSuchAlgorithmException;
public class Main {
    public static void main(String[] args) {
        SecureRandom sr = null;
        try {
            sr = SecureRandom.getInstanceStrong(); // 获取高强度安全随机数生成器
        } catch (NoSuchAlgorithmException e) {
            sr = new SecureRandom(); // 获取普通的安全随机数生成器
        }
        byte[] buffer = new byte[16];
        sr.nextBytes(buffer); // 用安全随机数填充buffer
        System.out.println(Arrays.toString(buffer));
    }
}
```

`SecureRandom`的安全性是通过操作系统提供的安全的随机种子来生成随机数。这个种子是通过CPU的热噪声、读写磁盘的字节、网络流量等各种随机事件产生的“熵”。

在密码学中，安全的随机数非常重要。如果使用不安全的伪随机数，所有加密体系都将被攻破。因此，时刻牢记必须使用`SecureRandom`来产生安全的随机数。

**需要使用安全随机数的时候，必须使用SecureRandom，绝不能使用Random！**

#### 3.9.4 小结

Java提供的常用工具类有：

- Math：数学计算
- Random：生成伪随机数
- SecureRandom：生成安全的随机数

## 4 异常处理

### 4.1 Java的异常

在计算机程序运行的过程中，总是会出现各种各样的错误。

有一些错误是用户造成的，比如，希望用户输入一个`int`类型的年龄，但是用户的输入是`abc`：

```java
// 假设用户输入了abc：
String s = "abc";
int n = Integer.parseInt(s); // NumberFormatException!
```

程序想要读写某个文件的内容，但是用户已经把它删除了：

```java
// 用户删除了该文件：
String t = readFile("C:\\abc.txt"); // FileNotFoundException!
```

还有一些错误是随机出现，并且永远不可能避免的。比如：

- 网络突然断了，连接不到远程服务器；
- 内存耗尽，程序崩溃了；
- 用户点“打印”，但根本没有打印机；
- ……

所以，一个健壮的程序必须处理各种各样的错误。

所谓错误，就是程序调用某个函数的时候，如果失败了，就表示出错。

调用方如何获知调用失败的信息？有两种方法：

方法一：约定返回错误码。

例如，处理一个文件，如果返回`0`，表示成功，返回其他整数，表示约定的错误码：

```java
int code = processFile("C:\\test.txt");
if (code == 0) {
    // ok:
} else {
    // error:
    switch (code) {
    case 1:
        // file not found:
    case 2:
        // no read permission:
    default:
        // unknown error:
    }
}
```

因为使用`int`类型的错误码，想要处理就非常麻烦。这种方式常见于底层C函数。

方法二：在语言层面上提供一个异常处理机制。

Java内置了一套异常处理机制，总是使用异常来表示错误。

异常是一种`class`，因此它本身带有类型信息。异常可以在任何地方抛出，但只需要在上层捕获，这样就和方法调用分离了：

```java
try {
    String s = processFile(“C:\\test.txt”);
    // ok:
} catch (FileNotFoundException e) {
    // file not found:
} catch (SecurityException e) {
    // no read permission:
} catch (IOException e) {
    // io error:
} catch (Exception e) {
    // other error:
}
```

因为Java的异常是`class`，它的继承关系如下：

```ascii
                     ┌───────────┐
                     │  Object   │
                     └───────────┘
                           ▲
                           │
                     ┌───────────┐
                     │ Throwable │
                     └───────────┘
                           ▲
                 ┌─────────┴─────────┐
                 │                   │
           ┌───────────┐       ┌───────────┐
           │   Error   │       │ Exception │
           └───────────┘       └───────────┘
                 ▲                   ▲
         ┌───────┘              ┌────┴──────────┐
         │                      │               │
┌─────────────────┐    ┌─────────────────┐┌───────────┐
│OutOfMemoryError │... │RuntimeException ││IOException│...
└─────────────────┘    └─────────────────┘└───────────┘
                                ▲
                    ┌───────────┴─────────────┐
                    │                         │
         ┌─────────────────────┐ ┌─────────────────────────┐
         │NullPointerException │ │IllegalArgumentException │...
         └─────────────────────┘ └─────────────────────────┘
```

从继承关系可知：`Throwable`是异常体系的根，它继承自`Object`。`Throwable`有两个体系：`Error`和`Exception`，`Error`表示严重的错误，程序对此一般无能为力，例如：

- `OutOfMemoryError`：内存耗尽
- `NoClassDefFoundError`：无法加载某个Class
- `StackOverflowError`：栈溢出

而`Exception`则是运行时的错误，它可以被捕获并处理。

某些异常是应用程序逻辑处理的一部分，应该捕获并处理。例如：

- `NumberFormatException`：数值类型的格式错误
- `FileNotFoundException`：未找到文件
- `SocketException`：读取网络失败

还有一些异常是程序逻辑编写不对造成的，应该修复程序本身。例如：

- `NullPointerException`：对某个`null`的对象调用方法或字段
- `IndexOutOfBoundsException`：数组索引越界

`Exception`又分为两大类：

1. `RuntimeException`以及它的子类；
2. 非`RuntimeException`（包括`IOException`、`ReflectiveOperationException`等等）

Java规定：

- 必须捕获的异常，包括`Exception`及其子类，但不包括`RuntimeException`及其子类，这种类型的异常称为Checked Exception。
- 不需要捕获的异常，包括`Error`及其子类，`RuntimeException`及其子类。

#### 4.1.1 捕获异常

 捕获异常使用`try...catch`语句，把可能发生异常的代码放到`try {...}`中，然后使用`catch`捕获对应的`Exception`及其子类：

```java
import java.io.UnsupportedEncodingException;
import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) {
        try {
            // 用指定编码转换String为byte[]:
            return s.getBytes("GBK");
        } catch (UnsupportedEncodingException e) {
            // 如果系统不支持GBK编码，会捕获到UnsupportedEncodingException:
            System.out.println(e); // 打印异常信息
            return s.getBytes(); // 尝试使用用默认编码
        }
    }
}

```

 如果我们不捕获`UnsupportedEncodingException`，会出现编译失败的问题：

```java
import java.io.UnsupportedEncodingException;
import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) {
        return s.getBytes("GBK");
    }
}

Main.java:11: 错误: 未报告的异常错误UnsupportedEncodingException; 必须对其进行捕获或声明以便抛出
        return s.getBytes("GBK");
                         ^
1 个错误
错误: 编译失败
```

编译器会报错，错误信息类似：unreported exception UnsupportedEncodingException; must be caught or declared to be thrown，并且准确地指出需要捕获的语句是`return s.getBytes("GBK");`。意思是说，像`UnsupportedEncodingException`这样的Checked Exception，必须被捕获。

这是因为`String.getBytes(String)`方法定义是：

```
//定义了throws后，必须要捕获异常
public byte[] getBytes(String charsetName) throws UnsupportedEncodingException {
    ...
}
```

在方法定义的时候，使用`throws Xxx`表示该方法可能抛出的异常类型。调用方在调用的时候，必须强制捕获这些异常，否则编译器会报错。

在`toGBK()`方法中，因为调用了`String.getBytes(String)`方法，就必须捕获`UnsupportedEncodingException`。我们也可以不捕获它，而是在方法定义处用throws表示`toGBK()`方法可能会抛出`UnsupportedEncodingException`，就可以让`toGBK()`方法通过编译器检查：

```java
import java.io.UnsupportedEncodingException;
import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        return s.getBytes("GBK");
    }
}

Main.java:6: 错误: 未报告的异常错误UnsupportedEncodingException; 必须对其进行捕获或声明以便抛出
        byte[] bs = toGBK("中文");
                         ^
1 个错误
错误: 编译失败
```

上述代码仍然会得到编译错误，但这一次，编译器提示的不是调用`return s.getBytes("GBK");`的问题，而是`byte[] bs = toGBK("中文");`。因为在`main()`方法中，调用`toGBK()`，没有捕获它声明的可能抛出的`UnsupportedEncodingException`。

修复方法是在`main()`方法中捕获异常并处理：

```java
import java.io.UnsupportedEncodingException;
import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        try {
            byte[] bs = toGBK("中文");
            System.out.println(Arrays.toString(bs));
        } catch (UnsupportedEncodingException e) {
            System.out.println(e);
        }
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        // 用指定编码转换String为byte[]:
        return s.getBytes("GBK");
    }
}

```

可见，只要是方法声明的Checked Exception，不在调用层捕获，也必须在更高的调用层捕获。所有未捕获的异常，最终也必须在`main()`方法中捕获，不会出现漏写`try`的情况。这是由编译器保证的。`main()`方法也是最后捕获`Exception`的机会。

如果是测试代码，上面的写法就略显麻烦。如果不想写任何`try`代码，可以直接把`main()`方法定义为`throws Exception`：

```java
import java.io.UnsupportedEncodingException;
import java.util.Arrays;
public class Main {
    public static void main(String[] args) throws Exception {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        // 用指定编码转换String为byte[]:
        return s.getBytes("GBK");
    }
}
```

因为`main()`方法声明了可能抛出`Exception`，也就声明了可能抛出所有的`Exception`，因此在内部就无需捕获了。代价就是一旦发生异常，程序会立刻退出。

还有一些童鞋喜欢在`toGBK()`内部“消化”异常：

```java
static byte[] toGBK(String s) {
    try {
        return s.getBytes("GBK");
    } catch (UnsupportedEncodingException e) {
        // 什么也不干
    }
    return null;
```

 这种捕获后不处理的方式是非常不好的，即使真的什么也做不了，也要先把异常记录下来：

```java
static byte[] toGBK(String s) {
    try {
        return s.getBytes("GBK");
    } catch (UnsupportedEncodingException e) {
        // 先记下来再说:
        e.printStackTrace();
    }
    return null;
```

所有异常都可以调用`printStackTrace()`方法打印异常栈，这是一个简单有用的快速打印异常的方法。

#### 4.1.2 小结

Java使用异常来表示错误，并通过`try ... catch`捕获异常；

Java的异常是`class`，并且从`Throwable`继承；

`Error`是无需捕获的严重错误，`Exception`是应该捕获的可处理的错误；

`RuntimeException`无需强制捕获，非`RuntimeException`（Checked Exception）需强制捕获，或者用`throws`声明；

不推荐捕获了异常但不进行任何处理。

### 4.2 捕获异常

 在Java中，凡是可能抛出异常的语句，都可以用`try ... catch`捕获。把可能发生异常的语句放在`try { ... }`中，然后使用`catch`捕获对应的`Exception`及其子类。 

#### 4.2.1 多catch语句

可以使用多个`catch`语句，每个`catch`分别捕获对应的`Exception`及其子类。JVM在捕获到异常后，会从上到下匹配`catch`语句，匹配到某个`catch`后，执行`catch`代码块，然后*不再*继续匹配。

简单地说就是：多个`catch`语句只有一个能被执行。例如：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println(e);
    } catch (NumberFormatException e) {
        System.out.println(e);
    }
}
```

存在多个`catch`的时候，`catch`的顺序非常重要：子类必须写在前面。例如：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println("IO error");
    } catch (UnsupportedEncodingException e) { // 永远捕获不到
        System.out.println("Bad encoding");
    }
}
```

对于上面的代码，`UnsupportedEncodingException`异常是永远捕获不到的，因为它是`IOException`的子类。当抛出`UnsupportedEncodingException`异常时，会被`catch (IOException e) { ... }`捕获并执行。

因此，正确的写法是把子类放到前面：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
    } catch (IOException e) {
        System.out.println("IO error");
    }
}
```

对于上面的代码，`UnsupportedEncodingException`异常是永远捕获不到的，因为它是`IOException`的子类。当抛出`UnsupportedEncodingException`异常时，会被`catch (IOException e) { ... }`捕获并执行。

因此，正确的写法是把子类放到前面：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
    } catch (IOException e) {
        System.out.println("IO error");
    }
}
```

#### 4.2.2 finally语句

无论是否有异常发生，如果我们都希望执行一些语句，例如清理工作，怎么写？

可以把执行语句写若干遍：正常执行的放到`try`中，每个`catch`再写一遍。例如：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
        System.out.println("END");
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
        System.out.println("END");
    } catch (IOException e) {
        System.out.println("IO error");
        System.out.println("END");
    }
}
```

上述代码无论是否发生异常，都会执行`System.out.println("END");`这条语句。

那么如何消除这些重复的代码？Java的`try ... catch`机制还提供了`finally`语句，`finally`语句块保证有无错误都会执行。上述代码可以改写如下：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
    } catch (IOException e) {
        System.out.println("IO error");
    } finally {
        System.out.println("END");
    }
}
```

注意`finally`有几个特点：

1. `finally`语句不是必须的，可写可不写；
2. `finally`总是最后执行。

如果没有发生异常，就正常执行`try { ... }`语句块，然后执行`finally`。如果发生了异常，就中断执行`try { ... }`语句块，然后跳转执行匹配的`catch`语句块，最后执行`finally`。

可见，`finally`是用来保证一些代码必须执行的。

某些情况下，可以没有`catch`，只使用`try ... finally`结构。例如：

```java
void process(String file) throws IOException {
    try {
        ...
    } finally {
        System.out.println("END");
    }
}
```

因为方法声明了可能抛出的异常，所以可以不写`catch`。

#### 4.2.3 捕获多种异常

如果某些异常的处理逻辑相同，但是异常本身不存在继承关系，那么就得编写多条`catch`子句：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println("Bad input");
    } catch (NumberFormatException e) {
        System.out.println("Bad input");
    } catch (Exception e) {
        System.out.println("Unknown error");
    }
}
```

因为处理`IOException`和`NumberFormatException`的代码是相同的，所以我们可以把它两用`|`合并到一起：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException | NumberFormatException e) { // IOException或NumberFormatException
        System.out.println("Bad input");
    } catch (Exception e) {
        System.out.println("Unknown error");
    }
}
```

#### 4.2.4 小结

使用`try ... catch ... finally`时：

- 多个`catch`语句的匹配顺序非常重要，子类必须放在前面；
- `finally`语句保证了有无异常都会执行，它是可选的；
- 一个`catch`语句也可以匹配多个非继承关系的异常。

### 4.3 抛出异常

#### 4.3.1 异常的传播

当某个方法抛出了异常时，如果当前方法没有捕获异常，异常就会被抛到上层调用方法，直到遇到某个`try ... catch`被捕获为止：

```java
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        process2();
    }

    static void process2() {
        Integer.parseInt(null); // 会抛出NumberFormatException
    }
}
```

通过`printStackTrace()`可以打印出方法的调用栈，类似：

```java
java.lang.NumberFormatException: null
    at java.base/java.lang.Integer.parseInt(Integer.java:614)
    at java.base/java.lang.Integer.parseInt(Integer.java:770)
    at Main.process2(Main.java:16)
    at Main.process1(Main.java:12)
    at Main.main(Main.java:5)
```

`printStackTrace()`对于调试错误非常有用，上述信息表示：`NumberFormatException`是在`java.lang.Integer.parseInt`方法中被抛出的，调用层次从上到下依次是：

1. `main()`调用`process1()`；
2. `process1()`调用`process2()`；
3. `process2()`调用`Integer.parseInt(String)`；
4. `Integer.parseInt(String)`调用`Integer.parseInt(String, int)`。

查看`Integer.java`源码可知，抛出异常的方法代码如下：

```java
public static int parseInt(String s, int radix) throws NumberFormatException {
    if (s == null) {
        throw new NumberFormatException("null");
    }
    ...
}
```

并且，每层调用均给出了源代码的行号，可直接定位。

#### 4.3.2 抛出异常

当发生错误时，例如，用户输入了非法的字符，我们就可以抛出异常。

如何抛出异常？参考`Integer.parseInt()`方法，抛出异常分两步：

1. 创建某个`Exception`的实例；
2. 用`throw`语句抛出。

下面是一个例子：

```java
void process2(String s) {
    if (s==null) {
        NullPointerException e = new NullPointerException();
        throw e;
    }
}
```

实际上，绝大部分抛出异常的代码都会合并写成一行：

```java
void process2(String s) {
    if (s==null) {
        throw new NullPointerException();
    }
}
```

如果一个方法捕获了某个异常后，又在`catch`子句中抛出新的异常，就相当于把抛出的异常类型“转换”了：

```java
void process1(String s) {
    try {
        process2();
    } catch (NullPointerException e) {
        throw new IllegalArgumentException();
    }
}

void process2(String s) {
    if (s==null) {
        throw new NullPointerException();
    }
}
```

当`process2()`抛出`NullPointerException`后，被`process1()`捕获，然后抛出`IllegalArgumentException()`。

如果在`main()`中捕获`IllegalArgumentException`，我们看看打印的异常栈：

```java
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
            throw new IllegalArgumentException();
        }
    }

    static void process2() {
        throw new NullPointerException();
    }
}
```

打印出的异常栈类似：

```java
java.lang.IllegalArgumentException
    at Main.process1(Main.java:15)
    at Main.main(Main.java:5)
```

这说明新的异常丢失了原始异常信息，我们已经看不到原始异常`NullPointerException`的信息了。

为了能追踪到完整的异常栈，在构造异常的时候，把原始的`Exception`实例传进去，新的`Exception`就可以持有原始`Exception`信息。对上述代码改进如下：

```java
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
            throw new IllegalArgumentException(e);
        }
    }

    static void process2() {
        throw new NullPointerException();
    }
}
```

运行上述代码，打印出的异常栈类似：

```java
java.lang.IllegalArgumentException: java.lang.NullPointerException
    at Main.process1(Main.java:15)
    at Main.main(Main.java:5)
Caused by: java.lang.NullPointerException
    at Main.process2(Main.java:20)
    at Main.process1(Main.java:13)
```

注意到`Caused by: Xxx`，说明捕获的`IllegalArgumentException`并不是造成问题的根源，根源在于`NullPointerException`，是在`Main.process2()`方法抛出的。

在代码中获取原始异常可以使用`Throwable.getCause()`方法。如果返回`null`，说明已经是“根异常”了。

有了完整的异常栈的信息，我们才能快速定位并修复代码的问题。

如果我们在`try`或者`catch`语句块中抛出异常，`finally`语句是否会执行？例如：

```java
public class Main {
    public static void main(String[] args) {
        try {
            Integer.parseInt("abc");
        } catch (Exception e) {
            System.out.println("catched");
            throw new RuntimeException(e);
        } finally {
            System.out.println("finally");
        }
    }
}
```

上述代码执行结果如下：

```java
catched
finally
Exception in thread "main" java.lang.RuntimeException: java.lang.NumberFormatException: For input string: "abc"
    at Main.main(Main.java:8)
Caused by: java.lang.NumberFormatException: For input string: "abc"
    at ...
```

第一行打印了`catched`，说明进入了`catch`语句块。第二行打印了`finally`，说明执行了`finally`语句块。

因此，在`catch`中抛出异常，不会影响`finally`的执行。JVM会先执行`finally`，然后抛出异常。

#### 4.3.3 异常屏蔽

如果在执行`finally`语句时抛出异常，那么，`catch`语句的异常还能否继续抛出？例如：

```java
public class Main {
    public static void main(String[] args) {
        try {
            Integer.parseInt("abc");
        } catch (Exception e) {
            System.out.println("catched");
            throw new RuntimeException(e);
        } finally {
            System.out.println("finally");
            throw new IllegalArgumentException();
        }
    }
}
```

执行上述代码，发现异常信息如下：

```java
catched
finally
Exception in thread "main" java.lang.IllegalArgumentException
    at Main.main(Main.java:11)
```

这说明`finally`抛出异常后，原来在`catch`中准备抛出的异常就“消失”了，因为只能抛出一个异常。**没有被抛出的异常称为“被屏蔽”的异常**（Suppressed Exception）。

在极少数的情况下，我们需要获知所有的异常。如何保存所有的异常信息？方法是先用`origin`变量保存原始异常，然后调用`Throwable.addSuppressed()`，把原始异常添加进来，最后在`finally`抛出：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        Exception origin = null;
        try {
            System.out.println(Integer.parseInt("abc"));
        } catch (Exception e) {
            origin = e;
            throw e;
        } finally {
            Exception e = new IllegalArgumentException();
            if (origin != null) {
                e.addSuppressed(origin);
            }
            throw e;
        }
    }
}
```

当`catch`和`finally`都抛出了异常时，虽然`catch`的异常被屏蔽了，但是，`finally`抛出的异常仍然包含了它：

```java
Exception in thread "main" java.lang.IllegalArgumentException
    at Main.main(Main.java:11)
Suppressed: java.lang.NumberFormatException: For input string: "abc"
    at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
    at java.base/java.lang.Integer.parseInt(Integer.java:652)
    at java.base/java.lang.Integer.parseInt(Integer.java:770)
    at Main.main(Main.java:6)
```

通过`Throwable.getSuppressed()`可以获取所有的`Suppressed Exception`。

绝大多数情况下，在`finally`中不要抛出异常。因此，我们通常不需要关心`Suppressed Exception`。

#### 4.3.4 小结

调用`printStackTrace()`可以打印异常的传播栈，对于调试非常有用；

捕获异常并再次抛出新的异常时，应该持有原始异常信息；

通常不要在`finally`中抛出异常。如果在`finally`中抛出异常，应该原始异常加入到原有异常中。调用方可通过`Throwable.getSuppressed()`获取所有添加的`Suppressed Exception`。

### 4.4 自定义异常

Java标准库定义的常用异常包括：

```ascii
Exception
│
├─ RuntimeException
│  │
│  ├─ NullPointerException
│  │
│  ├─ IndexOutOfBoundsException
│  │
│  ├─ SecurityException
│  │
│  └─ IllegalArgumentException
│     │
│     └─ NumberFormatException
│
├─ IOException
│  │
│  ├─ UnsupportedCharsetException
│  │
│  ├─ FileNotFoundException
│  │
│  └─ SocketException
│
├─ ParseException
│
├─ GeneralSecurityException
│
├─ SQLException
│
└─ TimeoutException
```

当我们在代码中需要抛出异常时，尽量使用JDK已定义的异常类型。例如，参数检查不合法，应该抛出`IllegalArgumentException`：

```java
static void process1(int age) {
    if (age <= 0) {
        throw new IllegalArgumentException();
    }
}
```

在一个大型项目中，可以自定义新的异常类型，但是，保持一个合理的异常继承体系是非常重要的。

一个常见的做法是自定义一个`BaseException`作为“根异常”，然后，派生出各种业务类型的异常。

`BaseException`需要从一个适合的`Exception`派生，通常建议从`RuntimeException`派生：

```java
public class BaseException extends RuntimeException {
}
```

其他业务类型的异常就可以从`BaseException`派生：

```java
public class UserNotFoundException extends BaseException {
}

public class LoginFailedException extends BaseException {
}

...
```

自定义的`BaseException`应该提供多个构造方法：

```java
public class BaseException extends RuntimeException {
    public BaseException() {
        super();
    }

    public BaseException(String message, Throwable cause) {
        super(message, cause);
    }

    public BaseException(String message) {
        super(message);
    }

    public BaseException(Throwable cause) {
        super(cause);
    }
}
```

上述构造方法实际上都是原样照抄`RuntimeException`。这样，抛出异常的时候，就可以选择合适的构造方法。通过IDE可以根据父类快速生成子类的构造方法。

#### 4.4.1 小结

抛出异常时，尽量复用JDK已定义的异常类型；

自定义异常体系时，推荐从`RuntimeException`派生“根异常”，再派生出业务异常；

自定义异常时，应该提供多种构造方法。

### 4.5 使用断言

断言（Assertion）是一种调试程序的方式。在Java中，使用`assert`关键字来实现断言。

我们先看一个例子：

```java
public static void main(String[] args) {
    double x = Math.abs(-123.45);
    assert x >= 0;
    System.out.println(x);
}
```

语句`assert x >= 0;`即为断言，断言条件`x >= 0`预期为`true`。如果计算结果为`false`，则断言失败，抛出`AssertionError`。

使用`assert`语句时，还可以添加一个可选的断言消息：

```java
assert x >= 0 : "x must >= 0";
```

这样，断言失败的时候，`AssertionError`会带上消息`x must >= 0`，更加便于调试。

Java断言的特点是：断言失败时会抛出`AssertionError`，导致程序结束退出。因此，断言不能用于可恢复的程序错误，只应该用于开发和测试阶段。

对于可恢复的程序错误，不应该使用断言。例如：

```java
void sort(int[] arr) {
    assert arr != null;
}
```

应该抛出异常并在上层捕获：

```java
void sort(int[] arr) {
    if (x == null) {
        throw new IllegalArgumentException("array cannot be null");
    }
}
```

当我们在程序中使用`assert`时，例如，一个简单的断言：

```java
public class Main {
    public static void main(String[] args) {
        int x = -1;
        assert x > 0;
        System.out.println(x);
    }
}
```

断言`x`必须大于`0`，实际上`x`为`-1`，断言肯定失败。执行上述代码，发现程序并未抛出`AssertionError`，而是正常打印了`x`的值。

这是怎么肥四？为什么`assert`语句不起作用？

这是因为JVM默认关闭断言指令，即遇到`assert`语句就自动忽略了，不执行。

要执行`assert`语句，必须给Java虚拟机传递`-enableassertions`（可简写为`-ea`）参数启用断言。所以，上述程序必须在命令行下运行才有效果：

```java
$ java -ea Main.java
Exception in thread "main" java.lang.AssertionError
    at Main.main(Main.java:5)
```

还可以有选择地对特定地类启用断言，命令行参数是：`-ea:com.itranswarp.sample.Main`，表示只对`com.itranswarp.sample.Main`这个类启用断言。

或者对特定地包启用断言，命令行参数是：`-ea:com.itranswarp.sample...`（注意结尾有3个`.`），表示对`com.itranswarp.sample`这个包启动断言。

实际开发中，很少使用断言。更好的方法是编写单元测试，后续我们会讲解`JUnit`的使用。

#### 4.5.1 小结

断言是一种调试方式，断言失败会抛出`AssertionError`，只能在开发和测试阶段启用断言；

对可恢复的错误不能使用断言，而应该抛出异常；

断言很少被使用，更好的方法是编写单元测试。

### 4.6 使用JDK Logging

在编写程序的过程中，发现程序运行结果与预期不符，怎么办？当然是用`System.out.println()`打印出执行过程中的某些变量，观察每一步的结果与代码逻辑是否符合，然后有针对性地修改代码。

代码改好了怎么办？当然是删除没有用的`System.out.println()`语句了。

如果改代码又改出问题怎么办？再加上`System.out.println()`。

反复这么搞几次，很快大家就发现使用`System.out.println()`非常麻烦。

怎么办？

解决方法是使用日志。

那什么是日志？日志就是Logging，它的目的是为了取代`System.out.println()`。

输出日志，而不是用`System.out.println()`，有以下几个好处：

1. 可以设置输出样式，避免自己每次都写`"ERROR: " + var`；
2. 可以设置输出级别，禁止某些级别输出。例如，只输出错误日志；
3. 可以被重定向到文件，这样可以在程序运行结束后查看日志；
4. 可以按包名控制日志级别，只输出某些包打的日志；
5. 可以……

总之就是好处很多啦。

那如何使用日志？

因为Java标准库内置了日志包`java.util.logging`，我们可以直接用。先看一个简单的例子：

```java
import java.util.logging.Level;
import java.util.logging.Logger;
public class Hello {
    public static void main(String[] args) {
        Logger logger = Logger.getGlobal();
        logger.info("start process...");
        logger.warning("memory is running out...");
        logger.fine("ignored.");
        logger.severe("process will be terminated...");
    }
}
```

再仔细观察发现，4条日志，只打印了3条，`logger.fine()`没有打印。这是因为，日志的输出可以设定级别。JDK的Logging定义了7个日志级别，从严重到普通：

- SEVERE
- WARNING
- INFO
- CONFIG
- FINE
- FINER
- FINEST

因为默认级别是INFO，因此，INFO级别以下的日志，不会被打印出来。使用日志级别的好处在于，调整级别，就可以屏蔽掉很多调试相关的日志输出。

使用Java标准库内置的Logging有以下局限：

Logging系统在JVM启动时读取配置文件并完成初始化，一旦开始运行`main()`方法，就无法修改配置；

配置不太方便，需要在JVM启动时传递参数`-Djava.util.logging.config.file=`。

因此，Java标准库内置的Logging使用并不是非常广泛。

#### 4.6.1 小结

日志是为了替代`System.out.println()`，可以定义格式，重定向到文件等；

日志可以存档，便于追踪问题；

日志记录可以按级别分类，便于打开或关闭某些级别；

可以根据配置文件调整日志，无需修改代码；

Java标准库提供了`java.util.logging`来实现日志功能。

### 4.7 使用Commons Logging

和Java标准库提供的日志不同，Commons Logging是一个第三方日志库，它是由Apache创建的日志模块。

Commons Logging的特色是，它可以挂接不同的日志系统，并通过配置文件指定挂接的日志系统。默认情况下，Commons Loggin自动搜索并使用Log4j（Log4j是另一个流行的日志系统），如果没有找到Log4j，再使用JDK Logging。

使用Commons Logging只需要和两个类打交道，并且只有两步：

第一步，通过`LogFactory`获取`Log`类的实例； 第二步，使用`Log`实例的方法打日志。

示例代码如下：

```java
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
public class Main {
    public static void main(String[] args) {
        Log log = LogFactory.getLog(Main.class);
        log.info("start...");
        log.warn("end.");
    }
}

```

运行上述代码，肯定会得到编译错误，类似`error: package org.apache.commons.logging does not exist`（找不到`org.apache.commons.logging`这个包）。因为Commons Logging是一个第三方提供的库，所以，必须先把它[下载](https://commons.apache.org/proper/commons-logging/download_logging.cgi)下来。下载后，解压，找到`commons-logging-1.2.jar`这个文件，再把Java源码`Main.java`放到一个目录下，例如`work`目录：

```ascii
work
│
├─ commons-logging-1.2.jar
│
└─ Main.java
```

然后用`javac`编译`Main.java`，编译的时候要指定`classpath`，不然编译器找不到我们引用的`org.apache.commons.logging`包。编译命令如下：

```shell
javac -cp commons-logging-1.2.jar Main.java
```

如果编译成功，那么当前目录下就会多出一个`Main.class`文件：

```ascii
work
│
├─ commons-logging-1.2.jar
│
├─ Main.java
│
└─ Main.class
```

现在可以执行这个`Main.class`，使用`java`命令，也必须指定`classpath`，命令如下：

```shell
java -cp .;commons-logging-1.2.jar Main
```

注意到传入的`classpath`有两部分：一个是`.`，一个是`commons-logging-1.2.jar`，用`;`分割。`.`表示当前目录，如果没有这个`.`，JVM不会在当前目录搜索`Main.class`，就会报错。

如果在Linux或macOS下运行，注意`classpath`的分隔符不是`;`，而是`:`：

```shell
java -cp .:commons-logging-1.2.jar Main
```

运行结果如下：

```shell
Mar 02, 2019 7:15:31 PM Main main
INFO: start...
Mar 02, 2019 7:15:31 PM Main main
WARNING: end.
```

Commons Logging定义了6个日志级别：

- FATAL
- ERROR
- WARNING
- INFO
- DEBUG
- TRACE

默认级别是`INFO`。

使用Commons Logging时，如果在静态方法中引用`Log`，通常直接定义一个静态类型变量：

```java
// 在静态方法中引用Log:
public class Main {
    static final Log log = LogFactory.getLog(Main.class);

    static void foo() {
        log.info("foo");
    }
}
```

在实例方法中引用`Log`，通常定义一个实例变量：

```java
// 在实例方法中引用Log:
public class Person {
    protected final Log log = LogFactory.getLog(getClass());

    void foo() {
        log.info("foo");
    }
}
```

注意到实例变量log的获取方式是`LogFactory.getLog(getClass())`，虽然也可以用`LogFactory.getLog(Person.class)`，但是前一种方式有个非常大的好处，就是子类可以直接使用该`log`实例。例如：

```java
// 在子类中使用父类实例化的log:
public class Student extends Person {
    void bar() {
        log.info("bar");
    }
}
```

由于Java类的动态特性，子类获取的`log`字段实际上相当于`LogFactory.getLog(Student.class)`，但却是从父类继承而来，并且无需改动代码。

此外，Commons Logging的日志方法，例如`info()`，除了标准的`info(String)`外，还提供了一个非常有用的重载方法：`info(String, Throwable)`，这使得记录异常更加简单：

```java
try {
    ...
} catch (Exception e) {
    log.error("got exception!", e);
}
```

#### 4.7.1小结

Commons Logging是使用最广泛的日志模块；

Commons Logging的API非常简单；

Commons Logging可以自动检测并使用其他日志模块。

### 4.8 使用Log4j

前面介绍了Commons Logging，可以作为“日志接口”来使用。而真正的“日志实现”可以使用Log4j。

Log4j是一种非常流行的日志框架，最新版本是2.x。

Log4j是一个组件化设计的日志系统，它的架构大致如下：

```ascii
log.info("User signed in.");
 │
 │   ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
 ├──>│ Appender │───>│  Filter  │───>│  Layout  │───>│ Console  │
 │   └──────────┘    └──────────┘    └──────────┘    └──────────┘
 │
 │   ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
 ├──>│ Appender │───>│  Filter  │───>│  Layout  │───>│   File   │
 │   └──────────┘    └──────────┘    └──────────┘    └──────────┘
 │
 │   ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
 └──>│ Appender │───>│  Filter  │───>│  Layout  │───>│  Socket  │
     └──────────┘    └──────────┘    └──────────┘    └──────────┘
```

当我们使用Log4j输出一条日志时，Log4j自动通过不同的Appender把同一条日志输出到不同的目的地。例如：

- console：输出到屏幕；
- file：输出到文件；
- socket：通过网络输出到远程计算机；
- jdbc：输出到数据库

在输出日志的过程中，通过Filter来过滤哪些log需要被输出，哪些log不需要被输出。例如，仅输出`ERROR`级别的日志。

最后，通过Layout来格式化日志信息，例如，自动添加日期、时间、方法名称等信息。

上述结构虽然复杂，但我们在实际使用的时候，并不需要关心Log4j的API，而是通过配置文件来配置它。

以XML配置为例，使用Log4j的时候，我们把一个`log4j2.xml`的文件放到`classpath`下就可以让Log4j读取配置文件并按照我们的配置来输出日志。下面是一个配置文件的例子：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
  <Properties>
        <!-- 定义日志格式 -->
        <Property name="log.pattern">%d{MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36}%n%msg%n%n</Property>
        <!-- 定义文件名变量 -->
        <Property name="file.err.filename">log/err.log</Property>
        <Property name="file.err.pattern">log/err.%i.log.gz</Property>
    </Properties>
    <!-- 定义Appender，即目的地 -->
    <Appenders>
        <!-- 定义输出到屏幕 -->
        <Console name="console" target="SYSTEM_OUT">
            <!-- 日志格式引用上面定义的log.pattern -->
            <PatternLayout pattern="${log.pattern}" />
        </Console>
        <!-- 定义输出到文件,文件名引用上面定义的file.err.filename -->
        <RollingFile name="err" bufferedIO="true" fileName="${file.err.filename}" filePattern="${file.err.pattern}">
            <PatternLayout pattern="${log.pattern}" />
            <Policies>
                <!-- 根据文件大小自动切割日志 -->
                <SizeBasedTriggeringPolicy size="1 MB" />
            </Policies>
            <!-- 保留最近10份 -->
            <DefaultRolloverStrategy max="10" />
        </RollingFile>
    </Appenders>
    <Loggers>
        <Root level="info">
            <!-- 对info级别的日志，输出到console -->
            <AppenderRef ref="console" level="info" />
            <!-- 对error级别的日志，输出到err，即上面定义的RollingFile -->
            <AppenderRef ref="err" level="error" />
        </Root>
    </Loggers>
</Configuration>
```

虽然配置Log4j比较繁琐，但一旦配置完成，使用起来就非常方便。对上面的配置文件，凡是`INFO`级别的日志，会自动输出到屏幕，而`ERROR`级别的日志，不但会输出到屏幕，还会同时输出到文件。并且，一旦日志文件达到指定大小（1MB），Log4j就会自动切割新的日志文件，并最多保留10份。

有了配置文件还不够，因为Log4j也是一个第三方库，我们需要从[这里](https://logging.apache.org/log4j/2.x/download.html)下载Log4j，解压后，把以下3个jar包放到`classpath`中：

- log4j-api-2.x.jar
- log4j-core-2.x.jar
- log4j-jcl-2.x.jar

因为Commons Logging会自动发现并使用Log4j，所以，把上一节下载的`commons-logging-1.2.jar`也放到`classpath`中。要打印日志，只需要按Commons Logging的写法写，不需要改动任何代码，就可以得到Log4j的日志输出，类似：

```shell
03-03 12:09:45.880 [main] INFO  com.itranswarp.learnjava.Main
Start process...
```

#### 4.8.1 最佳实践

在开发阶段，始终使用Commons Logging接口来写入日志，并且开发阶段无需引入Log4j。如果需要把日志写入文件， 只需要把正确的配置文件和Log4j相关的jar包放入`classpath`，就可以自动把日志切换成使用Log4j写入，无需修改任何代码。

#### 4.8.2 小结

通过Commons Logging实现日志，不需要修改代码即可使用Log4j 使用Log4j只需要把log4j2.xml和相关jar放入classpath 如果要更换Log4j，只需要移除log4j2.xml和相关jar 只有扩展Log4j时，才需要引用Log4j的接口（例如，将日志加密写入数据库的功能，需要自己开发）

### 4.9 使用SLF4J和Logback

前面介绍了Commons Logging和Log4j这一对好基友，它们一个负责充当日志API，一个负责实现日志底层，搭配使用非常便于开发。有的童鞋可能还听说过SLF4J和Logback。这两个东东看上去也像日志，它们又是啥？其实SLF4J类似于Commons Logging，也是一个日志接口，而Logback类似于Log4j，是一个日志的实现。为什么有了Commons Logging和Log4j，又会蹦出来SLF4J和Logback？这是因为Java有着非常悠久的开源历史，不但OpenJDK本身是开源的，而且我们用到的第三方库，几乎全部都是开源的。开源生态丰富的一个特定就是，同一个功能，可以找到若干种互相竞争的开源库。因为对Commons Logging的接口不满意，有人就搞了SLF4J。因为对Log4j的性能不满意，有人就搞了Logback。我们先来看看SLF4J对Commons Logging的接口有何改进。在Commons Logging中，我们要打印日志，有时候得这么写：

```java
int score = 99;
p.setScore(score);
log.info("Set score " + score + " for Person " + p.getName() + " ok.");
```

拼字符串是一个非常麻烦的事情，所以SLF4J的日志接口改进成这样了：

```java
int score = 99;
p.setScore(score);
logger.info("Set score {} for Person {} ok.", score, p.getName());
```

我们靠猜也能猜出来，SLF4J的日志接口传入的是一个带占位符的字符串，用后面的变量自动替换占位符，所以看起来更加自然。如何使用SLF4J？它的接口实际上和Commons Logging几乎一模一样：

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

class Main {
    final Logger logger = LoggerFactory.getLogger(getClass());
}
```

对比一下Commons Logging和SLF4J的接口：

| Commons Logging                       | SLF4J                   |
| :------------------------------------ | :---------------------- |
| org.apache.commons.logging.Log        | org.slf4j.Logger        |
| org.apache.commons.logging.LogFactory | org.slf4j.LoggerFactory |

不同之处就是Log变成了Logger，LogFactory变成了LoggerFactory。

使用SLF4J和Logback和前面讲到的使用Commons Logging加Log4j是类似的，先分别下载[SLF4J](https://www.slf4j.org/download.html)和[Logback](https://logback.qos.ch/download.html)，然后把以下jar包放到classpath下：

- slf4j-api-1.7.x.jar
- logback-classic-1.2.x.jar
- logback-core-1.2.x.jar

然后使用SLF4J的Logger和LoggerFactory即可。和Log4j类似，我们仍然需要一个Logback的配置文件，把`logback.xml`放到classpath下，配置如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
 <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
  <encoder>
   <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
  </encoder>
 </appender>

 <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
  <encoder>
   <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
   <charset>utf-8</charset>
  </encoder>
  <file>log/output.log</file>
  <rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
   <fileNamePattern>log/output.log.%i</fileNamePattern>
  </rollingPolicy>
  <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
   <MaxFileSize>1MB</MaxFileSize>
  </triggeringPolicy>
 </appender>

 <root level="INFO">
  <appender-ref ref="CONSOLE" />
  <appender-ref ref="FILE" />
 </root>
</configuration>
```

运行即可获得类似如下的输出：

``` java
13:15:25.328 [main] INFO  com.itranswarp.learnjava.Main - Start process...
```

从目前的趋势来看，越来越多的开源项目从Commons Logging加Log4j转向了SLF4J加Logback。

#### 4.9.1 小结

SLF4J和Logback可以取代Commons Logging和Log4j；始终使用SLF4J的接口写入日志，使用Logback只需要配置，不需要修改代码。

## 5 反射

什么是反射？**反射就是Reflection，Java的反射是指程序在运行期可以拿到一个对象的所有信息。**正常情况下，如果我们要调用一个对象的方法，或者访问一个对象的字段，通常会传入对象实例：

```java
// Main.java
import com.itranswarp.learnjava.Person;

public class Main {
    String getFullName(Person p) {
        return p.getFirstName() + " " + p.getLastName();
    }
}
```

但是，如果不能获得`Person`类，只有一个`Object`实例，比如这样：

```java
String getFullName(Object obj) {
    return ???
}
```

怎么办？有童鞋会说：强制转型啊！

```java
String getFullName(Object obj) {
    Person p = (Person) obj;
    return p.getFirstName() + " " + p.getLastName();
}
```

强制转型的时候，你会发现一个问题：编译上面的代码，仍然需要引用`Person`类。不然，去掉`import`语句，你看能不能编译通过？所以，反射是为了解决在运行期，对某个实例一无所知的情况下，如何调用其方法。

### 5.1 Class类

除了`int`等基本类型外，Java的其他类型全部都是`class`（包括`interface`）。例如：

- `String`
- `Object`
- `Runnable`
- `Exception`
- ...

仔细思考，我们可以得出结论：`class`（包括`interface`）的本质是数据类型（`Type`）。无继承关系的数据类型无法赋值：

```java
Number n = new Double(123.456); // OK
String s = new Double(123.456); // compile error!
```

而`class`是由JVM在执行过程中动态加载的。JVM在第一次读取到一种`class`类型时，将其加载进内存。每加载一种`class`，JVM就为其创建一个`Class`类型的实例，并关联起来。注意：这里的`Class`类型是一个名叫`Class`的`class`。它长这样：

```java
public final class Class {
    private Class() {}
}
```

以`String`类为例，当JVM加载`String`类时，它首先读取`String.class`文件到内存，然后，为`String`类创建一个`Class`实例并关联起来：

```java
Class cls = new Class(String);
```

这个`Class`实例是JVM内部创建的，如果我们查看JDK源码，可以发现`Class`类的构造方法是`private`，只有JVM能创建`Class`实例，我们自己的Java程序是无法创建`Class`实例的。所以，JVM持有的每个`Class`实例都指向一个数据类型（`class`或`interface`）：

```ascii
┌───────────────────────────┐
│      Class Instance       │──────> String
├───────────────────────────┤
│name = "java.lang.String"  │
└───────────────────────────┘
┌───────────────────────────┐
│      Class Instance       │──────> Random
├───────────────────────────┤
│name = "java.util.Random"  │
└───────────────────────────┘
┌───────────────────────────┐
│      Class Instance       │──────> Runnable
├───────────────────────────┤
│name = "java.lang.Runnable"│
└───────────────────────────┘
```

一个`Class`实例包含了该`class`的所有完整信息：

```ascii
┌───────────────────────────┐
│      Class Instance       │──────> String
├───────────────────────────┤
│name = "java.lang.String"  │
├───────────────────────────┤
│package = "java.lang"      │
├───────────────────────────┤
│super = "java.lang.Object" │
├───────────────────────────┤
│interface = CharSequence...│
├───────────────────────────┤
│field = value[],hash,...   │
├───────────────────────────┤
│method = indexOf()...      │
└───────────────────────────┘
```

由于JVM为每个加载的`class`创建了对应的`Class`实例，并在实例中保存了该`class`的所有信息，包括类名、包名、父类、实现的接口、所有方法、字段等，因此，如果获取了某个`Class`实例，我们就可以通过这个`Class`实例获取到该实例对应的`class`的所有信息。

这种通过`Class`实例获取`class`信息的方法称为反射（Reflection）。如何获取一个`class`的`Class`实例？有三个方法：

方法一：直接通过一个`class`的静态变量`class`获取：

```java
Class cls = String.class;
```

方法二：如果我们有一个实例变量，可以通过该实例变量提供的`getClass()`方法获取：

```java
String s = "Hello";
Class cls = s.getClass();
```

方法三：如果知道一个`class`的完整类名，可以通过静态方法`Class.forName()`获取：

```java
Class cls = Class.forName("java.lang.String");
```

因为`Class`实例在JVM中是唯一的，所以，上述方法获取的`Class`实例是同一个实例。可以用`==`比较两个`Class`实例：

```java
Class cls1 = String.class;

String s = "Hello";
Class cls2 = s.getClass();

boolean sameClass = cls1 == cls2; // true
```

注意一下`Class`实例比较和`instanceof`的差别：

```java
Integer n = new Integer(123);

boolean b1 = n instanceof Integer; // true，因为n是Integer类型
boolean b2 = n instanceof Number; // true，因为n是Number类型的子类

boolean b3 = n.getClass() == Integer.class; // true，因为n.getClass()返回Integer.class
boolean b4 = n.getClass() == Number.class; // false，因为Integer.class!=Number.class
```

用`instanceof`不但匹配指定类型，还匹配指定类型的子类。而用`==`判断`class`实例可以精确地判断数据类型，但不能作子类型比较。通常情况下，我们应该用`instanceof`判断数据类型，因为面向抽象编程的时候，我们不关心具体的子类型。只有在需要精确判断一个类型是不是某个`class`的时候，我们才使用`==`判断`class`实例。因为**反射的目的是为了获得某个实例的信息**。因此，当我们拿到某个`Object`实例时，我们可以通过反射获取该`Object`的`class`信息：

```java
void printObjectInfo(Object obj) {
    Class cls = obj.getClass();
}
```

要从`Class`实例获取获取的基本信息，参考下面的代码：

```java
public class Main {
    public static void main(String[] args) {
        printClassInfo("".getClass());
        printClassInfo(Runnable.class);
        printClassInfo(java.time.Month.class);
        printClassInfo(String[].class);
        printClassInfo(int.class);
    }

    static void printClassInfo(Class cls) {
        System.out.println("Class name: " + cls.getName());
        System.out.println("Simple name: " + cls.getSimpleName());
        if (cls.getPackage() != null) {
            System.out.println("Package name: " + cls.getPackage().getName());
        }
        System.out.println("is interface: " + cls.isInterface());
        System.out.println("is enum: " + cls.isEnum());
        System.out.println("is array: " + cls.isArray());
        System.out.println("is primitive: " + cls.isPrimitive());
    }
}
```

注意到数组（例如`String[]`）也是一种`Class`，而且不同于`String.class`，它的类名是`[Ljava.lang.String`。此外，JVM为每一种基本类型如int也创建了`Class`，通过`int.class`访问。

如果获取到了一个`Class`实例，我们就可以通过该`Class`实例来创建对应类型的实例：

```java
// 获取String的Class实例:
Class cls = String.class;
// 创建一个String实例:
String s = (String) cls.newInstance();
```

上述代码相当于`new String()`。通过`Class.newInstance()`可以创建类实例，它的局限是：只能调用`public`的无参数构造方法。带参数的构造方法，或者非`public`的构造方法都无法通过`Class.newInstance()`被调用。

#### 5.1.1 动态加载

JVM在执行Java程序的时候，并不是一次性把所有用到的class全部加载到内存，而是第一次需要用到class时才加载。例如：

```java
// Main.java
public class Main {
    public static void main(String[] args) {
        if (args.length > 0) {
            create(args[0]);
        }
    }

    static void create(String name) {
        Person p = new Person(name);
    }
}
```

当执行`Main.java`时，由于用到了`Main`，因此，JVM首先会把`Main.class`加载到内存。然而，并不会加载`Person.class`，除非程序执行到`create()`方法，JVM发现需要加载`Person`类时，才会首次加载`Person.class`。如果没有执行`create()`方法，那么`Person.class`根本就不会被加载。这就是JVM动态加载`class`的特性。

动态加载`class`的特性对于Java程序非常重要。利用JVM动态加载`class`的特性，我们才能在运行期根据条件加载不同的实现类。例如，Commons Logging总是优先使用Log4j，只有当Log4j不存在时，才使用JDK的logging。利用JVM动态加载特性，大致的实现代码如下：

```java
// Commons Logging优先使用Log4j:
LogFactory factory = null;
if (isClassPresent("org.apache.logging.log4j.Logger")) {
    factory = createLog4j();
} else {
    factory = createJdkLog();
}

boolean isClassPresent(String name) {
    try {
        Class.forName(name);
        return true;
    } catch (Exception e) {
        return false;
    }
}
```

这就是为什么我们只需要把Log4j的jar包放到classpath中，Commons Logging就会自动使用Log4j的原因。

#### 5.1.2 小结

JVM为每个加载的`class`及`interface`创建了对应的`Class`实例来保存`class`及`interface`的所有信息；

获取一个`class`对应的`Class`实例后，就可以获取该`class`的所有信息；

通过Class实例获取`class`信息的方法称为反射（Reflection）；

JVM总是动态加载`class`，可以在运行期根据条件来控制加载class。

### 5.2 访问字段

对任意的一个`Object`实例，只要我们获取了它的`Class`，就可以获取它的一切信息。

我们先看看如何通过`Class`实例获取字段信息。`Class`类提供了以下几个方法来获取字段：

- Field getField(name)：根据字段名获取某个public的field（包括父类）
- Field getDeclaredField(name)：根据字段名获取当前类的某个field（不包括父类）
- Field[] getFields()：获取所有public的field（包括父类）
- Field[] getDeclaredFields()：获取当前类的所有field（不包括父类）

我们来看一下示例代码：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        Class stdClass = Student.class;
        // 获取public字段"score":
        System.out.println(stdClass.getField("score"));
        // 获取继承的public字段"name":
        System.out.println(stdClass.getField("name"));
        // 获取private字段"grade":
        System.out.println(stdClass.getDeclaredField("grade"));
    }
}

class Student extends Person {
    public int score;
    private int grade;
}

class Person {
    public String name;
}
```

上述代码首先获取`Student`的`Class`实例，然后，分别获取`public`字段、继承的`public`字段以及`private`字段，打印出的`Field`类似：

```java
public int Student.score
public java.lang.String Person.name
private int Student.grade
```

一个`Field`对象包含了一个字段的所有信息：

- `getName()`：返回字段名称，例如，`"name"`；
- `getType()`：返回字段类型，也是一个`Class`实例，例如，`String.class`；
- `getModifiers()`：返回字段的修饰符，它是一个`int`，不同的bit表示不同的含义。

以`String`类的`value`字段为例，它的定义是：

```java
public final class String {
    private final byte[] value;
}
```

我们用反射获取该字段的信息，代码如下：

```java
Field f = String.class.getDeclaredField("value");
f.getName(); // "value"
f.getType(); // class [B 表示byte[]类型
int m = f.getModifiers();
Modifier.isFinal(m); // true
Modifier.isPublic(m); // false
Modifier.isProtected(m); // false
Modifier.isPrivate(m); // true
Modifier.isStatic(m); // false
```

#### 5.2.1 获取字段值

利用反射拿到字段的一个`Field`实例只是第一步，我们还可以拿到一个实例对应的该字段的值。例如，对于一个`Person`实例，我们可以先拿到`name`字段对应的`Field`，再获取这个实例的`name`字段的值：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        Object p = new Person("Xiao Ming");
        Class c = p.getClass();
        Field f = c.getDeclaredField("name");
        Object value = f.get(p);
        System.out.println(value); // "Xiao Ming"
    }
}

class Person {
    private String name;
    public Person(String name) {
        this.name = name;
    }
}
```

上述代码先获取`Class`实例，再获取`Field`实例，然后，用`Field.get(Object)`获取指定实例的指定字段的值。

运行代码，如果不出意外，会得到一个`IllegalAccessException`，这是因为`name`被定义为一个`private`字段，正常情况下，`Main`类无法访问`Person`类的`private`字段。要修复错误，可以将`private`改为`public`，或者，在调用`Object value = f.get(p);`前，先写一句：

```java
f.setAccessible(true);
```

调用`Field.setAccessible(true)`的意思是，别管这个字段是不是`public`，一律允许访问。可以试着加上上述语句，再运行代码，就可以打印出`private`字段的值。有童鞋会问：如果使用反射可以获取`private`字段的值，那么类的封装还有什么意义？答案是正常情况下，我们总是通过`p.name`来访问`Person`的`name`字段，编译器会根据`public`、`protected`和`private`决定是否允许访问字段，这样就达到了数据封装的目的。

而反射是一种非常规的用法，使用反射，首先代码非常繁琐，其次，它更多地是给工具或者底层框架来使用，目的是在不知道目标实例任何信息的情况下，获取特定字段的值。

此外，`setAccessible(true)`可能会失败。如果JVM运行期存在`SecurityManager`，那么它会根据规则进行检查，有可能阻止`setAccessible(true)`。例如，某个`SecurityManager`可能不允许对`java`和`javax`开头的`package`的类调用`setAccessible(true)`，这样可以保证JVM核心库的安全。

#### 5.2.2 设置字段值

通过Field实例既然可以获取到指定实例的字段值，自然也可以设置字段的值。设置字段值是通过`Field.set(Object, Object)`实现的，其中第一个`Object`参数是指定的实例，第二个`Object`参数是待修改的值。示例代码如下：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        Person p = new Person("Xiao Ming");
        System.out.println(p.getName()); // "Xiao Ming"
        Class c = p.getClass();
        Field f = c.getDeclaredField("name");
        f.setAccessible(true);
        f.set(p, "Xiao Hong");
        System.out.println(p.getName()); // "Xiao Hong"
    }
}

class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }
}
```

运行上述代码，打印的`name`字段从`Xiao Ming`变成了`Xiao Hong`，说明通过反射可以直接修改字段的值。同样的，修改非`public`字段，需要首先调用`setAccessible(true)`。

#### 5.2.3 小结

Java的反射API提供的`Field`类封装了字段的所有信息：

通过`Class`实例的方法可以获取`Field`实例：`getField()`，`getFields()`，`getDeclaredField()`，`getDeclaredFields()`；

通过Field实例可以获取字段信息：`getName()`，`getType()`，`getModifiers()`；

通过Field实例可以读取或设置某个对象的字段，如果存在访问限制，要首先调用`setAccessible(true)`来访问非`public`字段。

通过反射读写字段是一种非常规方法，它会破坏对象的封装。

### 5.3 调用方法

我们已经能通过`Class`实例获取所有`Field`对象，同样的，可以通过`Class`实例获取所有`Method`信息。`Class`类提供了以下几个方法来获取`Method`：

- `Method getMethod(name, Class...)`：获取某个`public`的`Method`（包括父类）
- `Method getDeclaredMethod(name, Class...)`：获取当前类的某个`Method`（不包括父类）
- `Method[] getMethods()`：获取所有`public`的`Method`（包括父类）
- `Method[] getDeclaredMethods()`：获取当前类的所有`Method`（不包括父类）

```java
public class Main {
    public static void main(String[] args) throws Exception {
        Class stdClass = Student.class;
        // 获取public方法getScore，参数为String:
        System.out.println(stdClass.getMethod("getScore", String.class));
        // 获取继承的public方法getName，无参数:
        System.out.println(stdClass.getMethod("getName"));
        // 获取private方法getGrade，参数为int:
        System.out.println(stdClass.getDeclaredMethod("getGrade", int.class));
    }
}

class Student extends Person {
    public int getScore(String type) {
        return 99;
    }
    private int getGrade(int year) {
        return 1;
    }
}

class Person {
    public String getName() {
        return "Person";
    }
}
```

上述代码首先获取`Student`的`Class`实例，然后，分别获取`public`方法、继承的`public`方法以及`private`方法，打印出的`Method`类似：

```java
public int Student.getScore(java.lang.String)
public java.lang.String Person.getName()
private int Student.getGrade(int)
```

一个`Method`对象包含一个方法的所有信息：

- `getName()`：返回方法名称，例如：`"getScore"`；
- `getReturnType()`：返回方法返回值类型，也是一个Class实例，例如：`String.class`；
- `getParameterTypes()`：返回方法的参数类型，是一个Class数组，例如：`{String.class, int.class}`；
- `getModifiers()`：返回方法的修饰符，它是一个`int`，不同的bit表示不同的含义。

#### 5.3.1 调用方法

当我们获取到一个`Method`对象时，就可以对它进行调用。我们以下面的代码为例：

```java
String s = "Hello world";
String r = s.substring(6); // "world"
```

如果用反射来调用`substring`方法，需要以下代码：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        // String对象:
        String s = "Hello world";
        // 获取String substring(int)方法，参数为int:
        Method m = String.class.getMethod("substring", int.class);
        // 在s对象上调用该方法并获取结果:
        String r = (String) m.invoke(s, 6);
        // 打印调用结果:
        System.out.println(r);
    }
}
```

注意到`substring()`有两个重载方法，我们获取的是`String substring(int)`这个方法。思考一下如何获取`String substring(int, int)`方法。

对`Method`实例调用`invoke`就相当于调用该方法，`invoke`的第一个参数是对象实例，即在哪个实例上调用该方法，后面的可变参数要与方法参数一致，否则将报错。

#### 5.3.2 调用静态方法

如果获取到的Method表示一个静态方法，调用静态方法时，由于无需指定实例对象，所以`invoke`方法传入的第一个参数永远为`null`。我们以`Integer.parseInt(String)`为例：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        // 获取Integer.parseInt(String)方法，参数为String:
        Method m = Integer.class.getMethod("parseInt", String.class);
        // 调用该静态方法并获取结果:
        Integer n = (Integer) m.invoke(null, "12345");
        // 打印调用结果:
        System.out.println(n);
    }
}
```

#### 5.3.3 调用非public方法

和Field类似，对于非public方法，我们虽然可以通过`Class.getDeclaredMethod()`获取该方法实例，但直接对其调用将得到一个`IllegalAccessException`。为了调用非public方法，我们通过`Method.setAccessible(true)`允许其调用：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        Person p = new Person();
        Method m = p.getClass().getDeclaredMethod("setName", String.class);
        m.setAccessible(true);
        m.invoke(p, "Bob");
        System.out.println(p.name);
    }
}

class Person {
    String name;
    private void setName(String name) {
        this.name = name;
    }
}
```

此外，`setAccessible(true)`可能会失败。如果JVM运行期存在`SecurityManager`，那么它会根据规则进行检查，有可能阻止`setAccessible(true)`。例如，某个`SecurityManager`可能不允许对`java`和`javax`开头的`package`的类调用`setAccessible(true)`，这样可以保证JVM核心库的安全。

#### 5.3.4 多态

我们来考察这样一种情况：一个`Person`类定义了`hello()`方法，并且它的子类`Student`也覆写了`hello()`方法，那么，从`Person.class`获取的`Method`，作用于`Student`实例时，调用的方法到底是哪个？

```java
public class Main {
    public static void main(String[] args) throws Exception {
        // 获取Person的hello方法:
        Method h = Person.class.getMethod("hello");
        // 对Student实例调用hello方法:
        h.invoke(new Student());
    }
}

class Person {
    public void hello() {
        System.out.println("Person:hello");
    }
}

class Student extends Person {
    public void hello() {
        System.out.println("Student:hello");
    }
}
```

运行上述代码，发现打印出的是`Student:hello`，因此，使用反射调用方法时，仍然遵循多态原则：即总是调用实际类型的覆写方法（如果存在）。上述的反射代码：

```java
Method m = Person.class.getMethod("hello");
m.invoke(new Student());
```

实际上相当于：

```java
Person p = new Student();
p.hello();
```

#### 5.3.5 小结

Java的反射API提供的Method对象封装了方法的所有信息：

通过`Class`实例的方法可以获取`Method`实例：`getMethod()`，`getMethods()`，`getDeclaredMethod()`，`getDeclaredMethods()`；

通过`Method`实例可以获取方法信息：`getName()`，`getReturnType()`，`getParameterTypes()`，`getModifiers()`；

通过`Method`实例可以调用某个对象的方法：`Object invoke(Object instance, Object... parameters)`；

通过设置`setAccessible(true)`来访问非`public`方法；

通过反射调用方法时，仍然遵循多态原则。

### 5.4 调用构造方法

我们通常使用`new`操作符创建新的实例：

```java
Person p = new Person();
```

如果通过反射来创建新的实例，可以调用Class提供的newInstance()方法：

```java
Person p = Person.class.newInstance();
```

调用Class.newInstance()的局限是，它只能调用该类的public无参数构造方法。如果构造方法带有参数，或者不是public，就无法直接通过Class.newInstance()来调用。

为了调用任意的构造方法，Java的反射API提供了Constructor对象，它包含一个构造方法的所有信息，可以创建一个实例。Constructor对象和Method非常类似，不同之处仅在于它是一个构造方法，并且，调用结果总是返回实例：

```java
import java.lang.reflect.Constructor;
public class Main {
    public static void main(String[] args) throws Exception {
        // 获取构造方法Integer(int):
        Constructor cons1 = Integer.class.getConstructor(int.class);
        // 调用构造方法:
        Integer n1 = (Integer) cons1.newInstance(123);
        System.out.println(n1);

        // 获取构造方法Integer(String)
        Constructor cons2 = Integer.class.getConstructor(String.class);
        Integer n2 = (Integer) cons2.newInstance("456");
        System.out.println(n2);
    }
}
```

通过Class实例获取Constructor的方法如下：

- `getConstructor(Class...)`：获取某个`public`的`Constructor`；
- `getDeclaredConstructor(Class...)`：获取某个`Constructor`；
- `getConstructors()`：获取所有`public`的`Constructor`；
- `getDeclaredConstructors()`：获取所有`Constructor`。

注意`Constructor`总是当前类定义的构造方法，和父类无关，因此不存在多态的问题。

调用非`public`的`Constructor`时，必须首先通过`setAccessible(true)`设置允许访问。`setAccessible(true)`可能会失败。

#### 5.4.1 小结

`Constructor`对象封装了构造方法的所有信息；

通过`Class`实例的方法可以获取`Constructor`实例：`getConstructor()`，`getConstructors()`，`getDeclaredConstructor()`，`getDeclaredConstructors()`；

通过`Constructor`实例可以创建一个实例对象：`newInstance(Object... parameters)`； 通过设置`setAccessible(true)`来访问非`public`构造方法。

### 5.5 获取继承关系

当我们获取到某个`Class`对象时，实际上就获取到了一个类的类型：

```java
Class cls = String.class; // 获取到String的Class
```

还可以用实例的`getClass()`方法获取：

```java
String s = "";
Class cls = s.getClass(); // s是String，因此获取到String的Class
```

最后一种获取`Class`的方法是通过`Class.forName("")`，传入`Class`的完整类名获取：

```java
Class s = Class.forName("java.lang.String");
```

这三种方式获取的`Class`实例都是同一个实例，因为JVM对每个加载的`Class`只创建一个`Class`实例来表示它的类型。

#### 5.5.1 获取父类的Class

有了`Class`实例，我们还可以获取它的父类的`Class`：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        Class i = Integer.class;
        Class n = i.getSuperclass();
        System.out.println(n);
        Class o = n.getSuperclass();
        System.out.println(o);
        System.out.println(o.getSuperclass());
    }
}
```

运行上述代码，可以看到，`Integer`的父类类型是`Number`，`Number`的父类是`Object`，`Object`的父类是`null`。除`Object`外，其他任何非`interface`的`Class`都必定存在一个父类类型。

#### 5.5.2 获取interface

由于一个类可能实现一个或多个接口，通过`Class`我们就可以查询到实现的接口类型。例如，查询`Integer`实现的接口：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        Class s = Integer.class;
        Class[] is = s.getInterfaces();
        for (Class i : is) {
            System.out.println(i);
        }
    }
}
```

运行上述代码可知，`Integer`实现的接口有：

- java.lang.Comparable
- java.lang.constant.Constable
- java.lang.constant.ConstantDesc

要特别注意：`getInterfaces()`只返回当前类直接实现的接口类型，并不包括其父类实现的接口类型：

```java
import java.lang.reflect.Method;
public class Main {
    public static void main(String[] args) throws Exception {
        Class s = Integer.class.getSuperclass();
        Class[] is = s.getInterfaces();
        for (Class i : is) {
            System.out.println(i);
        }
    }
}
```

`Integer`的父类是`Number`，`Number`实现的接口是`java.io.Serializable`。

此外，对所有`interface`的`Class`调用`getSuperclass()`返回的是`null`，获取接口的父接口要用`getInterfaces()`：

```java
System.out.println(java.io.DataInputStream.class.getSuperclass()); // java.io.FilterInputStream，因为DataInputStream继承自FilterInputStream
System.out.println(java.io.Closeable.class.getSuperclass()); // null，对接口调用getSuperclass()总是返回null，获取接口的父接口要用getInterfaces()
```

如果一个类没有实现任何`interface`，那么`getInterfaces()`返回空数组。

#### 5.5.3 继承关系

当我们判断一个实例是否是某个类型时，正常情况下，使用`instanceof`操作符：

```java
Object n = Integer.valueOf(123);
boolean isDouble = n instanceof Double; // false
boolean isInteger = n instanceof Integer; // true
boolean isNumber = n instanceof Number; // true
boolean isSerializable = n instanceof java.io.Serializable; // true
```

如果是两个`Class`实例，要判断一个向上转型是否成立，可以调用`isAssignableFrom()`：

```java
// Integer i = ?
Integer.class.isAssignableFrom(Integer.class); // true，因为Integer可以赋值给Integer
// Number n = ?
Number.class.isAssignableFrom(Integer.class); // true，因为Integer可以赋值给Number
// Object o = ?
Object.class.isAssignableFrom(Integer.class); // true，因为Integer可以赋值给Object
// Integer i = ?
Integer.class.isAssignableFrom(Number.class); // false，因为Number不能赋值给Integer
```

#### 5.5.4 小结

通过`Class`对象可以获取继承关系：

- `Class getSuperclass()`：获取父类类型；
- `Class[] getInterfaces()`：获取当前类实现的所有接口。

通过`Class`对象的`isAssignableFrom()`方法可以判断一个向上转型是否可以实现。

### 5.6 动态代理

我们来比较Java的`class`和`interface`的区别：

- 可以实例化`class`（非`abstract`）；
- 不能实例化`interface`。

所有`interface`类型的变量总是通过向上转型并指向某个实例的：

```java
CharSequence cs = new StringBuilder();
```

有没有可能不编写实现类，直接在运行期创建某个`interface`的实例呢？

这是可能的，因为Java标准库提供了一种动态代理（Dynamic Proxy）的机制：可以在运行期动态创建某个`interface`的实例。

什么叫运行期动态创建？听起来好像很复杂。所谓动态代理，是和静态相对应的。我们来看静态代码怎么写：

定义接口：

```java
public interface Hello {
    void morning(String name);
}
```

编写实现类：

```java
public class HelloWorld implements Hello {
    public void morning(String name) {
        System.out.println("Good morning, " + name);
    }
}
```

创建实例，转型为接口并调用：

```java
Hello hello = new HelloWorld();
hello.morning("Bob");
```

这种方式就是我们通常编写代码的方式。

还有一种方式是动态代码，我们仍然先定义了接口`Hello`，但是我们并不去编写实现类，而是直接通过JDK提供的一个`Proxy.newProxyInstance()`创建了一个`Hello`接口对象。这种没有实现类但是在运行期动态创建了一个接口对象的方式，我们称为**动态代码**。JDK提供的动态创建接口对象的方式，就叫**动态代理**。

一个最简单的动态代理实现如下：

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
public class Main {
    public static void main(String[] args) {
        InvocationHandler handler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println(method);
                if (method.getName().equals("morning")) {
                    System.out.println("Good morning, " + args[0]);
                }
                return null;
            }
        };
        Hello hello = (Hello) Proxy.newProxyInstance(
            Hello.class.getClassLoader(), // 传入ClassLoader
            new Class[] { Hello.class }, // 传入要实现的接口
            handler); // 传入处理调用方法的InvocationHandler
        hello.morning("Bob");
    }
}

interface Hello {
    void morning(String name);
}
```

在运行期动态创建一个`interface`实例的方法如下：

1. 定义一个`InvocationHandler`实例，它负责实现接口的方法调用；
2. 通过`Proxy.newProxyInstance()`创建`interface`实例，它需要3个参数：
   1. 使用的`ClassLoader`，通常就是接口类的`ClassLoader`；
   2. 需要实现的接口数组，至少需要传入一个接口进去；
   3. 用来处理接口方法调用的`InvocationHandler`实例。
3. 将返回的`Object`强制转型为接口。

动态代理实际上是JDK在运行期动态创建class字节码并加载的过程，它并没有什么黑魔法，把上面的动态代理改写为静态实现类大概长这样：

```java
public class HelloDynamicProxy implements Hello {
    InvocationHandler handler;
    public HelloDynamicProxy(InvocationHandler handler) {
        this.handler = handler;
    }
    public void morning(String name) {
        handler.invoke(
           this,
           Hello.class.getMethod("morning"),
           new Object[] { name });
    }
}
```

其实就是JDK帮我们自动编写了一个上述类（不需要源码，可以直接生成字节码），并不存在可以直接实例化接口的黑魔法。

#### 5.6.1 小结

Java标准库提供了动态代理功能，允许在运行期动态创建一个接口的实例；

动态代理是通过`Proxy`创建代理对象，然后将接口方法“代理”给`InvocationHandler`完成的。

## 6 注解

### 6.1 使用注解

什么是注解（Annotation）？注解是放在Java源码的类、方法、字段、参数前的一种特殊“注释”：

```java
// this is a component:
@Resource("hello")
public class Hello {
    @Inject
    int n;

    @PostConstruct
    public void hello(@Param String name) {
        System.out.println(name);
    }

    @Override
    public String toString() {
        return "Hello";
    }
}
```

注释会被编译器直接忽略，注解则可以被编译器打包进入class文件，因此，注解是一种用作标注的“元数据”。

#### 6.1.1 注解的作用

从JVM的角度看，注解本身对代码逻辑没有任何影响，如何使用注解完全由工具决定。

Java的注解可以分为三类：

第一类是由编译器使用的注解，例如：

- `@Override`：让编译器检查该方法是否正确地实现了覆写；
- `@SuppressWarnings`：告诉编译器忽略此处代码产生的警告。

这类注解不会被编译进入`.class`文件，它们在编译后就被编译器扔掉了。

第二类是由工具处理`.class`文件使用的注解，比如有些工具会在加载class的时候，对class做动态修改，实现一些特殊的功能。这类注解会被编译进入`.class`文件，但加载结束后并不会存在于内存中。这类注解只被一些底层库使用，一般我们不必自己处理。

第三类是在程序运行期能够读取的注解，它们在加载后一直存在于JVM中，这也是最常用的注解。例如，一个配置了`@PostConstruct`的方法会在调用构造方法后自动被调用（这是Java代码读取该注解实现的功能，JVM并不会识别该注解）。

定义一个注解时，还可以定义配置参数。配置参数可以包括：

- 所有基本类型；
- String；
- 枚举类型；
- 基本类型、String以及枚举的数组。

因为配置参数必须是常量，所以，上述限制保证了注解在定义时就已经确定了每个参数的值。注解的配置参数可以有默认值，缺少某个配置参数时将使用默认值。此外，大部分注解会有一个名为`value`的配置参数，对此参数赋值，可以只写常量，相当于省略了value参数。如果只写注解，相当于全部使用默认值。

举个栗子，对以下代码：

```java
public class Hello {
    @Check(min=0, max=100, value=55)
    public int n;

    @Check(value=99)
    public int p;

    @Check(99) // @Check(value=99)
    public int x;

    @Check
    public int y;
}
```

`@Check`就是一个注解。第一个`@Check(min=0, max=100, value=55)`明确定义了三个参数，第二个`@Check(value=99)`只定义了一个`value`参数，它实际上和`@Check(99)`是完全一样的。最后一个`@Check`表示所有参数都使用默认值。

#### 6.1.2 小结

注解（Annotation）是Java语言用于工具处理的标注：

注解可以配置参数，没有指定配置的参数使用默认值；

如果参数名称是`value`，且只有一个参数，那么可以省略参数名称。

### 6.2 定义注解

Java语言使用`@interface`语法来定义注解（`Annotation`），它的格式如下：

```java
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

注解的参数类似无参数方法，可以用`default`设定一个默认值（强烈推荐）。最常用的参数应当命名为`value`。

#### 6.2.1 元注解

有一些注解可以修饰其他注解，这些注解就称为元注解（meta annotation）。Java标准库已经定义了一些元注解，我们只需要使用元注解，通常不需要自己去编写元注解。

**@Target**

最常用的元注解是`@Target`。使用`@Target`可以定义`Annotation`能够被应用于源码的哪些位置：

- 类或接口：`ElementType.TYPE`；
- 字段：`ElementType.FIELD`；
- 方法：`ElementType.METHOD`；
- 构造方法：`ElementType.CONSTRUCTOR`；
- 方法参数：`ElementType.PARAMETER`。

例如，定义注解`@Report`可用在方法上，我们必须添加一个`@Target(ElementType.METHOD)`：

```java
@Target(ElementType.METHOD)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

定义注解`@Report`可用在方法或字段上，可以把`@Target`注解参数变为数组`{ ElementType.METHOD, ElementType.FIELD }`：

```java
@Target({
    ElementType.METHOD,
    ElementType.FIELD
})
public @interface Report {
    ...
}
```

实际上`@Target`定义的`value`是`ElementType[]`数组，只有一个元素时，可以省略数组的写法。

**@Retention**

另一个重要的元注解`@Retention`定义了`Annotation`的生命周期：

- 仅编译期：`RetentionPolicy.SOURCE`；
- 仅class文件：`RetentionPolicy.CLASS`；
- 运行期：`RetentionPolicy.RUNTIME`。

如果`@Retention`不存在，则该`Annotation`默认为`CLASS`。因为通常我们自定义的`Annotation`都是`RUNTIME`，所以，务必要加上`@Retention(RetentionPolicy.RUNTIME)`这个元注解：

```java
@Retention(RetentionPolicy.RUNTIME)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

**@Repeatable**

使用`@Repeatable`这个元注解可以定义`Annotation`是否可重复。这个注解应用不是特别广泛。

```java
@Repeatable
@Target(ElementType.TYPE)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

经过`@Repeatable`修饰后，在某个类型声明处，就可以添加多个`@Report`注解：

```java
@Report(type=1, level="debug")
@Report(type=2, level="warning")
public class Hello {
}
```

**@Inherited**

使用`@Inherited`定义子类是否可继承父类定义的`Annotation`。`@Inherited`仅针对`@Target(ElementType.TYPE)`类型的`annotation`有效，并且仅针对`class`的继承，对`interface`的继承无效：

```java
@Inherited
@Target(ElementType.TYPE)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

在使用的时候，如果一个类用到了`@Report`：

```java
@Report(type=1)
public class Person {
}
```

则它的子类默认也定义了该注解：

```java
public class Student extends Person {
}
```

#### 6.2.2 如何定义Annotation

我们总结一下定义`Annotation`的步骤：

第一步，用`@interface`定义注解：

```java
public @interface Report {
}
```

第二步，添加参数、默认值：

```java
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

把最常用的参数定义为`value()`，推荐所有参数都尽量设置默认值。

第三步，用元注解配置注解：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

其中，必须设置`@Target`和`@Retention`，`@Retention`一般设置为`RUNTIME`，因为我们自定义的注解通常要求在运行期读取。一般情况下，不必写`@Inherited`和`@Repeatable`。

#### 6.2.3 小结

Java使用`@interface`定义注解：

可定义多个参数和默认值，核心参数使用`value`名称；

必须设置`@Target`来指定`Annotation`可以应用的范围；

应当设置`@Retention(RetentionPolicy.RUNTIME)`便于运行期读取该`Annotation`。

### 6.3 处理注解

Java的注解本身对代码逻辑没有任何影响。根据`@Retention`的配置：

- `SOURCE`类型的注解在编译期就被丢掉了；
- `CLASS`类型的注解仅保存在class文件中，它们不会被加载进JVM；
- `RUNTIME`类型的注解会被加载进JVM，并且在运行期可以被程序读取。

如何使用注解完全由工具决定。`SOURCE`类型的注解主要由编译器使用，因此我们一般只使用，不编写。`CLASS`类型的注解主要由底层工具库使用，涉及到class的加载，一般我们很少用到。**只有`RUNTIME`类型的注解不但要使用，还经常需要编写。**

因此，我们只讨论如何读取`RUNTIME`类型的注解。

因为注解定义后也是一种`class`，所有的注解都继承自`java.lang.annotation.Annotation`，因此，读取注解，需要使用反射API。

Java提供的使用反射API读取`Annotation`的方法包括：

判断某个注解是否存在于`Class`、`Field`、`Method`或`Constructor`：

- `Class.isAnnotationPresent(Class)`
- `Field.isAnnotationPresent(Class)`
- `Method.isAnnotationPresent(Class)`
- `Constructor.isAnnotationPresent(Class)`

例如：

```java
// 判断@Report是否存在于Person类:
Person.class.isAnnotationPresent(Report.class);
```

使用反射API读取Annotation：

- `Class.getAnnotation(Class)`
- `Field.getAnnotation(Class)`
- `Method.getAnnotation(Class)`
- `Constructor.getAnnotation(Class)`

例如：

```java
// 获取Person定义的@Report注解:
Report report = Person.class.getAnnotation(Report.class);
int type = report.type();
String level = report.level();
```

使用反射API读取`Annotation`有两种方法。方法一是先判断`Annotation`是否存在，如果存在，就直接读取：

```java
Class cls = Person.class;
if (cls.isAnnotationPresent(Report.class)) {
    Report report = cls.getAnnotation(Report.class);
    ...
}
```

第二种方法是直接读取`Annotation`，如果`Annotation`不存在，将返回`null`：

```java
Class cls = Person.class;
Report report = cls.getAnnotation(Report.class);
if (report != null) {
   ...
}
```

读取方法、字段和构造方法的`Annotation`和Class类似。但要读取方法参数的`Annotation`就比较麻烦一点，因为方法参数本身可以看成一个数组，而每个参数又可以定义多个注解，所以，一次获取方法参数的所有注解就必须用一个二维数组来表示。例如，对于以下方法定义的注解：

```java
public void hello(@NotNull @Range(max=5) String name, @NotNull String prefix) {
}
```

要读取方法参数的注解，我们先用反射获取`Method`实例，然后读取方法参数的所有注解：

```java
// 获取Method实例:
Method m = ...
// 获取所有参数的Annotation:
Annotation[][] annos = m.getParameterAnnotations();
// 第一个参数（索引为0）的所有Annotation:
Annotation[] annosOfName = annos[0];
for (Annotation anno : annosOfName) {
    if (anno instanceof Range) { // @Range注解
        Range r = (Range) anno;
    }
    if (anno instanceof NotNull) { // @NotNull注解
        NotNull n = (NotNull) anno;
    }
}
```

#### 6.3.1 使用注解

注解如何使用，完全由程序自己决定。例如，JUnit是一个测试框架，它会自动运行所有标记为`@Test`的方法。

我们来看一个`@Range`注解，我们希望用它来定义一个`String`字段的规则：字段长度满足`@Range`的参数定义：

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Range {
    int min() default 0;
    int max() default 255;
}
```

在某个JavaBean中，我们可以使用该注解：

```java
public class Person {
    @Range(min=1, max=20)
    public String name;

    @Range(max=10)
    public String city;
}
```

但是，定义了注解，本身对程序逻辑没有任何影响。我们必须自己编写代码来使用注解。这里，我们编写一个`Person`实例的检查方法，它可以检查`Person`实例的`String`字段长度是否满足`@Range`的定义：

```java
void check(Person person) throws IllegalArgumentException, ReflectiveOperationException {
    // 遍历所有Field:
    for (Field field : person.getClass().getFields()) {
        // 获取Field定义的@Range:
        Range range = field.getAnnotation(Range.class);
        // 如果@Range存在:
        if (range != null) {
            // 获取Field的值:
            Object value = field.get(person);
            // 如果值是String:
            if (value instanceof String) {
                String s = (String) value;
                // 判断值是否满足@Range的min/max:
                if (s.length() < range.min() || s.length() > range.max()) {
                    throw new IllegalArgumentException("Invalid field: " + field.getName());
                }
            }
        }
    }
}
```

这样一来，我们通过`@Range`注解，配合`check()`方法，就可以完成`Person`实例的检查。注意检查逻辑完全是我们自己编写的，JVM不会自动给注解添加任何额外的逻辑。

#### 6.3.2 小结

可以在运行期通过反射读取`RUNTIME`类型的注解，注意千万不要漏写`@Retention(RetentionPolicy.RUNTIME)`，否则运行期无法读取到该注解。

可以通过程序处理注解来实现相应的功能：

- 对JavaBean的属性值按规则进行检查；
- JUnit会自动运行`@Test`标记的测试方法。

## 7 泛型

 泛型是一种“代码模板”，可以用一套代码套用各种类型。

### 7.1 什么是泛型

在讲解什么是泛型之前，我们先观察Java标准库提供的`ArrayList`，它可以看作“可变长度”的数组，因为用起来比数组更方便。

实际上`ArrayList`内部就是一个`Object[]`数组，配合存储一个当前分配的长度，就可以充当“可变数组”：

```java
public class ArrayList {
    private Object[] array;
    private int size;
    public void add(Object e) {...}
    public void remove(int index) {...}
    public Object get(int index) {...}
}
```

如果用上述`ArrayList`存储`String`类型，会有这么几个缺点：

- 需要强制转型；
- 不方便，易出错。

例如，代码必须这么写：

```java
ArrayList list = new ArrayList();
list.add("Hello");
// 获取到Object，必须强制转型为String:
String first = (String) list.get(0);
```

很容易出现ClassCastException，因为容易“误转型”：

```java
list.add(new Integer(123));
// ERROR: ClassCastException:
String second = (String) list.get(1);
```

要解决上述问题，我们可以为`String`单独编写一种`ArrayList`：

```java
public class StringArrayList {
    private String[] array;
    private int size;
    public void add(String e) {...}
    public void remove(int index) {...}
    public String get(int index) {...}
}
```

这样一来，存入的必须是`String`，取出的也一定是`String`，不需要强制转型，因为编译器会强制检查放入的类型：

```java
StringArrayList list = new StringArrayList();
list.add("Hello");
String first = list.get(0);
// 编译错误: 不允许放入非String类型:
list.add(new Integer(123));
```

问题暂时解决。

然而，新的问题是，如果要存储`Integer`，还需要为`Integer`单独编写一种`ArrayList`：

```java
public class IntegerArrayList {
    private Integer[] array;
    private int size;
    public void add(Integer e) {...}
    public void remove(int index) {...}
    public Integer get(int index) {...}
}
```

实际上，还需要为其他所有class单独编写一种`ArrayList`：

- LongArrayList
- DoubleArrayList
- PersonArrayList
- ...

这是不可能的，JDK的class就有上千个，而且它还不知道其他人编写的class。

为了解决新的问题，我们必须把`ArrayList`变成一种模板：`ArrayList`，代码如下：

```java
public class ArrayList<T> {
    private T[] array;
    private int size;
    public void add(T e) {...}
    public void remove(int index) {...}
    public T get(int index) {...}
}
```

`T`可以是任何class。这样一来，我们就实现了：编写一次模版，可以创建任意类型的`ArrayList`：

```java
// 创建可以存储String的ArrayList:
ArrayList<String> strList = new ArrayList<String>();
// 创建可以存储Float的ArrayList:
ArrayList<Float> floatList = new ArrayList<Float>();
// 创建可以存储Person的ArrayList:
ArrayList<Person> personList = new ArrayList<Person>();
```

因此，泛型就是定义一种模板，例如`ArrayList`，然后在代码中为用到的类创建对应的`ArrayList<类型>`：

```java
ArrayList<String> strList = new ArrayList<String>();
```

由编译器针对类型作检查：

```java
strList.add("hello"); // OK
String s = strList.get(0); // OK
strList.add(new Integer(123)); // compile error!
Integer n = strList.get(0); // compile error!
```

这样一来，既实现了**编写一次，万能匹配，又通过编译器保证了类型安全：这就是泛型**。

#### 7.1.1 向上转型

在Java标准库中的`ArrayList`实现了`List`接口，它可以向上转型为`List`：

```java
public class ArrayList<T> implements List<T> {
    ...
}

List<String> list = new ArrayList<String>();
```

即类型`ArrayList`可以向上转型为`List`。

要*特别注意*：不能把`ArrayList`向上转型为`ArrayList`或`List`。

这是为什么呢？假设`ArrayList`可以向上转型为`ArrayList`，观察一下代码：

```java
// 创建ArrayList<Integer>类型：
ArrayList<Integer> integerList = new ArrayList<Integer>();
// 添加一个Integer：
integerList.add(new Integer(123));
// “向上转型”为ArrayList<Number>：
ArrayList<Number> numberList = integerList;
// 添加一个Float，因为Float也是Number：
numberList.add(new Float(12.34));
// 从ArrayList<Integer>获取索引为1的元素（即添加的Float）：
Integer n = integerList.get(1); // ClassCastException!
```

我们把一个`ArrayList`转型为`ArrayList`类型后，这个`ArrayList`就可以接受`Float`类型，因为`Float`是`Number`的子类。但是，`ArrayList`实际上和`ArrayList`是同一个对象，也就是`ArrayList`类型，它不可能接受`Float`类型， 所以在获取`Integer`的时候将产生`ClassCastException`。

实际上，编译器为了避免这种错误，根本就不允许把`ArrayList`转型为`ArrayList`。`ArrayList<Integer>`和`ArrayList<Number>`两者完全没有继承关系。

#### 7.1.2 小结

泛型就是编写模板代码来适应任意类型；泛型的好处是使用时不必对类型进行强制转换，它通过编译器对类型进行检查；注意泛型的继承关系：可以把`ArrayList`向上转型为`List`（`T`不能变！），但不能把`ArrayList`向上转型为`ArrayList`（`T`不能变成父类）。

### 7.2 使用泛型

使用`ArrayList`时，如果不定义泛型类型时，泛型类型实际上就是`Object`：

```java
// 编译器警告:
List list = new ArrayList();
list.add("Hello");
list.add("World");
String first = (String) list.get(0);
String second = (String) list.get(1);
```

此时，只能把`<T>`当作`Object`使用，没有发挥泛型的优势。

当我们定义泛型类型`String`后，`List<T>`的泛型接口变为强类型`List<String>`：

```java
// 无编译器警告:
List<String> list = new ArrayList<String>();
list.add("Hello");
list.add("World");
// 无强制转型:
String first = list.get(0);
String second = list.get(1);
```

当我们定义泛型类型`<Number>`，`List<T>`的泛型接口变为强类型`List<Number>`：

```java
List<Number> list = new ArrayList<Number>();
list.add(new Integer(123));
list.add(new Double(12.34));
Number first = list.get(0);
Number second = list.get(1);
```

编译器如果能自动推断出泛型类型，就可以省略后面的泛型类型。例如，对于下面的代码：

```java
List<Number> list = new ArrayList<Number>();
```

编译器看到泛型类型`List`就可以自动推断出后面的`ArrayList`的泛型类型必须是`ArrayList`，因此，可以把代码简写为：

```java
// 可以省略后面的Number，编译器可以自动推断泛型类型：
List<Number> list = new ArrayList<>();
```

#### 7.2.1 泛型接口

除了`ArrayList`使用了泛型，还可以在接口中使用泛型。例如，`Arrays.sort(Object[])`可以对任意数组进行排序，但待排序的元素必须实现`Comparable`这个泛型接口：

```java
public interface Comparable<T> {
    /**
     * 返回-1: 当前实例比参数o小
     * 返回0: 当前实例与参数o相等
     * 返回1: 当前实例比参数o大
     */
    int compareTo(T o);
}
```

可以直接对`String`数组进行排序：

```java
// sort import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        String[] ss = new String[] { "Orange", "Apple", "Pear" };
        Arrays.sort(ss);
        System.out.println(Arrays.toString(ss));
```

这是因为`String`本身已经实现了`Comparable`接口。如果换成我们自定义的`Person`类型试试： 

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
            Person[] ps = new Person[] {
            new Person("Bob", 61),
            new Person("Alice", 88),
            new Person("Lily", 75),
        };
        Arrays.sort(ps);
        System.out.println(Arrays.toString(ps));
            }
}

class Person {
    String name;
    int score;
    Person(String name, int score) {
        this.name = name;
        this.score = score;
    }
    public String toString() {
        return this.name + "," + this.score;
    }
}
```

```java
Exception in thread "main" java.lang.ClassCastException: class Person cannot be cast to class java.lang.Comparable (Person is in unnamed module of loader com.sun.tools.javac.launcher.Main$MemoryClassLoader @18a70f16; java.lang.Comparable is in module java.base of loader 'bootstrap')
    at java.base/java.util.ComparableTimSort.countRunAndMakeAscending(ComparableTimSort.java:320)
    at java.base/java.util.ComparableTimSort.sort(ComparableTimSort.java:188)
    at java.base/java.util.Arrays.sort(Arrays.java:1250)
    at Main.main(Main.java:11)
```

 运行程序，我们会得到`ClassCastException`，即无法将`Person`转型为`Comparable`。我们修改代码，让`Person`实现`Comparable`接口：

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Person[] ps = new Person[]{
                new Person("Bob", 12),
                new Person("Alice", 88),
                new Person("Lily", 99),
        };
        Arrays.sort(ps);
        System.out.println(Arrays.toString(ps));
    }
}
class Person implements Comparable<Person>{
    private String name;
    private int score;

    Person(String name, int score){
        this.name = name;
        this.score = score;
    }

    public String toString(){
            return this.name + "," +this.score;
    }

    public int compareTo(Person other){
        return this.name.compareTo(other.name);
    }
}
```

运行上述代码，可以正确实现按`name`进行排序。

也可以修改比较逻辑，例如，按`score`从高到低排序。请自行修改测试。

#### 7.2.2 小结

使用泛型时，把泛型参数`<T>`替换为需要的class类型，例如：`ArrayList`，`ArrayList`等；

可以省略编译器能自动推断出的类型，例如：`List list = new ArrayList<>();`；

不指定泛型参数类型时，编译器会给出警告，且只能将视为`Object`类型；

可以在接口中定义泛型类型，实现此接口的类必须实现正确的泛型类型。

### 7.3 编写泛型

编写泛型类比普通类要复杂。通常来说，泛型类一般用在集合类中，例如`ArrayList`，我们很少需要编写泛型类。如果我们确实需要编写一个泛型类，那么，应该如何编写它？可以按照以下步骤来编写一个泛型类。首先，按照某种类型，例如：`String`，来编写类：

```java
public class Pair {
    private String first;
    private String last;
    public Pair(String first, String last) {
        this.first = first;
        this.last = last;
    }
    public String getFirst() {
        return first;
    }
    public String getLast() {
        return last;
    }
}
```

然后，标记所有的特定类型，这里是`String`：

```java
public class Pair {
    private String first;
    private String last;
    public Pair(String first, String last) {
        this.first = first;
        this.last = last;
    }
    public String getFirst() {
        return first;
    }
    public String getLast() {
        return last;
    }
}
```

最后，把特定类型`String`替换为`T`，并申明``：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() {
        return first;
    }
    public T getLast() {
        return last;
    }
}
```

熟练后即可直接从`T`开始编写。

#### 7.3.1 静态方法

编写泛型类时，要特别注意，泛型类型`<T>`不能用于静态方法。例如：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { ... }
    public T getLast() { ... }

    // 对静态方法使用<T>:
    public static Pair<T> create(T first, T last) {
        return new Pair<T>(first, last);
    }
}
```

上述代码会导致编译错误，我们无法在静态方法`create()`的方法参数和返回类型上使用泛型类型`T`。

有些同学在网上搜索发现，可以在`static`修饰符后面加一个`<T>`，编译就能通过：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { ... }
    public T getLast() { ... }

    // 可以编译通过:
    public static <T> Pair<T> create(T first, T last) {
        return new Pair<T>(first, last);
    }
}
```

但实际上，这个`<T>`和`Pair<T>`类型的`<T>`已经没有任何关系了。

对于静态方法，我们可以单独改写为“泛型”方法，只需要使用另一个类型即可。对于上面的`create()`静态方法，我们应该把它改为另一种泛型类型，例如，`<K>`：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { ... }
    public T getLast() { ... }

    // 静态泛型方法应该使用其他类型区分:
    public static <K> Pair<K> create(K first, K last) {
        return new Pair<K>(first, last);
    }
}
```

这样才能清楚地将静态方法的泛型类型和实例类型的泛型类型区分开。

#### 7.3.2 多个泛型类型

泛型还可以定义多种类型。例如，我们希望`Pair`不总是存储两个类型一样的对象，就可以使用类型`<T, K>`：

```java
public class Pair<T, K> {
    private T first;
    private K last;
    public Pair(T first, K last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { ... }
    public K getLast() { ... }
}
```

使用的时候，需要指出两种类型：

```java
Pair<String, Integer> p = new Pair<>("test", 123);
```

Java标准库的`Map`就是使用两种泛型类型的例子。它对Key使用一种类型，对Value使用另一种类型。

#### 7.3.3 小结

编写泛型时，需要定义泛型类型`<T>`；

静态方法不能引用泛型类型`<T>`，必须定义其他类型（例如`<K>`）来实现静态泛型方法；

泛型可以同时定义多种类型，例如`Map<T, K>`。

### 7.4 擦拭法

泛型是一种类似”模板代码“的技术，不同语言的泛型实现方式不一定相同。

Java语言的泛型实现方式是擦拭法（Type Erasure）。

所谓擦拭法是指，**虚拟机对泛型其实一无所知，所有的工作都是编译器做的**。

例如，我们编写了一个泛型类`Pair`，这是编译器看到的代码：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() {
        return first;
    }
    public T getLast() {
        return last;
    }
}
```

而虚拟机根本不知道泛型。这是虚拟机执行的代码：

```java
public class Pair {
    private Object first;
    private Object last;
    public Pair(Object first, Object last) {
        this.first = first;
        this.last = last;
    }
    public Object getFirst() {
        return first;
    }
    public Object getLast() {
        return last;
    }
}
```

因此，Java使用擦拭法实现泛型，导致了：

- 编译器把类型`<T>`视为`Object`；
- 编译器根据`<T>`实现安全的强制转型。

使用泛型的时候，我们编写的代码也是编译器看到的代码：

```java
Pair<String> p = new Pair<>("Hello", "world");
String first = p.getFirst();
String last = p.getLast();
```

而虚拟机执行的代码并没有泛型：

```java
Pair p = new Pair("Hello", "world");
String first = (String) p.getFirst();
String last = (String) p.getLast();
```

所以，Java的泛型是由编译器在编译时实行的，编译器内部永远把所有类型`T`视为`Object`处理，但是，在需要转型的时候，编译器会根据`T`的类型自动为我们实行安全地强制转型。

了解了Java泛型的实现方式——擦拭法，我们就知道了Java泛型的局限：

**局限一**：`<T>`不能是基本类型，例如`int`，因为实际类型是`Object`，`Object`类型无法持有基本类型：

```java
Pair<int> p = new Pair<>(1, 2); // compile error!
```

**局限二**：无法取得带泛型的`Class`。观察以下代码：

```java
public class Main {
    public static void main(String[] args) {
        Pair<String> p1 = new Pair<>("Hello", "world");
        Pair<Integer> p2 = new Pair<>(123, 456);
        Class c1 = p1.getClass();
        Class c2 = p2.getClass();
        System.out.println(c1==c2); // true
        System.out.println(c1==Pair.class); // true
    }
}

class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() {
        return first;
    }
    public T getLast() {
        return last;
    }
}
```

因为`T`是`Object`，我们对`Pair`和`Pair`类型获取`Class`时，获取到的是同一个`Class`，也就是`Pair`类的`Class`。换句话说，所有泛型实例，无论`T`的类型是什么，`getClass()`返回同一个`Class`实例，因为编译后它们全部都是`Pair`。

**局限三**：无法判断带泛型的`Class`：

```java
Pair<Integer> p = new Pair<>(123, 456);
// Compile error:
if (p instanceof Pair<String>.class) {
}
```

原因和前面一样，并不存在`Pair.class`，而是只有唯一的`Pair.class`。

**局限四**：不能实例化`T`类型：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair() {
        // Compile error:
        first = new T();
        last = new T();
    }
}
```

上述代码无法通过编译，因为构造方法的两行语句：

```java
first = new T();
last = new T();
```

擦拭后实际上变成了：

```java
first = new Object();
last = new Object();
```

这样一来，创建`new Pair()`和创建`new Pair()`就全部成了`Object`，显然编译器要阻止这种类型不对的代码。

要实例化`T`类型，我们必须借助额外的`Class`参数：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(Class<T> clazz) {
        first = clazz.newInstance();
        last = clazz.newInstance();
    }
}
```

上述代码借助`Class`参数并通过反射来实例化`T`类型，使用的时候，也必须传入`Class`。例如：

```java
Pair<String> pair = new Pair<>(String.class);
```

因为传入了`Class`的实例，所以我们借助`String.class`就可以实例化`String`类型。

#### 7.4.1 不恰当的覆写方法

有些时候，一个看似正确定义的方法会无法通过编译。例如：

```java
public class Pair<T> {
    public boolean equals(T t) {
        return this == t;
    }
}
```

这是因为，定义的`equals(T t)`方法实际上会被擦拭成`equals(Object t)`，而这个方法是继承自`Object`的，编译器会阻止一个实际上会变成覆写的泛型方法定义。

换个方法名，避开与`Object.equals(Object)`的冲突就可以成功编译：

```java
public class Pair<T> {
    public boolean same(T t) {
        return this == t;
    }
}
```

#### 7.4.2 泛型继承

一个类可以继承自一个泛型类。例如：父类的类型是`Pair`，子类的类型是`IntPair`，可以这么继承：

```java
public class IntPair extends Pair<Integer> {}
```

使用的时候，因为子类`IntPair`并没有泛型类型，所以，正常使用即可：

```java
IntPair ip = new IntPair(1, 2);
```

前面讲了，我们无法获取`Pair`的`T`类型，即给定一个变量`Pair p`，无法从`p`中获取到`Integer`类型。

但是，在父类是泛型类型的情况下，编译器就必须把类型`T`（对`IntPair`来说，也就是`Integer`类型）保存到子类的class文件中，不然编译器就不知道`IntPair`只能存取`Integer`这种类型。

在继承了泛型类型的情况下，子类可以获取父类的泛型类型。例如：`IntPair`可以获取到父类的泛型类型`Integer`。获取父类的泛型类型代码比较复杂：

```java
import java.lang.reflect.ParameterizedType;
import java.lang.reflect.Type;
public class Main {
public static void main(String[] args) {
        Class<IntPair> clazz = IntPair.class;
        Type t = clazz.getGenericSuperclass();
        if (t instanceof ParameterizedType) {
            ParameterizedType pt = (ParameterizedType) t;
            Type[] types = pt.getActualTypeArguments(); // 可能有多个泛型类型
            Type firstType = types[0]; // 取第一个泛型类型
            Class<?> typeClass = (Class<?>) firstType;
            System.out.println(typeClass); // Integer
        }
    }
}

class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() {
        return first;
    }
    public T getLast() {
        return last;
    }
}

class IntPair extends Pair<Integer> {
    public IntPair(Integer first, Integer last) {
        super(first, last);
    }
}
```

因为Java引入了泛型，所以，只用`Class`来标识类型已经不够了。实际上，Java的类型系统结构如下：

```ascii
                      ┌────┐
                      │Type│
                      └────┘
                         ▲
                         │
   ┌────────────┬────────┴─────────┬───────────────┐
   │            │                  │               │
┌─────┐┌─────────────────┐┌────────────────┐┌────────────┐
│Class││ParameterizedType││GenericArrayType││WildcardType│
└─────┘└─────────────────┘└────────────────┘└────────────┘
```

#### 7.4.3 小结

Java的泛型是采用擦拭法实现的；

擦拭法决定了泛型`<T>`：

- 不能是基本类型，例如：`int`；
- 不能获取带泛型类型的`Class`，例如：`Pair.class`；
- 不能判断带泛型类型的类型，例如：`x instanceof Pair`；
- 不能实例化`T`类型，例如：`new T()`。

泛型方法要防止重复定义方法，例如：`public boolean equals(T obj)`；

子类可以获取父类的泛型类型`<T>`。

### 7.5 extends通配符

我们前面已经讲到了泛型的继承关系：`Pair`不是`Pair`的子类。

假设我们定义了`Pair`：

```java
public class Pair<T> { ... }
```

然后，我们又针对`Pair`类型写了一个静态方法，它接收的参数类型是`Pair`：

```java
public class PairHelper {
    static int add(Pair<Number> p) {
        Number first = p.getFirst();
        Number last = p.getLast();
        return first.intValue() + last.intValue();
    }
}
```

上述代码是可以正常编译的。使用的时候，我们传入：

```java
int sum = PairHelper.add(new Pair<Number>(1, 2));
```

注意：传入的类型是`Pair`，实际参数类型是`(Integer, Integer)`。既然实际参数是`Integer`类型，试试传入`Pair`：

```java
public class Main {
public static void main(String[] args) {
        Pair<Integer> p = new Pair<>(123, 456);
        int n = add(p);
        System.out.println(n);
    }

    static int add(Pair<Number> p) {
        Number first = p.getFirst();
        Number last = p.getLast();
        return first.intValue() + last.intValue();
    }
}

class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() {
        return first;
    }
    public T getLast() {
        return last;
    }
}
```

直接运行，会得到一个编译错误：

```java
incompatible types: Pair<Integer> cannot be converted to Pair<Number>
```

原因很明显，因为`Pair`不是`Pair`的子类，因此，`add(Pair)`不接受参数类型`Pair`。

但是从`add()`方法的代码可知，传入`Pair`是完全符合内部代码的类型规范，因为语句：

```java
Number first = p.getFirst();
Number last = p.getLast();
```

实际类型是`Integer`，引用类型是`Number`，没有问题。问题在于方法参数类型定死了只能传入`Pair`。

有没有办法使得方法参数接受`Pair`？办法是有的，这就是使用`Pair`使得方法接收所有泛型类型为`Number`或`Number`子类的`Pair`类型。我们把代码改写如下：

```java
public class Main {
    public static void main(String[] args) {
        Pair<Integer> p = new Pair<>(123, 456);
        int n = add(p);
        System.out.println(n);
    }

    static int add(Pair<? extends Number> p){
        Number first = p.getFirst();
        Number last = p.getLast();
        return first.intValue() + last.intValue();
    }
}

class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() {
        return first;
    }
    public T getLast() {
        return last;
    }
}
```

这样一来，给方法传入`Pair<Integer>`类型时，它符合参数`Pair<? extends Number>`类型。这种使用` <? extends Number> `的泛型定义称之为**上界通配符**（Upper Bounds Wildcards），即把泛型类型`T`的上界限定在`Number`了。

除了可以传入`Pair<Integer>`类型，我们还可以传入`Pair<Double>`类型，`Pair<BigDecimal>`类型等等，因为`Double`和`BigDecimal`都是`Number`的子类。

如果我们考察对`Pair<? extends Number>`类型调用`getFirst()`方法，实际的方法签名变成了：

```java
<? extends Number> getFirst();
```

即返回值是`Number`或`Number`的子类，因此，可以安全赋值给`Number`类型的变量：

```java
Number x = p.getFirst();
```

然后，我们不可预测实际类型就是`Integer`，例如，下面的代码是无法通过编译的：

```java
Integer x = p.getFirst();
```

这是因为实际的返回类型可能是`Integer`，也可能是`Double`或者其他类型，编译器只能确定类型一定是`Number`的子类（包括`Number`类型本身），但具体类型无法确定。

我们再来考察一下`Pair`的`set`方法：

```java
public class Main {
        public static void main(String[] args) {
        Pair<Integer> p = new Pair<>(123, 456);
        int n = add(p);
        System.out.println(n);
    }

    static int add(Pair<? extends Number> p) {
        Number first = p.getFirst();
        Number last = p.getLast();
        p.setFirst(new Integer(first.intValue() + 100));
        p.setLast(new Integer(last.intValue() + 100));
        return p.getFirst().intValue() + p.getFirst().intValue();
    }
class Pair<T> {
    private T first;
    private T last;

    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }

    public T getFirst() {
        return first;
    }
    public T getLast() {
        return last;
    }
    public void setFirst(T first) {
        this.first = first;
    }
    public void setLast(T last) {
        this.last = last;
    }
}
```

 不出意外，我们会得到一个编译错误：

```java
incompatible types: Integer cannot be converted to CAP#1
where CAP#1 is a fresh type-variable:
    CAP#1 extends Number from capture of ? extends Number
```

编译错误发生在`p.setFirst()`传入的参数是`Integer`类型。有些童鞋会问了，既然`p`的定义是`Pair<? extends Number>`，那么`setFirst(? extends Number)`为什么不能传入`Integer`？

原因还在于擦拭法。如果我们传入的`p`是`Pair<Double>`，显然它满足参数定义`Pair<? extends Number>`，然而，`Pair<Double>`的`setFirst()`显然无法接受`Integer`类型。

这就是` <? extends Number> `通配符的一个重要限制：**方法参数签名`setFirst(? extends Number)`无法传递任何`Number`类型给`setFirst(? extends Number)`**。

这里唯一的例外是可以给方法参数传入`null`：

```java
p.setFirst(null); // ok, 但是后面会抛出NullPointerException
p.getFirst().intValue(); // NullPointerException
```

#### 7.5.1 extends通配符的作用

如果我们考察Java标准库的`java.util.List`接口，它实现的是一个类似“可变数组”的列表，主要功能包括：

```java
public interface List<T> {
    int size(); // 获取个数
    T get(int index); // 根据索引获取指定元素
    void add(T t); // 添加一个新元素
    void remove(T t); // 删除一个已有元素
}
```

现在，让我们定义一个方法来处理列表的每个元素：

```java
int sumOfList(List<? extends Integer> list) {
    int sum = 0;
    for (int i=0; i<list.size(); i++) {
        Integer n = list.get(i);
        sum = sum + n;
    }
    return sum;
}
```

为什么我们定义的方法参数类型是`List<? extends Integer>`而不是`List<Integer>`？从方法内部代码看，传入`List<? extends Integer>`或者`List<Integer>`是完全一样的，但是，注意到`List<? extends Integer>`的限制：

- 允许调用`get()`方法获取`Integer`的引用；
- 不允许调用`set(? extends Integer)`方法并传入任何`Integer`的引用（`null`除外）。

因此，方法参数类型`List<? extends Integer>`表明了该方法内部只会读取`List`的元素，不会修改`List`的元素（因为无法调用`add(? extends Integer)`、`remove(? extends Integer)`这些方法。换句话说，这是一个对参数`List<? extends Integer>`进行只读的方法（恶意调用`set(null)`除外）。

#### 7.5.2 使用extends限定T类型

在定义泛型类型`Pair<T>`的时候，也可以使用`extends`通配符来限定`T`的类型：

```java
public class Pair<T extends Number> { ... }
```

现在，我们只能定义：

```java
Pair<Number> p1 = null;
Pair<Integer> p2 = new Pair<>(1, 2);
Pair<Double> p3 = null;
```

因为`Number`、`Integer`和`Double`都符合` <T extends Number> `。

非`Number`类型将无法通过编译：

```java
Pair<String> p1 = null; // compile error!
Pair<Object> p2 = null; // compile error!
```

因为`String`、`Object`都不符合` <T extends Number> `，因为它们不是`Number`类型或`Number`的子类。

#### 7.5.3 小结

使用类似` <? extends Number> `通配符作为方法参数时表示：

- 方法内部可以调用获取`Number`引用的方法，例如：`Number n = obj.getFirst();`；
- 方法内部无法调用传入`Number`引用的方法（`null`除外），例如：`obj.setFirst(Number n);`。

即一句话总结：**使用`extends`通配符表示可以读，不能写**。

使用类似` <T extends Number> `定义泛型类时表示：

- 泛型类型限定为`Number`以及`Number`的子类。

### 7.6 super通配符

我们前面已经讲到了泛型的继承关系：`Pair<Integer>`不是`Pair<Number>`的子类。

考察下面的`set`方法：

```java
void set(Pair<Integer> p, Integer first, Integer last) {
    p.setFirst(first);
    p.setLast(last);
}
```

传入`Pair<Integer>`是允许的，但是传入`Pair<Number>`是不允许的。

和`extends`通配符相反，这次，我们希望接受`Pair<Integer>`类型，以及`Pair<Number>`、`Pair<Object>`，因为`Number`和`Object`是`Integer`的父类，`setFirst(Number)`和`setFirst(Object)`实际上允许接受`Integer`类型。

我们使用`super`通配符来改写这个方法：

```java
void set(Pair<? super Integer> p, Integer first, Integer last) {
    p.setFirst(first);
    p.setLast(last);
}
```

注意到`Pair<? super Integer>`表示，方法参数接受所有泛型类型为`Integer`或`Integer`父类的`Pair`类型。

下面的代码可以被正常编译：

```java
public class Main {
public static void main(String[] args) {
    Pair<Number> p1 = new Pair<>(12.3, 4.56);
    Pair<Integer> p2 = new Pair<>(123, 456);
    setSame(p1, 100);
    setSame(p2, 200);
    System.out.println(p1.getFirst() + ", " + p1.getLast());
    System.out.println(p2.getFirst() + ", " + p2.getLast());
}

static void setSame(Pair<? super Integer> p, Integer n) {
    p.setFirst(n);
    p.setLast(n);
}
}

class Pair<T> {
    private T first;
    private T last;

    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }

    public T getFirst() {
        return first;
    }
    public T getLast() {
        return last;
    }
    public void setFirst(T first) {
        this.first = first;
    }
    public void setLast(T last) {
        this.last = last;
    }
}
```

考察`Pair<? super Integer>`的`setFirst()`方法，它的方法签名实际上是： 

```java
void setFirst(? super Integer);
```

因此，可以安全地传入`Integer`类型。

再考察`Pair`的`getFirst()`方法，它的方法签名实际上是：

```java
? super Integer getFirst();
```

这里注意到我们无法使用`Integer`类型来接收`getFirst()`的返回值，即下面的语句将无法通过编译：

```java
Integer x = p.getFirst();
```

因为如果传入的实际类型是`Pair<Number>`，编译器无法将`Number`类型转型为`Integer`。

注意：虽然`Number`是一个抽象类，我们无法直接实例化它。但是，即便`Number`不是抽象类，这里仍然无法通过编译。此外，传入`Pair<Object>`类型时，编译器也无法将`Object`类型转型为`Integer`。

唯一可以接收`getFirst()`方法返回值的是`Object`类型：

```java
Object obj = p.getFirst();
```

因此，使用` <? super Integer> `通配符表示：

- 允许调用`set(? super Integer)`方法传入`Integer`的引用；
- 不允许调用`get()`方法获得`Integer`的引用。

唯一例外是可以获取`Object`的引用：`Object o = p.getFirst()`。

换句话说，**使用` <? super Integer> `通配符作为方法参数，表示方法内部代码对于参数只能写，不能读**。

#### 7.6.1 对比extends和super通配符

我们再回顾一下`extends`通配符。作为方法参数，` <? extends T> `类型和` <? super T> `类型的区别在于：

- ` <? extends T> `允许调用读方法`T get()`获取`T`的引用，但不允许调用写方法`set(T)`传入`T`的引用（传入`null`除外）；
- ` <? super T> `允许调用写方法`set(T)`传入`T`的引用，但不允许调用读方法`T get()`获取`T`的引用（获取`Object`除外）。

一个是允许读不允许写，另一个是允许写不允许读。

先记住上面的结论，我们来看Java标准库的`Collections`类定义的`copy()`方法：

```java
public class Collections {
    // 把src的每个元素复制到dest中:
    public static <T> void copy(List<? super T> dest, List<? extends T> src) {
        for (int i=0; i<src.size(); i++) {
            T t = src.get(i);
            dest.add(t);
        }
    }
}
```

它的作用是把一个`List`的每个元素依次添加到另一个`List`中。它的第一个参数是`List<? super T>`，表示目标`List`，第二个参数`List<? extends T>`，表示要复制的`List`。我们可以简单地用`for`循环实现复制。在`for`循环中，我们可以看到，对于类型` <? extends T> `的变量`src`，我们可以安全地获取类型`T`的引用，而对于类型` <? super T> `的变量`dest`，我们可以安全地传入`T`的引用。

这个`copy()`方法的定义就完美地展示了`extends`和`super`的意图：

- `copy()`方法内部不会读取`dest`，因为不能调用`dest.get()`来获取`T`的引用；
- `copy()`方法内部也不会修改`src`，因为不能调用`src.add(T)`。

这是由编译器检查来实现的。如果在方法代码中意外修改了`src`，或者意外读取了`dest`，就会导致一个编译错误：

```java
public class Collections {
    // 把src的每个元素复制到dest中:
    public static <T> void copy(List<? super T> dest, List<? extends T> src) {
        ...
        T t = dest.get(0); // compile error!
        src.add(t); // compile error!
    }
}
```

这个`copy()`方法的另一个好处是可以安全地把一个`List<Integer>`添加到`List<Number>`，但是无法反过来添加：

```java
// copy List<Integer> to List<Number> ok:
List<Number> numList = ...;
List<Integer> intList = ...;
Collections.copy(numList, intList);

// ERROR: cannot copy List<Number> to List<Integer>:
Collections.copy(intList, numList);
```

而这些都是通过`super`和`extends`通配符，并由编译器强制检查来实现的。

#### 7.6.2 PECS原则

何时使用`extends`，何时使用`super`？为了便于记忆，我们可以用PECS原则：Producer Extends Consumer Super。即：

- **如果需要返回`T`，它是生产者（Producer），要使用`extends`通配符；**
- **如果需要写入`T`，它是消费者（Consumer），要使用`super`通配符。**

还是以`Collections`的`copy()`方法为例：

```java
public class Collections {
    public static <T> void copy(List<? super T> dest, List<? extends T> src) {
        for (int i=0; i<src.size(); i++) {
            T t = src.get(i); // src是producer
            dest.add(t); // dest是consumer
        }
    }
}
```

需要返回`T`的`src`是生产者，因此声明为`List`，需要写入`T`的`dest`是消费者，因此声明为`List`。

#### 7.6.3 无限定通配符

我们已经讨论了` <? extends T> `和` <? super T> `作为方法参数的作用。实际上，Java的泛型还允许使用无限定通配符（Unbounded Wildcard Type），即只定义一个`?`：

```java
void sample(Pair<?> p) {
}
```

因为` <?> `通配符既没有`extends`，也没有`super`，因此：

- 不允许调用`set(T)`方法并传入引用（`null`除外）；
- 不允许调用`T get()`方法并获取`T`引用（只能获取`Object`引用）。

换句话说，既不能读，也不能写，那只能做一些`null`判断：

```java
static boolean isNull(Pair<?> p) {
    return p.getFirst() == null || p.getLast() == null;
}
```

大多数情况下，可以引入泛型参数`<T>`消除`<?>`通配符：

```java
static <T> boolean isNull(Pair<T> p) {
    return p.getFirst() == null || p.getLast() == null;
}
```

`<?>`通配符有一个独特的特点，就是：`Pair<?>`是所有`Pair<T>`的超类：

```java
public class Main {
    public static void main(String[] args) {
        Pair<Integer> p = new Pair<>(123, 456);
        Pair<?> p2 = p; // 安全地向上转型
        System.out.println(p2.getFirst() + ", " + p2.getLast());
    }
}

class Pair<T> {
    private T first;
    private T last;

    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }

    public T getFirst() {
        return first;
    }
    public T getLast() {
        return last;
    }
    public void setFirst(T first) {
        this.first = first;
    }
    public void setLast(T last) {
        this.last = last;
    }
}
```

上述代码是可以正常编译运行的，因为`Pair<Integer>`是`Pair<?>`的子类，可以安全地向上转型。

#### 7.6.4 小结

使用类似` <? super Integer> `通配符作为方法参数时表示：

- 方法内部可以调用传入`Integer`引用的方法，例如：`obj.setFirst(Integer n);`；
- 方法内部无法调用获取`Integer`引用的方法（`Object`除外），例如：`Integer n = obj.getFirst();`。

即使用`super`通配符表示只能写不能读。

使用`extends`和`super`通配符要遵循PECS原则。

无限定通配符` <?> `很少使用，可以用` <T> `替换，同时它是所有` <T> `类型的超类。

### 7.7 泛型和反射

Java的部分反射API也是泛型。例如：`Class`就是泛型：

```java
// compile warning:
Class clazz = String.class;
String str = (String) clazz.newInstance();

// no warning:
Class<String> clazz = String.class;
String str = clazz.newInstance();
```

调用`Class`的`getSuperclass()`方法返回的`Class`类型是`Class<? super T>`：

```java
Class<? super String> sup = String.class.getSuperclass();
```

构造方法`Constructor<T>`也是泛型：

```java
Class<Integer> clazz = Integer.class;
Constructor<Integer> cons = clazz.getConstructor(int.class);
Integer i = cons.newInstance(123);
```

我们可以声明带泛型的数组，但不能用`new`操作符创建带泛型的数组：

```java
Pair<String>[] ps = null; // ok
Pair<String>[] ps = new Pair<String>[2]; // compile error!
```

必须通过强制转型实现带泛型的数组：

```java
@SuppressWarnings("unchecked")
Pair<String>[] ps = (Pair<String>[]) new Pair[2];
```

使用泛型数组要特别小心，因为数组实际上在运行期没有泛型，编译器可以强制检查变量`ps`，因为它的类型是泛型数组。但是，编译器不会检查变量`arr`，因为它不是泛型数组。因为这两个变量实际上指向同一个数组，所以，操作`arr`可能导致从`ps`获取元素时报错，例如，以下代码演示了不安全地使用带泛型的数组：

```java
Pair[] arr = new Pair[2];
Pair<String>[] ps = (Pair<String>[]) arr;

ps[0] = new Pair<String>("a", "b");
arr[1] = new Pair<Integer>(1, 2);

// ClassCastException:
Pair<String> p = ps[1];
String s = p.getFirst();
```

要安全地使用泛型数组，必须扔掉`arr`的引用：

```java
@SuppressWarnings("unchecked")
Pair<String>[] ps = (Pair<String>[]) new Pair[2];
```

上面的代码中，由于拿不到原始数组的引用，就只能对泛型数组`ps`进行操作，这种操作就是安全的。带泛型的数组实际上是编译器的类型擦除：

```java
Pair[] arr = new Pair[2];
Pair<String>[] ps = (Pair<String>[]) arr;

System.out.println(ps.getClass() == Pair[].class); // true

String s1 = (String) arr[0].getFirst();
String s2 = ps[0].getFirst();
```

所以我们不能直接创建泛型数组`T[]`，因为擦拭后代码变为`Object[]`：

```java
// compile error:
public class Abc<T> {
    T[] createArray() {
        return new T[5];
    }
}
```

必须借助`Class`来创建泛型数组：

```java
T[] createArray(Class<T> cls) {
    return (T[]) Array.newInstance(cls, 5);
}
```

我们还可以利用可变参数创建泛型数组`T[]`：

```java
public class ArrayHelper {
    @SafeVarargs
    static <T> T[] asArray(T... objs) {
        return objs;
    }
}

String[] ss = ArrayHelper.asArray("a", "b", "c");
Integer[] ns = ArrayHelper.asArray(1, 2, 3);
```

#### 7.7.1 谨慎使用泛型可变参数

在上面的例子中，我们看到，通过：

```java
static <T> T[] asArray(T... objs) {
    return objs;
}
```

似乎可以安全地创建一个泛型数组。但实际上，这种方法非常危险。以下代码来自《Effective Java》的示例：

```java
import java.util.Arrays; 
public class Main {
    public static void main(String[] args) {
        String[] arr = asArray("one", "two", "three");
        System.out.println(Arrays.toString(arr));
        // ClassCastException:
        String[] firstTwo = pickTwo("one", "two", "three");
        System.out.println(Arrays.toString(firstTwo));
    }

    static <K> K[] pickTwo(K k1, K k2, K k3) {
        return asArray(k1, k2);
    }

    static <T> T[] asArray(T... objs) {
        return objs;
    }
```

直接调用`asArray(T...)`似乎没有问题，但是在另一个方法中，我们返回一个泛型数组就会产生`ClassCastException`，原因还是因为擦拭法，在`pickTwo()`方法内部，编译器无法检测`K[]`的正确类型，因此返回了`Object[]`。

如果仔细观察，可以发现编译器对所有可变泛型参数都会发出警告，除非确认完全没有问题，才可以用`@SafeVarargs`消除警告。**如果在方法内部创建了泛型数组，最好不要将它返回给外部使用。** 更详细的解释请参考《[Effective Java](https://www.oreilly.com/library/view/effective-java-3rd/9780134686097/)》“Item 32: Combine generics and varargs judiciously”。 

#### 7.7.2 小结

部分反射API是泛型，例如：`Class`，`Constructor`；

可以声明带泛型的数组，但不能直接创建带泛型的数组，必须强制转型；

可以通过`Array.newInstance(Class, int)`创建`T[]`数组，需要强制转型；

同时使用泛型和可变参数时需要特别小心。

## 8 集合

### 8.1 Java集合简介

什么是集合（Collection）？集合就是“由若干个确定的元素所构成的整体”。例如，5只小兔构成的集合：

```ascii
┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐

│   (\_(\     (\_/)     (\_/)     (\_/)      (\(\   │
    ( -.-)    (•.•)     (>.<)     (^.^)     (='.')
│  C(")_(")  (")_(")   (")_(")   (")_(")   O(_")")  │

└ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┘
```

在数学中，我们经常遇到集合的概念。例如：

- 有限集合：
  - 一个班所有的同学构成的集合；
  - 一个网站所有的商品构成的集合；
  - ...
- 无限集合：
  - 全体自然数集合：1，2，3，……
  - 有理数集合；
  - 实数集合；
  - ...

为什么要在计算机中引入集合呢？这是为了便于处理一组类似的数据，例如：

- 计算所有同学的总成绩和平均成绩；
- 列举所有的商品名称和价格；
- ……

在Java中，如果**一个Java对象可以在内部持有若干其他Java对象，并对外提供访问接口，我们把这种Java对象称为集合**。很显然，Java的数组可以看作是一种集合：

```java
String[] ss = new String[10]; // 可以持有10个String对象
ss[0] = "Hello"; // 可以放入String对象
String first = ss[0]; // 可以获取String对象
```

既然Java提供了数组这种数据类型，可以充当集合，那么，我们为什么还需要其他集合类？这是因为数组有如下限制：

- 数组初始化后大小不可变；
- 数组只能按索引顺序存取。

因此，我们需要各种不同类型的集合类来处理不同的数据，例如：

- 可变大小的顺序链表；
- 保证无重复元素的集合；
- ...

#### 8.1.1 Collection

Java标准库自带的`java.util`包提供了集合类：`Collection`，它是除`Map`外所有其他集合类的根接口。Java的`java.util`包主要提供了以下三种类型的集合：

- `List`：一种有序列表的集合，例如，按索引排列的`Student`的`List`；
- `Set`：一种保证没有重复元素的集合，例如，所有无重复名称的`Student`的`Set`；
- `Map`：一种通过键值（key-value）查找的映射表集合，例如，根据`Student`的`name`查找对应`Student`的`Map`。

Java集合的设计有几个特点：

- 一是实现了接口和实现类相分离，例如，有序表的接口是`List`，具体的实现类有`ArrayList`，`LinkedList`等，

- 二是支持泛型，我们可以限制在一个集合中只能放入同一种数据类型的元素，例如：

```java
List<String> list = new ArrayList<>(); // 只能放入String类型
```

最后，Java访问集合总是通过统一的方式——迭代器（Iterator）来实现，它最明显的好处在于无需知道集合内部元素是按什么方式存储的。

由于Java的集合设计非常久远，中间经历过大规模改进，我们要注意到有一小部分集合类是遗留类，不应该继续使用：

- `Hashtable`：一种线程安全的`Map`实现；
- `Vector`：一种线程安全的`List`实现；
- `Stack`：基于`Vector`实现的`LIFO`的栈。

还有一小部分接口是遗留接口，也不应该继续使用：

- `Enumeration<E>`：已被`Iterator<E>`取代。

#### 8.1.2 小结

Java的集合类定义在`java.util`包中，支持泛型，主要提供了3种集合类，包括`List`，`Set`和`Map`。Java集合使用统一的`Iterator`遍历，尽量不要使用遗留接口。

### 8.2 使用List

在集合类中，`List`是最基础的一种集合：它是一种有序链表。

`List`的行为和数组几乎完全相同：`List`内部按照放入元素的先后顺序存放，每个元素都可以通过索引确定自己的位置，`List`的索引和数组一样，从`0`开始。

数组和`List`类似，也是有序结构，如果我们使用数组，在添加和删除元素的时候，会非常不方便。例如，从一个已有的数组`{'A', 'B', 'C', 'D', 'E'}`中删除索引为`2`的元素：

```ascii
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ C │ D │ E │   │
└───┴───┴───┴───┴───┴───┘
              │   │
          ┌───┘   │
          │   ┌───┘
          │   │
          ▼   ▼
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ D │ E │   │   │
└───┴───┴───┴───┴───┴───┘
```

这个“删除”操作实际上是把`'C'`后面的元素依次往前挪一个位置，而“添加”操作实际上是把指定位置以后的元素都依次向后挪一个位置，腾出来的位置给新加的元素。这两种操作，用数组实现非常麻烦。

因此，在实际应用中，需要增删元素的有序列表，我们使用最多的是`ArrayList`。实际上，`ArrayList`在内部使用了数组来存储所有元素。例如，一个ArrayList拥有5个元素，实际数组大小为`6`（即有一个空位）：

```ascii
size=5
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ C │ D │ E │   │
└───┴───┴───┴───┴───┴───┘
```

当添加一个元素并指定索引到`ArrayList`时，`ArrayList`自动移动需要移动的元素：

```ascii
size=5
┌───┬───┬───┬───┬───┬───┐
│ A │ B │   │ C │ D │ E │
└───┴───┴───┴───┴───┴───┘
```

然后，往内部指定索引的数组位置添加一个元素，然后把`size`加`1`：

```ascii
size=6
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ F │ C │ D │ E │
└───┴───┴───┴───┴───┴───┘
```

继续添加元素，但是数组已满，没有空闲位置的时候，`ArrayList`先创建一个更大的新数组，然后把旧数组的所有元素复制到新数组，紧接着用新数组取代旧数组：

```ascii
size=6
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ A │ B │ F │ C │ D │ E │   │   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

现在，新数组就有了空位，可以继续添加一个元素到数组末尾，同时`size`加`1`：

```ascii
size=7
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ A │ B │ F │ C │ D │ E │ G │   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

可见，`ArrayList`把添加和删除的操作封装起来，让我们操作`List`类似于操作数组，却不用关心内部元素如何移动。

我们考察`List`接口，可以看到几个主要的接口方法：

- 在末尾添加一个元素：`void add(E e)`
- 在指定索引添加一个元素：`void add(int index, E e)`
- 删除指定索引的元素：`int remove(int index)`
- 删除某个元素：`int remove(Object e)`
- 获取指定索引的元素：`E get(int index)`
- 获取链表大小（包含元素的个数）：`int size()`

但是，实现`List`接口并非只能通过数组（即`ArrayList`的实现方式）来实现，另一种`LinkedList`通过“链表”也实现了List接口。在`LinkedList`中，它的内部每个元素都指向下一个元素：

```ascii
        ┌───┬───┐   ┌───┬───┐   ┌───┬───┐   ┌───┬───┐
HEAD ──>│ A │ ●─┼──>│ B │ ●─┼──>│ C │ ●─┼──>│ D │   │
        └───┴───┘   └───┴───┘   └───┴───┘   └───┴───┘
```

我们来比较一下`ArrayList`和`LinkedList`：

||ArrayList|LinkedList|
|--|--|--|
|获取指定元素|速度很快|需要从头开始查找元素|
|添加元素到末尾|速度很快|速度很快|
|在指定位置添加/删除|需要移动元素|不需要移动元素|
|内存占用|少|较大|

通常情况下，我们总是优先使用`ArrayList`。

#### 8.2.1 List的特点

使用`List`时，我们要关注`List`接口的规范。`List`接口允许我们添加重复的元素，即`List`内部的元素可以重复：

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple"); // size=1
        list.add("pear"); // size=2
        list.add("apple"); // 允许重复添加元素，size=3
        System.out.println(list.size());
    }
}
```

 `List`还允许添加`null`：

```java
public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple"); // size=1
        list.add(null); // size=2
        list.add("pear"); // size=3
        String second = list.get(1); // null
        System.out.println(second);
    }
}
```

#### 8.2.2 创建List

除了使用`ArrayList`和`LinkedList`，我们还可以通过`List`接口提供的`of()`方法，根据给定元素快速创建`List`：

```java
List<Integer> list = List.of(1, 2, 5);
```

但是`List.of()`方法不接受`null`值，如果传入`null`，会抛出`NullPointerException`异常。

#### 8.2.3 遍历List

和数组类型，我们要遍历一个List，完全可以用for循环根据索引配合`get(int)`方法遍历：

```java
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "pear", "banana");
        for (int i=0; i<list.size(); i++) {
            String s = list.get(i);
            System.out.println(s);
        }
    }
}
```

但这种方式并不推荐，一是代码复杂，二是因为`get(int)`方法只有`ArrayList`的实现是高效的，换成`LinkedList`后，索引越大，访问速度越慢。

所以我们要始终坚持使用迭代器`Iterator`来访问`List`。`Iterator`本身也是一个对象，但它是由`List`的实例调用`iterator()`方法的时候创建的。`Iterator`对象知道如何遍历一个`List`，并且不同的`List`类型，返回的`Iterator`对象实现也是不同的，但总是具有最高的访问效率。

`Iterator`对象有两个方法：`boolean hasNext()`判断是否有下一个元素，`E next()`返回下一个元素。因此，使用`Iterator`遍历`List`代码如下：

```java
import java.util.Iterator;
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "pear", "banana");
        for (Iterator<String> it = list.iterator(); it.hasNext(); ) {
            String s = it.next();
            System.out.println(s);
        }
    }
}
```

 可能觉得使用`Iterator`访问`List`的代码比使用索引更复杂。但是，要记住，通过`Iterator`遍历`List`永远是最高效的方式。并且，由于`Iterator`遍历是如此常用，所以，Java的`for each`循环本身就可以帮我们使用`Iterator`遍历。把上面的代码再改写如下：

```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "pear", "banana");
        for (String s : list) {
            System.out.println(s);
        }
    }
}
```

上述代码就是我们编写遍历`List`的常见代码。

实际上，只要实现了`Iterable`接口的集合类都可以直接用`for each`循环来遍历，Java编译器本身并不知道如何遍历集合对象，但它会自动把`for each`循环变成`Iterator`的调用，原因就在于`Iterable`接口定义了一个`Iterator iterator()`方法，强迫集合类必须返回一个`Iterator`实例。

#### 8.2.4 List和Array转换

把`List`变为`Array`有三种方法，第一种是调用`toArray()`方法直接返回一个`Object[]`数组：

```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "pear", "banana");
        Object[] array = list.toArray();
        for (Object s : array) {
            System.out.println(s);
        }
    }
}
```

这种方法会丢失类型信息，所以实际应用很少。

第二种方式是给`toArray(T[])`传入一个类型相同的`Array`，`List`内部自动把元素复制到传入的`Array`中：

```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<Integer> list = List.of(12, 34, 56);
        Integer[] array = list.toArray(new Integer[3]);
        for (Integer n : array) {
            System.out.println(n);
        }
    }
}
```

 注意到这个`toArray(T[])`方法的泛型参数`<T>`并不是`List`接口定义的泛型参数`<E>`，所以，我们实际上可以传入其他类型的数组，例如我们传入`Number`类型的数组，返回的仍然是`Number`类型：

```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<Integer> list = List.of(12, 34, 56);
        Number[] array = list.toArray(new Number[3]);
        for (Number n : array) {
            System.out.println(n);
        }
    }
}
```

但是，如果我们传入类型不匹配的数组，例如，`String[]`类型的数组，由于`List`的元素是`Integer`，所以无法放入`String`数组，这个方法会抛出`ArrayStoreException`。如果我们传入的数组大小和`List`实际的元素个数不一致怎么办？根据[List接口](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/List.html#toArray(T[]))的文档，我们可以知道：如果传入的数组不够大，那么`List`内部会创建一个新的刚好够大的数组，填充后返回；如果传入的数组比`List`元素还要多，那么填充完元素后，剩下的数组元素一律填充`null`。实际上，最常用的是传入一个“恰好”大小的数组：

```java
Integer[] array = list.toArray(new Integer[list.size()]);
```

最后一种更简洁的写法是通过`List`接口定义的`T[] toArray(IntFunction generator)`方法：

```java
Integer[] array = list.toArray(Integer[]::new);
```

这种函数式写法我们会在后续讲到。反过来，把`Array`变为`List`就简单多了，通过`List.of(T...)`方法最简单：

```java
Integer[] array = { 1, 2, 3 };
List<Integer> list = List.of(array);
```

对于JDK 11之前的版本，可以使用`Arrays.asList(T...)`方法把数组转换成`List`。要注意的是，返回的`List`不一定就是`ArrayList`或者`LinkedList`，因为`List`只是一个接口，如果我们调用`List.of()`，它返回的是一个只读`List`：

```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<Integer> list = List.of(12, 34, 56);
        list.add(999); // UnsupportedOperationException
    }
}
```

对只读`List`调用`add()`、`remove()`方法会抛出`UnsupportedOperationException`。

#### 8.2.5 小结

`List`是按索引顺序访问的长度可变的有序表，优先使用`ArrayList`而不是`LinkedList`；

可以直接使用`for each`遍历`List`；

`List`可以和`Array`相互转换。

### 8.3 编写equals方法

我们知道`List`是一种有序链表：`List`内部按照放入元素的先后顺序存放，并且每个元素都可以通过索引确定自己的位置。`List`还提供了`boolean contains(Object o)`方法来判断`List`是否包含某个指定元素。此外，`int indexOf(Object o)`方法可以返回某个元素的索引，如果元素不存在，就返回`-1`。我们来看一个例子：

```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("A", "B", "C");
        System.out.println(list.contains("C")); // true
        System.out.println(list.contains("X")); // false
        System.out.println(list.indexOf("C")); // 2
        System.out.println(list.indexOf("X")); // -1
    }
}
```

这里我们注意一个问题，我们往`List`中添加的`"C"`和调用`contains("C")`传入的`"C"`是不是同一个实例？如果这两个`"C"`不是同一个实例，这段代码是否还能得到正确的结果？我们可以改写一下代码测试一下：

```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("A", "B", "C");
        System.out.println(list.contains(new String("C"))); // true or false?
        System.out.println(list.indexOf(new String("C"))); // 2 or -1?
    }
}
```

因为我们传入的是`new String("C")`，所以一定是不同的实例。结果仍然符合预期，这是为什么呢？因为`List`内部并不是通过`==`判断两个元素是否相等，而是使用`equals()`方法判断两个元素是否相等，例如`contains()`方法可以实现如下：

```java
public class ArrayList {
    Object[] elementData;
    public boolean contains(Object o) {
        for (int i = 0; i < size; i++) {
            if (o.equals(elementData[i])) {
                return true;
            }
        }
        return false;
    }
}
```

因此，要正确使用`List`的`contains()`、`indexOf()`这些方法，放入的实例必须正确覆写`equals()`方法，否则，放进去的实例，查找不到。我们之所以能正常放入`String`、`Integer`这些对象，是因为Java标准库定义的这些类已经正确实现了`equals()`方法。

我们以`Person`对象为例，测试一下：

```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<Person> list = List.of(
            new Person("Xiao Ming"),
            new Person("Xiao Hong"),
            new Person("Bob")
        );
        System.out.println(list.contains(new Person("Bob"))); // false
    }
}

class Person {
    String name;
    public Person(String name) {
        this.name = name;
    }
}

```

不出意外，虽然放入了`new Person("Bob")`，但是用另一个`new Person("Bob")`查询不到，原因就是`Person`类没有覆写`equals()`方法。

#### 8.3.1 编写equals

如何正确编写`equals()`方法？`equals()`方法要求我们必须满足以下条件：

- **自反性（Reflexive）**：对于非`null`的`x`来说，`x.equals(x)`必须返回`true`；
- **对称性（Symmetric）**：对于非`null`的`x`和`y`来说，如果`x.equals(y)`为`true`，则`y.equals(x)`也必须为`true`；
- **传递性（Transitive）**：对于非`null`的`x`、`y`和`z`来说，如果`x.equals(y)`为`true`，`y.equals(z)`也为`true`，那么`x.equals(z)`也必须为`true`；
- **一致性（Consistent）**：对于非`null`的`x`和`y`来说，只要`x`和`y`状态不变，则`x.equals(y)`总是一致地返回`true`或者`false`；
- 对`null`的比较：即`x.equals(null)`永远返回`false`。

上述规则看上去似乎非常复杂，但其实代码实现`equals()`方法是很简单的，我们以`Person`类为例：

```java
public class Person {
    public String name;
    public int age;
}
```

首先，我们要定义“相等”的逻辑含义。对于`Person`类，如果`name`相等，并且`age`相等，我们就认为两个`Person`实例相等。

因此，编写`equals()`方法如下：

```java
public boolean equals(Object o) {
    if (o instanceof Person) {
        Person p = (Person) o;
        return this.name.equals(p.name) && this.age == p.age;
    }
    return false;
}
```

对于**引用字段比较，我们使用`equals()`**，对于**基本类型字段的比较，我们使用`==`**。

如果`this.name`为`null`，那么`equals()`方法会报错，因此，需要继续改写如下：

```java
public boolean equals(Object o) {
    if (o instanceof Person) {
        Person p = (Person) o;
        boolean nameEquals = false;
        if (this.name == null && p.name == null) {
            nameEquals = true;
        }
        if (this.name != null) {
            nameEquals = this.name.equals(p.name);
        }
        return nameEquals && this.age == p.age;
    }
    return false;
}
```

如果`Person`有好几个引用类型的字段，上面的写法就太复杂了。要简化引用类型的比较，我们使用`Objects.equals()`静态方法：

```java
public boolean equals(Object o) {
    if (o instanceof Person) {
        Person p = (Person) o;
        return Objects.equals(this.name, p.name) && this.age == p.age;
    }
    return false;
}
```

因此，我们总结一下`equals()`方法的正确编写方法：

1. 先确定实例“相等”的逻辑，即哪些字段相等，就认为实例相等；
2. 用`instanceof`判断传入的待比较的`Object`是不是当前类型，如果是，继续比较，否则，返回`false`；
3. 对引用类型用`Objects.equals()`比较，对基本类型直接用`==`比较。

使用`Objects.equals()`比较两个引用类型是否相等的目的是省去了判断`null`的麻烦。两个引用类型都是`null`时它们也是相等的。如果不调用`List`的`contains()`、`indexOf()`这些方法，那么放入的元素就不需要实现`equals()`方法。

#### 8.3.2 小结

在`List`中查找元素时，`List`的实现类通过元素的`equals()`方法比较两个元素是否相等，因此，放入的元素必须正确覆写`equals()`方法，Java标准库提供`String`、`Integer`等已经覆写了`equals()`方法；编写`equals()`方法可借助`Objects.equals()`判断。如果不在`List`中查找元素，就不必覆写`equals()`方法。

### 8.4 使用MAP

我们知道，`List`是一种顺序列表，如果有一个存储学生`Student`实例的`List`，要在`List`中根据`name`查找某个指定的`Student`的分数，应该怎么办？

最简单的方法是遍历`List`并判断`name`是否相等，然后返回指定元素：

```java
List<Student> list = ...
Student target = null;
for (Student s : list) {
    if ("Xiao Ming".equals(s.name)) {
        target = s;
        break;
    }
}
System.out.println(target.score);
```

这种需求其实非常常见，即通过一个键去查询对应的值。使用`List`来实现存在效率非常低的问题，因为平均需要扫描一半的元素才能确定，而`Map`这种键值（key-value）映射表的数据结构，作用就是能高效通过`key`快速查找`value`（元素）。

用`Map`来实现根据`name`查询某个`Student`的代码如下：

```java
import java.util.HashMap;
import java.util.Map;
public class Main {
    public static void main(String[] args) {
        Student s = new Student("Xiao Ming", 99);
        Map<String, Student> map = new HashMap<>();
        map.put("Xiao Ming", s); // 将"Xiao Ming"和Student实例映射并关联
        Student target = map.get("Xiao Ming"); // 通过key查找并返回映射的Student实例
        System.out.println(target == s); // true，同一个实例
        System.out.println(target.score); // 99
        Student another = map.get("Bob"); // 通过另一个key查找
        System.out.println(another); // 未找到返回null
    }
}

class Student {
    public String name;
    public int score;
    public Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
}
```

通过上述代码可知：`Map`是一种键-值映射表，当我们调用`put(K key, V value)`方法时，就把`key`和`value`做了映射并放入`Map`。当我们调用`V get(K key)`时，就可以通过`key`获取到对应的`value`。如果`key`不存在，则返回`null`。和`List`类似，**`Map`也是一个接口，最常用的实现类是`HashMap`**。如果只是想查询某个`key`是否存在，可以调用`boolean containsKey(K key)`方法。如果我们在存储`Map`映射关系的时候，对同一个key调用两次`put()`方法，分别放入不同的`value`，会有什么问题呢？例如：

```java
import java.util.HashMap;
import java.util.Map;
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        System.out.println(map.get("apple")); // 123
        map.put("apple", 789); // 再次放入apple作为key，但value变为789
        System.out.println(map.get("apple")); // 789
    }
}
```

重复放入`key-value`并不会有任何问题，但是一个`key`只能关联一个`value`。在上面的代码中，一开始我们把`key`对象`"apple"`映射到`Integer`对象`123`，然后再次调用`put()`方法把`"apple"`映射到`789`，这时，原来关联的`value`对象`123`就被“冲掉”了。实际上，`put()`方法的签名是`V put(K key, V value)`，如果放入的`key`已经存在，`put()`方法会返回被删除的旧的`value`，否则，返回`null`。

**始终牢记：Map中不存在重复的key，因为放入相同的key，只会把原有的key-value对应的value给替换掉。**

此外，在一个`Map`中，虽然`key`不能重复，但`value`是可以重复的：

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 123);
map.put("pear", 123); // ok
```

#### 8.4.1 遍历Map

对`Map`来说，要遍历`key`可以使用`for each`循环遍历`Map`实例的`keySet()`方法返回的`Set`集合，它包含不重复的`key`的集合：

```java
import java.util.HashMap;
import java.util.Map;
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        map.put("banana", 789);
        for (String key : map.keySet()) {
            Integer value = map.get(key);
            System.out.println(key + " = " + value);
        }
    }
}
```

同时遍历`key`和`value`可以使用`for each`循环遍历`Map`对象的`entrySet()`集合，它包含每一个`key-value`映射：

```java
import java.util.HashMap;
import java.util.Map;
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        map.put("banana", 789);
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            String key = entry.getKey();
            Integer value = entry.getValue();
            System.out.println(key + " = " + value);
        }
    }
}
```

`Map`和`List`不同的是，`Map`存储的是`key-value`的映射关系，并且，它*不保证顺序*。在遍历的时候，遍历的顺序既不一定是`put()`时放入的`key`的顺序，也不一定是`key`的排序顺序。使用`Map`时，任何依赖顺序的逻辑都是不可靠的。以`HashMap`为例，假设我们放入`"A"`，`"B"`，`"C"`这3个`key`，遍历的时候，每个`key`会保证被遍历一次且仅遍历一次，但顺序完全没有保证，甚至对于不同的JDK版本，相同的代码遍历的输出顺序都是不同的！

**遍历Map时，不可假设输出的key是有序的！**

#### 8.4.2 小结

`Map`是一种映射表，可以通过`key`快速查找`value`。

可以通过`for each`遍历`keySet()`，也可以通过`for each`遍历`entrySet()`，直接获取`key-value`。

最常用的一种`Map`实现是`HashMap`。

### 8.5 编写equals和hashCode

我们知道Map是一种键-值（key-value）映射表，可以通过key快速查找对应的value。

以HashMap为例，观察下面的代码：

```java
Map<String, Person> map = new HashMap<>();
map.put("a", new Person("Xiao Ming"));
map.put("b", new Person("Xiao Hong"));
map.put("c", new Person("Xiao Jun"));

map.get("a"); // Person("Xiao Ming")
map.get("x"); // null
```

`HashMap`之所以能根据`key`直接拿到`value`，原因是它内部通过空间换时间的方法，用一个大数组存储所有`value`，并根据key直接计算出`value`应该存储在哪个索引：

```ascii
  ┌───┐
0 │   │
  ├───┤
1 │ ●─┼───> Person("Xiao Ming")
  ├───┤
2 │   │
  ├───┤
3 │   │
  ├───┤
4 │   │
  ├───┤
5 │ ●─┼───> Person("Xiao Hong")
  ├───┤
6 │ ●─┼───> Person("Xiao Jun")
  ├───┤
7 │   │
  └───┘
```

如果`key`的值为`"a"`，计算得到的索引总是`1`，因此返回`value`为`Person("Xiao Ming")`，如果`key`的值为`"b"`，计算得到的索引总是`5`，因此返回`value`为`Person("Xiao Hong")`，这样，就不必遍历整个数组，即可直接读取`key`对应的`value`。

当我们使用`key`存取`value`的时候，就会引出一个问题：

我们放入`Map`的`key`是字符串`"a"`，但是，当我们获取`Map`的`value`时，传入的变量不一定就是放入的那个`key`对象。

换句话讲，**两个`key`应该是内容相同，但不一定是同一个对象**。测试代码如下：

```java
import java.util.HashMap;
import java.util.Map;
public class Main {
    public static void main(String[] args) {
        String key1 = "a";
        Map<String, Integer> map = new HashMap<>();
        map.put(key1, 123);

        String key2 = new String("a");
        map.get(key2); // 123

        System.out.println(key1 == key2); // false
        System.out.println(key1.equals(key2)); // true
    }
}
```

因为在`Map`的内部，对`key`做比较是通过`equals()`实现的，这一点和`List`查找元素需要正确覆写`equals()`是一样的，即正确使用`Map`必须保证：作为`key`的对象必须正确覆写`equals()`方法。

我们经常使用`String`作为`key`，因为`String`已经正确覆写了`equals()`方法。但如果我们放入的`key`是一个自己写的类，就必须保证正确覆写了`equals()`方法。

我们再思考一下`HashMap`为什么能通过`key`直接计算出`value`存储的索引。相同的`key`对象（使用`equals()`判断时返回`true`）必须要计算出相同的索引，否则，相同的`key`每次取出的`value`就不一定对。

通过`key`计算索引的方式就是调用`key`对象的`hashCode()`方法，它返回一个`int`整数。`HashMap`正是通过这个方法直接定位`key`对应的`value`的索引，继而直接返回`value`。

因此，正确使用`Map`必须保证：

1. 作为`key`的对象必须正确覆写`equals()`方法，相等的两个`key`实例调用`equals()`必须返回`true`；
2. 作为`key`的对象还必须正确覆写`hashCode()`方法，且`hashCode()`方法要严格遵循以下规范：

- 如果两个对象相等，则两个对象的`hashCode()`必须相等；
- 如果两个对象不相等，则两个对象的`hashCode()`尽量不要相等。

即对应两个实例`a`和`b`：

- 如果`a`和`b`相等，那么`a.equals(b)`一定为`true`，则`a.hashCode()`必须等于`b.hashCode()`；
- 如果`a`和`b`不相等，那么`a.equals(b)`一定为`false`，则`a.hashCode()`和`b.hashCode()`尽量不要相等。

上述第一条规范是正确性，必须保证实现，否则`HashMap`不能正常工作。

而第二条如果尽量满足，则可以保证查询效率，因为不同的对象，如果返回相同的`hashCode()`，会造成`Map`内部存储冲突，使存取的效率下降。

正确编写`equals()`的方法我们已经在`编写equals方法`一节中讲过了，以`Person`类为例：

```java
public class Person {
    String firstName;
    String lastName;
    int age;
}
```

把需要比较的字段找出来：

- firstName
- lastName
- age

然后，引用类型使用`Objects.equals()`比较，基本类型使用`==`比较。

在正确实现`equals()`的基础上，我们还需要正确实现`hashCode()`，即上述3个字段分别相同的实例，`hashCode()`返回的`int`必须相同：

```java
public class Person {
    String firstName;
    String lastName;
    int age;

    @Override
    int hashCode() {
        int h = 0;
        h = 31 * h + firstName.hashCode();
        h = 31 * h + lastName.hashCode();
        h = 31 * h + age;
        return h;
    }
}
```

注意到`String`类已经正确实现了`hashCode()`方法，我们在计算`Person`的`hashCode()`时，反复使用`31*h`，这样做的目的是为了尽量把不同的`Person`实例的`hashCode()`均匀分布到整个`int`范围。

和实现`equals()`方法遇到的问题类似，如果`firstName`或`lastName`为`null`，上述代码工作起来就会抛`NullPointerException`。为了解决这个问题，我们在计算`hashCode()`的时候，经常借助`Objects.hash()`来计算：

```java
int hashCode() {
    return Objects.hash(firstName, lastName, age);
}
```

所以，编写`equals()`和`hashCode()`遵循的原则是：

`equals()`用到的用于比较的每一个字段，都必须在`hashCode()`中用于计算；`equals()`中没有使用到的字段，绝不可放在`hashCode()`中计算。

另外注意，对于放入`HashMap`的`value`对象，没有任何要求。

#### 8.5.1 延伸阅读

既然`HashMap`内部使用了数组，通过计算`key`的`hashCode()`直接定位`value`所在的索引，那么第一个问题来了：hashCode()返回的`int`范围高达±21亿，先不考虑负数，`HashMap`内部使用的数组得有多大？

实际上`HashMap`初始化时默认的数组大小只有16，任何`key`，无论它的`hashCode()`有多大，都可以简单地通过：

```java
int index = key.hashCode() & 0xf; // 0xf = 15
```

把索引确定在0～15，即永远不会超出数组范围，上述算法只是一种最简单的实现。

第二个问题：如果添加超过16个`key-value`到`HashMap`，数组不够用了怎么办？

添加超过一定数量的`key-value`时，`HashMap`会在内部自动扩容，每次扩容一倍，即长度为16的数组扩展为长度32，相应地，需要重新确定`hashCode()`计算的索引位置。例如，对长度为32的数组计算`hashCode()`对应的索引，计算方式要改为：

```java
int index = key.hashCode() & 0x1f; // 0x1f = 31
```

由于扩容会导致重新分布已有的`key-value`，所以，频繁扩容对`HashMap`的性能影响很大。如果我们确定要使用一个容量为`10000`个`key-value`的`HashMap`，更好的方式是创建`HashMap`时就指定容量：

```java
Map<String, Integer> map = new HashMap<>(10000);
```

虽然指定容量是`10000`，但`HashMap`内部的数组长度总是2n，因此，实际数组长度被初始化为比`10000`大的`16384`（214）。

最后一个问题：如果不同的两个`key`，例如`"a"`和`"b"`，它们的`hashCode()`恰好是相同的（这种情况是完全可能的，因为不相等的两个实例，只要求`hashCode()`尽量不相等），那么，当我们放入：

```java
map.put("a", new Person("Xiao Ming"));
map.put("b", new Person("Xiao Hong"));
```

时，由于计算出的数组索引相同，后面放入的`"Xiao Hong"`会不会把`"Xiao Ming"`覆盖了？当然不会！使用`Map`的时候，只要`key`不相同，它们映射的`value`就互不干扰。但是，在`HashMap`内部，确实可能存在不同的`key`，映射到相同的`hashCode()`，即相同的数组索引上，肿么办？我们就假设`"a"`和`"b"`这两个`key`最终计算出的索引都是5，那么，在`HashMap`的数组中，实际存储的不是一个`Person`实例，而是一个`List`，它包含两个`Entry`，一个是`"a"`的映射，一个是`"b"`的映射：

```ascii
  ┌───┐
0 │   │
  ├───┤
1 │   │
  ├───┤
2 │   │
  ├───┤
3 │   │
  ├───┤
4 │   │
  ├───┤
5 │ ●─┼───> List<Entry<String, Person>>
  ├───┤
6 │   │
  ├───┤
7 │   │
  └───┘
```

在查找的时候，例如：

```java
Person p = map.get("a");
```

HashMap内部通过`"a"`找到的实际上是`List>`，它还需要遍历这个`List`，并找到一个`Entry`，它的`key`字段是`"a"`，才能返回对应的`Person`实例。我们把**不同的`key`具有相同的`hashCode()`的情况称之为哈希冲突**。在冲突的时候，一种最简单的解决办法是用`List`存储`hashCode()`相同的`key-value`。显然，如果冲突的概率越大，这个`List`就越长，`Map`的`get()`方法效率就越低，这就是为什么要尽量满足条件二：如果两个对象不相等，则两个对象的hashCode()尽量不要相等。

`hashCode()`方法编写得越好，`HashMap`工作的效率就越高。

#### 8.5.2 小结

要正确使用`HashMap`，作为`key`的类必须正确覆写`equals()`和`hashCode()`方法；

一个类如果覆写了`equals()`，就必须覆写`hashCode()`，并且覆写规则是：

- 如果`equals()`返回`true`，则`hashCode()`返回值必须相等；
- 如果`equals()`返回`false`，则`hashCode()`返回值尽量不要相等。

实现`hashCode()`方法可以通过`Objects.hashCode()`辅助方法实现。

### 8.6 使用EnumMap

因为`HashMap`是一种通过对key计算`hashCode()`，通过空间换时间的方式，直接定位到value所在的内部数组的索引，因此，查找效率非常高。

如果作为key的对象是`enum`类型，那么，还可以使用Java集合库提供的一种`EnumMap`，它在内部以一个非常紧凑的数组存储value，并且根据`enum`类型的key直接定位到内部数组的索引，并不需要计算`hashCode()`，不但效率最高，而且没有额外的空间浪费。

我们以`DayOfWeek`这个枚举类型为例，为它做一个“翻译”功能：

```java
import java.time.DayOfWeek;
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Map<DayOfWeek, String> map = new EnumMap<>(DayOfWeek.class);
        map.put(DayOfWeek.MONDAY, "星期一");
        map.put(DayOfWeek.TUESDAY, "星期二");
        map.put(DayOfWeek.WEDNESDAY, "星期三");
        map.put(DayOfWeek.THURSDAY, "星期四");
        map.put(DayOfWeek.FRIDAY, "星期五");
        map.put(DayOfWeek.SATURDAY, "星期六");
        map.put(DayOfWeek.SUNDAY, "星期日");
        System.out.println(map);
        System.out.println(map.get(DayOfWeek.MONDAY));
    }
}
```

使用`EnumMap`的时候，我们总是用`Map`接口来引用它，因此，实际上把`HashMap`和`EnumMap`互换，在客户端看来没有任何区别。

#### 8.6.1 小结

如果`Map`的key是`enum`类型，推荐使用`EnumMap`，既保证速度，也不浪费空间。

使用`EnumMap`的时候，根据面向抽象编程的原则，应持有`Map`接口。

### 8.7 使用TreeMap

我们已经知道，`HashMap`是一种以空间换时间的映射表，它的实现原理决定了内部的Key是无序的，即遍历`HashMap`的Key时，其顺序是不可预测的（但每个Key都会遍历一次且仅遍历一次）。

还有一种`Map`，它在内部会对Key进行排序，这种`Map`就是`SortedMap`。注意到`SortedMap`是接口，它的实现类是`TreeMap`。

```ascii
       ┌───┐
       │Map│
       └───┘
         ▲
    ┌────┴─────┐
    │          │
┌───────┐ ┌─────────┐
│HashMap│ │SortedMap│
└───────┘ └─────────┘
               ▲
               │
          ┌─────────┐
          │ TreeMap │
          └─────────┘
```

`SortedMap`保证遍历时以Key的顺序来进行排序。例如，放入的Key是`"apple"`、`"pear"`、`"orange"`，遍历的顺序一定是`"apple"`、`"orange"`、`"pear"`，因为`String`默认按字母排序：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new TreeMap<>();
        map.put("orange", 1);
        map.put("apple", 2);
        map.put("pear", 3);
        for (String key : map.keySet()) {
            System.out.println(key);
        }
        // apple, orange, pear
    }
}
```

使用`TreeMap`时，放入的Key必须实现`Comparable`接口。`String`、`Integer`这些类已经实现了`Comparable`接口，因此可以直接作为Key使用。作为Value的对象则没有任何要求。

如果作为Key的class没有实现`Comparable`接口，那么，必须在创建`TreeMap`时同时指定一个自定义排序算法：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Map<Person, Integer> map = new TreeMap<>(new Comparator<Person>() {
            public int compare(Person p1, Person p2) {
                return p1.name.compareTo(p2.name);
            }
        });
        map.put(new Person("Tom"), 1);
        map.put(new Person("Bob"), 2);
        map.put(new Person("Lily"), 3);
        for (Person key : map.keySet()) {
            System.out.println(key);
        }
        // {Person: Bob}, {Person: Lily}, {Person: Tom}
        System.out.println(map.get(new Person("Bob"))); // 2
    }
}

class Person {
    public String name;
    Person(String name) {
        this.name = name;
    }
    public String toString() {
        return "{Person: " + name + "}";
    }
}
```

注意到`Comparator`接口要求实现一个比较方法，它负责比较传入的两个元素`a`和`b`，如果`a，则返回负数，通常是`-1`，如果`a==b`，则返回`0`，如果`a>b`，则返回正数，通常是`1`。`TreeMap`内部根据比较结果对Key进行排序。

从上述代码执行结果可知，打印的Key确实是按照`Comparator`定义的顺序排序的。如果要根据Key查找Value，我们可以传入一个`new Person("Bob")`作为Key，它会返回对应的`Integer`值`2`。

另外，注意到`Person`类并未覆写`equals()`和`hashCode()`，因为`TreeMap`不使用`equals()`和`hashCode()`。

我们来看一个稍微复杂的例子：这次我们定义了`Student`类，并用分数`score`进行排序，高分在前：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Map<Student, Integer> map = new TreeMap<>(new Comparator<Student>() {
            public int compare(Student p1, Student p2) {
                return p1.score > p2.score ? -1 : 1;
            }
        });
        map.put(new Student("Tom", 77), 1);
        map.put(new Student("Bob", 66), 2);
        map.put(new Student("Lily", 99), 3);
        for (Student key : map.keySet()) {
            System.out.println(key);
        }
        System.out.println(map.get(new Student("Bob", 66))); // null?
    }
}

class Student {
    public String name;
    public int score;
    Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
    public String toString() {
        return String.format("{%s: score=%d}", name, score);
    }
}
```

在`for`循环中，我们确实得到了正确的顺序。但是，且慢！根据相同的Key：`new Student("Bob", 66)`进行查找时，结果为`null`！

这是怎么肥四？难道`TreeMap`有问题？遇到`TreeMap`工作不正常时，我们首先回顾Java编程基本规则：出现问题，不要怀疑Java标准库，要从自身代码找原因。

在这个例子中，`TreeMap`出现问题，原因其实出在这个`Comparator`上：

```java
public int compare(Student p1, Student p2) {
    return p1.score > p2.score ? -1 : 1;
}
```

在`p1.score`和`p2.score`不相等的时候，它的返回值是正确的，但是，在`p1.score`和`p2.score`相等的时候，它并没有返回`0`！这就是为什么`TreeMap`工作不正常的原因：`TreeMap`在比较两个Key是否相等时，依赖Key的`compareTo()`方法或者`Comparator.compare()`方法。在两个Key相等时，必须返回`0`。因此，修改代码如下：

```
public int compare(Student p1, Student p2) {
    if (p1.score == p2.score) {
        return 0;
    }
    return p1.score > p2.score ? -1 : 1;
}
```

或者直接借助`Integer.compare(int, int)`也可以返回正确的比较结果。

#### 8.7.1 小结

`SortedMap`在遍历时严格按照Key的顺序遍历，最常用的实现类是`TreeMap`；

作为`SortedMap`的Key必须实现`Comparable`接口，或者传入`Comparator`；

要严格按照`compare()`规范实现比较逻辑，否则，`TreeMap`将不能正常工作。

### 8.8 使用Properties

在编写应用程序的时候，经常需要读写配置文件。例如，用户的设置：

```java
# 上次最后打开的文件:
last_open_file=/data/hello.txt
# 自动保存文件的时间间隔:
auto_save_interval=60
```

配置文件的特点是，它的Key-Value一般都是`String`-`String`类型的，因此我们完全可以用`Map`来表示它。

因为配置文件非常常用，所以Java集合库提供了一个`Properties`来表示一组“配置”。由于历史遗留原因，`Properties`内部本质上是一个`Hashtable`，但我们只需要用到`Properties`自身关于读写配置的接口。

#### 8.8.1 读取配置文件

用`Properties`读取配置文件非常简单。Java默认配置文件以`.properties`为扩展名，每行以`key=value`表示，以`#`课开头的是注释。以下是一个典型的配置文件：

```java
# setting.properties

last_open_file=/data/hello.txt
auto_save_interval=60
```

可以从文件系统读取这个`.properties`文件：

```java
String f = "setting.properties";
Properties props = new Properties();
props.load(new java.io.FileInputStream(f));

String filepath = props.getProperty("last_open_file");
String interval = props.getProperty("auto_save_interval", "120");
```

可见，用`Properties`读取配置文件，一共有三步：

1. 创建`Properties`实例；
2. 调用`load()`读取文件；
3. 调用`getProperty()`获取配置。

调用`getProperty()`获取配置时，如果key不存在，将返回`null`。我们还可以提供一个默认值，这样，当key不存在的时候，就返回默认值。也可以从classpath读取`.properties`文件，因为`load(InputStream)`方法接收一个`InputStream`实例，表示一个字节流，它不一定是文件流，也可以是从jar包中读取的资源流：

```java
Properties props = new Properties();
props.load(getClass().getResourceAsStream("/common/setting.properties"));
```

试试从内存读取一个字节流：

```java
// properties
import java.io.*;
import java.util.Properties;

public class Main {
    public static void main(String[] args) throws IOException {
        String settings = "# test" + "\n" + "course=Java" + "\n" + "last_open_date=2019-08-07T12:35:01";
        ByteArrayInputStream input = new ByteArrayInputStream(settings.getBytes("UTF-8"));
        Properties props = new Properties();
        props.load(input);

        System.out.println("course: " + props.getProperty("course"));
        System.out.println("last_open_date: " + props.getProperty("last_open_date"));
        System.out.println("last_open_file: " + props.getProperty("last_open_file"));
        System.out.println("auto_save: " + props.getProperty("auto_save", "60"));
    }
}
```

如果有多个`.properties`文件，可以反复调用`load()`读取，后读取的key-value会覆盖已读取的key-value：

```properties
Properties props = new Properties();
props.load(getClass().getResourceAsString("/common/setting.properties"));
props.load(new FileInputStream("C:\\conf\\setting.properties"));
```

上面的代码演示了`Properties`的一个常用用法：可以把默认配置文件放到classpath中，然后，根据机器的环境编写另一个配置文件，覆盖某些默认的配置。

`Properties`设计的目的是存储`String`类型的key－value，但`Properties`实际上是从`Hashtable`派生的，它的设计实际上是有问题的，但是为了保持兼容性，现在已经没法修改了。除了`getProperty()`和`setProperty()`方法外，还有从`Hashtable`继承下来的`get()`和`put()`方法，这些方法的参数签名是`Object`，我们在使用`Properties`的时候，不要去调用这些从`Hashtable`继承下来的方法。

#### 8.8.2 写入配置文件

如果通过`setProperty()`修改了`Properties`实例，可以把配置写入文件，以便下次启动时获得最新配置。写入配置文件使用`store()`方法：

```properties
Properties props = new Properties();
props.setProperty("url", "http://www.liaoxuefeng.com");
props.setProperty("language", "Java");
props.store(new FileOutputStream("C:\\conf\\setting.properties"), "这是写入的properties注释");
```

#### 8.8.3 编码

早期版本的Java规定`.properties`文件编码是ASCII编码（ISO8859-1），如果涉及到中文就必须用`name=\u4e2d\u6587`来表示，非常别扭。从JDK9开始，Java的`.properties`文件可以使用UTF-8编码了。

不过，需要注意的是，由于`load(InputStream)`默认总是以ASCII编码读取字节流，所以会导致读到乱码。我们需要用另一个重载方法`load(Reader)`读取：

```java
Properties props = new Properties();
props.load(new FileReader("settings.properties", StandardCharsets.UTF_8));
```

就可以正常读取中文。`InputStream`和`Reader`的区别是一个是字节流，一个是字符流。字符流在内存中已经以`char`类型表示了，不涉及编码问题。

#### 8.8.4 小结

Java集合库提供的`Properties`用于读写配置文件`.properties`。`.properties`文件可以使用UTF-8编码。

可以从文件系统、classpath或其他任何地方读取`.properties`文件。

读写`Properties`时，注意仅使用`getProperty()`和`setProperty()`方法，不要调用继承而来的`get()`和`put()`等方法。

### 8.9 使用Set

我们知道，`Map`用于存储key-value的映射，对于充当key的对象，是不能重复的，并且，不但需要正确覆写`equals()`方法，还要正确覆写`hashCode()`方法。

如果我们只需要存储不重复的key，并不需要存储映射的value，那么就可以使用`Set`。

`Set`用于存储不重复的元素集合，它主要提供以下几个方法：

- 将元素添加进`Set`：`boolean add(E e)`
- 将元素从`Set`删除：`boolean remove(Object e)`
- 判断是否包含元素：`boolean contains(Object e)`

我们来看几个简单的例子：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Set<String> set = new HashSet<>();
        System.out.println(set.add("abc")); // true
        System.out.println(set.add("xyz")); // true
        System.out.println(set.add("xyz")); // false，添加失败，因为元素已存在
        System.out.println(set.contains("xyz")); // true，元素存在
        System.out.println(set.contains("XYZ")); // false，元素不存在
        System.out.println(set.remove("hello")); // false，删除失败，因为元素不存在
        System.out.println(set.size()); // 2，一共两个元素
    }
}
```

`Set`实际上相当于只存储key、不存储value的`Map`。我们经常用`Set`用于去除重复元素。因为放入`Set`的元素和`Map`的key类似，都要正确实现`equals()`和`hashCode()`方法，否则该元素无法正确地放入`Set`。最常用的`Set`实现类是`HashSet`，实际上，`HashSet`仅仅是对`HashMap`的一个简单封装，它的核心代码如下：

```java
public class HashSet<E> implements Set<E> {
    // 持有一个HashMap:
    private HashMap<E, Object> map = new HashMap<>();

    // 放入HashMap的value:
    private static final Object PRESENT = new Object();

    public boolean add(E e) {
        return map.put(e, PRESENT) == null;
    }

    public boolean contains(Object o) {
        return map.containsKey(o);
    }

    public boolean remove(Object o) {
        return map.remove(o) == PRESENT;
    }
}
```

`Set`接口并不保证有序，而`SortedSet`接口则保证元素是有序的：

- `HashSet`是无序的，因为它实现了`Set`接口，并没有实现`SortedSet`接口；
- `TreeSet`是有序的，因为它实现了`SortedSet`接口。

用一张图表示：

```ascii
       ┌───┐
       │Set│
       └───┘
         ▲
    ┌────┴─────┐
    │          │
┌───────┐ ┌─────────┐
│HashSet│ │SortedSet│
└───────┘ └─────────┘
               ▲
               │
          ┌─────────┐
          │ TreeSet │
          └─────────┘
```

我们来看`HashSet`的输出：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Set<String> set = new HashSet<>();
        set.add("apple");
        set.add("banana");
        set.add("pear");
        set.add("orange");
        for (String s : set) {
            System.out.println(s);
        }
    }
}
```

注意输出的顺序既不是添加的顺序，也不是`String`排序的顺序，在不同版本的JDK中，这个顺序也可能是不同的。把`HashSet`换成`TreeSet`，在遍历`TreeSet`时，输出就是有序的，这个顺序是元素的排序顺序：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Set<String> set = new TreeSet<>();
        set.add("apple");
        set.add("banana");
        set.add("pear");
        set.add("orange");
        for (String s : set) {
            System.out.println(s);
        }
    }
}
```

使用`TreeSet`和使用`TreeMap`的要求一样，添加的元素必须正确实现`Comparable`接口，如果没有实现`Comparable`接口，那么创建`TreeSet`时必须传入一个`Comparator`对象。

#### 8.9.1 小结

`Set`用于存储不重复的元素集合：

- 放入`HashSet`的元素与作为`HashMap`的key要求相同；
- 放入`TreeSet`的元素与作为`TreeMap`的Key要求相同；

利用`Set`可以去除重复元素；

遍历`SortedSet`按照元素的排序顺序遍历，也可以自定义排序算法。

### 8.10 使用Queue

队列（`Queue`）是一种经常使用的集合。`Queue`实际上是实现了一个先进先出（FIFO：First In First Out）的有序表。它和`List`的区别在于，`List`可以在任意位置添加和删除元素，而`Queue`只有两个操作：

- 把元素添加到队列末尾；
- 从队列头部取出元素。

在Java的标准库中，队列接口`Queue`定义了以下几个方法：

- `int size()`：获取队列长度；
- `boolean add(E)`/`boolean offer(E)`：添加元素到队尾；
- `E remove()`/`E poll()`：获取队首元素并从队列中删除；
- `E element()`/`E peek()`：获取队首元素但并不从队列中删除。

对于具体的实现类，有的Queue有最大队列长度限制，有的Queue没有。注意到添加、删除和获取队列元素总是有两个方法，这是因为在添加或获取元素失败时，这两个方法的行为是不同的。我们用一个表格总结如下：

||throwException|返回false或null|
|--|--|--|
|添加元素到队尾|add(Ee)|booleanoffer(Ee)|
|取队首元素并删除|Eremove()|Epoll()|
|取队首元素但不删除|Eelement()|Epeek()|

举个栗子，假设我们有一个队列，对它做一个添加操作，如果调用`add()`方法，当添加失败时（可能超过了队列的容量），它会抛出异常：

```java
Queue<String> q = ...
try {
    q.add("Apple");
    System.out.println("添加成功");
} catch(IllegalStateException e) {
    System.out.println("添加失败");
}
```

如果我们调用`offer()`方法来添加元素，当添加失败时，它不会抛异常，而是返回`false`：

```java
Queue<String> q = ...
if (q.offer("Apple")) {
    System.out.println("添加成功");
} else {
    System.out.println("添加失败");
}
```

当我们需要从`Queue`中取出队首元素时，如果当前`Queue`是一个空队列，调用`remove()`方法，它会抛出异常：

```java
Queue<String> q = ...
try {
    String s = q.remove();
    System.out.println("获取成功");
} catch(IllegalStateException e) {
    System.out.println("获取失败");
}
```

如果我们调用`poll()`方法来取出队首元素，当获取失败时，它不会抛异常，而是返回`null`：

```java
Queue<String> q = ...
String s = q.poll();
if (s != null) {
    System.out.println("获取成功");
} else {
    System.out.println("获取失败");
}
```

因此，两套方法可以根据需要来选择使用。注意：不要把`null`添加到队列中，否则`poll()`方法返回`null`时，很难确定是取到了`null`元素还是队列为空。接下来我们以`poll()`和`peek()`为例来说说“获取并删除”与“获取但不删除”的区别。对于`Queue`来说，每次调用`poll()`，都会获取队首元素，并且获取到的元素已经从队列中被删除了：

```java
import java.util.LinkedList;
import java.util.Queue;
public class Main {
    public static void main(String[] args) {
        Queue<String> q = new LinkedList<>();
        // 添加3个元素到队列:
        q.offer("apple");
        q.offer("pear");
        q.offer("banana");
        // 从队列取出元素:
        System.out.println(q.poll()); // apple
        System.out.println(q.poll()); // pear
        System.out.println(q.poll()); // banana
        System.out.println(q.poll()); // null,因为队列是空的
    }
}
```

如果用`peek()`，因为获取队首元素时，并不会从队列中删除这个元素，所以可以反复获取：

```java
import java.util.LinkedList;
import java.util.Queue;
public class Main {
    public static void main(String[] args) {
        Queue<String> q = new LinkedList<>();
        // 添加3个元素到队列:
        q.offer("apple");
        q.offer("pear");
        q.offer("banana");
        // 队首永远都是apple，因为peek()不会删除它:
        System.out.println(q.peek()); // apple
        System.out.println(q.peek()); // apple
        System.out.println(q.peek()); // apple
    }
}
```

从上面的代码中，我们还可以发现，`LinkedList`即实现了`List`接口，又实现了`Queue`接口，但是，在使用的时候，如果我们把它当作List，就获取List的引用，如果我们把它当作Queue，就获取Queue的引用：

```java
// 这是一个List:
List<String> list = new LinkedList<>();
// 这是一个Queue:
Queue<String> queue = new LinkedList<>();
```

始终按照面向抽象编程的原则编写代码，可以大大提高代码的质量。

#### 8.10.1 小结

队列`Queue`实现了一个先进先出（FIFO）的数据结构：

- 通过`add()`/`offer()`方法将元素添加到队尾；
- 通过`remove()`/`poll()`从队首获取元素并删除；
- 通过`element()`/`peek()`从队首获取元素但不删除。

要避免把`null`添加到队列。

### 8.11 使用PriorityQueue

我们知道，`Queue`是一个先进先出（FIFO）的队列。在银行柜台办业务时，我们假设只有一个柜台在办理业务，但是办理业务的人很多，怎么办？可以每个人先取一个号，例如：`A1`、`A2`、`A3`……然后，按照号码顺序依次办理，实际上这就是一个`Queue`。如果这时来了一个VIP客户，他的号码是`V1`，虽然当前排队的是`A10`、`A11`、`A12`……但是柜台下一个呼叫的客户号码却是`V1`。

这个时候，我们发现，要实现“VIP插队”的业务，用`Queue`就不行了，因为`Queue`会严格按FIFO的原则取出队首元素。我们需要的是优先队列：`PriorityQueue`。`PriorityQueue`和`Queue`的区别在于，它的出队顺序与元素的优先级有关，对`PriorityQueue`调用`remove()`或`poll()`方法，返回的总是优先级最高的元素。要使用`PriorityQueue`，我们就必须给每个元素定义“优先级”。我们以实际代码为例，先看看`PriorityQueue`的行为：

```java
import java.util.PriorityQueue;
import java.util.Queue;
public class Main {
    public static void main(String[] args) {
        Queue<String> q = new PriorityQueue<>();
        // 添加3个元素到队列:
        q.offer("apple");
        q.offer("pear");
        q.offer("banana");
        System.out.println(q.poll()); // apple
        System.out.println(q.poll()); // banana
        System.out.println(q.poll()); // pear
        System.out.println(q.poll()); // null,因为队列为空
    }
}
```

我们放入的顺序是`"apple"`、`"pear"`、`"banana"`，但是取出的顺序却是`"apple"`、`"banana"`、`"pear"`，这是因为从字符串的排序看，`"apple"`排在最前面，`"pear"`排在最后面。因此，放入`PriorityQueue`的元素，必须实现`Comparable`接口，`PriorityQueue`会根据元素的排序顺序决定出队的优先级。如果我们要放入的元素并没有实现`Comparable`接口怎么办？`PriorityQueue`允许我们提供一个`Comparator`对象来判断两个元素的顺序。我们以银行排队业务为例，实现一个`PriorityQueue`：

```java
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.Queue;
public class Main {
    public static void main(String[] args) {
        Queue<User> q = new PriorityQueue<>(new UserComparator());
        // 添加3个元素到队列:
        q.offer(new User("Bob", "A1"));
        q.offer(new User("Alice", "A2"));
        q.offer(new User("Boss", "V1"));
        System.out.println(q.poll()); // Boss/V1
        System.out.println(q.poll()); // Bob/A1
        System.out.println(q.poll()); // Alice/A2
        System.out.println(q.poll()); // null,因为队列为空
    }
}

class UserComparator implements Comparator<User> {
    public int compare(User u1, User u2) {
        if (u1.number.charAt(0) == u2.number.charAt(0)) {
            // 如果两人的号都是A开头或者都是V开头,比较号的大小:
            return u1.number.compareTo(u2.number);
        }
        if (u1.number.charAt(0) == 'V') {
            // u1的号码是V开头,优先级高:
            return -1;
        } else {
            return 1;
        }
    }
}

class User {
    public final String name;
    public final String number;

    public User(String name, String number) {
        this.name = name;
        this.number = number;
    }

    public String toString() {
        return name + "/" + number;
    }
}
```

实现`PriorityQueue`的关键在于提供的`UserComparator`对象，它负责比较两个元素的大小（较小的在前）。`UserComparator`总是把`V`开头的号码优先返回，只有在开头相同的时候，才比较号码大小。上面的`UserComparator`的比较逻辑其实还是有问题的，它会把`A10`排在`A2`的前面，请尝试修复该错误。

#### 8.11.1 小结

`PriorityQueue`实现了一个优先队列：从队首获取元素时，总是获取优先级最高的元素。

`PriorityQueue`默认按元素比较的顺序排序（必须实现`Comparable`接口），也可以通过`Comparator`自定义排序算法（元素就不必实现`Comparable`接口）。

### 8.12 使用Deque

我们知道，`Queue`是队列，只能一头进，另一头出。如果把条件放松一下，允许两头都进，两头都出，这种队列叫双端队列（Double Ended Queue），学名`Deque`。Java集合提供了接口`Deque`来实现一个双端队列，它的功能是：

- 既可以添加到队尾，也可以添加到队首；
- 既可以从队首获取，又可以从队尾获取。

我们来比较一下`Queue`和`Deque`出队和入队的方法：

| | Queue| Deque|
|--|--|--|
| 添加元素到队尾     | add(E e) / offer(E e)  | addLast(E e) / offerLast(E e)   |
| 取队首元素并删除   | E remove() / E poll()  | E removeFirst() / E pollFirst() |
| 取队首元素但不删除 | E element() / E peek() | E getFirst() / E peekFirst()    |
| 添加元素到队首     | 无                     | addFirst(E e) / offerFirst(E e) |
| 取队尾元素并删除   | 无                     | E removeLast() / E pollLast()   |
| 取队尾元素但不删除 | 无                     | E getLast() / E peekLast()      |

对于添加元素到队尾的操作，`Queue`提供了`add()`/`offer()`方法，而`Deque`提供了`addLast()`/`offerLast()`方法。添加元素到对首、取队尾元素的操作在`Queue`中不存在，在`Deque`中由`addFirst()`/`removeLast()`等方法提供。

注意到`Deque`接口实际上扩展自`Queue`：

```java
public interface Deque<E> extends Queue<E> {
    ...
}
```

因此，`Queue`提供的`add()`/`offer()`方法在`Deque`中也可以使用，但是，使用`Deque`，最好不要调用`offer()`，而是调用`offerLast()`：

```java
import java.util.Deque;
import java.util.LinkedList;
public class Main {
    public static void main(String[] args) {
        Deque<String> deque = new LinkedList<>();
        deque.offerLast("A"); // A
        deque.offerLast("B"); // B -> A
        deque.offerFirst("C"); // B -> A -> C
        System.out.println(deque.pollFirst()); // C, 剩下B -> A
        System.out.println(deque.pollLast()); // B
        System.out.println(deque.pollFirst()); // A
        System.out.println(deque.pollFirst()); // null
    }
}
```

如果直接写`deque.offer()`，我们就需要思考，`offer()`实际上是`offerLast()`，我们明确地写上`offerLast()`，不需要思考就能一眼看出这是添加到队尾。

因此，使用`Deque`，推荐总是明确调用`offerLast()`/`offerFirst()`或者`pollFirst()`/`pollLast()`方法。

`Deque`是一个接口，它的实现类有`ArrayDeque`和`LinkedList`。

我们发现`LinkedList`真是一个全能选手，它即是`List`，又是`Queue`，还是`Deque`。但是我们在使用的时候，总是用特定的接口来引用它，这是因为持有接口说明代码的抽象层次更高，而且接口本身定义的方法代表了特定的用途。

```java
// 不推荐的写法:
LinkedList<String> d1 = new LinkedList<>();
d1.offerLast("z");
// 推荐的写法：
Deque<String> d2 = new LinkedList<>();
d2.offerLast("z");
```

可见面向抽象编程的一个原则就是：**尽量持有接口，而不是具体的实现类**。

#### 8.12.1 小结

`Deque`实现了一个双端队列（Double Ended Queue），它可以：

- 将元素添加到队尾或队首：`addLast()`/`offerLast()`/`addFirst()`/`offerFirst()`；
- 从队首／队尾获取元素并删除：`removeFirst()`/`pollFirst()`/`removeLast()`/`pollLast()`；
- 从队首／队尾获取元素但不删除：`getFirst()`/`peekFirst()`/`getLast()`/`peekLast()`；
- 总是调用`xxxFirst()`/`xxxLast()`以便与`Queue`的方法区分开；
- 避免把`null`添加到队列。

### 8.13 使用Stack

栈（Stack）是一种后进先出（LIFO：Last In First Out）的数据结构。

什么是LIFO呢？我们先回顾一下`Queue`的特点FIFO：

```ascii
          ────────────────────────
  (\(\      (\(\    (\(\    (\(\      (\(\
 (='.') ─> (='.')  (='.')  (='.') ─> (='.')
O(_")")   O(_")") O(_")") O(_")")   O(_")")
          ────────────────────────
```

所谓FIFO，是最先进队列的元素一定最早出队列，而LIFO是最后进`Stack`的元素一定最早出`Stack`。如何做到这一点呢？只需要把队列的一端封死：

```ascii
           ───────────────────────────────┐
  (\(\       (\(\    (\(\    (\(\    (\(\ │
 (='.') <─> (='.')  (='.')  (='.')  (='.')│
O(_")")    O(_")") O(_")") O(_")") O(_")")│
           ───────────────────────────────┘
```

因此，`Stack`是这样一种数据结构：只能不断地往`Stack`中压入（push）元素，最后进去的必须最早弹出（pop）来。

`Stack`只有入栈和出栈的操作：

- 把元素压栈：`push(E)`；
- 把栈顶的元素“弹出”：`pop(E)`；
- 取栈顶元素但不弹出：`peek(E)`。

在Java中，我们用`Deque`可以实现`Stack`的功能：

- 把元素压栈：`push(E)`/`addFirst(E)`；
- 把栈顶的元素“弹出”：`pop(E)`/`removeFirst()`；
- 取栈顶元素但不弹出：`peek(E)`/`peekFirst()`。

为什么Java的集合类没有单独的`Stack`接口呢？因为有个遗留类名字就叫`Stack`，出于兼容性考虑，所以没办法创建`Stack`接口，只能用`Deque`接口来“模拟”一个`Stack`了。

当我们把`Deque`作为`Stack`使用时，注意只调用`push()`/`pop()`/`peek()`方法，不要调用`addFirst()`/`removeFirst()`/`peekFirst()`方法，这样代码更加清晰。

#### 8.13.1 Stack的作用

Stack在计算机中使用非常广泛，JVM在处理Java方法调用的时候就会通过栈这种数据结构维护方法调用的层次。例如：

```java
static void main(String[] args) {
    foo(123);
}

static String foo(x) {
    return "F-" + bar(x + 1);
}

static int bar(int x) {
    return x << 2;
}
```

JVM会创建方法调用栈，每调用一个方法时，先将参数压栈，然后执行对应的方法；当方法返回时，返回值压栈，调用方法通过出栈操作获得方法返回值。

因为方法调用栈有容量限制，嵌套调用过多会造成栈溢出，即引发`StackOverflowError`：

```java
// 测试无限递归调用
public class Main {
    public static void main(String[] args) {
        increase(1);
    }

    static int increase(int x) {
        return increase(x) + 1;
    }
}
```

我们再来看一个`Stack`的用途：对整数进行进制的转换就可以利用栈。

例如，我们要把一个`int`整数`12500`转换为十六进制表示的字符串，如何实现这个功能？

首先我们准备一个空栈：

```ascii
│   │
│   │
│   │
│   │
│   │
│   │
│   │
│   │
└───┘
```

然后计算12500÷16=781…4，余数是`4`，把余数`4`压栈：

```ascii
│   │
│   │
│   │
│   │
│   │
│   │
│   │
│ 4 │
└───┘
```

然后计算781÷16=48…13，余数是`13`，`13`的十六进制用字母`D`表示，把余数`D`压栈：

```ascii
│   │
│   │
│   │
│   │
│   │
│ D │
│   │
│ 4 │
└───┘
```

然后计算48÷16=3…0，余数是`0`，把余数`0`压栈：

```ascii
│   │
│   │
│   │
│ 0 │
│   │
│ D │
│   │
│ 4 │
└───┘
```

最后计算3÷16=0…3，余数是`3`，把余数`3`压栈：

```ascii
│   │
│ 3 │
│   │
│ 0 │
│   │
│ D │
│   │
│ 4 │
└───┘
```

当商是`0`的时候，计算结束，我们把栈的所有元素依次弹出，组成字符串`30D4`，这就是十进制整数`12500`的十六进制表示的字符串。

#### 8.13.2 计算中缀表达式

在编写程序的时候，我们使用的带括号的数学表达式实际上是中缀表达式，即运算符在中间，例如：`1 + 2 * (9 - 5)`。

但是计算机执行表达式的时候，它并不能直接计算中缀表达式，而是通过编译器把中缀表达式转换为后缀表达式，例如：`1 2 9 5 - * +`。

这个编译过程就会用到栈。我们先跳过编译这一步（涉及运算优先级，代码比较复杂），看看如何通过栈计算后缀表达式。

计算后缀表达式不考虑优先级，直接从左到右依次计算，因此计算起来简单。首先准备一个空的栈：

```ascii
│   │
│   │
│   │
│   │
│   │
│   │
│   │
│   │
└───┘
```

然后我们依次扫描后缀表达式`1 2 9 5 - * +`，遇到数字`1`，就直接扔到栈里：

```ascii
│   │
│   │
│   │
│   │
│   │
│   │
│   │
│ 1 │
└───┘
```

紧接着，遇到数字`2`，`9`，`5`，也扔到栈里：

```ascii
│   │
│ 5 │
│   │
│ 9 │
│   │
│ 2 │
│   │
│ 1 │
└───┘
```

接下来遇到减号时，弹出栈顶的两个元素，并计算`9-5=4`，把结果`4`压栈：

```ascii
│   │
│   │
│   │
│ 4 │
│   │
│ 2 │
│   │
│ 1 │
└───┘
```

接下来遇到`*`号时，弹出栈顶的两个元素，并计算`2*4=8`，把结果`8`压栈：

```ascii
│   │
│   │
│   │
│   │
│   │
│ 8 │
│   │
│ 1 │
└───┘
```

接下来遇到`+`号时，弹出栈顶的两个元素，并计算`1+8=9`，把结果`9`压栈：

```ascii
│   │
│   │
│   │
│   │
│   │
│   │
│   │
│ 9 │
└───┘
```

扫描结束后，没有更多的计算了，弹出栈的唯一一个元素，得到计算结果`9`。

#### 8.13.3 小结

栈（Stack）是一种后进先出（LIFO）的数据结构，操作栈的元素的方法有：

- 把元素压栈：`push(E)`；
- 把栈顶的元素“弹出”：`pop(E)`；
- 取栈顶元素但不弹出：`peek(E)`。

在Java中，我们用`Deque`可以实现`Stack`的功能，注意只调用`push()`/`pop()`/`peek()`方法，避免调用`Deque`的其他方法。

最后，不要使用遗留类`Stack`。

### 8.14 使用Iterator

Java的集合类都可以使用`for each`循环，`List`、`Set`和`Queue`会迭代每个元素，`Map`会迭代每个key。以`List`为例：

```java
List<String> list = List.of("Apple", "Orange", "Pear");
for (String s : list) {
    System.out.println(s);
}
```

实际上，Java编译器并不知道如何遍历`List`。上述代码能够编译通过，只是因为编译器把`for each`循环通过`Iterator`改写为了普通的`for`循环：

```java
for (Iterator<String> it = list.iterator(); it.hasNext(); ) {
     String s = it.next();
     System.out.println(s);
}
```

我们把这种通过`Iterator`对象遍历集合的模式称为**迭代器**。

使用迭代器的好处在于，调用方总是以统一的方式遍历各种集合类型，而不必关系它们内部的存储结构。

例如，我们虽然知道`ArrayList`在内部是以数组形式存储元素，并且，它还提供了`get(int)`方法。虽然我们可以用`for`循环遍历：

```java
for (int i=0; i<list.size(); i++) {
    Object value = list.get(i);
}
```

但是这样一来，调用方就必须知道集合的内部存储结构。并且，如果把`ArrayList`换成`LinkedList`，`get(int)`方法耗时会随着index的增加而增加。如果把`ArrayList`换成`Set`，上述代码就无法编译，因为`Set`内部没有索引。

用`Iterator`遍历就没有上述问题，**因为`Iterator`对象是集合对象自己在内部创建的，它自己知道如何高效遍历内部的数据集合**，调用方则获得了统一的代码，编译器才能把标准的`for each`循环自动转换为`Iterator`遍历。

如果我们自己编写了一个集合类，想要使用`for each`循环，只需满足以下条件：

- 集合类实现`Iterable`接口，该接口要求返回一个`Iterator`对象；
- 用`Iterator`对象迭代集合内部数据。

这里的关键在于，集合类通过调用`iterator()`方法，返回一个`Iterator`对象，这个对象必须自己知道如何遍历该集合。

一个简单的`Iterator`示例如下，它总是以倒序遍历集合：

```java
// Iterator
import java.util.*;

public class Main {
    public static void main(String[] args) {
        ReverseList<String> rlist = new ReverseList<>();
        rlist.add("Apple");
        rlist.add("Orange");
        rlist.add("Pear");
        for (String s : rlist) {
            System.out.println(s);
        }
    }
}

class ReverseList<T> implements Iterable<T> {

    private List<T> list = new ArrayList<>();

    public void add(T t) {
        list.add(t);
    }

    @Override
    public Iterator<T> iterator() {
        return new ReverseIterator(list.size());
    }

    class ReverseIterator implements Iterator<T> {
        int index;

        ReverseIterator(int index) {
            this.index = index;
        }

        @Override
        public boolean hasNext() {
            return index > 0;
        }

        @Override
        public T next() {
            index--;
            return ReverseList.this.list.get(index);
        }
    }
}
```

虽然`ReverseList`和`ReverseIterator`的实现类稍微比较复杂，但是，注意到这是底层集合库，只需编写一次。而调用方则完全按`for each`循环编写代码，根本不需要知道集合内部的存储逻辑和遍历逻辑。

在编写`Iterator`的时候，我们通常可以用一个内部类来实现`Iterator`接口，这个内部类可以直接访问对应的外部类的所有字段和方法。例如，上述代码中，内部类`ReverseIterator`可以用`ReverseList.this`获得当前外部类的`this`引用，然后通过这个`this`引用就可以访问`ReverseList`的所有字段和方法。

#### 8.14.1 小结

`Iterator`是一种抽象的数据访问模型。使用`Iterator`模式进行迭代的好处有：

- 对任何集合都采用同一种访问模型；
- 调用者对集合内部结构一无所知；
- 集合类返回的`Iterator`对象知道如何迭代。

Java提供了标准的迭代器模型，即集合类实现`java.util.Iterable`接口，返回`java.util.Iterator`实例。

### 8.15 使用Collections

`Collections`是JDK提供的工具类，同样位于`java.util`包中。它提供了一系列静态方法，能更方便地操作各种集合。

 **注意Collections结尾多了一个s，不是Collection！**

我们一般看方法名和参数就可以确认`Collections`提供的该方法的功能。例如，对于以下静态方法：

```java
public static boolean addAll(Collection<? super T> c, T... elements) { ... }
```

`addAll()`方法可以给一个`Collection`类型的集合添加若干元素。因为方法签名是`Collection`，所以我们可以传入`List`，`Set`等各种集合类型。

#### 8.15.1 创建空集合

`Collections`提供了一系列方法来创建空集合：

- 创建空List：`List emptyList()`
- 创建空Map：`Map emptyMap()`
- 创建空Set：`Set emptySet()`

要注意到返回的空集合是不可变集合，无法向其中添加或删除元素。

此外，也可以用各个集合接口提供的`of(T...)`方法创建空集合。例如，以下创建空`List`的两个方法是等价的：

```java
List<String> list1 = List.of();
List<String> list2 = Collections.emptyList();
```

#### 8.15.2 创建单元素集合

`Collections`提供了一系列方法来创建一个单元素集合：

- 创建一个元素的List：`List singletonList(T o)`
- 创建一个元素的Map：`Map singletonMap(K key, V value)`
- 创建一个元素的Set：`Set singleton(T o)`

要注意到返回的单元素集合也是不可变集合，无法向其中添加或删除元素。

此外，也可以用各个集合接口提供的`of(T...)`方法创建单元素集合。例如，以下创建单元素`List`的两个方法是等价的：

```java
List<String> list1 = List.of("apple");
List<String> list2 = Collections.singleton("apple");
```

实际上，使用`List.of(T...)`更方便，因为它既可以创建空集合，也可以创建单元素集合，还可以创建任意个元素的集合：

```java
List<String> list1 = List.of(); // empty list
List<String> list2 = List.of("apple"); // 1 element
List<String> list3 = List.of("apple", "pear"); // 2 elements
List<String> list4 = List.of("apple", "pear", "orange"); // 3 elements
```

#### 8.15.3 排序

`Collections`可以对`List`进行排序。因为排序会直接修改`List`元素的位置，因此必须传入可变`List`：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple");
        list.add("pear");
        list.add("orange");
        // 排序前:
        System.out.println(list);
        Collections.sort(list);
        // 排序后:
        System.out.println(list);
    }
}
```

#### 8.15.4 洗牌

`Collections`提供了洗牌算法，即传入一个有序的`List`，可以随机打乱`List`内部元素的顺序，效果相当于让计算机洗牌：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        for (int i=0; i<10; i++) {
            list.add(i);
        }
        // 洗牌前:
        System.out.println(list);
        Collections.shuffle(list);
        // 洗牌后:
        System.out.println(list);
    }
}
```

然而，继续对原始的可变`List`进行增删是可以的，并且，会直接影响到封装后的“不可变”`List`：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        List<String> mutable = new ArrayList<>();
        mutable.add("apple");
        mutable.add("pear");
        // 变为不可变集合:
        List<String> immutable = Collections.unmodifiableList(mutable);
        mutable.add("orange");
        System.out.println(immutable);
    }
}
```

因此，如果我们希望把一个可变`List`封装成不可变`List`，那么，返回不可变`List`后，最好立刻扔掉可变`List`的引用，这样可以保证后续操作不会意外改变原始对象，从而造成“不可变”`List`变化了：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        List<String> mutable = new ArrayList<>();
        mutable.add("apple");
        mutable.add("pear");
        // 变为不可变集合:
        List<String> immutable = Collections.unmodifiableList(mutable);
        // 立刻扔掉mutable的引用:
        mutable = null;
        System.out.println(immutable);
    }
}
```

#### 8.15.5 线程安全集合

`Collections`还提供了一组方法，可以把线程不安全的集合变为线程安全的集合：

- 变为线程安全的List：`List synchronizedList(List list)`
- 变为线程安全的Set：`Set synchronizedSet(Set s)`
- 变为线程安全的Map：`Map synchronizedMap(Map m)`

多线程的概念我们会在后面讲。因为从Java 5开始，引入了更高效的并发集合类，所以上述这几个同步方法已经没有什么用了。

#### 8.15.6 小结

`Collections`类提供了一组工具方法来方便使用集合类：

- 创建空集合；
- 创建单元素集合；
- 创建不可变集合；
- 排序／洗牌等操作。

## 9 IO

IO是指Input/Output，即输入和输出。以内存为中心：

- Input指从外部读入数据到内存，例如，把文件从磁盘读取到内存，从网络读取数据到内存等等。
- Output指把数据从内存输出到外部，例如，把数据从内存写入到文件，把数据从内存输出到网络等等。

为什么要把数据读到内存才能处理这些数据？因为代码是在内存中运行的，数据也必须读到内存，最终的表示方式无非是byte数组，字符串等，都必须存放在内存里。

从Java代码来看，输入实际上就是从外部，例如，硬盘上的某个文件，把内容读到内存，并且以Java提供的某种数据类型表示，例如，`byte[]`，`String`，这样，后续代码才能处理这些数据。

因为内存有“易失性”的特点，所以必须把处理后的数据以某种方式输出，例如，写入到文件。Output实际上就是把Java表示的数据格式，例如，`byte[]`，`String`等输出到某个地方。

IO流是一种顺序读写数据的模式，它的特点是单向流动。数据类似自来水一样在水管中流动，所以我们把它称为IO流。

**InputStream / OutputStream**

IO流以`byte`（字节）为最小单位，因此也称为*字节流*。例如，我们要从磁盘读入一个文件，包含6个字符，就相当于读入了6个字节的数据：

```ascii
╔════════════╗
║   Memory   ║
╚════════════╝
       ▲
       │0x48
       │0x65
       │0x6c
       │0x6c
       │0x6f
       │0x21
 ╔═══════════╗
 ║ Hard Disk ║
 ╚═══════════╝
```

这6个字节是按顺序读入的，所以是输入字节流。

反过来，我们把6个字节从内存写入磁盘文件，就是输出字节流：

```ascii
╔════════════╗
║   Memory   ║
╚════════════╝
       │0x21
       │0x6f
       │0x6c
       │0x6c
       │0x65
       │0x48
       ▼
 ╔═══════════╗
 ║ Hard Disk ║
 ╚═══════════╝
```

在Java中，`InputStream`代表输入字节流，`OuputStream`代表输出字节流，这是最基本的两种IO流。

**Reader / Writer**

如果我们需要读写的是字符，并且字符不全是单字节表示的ASCII字符，那么，按照`char`来读写显然更方便，这种流称为***字符流***。

Java提供了`Reader`和`Writer`表示字符流，字符流传输的最小数据单位是`char`。

例如，我们把`char[]`数组`Hi你好`这4个字符用`Writer`字符流写入文件，并且使用UTF-8编码，得到的最终文件内容是8个字节，英文字符`H`和`i`各占一个字节，中文字符`你好`各占3个字节：

```ascii
0x48
0x69
0xe4bda0
0xe5a5bd
```

反过来，我们用`Reader`读取以UTF-8编码的这8个字节，会从`Reader`中得到`Hi你好`这4个字符。

因此，**`Reader`和`Writer`本质上是一个能自动编解码的`InputStream`和`OutputStream`**。

使用`Reader`，数据源虽然是字节，但我们读入的数据都是`char`类型的字符，原因是`Reader`内部把读入的`byte`做了编码，转换成了`char`。使用`InputStream`，我们读入的数据和原始二进制数据一模一样，是`byte[]`数组，但是我们可以自己把二进制`byte[]`数组按照某种编码转换为字符串。究竟使用`Reader`还是`InputStream`，要取决于具体的使用场景。如果数据源不是文本，就只能使用`InputStream`，如果数据源是文本，使用Reader更方便一些。`Writer`和`OutputStream`是类似的。

**同步和异步**

同步IO是指，读写IO时代码必须等待数据返回后才继续执行后续代码，它的优点是代码编写简单，缺点是CPU执行效率低。

而异步IO是指，读写IO时仅发出请求，然后立刻执行后续代码，它的优点是CPU执行效率高，缺点是代码编写复杂。

Java标准库的包`java.io`提供了同步IO，而`java.nio`则是异步IO。上面我们讨论的`InputStream`、`OutputStream`、`Reader`和`Writer`都是同步IO的抽象类，对应的具体实现类，以文件为例，有`FileInputStream`、`FileOutputStream`、`FileReader`和`FileWriter`。

本节我们只讨论Java的同步IO，即输入/输出流的IO模型。

**小结**

IO流是一种流式的数据输入/输出模型：

- 二进制数据以`byte`为最小单位在`InputStream`/`OutputStream`中单向流动；
- 字符数据以`char`为最小单位在`Reader`/`Writer`中单向流动。

Java标准库的`java.io`包提供了同步IO功能：

- 字节流接口：`InputStream`/`OutputStream`；
- 字符流接口：`Reader`/`Writer`。

### 9.1 File对象

在计算机系统中，文件是非常重要的存储方式。Java的标准库`java.io`提供了`File`对象来操作文件和目录。

要构造一个`File`对象，需要传入文件路径：

```java
import java.io.*;
public class Main {
    public static void main(String[] args) {
        File f = new File("C:\\Windows\\notepad.exe");
        System.out.println(f);
    }
}

```

构造File对象时，既可以传入绝对路径，也可以传入相对路径。绝对路径是以根目录开头的完整路径，例如：

```java
File f = new File("C:\\Windows\\notepad.exe");
```

注意Windows平台使用`\`作为路径分隔符，在Java字符串中需要用`\\`表示一个`\`。Linux平台使用`/`作为路径分隔符：

```java
File f = new File("/usr/bin/javac");
```

传入相对路径时，相对路径前面加上当前目录就是绝对路径：

```java
// 假设当前目录是C:\Docs
File f1 = new File("sub\\javac"); // 绝对路径是C:\Docs\sub\javac
File f3 = new File(".\\sub\\javac"); // 绝对路径是C:\Docs\sub\javac
File f3 = new File("..\\sub\\javac"); // 绝对路径是C:\sub\javac
```

可以用`.`表示当前目录，`..`表示上级目录。

File对象有3种形式表示的路径，一种是`getPath()`，返回构造方法传入的路径，一种是`getAbsolutePath()`，返回绝对路径，一种是`getCanonicalPath`，它和绝对路径类似，但是返回的是规范路径。

什么是规范路径？我们看以下代码：

```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        File f = new File("..");
        System.out.println(f.getPath());
        System.out.println(f.getAbsolutePath());
        System.out.println(f.getCanonicalPath());
    }
}
```

绝对路径可以表示成`C:\Windows\System32\..\notepad.exe`，而规范路径就是把`.`和`..`转换成标准的绝对路径后的路径：`C:\Windows\notepad.exe`。

因为Windows和Linux的路径分隔符不同，File对象有一个静态变量用于表示当前平台的系统分隔符：

```java
System.out.println(File.separator); // 根据当前平台打印"\"或"/"
```

#### 9.1.1 文件和目录

`File`对象既可以表示文件，也可以表示目录。特别要注意的是，构造一个`File`对象，即使传入的文件或目录不存在，代码也不会出错，因为构造一个`File`对象，并不会导致任何磁盘操作。只有当我们调用`File`对象的某些方法的时候，才真正进行磁盘操作。

例如，调用`isFile()`，判断该`File`对象是否是一个已存在的文件，调用`isDirectory()`，判断该`File`对象是否是一个已存在的目录：

```java
public class Main {
    public static void main(String[] args) throws IOException {
        File f1 = new File("C:\\Windows");
        File f2 = new File("C:\\Windows\\notepad.exe");
        File f3 = new File("C:\\Windows\\nothing");
        System.out.println(f1.isFile());
        System.out.println(f1.isDirectory());
        System.out.println(f2.isFile());
        System.out.println(f2.isDirectory());
        System.out.println(f3.isFile());
        System.out.println(f3.isDirectory());
    }
}
```

用`File`对象获取到一个文件时，还可以进一步判断文件的权限和大小：

- `boolean canRead()`：是否可读；
- `boolean canWrite()`：是否可写；
- `boolean canExecute()`：是否可执行；
- `long length()`：文件字节大小。

对目录而言，是否可执行表示能否列出它包含的文件和子目录。

#### 9.1.2 创建和删除文件

当File对象表示一个文件时，可以通过`createNewFile()`创建一个新文件，用`delete()`删除该文件：

```java
File file = new File("/path/to/file");
if (file.createNewFile()) {
    // 文件创建成功:
    // TODO:
    if (file.delete()) {
        // 删除文件成功:
    }
}
```

有些时候，程序需要读写一些临时文件，File对象提供了`createTempFile()`来创建一个临时文件，以及`deleteOnExit()`在JVM退出时自动删除该文件。

```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        File f = File.createTempFile("tmp-", ".txt"); // 提供临时文件的前缀和后缀
        f.deleteOnExit(); // JVM退出时自动删除
        System.out.println(f.isFile());
        System.out.println(f.getAbsolutePath());
    }
}

```

#### 9.1.3 遍历文件和目录

当File对象表示一个目录时，可以使用`list()`和`listFiles()`列出目录下的文件和子目录名。`listFiles()`提供了一系列重载方法，可以过滤不想要的文件和目录：

```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        File f = new File("C:\\Windows");
        File[] fs1 = f.listFiles(); // 列出所有文件和子目录
        printFiles(fs1);
        File[] fs2 = f.listFiles(new FilenameFilter() { // 仅列出.exe文件
            public boolean accept(File dir, String name) {
                return name.endsWith(".exe"); // 返回true表示接受该文件
            }
        });
        printFiles(fs2);
    }

    static void printFiles(File[] files) {
        System.out.println("==========");
        if (files != null) {
            for (File f : files) {
                System.out.println(f);
            }
        }
        System.out.println("==========");
    }
}
```

和文件操作类似，File对象如果表示一个目录，可以通过以下方法创建和删除目录：

- `boolean mkdir()`：创建当前File对象表示的目录；
- `boolean mkdirs()`：创建当前File对象表示的目录，并在必要时将不存在的父目录也创建出来；
- `boolean delete()`：删除当前File对象表示的目录，当前目录必须为空才能删除成功。

#### 9.1.4 Path

Java标准库还提供了一个`Path`对象，它位于`java.nio.file`包。`Path`对象和`File`对象类似，但操作更加简单：

```java
import java.io.*;
import java.nio.file.*;
public class Main {
    public static void main(String[] args) throws IOException {
        Path p1 = Paths.get(".", "project", "study"); // 构造一个Path对象
        System.out.println(p1);
        Path p2 = p1.toAbsolutePath(); // 转换为绝对路径
        System.out.println(p2);
        Path p3 = p2.normalize(); // 转换为规范路径
        System.out.println(p3);
        File f = p3.toFile(); // 转换为File对象
        System.out.println(f);
        for (Path p : Paths.get("..").toAbsolutePath()) { // 可以直接遍历Path
            System.out.println("  " + p);
        }
    }
}
```

如果需要对目录进行复杂的拼接、遍历等操作，使用`Path`对象更方便。

#### 9.1.5 小结

Java标准库的`java.io.File`对象表示一个文件或者目录：

- 创建`File`对象本身不涉及IO操作；
- 可以获取路径／绝对路径／规范路径：`getPath()`/`getAbsolutePath()`/`getCanonicalPath()`；
- 可以获取目录的文件和子目录：`list()`/`listFiles()`；
- 可以创建或删除文件和目录。

### 9.2 InputStream

`InputStream`就是Java标准库提供的最基本的输入流。它位于`java.io`这个包里。`java.io`包提供了所有同步IO的功能。

要特别注意的一点是，`InputStream`并不是一个接口，而是一个抽象类，它是所有输入流的超类。这个抽象类定义的一个最重要的方法就是`int read()`，签名如下：

```java
public abstract int read() throws IOException;
```

这个方法会读取输入流的下一个字节，并返回字节表示的`int`值（0~255）。如果已读到末尾，返回`-1`表示不能继续读取了。

`FileInputStream`是`InputStream`的一个子类。顾名思义，`FileInputStream`就是从文件流中读取数据。下面的代码演示了如何完整地读取一个`FileInputStream`的所有字节：

```java
public void readFile() throws IOException {
    // 创建一个FileInputStream对象:
    InputStream input = new FileInputStream("src/readme.txt");
    for (;;) {
        int n = input.read(); // 反复调用read()方法，直到返回-1
        if (n == -1) {
            break;
        }
        System.out.println(n); // 打印byte的值
    }
    input.close(); // 关闭流
}
```

在计算机中，类似文件、网络端口这些资源，都是由操作系统统一管理的。应用程序在运行的过程中，如果打开了一个文件进行读写，完成后要及时地关闭，以便让操作系统把资源释放掉，否则，应用程序占用的资源会越来越多，不但白白占用内存，还会影响其他应用程序的运行。`InputStream`和`OutputStream`都是通过`close()`方法来关闭流。关闭流就会释放对应的底层资源。我们还要注意到在读取或写入IO流的过程中，可能会发生错误，例如，文件不存在导致无法读取，没有写权限导致写入失败，等等，这些底层错误由Java虚拟机自动封装成`IOException`异常并抛出。因此，所有与IO操作相关的代码都必须正确处理`IOException`。仔细观察上面的代码，会发现一个潜在的问题：如果读取过程中发生了IO错误，`InputStream`就没法正确地关闭，资源也就没法及时释放。因此，我们需要用`try ... finally`来保证`InputStream`在无论是否发生IO错误的时候都能够正确地关闭：

```java
public void readFile() throws IOException {
    InputStream input = null;
    try {
        input = new FileInputStream("src/readme.txt");
        int n;
        while ((n = input.read()) != -1) { // 利用while同时读取并判断
            System.out.println(n);
        }
    } finally {
        if (input != null) { input.close(); }
    }
}
```

用`try ... finally`来编写上述代码会感觉比较复杂，更好的写法是利用Java 7引入的新的`try(resource)`的语法，只需要编写`try`语句，让编译器自动为我们关闭资源。推荐的写法如下：

```java
public void readFile() throws IOException {
    try (InputStream input = new FileInputStream("src/readme.txt")) {
        int n;
        while ((n = input.read()) != -1) {
            System.out.println(n);
        }
    } // 编译器在此自动为我们写入finally并调用close()
}
```

实际上，编译器并不会特别地为`InputStream`加上自动关闭。编译器只看`try(resource = ...)`中的对象是否实现了`java.lang.AutoCloseable`接口，如果实现了，就自动加上`finally`语句并调用`close()`方法。`InputStream`和`OutputStream`都实现了这个接口，因此，都可以用在`try(resource)`中。

#### 9.2.1 缓冲

在读取流的时候，一次读取一个字节并不是最高效的方法。很多流支持一次性读取多个字节到缓冲区，对于文件和网络流来说，利用缓冲区一次性读取多个字节效率往往要高很多。`InputStream`提供了两个重载方法来支持读取多个字节：

- `int read(byte[] b)`：读取若干字节并填充到`byte[]`数组，返回读取的字节数
- `int read(byte[] b, int off, int len)`：指定`byte[]`数组的偏移量和最大填充数

利用上述方法一次读取多个字节时，需要先定义一个`byte[]`数组作为缓冲区，`read()`方法会尽可能多地读取字节到缓冲区， 但不会超过缓冲区的大小。`read()`方法的返回值不再是字节的`int`值，而是返回实际读取了多少个字节。如果返回`-1`，表示没有更多的数据了。

利用缓冲区一次读取多个字节的代码如下：

```java
public void readFile() throws IOException {
    try (InputStream input = new FileInputStream("src/readme.txt")) {
        // 定义1000个字节大小的缓冲区:
        byte[] buffer = new byte[1000];
        int n;
        while ((n = input.read(buffer)) != -1) { // 读取到缓冲区
            System.out.println("read " + n + " bytes.");
        }
    }
}
```

#### 9.2.2 阻塞

在调用`InputStream`的`read()`方法读取数据时，我们说`read()`方法是阻塞（Blocking）的。它的意思是，对于下面的代码：

```java
int n;
n = input.read(); // 必须等待read()方法返回才能执行下一行代码
int m = n;
```

执行到第二行代码时，必须等`read()`方法返回后才能继续。因为读取IO流相比执行普通代码，速度会慢很多，因此，无法确定`read()`方法调用到底要花费多长时间。

#### 9.2.3 InputStream实现类

用`FileInputStream`可以从文件获取输入流，这是`InputStream`常用的一个实现类。此外，`ByteArrayInputStream`可以在内存中模拟一个`InputStream`：

```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        byte[] data = { 72, 101, 108, 108, 111, 33 };
        try (InputStream input = new ByteArrayInputStream(data)) {
            int n;
            while ((n = input.read()) != -1) {
                System.out.println((char)n);
            }
        }
    }
}
```

`ByteArrayInputStream`实际上是把一个`byte[]`数组在内存中变成一个`InputStream`，虽然实际应用不多，但测试的时候，可以用它来构造一个`InputStream`。

举个栗子：我们想从文件中读取所有字节，并转换成`char`然后拼成一个字符串，可以这么写：

```java
public class Main {
    public static void main(String[] args) throws IOException {
        String s;
        try (InputStream input = new FileInputStream("C:\\test\\README.txt")) {
            int n;
            StringBuilder sb = new StringBuilder();
            while ((n = input.read()) != -1) {
                sb.append((char) n);
            }
            s = sb.toString();
        }
        System.out.println(s);
    }
}
```

要测试上面的程序，就真的需要在本地硬盘上放一个真实的文本文件。如果我们把代码稍微改造一下，提取一个`readAsString()`的方法：

```java
public class Main {
    public static void main(String[] args) throws IOException {
        String s;
        try (InputStream input = new FileInputStream("C:\\test\\README.txt")) {
            s = readAsString(input);
        }
        System.out.println(s);
    }

    public static String readAsString(InputStream input) throws IOException {
        int n;
        StringBuilder sb = new StringBuilder();
        while ((n = input.read()) != -1) {
            sb.append((char) n);
        }
        return sb.toString();
    }
}
```

对这个`String readAsString(InputStream input)`方法进行测试就相当简单，因为不一定要传入一个真的`FileInputStream`：

```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        byte[] data = { 72, 101, 108, 108, 111, 33 };
        try (InputStream input = new ByteArrayInputStream(data)) {
            String s = readAsString(input);
            System.out.println(s);
        }
    }

    public static String readAsString(InputStream input) throws IOException {
        int n;
        StringBuilder sb = new StringBuilder();
        while ((n = input.read()) != -1) {
            sb.append((char) n);
        }
        return sb.toString();
    }
}
```

这就是面向抽象编程原则的应用：接受`InputStream`抽象类型，而不是具体的`FileInputStream`类型，从而使得代码可以处理`InputStream`的任意实现类。

#### 9.2.4 小结

Java标准库的`java.io.InputStream`定义了所有输入流的超类：

- `FileInputStream`实现了文件流输入；
- `ByteArrayInputStream`在内存中模拟一个字节流输入。

总是使用`try(resource)`来保证`InputStream`正确关闭。

### 9.3 OutputStream

和`InputStream`相反，`OutputStream`是Java标准库提供的最基本的输出流。

和`InputStream`类似，`OutputStream`也是抽象类，它是所有输出流的超类。这个抽象类定义的一个最重要的方法就是`void write(int b)`，签名如下：

```java
public abstract void write(int b) throws IOException;
```

这个方法会写入一个字节到输出流。要注意的是，虽然传入的是`int`参数，但只会写入一个字节，即只写入`int`最低8位表示字节的部分（相当于`b & 0xff`）。

和`InputStream`类似，`OutputStream`也提供了`close()`方法关闭输出流，以便释放系统资源。要特别注意：`OutputStream`还提供了一个`flush()`方法，它的目的是将缓冲区的内容真正输出到目的地。

为什么要有`flush()`？因为向磁盘、网络写入数据的时候，出于效率的考虑，操作系统并不是输出一个字节就立刻写入到文件或者发送到网络，而是把输出的字节先放到内存的一个缓冲区里（本质上就是一个`byte[]`数组），等到缓冲区写满了，再一次性写入文件或者网络。对于很多IO设备来说，一次写一个字节和一次写1000个字节，花费的时间几乎是完全一样的，所以`OutputStream`有个`flush()`方法，能强制把缓冲区内容输出。

通常情况下，我们不需要调用这个`flush()`方法，因为缓冲区写满了`OutputStream`会自动调用它，并且，在调用`close()`方法关闭`OutputStream`之前，也会自动调用`flush()`方法。

但是，在某些情况下，我们必须手动调用`flush()`方法。举个栗子：

小明正在开发一款在线聊天软件，当用户输入一句话后，就通过`OutputStream`的`write()`方法写入网络流。小明测试的时候发现，发送方输入后，接收方根本收不到任何信息，怎么肥四？

原因就在于写入网络流是先写入内存缓冲区，等缓冲区满了才会一次性发送到网络。如果缓冲区大小是4K，则发送方要敲几千个字符后，操作系统才会把缓冲区的内容发送出去，这个时候，接收方会一次性收到大量消息。

解决办法就是每输入一句话后，立刻调用`flush()`，不管当前缓冲区是否已满，强迫操作系统把缓冲区的内容立刻发送出去。

实际上，`InputStream`也有缓冲区。例如，从`FileInputStream`读取一个字节时，操作系统往往会一次性读取若干字节到缓冲区，并维护一个指针指向未读的缓冲区。然后，每次我们调用`int read()`读取下一个字节时，可以直接返回缓冲区的下一个字节，避免每次读一个字节都导致IO操作。当缓冲区全部读完后继续调用`read()`，则会触发操作系统的下一次读取并再次填满缓冲区。

#### 9.3.1 FileOutputStream

我们以`FileOutputStream`为例，演示如何将若干个字节写入文件流：

```java
public void writeFile() throws IOException {
    OutputStream output = new FileOutputStream("out/readme.txt");
    output.write(72); // H
    output.write(101); // e
    output.write(108); // l
    output.write(108); // l
    output.write(111); // o
    output.close();
}
```

每次写入一个字节非常麻烦，更常见的方法是一次性写入若干字节。这时，可以用`OutputStream`提供的重载方法`void write(byte[])`来实现：

```java
public void writeFile() throws IOException {
    OutputStream output = new FileOutputStream("out/readme.txt");
    output.write("Hello".getBytes("UTF-8")); // Hello
    output.close();
}
```

和`InputStream`一样，上述代码没有考虑到在发生异常的情况下如何正确地关闭资源。写入过程也会经常发生IO错误，例如，磁盘已满，无权限写入等等。我们需要用`try(resource)`来保证`OutputStream`在无论是否发生IO错误的时候都能够正确地关闭：

```java
public void writeFile() throws IOException {
    try (OutputStream output = new FileOutputStream("out/readme.txt")) {
        output.write("Hello".getBytes("UTF-8")); // Hello
    } // 编译器在此自动为我们写入finally并调用close()
}
```

#### 9.3.2 阻塞

和`InputStream`一样，`OutputStream`的`write()`方法也是阻塞的。

#### 9.3.3 OutputStream实现类

用`FileOutputStream`可以从文件获取输出流，这是`OutputStream`常用的一个实现类。此外，`ByteArrayOutputStream`可以在内存中模拟一个`OutputStream`：

```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        byte[] data;
        try (ByteArrayOutputStream output = new ByteArrayOutputStream()) {
            output.write("Hello ".getBytes("UTF-8"));
            output.write("world!".getBytes("UTF-8"));
            data = output.toByteArray();
        }
        System.out.println(new String(data, "UTF-8"));
    }
}
```

`ByteArrayOutputStream`实际上是把一个`byte[]`数组在内存中变成一个`OutputStream`，虽然实际应用不多，但测试的时候，可以用它来构造一个`OutputStream`。

#### 9.3.4 小结

Java标准库的`java.io.OutputStream`定义了所有输出流的超类：

- `FileOutputStream`实现了文件流输出；
- `ByteArrayOutputStream`在内存中模拟一个字节流输出。

某些情况下需要手动调用`OutputStream`的`flush()`方法来强制输出缓冲区。

总是使用`try(resource)`来保证`OutputStream`正确关闭。

### 9.4 Filter模式

Java的IO标准库提供的`InputStream`根据来源可以包括：

- `FileInputStream`：从文件读取数据，是最终数据源；
- `ServletInputStream`：从HTTP请求读取数据，是最终数据源；
- `Socket.getInputStream()`：从TCP连接读取数据，是最终数据源；
- ...

如果我们要给`FileInputStream`添加缓冲功能，则可以从`FileInputStream`派生一个类：

```java
BufferedFileInputStream extends FileInputStream
```

如果要给`FileInputStream`添加计算签名的功能，类似的，也可以从`FileInputStream`派生一个类：

```java
DigestFileInputStream extends FileInputStream
```

如果要给`FileInputStream`添加加密/解密功能，还是可以从`FileInputStream`派生一个类：

```java
CipherFileInputStream extends FileInputStream
```

如果要给`FileInputStream`添加缓冲和签名的功能，那么我们还需要派生`BufferedDigestFileInputStream`。如果要给`FileInputStream`添加缓冲和加解密的功能，则需要派生`BufferedCipherFileInputStream`。

我们发现，给`FileInputStream`添加3种功能，至少需要3个子类。这3种功能的组合，又需要更多的子类：

```ascii
                             ┌────────────────┐
                             │FileInputStream │
                             └────────────────┘
                                     ▲
             ┌──────────-─┬──────────┼─────────┬─────────────┐
             │            │          │         │             │
┌─────────────────────────┐│┌───────────────────┐│┌─────────────────────┐
│BufferedFileInputStream  │││ DigestInputStream │││CipherFileInputStream│
└─────────────────────────┘│└───────────────────┘│└─────────────────────┘
                           │                     │
    ┌────────────────────────────────┐ ┌───────────────────────────────────┐
    │ BufferedDigestFileInputStream  │ │   BufferedCipherFileInputStream   │
    └────────────────────────────────┘ └───────────────────────────────────┘
```

这还只是针对`FileInputStream`设计，如果针对另一种`InputStream`设计，很快会出现子类爆炸的情况。

因此，直接使用继承，为各种`InputStream`附加更多的功能，根本无法控制代码的复杂度，很快就会失控。

为了解决依赖继承会导致子类数量失控的问题，JDK首先将`InputStream`分为两大类：

**一类是直接提供数据的基础`InputStream`**，例如：

- FileInputStream
- ByteArrayInputStream
- ServletInputStream
- ...

**一类是提供额外附加功能的`InputStream`**，例如：

- BufferedInputStream
- DigestInputStream
- CipherInputStream
- ...

当我们需要给一个“基础”`InputStream`附加各种功能时，我们先确定这个能提供数据源的`InputStream`，因为我们需要的数据总得来自某个地方，例如，`FileInputStream`，数据来源自文件：

```java
InputStream file = new FileInputStream("test.gz");
```

紧接着，我们希望`FileInputStream`能提供缓冲的功能来提高读取的效率，因此我们用`BufferedInputStream`包装这个`InputStream`，得到的包装类型是`BufferedInputStream`，但它仍然被视为一个`InputStream`：

```java
InputStream buffered = new BufferedInputStream(file);
```

最后，假设该文件已经用gzip压缩了，我们希望直接读取解压缩的内容，就可以再包装一个`GZIPInputStream`：

```java
InputStream gzip = new GZIPInputStream(buffered);
```

无论我们包装多少次，得到的对象始终是`InputStream`，我们直接用`InputStream`来引用它，就可以正常读取：

```ascii
┌─────────────────────────┐
│GZIPInputStream          │
│┌───────────────────────┐│
││BufferedFileInputStream││
││┌─────────────────────┐││
│││   FileInputStream   │││
││└─────────────────────┘││
│└───────────────────────┘│
└─────────────────────────┘
```

上述这种**通过一个“基础”组件再叠加各种“附加”功能组件的模式，称之为Filter模式**（或者装饰器模式：Decorator）。它可以让我们通过少量的类来实现各种功能的组合：

```ascii
                 ┌─────────────┐
                 │ InputStream │
                 └─────────────┘
                       ▲ ▲
┌────────────────────┐ │ │ ┌─────────────────┐
│  FileInputStream   │─┤ └─│FilterInputStream│
└────────────────────┘ │   └─────────────────┘
┌────────────────────┐ │     ▲ ┌───────────────────┐
│ByteArrayInputStream│─┤     ├─│BufferedInputStream│
└────────────────────┘ │     │ └───────────────────┘
┌────────────────────┐ │     │ ┌───────────────────┐
│ ServletInputStream │─┘     ├─│  DataInputStream  │
└────────────────────┘       │ └───────────────────┘
                             │ ┌───────────────────┐
                             └─│CheckedInputStream │
                               └───────────────────┘
```

类似的，`OutputStream`也是以这种模式来提供各种功能：

```ascii
                  ┌─────────────┐
                  │OutputStream │
                  └─────────────┘
                        ▲ ▲
┌─────────────────────┐ │ │ ┌──────────────────┐
│  FileOutputStream   │─┤ └─│FilterOutputStream│
└─────────────────────┘ │   └──────────────────┘
┌─────────────────────┐ │     ▲ ┌────────────────────┐
│ByteArrayOutputStream│─┤     ├─│BufferedOutputStream│
└─────────────────────┘ │     │ └────────────────────┘
┌─────────────────────┐ │     │ ┌────────────────────┐
│ ServletOutputStream │─┘     ├─│  DataOutputStream  │
└─────────────────────┘       │ └────────────────────┘
                              │ ┌────────────────────┐
                              └─│CheckedOutputStream │
                                └────────────────────┘
```

#### 9.4.1 编写FilterInputStream

我们也可以自己编写`FilterInputStream`，以便可以把自己的`FilterInputStream`“叠加”到任何一个`InputStream`中。

下面的例子演示了如何编写一个`CountInputStream`，它的作用是对输入的字节进行计数：

```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        byte[] data = "hello, world!".getBytes("UTF-8");
        try (CountInputStream input = new CountInputStream(new ByteArrayInputStream(data))) {
            int n;
            while ((n = input.read()) != -1) {
                System.out.println((char)n);
            }
            System.out.println("Total read " + input.getBytesRead() + " bytes");
        }
    }
}

class CountInputStream extends FilterInputStream {
    private int count = 0;

    CountInputStream(InputStream in) {
        super(in);
    }

    public int getBytesRead() {
        return this.count;
    }

    public int read() throws IOException {
        int n = in.read();
        if (n != -1) {
            this.count ++;
        }
        return n;
    }

    public int read(byte[] b, int off, int len) throws IOException {
        int n = in.read(b, off, len);
        this.count += n;
        return n;
    }
}
```

注意到在叠加多个`FilterInputStream`，我们只需要持有最外层的`InputStream`，并且，当最外层的`InputStream`关闭时（在`try(resource)`块的结束处自动关闭），内层的`InputStream`的`close()`方法也会被自动调用，并最终调用到最核心的“基础”`InputStream`，因此不存在资源泄露。

#### 9.4.2 小结

Java的IO标准库使用Filter模式为`InputStream`和`OutputStream`增加功能：

- 可以把一个`InputStream`和任意个`FilterInputStream`组合；
- 可以把一个`OutputStream`和任意个`FilterOutputStream`组合。

Filter模式可以在运行期动态增加功能（又称Decorator模式）。

### 9.5 操作Zip

`ZipInputStream`是一种`FilterInputStream`，它可以直接读取zip包的内容：

```ascii
┌───────────────────┐
│    InputStream    │
└───────────────────┘
          ▲
          │
┌───────────────────┐
│ FilterInputStream │
└───────────────────┘
          ▲
          │
┌───────────────────┐
│InflaterInputStream│
└───────────────────┘
          ▲
          │
┌───────────────────┐
│  ZipInputStream   │
└───────────────────┘
          ▲
          │
┌───────────────────┐
│  JarInputStream   │
└───────────────────┘
```

另一个`JarInputStream`是从`ZipInputStream`派生，它增加的主要功能是直接读取jar文件里面的`MANIFEST.MF`文件。因为本质上jar包就是zip包，只是额外附加了一些固定的描述文件。

#### 9.5.1 读取zip包

我们来看看`ZipInputStream`的基本用法。

我们要创建一个`ZipInputStream`，通常是传入一个`FileInputStream`作为数据源，然后，循环调用`getNextEntry()`，直到返回`null`，表示zip流结束。

一个`ZipEntry`表示一个压缩文件或目录，如果是压缩文件，我们就用`read()`方法不断读取，直到返回`-1`：

```java
try (ZipInputStream zip = new ZipInputStream(new FileInputStream(...))) {
    ZipEntry entry = null;
    while ((entry = zip.getNextEntry()) != null) {
        String name = entry.getName();
        if (!entry.isDirectory()) {
            int n;
            while ((n = zip.read()) != -1) {
                ...
            }
        }
    }
}
```

#### 9.5.2 写入zip包

`ZipOutputStream`是一种`FilterOutputStream`，它可以直接写入内容到zip包。我们要先创建一个`ZipOutputStream`，通常是包装一个`FileOutputStream`，然后，每写入一个文件前，先调用`putNextEntry()`，然后用`write()`写入`byte[]`数据，写入完毕后调用`closeEntry()`结束这个文件的打包。

```java
try (ZipOutputStream zip = new ZipOutputStream(new FileOutputStream(...))) {
    File[] files = ...
    for (File file : files) {
        zip.putNextEntry(new ZipEntry(file.getName()));
        zip.write(getFileDataAsBytes(file));
        zip.closeEntry();
    }
}
```

上面的代码没有考虑文件的目录结构。如果要实现目录层次结构，`new ZipEntry(name)`传入的`name`要用相对路径。

#### 9.5.3 小结

`ZipInputStream`可以读取zip格式的流，`ZipOutputStream`可以把多份数据写入zip包；

配合`FileInputStream`和`FileOutputStream`就可以读写zip文件。

### 9.6 读取classpath资源

很多Java程序启动的时候，都需要读取配置文件。例如，从一个`.properties`文件中读取配置：

```java
String conf = "C:\\conf\\default.properties";
try (InputStream input = new FileInputStream(conf)) {
    // TODO:
}
```

这段代码要正常执行，必须在C盘创建`conf`目录，然后在目录里创建`default.properties`文件。但是，在Linux系统上，路径和Windows的又不一样。因此，从磁盘的固定目录读取配置文件，不是一个好的办法。有没有路径无关的读取文件的方式呢？我们知道，Java存放`.class`的目录或jar包也可以包含任意其他类型的文件，例如：

- 配置文件，例如`.properties`；
- 图片文件，例如`.jpg`；
- 文本文件，例如`.txt`，`.csv`；
- ……

从classpath读取文件就可以避免不同环境下文件路径不一致的问题：如果我们把`default.properties`文件放到classpath中，就不用关心它的实际存放路径。在classpath中的资源文件，路径总是以`／`开头，我们先获取当前的`Class`对象，然后调用`getResourceAsStream()`就可以直接从classpath读取任意的资源文件：

```java
try (InputStream input = getClass().getResourceAsStream("/default.properties")) {
    // TODO:
}
```

调用`getResourceAsStream()`需要特别注意的一点是，如果资源文件不存在，它将返回`null`。因此，我们需要检查返回的`InputStream`是否为`null`，如果为`null`，表示资源文件在classpath中没有找到：

```java
try (InputStream input = getClass().getResourceAsStream("/default.properties")) {
    if (input != null) {
        // TODO:
    }
}
```

如果我们把默认的配置放到jar包中，再从外部文件系统读取一个可选的配置文件，就可以做到既有默认的配置文件，又可以让用户自己修改配置：

```java
Properties props = new Properties();
props.load(inputStreamFromClassPath("/default.properties"));
props.load(inputStreamFromFile("./conf.properties"));
```

这样读取配置文件，应用程序启动就更加灵活。

#### 9.6.1 小结

把资源存储在classpath中可以避免文件路径依赖；

`Class`对象的`getResourceAsStream()`可以从classpath中读取指定资源；

根据classpath读取资源时，需要检查返回的`InputStream`是否为`null`。

### 9.7 序列化

序列化是指把一个Java对象变成二进制内容，本质上就是一个`byte[]`数组。

为什么要把Java对象序列化呢？因为序列化后可以把`byte[]`保存到文件中，或者把`byte[]`通过网络传输到远程，这样，就相当于把Java对象存储到文件或者通过网络传输出去了。

有序列化，就有反序列化，即把一个二进制内容（也就是`byte[]`数组）变回Java对象。有了反序列化，保存到文件中的`byte[]`数组又可以“变回”Java对象，或者从网络上读取`byte[]`并把它“变回”Java对象。

我们来看看如何把一个Java对象序列化。

一个Java对象要能序列化，必须实现一个特殊的`java.io.Serializable`接口，它的定义如下：

```java
public interface Serializable {
}
```

`Serializable`接口没有定义任何方法，它是一个空接口。我们把这样的**空接口称为“标记接口”**（Marker Interface），实现了标记接口的类仅仅是给自身贴了个“标记”，并没有增加任何方法。

#### 9.7.1 序列化

把一个Java对象变为`byte[]`数组，需要使用`ObjectOutputStream`。它负责把一个Java对象写入一个字节流：

```java
import java.io.*;
import java.util.Arrays;
public class Main {
    public static void main(String[] args) throws IOException {
        ByteArrayOutputStream buffer = new ByteArrayOutputStream();
        try (ObjectOutputStream output = new ObjectOutputStream(buffer)) {
            // 写入int:
            output.writeInt(12345);
            // 写入String:
            output.writeUTF("Hello");
            // 写入Object:
            output.writeObject(Double.valueOf(123.456));
        }
        System.out.println(Arrays.toString(buffer.toByteArray()));
    }
}
```

`ObjectOutputStream`既可以写入基本类型，如`int`，`boolean`，也可以写入`String`（以UTF-8编码），还可以写入实现了`Serializable`接口的`Object`。因为写入`Object`时需要大量的类型信息，所以写入的内容很大。

#### 9.7.2 反序列化

和`ObjectOutputStream`相反，`ObjectInputStream`负责从一个字节流读取Java对象：

```java
try (ObjectInputStream input = new ObjectInputStream(...)) {
    int n = input.readInt();
    String s = input.readUTF();
    Double d = (Double) input.readObject();
}
```

除了能读取基本类型和`String`类型外，调用`readObject()`可以直接返回一个`Object`对象。要把它变成一个特定类型，必须强制转型。

`readObject()`可能抛出的异常有：

- `ClassNotFoundException`：没有找到对应的Class；
- `InvalidClassException`：Class不匹配。

对于`ClassNotFoundException`，这种情况常见于一台电脑上的Java程序把一个Java对象，例如，`Person`对象序列化以后，通过网络传给另一台电脑上的另一个Java程序，但是这台电脑的Java程序并没有定义`Person`类，所以无法反序列化。

对于`InvalidClassException`，这种情况常见于序列化的`Person`对象定义了一个`int`类型的`age`字段，但是反序列化时，`Person`类定义的`age`字段被改成了`long`类型，所以导致class不兼容。

为了避免这种class定义变动导致的不兼容，Java的序列化允许class定义一个特殊的`serialVersionUID`静态变量，用于标识Java类的序列化“版本”，通常可以由IDE自动生成。如果增加或修改了字段，可以改变`serialVersionUID`的值，这样就能自动阻止不匹配的class版本：

```java
public class Person implements Serializable {
    private static final long serialVersionUID = 2709425275741743919L;
}
```

要特别注意反序列化的几个重要特点：

反序列化时，由JVM直接构造出Java对象，不调用构造方法，构造方法内部的代码，在反序列化时根本不可能执行。

#### 9.7.3 安全性

因为Java的序列化机制可以导致一个实例能直接从`byte[]`数组创建，而不经过构造方法，因此，它存在一定的安全隐患。一个精心构造的`byte[]`数组被反序列化后可以执行特定的Java代码，从而导致严重的安全漏洞。

实际上，Java本身提供的基于对象的序列化和反序列化机制既存在安全性问题，也存在兼容性问题。更好的序列化方法是通过JSON这样的通用数据结构来实现，只输出基本类型（包括String）的内容，而不存储任何与代码相关的信息。

#### 9.7.4 小结

可序列化的Java对象必须实现`java.io.Serializable`接口，类似`Serializable`这样的空接口被称为“标记接口”（Marker Interface）；

反序列化时不调用构造方法，可设置`serialVersionUID`作为版本号（非必需）；

Java的序列化机制仅适用于Java，如果需要与其它语言交换数据，必须使用通用的序列化方法，例如JSON。

### 9.8 Reader

`Reader`是Java的IO库提供的另一个输入流接口。和`InputStream`的区别是，`InputStream`是一个字节流，即以`byte`为单位读取，而`Reader`是一个字符流，即以`char`为单位读取：

| InputStream                         | Reader                                |
| :---------------------------------- | :------------------------------------ |
| 字节流，以`byte`为单位              | 字符流，以`char`为单位                |
| 读取字节（-1，0~255）：`int read()` | 读取字符（-1，0~65535）：`int read()` |
| 读到字节数组：`int read(byte[] b)`  | 读到字符数组：`int read(char[] c)`    |

`java.io.Reader`是所有字符输入流的超类，它最主要的方法是：

```java
public int read() throws IOException;
```

这个方法读取字符流的下一个字符，并返回字符表示的`int`，范围是`0`~`65535`。如果已读到末尾，返回`-1`。

#### 9.8.1 FileReader

`FileReader`是`Reader`的一个子类，它可以打开文件并获取`Reader`。下面的代码演示了如何完整地读取一个`FileReader`的所有字符：

```java
public void readFile() throws IOException {
    // 创建一个FileReader对象:
    Reader reader = new FileReader("src/readme.txt"); // 字符编码是???
    for (;;) {
        int n = reader.read(); // 反复调用read()方法，直到返回-1
        if (n == -1) {
            break;
        }
        System.out.println((char)n); // 打印char
    }
    reader.close(); // 关闭流
}
```

如果我们读取一个纯ASCII编码的文本文件，上述代码工作是没有问题的。但如果文件中包含中文，就会出现乱码，因为`FileReader`默认的编码与系统相关，例如，Windows系统的默认编码可能是`GBK`，打开一个`UTF-8`编码的文本文件就会出现乱码。

要避免乱码问题，我们需要在创建`FileReader`时指定编码：

```java
Reader reader = new FileReader("src/readme.txt", StandardCharsets.UTF_8);
```

和`InputStream`类似，`Reader`也是一种资源，需要保证出错的时候也能正确关闭，所以我们需要用`try (resource)`来保证`Reader`在无论有没有IO错误的时候都能够正确地关闭：

```java
try (Reader reader = new FileReader("src/readme.txt", StandardCharsets.UTF_8) {
    // TODO
}
```

`Reader`还提供了一次性读取若干字符并填充到`char[]`数组的方法：

```java
public int read(char[] c) throws IOException
```

它返回实际读入的字符个数，最大不超过`char[]`数组的长度。返回`-1`表示流结束。

利用这个方法，我们可以先设置一个缓冲区，然后，每次尽可能地填充缓冲区：

```java
public void readFile() throws IOException {
    try (Reader reader = new FileReader("src/readme.txt", StandardCharsets.UTF_8)) {
        char[] buffer = new char[1000];
        int n;
        while ((n = reader.read(buffer)) != -1) {
            System.out.println("read " + n + " chars.");
        }
    }
}
```

#### 9.8.2 CharArrayReader

`CharArrayReader`可以在内存中模拟一个`Reader`，它的作用实际上是把一个`char[]`数组变成一个`Reader`，这和`ByteArrayInputStream`非常类似：

```java
try (Reader reader = new CharArrayReader("Hello".toCharArray())) {
}
```

#### 9.8.3 StringReader

`StringReader`可以直接把`String`作为数据源，它和`CharArrayReader`几乎一样：

```java
try (Reader reader = new StringReader("Hello")) {
}
```

#### 9.8.4 InputStreamReader

`Reader`和`InputStream`有什么关系？

除了特殊的`CharArrayReader`和`StringReader`，普通的`Reader`实际上是基于`InputStream`构造的，因为`Reader`需要从`InputStream`中读入字节流（`byte`），然后，根据编码设置，再转换为`char`就可以实现字符流。如果我们查看`FileReader`的源码，它在内部实际上持有一个`FileInputStream`。

既然`Reader`本质上是一个基于`InputStream`的`byte`到`char`的转换器，那么，如果我们已经有一个`InputStream`，想把它转换为`Reader`，是完全可行的。`InputStreamReader`就是这样一个转换器，它可以把任何`InputStream`转换为`Reader`。示例代码如下：

```java
// 持有InputStream:
InputStream input = new FileInputStream("src/readme.txt");
// 变换为Reader:
Reader reader = new InputStreamReader(input, "UTF-8");
```

构造`InputStreamReader`时，我们需要传入`InputStream`，还需要指定编码，就可以得到一个`Reader`对象。上述代码可以通过`try (resource)`更简洁地改写如下：

```java
try (Reader reader = new InputStreamReader(new FileInputStream("src/readme.txt"), "UTF-8")) {
    // TODO:
}
```

上述代码实际上就是`FileReader`的一种实现方式。

使用`try (resource)`结构时，当我们关闭`Reader`时，它会在内部自动调用`InputStream`的`close()`方法，所以，只需要关闭最外层的`Reader`对象即可。

 使用InputStreamReader，可以把一个InputStream转换成一个Reader。

#### 9.8.5 小结

`Reader`定义了所有字符输入流的超类：

- `FileReader`实现了文件字符流输入，使用时需要指定编码；
- `CharArrayReader`和`StringReader`可以在内存中模拟一个字符流输入。

`Reader`是基于`InputStream`构造的：可以通过`InputStreamReader`在指定编码的同时将任何`InputStream`转换为`Reader`。

总是使用`try (resource)`保证`Reader`正确关闭。

### 9.9 Writer

`Reader`是带编码转换器的`InputStream`，它把`byte`转换为`char`，而`Writer`就是带编码转换器的`OutputStream`，它把`char`转换为`byte`并输出。

`Writer`和`OutputStream`的区别如下：

| OutputStream                           | Writer                                   |
| :------------------------------------- | :--------------------------------------- |
| 字节流，以`byte`为单位                 | 字符流，以`char`为单位                   |
| 写入字节（0~255）：`void write(int b)` | 写入字符（0~65535）：`void write(int c)` |
| 写入字节数组：`void write(byte[] b)`   | 写入字符数组：`void write(char[] c)`     |
| 无对应方法                             | 写入String：`void write(String s)`       |

`Writer`是所有字符输出流的超类，它提供的方法主要有：

- 写入一个字符（0~65535）：`void write(int c)`；
- 写入字符数组的所有字符：`void write(char[] c)`；
- 写入String表示的所有字符：`void write(String s)`。

#### 9.9.1 FileWriter

`FileWriter`就是向文件中写入字符流的`Writer`。它的使用方法和`FileReader`类似：

```java
try (Writer writer = new FileWriter("readme.txt", StandardCharsets.UTF_8)) {
    writer.write('H'); // 写入单个字符
    writer.write("Hello".toCharArray()); // 写入char[]
    writer.write("Hello"); // 写入String
}
```

#### 9.9.2 CharArrayWriter

`CharArrayWriter`可以在内存中创建一个`Writer`，它的作用实际上是构造一个缓冲区，可以写入`char`，最后得到写入的`char[]`数组，这和`ByteArrayOutputStream`非常类似：

```java
try (CharArrayWriter writer = new CharArrayWriter()) {
    writer.write(65);
    writer.write(66);
    writer.write(67);
    char[] data = writer.toCharArray(); // { 'A', 'B', 'C' }
}
```

#### 9.9.3 StringWriter

`StringWriter`也是一个基于内存的`Writer`，它和`CharArrayWriter`类似。实际上，`StringWriter`在内部维护了一个`StringBuffer`，并对外提供了`Writer`接口。

#### 9.9.4 OutputStreamWriter

除了`CharArrayWriter`和`StringWriter`外，普通的Writer实际上是基于`OutputStream`构造的，它接收`char`，然后在内部自动转换成一个或多个`byte`，并写入`OutputStream`。因此，`OutputStreamWriter`就是一个将任意的`OutputStream`转换为`Writer`的转换器：

```java
try (Writer writer = new OutputStreamWriter(new FileOutputStream("readme.txt"), "UTF-8")) {
    // TODO:
}
```

上述代码实际上就是`FileWriter`的一种实现方式。这和上一节的`InputStreamReader`是一样的。

#### 9.9.5 小结

`Writer`定义了所有字符输出流的超类：

- `FileWriter`实现了文件字符流输出；
- `CharArrayWriter`和`StringWriter`在内存中模拟一个字符流输出。

使用`try (resource)`保证`Writer`正确关闭。

`Writer`是基于`OutputStream`构造的，可以通过`OutputStreamWriter`将`OutputStream`转换为`Writer`，转换时需要指定编码。

### 9.10 PrintStream和PrintWriter

`PrintStream`是一种`FilterOutputStream`，它在`OutputStream`的接口上，额外提供了一些写入各种数据类型的方法：

- 写入`int`：`print(int)`
- 写入`boolean`：`print(boolean)`
- 写入`String`：`print(String)`
- 写入`Object`：`print(Object)`，实际上相当于`print(object.toString())`
- ...

以及对应的一组`println()`方法，它会自动加上换行符。

我们经常使用的`System.out.println()`实际上就是使用`PrintStream`打印各种数据。其中，`System.out`是系统默认提供的`PrintStream`，表示标准输出：

```java
System.out.print(12345); // 输出12345
System.out.print(new Object()); // 输出类似java.lang.Object@3c7a835a
System.out.println("Hello"); // 输出Hello并换行
```

`System.err`是系统默认提供的标准错误输出。

`PrintStream`和`OutputStream`相比，除了添加了一组`print()`/`println()`方法，可以打印各种数据类型，比较方便外，它还有一个额外的优点，就是不会抛出`IOException`，这样我们在编写代码的时候，就不必捕获`IOException`。

#### 9.10.1 PrintWriter

`PrintStream`最终输出的总是byte数据，而`PrintWriter`则是扩展了`Writer`接口，它的`print()`/`println()`方法最终输出的是`char`数据。两者的使用方法几乎是一模一样的：

```java
import java.io.*;
public class Main {
    public static void main(String[] args)     {
        StringWriter buffer = new StringWriter();
        try (PrintWriter pw = new PrintWriter(buffer)) {
            pw.println("Hello");
            pw.println(12345);
            pw.println(true);
        }
        System.out.println(buffer.toString());
    }
}
```

#### 9.10.2 小结

`PrintStream`是一种能接收各种数据类型的输出，打印数据时比较方便：

- `System.out`是标准输出；
- `System.err`是标准错误输出。

`PrintWriter`是基于`Writer`的输出。

## 10 日期与时间

### 10.1 基本概念

在计算机中，我们经常需要处理日期和时间。

这是日期：

- 2019-11-20
- 2020-1-1

这是时间：

- 12:30:59
- 2020-1-1 20:21:59

日期是指某一天，它不是连续变化的，而是应该被看成离散的。

而时间有两种概念，一种是不带日期的时间，例如，12:30:59。另一种是带日期的时间，例如，2020-1-1 20:21:59，只有这种带日期的时间能唯一确定某个时刻，不带日期的时间是无法确定一个唯一时刻的。

#### 10.1.1 本地时间

当我们说当前时刻是2019年11月20日早上8:15的时候，我们说的实际上是本地时间。在国内就是北京时间。在这个时刻，如果地球上不同地方的人们同时看一眼手表，他们各自的本地时间是不同的，所以，不同的时区，在同一时刻，本地时间是不同的。全球一共分为24个时区，伦敦所在的时区称为标准时区，其他时区按东／西偏移的小时区分，北京所在的时区是东八区。

#### 10.1.2 时区

因为光靠本地时间还无法唯一确定一个准确的时刻，所以我们还需要给本地时间加上一个时区。时区有好几种表示方式。

一种是以`GMT`或者`UTC`加时区偏移表示，例如：`GMT+08:00`或者`UTC+08:00`表示东八区。

`GMT`和`UTC`可以认为基本是等价的，只是`UTC`使用更精确的原子钟计时，每隔几年会有一个闰秒，我们在开发程序的时候可以忽略两者的误差，因为计算机的时钟在联网的时候会自动与时间服务器同步时间。

另一种是缩写，例如，`CST`表示`China Standard Time`，也就是中国标准时间。但是`CST`也可以表示美国中部时间`Central Standard Time USA`，因此，缩写容易产生混淆，我们尽量不要使用缩写。

最后一种是以洲／城市表示，例如，`Asia/Shanghai`，表示上海所在地的时区。特别注意城市名称不是任意的城市，而是由国际标准组织规定的城市。

因为时区的存在，东八区的2019年11月20日早上8:15，和西五区的2019年11月19日晚上19:15，他们的时刻是相同的，时刻相同的意思就是，分别在两个时区的两个人，如果在这一刻通电话，他们各自报出自己手表上的时间，虽然本地时间是不同的，但是这两个时间表示的时刻是相同的。

#### 10.1.3 夏令时

时区还不是最复杂的，更复杂的是夏令时。所谓夏令时，就是夏天开始的时候，把时间往后拨1小时，夏天结束的时候，再把时间往前拨1小时。我们国家实行过一段时间夏令时，1992年就废除了，但是矫情的美国人到现在还在使用，所以时间换算更加复杂。

因为涉及到夏令时，相同的时区，如果表示的方式不同，转换出的时间是不同的。我们举个栗子：

对于2019-11-20和2019-6-20两个日期来说，假设北京人在纽约：

- 如果以`GMT`或者`UTC`作为时区，无论日期是多少，时间都是`19:00`；
- 如果以国家／城市表示，例如`America／NewYork`，虽然纽约也在西五区，但是，因为夏令时的存在，在不同的日期，`GMT`时间和纽约时间可能是不一样的：

| 时区             | 2019-11-20 | 2019-6-20 |
| :--------------- | :--------- | :-------- |
| GMT-05:00        | 19:00      | 19:00     |
| UTC-05:00        | 19:00      | 19:00     |
| America/New_York | 19:00      | 20:00     |

实行夏令时的不同地区，进入和退出夏令时的时间很可能是不同的。同一个地区，根据历史上是否实行过夏令时，标准时间在不同年份换算成当地时间也是不同的。因此，计算夏令时，没有统一的公式，必须按照一组给定的规则来算，并且，该规则要定期更新。

 **计算夏令时请使用标准库提供的相关类，不要试图自己计算夏令时。**

#### 10.1.4 本地化

在计算机中，通常使用`Locale`表示一个国家或地区的日期、时间、数字、货币等格式。`Locale`由`语言_国家`的字母缩写构成，例如，`zh_CN`表示中文+中国，`en_US`表示英文+美国。语言使用小写，国家使用大写。

对于日期来说，不同的Locale，例如，中国和美国的表示方式如下：

- zh_CN：2016-11-30
- en_US：11/30/2016

计算机用`Locale`在日期、时间、货币和字符串之间进行转换。一个电商网站会根据用户所在的`Locale`对用户显示如下：

|          | 中国用户   | 美国用户   |
| :------- | :--------- | :--------- |
| 购买价格 | 12000.00   | 12,000.00  |
| 购买日期 | 2016-11-30 | 11/30/2016 |

#### 10.1.5 小结

在编写日期和时间的程序前，我们要准确理解日期、时间和时刻的概念。

由于存在本地时间，我们需要理解时区的概念，并且必须牢记由于夏令时的存在，同一地区用`GMT/UTC`和城市表示的时区可能导致时间不同。

计算机通过`Locale`来针对当地用户习惯格式化日期、时间、数字、货币等。

### 10.2 Date和Calendar

在计算机中，应该如何表示日期和时间呢？

我们经常看到的日期和时间表示方式如下：

- 2019-11-20 0:15:00 GMT+00:00
- 2019年11月20日8:15:00
- 11/19/2019 19:15:00 America/New_York

如果直接以字符串的形式存储，那么不同的格式，不同的语言会让表示方式非常繁琐。

在理解日期和时间的表示方式之前，我们先要理解数据的存储和展示。

当我们定义一个整型变量并赋值时：

```java
int n = 123400;
```

编译器会把上述字符串（程序源码就是一个字符串）编译成字节码。在程序的运行期，变量`n`指向的内存实际上是一个4字节区域：

```ascii
┌──┬──┬──┬──┐
│00│01│e2│08│
└──┴──┴──┴──┘
```

注意到计算机内存除了二进制的`0`/`1`外没有其他任何格式。上述十六机制是为了简化表示。当我们用`System.out.println(n)`打印这个整数的时候，实际上`println()`这个方法在内部把`int`类型转换成`String`类型，然后打印出字符串`123400`。类似的，我们也可以以十六进制的形式打印这个整数，或者，如果`n`表示一个价格，我们就以`$123,400.00`的形式来打印它：

```java
import java.text.*;
import java.util.*;
public class Main {
    public static void main(String[] args) {
        int n = 123400;
        // 123400
        System.out.println(n);
        // 1e208
        System.out.println(Integer.toHexString(n));
        // $123,400.00
        System.out.println(NumberFormat.getCurrencyInstance(Locale.US).format(n));
    }
}
```

可见，整数`123400`是数据的存储格式，它的存储格式非常简单。而我们打印的各种各样的字符串，则是数据的展示格式。展示格式有多种形式，但本质上它就是一个转换方法：

```java
String toDisplay(int n) { ... }
```

理解了数据的存储和展示，我们回头看看以下几种日期和时间：

- 2019-11-20 0:15:01 GMT+00:00
- 2019年11月20日8:15:01
- 11/19/2019 19:15:01 America/New_York

它们实际上是数据的展示格式，分别按英国时区、中国时区、纽约时区对同一个时刻进行展示。而这个“同一个时刻”在计算机中存储的本质上只是一个整数，我们称它为`Epoch Time`。

`Epoch Time`是计算从1970年1月1日零点（格林威治时区／GMT+00:00）到现在所经历的秒数，例如：

`1574208900`表示从从1970年1月1日零点GMT时区到该时刻一共经历了1574208900秒，换算成伦敦、北京和纽约时间分别是：

```java
1574208900 = 北京时间2019-11-20 8:15:00
           = 伦敦时间2019-11-20 0:15:00
           = 纽约时间2019-11-19 19:15:00
```

因此，在计算机中，只需要存储一个整数`1574208900`表示某一时刻。当需要显示为某一地区的当地时间时，我们就把它格式化为一个字符串：

```java
String displayDateTime(int n, String timezone) { ... }
```

`Epoch Time`又称为时间戳，在不同的编程语言中，会有几种存储方式：

- 以秒为单位的整数：1574208900，缺点是精度只能到秒；
- 以毫秒为单位的整数：1574208900123，最后3位表示毫秒数；
- 以秒为单位的浮点数：1574208900.123，小数点后面表示零点几秒。

它们之间转换非常简单。而在Java程序中，时间戳通常是用`long`表示的毫秒数，即：

```java
long t = 1574208900123L;
```

转换成北京时间就是`2019-11-20T8:15:00.123`。要获取当前时间戳，可以使用`System.currentTimeMillis()`，这是Java程序获取时间戳最常用的方法。

#### 10.2.1 标准库API

我们再来看一下Java标准库提供的API。Java标准库有两套处理日期和时间的API：

- 一套定义在`java.util`这个包里面，主要包括`Date`、`Calendar`和`TimeZone`这几个类；
- 一套新的API是在Java 8引入的，定义在`java.time`这个包里面，主要包括`LocalDateTime`、`ZonedDateTime`、`ZoneId`等。

为什么会有新旧两套API呢？因为历史遗留原因，旧的API存在很多问题，所以引入了新的API。

那么我们能不能跳过旧的API直接用新的API呢？如果涉及到遗留代码就不行，因为很多遗留代码仍然使用旧的API，所以目前仍然需要对旧的API有一定了解，很多时候还需要在新旧两种对象之间进行转换。

本节我们快速讲解旧API的常用类型和方法。

#### 10.2.2 Date

`java.util.Date`是用于表示一个日期和时间的对象，注意与`java.sql.Date`区分，后者用在数据库中。如果观察Date的源码，可以发现它实际上存储了一个long类型的以毫秒表示的时间戳：

```java
public class Date implements Serializable, Cloneable, Comparable<Date> {

    private transient long fastTime;

    ...
}
```

我们来看Date的基本用法：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        // 获取当前时间:
        Date date = new Date();
        System.out.println(date.getYear() + 1900); // 必须加上1900
        System.out.println(date.getMonth() + 1); // 0~11，必须加上1
        System.out.println(date.getDate()); // 1~31，不能加1
        // 转换为String:
        System.out.println(date.toString());
        // 转换为GMT时区:
        System.out.println(date.toGMTString());
        // 转换为本地时区:
        System.out.println(date.toLocaleString());
    }
}
```

注意`getYear()`返回的年份必须加上`1900`，`getMonth()`返回的月份是`0`~`11`分别表示1~12月，所以要加1，而`getDate()`返回的日期范围是`1`~`31`，又不能加1。

打印本地时区表示的日期和时间时，不同的计算机可能会有不同的结果。如果我们想要针对用户的偏好精确地控制日期和时间的格式，就可以使用`SimpleDateFormat`对一个`Date`进行转换。它用预定义的字符串表示格式化：

- yyyy：年
- MM：月
- dd: 日
- HH: 小时
- mm: 分钟
- ss: 秒

我们来看如何以自定义的格式输出：

```java
import java.text.*;
import java.util.*;
public class Main {
    public static void main(String[] args) {
        // 获取当前时间:
        Date date = new Date();
        var sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        System.out.println(sdf.format(date));
    }
}
```

Java的格式化预定义了许多不同的格式，我们以`MMM`和`E`为例：

```java
import java.text.*;
import java.util.*;
public class Main {
    public static void main(String[] args) {
        // 获取当前时间:
        Date date = new Date();
        var sdf = new SimpleDateFormat("E MMM dd, yyyy");
        System.out.println(sdf.format(date));
    }
}
```

上述代码在不同的语言环境会打印出类似`Sun Sep 15, 2019`这样的日期。可以从[JDK文档](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/text/SimpleDateFormat.html)查看详细的格式说明。一般来说，字母越长，输出越长。以`M`为例，假设当前月份是9月：

- `M`：输出`9`
- `MM`：输出`09`
- `MMM`：输出`Sep`
- `MMMM`：输出`September`

`Date`对象有几个严重的问题：它不能转换时区，除了`toGMTString()`可以按`GMT+0:00`输出外，Date总是以当前计算机系统的默认时区为基础进行输出。此外，我们也很难对日期和时间进行加减，计算两个日期相差多少天，计算某个月第一个星期一的日期等。

#### 10.2.3 Calendar

`Calendar`可以用于获取并设置年、月、日、时、分、秒，它和`Date`比，主要多了一个可以做简单的日期和时间运算的功能。

我们来看`Calendar`的基本用法：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        // 获取当前时间:
        Calendar c = Calendar.getInstance();
        int y = c.get(Calendar.YEAR);
        int m = 1 + c.get(Calendar.MONTH);
        int d = c.get(Calendar.DAY_OF_MONTH);
        int w = c.get(Calendar.DAY_OF_WEEK);
        int hh = c.get(Calendar.HOUR_OF_DAY);
        int mm = c.get(Calendar.MINUTE);
        int ss = c.get(Calendar.SECOND);
        int ms = c.get(Calendar.MILLISECOND);
        System.out.println(y + "-" + m + "-" + d + " " + w + " " + hh + ":" + mm + ":" + ss + "." + ms);
    }
}
```

注意到`Calendar`获取年月日这些信息变成了`get(int field)`，返回的年份不必转换，返回的月份仍然要加1，返回的星期要特别注意，`1`~`7`分别表示周日，周一，……，周六。

`Calendar`只有一种方式获取，即`Calendar.getInstance()`，而且一获取到就是当前时间。如果我们想给它设置成特定的一个日期和时间，就必须先清除所有字段：

```java
import java.text.*;
import java.util.*;
public class Main {
    public static void main(String[] args) {
        // 当前时间:
        Calendar c = Calendar.getInstance();
        // 清除所有:
        c.clear();
        // 设置2019年:
        c.set(Calendar.YEAR, 2019);
        // 设置9月:注意8表示9月:
        c.set(Calendar.MONTH, 8);
        // 设置2日:
        c.set(Calendar.DATE, 2);
        // 设置时间:
        c.set(Calendar.HOUR_OF_DAY, 21);
        c.set(Calendar.MINUTE, 22);
        c.set(Calendar.SECOND, 23);
        System.out.println(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(c.getTime()));
        // 2019-09-02 21:22:23
    }
}
```

利用`Calendar.getTime()`可以将一个`Calendar`对象转换成`Date`对象，然后就可以用`SimpleDateFormat`进行格式化了。

#### 10.2.4 TimeZone

`Calendar`和`Date`相比，它提供了时区转换的功能。时区用`TimeZone`对象表示：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        TimeZone tzDefault = TimeZone.getDefault(); // 当前时区
        TimeZone tzGMT9 = TimeZone.getTimeZone("GMT+09:00"); // GMT+9:00时区
        TimeZone tzNY = TimeZone.getTimeZone("America/New_York"); // 纽约时区
        System.out.println(tzDefault.getID()); // Asia/Shanghai
        System.out.println(tzGMT9.getID()); // GMT+09:00
        System.out.println(tzNY.getID()); // America/New_York
    }
}
```

时区的唯一标识是以字符串表示的ID，我们获取指定`TimeZone`对象也是以这个ID为参数获取，`GMT+09:00`、`Asia/Shanghai`都是有效的时区ID。要列出系统支持的所有ID，请使用`TimeZone.getAvailableIDs()`。

有了时区，我们就可以对指定时间进行转换。例如，下面的例子演示了如何将北京时间`2019-11-20 8:15:00`转换为纽约时间：

```java
import java.text.*;
import java.util.*;
public class Main {
    public static void main(String[] args) {
        // 当前时间:
        Calendar c = Calendar.getInstance();
        // 清除所有:
        c.clear();
        // 设置为北京时区:
        c.setTimeZone(TimeZone.getTimeZone("Asia/Shanghai"));
        // 设置年月日时分秒:
        c.set(2019, 10 /* 11月 */, 20, 8, 15, 0);
        // 显示时间:
        var sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        sdf.setTimeZone(TimeZone.getTimeZone("America/New_York"));
        System.out.println(sdf.format(c.getTime()));
        // 2019-11-19 19:15:00
    }
}
```

可见，利用`Calendar`进行时区转换的步骤是：

1. 清除所有字段；
2. 设定指定时区；
3. 设定日期和时间；
4. 创建`SimpleDateFormat`并设定目标时区；
5. 格式化获取的`Date`对象（注意`Date`对象无时区信息，时区信息存储在`SimpleDateFormat`中）。

因此，本质上时区转换只能通过`SimpleDateFormat`在显示的时候完成。

`Calendar`也可以对日期和时间进行简单的加减：

```java
import java.text.*;
import java.util.*;
public class Main {
    public static void main(String[] args) {
        // 当前时间:
        Calendar c = Calendar.getInstance();
        // 清除所有:
        c.clear();
        // 设置年月日时分秒:
        c.set(2019, 10 /* 11月 */, 20, 8, 15, 0);
        // 加5天并减去2小时:
        c.add(Calendar.DAY_OF_MONTH, 5);
        c.add(Calendar.HOUR_OF_DAY, -2);
        // 显示时间:
        var sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date d = c.getTime();
        System.out.println(sdf.format(d));
        // 2019-11-25 6:15:00
    }
}
```

#### 10.2.5 小结

计算机表示的时间是以整数表示的时间戳存储的，即Epoch Time，Java使用`long`型来表示以毫秒为单位的时间戳，通过`System.currentTimeMillis()`获取当前时间戳。

Java有两套日期和时间的API：

- 旧的Date、Calendar和TimeZone；
- 新的LocalDateTime、ZonedDateTime、ZoneId等。

分别位于`java.util`和`java.time`包中。

### 10.3 LocalDateTime

从Java 8开始，`java.time`包提供了新的日期和时间API，主要涉及的类型有：

- 本地日期和时间：`LocalDateTime`，`LocalDate`，`LocalTime`；
- 带时区的日期和时间：`ZonedDateTime`；
- 时刻：`Instant`；
- 时区：`ZoneId`，`ZoneOffset`；
- 时间间隔：`Duration`。

以及一套新的用于取代`SimpleDateFormat`的格式化类型`DateTimeFormatter`。

和旧的API相比，新API严格区分了时刻、本地日期、本地时间和带时区的日期时间，并且，对日期和时间进行运算更加方便。

此外，新API修正了旧API不合理的常量设计：

- Month的范围用1~12表示1月到12月；
- Week的范围用1~7表示周一到周日。

最后，新API的类型几乎全部是不变类型（和String类似），可以放心使用不必担心被修改。

#### 10.3.1 LocalDateTime

我们首先来看最常用的`LocalDateTime`，它表示一个本地日期和时间：

```java
import java.time.*;
public class Main {
    public static void main(String[] args) {
        LocalDate d = LocalDate.now(); // 当前日期
        LocalTime t = LocalTime.now(); // 当前时间
        LocalDateTime dt = LocalDateTime.now(); // 当前日期和时间
        System.out.println(d); // 严格按照ISO 8601格式打印
        System.out.println(t); // 严格按照ISO 8601格式打印
        System.out.println(dt); // 严格按照ISO 8601格式打印
    }
}
```

本地日期和时间通过now()获取到的总是以当前默认时区返回的，和旧API不同，`LocalDateTime`、`LocalDate`和`LocalTime`默认严格按照[ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html)规定的日期和时间格式进行打印。

上述代码其实有一个小问题，在获取3个类型的时候，由于执行一行代码总会消耗一点时间，因此，3个类型的日期和时间很可能对不上（时间的毫秒数基本上不同）。为了保证获取到同一时刻的日期和时间，可以改写如下：

```java
LocalDateTime dt = LocalDateTime.now(); // 当前日期和时间
LocalDate d = dt.toLocalDate(); // 转换到当前日期
LocalTime t = dt.toLocalTime(); // 转换到当前时间
```

反过来，通过指定的日期和时间创建`LocalDateTime`可以通过`of()`方法：

```java
// 指定日期和时间:
LocalDate d2 = LocalDate.of(2019, 11, 30); // 2019-11-30, 注意11=11月
LocalTime t2 = LocalTime.of(15, 16, 17); // 15:16:17
LocalDateTime dt2 = LocalDateTime.of(2019, 11, 30, 15, 16, 17);
LocalDateTime dt3 = LocalDateTime.of(d2, t2);
```

因为严格按照ISO 8601的格式，因此，将字符串转换为`LocalDateTime`就可以传入标准格式：

```java
LocalDateTime dt = LocalDateTime.parse("2019-11-19T15:16:17");
LocalDate d = LocalDate.parse("2019-11-19");
LocalTime t = LocalTime.parse("15:16:17");
```

注意ISO 8601规定的日期和时间分隔符是`T`。标准格式如下：

- 日期：yyyy-MM-dd
- 时间：HH:mm:ss
- 带毫秒的时间：HH:mm:ss.SSS
- 日期和时间：yyyy-MM-dd'T'HH:mm:ss
- 带毫秒的日期和时间：yyyy-MM-dd'T'HH:mm:ss.SSS

#### 10.3.2 DateTimeFormatter

如果要自定义输出的格式，或者要把一个非ISO 8601格式的字符串解析成`LocalDateTime`，可以使用新的`DateTimeFormatter`：

```java
import java.time.*;
import java.time.format.*;
public class Main {
    public static void main(String[] args) {
        // 自定义格式化:
        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy/MM/dd HH:mm:ss");
        System.out.println(dtf.format(LocalDateTime.now()));

        // 用自定义格式解析:
        LocalDateTime dt2 = LocalDateTime.parse("2019/11/30 15:16:17", dtf);
        System.out.println(dt2);
    }
}
```

`LocalDateTime`提供了对日期和时间进行加减的非常简单的链式调用：

```java
import java.time.*;
public class Main {
    public static void main(String[] args) {
        LocalDateTime dt = LocalDateTime.of(2019, 10, 26, 20, 30, 59);
        System.out.println(dt);
        // 加5天减3小时:
        LocalDateTime dt2 = dt.plusDays(5).minusHours(3);
        System.out.println(dt2); // 2019-10-31T17:30:59
        // 减1月:
        LocalDateTime dt3 = dt2.minusMonths(1);
        System.out.println(dt3); // 2019-09-30T17:30:59
    }
}
```

注意到月份加减会自动调整日期，例如从`2019-10-31`减去1个月得到的结果是`2019-09-30`，因为9月没有31日。

对日期和时间进行调整则使用`withXxx()`方法，例如：`withHour(15)`会把`10:11:12`变为`15:11:12`：

- 调整年：withYear()
- 调整月：withMonth()
- 调整日：withDayOfMonth()
- 调整时：withHour()
- 调整分：withMinute()
- 调整秒：withSecond()

示例代码如下：

```java
import java.time.*;
public class Main {
    public static void main(String[] args) {
        LocalDateTime dt = LocalDateTime.of(2019, 10, 26, 20, 30, 59);
        System.out.println(dt);
        // 日期变为31日:
        LocalDateTime dt2 = dt.withDayOfMonth(31);
        System.out.println(dt2); // 2019-10-31T20:30:59
        // 月份变为9:
        LocalDateTime dt3 = dt2.withMonth(9);
        System.out.println(dt3); // 2019-09-30T20:30:59
    }
}
```

同样注意到调整月份时，会相应地调整日期，即把`2019-10-31`的月份调整为`9`时，日期也自动变为`30`。

实际上，`LocalDateTime`还有一个通用的`with()`方法允许我们做更复杂的运算。例如：

```java
import java.time.*;
import java.time.temporal.*;
public class Main {
    public static void main(String[] args) {
        // 本月第一天0:00时刻:
        LocalDateTime firstDay = LocalDate.now().withDayOfMonth(1).atStartOfDay();
        System.out.println(firstDay);

        // 本月最后1天:
        LocalDate lastDay = LocalDate.now().with(TemporalAdjusters.lastDayOfMonth());
        System.out.println(lastDay);

        // 下月第1天:
        LocalDate nextMonthFirstDay = LocalDate.now().with(TemporalAdjusters.firstDayOfNextMonth());
        System.out.println(nextMonthFirstDay);

        // 本月第1个周一:
        LocalDate firstWeekday = LocalDate.now().with(TemporalAdjusters.firstInMonth(DayOfWeek.MONDAY));
        System.out.println(firstWeekday);
    }
}
```

对于计算某个月第1个周日这样的问题，新的API可以轻松完成。

要判断两个`LocalDateTime`的先后，可以使用`isBefore()`、`isAfter()`方法，对于`LocalDate`和`LocalTime`类似：

```java
import java.time.*;
public class Main {
    public static void main(String[] args) {
        LocalDateTime now = LocalDateTime.now();
        LocalDateTime target = LocalDateTime.of(2019, 11, 19, 8, 15, 0);
        System.out.println(now.isBefore(target));
        System.out.println(LocalDate.now().isBefore(LocalDate.of(2019, 11, 19)));
        System.out.println(LocalTime.now().isAfter(LocalTime.parse("08:15:00")));
    }
}
```

注意到`LocalDateTime`无法与时间戳进行转换，因为`LocalDateTime`没有时区，无法确定某一时刻。后面我们要介绍的`ZonedDateTime`相当于`LocalDateTime`加时区的组合，它具有时区，可以与`long`表示的时间戳进行转换。

#### 10.3.3 Duration和Period

`Duration`表示两个时刻之间的时间间隔。另一个类似的`Period`表示两个日期之间的天数：

```java
import java.time.*;
public class Main {
    public static void main(String[] args) {
        LocalDateTime start = LocalDateTime.of(2019, 11, 19, 8, 15, 0);
        LocalDateTime end = LocalDateTime.of(2020, 1, 9, 19, 25, 30);
        Duration d = Duration.between(start, end);
        System.out.println(d); // PT1235H10M30S

        Period p = LocalDate.of(2019, 11, 19).until(LocalDate.of(2020, 1, 9));
        System.out.println(p); // P1M21D
    }
}
```

注意到两个`LocalDateTime`之间的差值使用`Duration`表示，类似`PT1235H10M30S`，表示1235小时10分钟30秒。而两个`LocalDate`之间的差值用`Period`表示，类似`P1M21D`，表示1个月21天。

`Duration`和`Period`的表示方法也符合ISO 8601的格式，它以`P...T...`的形式表示，`P...T`之间表示日期间隔，`T`后面表示时间间隔。如果是`PT...`的格式表示仅有时间间隔。利用`ofXxx()`或者`parse()`方法也可以直接创建`Duration`：

```java
Duration d1 = Duration.ofHours(10); // 10 hours
Duration d2 = Duration.parse("P1DT2H3M"); // 1 day, 2 hours, 3 minutes
```

有的童鞋可能发现Java 8引入的`java.time`API。怎么和一个开源的[Joda Time](https://www.joda.org/)很像？难道JDK也开始抄袭开源了？其实正是因为开源的Joda Time设计很好，应用广泛，所以JDK团队邀请Joda Time的作者Stephen Colebourne共同设计了`java.time`API。

#### 10.3.4 小结

Java 8引入了新的日期和时间API，它们是不变类，默认按ISO 8601标准格式化和解析；

使用`LocalDateTime`可以非常方便地对日期和时间进行加减，或者调整日期和时间，它总是返回新对象；

使用`isBefore()`和`isAfter()`可以判断日期和时间的先后；

使用`Duration`和`Period`可以表示两个日期和时间的“区间间隔”。

### 10.4 ZonedDateTime

`LocalDateTime`总是表示本地日期和时间，要表示一个带时区的日期和时间，我们就需要`ZonedDateTime`。

可以简单地把`ZonedDateTime`理解成`LocalDateTime`加`ZoneId`。`ZoneId`是`java.time`引入的新的时区类，注意和旧的`java.util.TimeZone`区别。

要创建一个`ZonedDateTime`对象，有以下几种方法，一种是通过`now()`方法返回当前时间：

```java
import java.time.*;
public class Main {
    public static void main(String[] args) {
        ZonedDateTime zbj = ZonedDateTime.now(); // 默认时区
        ZonedDateTime zny = ZonedDateTime.now(ZoneId.of("America/New_York")); // 用指定时区获取当前时间
        System.out.println(zbj);
        System.out.println(zny);
    }
}
```

观察打印的两个`ZonedDateTime`，发现它们时区不同，但表示的时间都是同一时刻（毫秒数不同是执行语句时的时间差）：

```shell
2019-09-15T20:58:18.786182+08:00[Asia/Shanghai]
2019-09-15T08:58:18.788860-04:00[America/New_York]
```

另一种方式是通过给一个`LocalDateTime`附加一个`ZoneId`，就可以变成`ZonedDateTime`：

```java
import java.time.*;
public class Main {
    public static void main(String[] args) {
        LocalDateTime ldt = LocalDateTime.of(2019, 9, 15, 15, 16, 17);
        ZonedDateTime zbj = ldt.atZone(ZoneId.systemDefault());
        ZonedDateTime zny = ldt.atZone(ZoneId.of("America/New_York"));
        System.out.println(zbj);
        System.out.println(zny);
    }
}
```

以这种方式创建的`ZonedDateTime`，它的日期和时间与`LocalDateTime`相同，但附加的时区不同，因此是两个不同的时刻：

```java
2019-09-15T15:16:17+08:00[Asia/Shanghai]
2019-09-15T15:16:17-04:00[America/New_York]
```

#### 10.4.1 时区转换

要转换时区，首先我们需要有一个`ZonedDateTime`对象，然后，通过`withZoneSameInstant()`将关联时区转换到另一个时区，转换后日期和时间都会相应调整。

下面的代码演示了如何将北京时间转换为纽约时间：

```java
import java.time.*;
public class Main {
    public static void main(String[] args) {
        // 以中国时区获取当前时间:
        ZonedDateTime zbj = ZonedDateTime.now(ZoneId.of("Asia/Shanghai"));
        // 转换为纽约时间:
        ZonedDateTime zny = zbj.withZoneSameInstant(ZoneId.of("America/New_York"));
        System.out.println(zbj);
        System.out.println(zny);
    }
}
```

要特别注意，时区转换的时候，由于夏令时的存在，不同的日期转换的结果很可能是不同的。这是北京时间9月15日的转换结果：

```shell
2019-09-15T21:05:50.187697+08:00[Asia/Shanghai]
2019-09-15T09:05:50.187697-04:00[America/New_York]
```

这是北京时间11月15日的转换结果：

```shell
2019-11-15T21:05:50.187697+08:00[Asia/Shanghai]
2019-11-15T08:05:50.187697-05:00[America/New_York]
```

两次转换后的纽约时间有1小时的夏令时时差。

 **涉及到时区时，千万不要自己计算时差，否则难以正确处理夏令时。**

有了`ZonedDateTime`，将其转换为本地时间就非常简单：

```java
ZonedDateTime zdt = ...
LocalDateTime ldt = zdt.toLocalDateTime();
```

转换为`LocalDateTime`时，直接丢弃了时区信息。

#### 10.4.2 小结

`ZonedDateTime`是带时区的日期和时间，可用于时区转换；

`ZonedDateTime`和`LocalDateTime`可以相互转换。

### 10.5 DateTimeFormatter

使用旧的`Date`对象时，我们用`SimpleDateFormat`进行格式化显示。使用新的`LocalDateTime`或`ZonedLocalDateTime`时，我们要进行格式化显示，就要使用`DateTimeFormatter`。

和`SimpleDateFormat`不同的是，`DateTimeFormatter`不但是不变对象，它还是线程安全的。线程的概念我们会在后面涉及到。现在我们只需要记住：因为`SimpleDateFormat`不是线程安全的，使用的时候，只能在方法内部创建新的局部变量。而`DateTimeFormatter`可以只创建一个实例，到处引用。

创建`DateTimeFormatter`时，我们仍然通过传入格式化字符串实现：

```java
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
```

格式化字符串的使用方式与`SimpleDateFormat`完全一致。

另一种创建`DateTimeFormatter`的方法是，传入格式化字符串时，同时指定`Locale`：

```java
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("E, yyyy-MMMM-dd HH:mm", Locale.US);
```

这种方式可以按照`Locale`默认习惯格式化。我们来看实际效果：

```java
import java.time.*;
import java.time.format.*;
import java.util.Locale;
public class Main {
    public static void main(String[] args) {
        ZonedDateTime zdt = ZonedDateTime.now();
        var formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm ZZZZ");
        System.out.println(formatter.format(zdt));

        var zhFormatter = DateTimeFormatter.ofPattern("yyyy MMM dd EE HH:mm", Locale.CHINA);
        System.out.println(zhFormatter.format(zdt));

        var usFormatter = DateTimeFormatter.ofPattern("E, MMMM/dd/yyyy HH:mm", Locale.US);
        System.out.println(usFormatter.format(zdt));
    }
}
```

在格式化字符串中，如果需要输出固定字符，可以用`'xxx'`表示。

运行上述代码，分别以默认方式、中国地区和美国地区对当前时间进行显示，结果如下：

```shell
2019-09-15T23:16 GMT+08:00
2019 9月 15 周日 23:16
Sun, September/15/2019 23:16
```

当我们直接调用`System.out.println()`对一个`ZonedDateTime`或者`LocalDateTime`实例进行打印的时候，实际上，调用的是它们的`toString()`方法，默认的`toString()`方法显示的字符串就是按照`ISO 8601`格式显示的，我们可以通过`DateTimeFormatter`预定义的几个静态变量来引用：

```java
var ldt = LocalDateTime.now();
System.out.println(DateTimeFormatter.ISO_DATE.format(ldt));
System.out.println(DateTimeFormatter.ISO_DATE_TIME.format(ldt));
```

得到的输出和`toString()`类似：

```shell
2019-09-15
2019-09-15T23:16:51.56217
```

#### 10.5.1 小结

对`ZonedDateTime`或`LocalDateTime`进行格式化，需要使用`DateTimeFormatter`类；

`DateTimeFormatter`可以通过格式化字符串和`Locale`对日期和时间进行定制输出。

### 10.6 Instant

计算机存储的当前时间，本质上只是一个不断递增的整数。Java提供的`System.currentTimeMillis()`返回的就是以毫秒表示的当前时间戳。

这个当前时间戳在`java.time`中以`Instant`类型表示，我们用`Instant.now()`获取当前时间戳，效果和`System.currentTimeMillis()`类似：

```java
import java.time.*;
public class Main {
    public static void main(String[] args) {
        Instant now = Instant.now();
        System.out.println(now.getEpochSecond()); // 秒
        System.out.println(now.toEpochMilli()); // 毫秒
    }
}
```

打印的结果类似：

```java
1568568760
1568568760316
```

实际上，`Instant`内部只有两个核心字段：

```java
public final class Instant implements ... {
    private final long seconds;
    private final int nanos;
}
```

一个是以秒为单位的时间戳，一个是更精确的纳秒精度。它和`System.currentTimeMillis()`返回的`long`相比，只是多了更高精度的纳秒。既然`Instant`就是时间戳，那么，给它附加上一个时区，就可以创建出`ZonedDateTime`：

```java
// 以指定时间戳创建Instant:
Instant ins = Instant.ofEpochSecond(1568568760);
ZonedDateTime zdt = ins.atZone(ZoneId.systemDefault());
System.out.println(zdt); // 2019-09-16T01:32:40+08:00[Asia/Shanghai]
```

可见，对于某一个时间戳，给它关联上指定的`ZoneId`，就得到了`ZonedDateTime`，继而可以获得了对应时区的`LocalDateTime`。所以，`LocalDateTime`，`ZoneId`，`Instant`，`ZonedDateTime`和`long`都可以互相转换：

```ascii
┌─────────────┐
│LocalDateTime│────┐
└─────────────┘    │    ┌─────────────┐
                   ├───>│ZonedDateTime│
┌─────────────┐    │    └─────────────┘
│   ZoneId    │────┘           ▲
└─────────────┘      ┌─────────┴─────────┐
                     │                   │
                     ▼                   ▼
              ┌─────────────┐     ┌─────────────┐
              │   Instant   │<───>│    long     │
              └─────────────┘     └─────────────┘
```

转换的时候，只需要留意`long`类型以毫秒还是秒为单位即可。

#### 10.6.1小结

`Instant`表示高精度时间戳，它可以和`ZonedDateTime`以及`long`互相转换。

### 10.7 最佳实践

由于Java提供了新旧两套日期和时间的API，除非涉及到遗留代码，否则我们应该坚持使用新的API。

如果需要与遗留代码打交道，如何在新旧API之间互相转换呢？

#### 10.7.1 旧API转新API

如果要把旧式的`Date`或`Calendar`转换为新API对象，可以通过`toInstant()`方法转换为`Instant`对象，再继续转换为`ZonedDateTime`：

```java
// Date -> Instant:
Instant ins1 = new Date().toInstant();

// Calendar -> Instant -> ZonedDateTime:
Calendar calendar = Calendar.getInstance();
Instant ins2 = Calendar.getInstance().toInstant();
ZonedDateTime zdt = ins2.atZone(calendar.getTimeZone().toZoneId());
```

从上面的代码还可以看到，旧的`TimeZone`提供了一个`toZoneId()`，可以把自己变成新的`ZoneId`。

#### 10.7.2 新API转旧API

如果要把新的`ZonedDateTime`转换为旧的API对象，只能借助`long`型时间戳做一个“中转”：

```java
// ZonedDateTime -> long:
ZonedDateTime zdt = ZonedDateTime.now();
long ts = zdt.toEpochSecond() * 1000;

// long -> Date:
Date date = new Date(ts);

// long -> Calendar:
Calendar calendar = Calendar.getInstance();
calendar.clear();
calendar.setTimeZone(TimeZone.getTimeZone(zdt.getZone().getId()));
calendar.setTimeInMillis(zdt.toEpochSecond() * 1000);
```

从上面的代码还可以看到，新的`ZoneId`转换为旧的`TimeZone`，需要借助`ZoneId.getId()`返回的`String`完成。

#### 10.7.3 在数据库中存储日期和时间

除了旧式的`java.util.Date`，我们还可以找到另一个`java.sql.Date`，它继承自`java.util.Date`，但会自动忽略所有时间相关信息。这个奇葩的设计原因要追溯到数据库的日期与时间类型。

在数据库中，也存在几种日期和时间类型：

- `DATETIME`：表示日期和时间；
- `DATE`：仅表示日期；
- `TIME`：仅表示时间；
- `TIMESTAMP`：和`DATETIME`类似，但是数据库会在创建或者更新记录的时候同时修改`TIMESTAMP`。

在使用Java程序操作数据库时，我们需要把数据库类型与Java类型映射起来。下表是数据库类型与Java新旧API的映射关系：

| 数据库    | 对应Java类（旧）   | 对应Java类（新） |
| :-------- | :----------------- | :--------------- |
| DATETIME  | java.util.Date     | LocalDateTime    |
| DATE      | java.sql.Date      | LocalDate        |
| TIME      | java.sql.Time      | LocalTime        |
| TIMESTAMP | java.sql.Timestamp | LocalDateTime    |

实际上，在数据库中，我们需要存储的最常用的是时刻（`Instant`），因为有了时刻信息，就可以根据用户自己选择的时区，显示出正确的本地时间。所以，最好的方法是直接用长整数`long`表示，在数据库中存储为`BIGINT`类型。

通过存储一个`long`型时间戳，我们可以编写一个`timestampToString()`的方法，非常简单地为不同用户以不同的偏好来显示不同的本地时间：

```java
import java.time.*;
import java.time.format.*;
import java.util.Locale;
public class Main {
    public static void main(String[] args) {
        long ts = 1574208900000L;
        System.out.println(timestampToString(ts, Locale.CHINA, "Asia/Shanghai"));
        System.out.println(timestampToString(ts, Locale.US, "America/New_York"));
    }

    static String timestampToString(long epochMilli, Locale lo, String zoneId) {
        Instant ins = Instant.ofEpochMilli(epochMilli);
        DateTimeFormatter f = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM, FormatStyle.SHORT);
        return f.withLocale(lo).format(ZonedDateTime.ofInstant(ins, ZoneId.of(zoneId)));
    }
}
```

对上述方法进行调用，结果如下：

```java
2019年11月20日 上午8:15
Nov 19, 2019, 7:15 PM
```

#### 10.7.4 小结

处理日期和时间时，尽量使用新的`java.time`包；

在数据库中存储时间戳时，尽量使用`long`型时间戳，它具有省空间，效率高，不依赖数据库的优点。

## 11 单元测试

### 11.1 编写JUnit测试

什么是单元测试呢？单元测试就是针对最小的功能单元编写测试代码。Java程序最小的功能单元是方法，因此，对Java程序进行单元测试就是针对单个Java方法的测试。

单元测试有什么好处呢？在学习单元测试前，我们可以先了解一下测试驱动开发。

所谓测试驱动开发，是指先编写接口，紧接着编写测试。编写完测试后，我们才开始真正编写实现代码。在编写实现代码的过程中，一边写，一边测，什么时候测试全部通过了，那就表示编写的实现完成了：

```ascii
    编写接口
     │
     ▼
    编写测试
     │
     ▼
┌─> 编写实现
│    │
│ N  ▼
└── 运行测试
     │ Y
     ▼
    任务完成
```

当然，这是一种理想情况。大部分情况是我们已经编写了实现代码，需要对已有的代码进行测试。

我们先通过一个示例来看如何编写测试。假定我们编写了一个计算阶乘的类，它只有一个静态方法来计算阶乘：
$$
n!=1\times2\times3\times...\times n*n*!=1×2×3×...×*n*
$$
代码如下：

```java
public class Factorial {
    public static long fact(long n) {
        long r = 1;
        for (long i = 1; i <= n; i++) {
            r = r * i;
        }
        return r;
    }
}
```

要测试这个方法，一个很自然的想法是编写一个`main()`方法，然后运行一些测试代码：

```java
public class Test {
    public static void main(String[] args) {
        if (fact(10) == 3628800) {
            System.out.println("pass");
        } else {
            System.out.println("fail");
        }
    }
}
```

这样我们就可以通过运行`main()`方法来运行测试代码。

不过，使用`main()`方法测试有很多缺点：

一是只能有一个`main()`方法，不能把测试代码分离，二是没有打印出测试结果和期望结果，例如，`expected: 3628800, but actual: 123456`，三是很难编写一组通用的测试代码。

因此，我们需要一种测试框架，帮助我们编写测试。

#### 11.1.1 JUnit

JUnit是一个开源的Java语言的单元测试框架，专门针对Java设计，使用最广泛。JUnit是事实上的单元测试的标准框架，任何Java开发者都应当学习并使用JUnit编写单元测试。

使用JUnit编写单元测试的好处在于，我们可以非常简单地组织测试代码，并随时运行它们，JUnit就会给出成功的测试和失败的测试，还可以生成测试报告，不仅包含测试的成功率，还可以统计测试的代码覆盖率，即被测试的代码本身有多少经过了测试。对于高质量的代码来说，测试覆盖率应该在80%以上。

此外，几乎所有的IDE工具都集成了JUnit，这样我们就可以直接在IDE中编写并运行JUnit测试。JUnit目前最新版本是5。

以Eclipse为例，当我们已经编写了一个`Factorial.java`文件后，我们想对其进行测试，需要编写一个对应的`FactorialTest.java`文件，以`Test`为后缀是一个惯例，并分别将其放入`src`和`test`目录中。最后，在`Project` - `Properties` - `Java Build Path` - `Libraries`中添加`JUnit 5`的库：

我们来看一下`FactorialTest.java`的内容：

```java
package com.itranswarp.learnjava;

import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class FactorialTest {

    @Test
    void testFact() {
        assertEquals(1, Factorial.fact(1));
        assertEquals(2, Factorial.fact(2));
        assertEquals(6, Factorial.fact(3));
        assertEquals(3628800, Factorial.fact(10));
        assertEquals(2432902008176640000L, Factorial.fact(20));
    }
}
```

核心测试方法`testFact()`加上了`@Test`注解，这是JUnit要求的，它会把带有`@Test`的方法识别为测试方法。在测试方法内部，我们用`assertEquals(1, Factorial.fact(1))`表示，期望`Factorial.fact(1)`返回`1`。`assertEquals(expected, actual)`是最常用的测试方法，它在`Assertion`类中定义。`Assertion`还定义了其他断言方法，例如：

- `assertTrue()`: 期待结果为`true`
- `assertFalse()`: 期待结果为`false`
- `assertNotNull()`: 期待结果为非`null`
- `assertArrayEquals()`: 期待结果为数组并与期望数组每个元素的值均相等
- ...

运行单元测试非常简单。选中`Factorial.java`文件，点击`Run` - `Run As` - `JUnit Test`，Eclipse会自动运行这个JUnit测试，并显示结果，如果测试结果与预期不符，`assertEquals()`会抛出异常，我们就会得到一个测试失败的结果。

在Failure Trace中，JUnit会告诉我们详细的错误结果：

```java
org.opentest4j.AssertionFailedError: expected: <3628800> but was: <362880>
   at org.junit.jupiter.api.AssertionUtils.fail(AssertionUtils.java:55)
   at org.junit.jupiter.api.AssertEquals.failNotEqual(AssertEquals.java:195)
   at org.junit.jupiter.api.AssertEquals.assertEquals(AssertEquals.java:168)
   at org.junit.jupiter.api.AssertEquals.assertEquals(AssertEquals.java:163)
   at org.junit.jupiter.api.Assertions.assertEquals(Assertions.java:611)
   at com.itranswarp.learnjava.FactorialTest.testFact(FactorialTest.java:14)
   at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
   at ...
```

第一行的失败信息的意思是期待结果`3628800`但是实际返回是`362880`，此时，我们要么修正实现代码，要么修正测试代码，直到测试通过为止。

使用浮点数时，由于浮点数无法精确地进行比较，因此，我们需要调用`assertEquals(double expected, double actual, double delta)`这个重载方法，指定一个误差值：

```java
assertEquals(0.1, Math.abs(1 - 9 / 10.0), 0.0000001);
```

#### 11.1.2 单元测试的好处

单元测试可以确保单个方法按照正确预期运行，如果修改了某个方法的代码，只需确保其对应的单元测试通过，即可认为改动正确。此外，测试代码本身就可以作为示例代码，用来演示如何调用该方法。

使用JUnit进行单元测试，我们可以使用断言（`Assertion`）来测试期望结果，可以方便地组织和运行测试，并方便地查看测试结果。此外，JUnit既可以直接在IDE中运行，也可以方便地集成到Maven这些自动化工具中运行。

在编写单元测试的时候，我们要遵循一定的规范：

- 一是单元测试代码本身必须非常简单，能一下看明白，决不能再为测试代码编写测试；
- 二是每个单元测试应当互相独立，不依赖运行的顺序；
- 三是测试时不但要覆盖常用测试用例，还要特别注意测试边界条件，例如输入为`0`，`null`，空字符串`""`等情况。

#### 11.1.3 小结

JUnit是一个单元测试框架，专门用于运行我们编写的单元测试：

一个JUnit测试包含若干`@Test`方法，并使用`Assertions`进行断言，注意浮点数`assertEquals()`要指定`delta`。

### 11.2 使用Fixture

在一个单元测试中，我们经常编写多个`@Test`方法，来分组、分类对目标代码进行测试。

在测试的时候，我们经常遇到一个对象需要初始化，测试完可能还需要清理的情况。如果每个`@Test`方法都写一遍这样的重复代码，显然比较麻烦。

JUnit提供了编写测试前准备、测试后清理的固定代码，我们称之为**Fixture**。

我们来看一个具体的`Calculator`的例子：

```java
public class Calculator {
    private long n = 0;

    public long add(long x) {
        n = n + x;
        return n;
    }

    public long sub(long x) {
        n = n - x;
        return n;
    }
}
```

这个类的功能很简单，但是测试的时候，我们要先初始化对象，我们不必在每个测试方法中都写上初始化代码，而是通过`@BeforeEach`来初始化，通过`@AfterEach`来清理资源：

```java
public class CalculatorTest {

    Calculator calculator;

    @BeforeEach
    public void setUp() {
        this.calculator = new Calculator();
    }

    @AfterEach
    public void tearDown() {
        this.calculator = null;
    }

    @Test
    void testAdd() {
        assertEquals(100, this.calculator.add(100));
        assertEquals(150, this.calculator.add(50));
        assertEquals(130, this.calculator.add(-20));
    }

    @Test
    void testSub() {
        assertEquals(-100, this.calculator.sub(100));
        assertEquals(-150, this.calculator.sub(50));
        assertEquals(-130, this.calculator.sub(-20));
    }
}
```

在`CalculatorTest`测试中，有两个标记为`@BeforeEach`和`@AfterEach`的方法，它们会在运行每个`@Test`方法前后自动运行。

上面的测试代码在JUnit中运行顺序如下：

```java
for (Method testMethod : findTestMethods(CalculatorTest.class)) {
    var test = new CalculatorTest(); // 创建Test实例
    invokeBeforeEach(test);
        invokeTestMethod(test, testMethod);
    invokeAfterEach(test);
}
```

可见，`@BeforeEach`和`@AfterEach`会“环绕”在每个`@Test`方法前后。

还有一些资源初始化和清理可能更加繁琐，而且会耗费较长的时间，例如初始化数据库。JUnit还提供了`@BeforeAll`和`@AfterAll`，它们在运行所有`@Test`前后运行，顺序如下：

```java
invokeBeforeAll(CalculatorTest.class);
for (Method testMethod : findTestMethods(CalculatorTest.class)) {
    var test = new CalculatorTest(); // 创建Test实例
    invokeBeforeEach(test);
        invokeTestMethod(test, testMethod);
    invokeAfterEach(test);
}
invokeAfterAll(CalculatorTest.class);
```

因为`@BeforeAll`和`@AfterAll`在所有`@Test`方法运行前后仅运行一次，因此，它们只能初始化静态变量，例如：

```java
public class DatabaseTest {
    static Database db;

    @BeforeAll
    public static void initDatabase() {
        db = createDb(...);
    }

    @AfterAll
    public static void dropDatabase() {
        ...
    }
}
```

事实上，`@BeforeAll`和`@AfterAll`也只能标注在静态方法上。

因此，我们总结出编写Fixture的套路如下：

1. 对于实例变量，在`@BeforeEach`中初始化，在`@AfterEach`中清理，它们在各个`@Test`方法中互不影响，因为是不同的实例；
2. 对于静态变量，在`@BeforeAll`中初始化，在`@AfterAll`中清理，它们在各个`@Test`方法中均是唯一实例，会影响各个`@Test`方法。

大多数情况下，使用`@BeforeEach`和`@AfterEach`就足够了。**只有某些测试资源初始化耗费时间太长，以至于我们不得不尽量“复用”时才会用到`@BeforeAll`和`@AfterAll`**。

最后，注意到每次运行一个`@Test`方法前，JUnit首先创建一个`XxxTest`实例，因此，每个`@Test`方法内部的成员变量都是独立的，不能也无法把成员变量的状态从一个`@Test`方法带到另一个`@Test`方法。

#### 11.2.1 小结

编写Fixture是指针对每个`@Test`方法，编写`@BeforeEach`方法用于初始化测试资源，编写`@AfterEach`用于清理测试资源；必要时，可以编写`@BeforeAll`和`@AfterAll`，使用静态变量来初始化耗时的资源，并且在所有`@Test`方法的运行前后仅执行一次。

### 11.3 异常测试

在Java程序中，异常处理是非常重要的。我们自己编写的方法，也经常抛出各种异常。对于可能抛出的异常进行测试，本身就是测试的重要环节。因此，在编写JUnit测试的时候，除了正常的输入输出，我们还要特别针对可能导致异常的情况进行测试。

我们仍然用`Factorial`举例：

```java
public class Factorial {
    public static long fact(long n) {
        if (n < 0) {
            throw new IllegalArgumentException();
        }
        long r = 1;
        for (long i = 1; i <= n; i++) {
            r = r * i;
        }
        return r;
    }
}
```

在方法入口，我们增加了对参数`n`的检查，如果为负数，则直接抛出`IllegalArgumentException`。现在，我们希望对异常进行测试。在JUnit测试中，我们可以编写一个`@Test`方法专门测试异常：

```java
@Test
void testNegative() {
    assertThrows(IllegalArgumentException.class, new Executable() {
        @Override
        public void execute() throws Throwable {
            Factorial.fact(-1);
        }
    });
}
```

JUnit提供`assertThrows()`来期望捕获一个指定的异常。第二个参数`Executable`封装了我们要执行的会产生异常的代码。当我们执行`Factorial.fact(-1)`时，必定抛出`IllegalArgumentException`。`assertThrows()`在**捕获到指定异常时表示通过测试，未捕获到异常，或者捕获到的异常类型不对，均表示测试失败**。

有些童鞋会觉得编写一个`Executable`的匿名类太繁琐了。实际上，Java 8开始引入了函数式编程，所有单方法接口都可以简写如下：

```java
@Test
void testNegative() {
    assertThrows(IllegalArgumentException.class, () -> {
        Factorial.fact(-1);
    });
}
```

上述奇怪的`->`语法就是函数式接口的实现代码，我们会在后面详细介绍。现在，我们只需要通过这种固定的代码编写能抛出异常的语句即可。

#### 11.3.1 小结

测试异常可以使用`assertThrows()`，期待捕获到指定类型的异常；

对可能发生的每种类型的异常都必须进行测试。

### 11.4 条件测试

在运行测试的时候，有些时候，我们需要排除某些`@Test`方法，不要让它运行，这时，我们就可以给它标记一个`@Disabled`：

```java
@Disabled
@Test
void testBug101() {
    // 这个测试不会运行
}
```

为什么我们不直接注释掉`@Test`，而是要加一个`@Disabled`？这是因为注释掉`@Test`，JUnit就不知道这是个测试方法，而加上`@Disabled`，JUnit仍然识别出这是个测试方法，只是暂时不运行。它会在测试结果中显示：

```java
Tests run: 68, Failures: 2, Errors: 0, Skipped: 5
```

类似`@Disabled`这种注解就称为**条件测试**，JUnit根据不同的条件注解，决定是否运行当前的`@Test`方法。

我们来看一个例子：

```java
public class Config {
    public String getConfigFile(String filename) {
        String os = System.getProperty("os.name").toLowerCase();
        if (os.contains("win")) {
            return "C:\\" + filename;
        }
        if (os.contains("mac") || os.contains("linux") || os.contains("unix")) {
            return "/usr/local/" + filename;
        }
        throw new UnsupportedOperationException();
    }
}
```

我们想要测试`getConfigFile()`这个方法，但是在Windows上跑，和在Linux上跑的代码路径不同，因此，针对两个系统的测试方法，其中一个只能在Windows上跑，另一个只能在Mac/Linux上跑：

```java
@Test
void testWindows() {
    assertEquals("C:\\test.ini", config.getConfigFile("test.ini"));
}

@Test
void testLinuxAndMac() {
    assertEquals("/usr/local/test.cfg", config.getConfigFile("test.cfg"));
}
```

因此，我们给上述两个测试方法分别加上条件如下：

```java
@Test
@EnabledOnOs(OS.WINDOWS)
void testWindows() {
    assertEquals("C:\\test.ini", config.getConfigFile("test.ini"));
}

@Test
@EnabledOnOs({ OS.LINUX, OS.MAC })
void testLinuxAndMac() {
    assertEquals("/usr/local/test.cfg", config.getConfigFile("test.cfg"));
}
```

`@EnableOnOs`就是一个条件测试判断。

我们来看一些常用的条件测试：

不在Windows平台执行的测试，可以加上`@DisabledOnOs(OS.WINDOWS)`：

```java
@Test
@DisabledOnOs(OS.WINDOWS)
void testOnNonWindowsOs() {
    // TODO: this test is disabled on windows
}
```

只能在Java 9或更高版本执行的测试，可以加上`@DisabledOnJre(JRE.JAVA_8)`：

```java
@Test
@DisabledOnJre(JRE.JAVA_8)
void testOnJava9OrAbove() {
    // TODO: this test is disabled on java 8
}
```

只能在64位操作系统上执行的测试，可以用`@EnabledIfSystemProperty`判断：

```java
@Test
@EnabledIfSystemProperty(named = "os.arch", matches = ".*64.*")
void testOnlyOn64bitSystem() {
    // TODO: this test is only run on 64 bit system
}
```

需要传入环境变量`DEBUG=true`才能执行的测试，可以用`@EnabledIfEnvironmentVariable`：

```java
@Test
@EnabledIfEnvironmentVariable(named = "DEBUG", matches = "true")
void testOnlyOnDebugMode() {
    // TODO: this test is only run on DEBUG=true
}
```

最后，万能的`@EnableIf`可以执行任意Java语句并根据返回的`boolean`决定是否执行测试。下面的代码演示了一个只能在星期日执行的测试：

```java
@Test
@EnabledIf("java.time.LocalDate.now().getDayOfWeek()==java.time.DayOfWeek.SUNDAY")
void testOnlyOnSunday() {
    // TODO: this test is only run on Sunday
}
```

当我们在JUnit中运行所有测试的时候，JUnit会给出执行的结果。在IDE中，我们能很容易地看到没有执行的测试，带有`⊘`标记的测试方法表示没有执行。

#### 11.4.1 小结

条件测试是根据某些注解在运行期让JUnit自动忽略某些测试。

### 11.5 参数化测试

如果待测试的输入和输出是一组数据： 可以把测试数据组织起来，用不同的测试数据调用相同的测试方法。参数化测试和普通测试稍微不同的地方在于，**一个测试方法需要接收至少一个参数，然后，传入一组参数反复运行**。

JUnit提供了一个`@ParameterizedTest`注解，用来进行参数化测试。

假设我们想对`Math.abs()`进行测试，先用一组正数进行测试：

```java
@ParameterizedTest
@ValueSource(ints = { 0, 1, 5, 100 })
void testAbs(int x) {
    assertEquals(x, Math.abs(x));
}
```

再用一组负数进行测试：

```java
@ParameterizedTest
@ValueSource(ints = { -1, -5, -100 })
void testAbsNegative(int x) {
    assertEquals(-x, Math.abs(x));
}
```

注意到参数化测试的注解是`@ParameterizedTest`，而不是普通的`@Test`。

实际的测试场景往往没有这么简单。假设我们自己编写了一个`StringUtils.capitalize()`方法，它会把字符串的第一个字母变为大写，后续字母变为小写：

```java
public class StringUtils {
    public static String capitalize(String s) {
        if (s.length() == 0) {
            return s;
        }
        return Character.toUpperCase(s.charAt(0)) + s.substring(1).toLowerCase();
    }
}
```

要用参数化测试的方法来测试，我们不但要给出输入，还要给出预期输出。因此，测试方法至少需要接收两个参数：

```java
@ParameterizedTest
void testCapitalize(String input, String result) {
    assertEquals(result, StringUtils.capitalize(input));
}
```

现在问题来了：参数如何传入？

最简单的方法是通过`@MethodSource`注解，它允许我们编写一个同名的静态方法来提供测试参数：

```java
@ParameterizedTest
@MethodSource
void testCapitalize(String input, String result) {
    assertEquals(result, StringUtils.capitalize(input));
}

static List<Arguments> testCapitalize() {
    return List.of( // arguments:
            Arguments.arguments("abc", "Abc"), //
            Arguments.arguments("APPLE", "Apple"), //
            Arguments.arguments("gooD", "Good"));
}
```

上面的代码很容易理解：静态方法`testCapitalize()`返回了一组测试参数，每个参数都包含两个`String`，正好作为测试方法的两个参数传入。

 如果静态方法和测试方法的名称不同，@MethodSource也允许指定方法名。但使用默认同名方法最方便。

另一种传入测试参数的方法是使用`@CsvSource`，它的每一个字符串表示一行，一行包含的若干参数用`,`分隔，因此，上述测试又可以改写如下：

```java
@ParameterizedTest
@CsvSource({ "abc, Abc", "APPLE, Apple", "gooD, Good" })
void testCapitalize(String input, String result) {
    assertEquals(result, StringUtils.capitalize(input));
}
```

如果有成百上千的测试输入，那么，直接写`@CsvSource`就很不方便。这个时候，我们可以把测试数据提到一个独立的CSV文件中，然后标注上`@CsvFileSource`：

```java
@ParameterizedTest
@CsvFileSource(resources = { "/test-capitalize.csv" })
void testCapitalizeUsingCsvFile(String input, String result) {
    assertEquals(result, StringUtils.capitalize(input));
}
```

JUnit只在classpath中查找指定的CSV文件，因此，`test-capitalize.csv`这个文件要放到`test`目录下，内容如下：

```java
apple, Apple
HELLO, Hello
JUnit, Junit
reSource, Resource
```

#### 11.5.1 小结

使用参数化测试，可以提供一组测试数据，对一个测试方法反复测试。

参数既可以在测试代码中写死，也可以通过`@CsvFileSource`放到外部的CSV文件中。

## 12 正则表达式

### 12.1 正则表达式简介

在了解正则表达式之前，我们先看几个非常常见的问题：

- 如何判断字符串是否是有效的电话号码？例如：`010-1234567`，`123ABC456`，`13510001000`等；
- 如何判断字符串是否是有效的电子邮件地址？例如：`test@example.com`，`test#example`等；
- 如何判断字符串是否是有效的时间？例如：`12:34`，`09:60`，`99:99`等。

一种直观的想法是通过程序判断，这种方法需要为每种用例创建规则，然后用代码实现。下面是判断手机号的代码：

```java
boolean isValidMobileNumber(String s) {
    // 是否是11位？
    if (s.length() != 11) {
        return false;
    }
    // 每一位都是0~9：
    for (int i=0; i<s.length(); i++) {
        char c = s.charAt(i);
        if (c < '0' || c > '9') {
            return false;
        }
    }
    return true;
}
```

上述代码仅仅做了非常粗略的判断，并未考虑首位数字不能为`0`等更详细的情况。

除了判断手机号，我们还需要判断电子邮件地址、电话、邮编等等：

- boolean isValidMobileNumber(String s) { ... }
- boolean isValidEmail(String s) { ... }
- boolean isValidPhoneNumber(String s) { ... }
- boolean isValidZipCode(String s) { ... }
- ...

为每一种判断逻辑编写代码实在是太繁琐了。有没有更简单的方法？

有！用正则表达式！

正则表达式可以用字符串来描述规则，并用来匹配字符串。例如，判断手机号，我们用正则表达式`\d{11}`：

```java
boolean isValidMobileNumber(String s) {
    return s.matches("\\d{11}");
}
```

使用正则表达式的好处有哪些？一个正则表达式就是一个描述规则的字符串，所以，只需要编写正确的规则，我们就可以让正则表达式引擎去判断目标字符串是否符合规则。

正则表达式是一套标准，它可以用于任何语言。Java标准库的`java.util.regex`包内置了正则表达式引擎，在Java程序中使用正则表达式非常简单。

举个例子：要判断用户输入的年份是否是`20##`年，我们先写出规则如下：

一共有4个字符，分别是：`2`，`0`，`0~9任意数字`，`0~9任意数字`。

对应的正则表达式就是：`20\d\d`，其中`\d`表示任意一个数字。

把正则表达式转换为Java字符串就变成了`20\\d\\d`，注意Java字符串用`\\`表示`\`。

最后，用正则表达式匹配一个字符串的代码如下：

```java
public class Main {
    public static void main(String[] args) {
        String regex = "20\\d\\d";
        System.out.println("2019".matches(regex)); // true
        System.out.println("2100".matches(regex)); // false
    }
}
```

可见，使用正则表达式，不必编写复杂的代码来判断，只需给出一个字符串表达的正则规则即可。

#### 12.1.1 小结

正则表达式是用字符串描述的一个匹配规则，使用正则表达式可以快速判断给定的字符串是否符合匹配规则。Java标准库`java.util.regex`内建了正则表达式引擎。

### 12.2 匹配规则

正则表达式的匹配规则是从左到右按规则匹配。我们首先来看如何使用正则表达式来做精确匹配。

对于正则表达式`abc`来说，它只能精确地匹配字符串`"abc"`，不能匹配`"ab"`，`"Abc"`，`"abcd"`等其他任何字符串。

如果正则表达式有特殊字符，那就需要用`\`转义。例如，正则表达式`a\&c`，其中`\&`是用来匹配特殊字符`&`的，它能精确匹配字符串`"a&c"`，但不能匹配`"ac"`、`"a-c"`、`"a&&c"`等。

要注意正则表达式在Java代码中也是一个字符串，所以，对于正则表达式`a\&c`来说，对应的Java字符串是`"a\\&c"`，因为`\`也是Java字符串的转义字符，两个`\\`实际上表示的是一个`\`：

```java
public class Main {
    public static void main(String[] args) {
        String re1 = "abc";
        System.out.println("abc".matches(re1));
        System.out.println("Abc".matches(re1));
        System.out.println("abcd".matches(re1));

        String re2 = "a\\&c"; // 对应的正则是a\&c
        System.out.println("a&c".matches(re2));
        System.out.println("a-c".matches(re2));
        System.out.println("a&&c".matches(re2));
    }
}
```

如果想匹配非ASCII字符，例如中文，那就用`\u####`的十六进制表示，例如：`a\u548cc`匹配字符串`"a和c"`，中文字符`和`的Unicode编码是`548c`。

#### 12.2.1 匹配任意字符

精确匹配实际上用处不大，因为我们直接用`String.equals()`就可以做到。大多数情况下，我们想要的匹配规则更多的是模糊匹配。我们可以用`.`匹配一个任意字符。

例如，正则表达式`a.c`中间的`.`可以匹配一个任意字符，例如，下面的字符串都可以被匹配：

- `"abc"`，因为`.`可以匹配字符`b`；
- `"a&c"`，因为`.`可以匹配字符`&`；
- `"acc"`，因为`.`可以匹配字符`c`。

但它不能匹配`"ac"`、`"a&&c"`，因为`.`匹配一个字符且仅限一个字符。

#### 12.2.2 匹配数字

用`.`可以匹配任意字符，这个口子开得有点大。如果我们只想匹配`0`~`9`这样的数字，可以用`\d`匹配。例如，正则表达式`00\d`可以匹配：

- `"007"`，因为`\d`可以匹配字符`7`；
- `"008"`，因为`\d`可以匹配字符`8`。

它不能匹配`"00A"`，`"0077"`，因为`\d`仅限单个数字字符。

#### 12.2.3 匹配常用字符

用`\w`可以匹配一个字母、数字或下划线，w的意思是word。例如，`java\w`可以匹配：

- `"javac"`，因为`\w`可以匹配英文字符`c`；
- `"java9"`，因为`\w`可以匹配数字字符`9`；。
- `"java_"`，因为`\w`可以匹配下划线`_`。

它不能匹配`"java#"`，`"java "`，因为`\w`不能匹配`#`、空格等字符。

#### 12.2.4 匹配空格字符

用`\s`可以匹配一个空格字符，注意空格字符不但包括空格，还包括tab字符（在Java中用`\t`表示）。例如，`a\sc`可以匹配：

- `"a c"`，因为`\s`可以匹配空格字符``；
- `"a c"`，因为`\s`可以匹配tab字符`\t`。

它不能匹配`"ac"`，`"abc"`等。

#### 12.2.5 匹配非数字

用`\d`可以匹配一个数字，而`\D`则匹配一个非数字。例如，`00\D`可以匹配：

- `"00A"`，因为`\D`可以匹配非数字字符`A`；
- `"00#"`，因为`\D`可以匹配非数字字符`#`。

`00\d`可以匹配的字符串`"007"`，`"008"`等，`00\D`是不能匹配的。

类似的，`\W`可以匹配`\w`不能匹配的字符，`\S`可以匹配`\s`不能匹配的字符，这几个正好是反着来的。

```java
public class Main {
    public static void main(String[] args) {
        String re1 = "java\\d"; // 对应的正则是java\d
        System.out.println("java9".matches(re1));
        System.out.println("java10".matches(re1));
        System.out.println("javac".matches(re1));

        String re2 = "java\\D";
        System.out.println("javax".matches(re2));
        System.out.println("java#".matches(re2));
        System.out.println("java5".matches(re2));
    }
}
```

#### 12.2.6 重复匹配

我们用`\d`可以匹配一个数字，例如，`A\d`可以匹配`"A0"`，`"A1"`，如果要匹配多个数字，比如`"A380"`，怎么办？

修饰符`*`可以匹配任意个字符，包括0个字符。我们用`A\d*`可以匹配：

- `A`：因为`\d*`可以匹配0个数字；
- `A0`：因为`\d*`可以匹配1个数字`0`；
- `A380`：因为`\d*`可以匹配多个数字`380`。

修饰符`+`可以匹配至少一个字符。我们用`A\d+`可以匹配：

- `A0`：因为`\d+`可以匹配1个数字`0`；
- `A380`：因为`\d+`可以匹配多个数字`380`。

但它无法匹配`"A"`，因为修饰符`+`要求至少一个字符。

修饰符`?`可以匹配0个或一个字符。我们用`A\d?`可以匹配：

- `A`：因为`\d?`可以匹配0个数字；
- `A0`：因为`\d+`可以匹配1个数字`0`。

但它无法匹配`"A33"`，因为修饰符`?`超过1个字符就不能匹配了。

如果我们想精确指定n个字符怎么办？用修饰符`{n}`就可以。`A\d{3}`可以精确匹配：

- `A380`：因为`\d{3}`可以匹配3个数字`380`。

如果我们想指定匹配n~m个字符怎么办？用修饰符`{n,m}`就可以。`A\d{3,5}`可以精确匹配：

- `A380`：因为`\d{3,5}`可以匹配3个数字`380`；
- `A3800`：因为`\d{3,5}`可以匹配4个数字`3800`；
- `A38000`：因为`\d{3,5}`可以匹配5个数字`38000`。

如果没有上限，那么修饰符`{n,}`就可以匹配至少n个字符。

#### 12.2.7 小结

单个字符的匹配规则如下：

| 正则表达式 | 规则                     | 可以匹配                       |
| :--------- | :----------------------- | :----------------------------- |
| `A`        | 指定字符                 | `A`                            |
| `\u548c`   | 指定Unicode字符          | `和`                           |
| `.`        | 任意字符                 | `a`，`b`，`&`，`0`             |
| `\d`       | 数字0~9                  | `0`~`9`                        |
| `\w`       | 大小写字母，数字和下划线 | `a`~`z`，`A`~`Z`，`0`~`9`，`_` |
| `\s`       | 空格、Tab键              | 空格，Tab                      |
| `\D`       | 非数字                   | `a`，`A`，`&`，`_`，……         |
| `\W`       | 非\w                     | `&`，`@`，`中`，……             |
| `\S`       | 非\s                     | `a`，`A`，`&`，`_`，……         |

多个字符的匹配规则如下：

| 正则表达式 | 规则             | 可以匹配                 |
| :--------- | :--------------- | :----------------------- |
| `A*`       | 任意个数字符     | 空，`A`，`AA`，`AAA`，…… |
| `A+`       | 至少1个字符      | `A`，`AA`，`AAA`，……     |
| `A?`       | 0个或1个字符     | 空，`A`                  |
| `A{3}`     | 指定个数字符     | `AAA`                    |
| `A{2,3}`   | 指定范围个数字符 | `AA`，`AAA`              |
| `A{2,}`    | 至少n个字符      | `AA`，`AAA`，`AAAA`，……  |
| `A{0,3}`   | 最多n个字符      | 空，`A`，`AA`，`AAA`     |

### 12.3 复杂匹配规则

#### 12.3.1 匹配开头和结尾

用正则表达式进行多行匹配时，我们用`^`表示开头，`$`表示结尾。例如，`^A\d{3}$`，可以匹配`"A001"`、`"A380"`。

#### 12.3.2 匹配指定范围

如果我们规定一个7~8位数字的电话号码不能以`0`开头，应该怎么写匹配规则呢？`\d{7,8}`是不行的，因为第一个`\d`可以匹配到`0`。

使用`[...]`可以匹配范围内的字符，例如，`[123456789]`可以匹配`1`~`9`，这样就可以写出上述电话号码的规则：`[123456789]\d{6,7}`。

把所有字符全列出来太麻烦，`[...]`还有一种写法，直接写`[1-9]`就可以。

要匹配大小写不限的十六进制数，比如`1A2b3c`，我们可以这样写：`[0-9a-fA-F]`，它表示一共可以匹配以下任意范围的字符：

- `0-9`：字符`0`~`9`；
- `a-f`：字符`a`~`f`；
- `A-F`：字符`A`~`F`。

如果要匹配6位十六进制数，前面讲过的`{n}`仍然可以继续配合使用：`[0-9a-fA-F]{6}`。

`[...]`还有一种排除法，即不包含指定范围的字符。假设我们要匹配任意字符，但不包括数字，可以写`[^1-9]{3}`：

- 可以匹配`"ABC"`，因为不包含字符`1`~`9`；
- 可以匹配`"A00"`，因为不包含字符`1`~`9`；
- 不能匹配`"A01"`，因为包含字符`1`；
- 不能匹配`"A05"`，因为包含字符`5`。

#### 12.3.3 或规则匹配

用`|`连接的两个正则规则是`或`规则，例如，`AB|CD`表示可以匹配`AB`或`CD`。

我们来看这个正则表达式`java|php`：

```java
public class Main {
    public static void main(String[] args) {
        String re = "java|php";
        System.out.println("java".matches(re));
        System.out.println("php".matches(re));
        System.out.println("go".matches(re));
    }
}
```

它可以匹配`"java"`或`"php"`，但无法匹配`"go"`。

要把`go`也加进来匹配，可以改写为`java|php|go`。

#### 12.3.4 使用括号

现在我们想要匹配字符串`learn java`、`learn php`和`learn go`怎么办？一个最简单的规则是`learn\sjava|learn\sphp|learn\sgo`，但是这个规则太复杂了，可以把公共部分提出来，然后用`(...)`把子规则括起来表示成`learn\\s(java|php|go)`。

```java
public class Main {
    public static void main(String[] args) {
        String re = "learn\\s(java|php|go)";
        System.out.println("learn java".matches(re));
        System.out.println("learn Java".matches(re));
        System.out.println("learn php".matches(re));
        System.out.println("learn Go".matches(re));
    }
}
```

#### 12.3.5 小结

复杂匹配规则主要有：

| 正则表达式 | 规则                 | 可以匹配                             |
| :--------- | :------------------- | :----------------------------------- |
| ^          | 开头                 | 字符串开头                           |
| $          | 结尾                 | 字符串结束                           |
| [ABC]      | […]内任意字符        | A，B，C                              |
| [A-F0-9xy] | 指定范围的字符       | `A`，……，`F`，`0`，……，`9`，`x`，`y` |
| [^A-F]     | 指定范围外的任意字符 | 非`A`~`F`                            |
| AB\|CD\|EF | AB或CD或EF           | `AB`，`CD`，`EF`                     |

### 12.4 分组匹配

`(...)`可以用来把一个子规则括起来，这样写`learn\s(java|php|go)`就可以更方便地匹配长字符串了。实际上`(...)`还有一个重要作用，就是分组匹配。

我们来看一下如何用正则匹配`区号-电话号`码这个规则。利用前面讲到的匹配规则，写出来很容易：

```java
\d{3,4}\-\d{6,8}
```

虽然这个正则匹配规则很简单，但是往往匹配成功后，下一步是提取区号和电话号码，分别存入数据库。于是问题来了：如何提取匹配的子串？

当然可以用`String`提供的`indexOf()`和`substring()`这些方法，但它们从正则匹配的字符串中提取子串没有通用性，下一次要提取`learn\s(java|php)`还得改代码。

正确的方法是用`(...)`先把要提取的规则分组，把上述正则表达式变为`(\d{3,4})\-(\d{6,8})`。

现在问题又来了：匹配后，如何按括号提取子串？

现在我们没办法用`String.matches()`这样简单的判断方法了，必须引入`java.util.regex`包，用`Pattern`对象匹配，匹配后获得一个`Matcher`对象，如果匹配成功，就可以直接从`Matcher.group(index)`返回子串：

```java
import java.util.regex.*;
public class Main {
    public static void main(String[] args) {
        Pattern p = Pattern.compile("(\\d{3,4})\\-(\\d{7,8})");
        Matcher m = p.matcher("010-12345678");
        if (m.matches()) {
            String g1 = m.group(1);
            String g2 = m.group(2);
            System.out.println(g1);
            System.out.println(g2);
        } else {
            System.out.println("匹配失败!");
        }
    }
}

```

运行上述代码，会得到两个匹配上的子串`010`和`12345678`。

要特别注意，`Matcher.group(index)`方法的参数用1表示第一个子串，2表示第二个子串。如果我们传入0会得到什么呢？答案是`010-12345678`，即整个正则匹配到的字符串。

#### 12.4.1 Pattern

我们在前面的代码中用到的正则表达式代码是`String.matches()`方法，而我们在分组提取的代码中用的是`java.util.regex`包里面的`Pattern`类和`Matcher`类。实际上这两种代码本质上是一样的，因为`String.matches()`方法内部调用的就是`Pattern`和`Matcher`类的方法。

但是反复使用`String.matches()`对同一个正则表达式进行多次匹配效率较低，因为每次都会创建出一样的`Pattern`对象。完全可以先创建出一个`Pattern`对象，然后反复使用，就可以实现编译一次，多次匹配：

```java
import java.util.regex.*;
public class Main {
    public static void main(String[] args) {
        Pattern pattern = Pattern.compile("(\\d{3,4})\\-(\\d{7,8})");
        pattern.matcher("010-12345678").matches(); // true
        pattern.matcher("021-123456").matches(); // true
        pattern.matcher("022#1234567").matches(); // false
        // 获得Matcher对象:
        Matcher matcher = pattern.matcher("010-12345678");
        if (matcher.matches()) {
            String whole = matcher.group(0); // "010-12345678", 0表示匹配的整个字符串
            String area = matcher.group(1); // "010", 1表示匹配的第1个子串
            String tel = matcher.group(2); // "12345678", 2表示匹配的第2个子串
            System.out.println(area);
            System.out.println(tel);
        }
    }
}

```

使用`Matcher`时，必须首先调用`matches()`判断是否匹配成功，匹配成功后，才能调用`group()`提取子串。利用提取子串的功能，我们轻松获得了区号和号码两部分。

#### 12.4.2 小结

正则表达式用`(...)`分组可以通过`Matcher`对象快速提取子串：

- `group(0)`表示匹配的整个字符串；
- `group(1)`表示第1个子串，`group(2)`表示第2个子串，以此类推。

### 12.5 非贪婪匹配

在介绍非贪婪匹配前，我们先看一个简单的问题：

给定一个字符串表示的数字，判断该数字末尾`0`的个数。例如：

- `"123000"`：3个`0`
- `"10100"`：2个`0`
- `"1001"`：0个`0`

可以很容易地写出该正则表达式：`(\d+)(0*)`，Java代码如下：

```java
import java.util.regex.*;
public class Main {
    public static void main(String[] args) {
        Pattern pattern = Pattern.compile("(\\d+)(0*)");
        Matcher matcher = pattern.matcher("1230000");
        if (matcher.matches()) {
            System.out.println("group1=" + matcher.group(1)); // "1230000"
            System.out.println("group2=" + matcher.group(2)); // ""
        }
    }
}

```

然而打印的第二个子串是空字符串`""`。

实际上，我们期望分组匹配结果是：

| input  | `\d+`  | `0*`  |
| :----- | :----- | :---- |
| 123000 | "123"  | "000" |
| 10100  | "101"  | "00"  |
| 1001   | "1001" | ""    |

但实际的分组匹配结果是这样的：

| input  | `\d+`    | `0*` |
| :----- | :------- | :--- |
| 123000 | "123000" | ""   |
| 10100  | "10100"  | ""   |
| 1001   | "1001"   | ""   |

仔细观察上述实际匹配结果，实际上它是完全合理的，因为`\d+`确实可以匹配后面任意个`0`。

这是因为正则表达式默认使用**贪婪匹配：任何一个规则，它总是尽可能多地向后匹配**，因此，`\d+`总是会把后面的`0`包含进来。

要让`\d+`尽量少匹配，让`0*`尽量多匹配，我们就必须让`\d+`使用非贪婪匹配。在规则`\d+`后面加个`?`即可表示非贪婪匹配。我们改写正则表达式如下：

```java
import java.util.regex.*;
public class Main {
    public static void main(String[] args) {
        Pattern pattern = Pattern.compile("(\\d+?)(0*)");
        Matcher matcher = pattern.matcher("1230000");
        if (matcher.matches()) {
            System.out.println("group1=" + matcher.group(1)); // "123"
            System.out.println("group2=" + matcher.group(2)); // "0000"
        }
    }
}

```

因此，给定一个匹配规则，加上`?`后就变成了非贪婪匹配。

我们再来看这个正则表达式`(\d??)(9*)`，注意`\d?`表示匹配0个或1个数字，后面第二个`?`表示非贪婪匹配，因此，给定字符串`"9999"`，匹配到的两个子串分别是`""`和`"9999"`，因为对于`\d?`来说，可以匹配1个`9`，也可以匹配0个`9`，但是因为后面的`?`表示非贪婪匹配，它就会尽可能少的匹配，结果是匹配了0个`9`。

#### 12.5.1 小结

正则表达式匹配默认使用贪婪匹配，可以使用`?`表示对某一规则进行非贪婪匹配。

注意区分`?`的含义：`\d??`。

### 12.6 搜索和替换

#### 12.6.1 分割字符串

使用正则表达式分割字符串可以实现更加灵活的功能。`String.split()`方法传入的正是正则表达式。我们来看下面的代码：

```java
"a b c".split("\\s"); // { "a", "b", "c" }
"a b  c".split("\\s"); // { "a", "b", "", "c" }
"a, b ;; c".split("[\\,\\;\\s]+"); // { "a", "b", "c" }

```

如果我们想让用户输入一组标签，然后把标签提取出来，因为用户的输入往往是不规范的，这时，使用合适的正则表达式，就可以消除多个空格、混合`,`和`;`这些不规范的输入，直接提取出规范的字符串。

#### 12.6.2 搜索字符串

使用正则表达式还可以搜索字符串，我们来看例子：

```java
import java.util.regex.*;
public class Main {
    public static void main(String[] args) {
        String s = "the quick brown fox jumps over the lazy dog.";
        Pattern p = Pattern.compile("\\wo\\w");
        Matcher m = p.matcher(s);
        while (m.find()) {
            String sub = s.substring(m.start(), m.end());
            System.out.println(sub);
        }
    }
}

```

我们获取到`Matcher`对象后，不需要调用`matches()`方法（因为匹配整个串肯定返回false），而是反复调用`find()`方法，在整个串中搜索能匹配上`\\wo\\w`规则的子串，并打印出来。这种方式比`String.indexOf()`要灵活得多，因为我们搜索的规则是3个字符：中间必须是`o`，前后两个必须是字符`[A-Za-z0-9_]`。

#### 12.6.3 替换字符串

使用正则表达式替换字符串可以直接调用`String.replaceAll()`，它的第一个参数是正则表达式，第二个参数是待替换的字符串。我们还是来看例子：

```java
// regex
public class Main {
    public static void main(String[] args) {
        String s = "The     quick\t\t brown   fox  jumps   over the  lazy dog.";
        String r = s.replaceAll("\\s+", " ");
        System.out.println(r); // "The quick brown fox jumps over the lazy dog."
    }
}

```

上面的代码把不规范的连续空格分隔的句子变成了规范的句子。可见，灵活使用正则表达式可以大大降低代码量。

#### 12.6.4 反向引用

如果我们要把搜索到的指定字符串按规则替换，比如前后各加一个`xxxx`，这个时候，使用`replaceAll()`的时候，我们传入的第二个参数可以使用`$1`、`$2`来反向引用匹配到的子串。例如：

```java
public class Main {
    public static void main(String[] args) {
        String s = "the quick brown fox jumps over the lazy dog.";
        String r = s.replaceAll("\\s([a-z]{4})\\s", " <b>$1</b> ");
        System.out.println(r);
    }
}

```

上述代码的运行结果是：

```java
the quick brown fox jumps <b>over</b> the <b>lazy</b> dog.

```

它实际上把任何4字符单词的前后用`xxxx`括起来。实现替换的关键就在于`" $1 "`，它用匹配的分组子串`([a-z]{4})`替换了`$1`。

#### 12.6.5 小结

使用正则表达式可以：

- 分割字符串：`String.split()`
- 搜索子串：`Matcher.find()`
- 替换字符串：`String.replaceAll()`

## 13 加密与安全

在计算机系统中，什么是加密与安全呢？

我们举个栗子：假设Bob要给Alice发一封邮件，在邮件传送的过程中，黑客可能会窃取到邮件的内容，所以需要防窃听。黑客还可能会篡改邮件的内容，Alice必须有能力识别出邮件有没有被篡改。最后，黑客可能假冒Bob给Alice发邮件，Alice必须有能力识别出伪造的邮件。

所以，应对潜在的安全威胁，需要做到三防：

- 防窃听

- 防篡改

- 防伪造

计算机加密技术就是为了实现上述目标，而现代计算机密码学理论是建立在严格的数学理论基础上的，密码学已经逐渐发展成一门科学。对于绝大多数开发者来说，设计一个安全的加密算法非常困难，验证一个加密算法是否安全更加困难，当前被认为安全的加密算法仅仅是迄今为止尚未被攻破。因此，要编写安全的计算机程序，我们要做到：

- 不要自己设计山寨的加密算法；
- 不要自己实现已有的加密算法；
- 不要自己修改已有的加密算法。

  本章我们会介绍最常用的加密算法，以及如何通过Java代码实现。

### 13.1 编码算法

要学习编码算法，我们先来看一看什么是编码。

ASCII码就是一种编码，字母`A`的编码是十六进制的`0x41`，字母`B`是`0x42`，以此类推：

| 字母 | ASCII编码 |
| :--- | :-------- |
| A    | 0x41      |
| B    | 0x42      |
| C    | 0x43      |
| D    | 0x44      |
| …    | …         |

因为ASCII编码最多只能有127个字符，要想对更多的文字进行编码，就需要用Unicode。而中文的中使用Unicode编码就是`0x4e2d`，使用UTF-8则需要3个字节编码：

| 汉字 | Unicode编码 | UTF-8编码 |
| :--- | :---------- | :-------- |
| 中   | 0x4e2d      | 0xe4b8ad  |
| 文   | 0x6587      | 0xe69687  |
| 编   | 0x7f16      | 0xe7bc96  |
| 码   | 0x7801      | 0xe7a081  |
| …    | …           | …         |

因此，最简单的编码是直接给每个字符指定一个若干字节表示的整数，复杂一点的编码就需要根据一个已有的编码推算出来。

比如UTF-8编码，它是一种不定长编码，但可以从给定字符的Unicode编码推算出来。

#### 13.1.1 URL编码

URL编码是浏览器发送数据给服务器时使用的编码，它通常附加在URL的参数部分，例如：

[https://www.baidu.com/s?wd=%E4%B8%AD%E6%96%87](https://www.baidu.com/s?wd=中文)

之所以需要URL编码，是因为出于兼容性考虑，很多服务器只识别ASCII字符。但如果URL中包含中文、日文这些非ASCII字符怎么办？不要紧，URL编码有一套规则：

- 如果字符是`A-Z`,`a-z`，`0-9`以及`-`、`_`、`.`、`*`，则保持不变；
- 如果是其他字符，先转换为UTF-8编码，然后对每个字节以`%XX`表示。

例如：字符`中`的UTF-8编码是`0xe4b8ad`，因此，它的URL编码是`%E4%B8%AD`。**URL编码总是大写**。

Java标准库提供了一个`URLEncoder`类来对任意字符串进行URL编码：

```java
import java.net.URLEncoder;
import java.nio.charset.StandardCharsets;
public class Main {
    public static void main(String[] args) {
        String encoded = URLEncoder.encode("中文!", StandardCharsets.UTF_8);
        System.out.println(encoded);
    }
}
```

上述代码的运行结果是`%E4%B8%AD%E6%96%87%21`，`中`的URL编码是`%E4%B8%AD`，`文`的URL编码是`%E6%96%87`，`!`虽然是ASCII字符，也要对其编码为`%21`。和标准的URL编码稍有不同，URLEncoder把空格字符编码成`+`，而现在的URL编码标准要求空格被编码为`%20`，不过，服务器都可以处理这两种情况。如果服务器收到URL编码的字符串，就可以对其进行解码，还原成原始字符串。Java标准库的`URLDecoder`就可以解码：

```java
import java.net.URLDecoder;
import java.nio.charset.StandardCharsets;
public class Main {
    public static void main(String[] args) {
        String decoded = URLDecoder.decode("%E4%B8%AD%E6%96%87%21", StandardCharsets.UTF_8);
        System.out.println(decoded);
    }
}

```

要特别注意：URL编码是编码算法，不是加密算法。URL编码的目的是把任意文本数据编码为`%`前缀表示的文本，编码后的文本仅包含`A-Z`,`a-z`，`0-9`以及`-`、`_`、`.`、`*`和`%`，便于浏览器和服务器处理。

#### 13.1.2 Base64编码

URL编码是对字符进行编码，表示成`%xx`的形式，而Base64编码是对二进制数据进行编码，表示成文本格式。

**Base64编码可以把任意长度的二进制数据变为纯文本**，且只包含`A~Z`、`a~z`、`0~9`、`+`、`/`、`=`这些字符。它的原理是把3字节的二进制数据按6 bit一组，用4个int整数表示，然后查表，把int整数用索引对应到字符，得到编码后的字符串。

举个例子：3个byte数据分别是`e4`、`b8`、`ad`，按6 bit分组得到`39`、`0b`、`22`和`2d`：

```ascii
┌───────────────┬───────────────┬───────────────┐
│      e4       │      b8       │      ad       │
└───────────────┴───────────────┴───────────────┘
┌─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┐
│1│1│1│0│0│1│0│0│1│0│1│1│1│0│0│0│1│0│1│0│1│1│0│1│
└─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┘
┌───────────┬───────────┬───────────┬───────────┐
│    39     │    0b     │    22     │    2d     │
└───────────┴───────────┴───────────┴───────────┘

```

因为6位整数的范围总是`0~63`，所以，能用64个字符表示：字符`A~Z`对应索引`0~25`，字符`a~z`对应索引`26~51`，字符`0~9`对应索引`52~61`，最后两个索引`62`、`63`分别用字符`+`和`/`表示。

在Java中，二进制数据就是`byte[]`数组。Java标准库提供了`Base64`来对`byte[]`数组进行编解码：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        byte[] input = new byte[] { (byte) 0xe4, (byte) 0xb8, (byte) 0xad };
        String b64encoded = Base64.getEncoder().encodeToString(input);
        System.out.println(b64encoded);
    }
}
```

编码后得到`5Lit`4个字符。要对`Base64`解码，仍然用`Base64`这个类：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        byte[] output = Base64.getDecoder().decode("5Lit");
        System.out.println(Arrays.toString(output)); // [-28, -72, -83]
    }
}
```

如果输入的`byte[]`数组长度不是3的整数倍肿么办？这种情况下，需要对输入的末尾补一个或两个`0x00`，编码后，在结尾加一个`=`表示补充了1个`0x00`，加两个`=`表示补充了2个`0x00`，解码的时候，去掉末尾补充的一个或两个`0x00`即可。

实际上，因为编码后的长度加上`=`总是4的倍数，所以即使不加`=`也可以计算出原始输入的`byte[]`。Base64编码的时候可以用`withoutPadding()`去掉`=`，解码出来的结果是一样的：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        byte[] input = new byte[] { (byte) 0xe4, (byte) 0xb8, (byte) 0xad, 0x21 };
        String b64encoded = Base64.getEncoder().encodeToString(input);
        String b64encoded2 = Base64.getEncoder().withoutPadding().encodeToString(input);
        System.out.println(b64encoded);
        System.out.println(b64encoded2);
        byte[] output = Base64.getDecoder().decode(b64encoded2);
        System.out.println(Arrays.toString(output));
    }
}

```

因为标准的Base64编码会出现`+`、`/`和`=`，所以不适合把Base64编码后的字符串放到URL中。一种针对URL的Base64编码可以在URL中使用的Base64编码，它仅仅是把`+`变成`-`，`/`变成`_`：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        byte[] input = new byte[] { 0x01, 0x02, 0x7f, 0x00 };
        String b64encoded = Base64.getUrlEncoder().encodeToString(input);
        System.out.println(b64encoded);
        byte[] output = Base64.getUrlDecoder().decode(b64encoded);
        System.out.println(Arrays.toString(output));
    }
}
```

Base64编码的目的是把二进制数据变成文本格式，这样在很多文本中就可以处理二进制数据。例如，电子邮件协议就是文本协议，如果要在电子邮件中添加一个二进制文件，就可以用Base64编码，然后以文本的形式传送。

Base64编码的缺点是传输效率会降低，因为它把原始数据的长度增加了1/3。和URL编码一样，Base64编码是一种编码算法，不是加密算法。如果把Base64的64个字符编码表换成32个、48个或者58个，就可以使用Base32编码，Base48编码和Base58编码。字符越少，编码的效率就会越低。

#### 13.1.3 小结

URL编码和Base64编码都是编码算法，它们不是加密算法；

URL编码的目的是把任意文本数据编码为%前缀表示的文本，便于浏览器和服务器处理；

Base64编码的目的是把任意二进制数据编码为文本，但编码后数据量会增加1/3。

### 13.2 哈希算法

哈希算法（Hash）又称摘要算法（Digest），它的作用是：对任意一组输入数据进行计算，得到一个固定长度的输出摘要。

哈希算法最重要的特点就是：

- 相同的输入一定得到相同的输出；
- 不同的输入大概率得到不同的输出。

哈希算法的目的就是为了验证原始数据是否被篡改。

Java字符串的`hashCode()`就是一个哈希算法，它的输入是任意字符串，输出是固定的4字节`int`整数：

```java
"hello".hashCode(); // 0x5e918d2
"hello, java".hashCode(); // 0x7a9d88e8
"hello, bob".hashCode(); // 0xa0dbae2f

```

两个相同的字符串永远会计算出相同的`hashCode`，否则基于`hashCode`定位的`HashMap`就无法正常工作。这也是为什么当我们自定义一个class时，覆写`equals()`方法时我们必须正确覆写`hashCode()`方法。

#### 13.2.1 哈希碰撞

哈希碰撞是指，两个不同的输入得到了相同的输出：

```java
"AaAaAa".hashCode(); // 0x7460e8c0
"BBAaBB".hashCode(); // 0x7460e8c0

```

碰撞能不能避免？答案是不能。碰撞是一定会出现的，因为输出的字节长度是固定的，`String`的`hashCode()`输出是4字节整数，最多只有4294967296种输出，但输入的数据长度是不固定的，有无数种输入。所以，哈希算法是把一个无限的输入集合映射到一个有限的输出集合，必然会产生碰撞。

碰撞不可怕，我们担心的不是碰撞，而是碰撞的概率，因为碰撞概率的高低关系到哈希算法的安全性。一个安全的哈希算法必须满足：

- 碰撞概率低；
- 不能猜测输出。

不能猜测输出是指，输入的任意一个bit的变化会造成输出完全不同，这样就很难从输出反推输入（只能依靠暴力穷举）。假设一种哈希算法有如下规律：

```java
hashA("java001") = "123456"
hashA("java002") = "123457"
hashA("java003") = "123458"

```

那么很容易从输出`123459`反推输入，这种哈希算法就不安全。安全的哈希算法从输出是看不出任何规律的：

```java
hashB("java001") = "123456"
hashB("java002") = "580271"
hashB("java003") = ???

```

常用的哈希算法有：

| 算法       | 输出长度（位） | 输出长度（字节） |
| :--------- | :------------- | :--------------- |
| MD5        | 128 bits       | 16 bytes         |
| SHA-1      | 160 bits       | 20 bytes         |
| RipeMD-160 | 160 bits       | 20 bytes         |
| SHA-256    | 256 bits       | 32 bytes         |
| SHA-512    | 512 bits       | 64 bytes         |

根据碰撞概率，哈希算法的输出长度越长，就越难产生碰撞，也就越安全。

Java标准库提供了常用的哈希算法，并且有一套统一的接口。我们以MD5算法为例，看看如何对输入计算哈希：

```java
import java.math.BigInteger;
import java.security.MessageDigest;
public class Main {
    public static void main(String[] args) throws Exception {
        // 创建一个MessageDigest实例:
        MessageDigest md = MessageDigest.getInstance("MD5");
        // 反复调用update输入数据:
        md.update("Hello".getBytes("UTF-8"));
        md.update("World".getBytes("UTF-8"));
        byte[] result = md.digest(); // 16 bytes: 68e109f0f40ca72a15e05cc22786f8e6
        System.out.println(new BigInteger(1, result).toString(16));
    }
}

```

使用`MessageDigest`时，我们首先根据哈希算法获取一个`MessageDigest`实例，然后，反复调用`update(byte[])`输入数据。当输入结束后，调用`digest()`方法获得byte[]数组表示的摘要，最后，把它转换为十六进制的字符串。

运行上述代码，可以得到输入`HelloWorld`的MD5是`68e109f0f40ca72a15e05cc22786f8e6`。

#### 13.2.2 哈希算法的用途

因为相同的输入永远会得到相同的输出，因此，如果输入被修改了，得到的输出就会不同。

我们在网站上下载软件的时候，经常看到下载页显示的哈希值。如何判断下载到本地的软件是原始的、未经篡改的文件？我们只需要自己计算一下本地文件的哈希值，再与官网公开的哈希值对比，如果相同，说明文件下载正确，否则，说明文件已被篡改。

哈希算法的另一个重要用途是存储用户口令。如果直接将用户的原始口令存放到数据库中，会产生极大的安全风险：

- 数据库管理员能够看到用户明文口令；
- 数据库数据一旦泄漏，黑客即可获取用户明文口令。

不存储用户的原始口令，那么如何对用户进行认证？

方法是存储用户口令的哈希，例如，MD5。

在用户输入原始口令后，系统计算用户输入的原始口令的MD5并与数据库存储的MD5对比，如果一致，说明口令正确，否则，口令错误。

因此，数据库存储用户名和口令的表内容应该像下面这样：

| username | password                         |
| :------- | :------------------------------- |
| bob      | f30aa7a662c728b7407c54ae6bfd27d1 |
| alice    | 25d55ad283aa400af464c76d713c07ad |
| tim      | 5f4dcc3b5aa765d61d8327deb882cf99 |

这样一来，数据库管理员看不到用户的原始口令。即使数据库泄漏，黑客也无法拿到用户的原始口令。想要拿到用户的原始口令，必须用暴力穷举的方法，一个口令一个口令地试，直到某个口令计算的MD5恰好等于指定值。

使用哈希口令时，还要注意防止彩虹表攻击。

什么是彩虹表呢？上面讲到了，如果只拿到MD5，从MD5反推明文口令，只能使用暴力穷举的方法。

然而黑客并不笨，暴力穷举会消耗大量的算力和时间。但是，如果有一个预先计算好的常用口令和它们的MD5的对照表：

| 常用口令 | MD5                              |
| :------- | :------------------------------- |
| hello123 | f30aa7a662c728b7407c54ae6bfd27d1 |
| 12345678 | 25d55ad283aa400af464c76d713c07ad |
| passw0rd | bed128365216c019988915ed3add75fb |
| 19700101 | 570da6d5277a646f6552b8832012f5dc |
| …        | …                                |
| 20201231 | 6879c0ae9117b50074ce0a0d4c843060 |

这个表就是彩虹表。如果用户使用了常用口令，黑客从MD5一下就能反查到原始口令：

bob的MD5：`f30aa7a662c728b7407c54ae6bfd27d1`，原始口令：`hello123`；

alice的MD5：`25d55ad283aa400af464c76d713c07ad`，原始口令：`12345678`；

tim的MD5：`bed128365216c019988915ed3add75fb`，原始口令：`passw0rd`。

这就是为什么不要使用常用密码，以及不要使用生日作为密码的原因。

即使用户使用了常用口令，我们也可以采取措施来抵御彩虹表攻击，方法是对每个口令额外添加随机数，这个方法称之为加盐（salt）：

```java
digest = md5(salt+inputPassword)

```

经过加盐处理的数据库表，内容如下：

| username | salt  | password                         |
| :------- | :---- | :------------------------------- |
| bob      | H1r0a | a5022319ff4c56955e22a74abcc2c210 |
| alice    | 7$p2w | e5de688c99e961ed6e560b972dab8b6a |
| tim      | z5Sk9 | 1eee304b92dc0d105904e7ab58fd2f64 |

加盐的目的在于使黑客的彩虹表失效，即使用户使用常用口令，也无法从MD5反推原始口令。

#### 13.2.3 SHA-1

SHA-1也是一种哈希算法，它的输出是160 bits，即20字节。SHA-1是由美国国家安全局开发的，SHA算法实际上是一个系列，包括SHA-0（已废弃）、SHA-1、SHA-256、SHA-512等。

在Java中使用SHA-1，和MD5完全一样，只需要把算法名称改为`"SHA-1"`：

```java
import java.math.BigInteger;
import java.security.MessageDigest;
public class Main {
    public static void main(String[] args) throws Exception {
        // 创建一个MessageDigest实例:
        MessageDigest md = MessageDigest.getInstance("SHA-1");
        // 反复调用update输入数据:
        md.update("Hello".getBytes("UTF-8"));
        md.update("World".getBytes("UTF-8"));
        byte[] result = md.digest(); // 20 bytes: 6f44e49f848dd8ed27f73f59ab5bd4631b3f6b0d
        System.out.println(new BigInteger(1, result).toString(16));
    }
}

```

类似的，计算SHA-256，我们需要传入名称`"SHA-256"`，计算SHA-512，我们需要传入名称`"SHA-512"`。Java标准库支持的所有哈希算法可以在[这里](https://docs.oracle.com/en/java/javase/13/docs/specs/security/standard-names.html#messagedigest-algorithms)查到。

**注意：MD5因为输出长度较短，短时间内破解是可能的，目前已经不推荐使用。**

#### 13.2.4 小结

哈希算法可用于验证数据完整性，具有防篡改检测的功能；

常用的哈希算法有MD5、SHA-1等；

用哈希存储口令时要考虑彩虹表攻击。

### 13.3 BouncyCastle

我们知道，Java标准库提供了一系列常用的哈希算法。

但如果我们要用的某种算法，Java标准库没有提供怎么办？

方法一：自己写一个，难度很大；

方法二：找一个现成的第三方库，直接使用。

[BouncyCastle](https://www.bouncycastle.org/)就是一个提供了很多哈希算法和加密算法的第三方库。它提供了Java标准库没有的一些算法，例如，RipeMD160哈希算法。

我们来看一下如何使用BouncyCastle这个第三方提供的算法。

首先，我们必须把BouncyCastle提供的jar包放到classpath中。这个jar包就是`bcprov-jdk15on-xxx.jar`，可以从[官方网站](https://www.bouncycastle.org/latest_releases.html)下载。

Java标准库的`java.security`包提供了一种标准机制，允许第三方提供商无缝接入。我们要使用BouncyCastle提供的RipeMD160算法，需要先把BouncyCastle注册一下：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        // 注册BouncyCastle:
        Security.addProvider(new BouncyCastleProvider());
        // 按名称正常调用:
        MessageDigest md = MessageDigest.getInstance("RipeMD160");
        md.update("HelloWorld".getBytes("UTF-8"));
        byte[] result = md.digest();
        System.out.println(new BigInteger(1, result).toString(16));
    }
}

```

其中，注册BouncyCastle是通过下面的语句实现的：

```java
Security.addProvider(new BouncyCastleProvider());

```

注册只需要在启动时进行一次，后续就可以使用BouncyCastle提供的所有哈希算法和加密算法。

#### 13.3.1 小结

BouncyCastle是一个开源的第三方算法提供商；

BouncyCastle提供了很多Java标准库没有提供的哈希算法和加密算法；

使用第三方算法前需要通过`Security.addProvider()`注册。

### 13.4 Hmac算法

在前面讲到哈希算法时，我们说，存储用户的哈希口令时，要加盐存储，目的就在于抵御彩虹表攻击。

我们回顾一下哈希算法：

```java
digest = hash(input)

```

正是因为相同的输入会产生相同的输出，我们加盐的目的就在于，使得输入有所变化：

```java
digest = hash(salt + input)

```

这个salt可以看作是一个额外的“认证码”，同样的输入，不同的认证码，会产生不同的输出。因此，要验证输出的哈希，必须同时提供“认证码”。

Hmac算法就是一种基于密钥的消息认证码算法，它的全称是Hash-based Message Authentication Code，是一种更安全的消息摘要算法。

Hmac算法总是和某种哈希算法配合起来用的。例如，我们使用MD5算法，对应的就是HmacMD5算法，它相当于“加盐”的MD5：

```java
HmacMD5 ≈ md5(secure_random_key, input)

```

因此，HmacMD5可以看作带有一个安全的key的MD5。使用HmacMD5而不是用MD5加salt，有如下好处：

- HmacMD5使用的key长度是64字节，更安全；
- Hmac是标准算法，同样适用于SHA-1等其他哈希算法；
- Hmac输出和原有的哈希算法长度一致。

可见，Hmac本质上就是把key混入摘要的算法。验证此哈希时，除了原始的输入数据，还要提供key。

为了保证安全，我们不会自己指定key，而是通过Java标准库的KeyGenerator生成一个安全的随机的key。下面是使用HmacMD5的代码：

```java
import java.math.BigInteger;
import javax.crypto.*;
public class Main {
    public static void main(String[] args) throws Exception {
        KeyGenerator keyGen = KeyGenerator.getInstance("HmacMD5");
        SecretKey key = keyGen.generateKey();
        // 打印随机生成的key:
        byte[] skey = key.getEncoded();
        System.out.println(new BigInteger(1, skey).toString(16));
        Mac mac = Mac.getInstance("HmacMD5");
        mac.init(key);
        mac.update("HelloWorld".getBytes("UTF-8"));
        byte[] result = mac.doFinal();
        System.out.println(new BigInteger(1, result).toString(16));
    }
}

```

和MD5相比，使用HmacMD5的步骤是：

1. 通过名称`HmacMD5`获取`KeyGenerator`实例；
2. 通过`KeyGenerator`创建一个`SecretKey`实例；
3. 通过名称`HmacMD5`获取`Mac`实例；
4. 用`SecretKey`初始化`Mac`实例；
5. 对`Mac`实例反复调用`update(byte[])`输入数据；
6. 调用`Mac`实例的`doFinal()`获取最终的哈希值。

我们可以用Hmac算法取代原有的自定义的加盐算法，因此，存储用户名和口令的数据库结构如下：

| username | secret_key (64 bytes) | password                         |
| :------- | :-------------------- | :------------------------------- |
| bob      | a8c06e05f92e...5e16   | 7e0387872a57c85ef6dddbaa12f376de |
| alice    | e6a343693985...f4be   | c1f929ac2552642b302e739bc0cdbaac |
| tim      | f27a973dfdc0...6003   | af57651c3a8a73303515804d4af43790 |

有了Hmac计算的哈希和`SecretKey`，我们想要验证怎么办？这时，`SecretKey`不能从`KeyGenerator`生成，而是从一个`byte[]`数组恢复：

```java
import java.util.Arrays;
import javax.crypto.*;
import javax.crypto.spec.*;
public class Main {
    public static void main(String[] args) throws Exception {
        byte[] hkey = new byte[] { 106, 70, -110, 125, 39, -20, 52, 56, 85, 9, -19, -72, 52, -53, 52, -45, -6, 119, -63,
                30, 20, -83, -28, 77, 98, 109, -32, -76, 121, -106, 0, -74, -107, -114, -45, 104, -104, -8, 2, 121, 6,
                97, -18, -13, -63, -30, -125, -103, -80, -46, 113, -14, 68, 32, -46, 101, -116, -104, -81, -108, 122,
                89, -106, -109 };

        SecretKey key = new SecretKeySpec(hkey, "HmacMD5");
        Mac mac = Mac.getInstance("HmacMD5");
        mac.init(key);
        mac.update("HelloWorld".getBytes("UTF-8"));
        byte[] result = mac.doFinal();
        System.out.println(Arrays.toString(result));
        // [126, 59, 37, 63, 73, 90, 111, -96, -77, 15, 82, -74, 122, -55, -67, 54]
    }
}

```

恢复`SecretKey`的语句就是`new SecretKeySpec(hkey, "HmacMD5")`。

#### 13.4.1 小结

Hmac算法是一种标准的基于密钥的哈希算法，可以配合MD5、SHA-1等哈希算法，计算的摘要长度和原摘要算法长度相同。

### 13.5 对称加密算法

对称加密算法就是传统的用一个密码进行加密和解密。例如，我们常用的WinZIP和WinRAR对压缩包的加密和解密，就是使用对称加密算法。

从程序的角度看，所谓加密，就是这样一个函数，它接收密码和明文，然后输出密文：

```java
secret = encrypt(key, message);

```

而解密则相反，它接收密码和密文，然后输出明文：

```java
plain = decrypt(key, secret);

```

在软件开发中，常用的对称加密算法有：

| 算法 | 密钥长度    | 工作模式             | 填充模式                                |
| :--- | :---------- | :------------------- | :-------------------------------------- |
| DES  | 56/64       | ECB/CBC/PCBC/CTR/... | NoPadding/PKCS5Padding/...              |
| AES  | 128/192/256 | ECB/CBC/PCBC/CTR/... | NoPadding/PKCS5Padding/PKCS7Padding/... |
| IDEA | 128         | ECB                  | PKCS5Padding/PKCS7Padding/...           |

密钥长度直接决定加密强度，而工作模式和填充模式可以看成是对称加密算法的参数和格式选择。Java标准库提供的算法实现并不包括所有的工作模式和所有填充模式，但是通常我们只需要挑选常用的使用就可以了。

最后注意，DES算法由于密钥过短，可以在短时间内被暴力破解，所以现在已经不安全了。

#### 13.5.1 使用AES加密

AES算法是目前应用最广泛的加密算法。我们先用ECB模式加密并解密：

```java
import java.security.*;
import java.util.Base64;

import javax.crypto.*;
import javax.crypto.spec.*;

public class Main {
    public static void main(String[] args) throws Exception {
        // 原文:
        String message = "Hello, world!";
        System.out.println("Message: " + message);
        // 128位密钥 = 16 bytes Key:
        byte[] key = "1234567890abcdef".getBytes("UTF-8");
        // 加密:
        byte[] data = message.getBytes("UTF-8");
        byte[] encrypted = encrypt(key, data);
        System.out.println("Encrypted: " + Base64.getEncoder().encodeToString(encrypted));
        // 解密:
        byte[] decrypted = decrypt(key, encrypted);
        System.out.println("Decrypted: " + new String(decrypted, "UTF-8"));
    }

    // 加密:
    public static byte[] encrypt(byte[] key, byte[] input) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");
        SecretKey keySpec = new SecretKeySpec(key, "AES");
        cipher.init(Cipher.ENCRYPT_MODE, keySpec);
        return cipher.doFinal(input);
    }

    // 解密:
    public static byte[] decrypt(byte[] key, byte[] input) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");
        SecretKey keySpec = new SecretKeySpec(key, "AES");
        cipher.init(Cipher.DECRYPT_MODE, keySpec);
        return cipher.doFinal(input);
    }
}

```

Java标准库提供的对称加密接口非常简单，使用时按以下步骤编写代码：

1. 根据算法名称/工作模式/填充模式获取Cipher实例；
2. 根据算法名称初始化一个SecretKey实例，密钥必须是指定长度；
3. 使用SerectKey初始化Cipher实例，并设置加密或解密模式；
4. 传入明文或密文，获得密文或明文。

ECB模式是最简单的AES加密模式，它只需要一个固定长度的密钥，固定的明文会生成固定的密文，这种一对一的加密方式会导致安全性降低，更好的方式是通过CBC模式，它需要一个随机数作为IV参数，这样对于同一份明文，每次生成的密文都不同：

```java
import java.security.*;
import java.util.Base64;
import javax.crypto.*;
import javax.crypto.spec.*;
public class Main {
    public static void main(String[] args) throws Exception {
        // 原文:
        String message = "Hello, world!";
        System.out.println("Message: " + message);
        // 256位密钥 = 32 bytes Key:
        byte[] key = "1234567890abcdef1234567890abcdef".getBytes("UTF-8");
        // 加密:
        byte[] data = message.getBytes("UTF-8");
        byte[] encrypted = encrypt(key, data);
        System.out.println("Encrypted: " + Base64.getEncoder().encodeToString(encrypted));
        // 解密:
        byte[] decrypted = decrypt(key, encrypted);
        System.out.println("Decrypted: " + new String(decrypted, "UTF-8"));
    }

    // 加密:
    public static byte[] encrypt(byte[] key, byte[] input) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
        SecretKeySpec keySpec = new SecretKeySpec(key, "AES");
        // CBC模式需要生成一个16 bytes的initialization vector:
        SecureRandom sr = SecureRandom.getInstanceStrong();
        byte[] iv = sr.generateSeed(16);
        IvParameterSpec ivps = new IvParameterSpec(iv);
        cipher.init(Cipher.ENCRYPT_MODE, keySpec, ivps);
        byte[] data = cipher.doFinal(input);
        // IV不需要保密，把IV和密文一起返回:
        return join(iv, data);
    }

    // 解密:
    public static byte[] decrypt(byte[] key, byte[] input) throws GeneralSecurityException {
        // 把input分割成IV和密文:
        byte[] iv = new byte[16];
        byte[] data = new byte[input.length - 16];
        System.arraycopy(input, 0, iv, 0, 16);
        System.arraycopy(input, 16, data, 0, data.length);
        // 解密:
        Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
        SecretKeySpec keySpec = new SecretKeySpec(key, "AES");
        IvParameterSpec ivps = new IvParameterSpec(iv);
        cipher.init(Cipher.DECRYPT_MODE, keySpec, ivps);
        return cipher.doFinal(data);
    }

    public static byte[] join(byte[] bs1, byte[] bs2) {
        byte[] r = new byte[bs1.length + bs2.length];
        System.arraycopy(bs1, 0, r, 0, bs1.length);
        System.arraycopy(bs2, 0, r, bs1.length, bs2.length);
        return r;
    }
}

```

在CBC模式下，需要一个随机生成的16字节IV参数，必须使用`SecureRandom`生成。因为多了一个`IvParameterSpec`实例，因此，初始化方法需要调用`Cipher`的一个重载方法并传入`IvParameterSpec`。

观察输出，可以发现每次生成的IV不同，密文也不同。

#### 13.5.2 小结

对称加密算法使用同一个密钥进行加密和解密，常用算法有DES、AES和IDEA等；

密钥长度由算法设计决定，AES的密钥长度是128/192/256位；

使用对称加密算法需要指定算法名称、工作模式和填充模式。

### 13.6 口令加密算法

上一节我们讲的AES加密，细心的童鞋可能会发现，密钥长度是固定的128/192/256位，而不是我们用WinZip/WinRAR那样，随便输入几位都可以。

这是因为对称加密算法决定了口令必须是固定长度，然后对明文进行分块加密。又因为安全需求，口令长度往往都是128位以上，即至少16个字符。

但是我们平时使用的加密软件，输入6位、8位都可以，难道加密方式不一样？

实际上用户输入的口令并不能直接作为AES的密钥进行加密（除非长度恰好是128/192/256位），并且用户输入的口令一般都有规律，安全性远远不如安全随机数产生的随机口令。因此，用户输入的口令，通常还需要使用PBE算法，采用随机数杂凑计算出真正的密钥，再进行加密。

PBE就是Password Based Encryption的缩写，它的作用如下：

```java
key = generate(userPassword, secureRandomPassword);

```

PBE的作用就是把用户输入的口令和一个安全随机的口令采用杂凑后计算出真正的密钥。以AES密钥为例，我们让用户输入一个口令，然后生成一个随机数，通过PBE算法计算出真正的AES口令，再进行加密，代码如下：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        // 把BouncyCastle作为Provider添加到java.security:
        Security.addProvider(new BouncyCastleProvider());
        // 原文:
        String message = "Hello, world!";
        // 加密口令:
        String password = "hello12345";
        // 16 bytes随机Salt:
        byte[] salt = SecureRandom.getInstanceStrong().generateSeed(16);
        System.out.printf("salt: %032x\n", new BigInteger(1, salt));
        // 加密:
        byte[] data = message.getBytes("UTF-8");
        byte[] encrypted = encrypt(password, salt, data);
        System.out.println("encrypted: " + Base64.getEncoder().encodeToString(encrypted));
        // 解密:
        byte[] decrypted = decrypt(password, salt, encrypted);
        System.out.println("decrypted: " + new String(decrypted, "UTF-8"));
    }

    // 加密:
    public static byte[] encrypt(String password, byte[] salt, byte[] input) throws GeneralSecurityException {
        PBEKeySpec keySpec = new PBEKeySpec(password.toCharArray());
        SecretKeyFactory skeyFactory = SecretKeyFactory.getInstance("PBEwithSHA1and128bitAES-CBC-BC");
        SecretKey skey = skeyFactory.generateSecret(keySpec);
        PBEParameterSpec pbeps = new PBEParameterSpec(salt, 1000);
        Cipher cipher = Cipher.getInstance("PBEwithSHA1and128bitAES-CBC-BC");
        cipher.init(Cipher.ENCRYPT_MODE, skey, pbeps);
        return cipher.doFinal(input);
    }

    // 解密:
    public static byte[] decrypt(String password, byte[] salt, byte[] input) throws GeneralSecurityException {
        PBEKeySpec keySpec = new PBEKeySpec(password.toCharArray());
        SecretKeyFactory skeyFactory = SecretKeyFactory.getInstance("PBEwithSHA1and128bitAES-CBC-BC");
        SecretKey skey = skeyFactory.generateSecret(keySpec);
        PBEParameterSpec pbeps = new PBEParameterSpec(salt, 1000);
        Cipher cipher = Cipher.getInstance("PBEwithSHA1and128bitAES-CBC-BC");
        cipher.init(Cipher.DECRYPT_MODE, skey, pbeps);
        return cipher.doFinal(input);
    }
}

```

使用PBE时，我们还需要引入BouncyCastle，并指定算法是`PBEwithSHA1and128bitAES-CBC-BC`。观察代码，实际上真正的AES密钥是调用`Cipher`的`init()`方法时同时传入`SecretKey`和`PBEParameterSpec`实现的。在创建`PBEParameterSpec`的时候，我们还指定了循环次数`1000`，循环次数越多，暴力破解需要的计算量就越大。

如果我们把salt和循环次数固定，就得到了一个通用的“口令”加密软件。如果我们把随机生成的salt存储在U盘，就得到了一个“口令”加USB Key的加密软件，它的好处在于，即使用户使用了一个非常弱的口令，没有USB Key仍然无法解密，因为USB Key存储的随机数密钥安全性非常高。

#### 13.6.1 小结

PBE算法通过用户口令和安全的随机salt计算出Key，然后再进行加密；

Key通过口令和安全的随机salt计算得出，大大提高了安全性；

PBE算法内部使用的仍然是标准对称加密算法（例如AES）。

### 13.7 密钥交换算法

对称加密算法解决了数据加密的问题。我们以AES加密为例，在现实世界中，小明要向路人甲发送一个加密文件，他可以先生成一个AES密钥，对文件进行加密，然后把加密文件发送给对方。因为对方要解密，就必须需要小明生成的密钥。

现在问题来了：如何传递密钥？

在不安全的信道上传递加密文件是没有问题的，因为黑客拿到加密文件没有用。但是，如何如何在不安全的信道上安全地传输密钥？

要解决这个问题，密钥交换算法即DH算法：Diffie-Hellman算法应运而生。

DH算法解决了密钥在双方不直接传递密钥的情况下完成密钥交换，这个神奇的交换原理完全由数学理论支持。

我们来看DH算法交换密钥的步骤。假设甲乙双方需要传递密钥，他们之间可以这么做：

1. 甲首选选择一个素数`p`，例如509，底数`g`，任选，例如5，随机数`a`，例如123，然后计算`A=g^a mod p`，结果是215，然后，甲发送`p＝509`，`g=5`，`A=215`给乙；
2. 乙方收到后，也选择一个随机数`b`，例如，456，然后计算`B=g^b mod p`，结果是181，乙再同时计算`s=A^b mod p`，结果是121；
3. 乙把计算的`B=181`发给甲，甲计算`s＝B^a mod p`的余数，计算结果与乙算出的结果一样，都是121。

所以最终双方协商出的密钥`s`是121。注意到这个密钥`s`并没有在网络上传输。而通过网络传输的`p`，`g`，`A`和`B`是无法推算出`s`的，因为实际算法选择的素数是非常大的。

所以，更确切地说，DH算法是一个密钥协商算法，双方最终协商出一个共同的密钥，而这个密钥不会通过网络传输。

如果我们把`a`看成甲的私钥，`A`看成甲的公钥，`b`看成乙的私钥，`B`看成乙的公钥，DH算法的本质就是双方各自生成自己的私钥和公钥，私钥仅对自己可见，然后交换公钥，并根据自己的私钥和对方的公钥，生成最终的密钥`secretKey`，DH算法通过数学定律保证了双方各自计算出的`secretKey`是相同的。

使用Java实现DH算法的代码如下：

```java
import java.math.BigInteger;
import java.security.*;
import java.security.spec.*;
import javax.crypto.KeyAgreement;
public class Main {
    public static void main(String[] args) {
        // Bob和Alice:
        Person bob = new Person("Bob");
        Person alice = new Person("Alice");

        // 各自生成KeyPair:
        bob.generateKeyPair();
        alice.generateKeyPair();

        // 双方交换各自的PublicKey:
        // Bob根据Alice的PublicKey生成自己的本地密钥:
        bob.generateSecretKey(alice.publicKey.getEncoded());
        // Alice根据Bob的PublicKey生成自己的本地密钥:
        alice.generateSecretKey(bob.publicKey.getEncoded());

        // 检查双方的本地密钥是否相同:
        bob.printKeys();
        alice.printKeys();
        // 双方的SecretKey相同，后续通信将使用SecretKey作为密钥进行AES加解密...
    }
}

class Person {
    public final String name;

    public PublicKey publicKey;
    private PrivateKey privateKey;
    private byte[] secretKey;

    public Person(String name) {
        this.name = name;
    }

    // 生成本地KeyPair:
    public void generateKeyPair() {
        try {
            KeyPairGenerator kpGen = KeyPairGenerator.getInstance("DH");
            kpGen.initialize(512);
            KeyPair kp = kpGen.generateKeyPair();
            this.privateKey = kp.getPrivate();
            this.publicKey = kp.getPublic();
        } catch (GeneralSecurityException e) {
            throw new RuntimeException(e);
        }
    }

    public void generateSecretKey(byte[] receivedPubKeyBytes) {
        try {
            // 从byte[]恢复PublicKey:
            X509EncodedKeySpec keySpec = new X509EncodedKeySpec(receivedPubKeyBytes);
            KeyFactory kf = KeyFactory.getInstance("DH");
            PublicKey receivedPublicKey = kf.generatePublic(keySpec);
            // 生成本地密钥:
            KeyAgreement keyAgreement = KeyAgreement.getInstance("DH");
            keyAgreement.init(this.privateKey); // 自己的PrivateKey
            keyAgreement.doPhase(receivedPublicKey, true); // 对方的PublicKey
            // 生成SecretKey密钥:
            this.secretKey = keyAgreement.generateSecret();
        } catch (GeneralSecurityException e) {
            throw new RuntimeException(e);
        }
    }

    public void printKeys() {
        System.out.printf("Name: %s\n", this.name);
        System.out.printf("Private key: %x\n", new BigInteger(1, this.privateKey.getEncoded()));
        System.out.printf("Public key: %x\n", new BigInteger(1, this.publicKey.getEncoded()));
        System.out.printf("Secret key: %x\n", new BigInteger(1, this.secretKey));
    }
}

```

但是DH算法并未解决中间人攻击，即甲乙双方并不能确保与自己通信的是否真的是对方。消除中间人攻击需要其他方法。

#### 13.7.1 小结

DH算法是一种密钥交换协议，通信双方通过不安全的信道协商密钥，然后进行对称加密传输。

DH算法没有解决中间人攻击。

### 13.8 非对称加密算法

从DH算法我们可以看到，公钥-私钥组成的密钥对是非常有用的加密方式，因为公钥是可以公开的，而私钥是完全保密的，由此奠定了非对称加密的基础。非对称加密就是加密和解密使用的不是相同的密钥：只有同一个公钥-私钥对才能正常加解密。因此，如果小明要加密一个文件发送给小红，他应该首先向小红索取她的公钥，然后，他用小红的公钥加密，把加密文件发送给小红，此文件只能由小红的私钥解开，因为小红的私钥在她自己手里，所以，除了小红，没有任何人能解开此文件。

非对称加密的典型算法就是RSA算法，它是由Ron Rivest，Adi Shamir，Leonard Adleman这三个哥们一起发明的，所以用他们仨的姓的首字母缩写表示。非对称加密相比对称加密的显著优点在于，对称加密需要协商密钥，而非对称加密可以安全地公开各自的公钥，在N个人之间通信的时候：使用非对称加密只需要N个密钥对，每个人只管理自己的密钥对。而使用对称加密需要则需要`N*(N-1)/2`个密钥，因此每个人需要管理`N-1`个密钥，密钥管理难度大，而且非常容易泄漏。

既然非对称加密这么好，那我们抛弃对称加密，完全使用非对称加密行不行？也不行。因为非对称加密的缺点就是运算速度非常慢，比对称加密要慢很多。所以，在实际应用的时候，非对称加密总是和对称加密一起使用。假设小明需要给小红需要传输加密文件，他俩首先交换了各自的公钥，然后：

1. 小明生成一个随机的AES口令，然后用小红的公钥通过RSA加密这个口令，并发给小红；
2. 小红用自己的RSA私钥解密得到AES口令；
3. 双方使用这个共享的AES口令用AES加密通信。

可见非对称加密实际上应用在第一步，即加密“AES口令”。这也是我们在浏览器中常用的HTTPS协议的做法，即浏览器和服务器先通过RSA交换AES口令，接下来双方通信实际上采用的是速度较快的AES对称加密，而不是缓慢的RSA非对称加密。

Java标准库提供了RSA算法的实现，示例代码如下：

```java
import java.math.BigInteger;
import java.security.*;
import javax.crypto.Cipher;
public class Main {
    public static void main(String[] args) throws Exception {
        // 明文:
        byte[] plain = "Hello, encrypt use RSA".getBytes("UTF-8");
        // 创建公钥／私钥对:
        Person alice = new Person("Alice");
        // 用Alice的公钥加密:
        byte[] pk = alice.getPublicKey();
        System.out.println(String.format("public key: %x", new BigInteger(1, pk)));
        byte[] encrypted = alice.encrypt(plain);
        System.out.println(String.format("encrypted: %x", new BigInteger(1, encrypted)));
        // 用Alice的私钥解密:
        byte[] sk = alice.getPrivateKey();
        System.out.println(String.format("private key: %x", new BigInteger(1, sk)));
        byte[] decrypted = alice.decrypt(encrypted);
        System.out.println(new String(decrypted, "UTF-8"));
    }
}

class Person {
    String name;
    // 私钥:
    PrivateKey sk;
    // 公钥:
    PublicKey pk;

    public Person(String name) throws GeneralSecurityException {
        this.name = name;
        // 生成公钥／私钥对:
        KeyPairGenerator kpGen = KeyPairGenerator.getInstance("RSA");
        kpGen.initialize(1024);
        KeyPair kp = kpGen.generateKeyPair();
        this.sk = kp.getPrivate();
        this.pk = kp.getPublic();
    }

    // 把私钥导出为字节
    public byte[] getPrivateKey() {
        return this.sk.getEncoded();
    }

    // 把公钥导出为字节
    public byte[] getPublicKey() {
        return this.pk.getEncoded();
    }

    // 用公钥加密:
    public byte[] encrypt(byte[] message) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.ENCRYPT_MODE, this.pk);
        return cipher.doFinal(message);
    }

    // 用私钥解密:
    public byte[] decrypt(byte[] input) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.DECRYPT_MODE, this.sk);
        return cipher.doFinal(input);
    }
}

```

RSA的公钥和私钥都可以通过`getEncoded()`方法获得以`byte[]`表示的二进制数据，并根据需要保存到文件中。要从`byte[]`数组恢复公钥或私钥，可以这么写：

```java
byte[] pkData = ...
byte[] skData = ...
KeyFactory kf = KeyFactory.getInstance("RSA");
// 恢复公钥:
X509EncodedKeySpec pkSpec = new X509EncodedKeySpec(pkData);
PublicKey pk = kf.generatePublic(pkSpec);
// 恢复私钥:
PKCS8EncodedKeySpec skSpec = new PKCS8EncodedKeySpec(skData);
PrivateKey sk = kf.generatePrivate(skSpec);

```

以RSA算法为例，它的密钥有256/512/1024/2048/4096等不同的长度。长度越长，密码强度越大，当然计算速度也越慢。

如果修改待加密的`byte[]`数据的大小，可以发现，使用512bit的RSA加密时，明文长度不能超过53字节，使用1024bit的RSA加密时，明文长度不能超过117字节，这也是为什么使用RSA的时候，总是配合AES一起使用，即用AES加密任意长度的明文，用RSA加密AES口令。

此外，只使用非对称加密算法不能防止中间人攻击。

#### 13.8.1 小结

非对称加密就是加密和解密使用的不是相同的密钥，只有同一个公钥-私钥对才能正常加解密；

只使用非对称加密算法不能防止中间人攻击。

### 13.9 签名算法

我们使用非对称加密算法的时候，对于一个公钥-私钥对，通常是用公钥加密，私钥解密。

如果使用私钥加密，公钥解密是否可行呢？实际上是完全可行的。

不过我们再仔细想一想，私钥是保密的，而公钥是公开的，用私钥加密，那相当于所有人都可以用公钥解密。这个加密有什么意义？

这个加密的意义在于，如果小明用自己的私钥加密了一条消息，比如`小明喜欢小红`，然后他公开了加密消息，由于任何人都可以用小明的公钥解密，从而使得任何人都可以确认`小明喜欢小红`这条消息肯定是小明发出的，其他人不能伪造这个消息，小明也不能抵赖这条消息不是自己写的。

因此，私钥加密得到的密文实际上就是数字签名，要验证这个签名是否正确，只能用私钥持有者的公钥进行解密验证。使用数字签名的目的是为了确认某个信息确实是由某个发送方发送的，任何人都不可能伪造消息，并且，发送方也不能抵赖。

在实际应用的时候，签名实际上并不是针对原始消息，而是针对原始消息的哈希进行签名，即：

```java
signature = encrypt(privateKey, sha256(message))

```

对签名进行验证实际上就是用公钥解密：

```java
hash = decrypt(publicKey, signature)

```

然后把解密后的哈希与原始消息的哈希进行对比。

因为用户总是使用自己的私钥进行签名，所以，私钥就相当于用户身份。而公钥用来给外部验证用户身份。

常用数字签名算法有：

- MD5withRSA
- SHA1withRSA
- SHA256withRSA

它们实际上就是指定某种哈希算法进行RSA签名的方式。

```java
import java.math.BigInteger;
import java.nio.charset.StandardCharsets;
import java.security.*;
public class Main {
    public static void main(String[] args) throws GeneralSecurityException {
        // 生成RSA公钥/私钥:
        KeyPairGenerator kpGen = KeyPairGenerator.getInstance("RSA");
        kpGen.initialize(1024);
        KeyPair kp = kpGen.generateKeyPair();
        PrivateKey sk = kp.getPrivate();
        PublicKey pk = kp.getPublic();

        // 待签名的消息:
        byte[] message = "Hello, I am Bob!".getBytes(StandardCharsets.UTF_8);

        // 用私钥签名:
        Signature s = Signature.getInstance("SHA1withRSA");
        s.initSign(sk);
        s.update(message);
        byte[] signed = s.sign();
        System.out.println(String.format("signature: %x", new BigInteger(1, signed)));

        // 用公钥验证:
        Signature v = Signature.getInstance("SHA1withRSA");
        v.initVerify(pk);
        v.update(message);
        boolean valid = v.verify(signed);
        System.out.println("valid? " + valid);
    }
}

```

使用其他公钥，或者验证签名的时候修改原始信息，都无法验证成功。

#### 13.9.1 DSA签名

除了RSA可以签名外，还可以使用DSA算法进行签名。DSA是Digital Signature Algorithm的缩写，它使用ElGamal数字签名算法。

DSA只能配合SHA使用，常用的算法有：

- SHA1withDSA
- SHA256withDSA
- SHA512withDSA

和RSA数字签名相比，DSA的优点是更快。

#### 13.9.2 ECDSA签名

椭圆曲线签名算法ECDSA：Elliptic Curve Digital Signature Algorithm也是一种常用的签名算法，它的特点是可以从私钥推出公钥。比特币的签名算法就采用了ECDSA算法，使用标准椭圆曲线secp256k1。BouncyCastle提供了ECDSA的完整实现。

#### 13.9.3 小结

数字签名就是用发送方的私钥对原始数据进行签名，只有用发送方公钥才能通过签名验证。

数字签名用于：

- 防止伪造；
- 防止抵赖；
- 检测篡改。

常用的数字签名算法包括：MD5withRSA／SHA1withRSA／SHA256withRSA／SHA1withDSA／SHA256withDSA／SHA512withDSA／ECDSA等。

### 13.10 数字证书

我们知道，摘要算法用来确保数据没有被篡改，非对称加密算法可以对数据进行加解密，签名算法可以确保数据完整性和抗否认性，把这些算法集合到一起，并搞一套完善的标准，这就是数字证书。

因此，数字证书就是集合了多种密码学算法，用于实现数据加解密、身份认证、签名等多种功能的一种安全标准。

数字证书可以防止中间人攻击，因为它采用链式签名认证，即通过根证书（Root CA）去签名下一级证书，这样层层签名，直到最终的用户证书。而Root CA证书内置于操作系统中，所以，任何经过CA认证的数字证书都可以对其本身进行校验，确保证书本身不是伪造的。

我们在上网时常用的HTTPS协议就是数字证书的应用。浏览器会自动验证证书的有效性。

要使用数字证书，首先需要创建证书。正常情况下，一个合法的数字证书需要经过CA签名，这需要认证域名并支付一定的费用。开发的时候，我们可以使用自签名的证书，这种证书可以正常开发调试，但不能对外作为服务使用，因为其他客户端并不认可未经CA签名的证书。

在Java程序中，数字证书存储在一种Java专用的key store文件中，JDK提供了一系列命令来创建和管理key store。我们用下面的命令创建一个key store，并设定口令123456：

```bash
keytool -storepass 123456 -genkeypair -keyalg RSA -keysize 1024 -sigalg SHA1withRSA -validity 3650 -alias mycert -keystore my.keystore -dname "CN=www.sample.com, OU=sample, O=sample, L=BJ, ST=BJ, C=CN"

```

几个主要的参数是：

- keyalg：指定RSA加密算法；
- sigalg：指定SHA1withRSA签名算法；
- validity：指定证书有效期3650天；
- alias：指定证书在程序中引用的名称；
- dname：最重要的`CN=www.sample.com`指定了`Common Name`，如果证书用在HTTPS中，这个名称必须与域名完全一致。

执行上述命令，JDK会在当前目录创建一个`my.keystore`文件，并存储创建成功的一个私钥和一个证书，它的别名是`mycert`。

有了key store存储的证书，我们就可以通过数字证书进行加解密和签名：

```java
import java.io.InputStream;
import java.math.BigInteger;
import java.security.*;
import java.security.cert.*;
import javax.crypto.Cipher;

public class Main {
    public static void main(String[] args) throws Exception {
        byte[] message = "Hello, use X.509 cert!".getBytes("UTF-8");
        // 读取KeyStore:
        KeyStore ks = loadKeyStore("/my.keystore", "123456");
        // 读取私钥:
        PrivateKey privateKey = (PrivateKey) ks.getKey("mycert", "123456".toCharArray());
        // 读取证书:
        X509Certificate certificate = (X509Certificate) ks.getCertificate("mycert");
        // 加密:
        byte[] encrypted = encrypt(certificate, message);
        System.out.println(String.format("encrypted: %x", new BigInteger(1, encrypted)));
        // 解密:
        byte[] decrypted = decrypt(privateKey, encrypted);
        System.out.println("decrypted: " + new String(decrypted, "UTF-8"));
        // 签名:
        byte[] sign = sign(privateKey, certificate, message);
        System.out.println(String.format("signature: %x", new BigInteger(1, sign)));
        // 验证签名:
        boolean verified = verify(certificate, message, sign);
        System.out.println("verify: " + verified);
    }

    static KeyStore loadKeyStore(String keyStoreFile, String password) {
        try (InputStream input = Main.class.getResourceAsStream(keyStoreFile)) {
            if (input == null) {
                throw new RuntimeException("file not found in classpath: " + keyStoreFile);
            }
            KeyStore ks = KeyStore.getInstance(KeyStore.getDefaultType());
            ks.load(input, password.toCharArray());
            return ks;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    static byte[] encrypt(X509Certificate certificate, byte[] message) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance(certificate.getPublicKey().getAlgorithm());
        cipher.init(Cipher.ENCRYPT_MODE, certificate.getPublicKey());
        return cipher.doFinal(message);
    }

    static byte[] decrypt(PrivateKey privateKey, byte[] data) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance(privateKey.getAlgorithm());
        cipher.init(Cipher.DECRYPT_MODE, privateKey);
        return cipher.doFinal(data);
    }

    static byte[] sign(PrivateKey privateKey, X509Certificate certificate, byte[] message)
            throws GeneralSecurityException {
        Signature signature = Signature.getInstance(certificate.getSigAlgName());
        signature.initSign(privateKey);
        signature.update(message);
        return signature.sign();
    }

    static boolean verify(X509Certificate certificate, byte[] message, byte[] sig) throws GeneralSecurityException {
        Signature signature = Signature.getInstance(certificate.getSigAlgName());
        signature.initVerify(certificate);
        signature.update(message);
        return signature.verify(sig);
    }
}

```

在上述代码中，我们从key store直接读取了私钥-公钥对，私钥以`PrivateKey`实例表示，公钥以`X509Certificate`表示，实际上数字证书只包含公钥，因此，读取证书并不需要口令，只有读取私钥才需要。如果部署到Web服务器上，例如Nginx，需要把私钥导出为Private Key格式，把证书导出为X509Certificate格式。

以HTTPS协议为例，浏览器和服务器建立安全连接的步骤如下：

1. 浏览器向服务器发起请求，服务器向浏览器发送自己的数字证书；
2. 浏览器用操作系统内置的Root CA来验证服务器的证书是否有效，如果有效，就使用该证书加密一个随机的AES口令并发送给服务器；
3. 服务器用自己的私钥解密获得AES口令，并在后续通讯中使用AES加密。

上述流程只是一种最常见的单向验证。如果服务器还要验证客户端，那么客户端也需要把自己的证书发送给服务器验证，这种场景常见于网银等。

注意：数字证书存储的是公钥，以及相关的证书链和算法信息。私钥必须严格保密，如果数字证书对应的私钥泄漏，就会造成严重的安全威胁。如果CA证书的私钥泄漏，那么该CA证书签发的所有证书将不可信。数字证书服务商[DigiNotar](https://en.wikipedia.org/wiki/DigiNotar)就发生过私钥泄漏导致公司破产的事故。

#### 13.10.1 小结

数字证书就是集合了多种密码学算法，用于实现数据加解密、身份认证、签名等多种功能的一种安全标准。

数字证书采用链式签名管理，顶级的Root CA证书已内置在操作系统中。

数字证书存储的是公钥，可以安全公开，而私钥必须严格保密。

## 14 多线程

多线程是Java最基本的一种并发模型，本章我们将详细介绍Java多线程编程。

### 14.1 多线程基础

现代操作系统（Windows，macOS，Linux）都可以执行多任务。多任务就是同时运行多个任务，CPU执行代码都是一条一条顺序执行的，但是，即使是单核cpu，也可以同时运行多个任务。因为操作系统执行多任务实际上就是让CPU对多个任务轮流交替执行。

例如，假设我们有语文、数学、英语3门作业要做，每个作业需要30分钟。我们把这3门作业看成是3个任务，可以做1分钟语文作业，再做1分钟数学作业，再做1分钟英语作业。这样轮流做下去，在某些人眼里看来，做作业的速度就非常快，看上去就像同时在做3门作业一样。

类似的，操作系统轮流让多个任务交替执行，例如，让浏览器执行0.001秒，让QQ执行0.001秒，再让音乐播放器执行0.001秒，在人看来，CPU就是在同时执行多个任务。

即使是多核CPU，因为通常任务的数量远远多于CPU的核数，所以任务也是交替执行的。

#### 14.1.1 进程

在计算机中，我们把一个任务称为一个进程，浏览器就是一个进程，视频播放器是另一个进程，类似的，音乐播放器和Word都是进程。

某些进程内部还需要同时执行多个子任务。例如，我们在使用Word时，Word可以让我们一边打字，一边进行拼写检查，同时还可以在后台进行打印，我们把子任务称为**线程**。

进程和线程的关系就是：**一个进程可以包含一个或多个线程，但至少会有一个线程**。

```ascii
                        ┌──────────┐
                        │Process   │
                        │┌────────┐│
            ┌──────────┐││ Thread ││┌──────────┐
            │Process   ││└────────┘││Process   │
            │┌────────┐││┌────────┐││┌────────┐│
┌──────────┐││ Thread ││││ Thread ││││ Thread ││
│Process   ││└────────┘││└────────┘││└────────┘│
│┌────────┐││┌────────┐││┌────────┐││┌────────┐│
││ Thread ││││ Thread ││││ Thread ││││ Thread ││
│└────────┘││└────────┘││└────────┘││└────────┘│
└──────────┘└──────────┘└──────────┘└──────────┘
┌──────────────────────────────────────────────┐
│               Operating System               │
└──────────────────────────────────────────────┘
```

操作系统调度的最小任务单位其实不是进程，而是线程。常用的Windows、Linux等操作系统都采用抢占式多任务，如何调度线程完全由操作系统决定，程序自己不能决定什么时候执行，以及执行多长时间。

因为同一个应用程序，既可以有多个进程，也可以有多个线程，因此，实现多任务的方法，有以下几种：

多进程模式（每个进程只有一个线程）：

```ascii
┌──────────┐ ┌──────────┐ ┌──────────┐
│Process   │ │Process   │ │Process   │
│┌────────┐│ │┌────────┐│ │┌────────┐│
││ Thread ││ ││ Thread ││ ││ Thread ││
│└────────┘│ │└────────┘│ │└────────┘│
└──────────┘ └──────────┘ └──────────┘
```

多线程模式（一个进程有多个线程）：

```ascii
┌────────────────────┐
│Process             │
│┌────────┐┌────────┐│
││ Thread ││ Thread ││
│└────────┘└────────┘│
│┌────────┐┌────────┐│
││ Thread ││ Thread ││
│└────────┘└────────┘│
└────────────────────┘
```

多进程＋多线程模式（复杂度最高）：

```ascii
┌──────────┐┌──────────┐┌──────────┐
│Process   ││Process   ││Process   │
│┌────────┐││┌────────┐││┌────────┐│
││ Thread ││││ Thread ││││ Thread ││
│└────────┘││└────────┘││└────────┘│
│┌────────┐││┌────────┐││┌────────┐│
││ Thread ││││ Thread ││││ Thread ││
│└────────┘││└────────┘││└────────┘│
└──────────┘└──────────┘└──────────┘
```

#### 14.1.2 进程 vs 线程

进程和线程是包含关系，但是多任务既可以由多进程实现，也可以由单进程内的多线程实现，还可以混合多进程＋多线程。具体采用哪种方式，要考虑到进程和线程的特点。和多线程相比，多进程的缺点在于：

- 创建进程比创建线程开销大，尤其是在Windows系统上；
- 进程间通信比线程间通信要慢，因为线程间通信就是读写同一个变量，速度很快。

而多进程的优点在于：多进程稳定性比多线程高，因为在多进程的情况下，一个进程崩溃不会影响其他进程，而在多线程的情况下，任何一个线程崩溃会直接导致整个进程崩溃。

#### 14.1.3 多线程

Java语言内置了多线程支持：一个Java程序实际上是一个JVM进程，JVM进程用一个主线程来执行`main()`方法，在`main()`方法内部，我们又可以启动多个线程。此外，JVM还有负责垃圾回收的其他工作线程等。因此，对于大多数Java程序来说，我们说多任务，实际上是说如何使用多线程实现多任务。

和单线程相比，多线程编程的特点在于：多线程经常需要读写共享数据，并且需要同步。例如，播放电影时，就必须由一个线程播放视频，另一个线程播放音频，两个线程需要协调运行，否则画面和声音就不同步。因此，多线程编程的复杂度高，调试更困难。

Java多线程编程的特点又在于：

- 多线程模型是Java程序最基本的并发模型；
- 后续读写网络、数据库、Web开发等都依赖Java多线程模型。

因此，必须掌握Java多线程编程才能继续深入学习其他内容。

### 14.2 创建新线程

Java语言内置了多线程支持。当Java程序启动的时候，实际上是启动了一个JVM进程，然后，JVM启动主线程来执行`main()`方法。在`main()`方法中，我们又可以启动其他线程。要创建一个新线程非常容易，我们需要实例化一个`Thread`实例，然后调用它的`start()`方法：

```java
// 多线程
public class Main {
    public static void main(String[] args) {
        Thread t = new Thread();
        t.start(); // 启动新线程
    }
}
```

但是这个线程启动后实际上什么也不做就立刻结束了。我们希望新线程能执行指定的代码，有以下几种方法：

方法一：从`Thread`派生一个自定义类，然后覆写`run()`方法：

```java
// 多线程
public class Main {
    public static void main(String[] args) {
        Thread t = new MyThread();
        t.start(); // 启动新线程
    }
}

class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("start new thread!");
    }
}
```

执行上述代码，注意到`start()`方法会在内部自动调用实例的`run()`方法。

方法二：创建`Thread`实例时，传入一个`Runnable`实例：

```java
// 多线程
public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable());
        t.start(); // 启动新线程
    }
}

class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("start new thread!");
    }
}
```

或者用Java8引入的lambda语法进一步简写为：

```java
// 多线程
public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(() -> System.out.println("start a new thread"));
        t.start();
    }
}
```

有童鞋会问，使用线程执行的打印语句，和直接在`main()`方法执行有区别吗？区别大了去了。我们看以下代码：

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("main start...");
        Thread t = new Thread() {
            public void run() {
                System.out.println("thread run...");
                System.out.println("thread end.");
            }
        };
        t.start();
        System.out.println("main end...");
    }
}
```

我们用蓝色表示主线程，也就是`main`线程，`main`线程执行的代码有4行，首先打印`main start`，然后创建`Thread`对象，紧接着调用`start()`启动新线程。当`start()`方法被调用时，JVM就创建了一个新线程，我们通过实例变量`t`来表示这个新线程对象，并开始执行。

接着，`main`线程继续执行打印`main end`语句，而`t`线程在`main`线程执行的同时会并发执行，打印`thread run`和`thread end`语句。

当`run()`方法结束时，新线程就结束了。而`main()`方法结束时，主线程也结束了。

我们再来看线程的执行顺序：

1. `main`线程肯定是先打印`main start`，再打印`main end`；
2. `t`线程肯定是先打印`thread run`，再打印`thread end`。

但是，除了可以肯定，`main start`会先打印外，`main end`打印在`thread run`之前、`thread end`之后或者之间，都无法确定。因为从`t`线程开始运行以后，两个线程就开始同时运行了，并且由操作系统调度，程序本身无法确定线程的调度顺序。

要模拟并发执行的效果，我们可以在线程中调用`Thread.sleep()`，强迫当前线程暂停一段时间：

```java
// 多线程
public class Main {
    public static void main(String[] args) {
        System.out.println("main start...");
        Thread t = new Thread() {
            public void run() {
                System.out.println("thread run...");
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {}
                System.out.println("thread end.");
            }
        };
        t.start();
        try {
            Thread.sleep(20);
        } catch (InterruptedException e) {}
        System.out.println("main end...");
    }
}
```

`sleep()`传入的参数是毫秒。调整暂停时间的大小，我们可以看到`main`线程和`t`线程执行的先后顺序。

要特别注意：**直接调用`Thread`实例的`run()`方法是无效的**：

```java
public class Main {
    public static void main(String[] args) {
        Thread t = new MyThread();
        t.run();
    }
}

class MyThread extends Thread {
    public void run {
        System.out.println("hello");
    }
}
```

直接调用`run()`方法，相当于调用了一个普通的Java方法，当前线程并没有任何改变，也不会启动新线程。上述代码实际上是在`main()`方法内部又调用了`run()`方法，打印`hello`语句是在`main`线程中执行的，没有任何新线程被创建。

必须调用`Thread`实例的`start()`方法才能启动新线程，如果我们查看`Thread`类的源代码，会看到`start()`方法内部调用了一个`private native void start0()`方法，`native`修饰符表示这个方法是由JVM虚拟机内部的C代码实现的，不是由Java代码实现的。

#### 14.2.1 线程的优先级

可以对线程设定优先级，设定优先级的方法是：

```java
Thread.setPriority(int n) // 1~10, 默认值5
```

优先级高的线程被操作系统调度的优先级较高，操作系统对高优先级线程可能调度更频繁，但我们决不能通过设置优先级来确保高优先级的线程一定会先执行。

#### 14.2.2 小结

- Java用`Thread`对象表示一个线程，通过调用`start()`启动一个新线程；
- 一个线程对象只能调用一次`start()`方法；
- 线程的执行代码写在`run()`方法中；
- 线程调度由操作系统决定，程序本身无法决定调度顺序；
- `Thread.sleep()`可以把当前线程暂停一段时间。

### 14.3 线程的状态

在Java程序中，一个线程对象只能调用一次`start()`方法启动新线程，并在新线程中执行`run()`方法。一旦`run()`方法执行完毕，线程就结束了。因此，Java线程的状态有以下几种：

- New：新创建的线程，尚未执行；
- Runnable：运行中的线程，正在执行`run()`方法的Java代码；
- Blocked：运行中的线程，因为某些操作被阻塞而挂起；
- Waiting：运行中的线程，因为某些操作在等待中；
- Timed Waiting：运行中的线程，因为执行`sleep()`方法正在计时等待；
- Terminated：线程已终止，因为`run()`方法执行完毕。

用一个状态转移图表示如下：

```ascii
         ┌─────────────┐
         │     New     │
         └─────────────┘
                │
                ▼
┌───────────────────────────────┐
│┌─────────────┐ ┌─────────────┐│
││  Runnable   │ │   Blocked   ││
│└─────────────┘ └─────────────┘│
│┌─────────────┐ ┌─────────────┐│
││   Waiting   │ │Timed Waiting││
│└─────────────┘ └─────────────┘│
└───────────────────────────────┘
                │
                ▼
         ┌─────────────┐
         │ Terminated  │
         └─────────────┘
```

当线程启动后，它可以在`Runnable`、`Blocked`、`Waiting`和`Timed Waiting`这几个状态之间切换，直到最后变成`Terminated`状态，线程终止。

线程终止的原因有：

- 线程正常终止：`run()`方法执行到`return`语句返回；
- 线程意外终止：`run()`方法因为未捕获的异常导致线程终止；
- 对某个线程的`Thread`实例调用`stop()`方法强制终止（强烈不推荐使用）。

一个线程还可以等待另一个线程直到其运行结束。例如，`main`线程在启动`t`线程后，可以通过`t.join()`等待`t`线程结束后再继续运行：

```java
// 多线程
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new Thread(() -> {
            System.out.println("hello");
        });
        System.out.println("start");
        t.start();
        t.join();
        System.out.println("end");
    }
}
```

当`main`线程对线程对象`t`调用`join()`方法时，主线程将等待变量`t`表示的线程运行结束，即`join`就是指等待该线程结束，然后才继续往下执行自身线程。所以，上述代码打印顺序可以肯定是`main`线程先打印`start`，`t`线程再打印`hello`，`main`线程最后再打印`end`。

如果`t`线程已经结束，对实例`t`调用`join()`会立刻返回。此外，`join(long)`的重载方法也可以指定一个等待时间，超过等待时间后就不再继续等待。

#### 14.3.1 小结

- Java线程对象`Thread`的状态包括：`New`、`Runnable`、`Blocked`、`Waiting`、`Timed Waiting`和`Terminated`；
- 通过对另一个线程对象调用`join()`方法可以等待其执行结束；
- 可以指定等待时间，超过等待时间线程仍然没有结束就不再等待；
- 对已经运行结束的线程调用`join()`方法会立刻返回。

### 14.4 中断线程

如果线程需要执行一个长时间任务，就可能需要能中断线程。中断线程就是其他线程给该线程发一个信号，该线程收到信号后结束执行`run()`方法，使得自身线程能立刻结束运行。

我们举个栗子：假设从网络下载一个100M的文件，如果网速很慢，用户等得不耐烦，就可能在下载过程中点“取消”，这时，程序就需要中断下载线程的执行。中断一个线程非常简单，只需要在其他线程中对目标线程调用`interrupt()`方法，目标线程需要反复检测自身状态是否是interrupted状态，如果是，就立刻结束运行。

我们还是看示例代码：

```java
// 中断线程
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new MyThread();
        t.start();
        Thread.sleep(1); // 暂停1毫秒
        t.interrupt(); // 中断t线程
        t.join(); // 等待t线程结束
        System.out.println("end");
    }
}

class MyThread extends Thread {
    public void run() {
        int n = 0;
        while (! isInterrupted()) {
            n ++;
            System.out.println(n + " hello!");
        }
    }
}
```

仔细看上述代码，`main`线程通过调用`t.interrupt()`方法中断`t`线程，但是要注意，`interrupt()`方法仅仅向`t`线程发出了“中断请求”，至于`t`线程是否能立刻响应，要看具体代码。而`t`线程的`while`循环会检测`isInterrupted()`，所以上述代码能正确响应`interrupt()`请求，使得自身立刻结束运行`run()`方法。

如果线程处于等待状态，例如，`t.join()`会让`main`线程进入等待状态，此时，如果对`main`线程调用`interrupt()`，`join()`方法会立刻抛出`InterruptedException`，因此，目标线程只要捕获到`join()`方法抛出的`InterruptedException`，就说明有其他线程对其调用了`interrupt()`方法，通常情况下该线程应该立刻结束运行。

我们来看下面的示例代码：

```java
// 中断线程
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new MyThread();
        t.start();
        Thread.sleep(1000);
        t.interrupt(); // 中断t线程
        t.join(); // 等待t线程结束
        System.out.println("end");
    }
}

class MyThread extends Thread {
    public void run() {
        Thread hello = new HelloThread();
        hello.start(); // 启动hello线程
        try {
            hello.join(); // 等待hello线程结束
        } catch (InterruptedException e) {
            System.out.println("interrupted!");
        }
        hello.interrupt();
    }
}

class HelloThread extends Thread {
    public void run() {
        int n = 0;
        while (!isInterrupted()) {
            n++;
            System.out.println(n + " hello!");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                break;
            }
        }
    }
}
```

`main`线程通过调用`t.interrupt()`从而通知`t`线程中断，而此时`t`线程正位于`hello.join()`的等待中，此方法会立刻结束等待并抛出`InterruptedException`。由于我们在`t`线程中捕获了`InterruptedException`，因此，就可以准备结束该线程。在`t`线程结束前，对`hello`线程也进行了`interrupt()`调用通知其中断。如果去掉这一行代码，可以发现`hello`线程仍然会继续运行，且JVM不会退出。

另一个常用的中断线程的方法是**设置标志位**。我们通常会用一个`running`标志位来标识线程是否应该继续运行，在外部线程中，通过把`HelloThread.running`置为`false`，就可以让线程结束：

```java
// 中断线程
public class Main {
    public static void main(String[] args)  throws InterruptedException {
        HelloThread t = new HelloThread();
        t.start();
        Thread.sleep(1);
        t.running = false; // 标志位置为false
    }
}

class HelloThread extends Thread {
    public volatile boolean running = true;
    public void run() {
        int n = 0;
        while (running) {
            n ++;
            System.out.println(n + " hello!");
        }
        System.out.println("end!");
    }
}
```

注意到`HelloThread`的标志位`boolean running`是一个线程间共享的变量。线程间共享变量需要使用`volatile`关键字标记，确保每个线程都能读取到更新后的变量值。

为什么要对线程间共享的变量用关键字`volatile`声明？这涉及到Java的内存模型。在Java虚拟机中，变量的值保存在主内存中，但是，当线程访问变量时，它会先获取一个副本，并保存在自己的工作内存中。如果线程修改了变量的值，虚拟机会在某个时刻把修改后的值回写到主内存，但是，这个时间是不确定的！

```ascii
┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐
│           Main Memory         │
│                               │
│  ┌───────┐┌───────┐┌───────┐  │
│  │ var A ││ var B ││ var C │  │
│  └───────┘└───────┘└───────┘  │
│     │ ▲               │ ▲     │
└  ─ ─│─│─ ─ ─ ─ ─ ─ ─ ─│─│─ ─ ─┘
      │ │               │ │
┌ ─ ─ ┼ ┼ ─ ─ ┐   ┌ ─ ─ ┼ ┼ ─ ─ ┐
│     ▼ │     │   │     ▼ │     │
│  ┌───────┐  │   │  ┌───────┐  │
│  │ var A │  │   │  │ var C │  │
│  └───────┘  │   │  └───────┘  │
│  Thread 1   │   │  Thread 2   │
└ ─ ─ ─ ─ ─ ─ ┘   └ ─ ─ ─ ─ ─ ─ ┘
```

这会导致如果一个线程更新了某个变量，另一个线程读取的值可能还是更新前的。例如，主内存的变量`a = true`，线程1执行`a = false`时，它在此刻仅仅是把变量`a`的副本变成了`false`，主内存的变量`a`还是`true`，在JVM把修改后的`a`回写到主内存之前，其他线程读取到的`a`的值仍然是`true`，这就造成了多线程之间共享的变量不一致。

因此，`volatile`关键字的目的是告诉虚拟机：

- 每次访问变量时，总是获取主内存的最新值；
- 每次修改变量后，立刻回写到主内存。

`volatile`关键字解决的是可见性问题：**当一个线程修改了某个共享变量的值，其他线程能够立刻看到修改后的值**。

如果我们去掉`volatile`关键字，运行上述程序，发现效果和带`volatile`差不多，这是因为在x86的架构下，JVM回写主内存的速度非常快，但是，换成ARM的架构，就会有显著的延迟。

#### 14.4.1 小结

对目标线程调用`interrupt()`方法可以请求中断一个线程，目标线程通过检测`isInterrupted()`标志获取自身是否已中断。如果目标线程处于等待状态，该线程会捕获到`InterruptedException`；目标线程检测到`isInterrupted()`为`true`或者捕获了`InterruptedException`都应该立刻结束自身线程；通过标志位判断需要正确使用`volatile`关键字；

`volatile`关键字解决了共享变量在线程间的可见性问题。

### 14.5 守护线程

Java程序入口就是由JVM启动`main`线程，`main`线程又可以启动其他线程。当所有线程都运行结束时，JVM退出，进程结束。如果有一个线程没有退出，JVM进程就不会退出。所以，必须保证所有线程都能及时结束。但是有一种线程的目的就是无限循环，例如，一个定时触发任务的线程：

```java
class TimerThread extends Thread {
    @Override
    public void run() {
        while (true) {
            System.out.println(LocalTime.now());
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                break;
            }
        }
    }
}
```

如果这个线程不结束，JVM进程就无法结束。问题是，由谁负责结束这个线程？然而这类线程经常没有负责人来负责结束它们。但是，当其他线程结束时，JVM进程又必须要结束，怎么办？答案是使用守护线程（Daemon Thread）。

**守护线程是指为其他线程服务的线程**。在JVM中，所有非守护线程都执行完毕后，无论有没有守护线程，虚拟机都会自动退出。因此，JVM退出时，不必关心守护线程是否已结束。如何创建守护线程呢？方法和普通线程一样，只是在调用`start()`方法前，调用`setDaemon(true)`把该线程标记为守护线程：

```java
Thread t = new MyThread();
t.setDaemon(true);
t.start();
```

在守护线程中，编写代码要注意：**守护线程不能持有任何需要关闭的资源**，例如打开文件等，因为虚拟机退出时，守护线程没有任何机会来关闭文件，这会导致数据丢失。

#### 14.5.1 小结

守护线程是为其他线程服务的线程；

所有非守护线程都执行完毕后，虚拟机退出；

守护线程不能持有需要关闭的资源（如打开文件等）。

### 14.6 线程同步

当多个线程同时运行时，线程的调度由操作系统决定，程序本身无法决定。因此，任何一个线程都有可能在任何指令处被操作系统暂停，然后在某个时间段后继续执行。这个时候，有个单线程模型下不存在的问题就来了：如果多个线程同时读写共享变量，会出现数据不一致的问题。

我们来看一个例子：

```java
// 多线程
public class Main {
    public static void main(String[] args) throws Exception {
        var add = new AddThread();
        var dec = new DecThread();
        add.start();
        dec.start();
        add.join();
        dec.join();
        System.out.println(Counter.count);
    }
}

class Counter {
    public static int count = 0;
}

class AddThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) { Counter.count += 1; }
    }
}

class DecThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) { Counter.count -= 1; }
    }
}
```

上面的代码很简单，两个线程同时对一个`int`变量进行操作，一个加10000次，一个减10000次，最后结果应该是0，但是，每次运行，结果实际上都是不一样的。这是因为对变量进行读取和写入时，结果要正确，必须保证是原子操作。原子操作是指不能被中断的一个或一系列操作。

例如，对于语句：

```java
n = n + 1;
```

看上去是一行语句，实际上对应了3条指令：

```java
ILOAD
IADD
ISTORE
```

我们假设`n`的值是`100`，如果两个线程同时执行`n = n + 1`，得到的结果很可能不是`102`，而是`101`，原因在于：

```ascii
┌───────┐    ┌───────┐
│Thread1│    │Thread2│
└───┬───┘    └───┬───┘
    │            │
    │ILOAD (100) │
    │            │ILOAD (100)
    │            │IADD
    │            │ISTORE (101)
    │IADD        │
    │ISTORE (101)│
    ▼            ▼
```

如果线程1在执行`ILOAD`后被操作系统中断，此刻如果线程2被调度执行，它执行`ILOAD`后获取的值仍然是`100`，最终结果被两个线程的`ISTORE`写入后变成了`101`，而不是期待的`102`。

这说明多线程模型下，要保证逻辑正确，对共享变量进行读写时，必须保证一组指令以原子方式执行：即某一个线程执行时，其他线程必须等待：

```ascii
┌───────┐     ┌───────┐
│Thread1│     │Thread2│
└───┬───┘     └───┬───┘
    │             │
    │-- lock --   │
    │ILOAD (100)  │
    │IADD         │
    │ISTORE (101) │
    │-- unlock -- │
    │             │-- lock --
    │             │ILOAD (101)
    │             │IADD
    │             │ISTORE (102)
    │             │-- unlock --
    ▼             ▼
```

通过加锁和解锁的操作，就能保证3条指令总是在一个线程执行期间，不会有其他线程会进入此指令区间。即使在执行期线程被操作系统中断执行，其他线程也会因为无法获得锁导致无法进入此指令区间。只有执行线程将锁释放后，其他线程才有机会获得锁并执行。这种**加锁和解锁之间的代码块我们称之为临界区**（Critical Section），**任何时候临界区最多只有一个线程能执行**。

可见，保证一段代码的原子性就是通过加锁和解锁实现的。Java程序使用`synchronized`关键字对一个对象进行加锁：

```java
synchronized(lock) {
    n = n + 1;
}
```

`synchronized`保证了代码块在任意时刻最多只有一个线程能执行。我们把上面的代码用`synchronized`改写如下：

```java
// 多线程
public class Main {
    public static void main(String[] args) throws Exception {
        var add = new AddThread();
        var dec = new DecThread();
        add.start();
        dec.start();
        add.join();
        dec.join();
        System.out.println(Counter.count);
    }
}

class Counter {
    public static final Object lock = new Object();
    public static int count = 0;
}

class AddThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {
                Counter.count += 1;
            }
        }
    }
}

class DecThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {
                Counter.count -= 1;
            }
        }
    }
}
```

注意到代码：

```java
synchronized(Counter.lock) { // 获取锁
    ...
} // 释放锁
```

它表示用`Counter.lock`实例作为锁，两个线程在执行各自的`synchronized(Counter.lock) { ... }`代码块时，必须先获得锁，才能进入代码块进行。执行结束后，在`synchronized`语句块结束会自动释放锁。这样一来，对`Counter.count`变量进行读写就不可能同时进行。上述代码无论运行多少次，最终结果都是0。

使用`synchronized`解决了多线程同步访问共享变量的正确性问题。但是，它的缺点是带来了性能下降。因为`synchronized`代码块无法并发执行。此外，加锁和解锁需要消耗一定的时间，所以，`synchronized`会降低程序的执行效率。

我们来概括一下如何使用`synchronized`：

1. 找出修改共享变量的线程代码块；
2. 选择一个共享实例作为锁；
3. 使用`synchronized(lockObject) { ... }`。

在使用`synchronized`的时候，不必担心抛出异常。因为无论是否有异常，都会在`synchronized`结束处正确释放锁：

```java
public void add(int m) {
    synchronized (obj) {
        if (m < 0) {
            throw new RuntimeException();
        }
        this.value += m;
    } // 无论有无异常，都会在此释放锁
}
```

我们再来看一个错误使用`synchronized`的例子：

```java
// 多线程
public class Main {
    public static void main(String[] args) throws Exception {
        var add = new AddThread();
        var dec = new DecThread();
        add.start();
        dec.start();
        add.join();
        dec.join();
        System.out.println(Counter.count);
    }
}

class Counter {
    public static final Object lock1 = new Object();
    public static final Object lock2 = new Object();
    public static int count = 0;
}

class AddThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock1) {
                Counter.count += 1;
            }
        }
    }
}

class DecThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock2) {
                Counter.count -= 1;
            }
        }
    }
}
```

结果并不是0，这是因为两个线程各自的`synchronized`锁住的*不是同一个对象*！这使得两个线程各自都可以同时获得锁：因为**JVM只保证同一个锁在任意时刻只能被一个线程获取，但两个不同的锁在同一时刻可以被两个线程分别获取。**

因此，使用`synchronized`的时候，获取到的是哪个锁非常重要。锁对象如果不对，代码逻辑就不对。

我们再看一个例子：

```java
// 多线程
public class Main {
    public static void main(String[] args) throws Exception {
        var ts = new Thread[] { new AddStudentThread(), new DecStudentThread(), new AddTeacherThread(), new DecTeacherThread() };
        for (var t : ts) {
            t.start();
        }
        for (var t : ts) {
            t.join();
        }
        System.out.println(Counter.studentCount);
        System.out.println(Counter.teacherCount);
    }
}

class Counter {
    public static final Object lock = new Object();
    public static int studentCount = 0;
    public static int teacherCount = 0;
}

class AddStudentThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {
                Counter.studentCount += 1;
            }
        }
    }
}

class DecStudentThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {
                Counter.studentCount -= 1;
            }
        }
    }
}

class AddTeacherThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {
                Counter.teacherCount += 1;
            }
        }
    }
}

class DecTeacherThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {
                Counter.teacherCount -= 1;
            }
        }
    }
}
```

上述代码的4个线程对两个共享变量分别进行读写操作，但是使用的锁都是`Counter.lock`这一个对象，这就造成了原本可以并发执行的`Counter.studentCount += 1`和`Counter.teacherCount += 1`，现在无法并发执行了，执行效率大大降低。实际上，需要同步的线程可以分成两组：`AddStudentThread`和`DecStudentThread`，`AddTeacherThread`和`DecTeacherThread`，组之间不存在竞争，因此，应该使用两个不同的锁，即：

`AddStudentThread`和`DecStudentThread`使用`lockStudent`锁：

```java
synchronized(Counter.lockStudent) {
    ...
}
```

`AddTeacherThread`和`DecTeacherThread`使用`lockTeacher`锁：

```java
synchronized(Counter.lockTeacher) {
    ...
}
```

这样才能最大化地提高执行效率。

#### 14.6.1 不需要synchronized的操作

JVM规范定义了几种原子操作：

- 基本类型（`long`和`double`除外）赋值，例如：`int n = m`；
- 引用类型赋值，例如：`List list = anotherList`。

`long`和`double`是64位数据，JVM没有明确规定64位赋值操作是不是一个原子操作，不过在x64平台的JVM是把`long`和`double`的赋值作为原子操作实现的。

单条原子操作的语句不需要同步。例如：

```java
public void set(int m) {
    synchronized(lock) {
        this.value = m;
    }
}
```

就不需要同步。对引用也是类似。例如：

```java
public void set(String s) {
    this.value = s;
}
```

上述赋值语句并不需要同步。

但是，如果是多行赋值语句，就必须保证是同步操作，例如：

```java
class Pair {
    int first;
    int last;
    public void set(int first, int last) {
        synchronized(this) {
            this.first = first;
            this.last = last;
        }
    }
}
```

有些时候，通过一些巧妙的转换，可以把非原子操作变为原子操作。例如，上述代码如果改造成：

```java
class Pair {
    int[] pair;
    public void set(int first, int last) {
        int[] ps = new int[] { first, last };
        this.pair = ps;
    }
}
```

就不再需要同步，因为`this.pair = ps`是引用赋值的原子操作。而语句：

```java
int[] ps = new int[] { first, last };
```

这里的`ps`是方法内部定义的局部变量，每个线程都会有各自的局部变量，互不影响，并且互不可见，并不需要同步。

#### 14.6.2 小结

- 多线程同时读写共享变量时，会造成逻辑错误，因此需要通过`synchronized`同步；
- 同步的本质就是给指定对象加锁，加锁后才能继续执行后续代码；
- 注意加锁对象必须是同一个实例；
- 对JVM定义的单个原子操作不需要同步。

### 14.7 同步方法

我们知道Java程序依靠`synchronized`对线程进行同步，使用`synchronized`的时候，锁住的是哪个对象非常重要。

让线程自己选择锁对象往往会使得代码逻辑混乱，也不利于封装。更好的方法是把`synchronized`逻辑封装起来。例如，我们编写一个计数器如下：

```java
public class Counter {
    private int count = 0;

    public void add(int n) {
        synchronized(this) {
            count += n;
        }
    }

    public void dec(int n) {
        synchronized(this) {
            count -= n;
        }
    }

    public int get() {
        return count;
    }
}
```

这样一来，线程调用`add()`、`dec()`方法时，它不必关心同步逻辑，因为`synchronized`代码块在`add()`、`dec()`方法内部。并且，我们注意到，`synchronized`锁住的对象是`this`，即当前实例，这又使得创建多个`Counter`实例的时候，它们之间互不影响，可以并发执行：

```java
var c1 = new Counter();
var c2 = new Counter();
new Thread(()-> c1.add(1)).start();
new Thread(()-> c1.dec(1)).start();
new Thread(()-> c2.add(1)).start();
new Thread(()-> c2.dec(1)).start();
```

现在，对于`Counter`类，多线程可以正确调用。

如果一个类被设计为允许多线程正确访问，我们就说这个类就是“**线程安全**”的（thread-safe），上面的`Counter`类就是线程安全的。Java标准库的`java.lang.StringBuffer`也是线程安全的。

还有一些不变类，例如`String`，`Integer`，`LocalDate`，它们的所有成员变量都是`final`，多线程同时访问时只能读不能写，这些不变类也是线程安全的。

最后，类似`Math`这些只提供静态方法，没有成员变量的类，也是线程安全的。

除了上述几种少数情况，大部分类，例如`ArrayList`，都是非线程安全的类，我们不能在多线程中修改它们。但是，如果所有线程都只读取，不写入，那么`ArrayList`是可以安全地在线程间共享的。

 **没有特殊说明时，一个类默认是非线程安全的**。

我们再观察`Counter`的代码：

```java
public class Counter {
    public void add(int n) {
        synchronized(this) {
            count += n;
        }
    }
    ...
}
```

当我们锁住的是`this`实例时，实际上可以用`synchronized`修饰这个方法。下面两种写法是等价的：

```java
public void add(int n) {
    synchronized(this) { // 锁住this
        count += n;
    } // 解锁
}
public synchronized void add(int n) { // 锁住this
    count += n;
} // 解锁
```

因此，用`synchronized`修饰的方法就是同步方法，它表示整个方法都必须用`this`实例加锁。

我们再思考一下，如果对一个静态方法添加`synchronized`修饰符，它锁住的是哪个对象？

```java
public synchronized static void test(int n) {
    ...
}
```

对于`static`方法，是没有`this`实例的，因为`static`方法是针对类而不是实例。但是我们注意到任何一个类都有一个由JVM自动创建的`Class`实例，因此，**对`static`方法添加`synchronized`，锁住的是该类的`class`实例**。上述`synchronized static`方法实际上相当于：

```java
public class Counter {
    public static void test(int n) {
        synchronized(Counter.class) {
            ...
        }
    }
}
```

我们再考察`Counter`的`get()`方法：

```java
public class Counter {
    private int count;

    public int get() {
        return count;
    }
    ...
}
```

它没有同步，因为读一个`int`变量不需要同步。

然而，如果我们把代码稍微改一下，返回一个包含两个`int`的对象：

```java
public class Counter {
    private int first;
    private int last;

    public Pair get() {
        Pair p = new Pair();
        p.first = first;
        p.last = last;
        return p;
    }
    ...
}
```

就必须要同步了。

#### 14.7.1 小结

- 用`synchronized`修饰方法可以把整个方法变为同步代码块，`synchronized`方法加锁对象是`this`；
- 通过合理的设计和数据封装可以让一个类变为“线程安全”；
- 一个类没有特殊说明，默认不是thread-safe；
- 多线程能否安全访问某个非线程安全的实例，需要具体问题具体分析。

### 14.8 可重入的锁和死锁

Java的线程锁是可重入的锁。

什么是可重入的锁？我们还是来看例子：

```java
public class Counter {
    private int count = 0;

    public synchronized void add(int n) {
        if (n < 0) {
            dec(-n);
        } else {
            count += n;
        }
    }

    public synchronized void dec(int n) {
        count += n;
    }
}
```

观察`synchronized`修饰的`add()`方法，一旦线程执行到`add()`方法内部，说明它已经获取了当前实例

的`this`锁。如果传入的`n < 0`，将在`add()`方法内部调用`dec()`方法。由于`dec()`方法也需要获取`this`锁，现在问题来了：

对同一个线程，能否在获取到锁以后继续获取同一个锁？

答案是肯定的。**JVM允许同一个线程重复获取同一个锁，这种能被同一个线程反复获取的锁，就叫做可重入锁**。

由于Java的线程锁是可重入锁，所以，获取锁的时候，不但要判断是否是第一次获取，还要记录这是第几次获取。每获取一次锁，记录+1，每退出`synchronized`块，记录-1，减到0的时候，才会真正释放锁。

#### 14.8.1 死锁

一个线程可以获取一个锁后，再继续获取另一个锁。例如：

```java
public void add(int m) {
    synchronized(lockA) { // 获得lockA的锁
        this.value += m;
        synchronized(lockB) { // 获得lockB的锁
            this.another += m;
        } // 释放lockB的锁
    } // 释放lockA的锁
}

public void dec(int m) {
    synchronized(lockB) { // 获得lockB的锁
        this.another -= m;
        synchronized(lockA) { // 获得lockA的锁
            this.value -= m;
        } // 释放lockA的锁
    } // 释放lockB的锁
}
```

在获取多个锁的时候，不同线程获取多个不同对象的锁可能导致死锁。对于上述代码，线程1和线程2如果分别执行`add()`和`dec()`方法时：

- 线程1：进入`add()`，获得`lockA`；
- 线程2：进入`dec()`，获得`lockB`。

随后：

- 线程1：准备获得`lockB`，失败，等待中；
- 线程2：准备获得`lockA`，失败，等待中。

此时，**两个线程各自持有不同的锁，然后各自试图获取对方手里的锁，造成了双方无限等待下去，这就是死锁**。

死锁发生后，没有任何机制能解除死锁，只能强制结束JVM进程。

因此，在编写多线程应用时，要特别注意防止死锁。因为死锁一旦形成，就只能强制结束进程。

那么我们应该**如何避免死锁**呢？答案是：**线程获取锁的顺序要一致**。即严格按照先获取`lockA`，再获取`lockB`的顺序，改写`dec()`方法如下：

```java
public void dec(int m) {
    synchronized(lockA) { // 获得lockA的锁
        this.value -= m;
        synchronized(lockB) { // 获得lockB的锁
            this.another -= m;
        } // 释放lockB的锁
    } // 释放lockA的锁
}
```

#### 14.8.2 小结

- Java的`synchronized`锁是可重入锁；
- 死锁产生的条件是多线程各自持有不同的锁，并互相试图获取对方已持有的锁，导致无限等待；
- 避免死锁的方法是多线程获取锁的顺序要一致。

### 14.9 使用wait和notify

在Java程序中，`synchronized`解决了多线程竞争的问题。例如，对于一个任务管理器，多个线程同时往队列中添加任务，可以用`synchronized`加锁：

```java
class TaskQueue {
    Queue<String> queue = new LinkedList<>();

    public synchronized void addTask(String s) {
        this.queue.add(s);
    }
}
```

但是`synchronized`并没有解决多线程协调的问题。仍然以上面的`TaskQueue`为例，我们再编写一个`getTask()`方法取出队列的第一个任务：

```java
class TaskQueue {
    Queue<String> queue = new LinkedList<>();

    public synchronized void addTask(String s) {
        this.queue.add(s);
    }

    public synchronized String getTask() {
        while (queue.isEmpty()) {
        }
        return queue.remove();
    }
}
```

上述代码看上去没有问题：`getTask()`内部先判断队列是否为空，如果为空，就循环等待，直到另一个线程往队列中放入了一个任务，`while()`循环退出，就可以返回队列的元素了。

但实际上`while()`循环永远不会退出。因为线程在执行`while()`循环时，已经在`getTask()`入口获取了`this`锁，其他线程根本无法调用`addTask()`，因为`addTask()`执行条件也是获取`this`锁。

因此，执行上述代码，线程会在`getTask()`中因为死循环而100%占用CPU资源。

如果深入思考一下，我们想要的执行效果是：

- 线程1可以调用`addTask()`不断往队列中添加任务；
- 线程2可以调用`getTask()`从队列中获取任务。如果队列为空，则`getTask()`应该等待，直到队列中至少有一个任务时再返回。

因此，多线程协调运行的原则就是：**当条件不满足时，线程进入等待状态；当条件满足时，线程被唤醒，继续执行任务**。

对于上述`TaskQueue`，我们先改造`getTask()`方法，在条件不满足时，线程进入等待状态：

```java
public synchronized String getTask() {
    while (queue.isEmpty()) {
        this.wait();
    }
    return queue.remove();
}
```

当一个线程执行到`getTask()`方法内部的`while`循环时，它必定已经获取到了`this`锁，此时，线程执行`while`条件判断，如果条件成立（队列为空），线程将执行`this.wait()`，进入等待状态。这里的关键是：`wait()`方法必须在当前获取的锁对象上调用，这里获取的是`this`锁，因此调用`this.wait()`。调用`wait()`方法后，线程进入等待状态，`wait()`方法不会返回，直到将来某个时刻，线程从等待状态被其他线程唤醒后，`wait()`方法才会返回，然后，继续执行下一条语句。

有些仔细的童鞋会指出：即使线程在`getTask()`内部等待，其他线程如果拿不到`this`锁，照样无法执行`addTask()`，肿么办？这个问题的关键就在于`wait()`方法的执行机制非常复杂。首先，它不是一个普通的Java方法，而是定义在`Object`类的一个`native`方法，也就是由JVM的C代码实现的。其次，必须在`synchronized`块中才能调用`wait()`方法，因为`wait()`方法调用时，会*释放*线程获得的锁，`wait()`方法返回后，线程又会重新试图获得锁。

因此，只能在锁对象上调用`wait()`方法。因为在`getTask()`中，我们获得了`this`锁，因此，只能在`this`对象上调用`wait()`方法：

```java
public synchronized String getTask() {
    while (queue.isEmpty()) {
        // 释放this锁:
        this.wait();
        // 重新获取this锁
    }
    return queue.remove();
}
```

当一个线程在`this.wait()`等待时，它就会释放`this`锁，从而使得其他线程能够在`addTask()`方法获得`this`锁。现在我们面临第二个问题：如何让等待的线程被重新唤醒，然后从`wait()`方法返回？答案是在相同的锁对象上调用`notify()`方法。我们修改`addTask()`如下：

```java
public synchronized void addTask(String s) {
    this.queue.add(s);
    this.notify(); // 唤醒在this锁等待的线程
}
```

注意到在往队列中添加了任务后，线程立刻对`this`锁对象调用`notify()`方法，这个方法会唤醒一个正在`this`锁等待的线程（就是在`getTask()`中位于`this.wait()`的线程），从而使得等待线程从`this.wait()`方法返回。

我们来看一个完整的例子：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) throws InterruptedException {
        var q = new TaskQueue();
        var ts = new ArrayList<Thread>();
        for (int i=0; i<5; i++) {
            var t = new Thread() {
                public void run() {
                    // 执行task:
                    while (true) {
                        try {
                            String s = q.getTask();
                            System.out.println("execute task: " + s);
                        } catch (InterruptedException e) {
                            return;
                        }
                    }
                }
            };
            t.start();
            ts.add(t);
        }
        var add = new Thread(() -> {
            for (int i=0; i<10; i++) {
                // 放入task:
                String s = "t-" + Math.random();
                System.out.println("add task: " + s);
                q.addTask(s);
                try { Thread.sleep(100); } catch(InterruptedException e) {}
            }
        });
        add.start();
        add.join();
        Thread.sleep(100);
        for (var t : ts) {
            t.interrupt();
        }
    }
}

class TaskQueue {
    Queue<String> queue = new LinkedList<>();

    public synchronized void addTask(String s) {
        this.queue.add(s);
        this.notifyAll();
    }

    public synchronized String getTask() throws InterruptedException {
        while (queue.isEmpty()) {
            this.wait();
        }
        return queue.remove();
    }
}
```

这个例子中，我们重点关注`addTask()`方法，内部调用了`this.notifyAll()`而不是`this.notify()`，使用`notifyAll()`将唤醒所有当前正在`this`锁等待的线程，而`notify()`只会唤醒其中一个（具体哪个依赖操作系统，有一定的随机性）。这是因为可能有多个线程正在`getTask()`方法内部的`wait()`中等待，使用`notifyAll()`将一次性全部唤醒。通常来说，`notifyAll()`更安全。有些时候，如果我们的代码逻辑考虑不周，用`notify()`会导致只唤醒了一个线程，而其他线程可能永远等待下去醒不过来了。

但是，注意到`wait()`方法返回时需要***重新***获得`this`锁。假设当前有3个线程被唤醒，唤醒后，首先要等待执行`addTask()`的线程结束此方法后，才能释放`this`锁，随后，这3个线程中只能有一个获取到`this`锁，剩下两个将继续等待。

再注意到我们在`while()`循环中调用`wait()`，而不是小结

`wait`和`notify`用于多线程协调运行：

- 在`synchronized`内部可以调用`wait()`使线程进入等待状态；
- 必须在已获得的锁对象上调用`wait()`方法；
- 在`synchronized`内部可以调用`notify()`或`notifyAll()`唤醒其他等待线程；
- 必须在已获得的锁对象上调用`notify()`或`notifyAll()`方法；
- 已唤醒的线程还需要重新获得锁后才能继续执行。

`if`语句：

```java
public synchronized String getTask() throws InterruptedException {
    if (queue.isEmpty()) {
        this.wait();
    }
    return queue.remove();
}
```

这种写法实际上是错误的，因为线程被唤醒时，需要再次获取`this`锁。多个线程被唤醒后，只有一个线程能获取`this`锁，此刻，该线程执行`queue.remove()`可以获取到队列的元素，然而，剩下的线程如果获取`this`锁后执行`queue.remove()`，此刻队列可能已经没有任何元素了，所以，要始终在`while`循环中`wait()`，并且每次被唤醒后拿到`this`锁就必须再次判断：

```java
while (queue.isEmpty()) {
    this.wait();
}
```

所以，正确编写多线程代码是非常困难的，需要仔细考虑的条件非常多，任何一个地方考虑不周，都会导致多线程运行时不正常。

#### 14.9.1 小结

`wait`和`notify`用于多线程协调运行：

- 在`synchronized`内部可以调用`wait()`使线程进入等待状态；
- 必须在已获得的锁对象上调用`wait()`方法；
- 在`synchronized`内部可以调用`notify()`或`notifyAll()`唤醒其他等待线程；
- 必须在已获得的锁对象上调用`notify()`或`notifyAll()`方法；
- 已唤醒的线程还需要重新获得锁后才能继续执行。

### 14.10 使用ReentrantLock

从Java 5开始，引入了一个高级的处理并发的`java.util.concurrent`包，它提供了大量更高级的并发功能，能大大简化多线程程序的编写。

我们知道Java语言直接提供了`synchronized`关键字用于加锁，但这种锁一是很重，二是获取时必须一直等待，没有额外的尝试机制。

`java.util.concurrent.locks`包提供的`ReentrantLock`用于替代`synchronized`加锁，我们来看一下传统的`synchronized`代码：

```java
public class Counter {
    private int count;

    public void add(int n) {
        synchronized(this) {
            count += n;
        }
    }
}
```

如果用`ReentrantLock`替代，可以把代码改造为：

```java
public class Counter {
    private final Lock lock = new ReentrantLock();
    private int count;

    public void add(int n) {
        lock.lock();
        try {
            count += n;
        } finally {
            lock.unlock();
        }
    }
}
```

因为`synchronized`是Java语言层面提供的语法，所以我们不需要考虑异常，而`ReentrantLock`是Java代码实现的锁，我们就必须先获取锁，然后在`finally`中正确释放锁。

顾名思义，`ReentrantLock`是可重入锁，它和`synchronized`一样，一个线程可以多次获取同一个锁。

和`synchronized`不同的是，`ReentrantLock`可以尝试获取锁：

```java
if (lock.tryLock(1, TimeUnit.SECONDS)) {
    try {
        ...
    } finally {
        lock.unlock();
    }
}
```

上述代码在尝试获取锁的时候，最多等待1秒。如果1秒后仍未获取到锁，`tryLock()`返回`false`，程序就可以做一些额外处理，而不是无限等待下去。

所以，使用`ReentrantLock`比直接使用`synchronized`更安全，线程在`tryLock()`失败的时候不会导致死锁。

#### 14.10.1 小结

- `ReentrantLock`可以替代`synchronized`进行同步；
- `ReentrantLock`获取锁更安全；
- 必须先获取到锁，再进入`try {...}`代码块，最后使用`finally`保证释放锁；
- 可以使用`tryLock()`尝试获取锁。

### 14.11 使用Condition

使用`ReentrantLock`比直接使用`synchronized`更安全，可以替代`synchronized`进行线程同步。

但是，`synchronized`可以配合`wait`和`notify`实现线程在条件不满足时等待，条件满足时唤醒，用`ReentrantLock`我们怎么编写`wait`和`notify`的功能呢？

答案是使用`Condition`对象来实现`wait`和`notify`的功能。

我们仍然以`TaskQueue`为例，把前面用`synchronized`实现的功能通过`ReentrantLock`和`Condition`来实现：

```java
class TaskQueue {
    private final Lock lock = new ReentrantLock();
    private final Condition condition = lock.newCondition();
    private Queue<String> queue = new LinkedList<>();

    public void addTask(String s) {
        lock.lock();
        try {
            queue.add(s);
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }

    public String getTask() {
        lock.lock();
        try {
            while (queue.isEmpty()) {
                condition.await();
            }
            return queue.remove();
        } finally {
            lock.unlock();
        }
    }
}
```

可见，使用`Condition`时，引用的`Condition`对象必须从`Lock`实例的`newCondition()`返回，这样才能获得一个绑定了`Lock`实例的`Condition`实例。

`Condition`提供的`await()`、`signal()`、`signalAll()`原理和`synchronized`锁对象的`wait()`、`notify()`、`notifyAll()`是一致的，并且其行为也是一样的：

- `await()`会释放当前锁，进入等待状态；
- `signal()`会唤醒某个等待线程；
- `signalAll()`会唤醒所有等待线程；
- 唤醒线程从`await()`返回后需要重新获得锁。

此外，和`tryLock()`类似，`await()`可以在等待指定时间后，如果还没有被其他线程通过`signal()`或`signalAll()`唤醒，可以自己醒来：

```java
if (condition.await(1, TimeUnit.SECOND)) {
    // 被其他线程唤醒
} else {
    // 指定时间内没有被其他线程唤醒
}
```

可见，使用`Condition`配合`Lock`，我们可以实现更灵活的线程同步。

#### 14.11.1 小结

`Condition`可以替代`wait`和`notify`；

`Condition`对象必须从`Lock`对象获取。

### 14.12 使用ReadWriteLock

前面讲到的`ReentrantLock`保证了只有一个线程可以执行临界区代码：

```java
public class Counter {
    private final Lock lock = new ReentrantLock();
    private int[] counts = new int[10];

    public void inc(int index) {
        lock.lock();
        try {
            counts[index] += 1;
        } finally {
            lock.unlock();
        }
    }

    public int[] get() {
        lock.lock();
        try {
            return Arrays.copyOf(counts, counts.length);
        } finally {
            lock.unlock();
        }
    }
}
```

但是有些时候，这种保护有点过头。因为我们发现，任何时刻，只允许一个线程修改，也就是调用`inc()`方法是必须获取锁，但是，`get()`方法只读取数据，不修改数据，它实际上允许多个线程同时调用。

实际上我们想要的是：**允许多个线程同时读，但只要有一个线程在写，其他线程就必须等待**：

|      | 读     | 写     |
| :--- | :----- | :----- |
| 读   | 允许   | 不允许 |
| 写   | 不允许 | 不允许 |

使用`ReadWriteLock`可以解决这个问题，它保证：

- 只允许一个线程写入（其他线程既不能写入也不能读取）；
- 没有写入时，多个线程允许同时读（提高性能）。

用`ReadWriteLock`实现这个功能十分容易。我们需要创建一个`ReadWriteLock`实例，然后分别获取读锁和写锁：

```java
public class Counter {
    private final ReadWriteLock rwlock = new ReentrantReadWriteLock();
    private final Lock rlock = rwlock.readLock();
    private final Lock wlock = rwlock.writeLock();
    private int[] counts = new int[10];

    public void inc(int index) {
        wlock.lock(); // 加写锁
        try {
            counts[index] += 1;
        } finally {
            wlock.unlock(); // 释放写锁
        }
    }

    public int[] get() {
        rlock.lock(); // 加读锁
        try {
            return Arrays.copyOf(counts, counts.length);
        } finally {
            rlock.unlock(); // 释放读锁
        }
    }
}
```

把读写操作分别用读锁和写锁来加锁，在读取时，多个线程可以同时获得读锁，这样就大大提高了并发读的执行效率。

使用`ReadWriteLock`时，适用条件是同一个数据，有大量线程读取，但仅有少数线程修改。

例如，一个论坛的帖子，回复可以看做写入操作，它是不频繁的，但是，浏览可以看做读取操作，是非常频繁的，这种情况就可以使用`ReadWriteLock`。

#### 14.12.1 小结

使用`ReadWriteLock`可以提高读取效率：

- `ReadWriteLock`只允许一个线程写入；
- `ReadWriteLock`允许多个线程在没有写入时同时读取；
- `ReadWriteLock`适合读多写少的场景。

### 14.13 使用StampedLock

前面介绍的`ReadWriteLock`可以解决多线程同时读，但只有一个线程能写的问题。

如果我们深入分析`ReadWriteLock`，会发现它有个潜在的问题：**如果有线程正在读，写线程需要等待读线程释放锁后才能获取写锁，即读的过程中不允许写，这是一种悲观的读锁**。

要进一步提升并发执行效率，Java 8引入了新的读写锁：`StampedLock`。

`StampedLock`和`ReadWriteLock`相比，改进之处在于：**读的过程中也允许获取写锁后写入**！这样一来，我们读的数据就可能不一致，所以，需要一点额外的代码来判断读的过程中是否有写入，这种读锁是一种乐观锁。

**乐观锁的意思就是乐观地估计读的过程中大概率不会有写入，因此被称为乐观锁**。反过来，悲观锁则是读的过程中拒绝有写入，也就是写入必须等待。显然乐观锁的并发效率更高，但一旦有小概率的写入导致读取的数据不一致，需要能检测出来，再读一遍就行。

我们来看例子：

```java
public class Point {
    private final StampedLock stampedLock = new StampedLock();

    private double x;
    private double y;

    public void move(double deltaX, double deltaY) {
        long stamp = stampedLock.writeLock(); // 获取写锁
        try {
            x += deltaX;
            y += deltaY;
        } finally {
            stampedLock.unlockWrite(stamp); // 释放写锁
        }
    }

    public double distanceFromOrigin() {
        long stamp = stampedLock.tryOptimisticRead(); // 获得一个乐观读锁
        // 注意下面两行代码不是原子操作
        // 假设x,y = (100,200)
        double currentX = x;
        // 此处已读取到x=100，但x,y可能被写线程修改为(300,400)
        double currentY = y;
        // 此处已读取到y，如果没有写入，读取是正确的(100,200)
        // 如果有写入，读取是错误的(100,400)
        if (!stampedLock.validate(stamp)) { // 检查乐观读锁后是否有其他写锁发生
            stamp = stampedLock.readLock(); // 获取一个悲观读锁
            try {
                currentX = x;
                currentY = y;
            } finally {
                stampedLock.unlockRead(stamp); // 释放悲观读锁
            }
        }
        return Math.sqrt(currentX * currentX + currentY * currentY);
    }
}
```

和`ReadWriteLock`相比，写入的加锁是完全一样的，不同的是读取。注意到首先我们通过`tryOptimisticRead()`获取一个乐观读锁，并返回版本号。接着进行读取，读取完成后，我们通过`validate()`去验证版本号，如果在读取过程中没有写入，版本号不变，验证成功，我们就可以放心地继续后续操作。如果在读取过程中有写入，版本号会发生变化，验证将失败。在失败的时候，我们再通过获取悲观读锁再次读取。由于写入的概率不高，程序在绝大部分情况下可以通过乐观读锁获取数据，极少数情况下使用悲观读锁获取数据。

可见，`StampedLock`把读锁细分为乐观读和悲观读，能进一步提升并发效率。但这也是有代价的：

- 一是代码更加复杂；
- 二是`StampedLock`是不可重入锁，不能在一个线程中反复获取同一个锁。

`StampedLock`还提供了更复杂的将悲观读锁升级为写锁的功能，它主要使用在if-then-update的场景：即先读，如果读的数据满足条件，就返回，如果读的数据不满足条件，再尝试写。

#### 14.13.1 小结

`StampedLock`提供了乐观读锁，可取代`ReadWriteLock`以进一步提升并发性能；

`StampedLock`是不可重入锁。

### 14.14 使用Concurrent集合

我们在前面已经通过`ReentrantLock`和`Condition`实现了一个`BlockingQueue`：

```java
public class TaskQueue {
    private final Lock lock = new ReentrantLock();
    private final Condition condition = lock.newCondition();
    private Queue<String> queue = new LinkedList<>();

    public void addTask(String s) {
        lock.lock();
        try {
            queue.add(s);
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }

    public String getTask() {
        lock.lock();
        try {
            while (queue.isEmpty()) {
                condition.await();
            }
            return queue.remove();
        } finally {
            lock.unlock();
        }
    }
}
```

`BlockingQueue`的意思就是说，当一个线程调用这个`TaskQueue`的`getTask()`方法时，该方法内部可能会让线程变成等待状态，直到队列条件满足不为空，线程被唤醒后，`getTask()`方法才会返回。

因为`BlockingQueue`非常有用，所以我们不必自己编写，可以直接使用Java标准库的`java.util.concurrent`包提供的线程安全的集合：`ArrayBlockingQueue`。

除了`BlockingQueue`外，针对`List`、`Map`、`Set`、`Deque`等，`java.util.concurrent`包也提供了对应的并发集合类。我们归纳一下：

| interface | non-thread-safe         | thread-safe                              |
| :-------- | :---------------------- | :--------------------------------------- |
| List      | ArrayList               | CopyOnWriteArrayList                     |
| Map       | HashMap                 | ConcurrentHashMap                        |
| Set       | HashSet / TreeSet       | CopyOnWriteArraySet                      |
| Queue     | ArrayDeque / LinkedList | ArrayBlockingQueue / LinkedBlockingQueue |
| Deque     | ArrayDeque / LinkedList | LinkedBlockingDeque                      |

使用这些并发集合与使用非线程安全的集合类完全相同。我们以`ConcurrentHashMap`为例：

```java
Map<String, String> map = ConcurrentHashMap<>();
// 在不同的线程读写:
map.put("A", "1");
map.put("B", "2");
map.get("A", "1");
```

因为所有的同步和加锁的逻辑都在集合内部实现，对外部调用者来说，只需要正常按接口引用，其他代码和原来的非线程安全代码完全一样。即当我们需要多线程访问时，把：

```java
Map<String, String> map = HashMap<>();
```

改为：

```java
Map<String, String> map = ConcurrentHashMap<>();
```

就可以了。

`java.util.Collections`工具类还提供了一个旧的线程安全集合转换器，可以这么用：

```java
Map unsafeMap = new HashMap();
Map threadSafeMap = Collections.synchronizedMap(unsafeMap);
```

但是它实际上是用一个包装类包装了非线程安全的`Map`，然后对所有读写方法都用`synchronized`加锁，这样获得的线程安全集合的性能比`java.util.concurrent`集合要低很多，所以不推荐使用。

#### 14.14.1 小结

使用`java.util.concurrent`包提供的线程安全的并发集合可以大大简化多线程编程：

多线程同时读写并发集合是安全的；

尽量使用Java标准库提供的并发集合，避免自己编写同步代码。

### 14.15 使用Atomic

Java的`java.util.concurrent`包除了提供底层锁、并发集合外，还提供了一组原子操作的封装类，它们位于`java.util.concurrent.atomic`包。

我们以`AtomicInteger`为例，它提供的主要操作有：

- 增加值并返回新值：`int addAndGet(int delta)`
- 加1后返回新值：`int incrementAndGet()`
- 获取当前值：`int get()`
- 用CAS方式设置：`int compareAndSet(int expect, int update)`

Atomic类是通过无锁（lock-free）的方式实现的线程安全（thread-safe）访问。它的主要原理是利用了CAS：Compare and Set。

如果我们自己通过CAS编写`incrementAndGet()`，它大概长这样：

```java
public int incrementAndGet(AtomicInteger var) {
    int prev, next;
    do {
        prev = var.get();
        next = prev + 1;
    } while ( ! var.compareAndSet(prev, next));
    return prev;
}
```

CAS是指，在这个操作中，如果`AtomicInteger`的当前值是`prev`，那么就更新为`next`，返回`true`。如果`AtomicInteger`的当前值不是`prev`，就什么也不干，返回`false`。通过CAS操作并配合`do ... while`循环，即使其他线程修改了`AtomicInteger`的值，最终的结果也是正确的。

我们利用`AtomicLong`可以编写一个多线程安全的全局唯一ID生成器：

```java
class IdGenerator {
    AtomicLong var = new AtomicLong(0);

    public long getNextId() {
        return var.incrementAndGet();
    }
}
```

通常情况下，我们并不需要直接用`do ... while`循环调用`compareAndSet`实现复杂的并发操作，而是用`incrementAndGet()`这样的封装好的方法，因此，使用起来非常简单。

在高度竞争的情况下，还可以使用Java 8提供的`LongAdder`和`LongAccumulator`。

#### 14.15.1 小结

使用`java.util.concurrent.atomic`提供的原子操作可以简化多线程编程：

- 原子操作实现了无锁的线程安全；
- 适用于计数器，累加器等。

### 14.16 使用线程池

Java语言虽然内置了多线程支持，启动一个新线程非常方便，但是，创建线程需要操作系统资源（线程资源，栈空间等），频繁创建和销毁大量线程需要消耗大量时间。如果可以复用一组线程：

```ascii
┌─────┐ execute  ┌──────────────────┐
│Task1│─────────>│ThreadPool        │
├─────┤          │┌───────┐┌───────┐│
│Task2│          ││Thread1││Thread2││
├─────┤          │└───────┘└───────┘│
│Task3│          │┌───────┐┌───────┐│
├─────┤          ││Thread3││Thread4││
│Task4│          │└───────┘└───────┘│
├─────┤          └──────────────────┘
│Task5│
├─────┤
│Task6│
└─────┘
  ...
```

那么我们就可以把很多小任务让一组线程来执行，而不是一个任务对应一个新线程。这种能接收大量小任务并进行分发处理的就是线程池。

简单地说，线程池内部维护了若干个线程，没有任务的时候，这些线程都处于等待状态。如果有新任务，就分配一个空闲线程执行。如果所有线程都处于忙碌状态，新任务要么放入队列等待，要么增加一个新线程进行处理。

Java标准库提供了`ExecutorService`接口表示线程池，它的典型用法如下：

```java
// 创建固定大小的线程池:
ExecutorService executor = Executors.newFixedThreadPool(3);
// 提交任务:
executor.submit(task1);
executor.submit(task2);
executor.submit(task3);
executor.submit(task4);
executor.submit(task5);
```

因为`ExecutorService`只是接口，Java标准库提供的几个常用实现类有：

- FixedThreadPool：线程数固定的线程池；
- CachedThreadPool：线程数根据任务动态调整的线程池；
- SingleThreadExecutor：仅单线程执行的线程池。

创建这些线程池的方法都被封装到`Executors`这个类中。我们以`FixedThreadPool`为例，看看线程池的执行逻辑：

```java
// thread-pool
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) {
        // 创建一个固定大小的线程池:
        ExecutorService es = Executors.newFixedThreadPool(4);
        for (int i = 0; i < 6; i++) {
            es.submit(new Task("" + i));
        }
        // 关闭线程池:
        es.shutdown();
    }
}

class Task implements Runnable {
    private final String name;

    public Task(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        System.out.println("start task " + name);
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
        System.out.println("end task " + name);
    }
}
```

我们观察执行结果，一次性放入6个任务，由于线程池只有固定的4个线程，因此，前4个任务会同时执行，等到有线程空闲后，才会执行后面的两个任务。

线程池在程序结束的时候要关闭。使用`shutdown()`方法关闭线程池的时候，它会等待正在执行的任务先完成，然后再关闭。`shutdownNow()`会立刻停止正在执行的任务，`awaitTermination()`则会等待指定的时间让线程池关闭。

如果我们把线程池改为`CachedThreadPool`，由于这个线程池的实现会根据任务数量动态调整线程池的大小，所以6个任务可一次性全部同时执行。

如果我们想把线程池的大小限制在4～10个之间动态调整怎么办？我们查看`Executors.newCachedThreadPool()`方法的源码：

```java
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                    60L, TimeUnit.SECONDS,
                                    new SynchronousQueue<Runnable>());
}
```

因此，想创建指定动态范围的线程池，可以这么写：

```java
int min = 4;
int max = 10;
ExecutorService es = new ThreadPoolExecutor(min, max,
        60L, TimeUnit.SECONDS, new SynchronousQueue<Runnable>());
```

#### 14.16.1 ScheduledThreadPool

还有一种任务，需要定期反复执行，例如，每秒刷新证券价格。这种任务本身固定，需要反复执行的，可以使用`ScheduledThreadPool`。放入`ScheduledThreadPool`的任务可以定期反复执行。

创建一个`ScheduledThreadPool`仍然是通过`Executors`类：

```java
ScheduledExecutorService ses = Executors.newScheduledThreadPool(4);
```

我们可以提交一次性任务，它会在指定延迟后只执行一次：

```java
// 1秒后执行一次性任务:
ses.schedule(new Task("one-time"), 1, TimeUnit.SECONDS);
```

如果任务以固定的每3秒执行，我们可以这样写：

```java
// 2秒后开始执行定时任务，每3秒执行:
ses.scheduleAtFixedRate(new Task("fixed-rate"), 2, 3, TimeUnit.SECONDS);
```

如果任务以固定的3秒为间隔执行，我们可以这样写：

```java
// 3秒后开始执行定时任务，以3秒为间隔执行:
ses.scheduleWithFixedDelay(new Task("fixed-delay"), 2, 3, TimeUnit.SECONDS);
```

注意FixedRate和FixedDelay的区别。FixedRate是指任务总是以固定时间间隔触发，不管任务执行多长时间：

```ascii
│░░░░   │░░░░░░ │░░░    │░░░░░  │░░░  
├───────┼───────┼───────┼───────┼────>
│<─────>│<─────>│<─────>│<─────>│
```

而FixedDelay是指，上一次任务执行完毕后，等待固定的时间间隔，再执行下一次任务：

```ascii
│░░░│       │░░░░░│       │░░│       │░
└───┼───────┼─────┼───────┼──┼───────┼──>
    │<─────>│     │<─────>│  │<─────>│
```

因此，使用`ScheduledThreadPool`时，我们要根据需要选择执行一次、FixedRate执行还是FixedDelay执行。

细心的童鞋还可以思考下面的问题：

- 在FixedRate模式下，假设每秒触发，如果某次任务执行时间超过1秒，后续任务会不会并发执行？
- 如果任务抛出了异常，后续任务是否继续执行？

Java标准库还提供了一个`java.util.Timer`类，这个类也可以定期执行任务，但是，一个`Timer`会对应一个`Thread`，所以，一个`Timer`只能定期执行一个任务，多个定时任务必须启动多个`Timer`，而一个`ScheduledThreadPool`就可以调度多个定时任务，所以，我们完全可以用`ScheduledThreadPool`取代旧的`Timer`。

#### 14.16.2 小结

JDK提供了`ExecutorService`实现了线程池功能：

- 线程池内部维护一组线程，可以高效执行大量小任务；
- `Executors`提供了静态方法创建不同类型的`ExecutorService`；
- 必须调用`shutdown()`关闭`ExecutorService`；
- `ScheduledThreadPool`可以定期调度多个任务。

### 14.17 使用Future

在执行多个任务的时候，使用Java标准库提供的线程池是非常方便的。我们提交的任务只需要实现`Runnable`接口，就可以让线程池去执行：

```java
class Task implements Runnable {
    public String result;

    public void run() {
        this.result = longTimeCalculation();
    }
}
```

`Runnable`接口有个问题，它的方法没有返回值。如果任务需要一个返回结果，那么只能保存到变量，还要提供额外的方法读取，非常不便。所以，Java标准库还提供了一个`Callable`接口，和`Runnable`接口比，它多了一个返回值：

```java
class Task implements Callable<String> {
    public String call() throws Exception {
        return longTimeCalculation();
    }
}
```

并且`Callable`接口是一个泛型接口，可以返回指定类型的结果。

现在的问题是，如何获得异步执行的结果？

如果仔细看`ExecutorService.submit()`方法，可以看到，它返回了一个`Future`类型，一个`Future`类型的实例代表一个未来能获取结果的对象：

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
// 定义任务:
Callable<String> task = new Task();
// 提交任务并获得Future:
Future<String> future = executor.submit(task);
// 从Future获取异步执行返回的结果:
String result = future.get(); // 可能阻塞
```

当我们提交一个`Callable`任务后，我们会同时获得一个`Future`对象，然后，我们在主线程某个时刻调用`Future`对象的`get()`方法，就可以获得异步执行的结果。在调用`get()`时，如果异步任务已经完成，我们就直接获得结果。如果异步任务还没有完成，那么`get()`会阻塞，直到任务完成后才返回结果。

一个`Future`接口表示一个未来可能会返回的结果，它定义的方法有：

- `get()`：获取结果（可能会等待）
- `get(long timeout, TimeUnit unit)`：获取结果，但只等待指定的时间；
- `cancel(boolean mayInterruptIfRunning)`：取消当前任务；
- `isDone()`：判断任务是否已完成。

#### 14.17.1 小结

- 对线程池提交一个`Callable`任务，可以获得一个`Future`对象；

- 可以用`Future`在将来某个时刻获取结果。

### 14.18 使用CompletableFuture

  使用`Future`获得异步执行结果时，要么调用阻塞方法`get()`，要么轮询看`isDone()`是否为`true`，这两种方法都不是很好，因为主线程也会被迫等待。

从Java 8开始引入了`CompletableFuture`，它针对`Future`做了改进，可以传入回调对象，当异步任务完成或者发生异常时，自动调用回调对象的回调方法。

我们以获取股票价格为例，看看如何使用`CompletableFuture`：

```java
// CompletableFuture
import java.util.concurrent.CompletableFuture;
public class Main {
    public static void main(String[] args) throws Exception {
        // 创建异步执行任务:
        CompletableFuture<Double> cf = CompletableFuture.supplyAsync(Main::fetchPrice);
        // 如果执行成功:
        cf.thenAccept((result) -> {
            System.out.println("price: " + result);
        });
        // 如果执行异常:
        cf.exceptionally((e) -> {
            e.printStackTrace();
            return null;
        });
        // 主线程不要立刻结束，否则CompletableFuture默认使用的线程池会立刻关闭:
        Thread.sleep(2000);
    }

    static Double fetchPrice() {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
        if (Math.random() < 0.3) {
            throw new RuntimeException("fetch price failed!");
        }
        return 5 + Math.random() * 20;
    }
}
```

创建一个`CompletableFuture`是通过`CompletableFuture.supplyAsync()`实现的，它需要一个实现了`Supplier`接口的对象：

```java
public interface Supplier<T> {
    T get();
}
```

这里我们用lambda语法简化了一下，直接传入`Main::fetchPrice`，因为`Main.fetchPrice()`静态方法的签名符合`Supplier`接口的定义（除了方法名外）。

紧接着，`CompletableFuture`已经被提交给默认的线程池执行了，我们需要定义的是`CompletableFuture`完成时和异常时需要回调的实例。完成时，`CompletableFuture`会调用`Consumer`对象：

```java
public interface Consumer<T> {
    void accept(T t);
}
```

异常时，`CompletableFuture`会调用`Function`对象：

```java
public interface Function<T, R> {
    R apply(T t);
}
```

这里我们都用lambda语法简化了代码。

可见`CompletableFuture`的优点是：

- 异步任务结束时，会自动回调某个对象的方法；
- 异步任务出错时，会自动回调某个对象的方法；
- 主线程设置好回调后，不再关心异步任务的执行。

如果只是实现了异步回调机制，我们还看不出`CompletableFuture`相比`Future`的优势。`CompletableFuture`更强大的功能是，多个`CompletableFuture`可以串行执行，例如，定义两个`CompletableFuture`，第一个`CompletableFuture`根据证券名称查询证券代码，第二个`CompletableFuture`根据证券代码查询证券价格，这两个`CompletableFuture`实现串行操作如下：

```java
// CompletableFuture
import java.util.concurrent.CompletableFuture;
public class Main {
    public static void main(String[] args) throws Exception {
        // 第一个任务:
        CompletableFuture<String> cfQuery = CompletableFuture.supplyAsync(() -> {
            return queryCode("中国石油");
        });
        // cfQuery成功后继续执行下一个任务:
        CompletableFuture<Double> cfFetch = cfQuery.thenApplyAsync((code) -> {
            return fetchPrice(code);
        });
        // cfFetch成功后打印结果:
        cfFetch.thenAccept((result) -> {
            System.out.println("price: " + result);
        });
        // 主线程不要立刻结束，否则CompletableFuture默认使用的线程池会立刻关闭:
        Thread.sleep(2000);
    }

    static String queryCode(String name) {
        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
        }
        return "601857";
    }

    static Double fetchPrice(String code) {
        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
        }
        return 5 + Math.random() * 20;
    }
}
```

除了串行执行外，多个`CompletableFuture`还可以并行执行。例如，我们考虑这样的场景：

同时从新浪和网易查询证券代码，只要任意一个返回结果，就进行下一步查询价格，查询价格也同时从新浪和网易查询，只要任意一个返回结果，就完成操作：

```java
// CompletableFuture
import java.util.concurrent.CompletableFuture;
public class Main {
    public static void main(String[] args) throws Exception {
        // 两个CompletableFuture执行异步查询:
        CompletableFuture<String> cfQueryFromSina = CompletableFuture.supplyAsync(() -> {
            return queryCode("中国石油", "https://finance.sina.com.cn/code/");
        });
        CompletableFuture<String> cfQueryFrom163 = CompletableFuture.supplyAsync(() -> {
            return queryCode("中国石油", "https://money.163.com/code/");
        });

        // 用anyOf合并为一个新的CompletableFuture:
        CompletableFuture<Object> cfQuery = CompletableFuture.anyOf(cfQueryFromSina, cfQueryFrom163);

        // 两个CompletableFuture执行异步查询:
        CompletableFuture<Double> cfFetchFromSina = cfQuery.thenApplyAsync((code) -> {
            return fetchPrice((String) code, "https://finance.sina.com.cn/price/");
        });
        CompletableFuture<Double> cfFetchFrom163 = cfQuery.thenApplyAsync((code) -> {
            return fetchPrice((String) code, "https://money.163.com/price/");
        });

        // 用anyOf合并为一个新的CompletableFuture:
        CompletableFuture<Object> cfFetch = CompletableFuture.anyOf(cfFetchFromSina, cfFetchFrom163);

        // 最终结果:
        cfFetch.thenAccept((result) -> {
            System.out.println("price: " + result);
        });
        // 主线程不要立刻结束，否则CompletableFuture默认使用的线程池会立刻关闭:
        Thread.sleep(2000);
    }

    static String queryCode(String name, String url) {
        System.out.println("query code from " + url + "...");
        try {
            Thread.sleep((long) Math.random() * 1000);
        } catch (InterruptedException e) {
        }
        return "601857";
    }

    static Double fetchPrice(String code, String url) {
        System.out.println("query price from " + url + "...");
        try {
            Thread.sleep((long) Math.random() * 1000);
        } catch (InterruptedException e) {
        }
        return 5 + Math.random() * 20;
    }
}
```

上述逻辑实现的异步查询规则实际上是：

```ascii
┌─────────────┐ ┌─────────────┐
│ Query Code  │ │ Query Code  │
│  from sina  │ │  from 163   │
└─────────────┘ └─────────────┘
       │               │
       └───────┬───────┘
               ▼
        ┌─────────────┐
        │    anyOf    │
        └─────────────┘
               │
       ┌───────┴────────┐
       ▼                ▼
┌─────────────┐  ┌─────────────┐
│ Query Price │  │ Query Price │
│  from sina  │  │  from 163   │
└─────────────┘  └─────────────┘
       │                │
       └────────┬───────┘
                ▼
         ┌─────────────┐
         │    anyOf    │
         └─────────────┘
                │
                ▼
         ┌─────────────┐
         │Display Price│
         └─────────────┘
```

除了`anyOf()`可以实现“任意个`CompletableFuture`只要一个成功”，`allOf()`可以实现“所有`CompletableFuture`都必须成功”，这些组合操作可以实现非常复杂的异步流程控制。

最后我们注意`CompletableFuture`的命名规则：

- `xxx()`：表示该方法将继续在已有的线程中执行；
- `xxxAsync()`：表示将异步在线程池中执行。

#### 14.18.1 小结

`CompletableFuture`可以指定异步处理流程：

- `thenAccept()`处理正常结果；
- `exceptional()`处理异常结果；
- `thenApplyAsync()`用于串行化另一个`CompletableFuture`；
- `anyOf()`和`allOf()`用于并行化多个`CompletableFuture`。

### 14.19 使用ForkJoin

Java 7开始引入了一种新的Fork/Join线程池，它可以执行一种特殊的任务：把一个大任务拆成多个小任务并行执行。

我们举个例子：如果要计算一个超大数组的和，最简单的做法是用一个循环在一个线程内完成：

```ascii
┌─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┐
└─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┘
```

还有一种方法，可以把数组拆成两部分，分别计算，最后加起来就是最终结果，这样可以用两个线程并行执行：

```ascii
┌─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┐
└─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┘
┌─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┐
└─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┘
```

如果拆成两部分还是很大，我们还可以继续拆，用4个线程并行执行：

```ascii
┌─┬─┬─┬─┬─┬─┐
└─┴─┴─┴─┴─┴─┘
┌─┬─┬─┬─┬─┬─┐
└─┴─┴─┴─┴─┴─┘
┌─┬─┬─┬─┬─┬─┐
└─┴─┴─┴─┴─┴─┘
┌─┬─┬─┬─┬─┬─┐
└─┴─┴─┴─┴─┴─┘
```

这就是Fork/Join任务的原理：判断一个任务是否足够小，如果是，直接计算，否则，就分拆成几个小任务分别计算。这个过程可以反复“裂变”成一系列小任务。

我们来看如何使用Fork/Join对大数据进行并行求和：

```java
import java.util.Random;
import java.util.concurrent.*;
public class Main {
    public static void main(String[] args) throws Exception {
        // 创建2000个随机数组成的数组:
        long[] array = new long[2000];
        long expectedSum = 0;
        for (int i = 0; i < array.length; i++) {
            array[i] = random();
            expectedSum += array[i];
        }
        System.out.println("Expected sum: " + expectedSum);
        // fork/join:
        ForkJoinTask<Long> task = new SumTask(array, 0, array.length);
        long startTime = System.currentTimeMillis();
        Long result = ForkJoinPool.commonPool().invoke(task);
        long endTime = System.currentTimeMillis();
        System.out.println("Fork/join sum: " + result + " in " + (endTime - startTime) + " ms.");
    }

    static Random random = new Random(0);

    static long random() {
        return random.nextInt(10000);
    }
}

class SumTask extends RecursiveTask<Long> {
    static final int THRESHOLD = 500;
    long[] array;
    int start;
    int end;

    SumTask(long[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }

    @Override
    protected Long compute() {
        if (end - start <= THRESHOLD) {
            // 如果任务足够小,直接计算:
            long sum = 0;
            for (int i = start; i < end; i++) {
                sum += this.array[i];
                // 故意放慢计算速度:
                try {
                    Thread.sleep(1);
                } catch (InterruptedException e) {
                }
            }
            return sum;
        }
        // 任务太大,一分为二:
        int middle = (end + start) / 2;
        System.out.println(String.format("split %d~%d ==> %d~%d, %d~%d", start, end, start, middle, middle, end));
        SumTask subtask1 = new SumTask(this.array, start, middle);
        SumTask subtask2 = new SumTask(this.array, middle, end);
        invokeAll(subtask1, subtask2);
        Long subresult1 = subtask1.join();
        Long subresult2 = subtask2.join();
        Long result = subresult1 + subresult2;
        System.out.println("result = " + subresult1 + " + " + subresult2 + " ==> " + result);
        return result;
    }
}
```

观察上述代码的执行过程，一个大的计算任务0~2000首先分裂为两个小任务0~1000和1000~2000，这两个小任务仍然太大，继续分裂为更小的0~500，500~1000，1000~1500，1500~2000，最后，计算结果被依次合并，得到最终结果。

因此，核心代码`SumTask`继承自`RecursiveTask`，在`compute()`方法中，关键是如何“分裂”出子任务并且提交子任务：

```java
class SumTask extends RecursiveTask<Long> {
    protected Long compute() {
        // “分裂”子任务:
        SumTask subtask1 = new SumTask(...);
        SumTask subtask2 = new SumTask(...);
        // invokeAll会并行运行两个子任务:
        invokeAll(subtask1, subtask2);
        // 获得子任务的结果:
        Long result1 = fork1.join();
        Long result2 = fork2.join();
        // 汇总结果:
        return result1 + result2;
    }
}
```

Fork/Join线程池在Java标准库中就有应用。Java标准库提供的`java.util.Arrays.parallelSort(array)`可以进行并行排序，它的原理就是内部通过Fork/Join对大数组分拆进行并行排序，在多核CPU上就可以大大提高排序的速度。

#### 14.19.1 小结

Fork/Join是一种基于“分治”的算法：通过分解任务，并行执行，最后合并结果得到最终结果。

`ForkJoinPool`线程池可以把一个大任务分拆成小任务并行执行，任务类必须继承自`RecursiveTask`或`RecursiveAction`。

使用Fork/Join模式可以进行并行计算以提高效率。

### 14.20 使用ThreadLocal

多线程是Java实现多任务的基础，`Thread`对象代表一个线程，我们可以在代码中调用`Thread.currentThread()`获取当前线程。例如，打印日志时，可以同时打印出当前线程的名字：

```java
// Thread
public class Main {
    public static void main(String[] args) throws Exception {
        log("start main...");
        new Thread(() -> {
            log("run task...");
        }).start();
        new Thread(() -> {
            log("print...");
        }).start();
        log("end main.");
    }

    static void log(String s) {
        System.out.println(Thread.currentThread().getName() + ": " + s);
    }
}
```

对于多任务，Java标准库提供的线程池可以方便地执行这些任务，同时复用线程。Web应用程序就是典型的多任务应用，每个用户请求页面时，我们都会创建一个任务，类似：

```java
public void process(User user) {
    checkPermission();
    doWork();
    saveStatus();
    sendResponse();
}
```

然后，通过线程池去执行这些任务。

观察`process()`方法，它内部需要调用若干其他方法，同时，我们遇到一个问题：如何在一个线程内传递状态？

`process()`方法需要传递的状态就是`User`实例。有的童鞋会想，简单地传入`User`就可以了：

```java
public void process(User user) {
    checkPermission(user);
    doWork(user);
    saveStatus(user);
    sendResponse(user);
}
```

但是往往一个方法又会调用其他很多方法，这样会导致`User`传递到所有地方：

```java
void doWork(User user) {
    queryStatus(user);
    checkStatus();
    setNewStatus(user);
    log();
}
```

这种在一个线程中，横跨若干方法调用，需要传递的对象，我们通常称之为上下文（Context），它是一种状态，可以是用户身份、任务信息等。给每个方法增加一个context参数非常麻烦，而且有些时候，如果调用链有无法修改源码的第三方库，`User`对象就传不进去了。Java标准库提供了一个特殊的`ThreadLocal`，它可以在一个线程中传递同一个对象。

`ThreadLocal`实例通常总是以静态字段初始化如下：

```java
static ThreadLocal<String> threadLocalUser = new ThreadLocal<>();
```

它的典型使用方式如下：

```java
void processUser(user) {
    try {
        threadLocalUser.set(user);
        step1();
        step2();
    } finally {
        threadLocalUser.remove();
    }
}
```

通过设置一个`User`实例关联到`ThreadLocal`中，在移除之前，所有方法都可以随时获取到该`User`实例：

```java
void step1() {
    User u = threadLocalUser.get();
    log();
    printUser();
}

void log() {
    User u = threadLocalUser.get();
    println(u.name);
}

void step2() {
    User u = threadLocalUser.get();
    checkUser(u.id);
}
```

注意到普通的方法调用一定是同一个线程执行的，所以，`step1()`、`step2()`以及`log()`方法内，`threadLocalUser.get()`获取的`User`对象是同一个实例。

实际上，可以把`ThreadLocal`看成一个全局`Map`：每个线程获取`ThreadLocal`变量时，总是使用`Thread`自身作为key：

```java
Object threadLocalValue = threadLocalMap.get(Thread.currentThread());
```

因此，`ThreadLocal`相当于给每个线程都开辟了一个独立的存储空间，各个线程的`ThreadLocal`关联的实例互不干扰。

最后，特别注意`ThreadLocal`一定要在`finally`中清除：

```java
try {
    threadLocalUser.set(user);
    ...
} finally {
    threadLocalUser.remove();
}
```

这是因为当前线程执行完相关代码后，很可能会被重新放入线程池中，如果`ThreadLocal`没有被清除，该线程执行其他代码时，会把上一次的状态带进去。

为了保证能释放`ThreadLocal`关联的实例，我们可以通过`AutoCloseable`接口配合`try (resource) {...}`结构，让编译器自动为我们关闭。例如，一个保存了当前用户名的`ThreadLocal`可以封装为一个`UserContext`对象：

```java
public class UserContext implements AutoCloseable {

    static final ThreadLocal<String> ctx = new ThreadLocal<>();

    public UserContext(String user) {
        ctx.set(user);
    }

    public static String currentUser() {
        return ctx.get();
    }

    @Override
    public void close() {
        ctx.remove();
    }
}
```

使用的时候，我们借助`try (resource) {...}`结构，可以这么写：

```java
try (var ctx = new UserContext("Bob")) {
    // 可任意调用UserContext.currentUser():
    String currentUser = UserContext.currentUser();
} // 在此自动调用UserContext.close()方法释放ThreadLocal关联对象
```

这样就在`UserContext`中完全封装了`ThreadLocal`，外部代码在`try (resource) {...}`内部可以随时调用`UserContext.currentUser()`获取当前线程绑定的用户名。

#### 14.20.1 小结

`ThreadLocal`表示线程的“局部变量”，它确保每个线程的`ThreadLocal`变量都是各自独立的；

`ThreadLocal`适合在一个线程的处理流程中保持上下文（避免了同一参数在所有方法中传递）；

使用`ThreadLocal`要用`try ... finally`结构，并在`finally`中清除。
