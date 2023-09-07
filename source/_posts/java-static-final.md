---
title: Java 中 static 与 final 关键字
date: 2021-07-15 17:45:12
tags: Java
categories: Java
cover: /img/java/java_logo.jpg
description: Java 中 static 关键字可以用来修饰类的成员变量和成员方法，此外，还可以编写 static 代码块来优化程序性能。在 Java 中，final 关键字可以用来修饰类、方法和变量（包括成员变量和局部变量）。
---

## static关键字
Java中**static**关键字可以用来修饰类的成员变量和成员方法，此外，还可以编写static代码块来优化程序性能。
### static成员变量
static变量也称作静态变量，静态变量和非静态变量的区别是：
+ 静态变量被所有的对象所共享，在内存中只有一个副本，它当且仅当在类初次加载时会被初始化。
+ 而非静态变量是对象所拥有的，在创建对象的时候被初始化，存在多个副本，各个对象拥有的副本互不影响。  

static成员变量的初始化顺序按照定义的顺序进行初始化。  
static成员变量的定义方法如下：
```java
private static DataType1 data1;
public static DataType2 data2;
static DataType3 data3;
```
### static成员方法
static方法一般称作静态方法，由于静态方法不依赖于任何对象就可以进行访问，因此对于静态方法来说，是没有this、super的，因为它不依附于任何对象，既然都没有对象，就谈不上this、super了。  
在静态方法中不能访问类的非静态成员变量和非静态成员方法，因为非静态成员方法/变量都是必须依赖具体的对象才能够被调用，但是在非静态成员方法中是可以访问静态成员方法/变量的。  

静态成员属于类,有两种方式可以引用静态成员：
+ 在引用该静态成员时，通常不需要生成该类的对象，而是通过类名直接引用。引用的方法是“类名 . 静态成员名”。
+ 当然仍然可以通过“对象名 . 静态成员名”的方式引用该静态成员变量。

### static代码块
```java
public class Employee{
    private static String name;
    private static String gender;
    private static int age;

    static{      //静态代码块
        Employee.name="Bob";
        Employee.gender="male";
        Employee.age=27;
    }
}
```
static代码块又称为静态代码块，或静态初始化器。它是在类中独立于成员函数的代码块。static代码块不需要程序主动调用，在JVM加载类时系统会执行 static代码块，因此在static代码块中可以做一些类成员变量的初始化工作。如果一个类中有多个 static代码块，JVM将会按顺序依次执行。需要注意的是，所有的static代码块只能在JVM加载类时被执行一次。  
### static内部类
在内部类前添加修饰符static，这个内部类就变成为静态内部类了。
+ 一个静态内部类中可以声明static成员，但是在非静态内部类中不可以声明静态成员。
+ 静态内部类不可以使用外部类的非静态成员。
+ 静态成员内部类的特点主要是它本身是类相关的内部类，所以它可以不依赖于外部类实例而被实例化。  

```java
public class OuterClass{
    static class InnerClass{
        System.out.println("This is a static class.");
    }
}
```
### static成员加载顺序
```java
public class Test extends Base{
    static{
        System.out.println("test static");
    }
    public Test(){
        System.out.println("test constructor");
    }
    public static void main(String[] args) {
        new Test();
    }
}

class Base{
    static{
        System.out.println("base static");
    }
    public Base(){
        System.out.println("base constructor");
    }
}
```
上面程序输出结果为：
```
base static
test static
base constructor
test constructor
```
分析这段代码的执行过程：
+ 找到main方法入口，main方法是程序入口，但在执行main方法之前，要先加载Test类；  
+ 加载Test类的时候，发现Test类继承Base类，于是先去加载Base类；  
+ 加载Base类的时候，发现Base类有static块，而是先执行static块，输出base static结果；  
+ Base类加载完成后，再去加载Test类，发现Test类也有static块，而是执行Test类中的static块，输出test static结果；  
+ Base类和Test类加载完成后，然后执行main方法中的new Test()，调用子类构造器之前会先调用父类构造器；  
+ 调用父类构造器，输出base constructor结果；  
+ 再调用子类构造器，输出test constructor结果。
## final关键字
在Java中，**final**关键字可以用来修饰类、方法和变量（包括成员变量和局部变量）。
### final变量
用final关键字修饰的变量，只能进行一次赋值操作，并且在生存期内不可以改变它的值。final修饰的变量可以先声明，后赋值。  
当用final作用于类的成员变量时，成员变量必须在定义时或者构造器中进行初始化赋值，final作用于局部变量只需要保证在使用之前被初始化赋值即可。  
对于一个final变量，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。  
```java
final double PI=3.14;
static final int VALUE_1=100; //一个既是static又是final的字段只占据一段不能改变的存储空间。
public static int final VALUE_2=10; //在Java中定义全局变量，通常使用public static final修饰。
```
### final方法
final关键字修饰方法，它表示该方法不能被覆盖（重写）。另外，类中所有的private方法都隐式地指定为是final的，由于无法在类外使用private方法，所以也就无法覆盖它。此时可以在子类中定义相同的方法名和参数，这种情况不再产生重写与final的矛盾，而是在子类中重新定义了新的方法。可以对private方法添加final修饰符，但并没有添加任何额外意义。
### final方法参数
编写方法时，可以在参数前面添加final关键字，它表示在整个方法中，不会（实际上是不能）改变参数的值，具体类似于修饰数据。即不能改变参数的值，但是可以改变引用类型参数的对象的值。
```java
void fun(final int a,final String b);
//a不能被改变，b所指向的引用不能被改变。
```
### final类
定义为final的类不能被继承。如果希望一个类不被任何类继承，并且不允许其他人对这个类进行任何改动，可以将这个类设置为final形式，final类的语法如下：
```java
final ExmapleClass{}
```
如果将某个类设置为final形式，则类中的所有方法都被隐式设置为final形式，但是final类中的成员变量可以被定义为final或非final形式。
