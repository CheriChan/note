# 面向对象

[TOC]



## es5面向对象

![](D:\note\source\images\jsmxdx.png)



## 原型链继承

```js
//这是一个构造函数
function Parent(name){
    this.name = name
    this.info = {
        age:18,
        hobby:'跑步'
    }
}  
//定义构造函数上原型的方法
Parent.prototype.getName = function (){
    return this.name                            
}
//这也是一个构造函数
function Child(){}
//将子类的原型对象指向父类的实例
Child.prototype = new Parent()
//绑定构造函数的constructor
Child.prototype.constructor = Child

let child = new Child()
console.log(child.name) //undefined
let child2 = new Child()
console.log(child2.name)//undefined

child.name = '寂静无声'
console.log(child.name,child2.name,child.info,child2.info) //'寂静无声' undefined {age: 18, hobby: "跑步"}  {age: 18, hobby: "跑步"} 

child.info.age = 17
console.log(child.info,child2.info) //{age: 17, hobby: "跑步"} {age: 17, hobby: "跑步"} 

```

> 原型链继承的弊端：
>
> + 所有的Child的实例原型都会指向同一个父类实例，对某个子类实例引用类型变量的修改会影响所有的子类实例
> + 创建子类实例时，无法向父类构造函数传参
>

## 构造函数继承

```js
function Parent(name){
    this.name = name
    this.age = 18
}
//原型上的方法
Parent.prototype.getName = function (){
    return this.name
}
//原型上的属性
Parent.prototype.info  = {
    age:18,
    hobby:"跑步"
}
function Child (){
    Parent.call(this,'zhangsan')
}

let child = new Child()
console.log(child.name,child.age) //'zhangsan' 18
console.log(child.getName()) //child.getName is not a function
console.log(child.info)  //undefined

let parent = new Parent()
console.log(parent.info)  //{age: 18, hobby: "跑步"}


```

> 构造函数继承的缺点：继承不到父类原型上的属性和方法

## 组合式继承

```js
function Parent(name){
    this.name = name
}
Parent.prototype.getName = function (){
    return this.name 
}
function Child (){
    Parent.call(this,'zhangsan') // 调用一次构造函数，创建了属性
}
Child.prototype = new Parent() // 这个时候会将父类的构造函数上的属性以及原型上的属性及方法全部放到子类实例的原型里，重复了构造函数里面的属性
Child.prototype.constructor = Child

let child1 = new Child()
console.log(child1.name) //'zhangsan'
console.log(child1.getName())  //'zhangsan'

//child的属性里面会有一个name:"zhangsan" ，属性_proto_下面还会有一个通过原型链继承的name：undefined 
```

> 组合式继承，每次创建Child的实例的时候，都会执行Parent.call和new Parent()，此时原型总会存在两份相同的属性和方法

## 寄生式组合继承

```js
function Parent(name){
    this.name = name
}
Parent.prototype.getName = function(){
    return this.name
}
function Child(){
    Parent.call(this,'zhangsan')
}
Child.prototype = Object.create(Parent.prototype) //Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__
Child.prototype.constructor = Child
let child = new Child()
let parent = new Parent()
console.log(child.getName()) //'zhangsan'
console.log(parent.getName()) // undefined

```

以上方法有一个封装写法

```js
function object(o){
    function F(){}
    F.prototype = o
    return new F()
}
function prototype(child,parent){
    var prototype = object (parent.prototype)
    prototype.constructor = child
    child.prototype = prototype
}
//prototype(Child,Parent)


function Foo (){
    this.name = 'foo'
}
Foo.prototype.getName = function (){
    return this.name
}
function FooChild(){
}
prototype(FooChild,Foo)
let child = new FooChild()
console.log(child.getName())
//还是第一种简单

```



## ES6面向对象

创建一个对象

```js
class Student{
    constructor(name){
        this.name = name
    }
    getName(){
        return this.name
    }
}
let student1 = new Student('小红')
console.log(student1.getName()) //'小红'

class Child extends Student{
    constructor(name,age){
        super(name)
        this.age = age
    }
}
let child = new Child('小明','3岁')
console.log(child.getName()) //'小明'
```

