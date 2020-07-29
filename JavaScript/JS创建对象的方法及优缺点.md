#### 工厂模式创建对象

```js
function creatPerson(name){
    var o = new Object()
    o.name = name 
    o.getName = function(){
        console.log(this.name)
    }
    return o
}
var p1 = creatPerson('小红') 
var p2 = creatPerson('小明')
console.log(p1.name) //'小红'
console.log(p2.name) //'小明'
//缺点：对象无法识别，所有的实例都会指向同一个原型
```

#### 构造函数模式

```js
function Person(name){
    this.name = name
    this.getName = function(){
        return this.name
    }
}
var p1 = new Person('小红')
//缺点：每次创建实例时，每个方法都要被创建一次
//优点：实例可以识别为一个特定的类型

```

#### 原型模式

```js
function Person(){}
Person.prototype.name = "小红"
Person.prototype.getName = function(){
    return this.name
}
var p1 = new Person()
//优点：方法不会重新创建
//缺点：1. 所有的属性和方法都共享 2. 不能初始化参数
```

#### 组合模式

```js
function Person(name) {
    this.name = name;
}

Person.prototype = {
    constructor: Person,
    getName: function () {
        console.log(this.name);
    }
};

var person1 = new Person();
//优点：该共享的共享，该私有的私有，使用最广泛的方式
//缺点：有的人就是希望全部都写在一起，即更好的封装性
```

