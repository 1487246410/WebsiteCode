---
layout: post
title: "Activity生命周期"
author: SXJCLZH
date: 2016-09-27 18:15:06
categories: Android 
description: "Activity生命周期"
tag: Android
---


Activity恐怕是Android用得最多且是最基本的组件了，估计也是每个学Android的人接触的第一概念，Activity的生命周期与启动模式是作为一个Android开发程序员必要掌握内容 ， 还希望本篇文章对您有所帮助。

### Activity的形态

 **Active/Running:** Activity处于活动状态，此时Activity处于栈顶，是可见状态，可与用户进行交互。 

**Paused：** 当Activity失去焦点时，或被一个新的非全屏的Activity，或被一个透明的Activity放置在栈顶时，Activity就转化为Paused状态。但我们需要明白，此时Activity只是失去了与用户交互的能力，其所有的状态信息及其成员变量都还存在，只有在系统内存紧张的情况下，才有可能被系统回收掉。 

**Stopped： **当一个Activity被另一个Activity完全覆盖时，被覆盖的Activity就会进入Stopped状态，此时它不再可见，但是跟Paused状态一样保持着其所有状态信息及其成员变量。 

**Killed： **当Activity被系统回收掉时，Activity就处于Killed状态。 

###  Activity 的生命周期

	 

Activity 类中定义了七回调方法，覆盖了活动生命周期的每一个环节，从启动到销毁的过程中只有当我们熟练的掌握了它的生命周期才能在它的回调方法中做合适的事情,先为大家献上一张生命周期图，供大家参考
  ![](http://pic001.cnblogs.com/img/tea9/201008/2010080516521645.png)
  


**onCreate:** 该方法是我们见到最多的回调方法,是在Activity被创建时回调，它是生命周期第一个调用的方法，我们在创建Activity时一般都需要重写该方法，然后在该方法中做一些初始化的操作，如通过setContentView设置界面布局的资源，初始化所需要的组件信息等。 

**onStart:** 此方法被回调时表示Activity正在启动，此时Activity已处于可见状态，只是还没有在前台显示，因此无法与用户进行交互。可以简单理解为Activity已显示而我们无法看见摆了。 
onResume : 当此方法回调时，则说明Activity已在前台可见，可与用户交互了（处于前面所说的Active/Running形态），onResume方法与onStart的相同点是两者都表示Activity可见，只不过onStart回调时Activity还是后台无法与用户交互，而onResume则已显示在前台，可与用户交互。当然从流程图，我们也可以看出当Activity停止后（onPause方法和onStop方法被调用），重新回到前台时也会调用onResume方法，因此我们也可以在onResume方法中初始化一些资源，比如重新初始化在onPause或者onStop方法中释放的资源。

**onPause:** 此方法被回调时则表示Activity正在停止（Paused形态），一般情况下onStop方法会紧接着被回调。但通过流程图我们还可以看到一种情况是onPause方法执行后直接执行了onResume方法，这属于比较极端的现象了，这可能是用户操作使当前Activity退居后台后又迅速地再回到到当前的Activity，此时onResume方法就会被回调。当然，在onPause方法中我们可以做一些数据存储或者动画停止或者资源回收的操作，但是不能太耗时，因为这可能会影响到新的Activity的显示——onPause方法执行完成后，新Activity的onResume方法才会被执行。

**onStop:**  一般在onPause方法执行完成直接执行，表示Activity即将停止或者完全被覆盖（Stopped形态），此时Activity不可见，仅在后台运行。同样地，在onStop方法可以做一些资源释放的操作（不能太耗时）。 

**onRestart:** 表示Activity正在重新启动，当Activity由不可见变为可见状态时，该方法被回调。这种情况一般是用户打开了一个新的Activity时，当前的Activity就会被暂停（onPause和onStop被执行了），接着又回到当前Activity页面时，onRestart方法就会被回调。 
onDestroy :此时Activity正在被销毁，也是生命周期最后一个执行的方法，一般我们可以在此方法中做一些回收工作和最终的资源释放。 

### Activity的四种启动模式

1. Standard模式（默认）

    我们平时直接创建的Activity都是这种模式的Activity，这种模式的Activity的特点是：只要你创建了Activity实例，一旦激活该Activity，则会向任务栈中加入新创建的实例，退出Activity则会在任务栈中销毁该实例。

 

2. SingleTop模式

    这种模式会考虑当前要激活的Activity实例在任务栈中是否正处于栈顶，如果处于栈顶则无需重新创建新的实例，会重用已存在的实例，否则会在任务栈中创建新的实例。

 

3. SingleTask模式

   如果任务栈中存在该模式的Activity实例，则把栈中该实例以上的Activity实例全部移除，调用该实例的newInstance()方法重用该Activity，使该实例处於栈顶位置，否则就重新创建一个新的Activity实例。


4. SingleInstance模式
   
   SingleInstance启动模式非常特殊,Activity会运行在自己的任务栈中,并且这个任务栈中只有一个实例存在,如果你想保证手机系统中只有一个实例存在那么可以使用这种启动方式
   应用场景：手机来电
   

### 如何更改Activity的启动模式:
    
       打开AndroidManifest.xml配置文件, 
       找到对应的Activity  配置 android:launchMode="" 更改自己所要更改的启动模式即可

![如何更改Activity的启动模式](http://img.blog.csdn.net/20180402102141784?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2h1YW5nbml1MTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


<p align="right">编写：栗郑辉</p>