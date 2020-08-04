# Symbol

**是一种基本数据类型，不能new**

Symbol([description])

description可选的，字符串类型。对symbol的描述，可用于调试但不是访问symbol本身。

```js
var s1 = Symbol('foo')
var s2 = Symbol('foo')
s1 === s2 //false
```

```js
let myS = Symbol()
//1.
var a = {}
a[myS]= "hello"
//2.
var a = {}
Object.defineProperty(a,myS,{value:'Hello!'})
//3.
var a = {
    [myS]:"hello"
}

```

**为对象属性名时，不能用点运算符,可以用[]**

```js
let a = {}
let name = Symbol()
a.name = 'lili'
a[name] = 'lucy'
console.log(a.name,a[name]) //lili lucy
```

**遍历不会被for...in、for...of和Object.keys()、Object.getOwnPropertyNames()取到该属性**

### Symbol.for

在全局中搜索有没有以该参数作为名称的Symbol值，如果有，就返回这个Symbol值，否则就新建并返回一个以该字符串为名称的Symbol值

```js
let s1 = Symbol.for('foo')
let s2 = Symbol.for('foo')
s1 === s2 //true
```

###  Symbol.keyFor

```js
var s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

var s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined 
```

