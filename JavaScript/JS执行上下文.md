

# 通过实例了解JS执行上下文的概念（待补）

[toc]

## 关于函数申明，和函数表达式，及变量声明的坑

只有函数声明（function declaration）会被加入到变量对象中，而函数表达式（function expression）会被忽略。

下面就是函数表达式的写法

```javascript
var foo = function (){
  console.log('foo1')
}
foo()
var foo = function(){
  console.log('foo2')
}
foo()


// 'foo1'
// 'foo2'
```

函数声明提升、变量声明提升

```js
foo();
var foo = function foo() {
    console.log('foo1');
}
function foo() {
    console.log('foo2');
}
foo(); 

// 'foo2'
// 'foo1'
```

上面的写法等价于

```javascript
var foo; //变量声明提升
function foo(){
  console.log('foo2');
}       //函数声明提升
foo();  //第一个执行函数，执行的是已经函数声明的函数
foo = function foo(){
  conosole.log('foo1')
}       //重新给变量赋值新的函数
foo();  //执行新赋值的函数


```

------

## 关于变量提升的坑

```javascript
var foo = 1;
function bar () {
    console.log(foo);
    var foo = 10;
    console.log(foo);
}

bar();

//undefined
//10
```

等价于

```javascript
var foo;
function bar(){
  var foo;
  console.log(foo);
  foo=10;
  console.log(foo);
}
foo = 1;
bar()
```

`bar` 函数运行，内部变量申明提升，当执行代码块中有访问变量时，**先查找本地作用域**，找到了 `foo` 为 `undefined` ，打印出来。然后 `foo` 被赋值为 `10` ，打印出 `10`。



```javascript
var foo = 1;
function bar (foo) {
    console.log(foo);
    foo = 234;
}
bar(123);
console.log(foo);

// 123
//1
```

等价于

```javascript
var foo
function bar (var foo){  
  console.log(foo); //此时打印的是入参的值123
  foo = 234; //此时改变的也是入参时候声明的foo，而不是全局的foo
}
bar(123)
console.log(foo) //只能访问全局的foo
```



