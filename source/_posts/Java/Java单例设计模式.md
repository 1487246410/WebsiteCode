---
layout: post
title: "Java 设计模式 -   单例模式"
author: SXJCLZH
date: 2016-09-26 18:15:06 
categories: 设计模式
description: "Java 设计模式 - 单例模式"
tag: Java
---


### JAVA设计模式 - 单例模式

---

  单例模式（Singleton Pattern）是 Java 中最简单的设计模式之一。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。

  这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。
  
###  饿汉式单例模式
	  

```
public class Singleton{
    //内部创建静态对象
    private static Singleton instance=new Singleton();
    //私有化构造函数
    private Singleton(){}
    //静态方法返回实例
    pulic static Singleton getInstance(){
        return instance;
    }
  }
```

  
 
 
  > 特点：饿汉式会提前进行实例化，没有延迟加载，不管是否使用都会有一个已经初始化的实例在内存中，但不会出现懒汉式中的多线程问题。
  
### 懒汉式单例模式
```
  public class Singleton{
  
    private static Singleton instance;
    private Singleton(){}
    public static Singleton getInstance(){
        //此处为空的判断如果不加线程锁,会出现线程安全问题
        if(instance==null)
            return instance=new Singleton();
        else
            return instance;
    }
  }
```
  
  > 特点：实现了延迟加载，但在多线程情况下可能会出现问题，不能保证线程安全。
  
### 确保线程安全的懒汉式单例模式
```
  public class Singleton{
    private static Singleton instance;
    private Singleton(){}
    public static sychronized Singleton getInstance(){
        if(instance==null)
            return instance=new Singleton();
        else
            return instance;
    }
  }
  
```
  > 特点：synchronized限制了整个getInstance方法的完成执行

### 静态内部类的单例模式
  
```
  public class Singleton{
    private Singleton(){}
    private static class Inner{
        private static Singleton instanceHolder=new Singleton();
    }
    public static Singleton getInstacen(){
        return Inner.instanceHolder;
    }
  }
  
```
> 特点:由于内部类在编译完成后也是一个单独的class文件，因此在不使用的情况下Inner类是不会被加载的。同时，JVM保证在类加载的过程中static代码块在多线程或者单线程下都正确执行，且仅执行一次。解决了延迟加载以及线程安全的问题。