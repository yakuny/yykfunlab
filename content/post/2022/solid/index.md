+++
author = "YYK"
title = "面相对象设计-SOLID原则（简介）"
date = "2021-09-01"
categories = [
    "技术",
]
tags = [
    "软件设计",
]
+++

在程序设计领域，**SOLID** (**单一功能**、**开闭原则**、**里氏替换**、**接口隔离**以及**依赖反转**) 是由 Robert C. Martin 在21世纪早起引入的记忆术首字母缩略字，指代了**面向对象编程**和**面向对象设计**的五个基本原则。当这些原则被一起应用时，它们使得一个程序员开发一个进行软件维护和扩展的系统变得更加可能。

- Single Responsibility Principle: 单一功能原则
- Open Closed Principle: 开闭原则
- Liskov Substitution Principle: 里氏替换原则
- Interface Segregation Principle: 接口隔离原则
- Dependence Inversion Principle: 依赖倒置原则

---

## 单一功能原则（SRP）

- The **single-responsibility principle (SRP)** is a computer-programming principle that states that every **module, class** or **function** in a computer program should have responsibility over a single part of that program's functionality, and it should **encapsulate** that part. All of that module, class or function's **services** should be narrowly aligned with that responsibility.

在面相对象编程领域中，**单一功能原则(SRP)**规定每个类都应该有一个单一的功能，并且该功能应该由这个类完全封装起来。所有这个类的服务都应该严密的和该功能平行，没有依赖。
马丁把功能（职责）定义为：“改变的原因”，并且总结出一个类或者模块应该有且只有一个改变的原因。一个具体的例子就是，想象有一个用于编辑和打印报表的模块。这样的一个模块存在两个改变的原因。第一，报表的内容可以改变（编辑）。第二，报表的格式可以改变（打印）。这两方面会的改变因为完全不同的起因而发生：一个是本质的修改，一个是表面的修改。单一功能原则认为这两方面的问题事实上是两个分离的功能，因此他们应该分离在不同的类或者模块里。把有不同的改变原因的事物耦合在一起的设计是糟糕的。
保持一个类专注于单一功能点上的一个重要的原因是，它会使得类更加的健壮。继续上面的例子，如果有一个对于报表编辑流程的修改，那么将存在极大的危险性，因为假设这两个功能存在于同一个类中，修改报表的编辑流程会导致公共状态或者依赖关系的改变，打印功能的代码会因此不工作。

---

## 开闭原则（OCP）

- In object-oriented programming, the **open-closed principle** states "_software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification_"; that is, such an entity can allow its behaviour to be extended without modifying its source code.

在面向对象编程领域中，开闭原则规定“软件中的对象（类，模块，函数等等）应该对于扩展是开放的，但是对于修改是封闭的”[1]，这意味着一个实体是允许在不改变它的源代码的前提下变更它的行为。该特性在产品化的环境中是特别有价值的，在这种环境中，改变源代码需要代码审查，单元测试以及诸如此类的用以确保产品使用品质的过程。遵循这种原则的代码在扩展时并不发生改变，因此无需上述的过程。
开闭原则的命名被应用在两种方式上。这两种方式都使用了继承来解决明显的困境，但是它们的目的，技术以及结果是不同的。

---

## 里氏替换原则（LSP）

- **Substitutability** is a principle in object-oriented programming stating that, in a computer program, if S is a subtype of T, the objects of type T may be **replaced** with objects of type S (i.e., an object of type T may be substituted with any object of a subtype S) without altering any of the desirable properties of the program (correctness, task performed, etc,).

在面向对象的程序设计中，里氏替换原则（Liskov Substitution principle）是对子类型的特别定义。里氏替换原则的内容可以描述为： “派生类（子类）对象可以在程序中代替其基类（超类）对象。”

---

## 接口隔离原则（ISP）

- In the field of software engineering, the **interface-segregation principle (ISP)** states that no client should be forced to depend on methods it does not use. ISP splits interfaces that are very large into smaller and more specific ones so that clients will only have to know about the methods that are of interest to them. Such shrunken interfaces are also called _role interfaces._ ISP is intended to keep a system decoupled and thus easier to refactor, change, and redeploy.

接口隔离原则（英语：interface-segregation principles， 缩写：ISP）指明客户（client）不应被迫使用对其而言无用的方法或功能。接口隔离原则（ISP）拆分非常庞大臃肿的接口成为更小的和更具体的接口，这样客户将会只需要知道他们感兴趣的方法。这种缩小的接口也被称为角色接口（role interfaces）。接口隔离原则（ISP）的目的是系统解开耦合，从而容易重构，更改和重新部署。

---

## 依赖反转原则（DIP）

- In object-oriented design, the **dependency inversion principle** is a specific form of loosely coupling software modules. When following this principle, the conventional dependency relationships established from high-level, policy-setting modules to low-level, dependency modules are reversed, thus rendering high-level modules independent of the low-level module implementation details. The principle states:

在面向对象编程领域中，**依赖反转原则**（Dependency inversion principle，DIP）是指一种特定的解耦（传统的依赖关系创建在高层次上，而具体的策略设置则应用在低层次的模块上）形式，使得高层次的模块不依赖于低层次的模块的实现细节，依赖关系被颠倒（反转），从而使得低层次模块依赖于高层次模块的需求抽象。
该原则规定：

1. 高层次的模块不应该依赖于低层次的模块，两者都应该依赖于抽象接口。
2. 抽象接口不应该依赖于具体实现。而具体实现则应该依赖于抽象接口。

该原则颠倒了一部分人对于面向对象设计的认识方式。如高层次和低层次对象都应该依赖于相同的抽象接口。
应用依赖反转原则同样被认为是应用了适配器模式，例如：高层的类定义了它自己的适配器接口（高层类所依赖的抽象接口）。被适配的对象同样依赖于适配器接口的抽象（这是当然的，因为它实现了这个接口），同时它的实现则可以使用它自身所在低层模块的代码。通过这种方式，高层组件则不依赖于低层组件，因为它（高层组件）仅间接的通过调用适配器接口多态方法使用了低层组件，而这些多态方法则是由被适配对象以及它的低层模块所实现的。

---

## Reference

- [https://zh.wikipedia.org/wiki/SOLID_(%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E8%AE%BE%E8%AE%A1)](https://zh.wikipedia.org/wiki/SOLID_(%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E8%AE%BE%E8%AE%A1))
