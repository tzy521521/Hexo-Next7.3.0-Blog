---
title: Java类加载机制
date: 2018-11-12 08:24:54
tags: java基础
categories: 面试题
---
# Java虚拟机类加载机制
　　感谢[朱小厮](http://blog.csdn.net/u013256816/article/details/50829596)和[xixicat](https://segmentfault.com/a/1190000004597758#articleHeader1)
## 代码思考
```java
public class NotInitialization{
    public static void main(String[] args) {
        System.out.println(SubClass.value);
    }
    public static class SSClass{
        static {
            System.out.println("SSClass Static Block");
        }
    }

    public static class SuperClass extends SSClass{
        static {
            System.out.println("SuperClass Static Block");
        }
        public static int value = 123;
        public SuperClass() {
            System.out.println("init SuperClass");
        }
    }

    public static class SubClass extends SuperClass{
        static {
            System.out.println("SubClass Static Block");
        }
        static int a;
        public SubClass() {
            System.out.println("init SubClass");
        }
    }
}
```
　　输出结果
```html
SSClass Static Block
SuperClass Static Block
123
```
　　也许有人会疑问：为什么没有输出SubClass Static Block。解释一下：对于静态字段，只有直接定义这个字段的类才会被初始化，因此通过其子类来引用父类中定义的静态字段，只会触发父类的初始化而不会触发子类的初始化。 
上面就牵涉到了虚拟机类加载机制。如果有兴趣，可以继续看下去。
<!-- more -->
## 双亲委派模型
### 类加载器种类
　　从Java虚拟机的角度来讲，只存在两种不同的类加载器：一种是启动类加载器(Bootstrap ClassLoader),这个类加载器使用c++语言实现，是虚拟机自身的一部分；另一种就是所有其他的类加载器，这些类加载器都由Java语言实现，独立于虚拟机外部，并且全部都继承自抽象类java.lang.ClassLoader
　　1、启动类加载器（Bootstrap ClassLoader）:这个类加载器负责加载<JAVA_HOME>lib目录中的。
　　2、扩展类加载器（Extension ClassLoader）:这个类加载器负责加载<JAVA_HOME>libext目录中的。
　　3、应用程序类加载器（Application ClassLoader）:这个类加载器负责加载用户类路径上所指定的类库。
　　4、自定义类加载器
　　　　程序中如果没有显式指定类加载器的话，默认是AppClassLoader来加载，它负责加载ClassPath目录中的所有类型，如果被加载的类型并没有在ClassPath目录中时，抛出java.lang.ClassNotFoundException异常。
　　　　一般是继承ClassLoader，如果要符合双亲委派规范，则重写findClass方法；要破坏的话，重写loadClass方法。
### 双亲委派模型的工作过程
　　如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成，每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到顶层的启动类加载器中，只有当父加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需的类）时，子加载器才会尝试自己去加载。
　　使用这种方式的好处是：能够有效确保一个类的全局唯一性，当程序中出现多个全限定名相同的类时，类加载器在执行加载时，始终只会加载其中的某一个类。

## 类加载过程
　　类从被加载到虚拟机内存中开始，到卸载出内存为止，它的整个生命周期包括：加载（Loading）、验证（Verification）、准备(Preparation)、解析(Resolution)、初始化(Initialization)、使用(Using)和卸载(Unloading)7个阶段。其中准备、验证、解析3个部分统称为连接（Linking）。如图所示。
　　加载、验证、准备、初始化和卸载这5个阶段的顺序是确定的，类的加载过程必须按照这种顺序按部就班地开始，而解析阶段则不一定：它在某些情况下可以在初始化阶段之后再开始，这是为了支持Java语言的运行时绑定（也称为动态绑定或晚期绑定）。以下陈述的内容都已HotSpot为基准。
### 加载
在加载阶段（可以参考java.lang.ClassLoader的loadClass()方法），虚拟机需要完成以下3件事情：
　　1.通过一个类的全限定名来获取定义此类的二进制字节流（并没有指明要从一个Class文件中获取，可以从其他渠道，譬如：网络、动态生成、数据库等）；
　　2.将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构；
　　3.在内存中生成一个代表这个类的java.lang.Class对象，作为方法区这个类的各种数据的访问入口；
加载阶段和连接阶段（Linking）的部分内容（如一部分字节码文件格式验证动作）是交叉进行的，加载阶段尚未完成，连接阶段可能已经开始，但这些夹在加载阶段之中进行的动作，仍然属于连接阶段的内容，这两个阶段的开始时间仍然保持着固定的先后顺序。
### 验证
　　验证是连接阶段的第一步，这一阶段的目的是为了确保Class文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全。 
　　验证阶段大致会完成4个阶段的检验动作：
　　　　1.文件格式验证：验证字节流是否符合Class文件格式的规范；例如：是否以魔术0xCAFEBABE开头、主次版本号是否在当前虚拟机的处理范围之内、常量池中的常量是否有不被支持的类型。
　　　　2.元数据验证：对字节码描述的信息进行语义分析（注意：对比javac编译阶段的语义分析），以保证其描述的信息符合Java语言规范的要求；例如：这个类是否有父类，除了java.lang.Object之外。
　　　　3.字节码验证：通过数据流和控制流分析，确定程序语义是合法的、符合逻辑的。
　　　　4.符号引用验证：确保解析动作能正确执行。
　　验证阶段是非常重要的，但不是必须的，它对程序运行期没有影响，如果所引用的类经过反复验证，那么可以考虑采用-Xverifynone参数来关闭大部分的类验证措施，以缩短虚拟机类加载的时间。
### 准备
　　准备阶段是正式为类变量分配内存并设置类变量初始值的阶段，这些变量所使用的内存都将在方法区中进行分配。这时候进行内存分配的仅包括类变量（被static修饰的变量），而不包括实例变量，实例变量将会在对象实例化时随着对象一起分配在堆中。其次，这里所说的初始值“通常情况”下是数据类型的零值，假设一个类变量的定义为：
```java
public static int value=123;
```
　　那变量value在准备阶段过后的初始值为0而不是123.因为这时候尚未开始执行任何java方法，而把value赋值为123的putstatic指令是程序被编译后，存放于类构造器()方法之中，所以把value赋值为123的动作将在初始化阶段才会执行。 
　　至于“特殊情况”是指：public static final int value=123，即当类字段的字段属性是ConstantValue时，会在准备阶段初始化为指定的值，所以标注为final之后，value的值在准备阶段初始化为123而非0.
### 解析
　　解析阶段是虚拟机将常量池内的符号引用替换为直接引用的过程。解析动作主要针对类或接口、字段、类方法、接口方法、方法类型、方法句柄和调用点限定符7类符号引用进行。这个阶段可以在初始化之后再执行。
### 初始化
　　类初始化阶段是类加载过程的最后一步，到了初始化阶段，才真正开始执行类中定义的java程序代码。在准备阶段，变量已经赋过一次系统要求的初始值，而在初始化阶段，则根据程序猿通过程序制定的主观计划去初始化类变量和其他资源，或者说：初始化阶段是执行类构造器&ltclinit&rt()方法的过程. 
　　clinit()方法是由编译器自动收集类中的所有类变量的赋值动作和静态语句块static{}中的语句合并产生的，编译器收集的顺序是由语句在源文件中出现的顺序所决定的，静态语句块只能访问到定义在静态语句块之前的变量，定义在它之后的变量，在前面的静态语句块可以赋值，但是不能访问。如下：
```java
public class Test {
    static {
        i=0;
        //这句编译器会报错：Illegal forward reference（非法向前引用）
        System.out.println(i);
    }
    static int i=1;
}
```
　　那么去掉报错的那句，改成下面：
```java
public class Test {
    static {
        i=0;
        //这句编译器会报错：Cannot reference a field before it is defined（非法向前应用）
        //System.out.println(i);
    }
    static int i=1;
    public static void main(String args[]) {
        System.out.println(i);
    }
}
```
　　输出结果是什么呢？当然是1啦~在准备阶段我们知道i=0，然后类初始化阶段按照顺序执行，首先执行static块中的i=0,接着执行static赋值操作i=1,最后在main方法中获取i的值为1。
　　clinit()方法与实例构造器linit()方法不同，它不需要显示地调用父类构造器，虚拟机会保证在子类clinit()方法执行之前，父类的clinit()方法已经执行完毕，回到本文开篇的举例代码中，结果会打印输出：SSClass就是这个道理。 
　　由于父类的clinit()方法先执行，也就意味着父类中定义的静态语句块要优先于子类的变量赋值操作。
　　clinit()方法对于类或者接口来说并不是必需的，如果一个类中没有静态语句块，也没有对变量的赋值操作，那么编译器可以不为这个类生产clinit()方法。 
　　接口中不能使用静态语句块，但仍然有变量初始化的赋值操作，因此接口与类一样都会生成clinit()方法。但接口与类不同的是，执行接口的clinit()方法不需要先执行父接口的clinit()方法。只有当父接口中定义的变量使用时，父接口才会初始化。另外，接口的实现类在初始化时也一样不会执行接口的clinit()方法。
    虚拟机会保证一个类的clinit()方法在多线程环境中被正确的加锁、同步，如果多个线程同时去初始化一个类，那么只会有一个线程去执行这个类的clinit()方法，其他线程都需要阻塞等待，直到活动线程执行clinit()方法完毕。如果在一个类的clinit()方法中有耗时很长的操作，就可能造成多个线程阻塞，在实际应用中这种阻塞往往是隐藏的。
```java
public class DealLoopTest {
    static class DeadLoopClass {
        static {
            if(true)
            {
                System.out.println(Thread.currentThread()+"init DeadLoopClass");
                while(true) {
                }
            }
        }
    }
    public static void main(String[] args)
    {
        Runnable script = new Runnable(){
            @Override
            public void run() {
                System.out.println(Thread.currentThread()+" start");
                DeadLoopClass dlc = new DeadLoopClass();
                System.out.println(Thread.currentThread()+" run over");
            }
        };

        Thread thread1 = new Thread(script);
        Thread thread2 = new Thread(script);
        thread1.start();
        thread2.start();
    }
}
```
　　运行结果:（即一条线程在死循环以模拟长时间操作，另一条线程在阻塞等待）
```html
Thread[Thread-0,5,main] start
Thread[Thread-1,5,main] start
Thread[Thread-0,5,main]init DeadLoopClass
```
　　需要注意的是，其他线程虽然会被阻塞，但如果执行<clinit>()方法的那条线程退出<clinit>()方法后，其他线程唤醒之后不会再次进入<clinit>()方法。同一个类加载器下，一个类型只会初始化一次。 
将上面代码中的静态块替换如下：
```java
import java.util.concurrent.TimeUnit;

public class DealLoopTest {
    static {
        System.out.println(Thread.currentThread() + "init DeadLoopClass");
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    public static void main(String[] args)
    {
        Runnable script = new Runnable(){
            @Override
            public void run() {
                System.out.println(Thread.currentThread()+" start");
                DealLoopTest dlc = new DealLoopTest();
                System.out.println(Thread.currentThread()+" run over");
            }
        };
        Thread thread1 = new Thread(script);
        Thread thread2 = new Thread(script);
        thread1.start();
        thread2.start();
    }
}
```
运行结果
```html
Thread[main,5,main]init DeadLoopClass
Thread[Thread-1,5,main] start
Thread[Thread-1,5,main] run over
Thread[Thread-0,5,main] start
Thread[Thread-0,5,main] run over
```
# 类初始化的时机
　　1.JVM启动包含main方法的启动类时。
　　2.当初始化一个类的时候，如果发现其父类还没有进行过初始化，则需要先触发其父类的初始化。(接口例外)
　　3.调用JavaAPI中的反射方法时（比如调用java.lang.Class中的方法，或者java.lang.reflect包中其他类的方法）,如果类没有进行过初始化，则需要先触发其初始化。
　　4.遇到new,getstatic,putstatic,invokestatic这失调字节码指令时，如果类没有进行过初始化，则需要先触发其初始化。生成这4条指令的最常见的Java代码场景是：
　　　　使用new关键字实例化对象的时候
　　　　读取或设置一个类的静态字段（被final修饰、已在编译器把结果放入常量池的静态字段除外）的时候
　　　　调用一个类的静态方法的时候
　　5.当使用jdk1.7动态语言支持时，如果一个java.lang.invoke.MethodHandle实例最后的解析结果REF_getstatic,REF_putstatic,REF_invokeStatic的方法句柄，并且这个方法句柄所对应的类没有进行初始化，则需要先出触发其初始化。
# 不会触类的初始化的情况
　　1.开篇已经举了一个范例：通过子类引用父类的静态字段，不会导致子类初始化。
　　２.通过数组定义来引用类，不会触发此类的初始化：（SuperClass类已在本文开篇定义）
```java
public class NotInitialization{
    public static void main(String[] args){
        SuperClass[] sca = new SuperClass[10];
    }
}
```
　　３.常量在编译阶段会存入调用类的常量池中，本质上并没有直接引用到定义常量的类，因此不会触发定义常量的类的初始化
```java
public class NotInitialization1 {
    public static void main(String[] args){
        System.out.println(ConstClass.HELLOWORLD);
    }
    public static class ConstClass{
        static{
            System.out.println("ConstClass init!");
        }
        public static  final String HELLOWORLD = "hello world";
    }
}
```
　　运行结果：
```html
hello world
```