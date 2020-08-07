# History

允许**操作**浏览器的曾经在标签页或框架里访问的**会话历史记录**

![image-20200807114754799](C:\Users\chenxy56\AppData\Roaming\Typora\typora-user-images\image-20200807114754799.png)

#####  History.length 是只读 ，该整数表示会话历史中元素的数目，包括当前加载的页

##### History.scrollRestoration 允许Web应用程序在历史导航上显式地设置默认滚动恢复行为。此属性可以是自动的auto，也可以手动manual

##### History.state只读，返回一个表示历史堆栈顶部的状态的值。这是一种可以不必等待`popstate` 事件而查看状态的方式。

### History的方法

+ History.back()   前往上一页 等价于history.go(-1)
+ History.forward()   前往下一页 等价于history.go(1)
+ History.go()  通过当前页面的相对位置从浏览器历史记录( 会话记录 )加载页面。例如: 如果当前页为第一页，前面已经没有页面了，我传参的值为-1，那么这个方法没有任何效果也不会报错。调用没有参数的 `go() `方法或者不是整数的参数时也没有效果。
+ History.pushState() 按指定的名称和url将数据push进会话历史栈，pushState方法不会触发页面刷新，只是导致history对象发生变化，地址栏会有反应
+ History.replaceState(）按指定的数据，名称和url，更新历史栈上最新的入口

```js
history.pushState(state, title[, url])
history.replaceState(state, title[, url])
```



