# 面向对象

#### es5面向对象

![img](https://user-gold-cdn.xitu.io/2020/4/6/1714fd86c8983189?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

[图片来源]: https://juejin.im/post/5e8b261ae51d4546c0382ab4#heading-18

##### 原型链继承

```js
//这是一个构造函数
function Parent(name){
    this.name = name
}  
//定义构造函数上原型的方法
Parent.prototype.getName = funciton (){
    return this.name                            
}
//这也是一个构造函数
function Child(){
    
}
//将子类的原型对象指向父类的实例
Child.prototype = new Parent()
//绑定构造函数的constructor
Child.prototype.constructor = Child

let child = new Child()
```

> 原型链继承的弊端：
>
> + 所有的Child的实例原型都会指向同一个父类实例，对某个子类实例引用类型变量的修改会影响所有的字类实例
> + 创建子类实例时，无法向父类构造函数擦传参
>
> 

