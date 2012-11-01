---
layout: post
title: "设计模式--装饰模式（Decorator）"
tag: "tec"
comment: true
published: true
date: 2012-10-01
---

我想该是自己学习一些设计模式的时候了，之前一直没有敢接触，是因为工作中还没有碰到一些难以解决的问题，而最近在参与讨论和设计软件的工作，需要一些新的设计的思想和方法技巧，既然有设计模式，那就翻出来看看，到底能不能为自己所用，本文就从装饰者模式开始。

本文的主要内容是翻译一篇讲述Decorator的外文。
###Decorator定义
动态给一个对象添加一些额外的职责。

###为什么使用Decorator
我们通常可以使用继承来实现功能的拓展,如果这些需要拓展的功能的种类很繁多,那么势必生成很多子类,增加系统的复杂性,同时,使用继承实现功能拓展,我们必须可预见这些拓展功能,这些功能是编译时就确定了,是静态的。

*使用Decorator的理由是*   
这些功能需要由用户动态决定加入的方式和时机。Decorator提供了"即插即用"的方法,在运行期间决定何时增加何种功能。

###翻译
--------------
Decorator常被用于在运行时扩展和修改一个实例的行为。继承常被用于扩展一个类的功能。和继承不同，你可以选择任何一个类的对象并修改它的行为，而不修改实例。

为了实现Decorator模式，你要扩展它的行为来构建一个包装器对象。包装器将在被包装的对象处理其工作之前或之后完成自己的任务，并委托调用被包装的实例。

待完成…

###原文
------------

To extend or modify the behaviour of ‘an instance’ at runtime decorator design pattern is used. Inheritance is used to extend the abilities of ‘a class’. Unlike inheritance, you can choose any single object of a class and modify its behaviour leaving the other instances unmodified.

In implementing the decorator pattern you construct a wrapper around an object by extending its behavior. The wrapper will do its job before or after and delegate the call to the wrapped instance.

####Design of decorator pattern
You start with an interface which creates a blue print for the class which will have decorators. Then implement that interface with basic functionalities. Till now we have got an interface and an implementation concrete class. Create an abstract class that contains (aggregation relationship) an attribute type of the interface. The constructor of this class assigns the interface type instance to that attribute. This class is the decorator base class. Now you can extend this class and create as many concrete decorator classes. The concrete decorator class will add its own methods. After / before executing its own method the concrete decorator will call the base instance’s method. Key to this decorator design pattern is the binding of method and the base instance happens at runtime based on the object passed as parameter to the constructor. Thus dynamically customizing the behavior of that specific instance alone.

####Decorator Design Pattern – UML Diagram   
![a](http://javapapers.com/wp-content/uploads/2011/05/Decorator-Pattern.jpg)

####Implementation of decorator pattern

Following given example is an implementation of decorator design pattern. Icecream is a classic example for decorator design pattern. You create a basic icecream and then add toppings to it as you prefer. The added toppings change the taste of the basic icecream. You can add as many topping as you want. This sample scenario is implemented below.

```
package com.javapapers.sample.designpattern;   
public interface Icecream {       
      public String makeIcecream();      
}      
```   

The above is an interface depicting an icecream. I have kept things as simple as possible so that the focus will be on understanding the design pattern. Following class is a concrete implementation of this interface. This is the base class on which the decorators will be added.

```
package com.javapapers.sample.designpattern;
public class SimpleIcecream implements Icecream {
  @Override
  public String makeIcecream() {
    return "Base Icecream";
  } 
}
```
Following class is the decorator class. It is the core of the decorator design pattern. It contains an attribute for the type of interface. Instance is assigned dynamically at the creation of decorator using its constructor. Once assigned that instance method will be invoked.

```
package com.javapapers.sample.designpattern;
abstract class IcecreamDecorator implements Icecream { 
  protected Icecream specialIcecream;
  public IcecreamDecorator(Icecream specialIcecream) {
    this.specialIcecream = specialIcecream;
  }
  public String makeIcecream() {
    return specialIcecream.makeIcecream();
  }
}
```

Following two classes are similar. These are two decorators, concrete class implementing the abstract decorator. When the decorator is created the base instance is passed using the constructor and is assigned to the super class. In the makeIcecream method we call the base method followed by its own method addNuts(). This addNuts() extends the behavior by adding its own steps.

```
package com.javapapers.sample.designpattern;
public class NuttyDecorator extends IcecreamDecorator { 
  public NuttyDecorator(Icecream specialIcecream) {
    super(specialIcecream);
  } 
  public String makeIcecream() {
    return specialIcecream.makeIcecream() + addNuts();
  } 
  private String addNuts() {
    return " + cruncy nuts";
  }
}
```
```
package com.javapapers.sample.designpattern;
public class HoneyDecorator extends IcecreamDecorator {
  public HoneyDecorator(Icecream specialIcecream) {
    super(specialIcecream);
  } 
  public String makeIcecream() {
    return specialIcecream.makeIcecream() + addHoney();
  }
  private String addHoney() {
    return " + sweet honey";
  }
}
```
####Execution of the decorator pattern

I have created a simple icecream and decorated that with nuts and on top of it with honey. We can use as many decorators in any order we want. This excellent flexibility and changing the behaviour of an instance of our choice at runtime is the main advantage of the decorator design pattern.

```
package com.javapapers.sample.designpattern; 
public class TestDecorator {
  public static void main(String args[]) {
    Icecream icecream = new HoneyDecorator(new NuttyDecorator(new SimpleIcecream()));
    System.out.println(icecream.makeIcecream());
  } 
}
```
*Output*

```
Base Icecream + cruncy nuts + sweet honey
```
###Decorator Design Pattern in java API

```
java.io.BufferedReader;
java.io.FileReader;
java.io.Reader;
```
The above readers of java API are designed using decorator design pattern.

----------------
###参考
1. [板桥里人](http://www.jdon.com/designpatterns/decorator.htm)
2. [javapapers](http://javapapers.com/design-patterns/decorator-pattern/)
