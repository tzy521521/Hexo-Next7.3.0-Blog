---
title: Java反射机制
date: 2018-08-27 10:26:26
tags: java基础
categories: 面试题
---
# 反射机制
## 反射
　　当程序无法获知对象类型时，在运行期间动态获取类的所有属性和方法，这种动态获取类信息和动态调用对象方法的功能称为反射机制。
## 反射机制的实现
　　Class类与java.lang.reflect类库一起实现反射机制，java.lang.reflect类库包含Field/Method/Constructors类。这些类型的对象由JVM在运行时出创建，分别用于获取未知类的域/方法/构造器：通过Class类和java.lang.reflect类包，未知对象的类信息在运行时被确定，并且在编译时无需获取。
<!-- more -->
## RTTI与java.lang.Class类
　　RTTI，runtime type information/运行时类型信息，JVM运行时负责记录一个对象的属性。
　　运行期间，Java通过Class对象记录每个对象的RTTI；每当编写并且编译一个新类时，就会产生一个对应的Class对象（和新类保存在一个同名的.class文件中）
## JVM通过类加载器创建类的对象实例
　　类加载器首先检查类的Class对象是否加载，未加载的话从类的.class文件中加载；
　　一旦类的Class对象被载入内存，它就被用来创建类的所有对象。
## java.lang.reflect类
　　reflect包提供以下类供反射使用，解析目标类。
　　Class类：代表一个目标类；Field类：代表目标类的成员变量；Method类：代表目标类的方法。Constructor类：代表目标类的构造方法。Array类：提供了动态创建数组，以及访问数组的元素的静态方法；
# 反射步骤
## 获得目标类的java.lang.Class对象。
　　1.已获得目标类对象实例，通过目标类对象实例.getClass()返回该类Class对象
```java
// Object类
public final native Class<?> getClass();
```
　　２.已获得目标类名，通过Class c = MyClass.getClass()获得该类Class对象；
　　３.目标类名在编译器不确定，在运行期确定
　　如果目标类名在编译器不确定，在运行期可以确定，使用Class.forName(目标类名)获取该类Class对象，要求目标类名必须是全限定；Class.forName(目标类名)内部通过反射API根据目标类名将类手动加载到内存中，称为类加载器加载方法。加载过程中会把目标类的static方法，变量，代码块加载到JVM，注意此时尚未创建对象实例。
## 利用java.lang.Class对象通过反射API获取目标类信息
　　１.创建目标类对象实例
　　Object newInstance()：通过调用默认构造器创建一个对象实例。
　　２.获得构造器
```java
Constructor[] getConstructors() //获得所有public构造器；
Constructor[] getDeclaredConstructors() //获得所有访问权限的构造器
Constructor getConstructor(Class[] params) //根据指定参数获得对应构造器；
Constructor getDeclaredConstructor(Class[] params) //根据指定参数获得对应构造器；
```
　　３.获得变量
```java
Field[] getFields() //获得类中所有public变量
Field[] getDeclaredFields() //获得类中所有访问权限变量
Field getField(String name) //根据变量名得到对应的public变量
Field getDeclaredField(String name) //根据变量名获得对应的变量，访问权限不限；
```
# 反射的应用
　　广泛应用于对象序列化和JavaBean中；
　　eclipse等IDE补全机制：eclipse等IDE在代码构建对象时，通过反射机制自动把该对象能使用的方法和属性全部列出来，供用户选择。