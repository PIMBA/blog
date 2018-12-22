---
title: JDK 各版本差异
date: 2018-05-25 21:21:21
tags:
---
> 本文摘抄自[这篇文章](https://blog.csdn.net/bigtree_3721/article/details/74375289)

# 1.5
1. 自动装箱与拆箱：

自动装箱的过程：每当需要一种类型的对象时，这种基本类型就自动地封装到与它相同类型的包装中。
自动拆箱的过程：每当需要一个值时，被装箱对象中的值就被自动地提取出来，没必要再去调用intValue()和doubleValue()方法。
自动装箱，只需将该值赋给一个类型包装器引用，java会自动创建一个对象。
自动拆箱，只需将该对象值赋给一个基本类型即可。
java——类的包装器
类型包装器有：Double,Float,Long,Integer,Short,Character和Boolean
 
2. 枚举

把集合里的对象元素一个一个提取出来。枚举类型使代码更具可读性，理解清晰，易于维护。枚举类型是强类型的，从而保证了系统安全性。而以类的静态字段实现的类似替代模型，不具有枚举的简单性和类型安全性。
简单的用法：JavaEnum简单的用法一般用于代表一组常用常量，可用来代表一类相同类型的常量值。
复杂用法：Java为枚举类型提供了一些内置的方法，同事枚举常量还可以有自己的方法。可以很方便的遍历枚举对象。
 
3. 静态导入

通过使用 import static，就可以不用指定 Constants 类名而直接使用静态成员，包括静态方法。
import xxxx 和 import static xxxx的区别是前者一般导入的是类文件如import java.util.Scanner;后者一般是导入静态的方法，import static java.lang.System.out。
 
4. 可变参数（Varargs）

可变参数的简单语法格式为：
methodName([argumentList], dataType...argumentName);
 
5. 内省（Introspector）

是 Java语言对Bean类属性、事件的一种缺省处理方法。例如类A中有属性name,那我们可以通过getName,setName来得到其值或者设置新 的值。通过getName/setName来访问name属性，这就是默认的规则。Java中提供了一套API用来访问某个属性的getter /setter方法，通过这些API可以使你不需要了解这个规则（但你最好还是要搞清楚），这些API存放于包java.beans中。
一 般的做法是通过类Introspector来获取某个对象的BeanInfo信息，然后通过BeanInfo来获取属性的描述器 （PropertyDescriptor），通过这个属性描述器就可以获取某个属性对应的getter/setter方法，然后我们就可以通过反射机制来 调用这些方法。
 
6. 泛型(Generic) 

C++ 通过模板技术可以指定集合的元素类型，而Java在1.5之前一直没有相对应的功能。一个集合可以放任何类型的对象，相应地从集合里面拿对象的时候我们也 不得不对他们进行强制得类型转换。猛虎引入了泛型，它允许指定集合里元素的类型，这样你可以得到强类型在编译时刻进行类型检查的好处。
 
7. For-Each循环 

For-Each循环得加入简化了集合的遍历。假设我们要遍历一个集合对其中的元素进行一些处理。

8. JUC

ConcurrentHashMap(简称CHM)是在Java 1.5作为Hashtable的替代选择新引入的,是concurrent包的重要成员。

# 1.6
有关JDK1.6的新特性reamerit的博客文章已经说的很详细了。
 
1. Desktop类和SystemTray类 

在JDK6中 ,AWT新增加了两个类:Desktop和SystemTray。 
前者可以用来打开系统默认浏览器浏览指定的URL,打开系统默认邮件客户端给指定的邮箱发邮件,用默认应用程序打开或编辑文件(比如,用记事本打开以txt为后缀名的文件),用系统默认的打印机打印文档;后者可以用来在系统托盘区创建一个托盘程序. 

2. 使用JAXB2来实现对象与XML之间的映射 

JAXB是Java Architecture for XML Binding的缩写，可以将一个Java对象转变成为XML格式，反之亦然。我们把对象与关系数据库之间的映射称为ORM, 其实也可以把对象与XML之间的映射称为OXM(Object XML Mapping). 原来JAXB是Java EE的一部分，在JDK6中，SUN将其放到了Java SE中，这也是SUN的一贯做法。JDK6中自带的这个JAXB版本是2.0, 比起1.0(JSR 31)来，JAXB2(JSR 222)用JDK5的新特性Annotation来标识要作绑定的类和属性等，这就极大简化了开发的工作量。 实际上，在Java EE 5.0中，EJB和Web Services也通过Annotation来简化开发工作。另外,JAXB2在底层是用StAX(JSR 173)来处理XML文档。除了JAXB之外，我们还可以通过XMLBeans和Castor等来实现同样的功能。 
 
3. 理解StAX 

StAX(JSR 173)是JDK6.0中除了DOM和SAX之外的又一种处理XML文档的API。 
StAX 的来历 ：在JAXP1.3(JSR 206)有两种处理XML文档的方法:DOM(Document Object Model)和SAX(Simple API for XML). 
由 于JDK6.0中的JAXB2(JSR 222)和JAX-WS 2.0(JSR 224)都会用到StAX所以Sun决定把StAX加入到JAXP家族当中来，并将JAXP的版本升级到1.4(JAXP1.4是JAXP1.3的维护版 本). JDK6里面JAXP的版本就是1.4. 。 
StAX是The Streaming API for XML的缩写，一种利用拉模式解析(pull-parsing)XML文档的API.StAX通过提供一种基于事件迭代器(Iterator)的API让 程序员去控制xml文档解析过程,程序遍历这个事件迭代器去处理每一个解析事件，解析事件可以看做是程序拉出来的，也就是程序促使解析器产生一个解析事件 然后处理该事件，之后又促使解析器产生下一个解析事件，如此循环直到碰到文档结束符； 
SAX也是基于事件处理xml文档，但却 是用推模式解析，解析器解析完整个xml文档后，才产生解析事件，然后推给程序去处理这些事件；DOM 采用的方式是将整个xml文档映射到一颗内存树，这样就可以很容易地得到父节点和子结点以及兄弟节点的数据，但如果文档很大，将会严重影响性能。 
 
4. 使用Compiler API 

现在我们可以用JDK6 的Compiler API(JSR 199)去动态编译Java源文件，Compiler API结合反射功能就可以实现动态的产生Java代码并编译执行这些代码，有点动态语言的特征。 
这个特性对于某些需要用到动态编译的应用程序相当有用， 比如JSP Web Server，当我们手动修改JSP后，是不希望需要重启Web Server才可以看到效果的，这时候我们就可以用Compiler API来实现动态编译JSP文件，当然，现在的JSP Web Server也是支持JSP热部署的，现在的JSP Web Server通过在运行期间通过Runtime.exec或ProcessBuilder来调用javac来编译代码，这种方式需要我们产生另一个进程去 做编译工作，不够优雅而且容易使代码依赖与特定的操作系统；Compiler API通过一套易用的标准的API提供了更加丰富的方式去做动态编译,而且是跨平台的。 
 
5. 轻量级Http Server API 
 
JDK6 提供了一个简单的Http Server API,据此我们可以构建自己的嵌入式Http Server,它支持Http和Https协议,提供了HTTP1.1的部分实现，没有被实现的那部分可以通过扩展已有的Http Server API来实现,程序员必须自己实现HttpHandler接口,HttpServer会调用HttpHandler实现类的回调方法来处理客户端请求,在 这里,我们把一个Http请求和它的响应称为一个交换,包装成HttpExchange类,HttpServer负责将HttpExchange传给 HttpHandler实现类的回调方法. 
 
6. 插入式注解处理API(Pluggable Annotation Processing API) 
 
插入式注解处理API(JSR 269)提供一套标准API来处理Annotations(JSR 175) 
实 际上JSR 269不仅仅用来处理Annotation,我觉得更强大的功能是它建立了Java 语言本身的一个模型,它把method, package, constructor, type, variable, enum, annotation等Java语言元素映射为Types和Elements(两者有什么区别?), 从而将Java语言的语义映射成为对象, 我们可以在javax.lang.model包下面可以看到这些类. 所以我们可以利用JSR 269提供的API来构建一个功能丰富的元编程(metaprogramming)环境. 
JSR 269用Annotation Processor在编译期间而不是运行期间处理Annotation, Annotation Processor相当于编译器的一个插件,所以称为插入式注解处理.如果Annotation Processor处理Annotation时(执行process方法)产生了新的Java代码,编译器会再调用一次Annotation Processor,如果第二次处理还有新代码产生,就会接着调用Annotation Processor,直到没有新代码产生为止.每执行一次process()方法被称为一个"round",这样整个Annotation processing过程可以看作是一个round的序列. 
JSR 269主要被设计成为针对Tools或者容器的API. 举个例子,我们想建立一套基于Annotation的单元测试框架(如TestNG),在测试类里面用Annotation来标识测试期间需要执行的测试方法。 
 
7. 用Console开发控制台程序 
 
JDK6 中提供了java.io.Console 类专用来访问基于字符的控制台设备. 你的程序如果要与Windows下的cmd或者Linux下的Terminal交互,就可以用Console类代劳. 但我们不总是能得到可用的Console, 一个JVM是否有可用的Console依赖于底层平台和JVM如何被调用. 如果JVM是在交互式命令行(比如Windows的cmd)中启动的,并且输入输出没有重定向到另外的地方,那么就可以得到一个可用的Console实 例. 
 
8. 对脚本语言的支持如: ruby, groovy, javascript. 
 
9. Common Annotations 
 
Common annotations原本是Java EE 5.0(JSR 244)规范的一部分，现在SUN把它的一部分放到了Java SE 6.0中. 
随 着Annotation元数据功能(JSR 175)加入到Java SE 5.0里面，很多Java 技术(比如EJB,Web Services)都会用Annotation部分代替XML文件来配置运行参数（或者说是支持声明式编程,如EJB的声明式事务）, 如果这些技术为通用目的都单独定义了自己的Annotations,显然有点重复建设, 所以,为其他相关的Java技术定义一套公共的Annotation是有价值的，可以避免重复建设的同时，也保证Java SE和Java EE 各种技术的一致性. 
下面列举出Common Annotations 1.0里面的10个Annotations Common Annotations 
Annotation Retention Target Description 
Generated Source ANNOTATION_TYPE, CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE 用于标注生成的源代码 
Resource Runtime TYPE, METHOD, FIELD 用于标注所依赖的资源,容器据此注入外部资源依赖，有基于字段的注入和基于setter方法的注入两种方式 
Resources Runtime TYPE 同时标注多个外部依赖，容器会把所有这些外部依赖注入 
PostConstruct Runtime METHOD 标注当容器注入所有依赖之后运行的方法，用来进行依赖注入后的初始化工作，只有一个方法可以标注为PostConstruct 
PreDestroy Runtime METHOD 当对象实例将要被从容器当中删掉之前，要执行的回调方法要标注为PreDestroy RunAs Runtime TYPE 用于标注用什么安全角色来执行被标注类的方法，这个安全角色必须和Container 的Security角色一致的。RolesAllowed Runtime TYPE, METHOD 用于标注允许执行被标注类或方法的安全角色，这个安全角色必须和Container 的Security角色一致的 
PermitAll Runtime TYPE, METHOD 允许所有角色执行被标注的类或方法 
DenyAll Runtime TYPE, METHOD 不允许任何角色执行被标注的类或方法，表明该类或方法不能在Java EE容器里面运行 
DeclareRoles Runtime TYPE 用来定义可以被应用程序检验的安全角色，通常用isUserInRole来检验安全角色 
 
    注意: 
    1. RolesAllowed,PermitAll,DenyAll不能同时应用到一个类或方法上 
    
    2. 标注在方法上的RolesAllowed,PermitAll,DenyAll会覆盖标注在类上的RolesAllowed,PermitAll,DenyAll 
    
    3. RunAs,RolesAllowed,PermitAll,DenyAll和DeclareRoles还没有加到Java SE 6.0上来 

    4. 处理以上Annotations的工作是由Java EE容器来做, Java SE 6.0只是包含了上面表格的前五种Annotations的定义类,并没有包含处理这些Annotations的引擎,这个工作可以由Pluggable Annotation Processing API(JSR 269)来做
 
改动的地方最大的就是java GUI界面的显示了，JDK6.0（也就是JDK1.6）支持最新的windows vista系统的Windows Aero视窗效果，而JDK1.5不支持！！！ 
你要在vista环境下编程的话最好装jdk6.0，否则它总是换到windows basic视窗效果.

# 1.7 

1. switch中可以使用字串了
```java
String s = "test";
switch (s) {
 case "test" :
   System.out.println("test");
case "test1" :
   System.out.println("test1");
break ;
default :
  System.out.println("break");
break ;
}
```

2. "<>"这个玩意儿的运用List<String> tempList = new ArrayList<>(); 即泛型实例化类型自动推断。

```java
import java.util.*;
public class JDK7GenericTest {
   public static void main(String[] args) {
      // Pre-JDK 7
      List<String> lst1 = new ArrayList<String>();
      // JDK 7 supports limited type inference for generic instance creation
      List<String> lst2 = new ArrayList<>();
 
      lst1.add("Mon");
      lst1.add("Tue");
      lst2.add("Wed");
      lst2.add("Thu");
 
      for (String item: lst1) {
         System.out.println(item);
      }
 
      for (String item: lst2) {
         System.out.println(item);
      }
   }
}
```

3. 自定义自动关闭类

以下是jdk7 api中的接口，（不过注释太长，删掉了close()方法的一部分注释）

```java
/**
 * A resource that must be closed when it is no longer needed.
 *
 * @author Josh Bloch
 * @since 1.7
 */
public interface AutoCloseable {
    /**
     * Closes this resource, relinquishing any underlying resources.
     * This method is invoked automatically on objects managed by the
     * {@code try}-with-resources statement.
     *
     */
    void close() throws Exception;
}
```
只要实现该接口，在该类对象销毁时自动调用close方法，你可以在close方法关闭你想关闭的资源，例子如下


```java
class TryClose implements AutoCloseable {

 @Override
    public void close() throws Exception {
        System.out.println(" Custom close method … close resources ");
  }
}
//请看jdk自带类BufferedReader如何实现close方法（当然还有很多类似类型的类）

public void close() throws IOException {
    synchronized (lock) {
        if (in == null)
            return;
        in.close();
        in = null;
        cb = null;
    }
}
```
 


4. 新增一些取环境信息的工具方法

```java
File System.getJavaIoTempDir() // IO临时文件夹

File System.getJavaHomeDir() // JRE的安装目录

File System.getUserHomeDir() // 当前用户目录

File System.getUserDir() // 启动java进程时所在的目录

.......
```

5. Boolean类型反转，空指针安全,参与位运算
```java
Boolean Booleans.negate(Boolean booleanObj)

True => False , False => True, Null => Null

boolean Booleans.and(boolean[] array)

boolean Booleans.or(boolean[] array)

boolean Booleans.xor(boolean[] array)

boolean Booleans.and(Boolean[] array)

boolean Booleans.or(Boolean[] array)

boolean Booleans.xor(Boolean[] array)
```

6. 两个char间的equals
```java
boolean Character.equalsIgnoreCase(char ch1, char ch2)
```
7. 安全的加减乘除
```java
int Math.safeToInt(long value)

int Math.safeNegate(int value)

long Math.safeSubtract(long value1, int value2)

long Math.safeSubtract(long value1, long value2)

int Math.safeMultiply(int value1, int value2)

long Math.safeMultiply(long value1, int value2)

long Math.safeMultiply(long value1, long value2)

long Math.safeNegate(long value)

int Math.safeAdd(int value1, int value2)

long Math.safeAdd(long value1, int value2)

long Math.safeAdd(long value1, long value2)

int Math.safeSubtract(int value1, int value2)
```

8. 数值可加下划线
例如：```int one_million = 1_000_000;```

9. 支持二进制文字
例如：```int binary = 0b1001_1001;```

10. 在try catch异常扑捉中，一个catch可以写多个异常类型，用"|"隔开，

jdk7之前：

```java
try {
   ......
} catch(ClassNotFoundException ex) {
   ex.printStackTrace();
} catch(SQLException ex) {
   ex.printStackTrace();
}
```

jdk7例子如下
```java
try {
   ......
} catch( ClassNotFoundException | SQLException ex) {
   ex.printStackTrace();
}
```

11. jdk7之前，你必须用try{}finally{}在try()内使用资源，在finally中关闭资源，不管try中的代码是否正常退出或者异常退出。jdk7之后，你可以不必要写finally语句来关闭资源，只要你在try()的括号内部定义要使用的资源。

前提是try()内的那些资源要实现自定义自动关闭类，这是新特性　

try-with-resources statement。
请看例子：

jdk7之前

```java
import java.io.*;
// Copy from one file to another file character by character.
// Pre-JDK 7 requires you to close the resources using a finally block.
public class FileCopyPreJDK7 {
   public static void main(String[] args) {
      BufferedReader in = null;
      BufferedWriter out = null;
      try {
         in  = new BufferedReader(new FileReader("in.txt"));
         out = new BufferedWriter(new FileWriter("out.txt"));
         int charRead;
         while ((charRead = in.read()) != -1) {
            System.out.printf("%c ", (char)charRead);
            out.write(charRead);
         }
      } catch (IOException ex) {
         ex.printStackTrace();
      } finally {            // always close the streams
         try {
            if (in != null) in.close();
            if (out != null) out.close();
         } catch (IOException ex) {
            ex.printStackTrace();
         }
      }
 
      try {
         in.read();   // Trigger IOException: Stream closed
      } catch (IOException ex) {
         ex.printStackTrace();
      }
   }
}
```
jdk7之后

```java
import java.io.*;
// Copy from one file to another file character by character.
// JDK 7 has a try-with-resources statement, which ensures that
// each resource opened in try() is closed at the end of the statement.
public class FileCopyJDK7 {
   public static void main(String[] args) {
      try (BufferedReader in  = new BufferedReader(new FileReader("in.txt"));
           BufferedWriter out = new BufferedWriter(new FileWriter("out.txt"))) {
         int charRead;
         while ((charRead = in.read()) != -1) {
            System.out.printf("%c ", (char)charRead);
            out.write(charRead);
         }
      } catch (IOException ex) {
         ex.printStackTrace();
      }
   }
}
```
12. JAVA7新增加了Objects类

它提供了一些方法来操作对象，这些方法大多数是“空指针”安全的，比如　Objects.toString(obj)；如果obj是null,，则输出是null。否则如果你自己用obj.toString()，如果obj是null，则会抛出NullPointerException.

# 8 
> 从 Java 8开始 你可以在`http://openjdk.java.net/jeps/{JEP编号}`页面看到所有的特性提案

JDK 8 于2014年3月14号发布。从 Java 8 开始开发代号已经弃用了。新特性有:
```
Lambda 表达式
Pipelines 和 Streams
Date 和 Time API
Default 方法
Type 注解
Nashhorn JavaScript 引擎
并发计数器
Parallel 操作
移除 PermGen Error
TLS SNI
第三个有里程碑意义的 Java 版本。其中最引人注目的便是 Lambda 表达式了，从此 Java 语言原生提供了函数式编程能力。Java 8 更加适应海量云计算的需要。
```
具体的特性包括：
```
JEP 117: 移除注解处理工具（Annotation-Processing Tool，apt）。
JEP 124: 增强证书撤销检查 API。
JEP 130: 实现 SHA-224 消息摘要算法。
JEP 131: 在 64-bit Windows 中支持 PKCS#11。
JEP 112: Charset 实现改善。
JEP 129: 实现 NSA Suite B 加密算法。
JEP 105: DocTree API。
JEP 106: 扩展 javax.tools API 来支持 javadoc 的访问。
JEP 113: 在 JDK 的 Kerberos 5 中添加 MS-SFU 扩展。
JEP 114: TLS Server Name Indication (SNI) 扩展。
JEP 121: 提供更强的 Password-Based-Encryption (PBE) 算法实现。
JEP 122: 移除永久带（Permanent Generation）。
JEP 127: 改善 Locale Data Packaging，并且采用 Unicode CLDR Data。
JEP 128: Unicode BCP 47 本地匹配。
JEP 133: 支持 Unicode 6.2。
JEP 136: 增强错误验证。
JEP 153: 启动 JavaFX 应用。
JEP 177: 优化 java.text.DecimalFormat.format。
JEP 103: 并行数组排序。
JEP 135: Base64 编码和解码。
JEP 138: 基于 Autoconf 的自动构建系统。
JEP 139: 增强 javac 来提高构建速度。
JEP 142: 减少对于特定域的高速缓存的争夺。
JEP 147: 减少类元数据占用。
JEP 148: 支持小虚拟机（不超过3M）的创建。
JEP 149: 减少核心库的内存使用。
JEP 150: 新的 Date 和 Time API。
JEP 160: lambda 函数表达式。
JEP 164: 利用 CPU 指令进行 AES 加密。
JEP 166: 针对JKS、JCEKS、PKCS12秘钥存储的修改。
JEP 170: JDBC 4.2。
JEP 172: DocLint。
JEP 173: 放弃一些很少使用的 GC 组合。
JEP 101: 泛华目标类型接口。
JEP 104: 在 Java 类型上加注解。
JEP 107: 增加集合的批量数据操作。
JEP 109: 在核心库中增加 Lambda 表达式。
JEP 115: 认证加密的密码套件。
JEP 118: 在运行时访问参数名称。
JEP 119: 通过反射实现  javax.lang.model.* API。
JEP 120: 重复注解。
JEP 123: 可配置的安全随机数生成。
JEP 126: lambda 表达式和虚拟扩展方法。
JEP 140: 限制的 doPrivileged。
JEP 155: 并发库更新。
JEP 161: 紧凑版本。
JEP 162: 为模块化做准备。
JEP 171: 在 sun.misc.Unsafe 中增加三个内存排序相关的指令。
JEP 174: Nashorn JavaScript 引擎。
JEP 176: 提供调用者敏感的检测机制。
JEP 178: 静态链接的 jni 库。
JEP 179: JDK API 的文档的支持和稳定。
JEP 180: 对于频繁冲突的 HashMap 使用平衡树。
JEP 184: HTTP URL的权限。
JEP 185: 限制外部 XML 资源的获取。
```

# 9

JDK 9 于2017年9月21日发布。新特性有：
```
模块化 —— Jigsaw
交互式命令行 —— JShell
默认的垃圾回收器 —— G1
进程操作改进
竞争锁的性能优化
分段代码缓存
优化字符串占用空间
```
这个版本中最引人注目的时候模块化，通过这个工作，可以构建更小的运行时环境，只需要包括Java平台中任务依赖的部分。这可以更好地适应云端的开发。

具体特性包括:
```
JEP 102: 改善了控制和管理操作系统进程的 API。
JEP 110: 定义了一个新的 Http 客户端 API，它实现了 HTTP/2 和 WebSocket，并且可以替代遗留的 HttpURLConnection API。该 API 将会以一个 incubator 模块的形式进行交付。
JEP 143: 提高竞争 Java 对象的监视性能。
JEP 158: 统一 JVM 的日志。
JEP 165: 通过支持运行时管理来增加对 JVM 编译器的管理。
JEP 193: 对变量处理的改进。
JEP 197: 将代码缓存划分成不同的段。
JEP 200: 采用 Java 平台模块化系统（Java Platform Module System，JPMS）对JDK进行模块化。
JEP 201: 源代码模块化。
JEP 211: 在 import 语句中 省略 deprecation 的警告。
JEP 212: 解决 lint 和 doclint 警告。
JEP 213: Project Coin 的改变
JEP 214: 移除 JDK 8 中 GC 组合器的废弃说明。
JEP 215: 在 javac 中实现了一个新的类型检测策略。
JEP 216: 正确地处理导入声明。
JEP 217: 注解流水线 2.0。
JEP 219: 定义了数据传输层安全（Datagram Transport Layer Security, DTLS）API。
JEP 220: 模块化运行时镜像。
JEP 221: 简化 Doclet API。
JEP 222: jshell - Java 中的交互式命令行。
JEP 223: 新的版本字符串模式。
JEP 224: 增强了 javadoc 工具来生成 HTML5 标记。
JEP 225: 增加了 javadoc 搜索。
JEP 226: UTF-8 属性文件资源的Bundle相关变化。
JEP 227: Unicode 7.0。
JEP 228: 增加更多可诊断的命令。
JEP 229: 将默认的秘钥库从 JKS 替换为 PKCS12。
JEP 231: 移除运行时 JRE 版本选择。
JEP 232: 增强了安全相关应用的性能。
JEP 233: 开发了一个工具来自动测试运行时编译器。
JEP 235: 增加关于 javac 生成类文件属性的 测试。
JEP 236: 定义了解析 API 来支持 Nashorn 的 ECMAScript 抽象语法树。
JEP 237: Linux/AArch64 端口相关。
JEP 238: 多版本 JAR 文件。
JEP 240: 移除 JVM 的 TI hprof 客户端。
JEP 241: 移除 jhat 工具。
JEP 243: 提供 Java 语言级的 JVM 编译接口。
JEP 244: TLS 应用层协议协商。
JEP 245: 验证 JVM 命令行标志参数。
JEP 246: 利用 CPU 指令提升 GHASH 和 RSAd 的性能。
JEP 247: 对老平台版本的编译支持。
JEP 248: G1 作为默认的垃圾回收器。
JEP 249: 基于 TLS 实现 OCSP Stapling。
JEP 250: 在类数据分享（CDS）归档中存储 interned 字符串。
JEP 251: 多方案镜像。
JEP 252: 默认使用 CLDR Locale Data。
JEP 253: 为 JavaFX UI 控制 和 CSS API 的模块化做准备。
JEP 254: 采用一个空间更加高效的 String 内部表示。
JEP 255: 合并 Xerces 2.11.0 中的更新。
JEP 256: BeanInfo 注解调整。
JEP 257: 更新 JavaFX/Media 中 GStreamer 的版本。
JEP 258: 使用 HarfBuzz 作为字体布局引擎。
JEP 259: 定义了一个高效标准的 Stack-Walking API。
JEP 260: 封装大部分的内部 API。
JEP 261: 实现模块化系统。
JEP 262: 支持 TIFF 图像 I/O。
JEP 263: 实现 Windows 和 Linux 高分辨率图像接口。
JEP 264: 平台日志 API 和 服务。
JEP 265: Java 2D 使用 Marlin Graphics Renderer。
JEP 266: 并发相关的一些更新。
JEP 267: 支持 Unicode 8.0。
JEP 268: 支持 XML 目录。
JEP 269: 增加一些集合类创建的工厂方法。
JEP 270: 为临界区预留栈的某些区域。
JEP 271: 统一 GC 日志。
JEP 272: 增加特定平台的桌面特性。
JEP 273: Deterministic Random Bit Generator (DRBG) 的实现。
JEP 274: 增强方法处理器。
JEP 275: Java 应用模块化打包。
JEP 276: 语言定义对象模型的动态链接。
JEP 277: 改善 Deprecation。
JEP 278: 为 G1 中的巨大对象增加测试。
JEP 279: 改进测试故障排除。
JEP 280: 指示字符串串联。
JEP 281: HotSpot C++ 单元测试框架。
JEP 282: Java连接器 jlink。
JEP 283: 在 Linux 上支持 GTK 3。
JEP 284: 新的 HotSpot 构建系统。
JEP 285: 自旋等待提示。
JEP 287: 实现 SHA-3 Hash 算法。
JEP 288: 禁止 SHA-1 验证。
JEP 289: 废弃 Applet API。
JEP 290: 过滤输入的序列化数据。
JEP 291: 废弃 Concurrent Mark Sweep (CMS) 垃圾收集器。
JEP 292: 在 Nashorn 中支持 ECMAScript 6 特征。
JEP 294: Linux/s390x 端口。
JEP 295: AOT支持。
JEP 297: 统一 arm32/arm64 端口。
JEP 298: 移除过时的例子。
JEP 299: 重新组织文档。
```

# 10

JDK 10 按计划将于2018年3月20日发布。新特性有：
```
JEP 286: 局部变量的类型推导。该特性在社区讨论了很久并做了调查，可查看 JEP 286 调查结果。
JEP 296: 将 JDK 的多个代码仓库合并到一个储存库中。
JEP 304: 垃圾收集器接口。通过引入一个干净的垃圾收集器（GC）接口，改善不同垃圾收集器的源码隔离性。
JEP 307: 向 G1 引入并行 Full GC。
JEP 310: 应用类数据共享。为改善启动和占用空间，在现有的类数据共享（“CDS”）功能上再次拓展，以允许应用类放置在共享存档中。
JEP 312: 线程局部管控。允许停止单个线程，而不是只能启用或停止所有线程。
JEP 313: 移除 Native-Header Generation Tool (javah)
JEP 314: 额外的 Unicode 语言标签扩展。包括：cu (货币类型)、fw (每周第一天为星期几)、rg (区域覆盖)、tz (时区) 等。
JEP 316: 在备用内存设备上分配堆内存。允许 HotSpot 虚拟机在备用内存设备上分配 Java 对象堆。
JEP 317: 基于 Java 的 JIT 编译器（试验版本）。
JEP 319: 根证书。开源 Java SE Root CA 程序中的根证书。
JEP 322: 基于时间的版本发布模式。“Feature releases” 版本将包含新特性，“Update releases” 版本仅修复 Bug 。
```

# 11

[JDK 11](http://openjdk.java.net/projects/jdk/11/)目前已经加入的提案有：
```
309: Dynamic Class-File Constants
318: Epsilon GC
320: 移除 Java EE 和 CORBA 模块
321: HTTP Client (标准)
323: Lambda中的局部变量声明
324: 关于 Curve25519 和 Curve448 的重要协议
327: Unicode 10
328: Flight Recorder
329: ChaCha20 和 Poly1305 加密
```
