[TOC]

# this

## this是什么

> this就是代码执行时当前的context object。


**代码没有在任何函数中执行，而是在全局作用域中执行时，this的值是global对象，对于浏览器来说，this就是window**

**而函数中的this值取决于这个函数是怎样被调用的**，当函数作为对象的方法被调用时，它的this值就是该对象

> this永远指向最后调用它的那个对象

```js
var name = "windowName"
function foo (){
    var name = "Cherry"
    console.log(this.name) 
    console.log(name)
}
foo() // windowName Cherry
```

```js
var name = "windowsName";
    var a = {
        name: "Cherry",
        fn : function () {
            console.log(this.name);// Cherry
        }
    }
    a.fn();
```

> ES6 的箭头函数是可以避免 ES5 中使用 this 的坑的。箭头函数的 this 始终指向函数定义时的 this，而非执行时。箭头函数中没有 this 绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层非箭头函数的 this，否则，this 为 undefined。

**箭头函数没有自己的this值，箭头函数中所使用的this都是来自函数作用域链，它的取值遵循普通普通变量一样的规则，在函数作用域链中一层一层往上找**

```js

var name = "windowsName";
var a = {
 
        name : "Cherry",
        func1: function () {
            console.log(this.name)     
        },
        func2: function () {
            setTimeout( () => {
                this.func1()
            },100);
        }

    };
    a.func2()     // Cherry
```

## 改变this指向的方法

### new关键字改变this指向

```javascript
function Fn(){
this.user = "Alice"
}
let test = new Fn()
console.log(test.user)  //Alice
```

用变量test创建一个Fn的实例，调用函数Fn的对象是test，this就是指向test

### call

```js
var a = {
    user:"Alice",
    fn:function(){
        console.log(this.user)
    }
}
var b = a.fn
b.call(a)  //Alice
```

### apply

```js
var a = {
    user:'Alice',
    fn:function(n,m){
        console.log(this.user)
        console.log(n+m)
    }
}
var b = a.fn
b.apply(a,[1,2]) //Alice 3
```

### bind

```js
var a = {
    user:"Alice",
    fn:function(n,m){
        console.log(this.user)
    }
}
var b = a.fn
var c = b.bind(a) //返回的是一个修改过的函数
console.log(c) //f(n,m){console.log(this.user)}
c()  //Alice
```



```js
function a(){
    console.log(this)
}
var o = {
    a:a,
    b:{
        k:1,
        n:()=>{
            console.log(this)
        }
    }
}
a()   	//window
o.a() 	//{
      	//a:function(){console.log(this)},
      	//b:{}
      	//  }
a.b.n() //Window

```

