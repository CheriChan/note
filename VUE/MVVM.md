# MVVM

[TOC]



## MVVM

+ **MVVM是Model-View-ViewModel，一种软件架构模式**
+ **Model代表数据模型**
+ **View代表UI组件**
+ **ViewModel是视图层和数据模型之间的桥梁**

优点：

低耦合：视图（View）可以独立于Model变化和修改，一个ViewModel可以绑定到不同的"View"上，当View变化的时候Model可以不变，当Model变化的时候View也可以不变

可重用性：你可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑

独立开发：开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计，使用Expression Blend可以很容易设计界面并生成xaml代码

## MVC

**MVC**中的**M**就是单纯的从网络获取回来的数据模型，**V**指的我们的视图界面，而**C**就是我们的ViewController

+ **Model表示应用程序核心（数据库）**
+ **View显示效果**
+ **Controller处理业务逻辑**