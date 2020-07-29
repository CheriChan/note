# call和apply

## call

funciotn.call(obj, arg1, arg2, ...)

```javascript
let obj1 = {
  name:'小红',
  age:18,
  getInfo:function(){
    let info = `${this.name}今年${this.age}岁`
    return info
  }
}
let info = obj1.getInfo()
console.log(info) // 小红今年18岁
```

```javascript
let obj1 = {
  name:'小红',
  age:18,
  getInfo:function(){
    let info = `${this.name}今年${this.age}岁`
    return info
  }
}
let obj2 ={
  name:"小明",
  age:88
}

let info = obj1.getInfo.call(obj2)
console.log(info) // 小明今年88岁
```

```javascript
let obj2 = {
  name:'小黄',
  age:18,
  getInfo:function(contry,city){
    let info = `${this.name}今年${this.age}岁，出生在${contry}${city}`
    return info
  }
}
let obj3 ={
  name:"小王",
  age:88
}

let info = obj2.getInfo('中国','北京')
console.log(info) //小黄今年18岁，出生在中国北京
```

```javascript
let obj2 = {
  name:'小黄',
  age:18,
  getInfo:function(contry='',city=''){
    let info = `${this.name}今年${this.age}岁，出生在${contry}${city}`
    return info
  }
}
let obj3 ={
  name:"小王",
  age:88
}

let info = obj2.getInfo.call(obj3,'湖南')
console.log(info) //小王今年88岁，出生在湖南
```

如何手写call()

call()方法是预定义的js的函数的方法

所以手写call，也需要在js函数上定义一个方法

```javascript
Function.prototype.mycall = function(){}
```

```javascript
let obj2 = {
  name:'小黄',
  age:18,
  getInfo:function(contry='',city=''){
    let info = `${this.name}今年${this.age}岁，出生在${contry}${city}`
    return info
  }
}
let obj3 ={
  name:"小王",
  age:88
}
Function.prototype.myCall = function(thisArg, ...args) {
    const fn = Symbol('fn')        // 声明一个独有的Symbol属性, 防止fn覆盖已有属性
    thisArg = thisArg || window    // 若没有传入this, 默认绑定window对象
    thisArg[fn] = this              // this指向调用call的对象,即我们要改变this指向的函数
    const result = thisArg[fn](...args)  // 执行当前函数
    delete thisArg[fn]              // 删除我们声明的fn属性
    return result                  // 返回函数执行结果
}
let info = obj2.getInfo.myCall(obj3,'湖南')
console.log(info) //小王今年88岁，出生在湖南
//手写call的理念在于this的转换，将执行上下文的this指向调用call的函数，将调用call的函数转为传入的this的属性之一
```

## apply

function.apply(obj,[arr])

**apply()方法与call()方法类似，区别在与入参形式，call()分别接受参数，apply()接受数组形式的参数**

手写apply，与call也类似，只是改变了入参的形式

```javascript
let obj = {
  text:"天王盖地虎"
}
function foo(){
  console.log(this.text)
}

Function.prototype.myApply= function(objArgs,arr){
  const fn = Symbol('fn')
  objArgs = objArgs||window
  objArgs[fn]=this
  let result = objArgs[fn](...arr)
  delete objArgs[fn]
  return result
}
foo.myApply(obj) //天王盖地虎

```

## bind

function.bind(thisArg,arg1,arg2,...)

`bind()` 方法创建一个新的函数，在 `bind()` 被调用时，这个新函数的 `this` 被指定为 `bind()` 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

还是用例子说明比较好理解

```javascript
let obj = {
  a:"小明",
  getName:function (){
    return this.a 
  }
}
let foo = obj.getName
console.log(foo())   //undefined
```

```javascript
let obj = {
  other:"小红",
  getName:function(){
    return this.other
  }
}
let other = "小黄"
let foo = obj.getName
console.log(foo()) //undefind ，此处按照流程应该是打印出“小黄”

let a = foo.bind(obj)
console.log(a()) //小红
```

手写bind()

```javascript
Function.prototype.myBind = function (thisArg, ...args) {
    var self = this
    // new优先级
    var fbound = function () {
        self.apply(this instanceof self ? this : thisArg, args.concat(Array.prototype.slice.call(arguments)))
    }
    // 继承原型上的属性和方法
    fbound.prototype = Object.create(self.prototype);
    return fbound;
}

//测试
const obj = { name: '写代码像蔡徐抻' }
function foo() {
    console.log(this.name)
    console.log(arguments)
}

foo.myBind(obj, 'a', 'b', 'c')() 
//写代码像蔡徐抻 ['a', 'b', 'c']
```

