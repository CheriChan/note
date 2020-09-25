# for

#### for语句用于创建一个循环，包含三个可选的表达式

`for` 语句头部圆括号中的所有三个表达式都是可选的。

**for (  [initialization]; [condition]; [final-expression] )**

------

**initialization** 一个表达式（包含赋值语句）或者变量声明。典型被用于初始化一个计数器。该表达式可以使用var或let关键字声明新的变量

使用var声明的变量是与for循环在同样的作用域中

用let声明的变量是语句的局部变量

------

**condition** 一个条件表达式，被用于确定每一次循环是否能被执行。如果该表达式的结果为true，则循环被执行。这个表达式可选，如果别忽略，那么就被认为永远为真。如果计算结果为假，那么执行流程将被跳到 `for` 语句结构后面的第一条语句。

------

**final-expression**

每次循环的最后都要执行的表达式。执行时机是在下一次 `condition` 的计算之前。通常被用于更新或者递增计数器变量。



```js
for (var i = 0; i < 5; i++) {
   console.log(i);
   // more statements
}
//0 1 2 3 4
for (let i = 0;i<5;i++){
    console.log(i)
}
//0 1 2 3 4
```

### for循环中let声明和var声明的区别

var是全局作用域，有变量提升的作用，所以在for中定义一个变量，全局可以使用，循环中的每一次给变量赋值都是给全局变量赋值。

let是块级作用域,只能在代码块中起作用，在js中一个{}中的语句我们也称为叫一个代码块，每次循环会产生一个代码块，每个代码块中的都是一个新的变量

```js
var a = []
for (var i=0;i<3;i++){
    a[i] = function (){
        console.log(i)
    }
}
a[0]()//3
a[1]()//3
a[2]()//3
```

以上代码可以理解为

```js
{
    var i = 0
    if(0<3){
        a[0] = function(){
            console.log(i)
        }
    }
    i++;  //1
    if(1<3){
        a[1] = function(){
            console.log(i)
        }
    };
    i++; //2
    if(2<3){
        a[2]=function(){
            console.log(i)
        }
    }
    i++; //3
    //跳出循环
}
a[0]()  //3
a[1]()  //3
a[2]()  //3
```

let声明的写法

```js
var a = [];
for (let i = 0; i < 3; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[0](); //0
a[1](); //1
a[2](); //2
```

```js
var a = []; 
{
    //我是父作用域
    let i = 0;
    if (i < 3) {
        //这一步模拟底层实现
        let k = i;
        a[k] = function () {
            //我是子作用域
            console.log(k);
        };
    };
    i++; //为1
    if (i < 3) {
        let k = i;
        a[k] = function () {
            console.log(k);
        };
    };
    i++; //为2
    if (i < 3) {
        let k = i;
        a[k] = function () {
            console.log(k);
        };
    };
    i++; //为3
    // 跳出循环
}
a[0](); //0
a[1](); //1
a[2](); //2

```

