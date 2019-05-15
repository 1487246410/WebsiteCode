---
layout: post
title: "Java 设计模式 -   工厂模式"
author: SXJCLZH
date: 2016-09-25 18:15:06 
categories: 设计模式
description: "Java 设计模式 - 工厂模式"
tag: Java
---


### JAVA设计模式 - 工厂模式

---
简单工厂的定义：提供一个创建对象实例的功能，而无须关心其具体实现。被创建实例的类型可以是接口、抽象类，也可以是具体的类

### 实现汽车接口

```
public interface Car {
    String getName();
<strong>}</strong>

```
### 奔驰类

```
public class Benz implements Car {
    @Override
    public String getName() {
        return "Benz";
    }
}
```
### 宝马类

```
public class BMW implements Car {
    @Override
    public String getName() {
        return "BMW";
    }
}
```

### 简单工厂，既能生产宝马又能生产奔驰

```
public class SimpleFactory {
    public Car getCar(String name){
        if (name.equals("BMW")){
            return new BMW();
        }else if (name.equals("benz")){
            return new Benz();
        }else {
            System.out.println("不好意思，这个品牌的汽车生产不了");
            return null;
        }
    }
}
```

### 测试类
```
public class SimpleFactoryTest {
    public static void main(String[] args){
        SimpleFactory simpleFactory = new SimpleFactory();
        Car car = simpleFactory.getCar("BMW");
        System.out.println(car.getName());
    }
}
```
### 测试结果
```
BMW
```

---
根据简单工厂的定义，用户只要产品而不在乎产品如何生产，看起来好像很完美的样子。但大家想想，这个世界存在什么都生产的工厂吗？

显然是不存在的，每一个汽车品牌都有自己的生产工厂，都有自己生产技术。映射到spring框架中，我们有很多很多种的bean需要生产，如果只依靠一个简单工厂来实现，那么我们得在工厂类中嵌套多少个if..else if啊？

而且我们在代码中生产一辆汽车只是new一下就出来了，但实际操作中却不知道需要进行多少操作，加载、注册等操作都将体现在工厂类中，那么这个类就会变得紊乱，管理起来也很不方便，所以说每个品牌应该有自己的生产类。

因为专一，所以专业嘛，这个时候工厂方法就出现了。

### 二、工厂方法

### 工厂接口

```
//定义一个工厂接口，功能就是生产汽车
public interface Factory {
    Car getCar();
}
```

### 奔驰工厂

```
public class BenzFactory implements Factory {
    @Override
    public Car getCar() {
        return new Benz();
    }
}
```

### 宝马工厂

```
public class BMWFactory implements Factory{
    @Override
    public Car getCar() {
        return new BMW();
    }
}
```

### 测试类
```
public class FactoryTest {
   public static void main(String[] args){
       Factory bmwFactory = new BMWFactory();
       System.out.println(bmwFactory.getCar().getName());
       Factory benzFactory = new BenzFactory();
       System.out.println(benzFactory.getCar().getName());
   }
}
```
### 测试结果

```
BMW
Benz
```

---
根据上述代码可以看出，不同品牌的汽车是由不同的工厂生产的，貌似又是很完美的。但大家看一下测试类，当一个人想要去买一辆宝马汽车的时候（假设没有销售商），那么他就要去找宝马工厂给他生产一辆，过几天又想要买一辆奔驰汽车的时候，又得跑到奔驰工厂请人生产，这无疑就增加了用户的操作复杂性。所以有没有一种方便用户操作的方法呢？这个时候抽象工厂模式就出现了。


### 三、抽象工厂

###  抽象工厂
```
public abstract class AbstractFactory {

     protected abstract Car getCar();
     
     //这段代码就是动态配置的功能
     //固定模式的委派
     public Car getCar(String name){
        if("BMW".equalsIgnoreCase(name)){
            return new BmwFactory().getCar();
        }else if("Benz".equalsIgnoreCase(name)){
            return new BenzFactory().getCar();
        }else if("Audi".equalsIgnoreCase(name)){
            return new AudiFactory().getCar();
        }else{
            System.out.println("这个产品产不出来");
            return null;
        }
    }
}
```

### 默认工厂

```
public class DefaultFactory extends AbstractFactory {

    private AudiFactory defaultFactory = new AudiFactory();
    
    public Car getCar() {
        return defaultFactory.getCar();
    }

}
```

### 宝马工厂

```
public class BMWFactory extends AbstractFactory {
    @Override
    public Car getCar() {
        return new BMW();
    }
}
```

### 奔驰工厂
```
public class BenzFactory extends AbstractFactory {
    @Override
    public Car getCar() {
        return new Benz();
    }
}
```

###  测试类
```
public class AbstractFactoryTest {
    public static void main(String[] args) {        
        DefaultFactory factory = new DefaultFactory();  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在此我向大家推荐一个架构学习交流圈：830478757
        System.out.println(factory.getCar("Benz").getName());            
    }
}
```

### 测试结果

```
Benz

```

---
根据上述代码可以看出，用户需要一辆汽车，只需要去找默认的工厂提出自己的需求（传入参数），便能得到自己想要产品，而不用根据产品去寻找不同的生产工厂，方便用户操作。


按我粗浅的理解，设计模式的经典之处，就在于解决了编写代码的人和调用代码的人双方的痛楚，不同的设计模式也只适用于不同的场景。至于用或者不用，如何使用，那就需要各位看官着重考虑了。

但为了使用而使用是不应该的，细微之处，只有留给大家慢慢品味了。

<p align="right">编写：栗郑辉</p>