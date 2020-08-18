# SPA (single-page application)

在web页面初始化的时候加载相应的HTML，JS和CSS。页面加载完成，SPA不会因为用户的操作而重新加载或者跳转

取而代之是利用路由机制实现HTML内容的变化。

### 优点

+ 用户体验好，避免重复渲染
+ 服务器压力小
+ 前后端分离

### 缺点

+ 初次加载时间长
+ 前进后退路由管理
+ 不利于SEO

#### SPA应用实现前进后退功能所遇到的问题

浏览器会记录地址栏的url的资源链接，如果url链接发生变化，浏览器回吧该url链接保存到一个特殊的数据结构中，用户点击返回与前进按钮时，会快速访问已经被记录的url链接资源

```javascript
console.log(window.history) // History {length:1,scrollRestoration:"auto",state:null}
//首次进入一个网站，此时的History length 为1
```

![image-20200807114754799](C:\Users\chenxy56\AppData\Roaming\Typora\typora-user-images\image-20200807114754799.png)

一般会用到forward/back/go

浏览器有两个实用的接口**pushState** 和 **replaceState**   （详细内容减浏览器History）

------

而使用ajax进行局部数据更新时，浏览器地址栏的url链接没有改变，所以浏览器不会保存这个记录。这个时候点击返回按钮就会出现非期待性的结果。再SPA中这种问题非常常见





