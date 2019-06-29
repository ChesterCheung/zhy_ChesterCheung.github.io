﻿---
layout: post
title:  "类的继承和方法重写"
categories: Java
tags:  Java
author: Chester Cheung
---

* content
{:toc}


## 类的继承
接着之前学过构造方法的内容，这节课又学习了有关类的继承的相关内容。首先来看一下java中类的继承的语法格式：



	public class 类名(子类、派生类、超类) extends 类名(父类、基类){

	}      其中，继承类的关键字是：extends
1
2


需要注意的是，java中的继承是单继承，即一个子类只能继承一个父类，且子类继承父类后，子类必须要调用父类中的某一个构造方法。那么继承关系又好比生活中的什么关系呢？继承就好像包含关系一样，








比如电脑包括笔记本、台式电脑、平板电脑、…

学生包括大学生、高中生、初中生、…


对于子类所能继承到父类中的哪些内容：子类能继承到父类中所有的属性和所有的普通方法，但并不包含构造方法，因为构造方法是用来实例化该类的对象的。解释完了什么是继承，想一想继承有哪些作用呢，继承不仅可以提高代码的重用性，使程序变得更加简洁；还能提高程序的扩张性，使程序的可移植性更强。



## 方法重写
最后一部分就是方法重写，什么时候需要用到方法重写呢？当子类和父类都有同一个方法，但是方法的具体实现不同时就要用到方法重写。方法重写必须有以下几个条件：



1.必须存在继承关系


2.子类在重写方法时，子类方法的访问修饰符要大于等于父类方法的访问修饰符


3.子类在重写方法时，子类的返回值类型、方法名、参数都必须和父类的一致


4.子类方法的具体实现要和父类的不同。



完成重写后，**如何调用重写后的方法**？此时，根据new关键字后的类名来决定，如果类名是子类，则优先调用子类的方法，如果子类中没有才会调用父类中的。



**注意**：这种优先调用子类的方法就好比，有一个钱包对象，里面装有2张10元钞票，另有一个对象继承于这个钱包，里面装有1张10元，2张5元钞票；此时如果要实现方法max和min，就优先看子类钱包里面的，max=10，min=5；但如果子类和父类中的钱数相同，则max=10，min=10，就是所说的子类中没有时才会调用父类中的。


总结一下，继承和方法重写是一脉相承的，方法重写是在继承的基础上进行子类和父类之间方法的不同实现而进行的改写，最终方法继承和方法重写的作用都是一样的，能提高代码的重用性。



（补）对于访问修饰符的使用范围：

访问修饰符	同类中	同包不同类中	不同包中	不同包但是有继承关系的子类中

private		可以	不可以	不可以	不可以

默认void		可以	可以	不可以	不可以

protected	可以	可以	不可以	可以

public		可以	可以	可以	可以
 
