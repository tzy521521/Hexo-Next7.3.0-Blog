---
title: Java中创建对象的5种不同的方式
date: 2018-11-11 18:58:32
description: 作为Java开发者，我们每天都会创建大量的对象，但是，我们总是使用管理依赖系统(如Spring框架)来创建这些对象。其实还有其他方法可以创建对象，在接下来的文章中我会进行详细介绍。
tags: java基础
categories: 面试题
---
　　本文译自：[Dzone](https://dzone.com/articles/5-different-ways-to-create-objects-in-java-with-ex)
　　作为Java开发者，我们每天都会创建大量的对象，但是，我们总是使用管理依赖系统(如Spring框架)来创建这些对象。其实还有其他方法可以创建对象，在接下来的文章中我会进行详细介绍。
# 创建对象的5种不同的方式
## (1)使用new关键字
　　这是最常见的创建对象的方法，并且也非常简单。通过使用这种方法我们可以调用任何我们需要调用的构造函数。
```java
Employee emp1 = new Employee();
```
```java
NEW org/programming/mitra/exercises/Employee
DUP
INVOKESPECIAL org/programming/mitra/exercises/Employee.<init> ()V
```

## (2)使用class类的newInstance方法
　　我们也可以使用class类的newInstance()方法来创建对象。此newInstance()方法调用无参构造函数以创建对象。
```java
Employee emp2 = (Employee) Class.forName("org.programming.mitra.exercises.Employee").newInstance();
```
```java
LDC "org.programming.mitra.exercises.Employee"
INVOKESTATIC java/lang/Class.forName (Ljava/lang/String;)Ljava/lang/Class;
INVOKEVIRTUAL java/lang/Class.newInstance ()Ljava/lang/Object;
CHECKCAST org/programming/mitra/exercises/Employee
```
　　或者
```java
Employee emp2 = Employee.class.newInstance();
```
```java
LDC Lorg/programming/mitra/exercises/Employee;.class
INVOKEVIRTUAL java/lang/Class.newInstance ()Ljava/lang/Object;
CHECKCAST org/programming/mitra/exercises/Employee
```
## (3)使用Constructor类的newInstance（）方法 
　　与使用class类的newInstance()方法相似，java.lang.reflect.Constructor类中有一个可以用来创建对象的newInstance()函数方法。通过使用这个newInstance()方法我们也可以调用参数化构造函数和私有构造函数。
```java
Constructor<Employee> constructor = Employee.class.getConstructor();
Employee emp3 = constructor.newInstance();
```
```java
ANEWARRAY java/lang/Object
INVOKEVIRTUAL java/lang/reflect/Constructor.newInstance ([Ljava/lang/Object;)Ljava/lang/Object;
CHECKCAST org/programming/mitra/exercises/Employee
```
　　newInstance()方法都被称为创建对象的反射方式。事实上，Class类的newInstance()方法内部使用Constructor类的newInstance()方法。这就是为什么后者是首选的，也被不同的框架，如Spring，Hibernate，Struts等使用。
## (4)使用clone()方法
　　实际上无论何时我们调用clone() 方法，JAVA虚拟机都为我们创建了一个新的对象并且复制了之前对象的内容到这个新的对象中。使用 clone()方法创建对象不会调用任何构造函数。要在对象上使用clone()方法，我们需要实现Cloneable接口并在其中定义clone()方法。
```java
Employee emp4 = (Employee) emp3.clone();
```
```java
INVOKEVIRTUAL org/programming/mitra/exercises/Employee.clone ()Ljava/lang/Object;
CHECKCAST org/programming/mitra/exercises/Employee
```
　　Java克隆是Java社区中最值得探讨的话题，它肯定有它的缺点，但它仍然是创建任何对象的副本的最流行和最简单的方法。

## (5)使用反序列化
　　无论何时我们对一个对象进行序列化和反序列化，JAVA虚拟机都会为我们创建一个单独的对象。在反序列化中，JAVA虚拟机不会使用任何构造函数来创建对象。
　　对一个对象进行序列化需要我们在类中实现可序列化的接口。
```java
ObjectInputStream in = new ObjectInputStream(new FileInputStream("data.obj"));
Employee emp5 = (Employee) in.readObject();
```
```java
INVOKEVIRTUAL java/io/ObjectInputStream.readObject ()Ljava/lang/Object;
CHECKCAST org/programming/mitra/exercises/Employee
```
　　正如我们在上面的字节码片段中看到的，除了第一个被转换为两个调用：一个是new，另外一个是invokespecial(调用构造函数)，其他4个方法被转换为invokevirtual调用(对象创建直接由这些方法处理)。
# 示例：
## 准备创建对象的 Employee 类
```java
class Employee implements Cloneable, Serializable {
    private static final long serialVersionUID = 1L;
    private String name;
    public Employee() {
        System.out.println("Employee Constructor Called...");
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + ((name == null) ? 0 : name.hashCode());
        return result;
    }
    @Override
    public boolean equals(Object obj) {
        if (this == obj)
            return true;
        if (obj == null)
            return false;
        if (getClass() != obj.getClass())
            return false;
        Employee other = (Employee) obj;
        if (name == null) {
            if (other.name != null)
                return false;
        } else if (!name.equals(other.name))
            return false;
        return true;
    }
    @Override
    public String toString() {
        return "Employee [name=" + name + "]";
    }
    @Override
    public Object clone() {
        Object obj = null;
        try {
            obj = super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return obj;
    }
}
```
## 创建 Employee对象的5种方式
　　在下面的Java程序中我们用5种方式来创建 Employee对象。您也可以在[GitHub](https://github.com/njnareshjoshi/exercises/tree/master/src/org/programming/mitra/exercises)上找到源代码
```java
public class ObjectCreation {
    public static void main(String... args) throws Exception {
        // By using new keyword
        Employee emp1 = new Employee();
        emp1.setName("Naresh");
        System.out.println(emp1 + ", hashcode : " + emp1.hashCode());
        // By using Class class's newInstance() method
        Employee emp2 = (Employee) Class.forName("org.programming.mitra.exercises.Employee")
                               .newInstance();
        // Or we can simply do this
        // Employee emp2 = Employee.class.newInstance();
        emp2.setName("Rishi");
        System.out.println(emp2 + ", hashcode : " + emp2.hashCode());
        // By using Constructor class's newInstance() method
        Constructor<Employee> constructor = Employee.class.getConstructor();
        Employee emp3 = constructor.newInstance();
        emp3.setName("Yogesh");
        System.out.println(emp3 + ", hashcode : " + emp3.hashCode());
        // By using clone() method
        Employee emp4 = (Employee) emp3.clone();
        emp4.setName("Atul");
        System.out.println(emp4 + ", hashcode : " + emp4.hashCode());
        // By using Deserialization
        // Serialization
        ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("data.obj"));
        out.writeObject(emp4);
        out.close();
        //Deserialization
        ObjectInputStream in = new ObjectInputStream(new FileInputStream("data.obj"));
        Employee emp5 = (Employee) in.readObject();
        in.close();
        emp5.setName("Akash");
        System.out.println(emp5 + ", hashcode : " + emp5.hashCode());
    }
}
```
## 输出结果
```java
Employee Constructor Called...
Employee [name=Naresh], hashcode : -1968815046
Employee Constructor Called...
Employee [name=Rishi], hashcode : 78970652
Employee Constructor Called...
Employee [name=Yogesh], hashcode : -1641292792
Employee [name=Atul], hashcode : 2051657
Employee [name=Akash], hashcode : 63313419
```


