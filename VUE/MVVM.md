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

缺点：

+ 所有业务逻辑都在Controller里操作，逻辑复杂且不利于维护
+ 大量的DOM 操作使页面渲染性能降低，加载速度变慢，影响用户体验
+ 当 Model 频繁发生变化，需要主动更新到View ；当用户的操作导致Model发生变化，同样需要将变化的数据同步到Model中， 这样的工作不仅繁琐，而且很难维护复杂多变的数据状态

## 两者区别

MVVM与MVC最大的区别就是：它实现了View和Model的自动同步，也就是当Model的属性改变时，我们不用再自己手动操作Dom元素，来改变View的显示，而是改变属性后该属性对应View层显示会自动改变。

MVVM比MVC精简很多，不仅简化了业务与界面的依赖，还解决了数据频繁更新的问题，不用再用选择器操作DOM元素。因为在MVVM中，View不知道Model的存在，Model和ViewModel也观察不到View，这种低耦合模式提高代码的可重用性。