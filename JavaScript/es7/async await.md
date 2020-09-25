# promise，async，await的使用

[TOC]

## promise

promise对象代表一个异步操作，其不受外界影响，有三种状态：

+ Pending 进行中，未完成的
+ Resolved已完成
+ Rejected 已失败

### promise的优势

+ 解决回调地狱
+ 更好的进行错误捕获

**成功的结果需要用resolve包裹，失败的结果需要用reject包裹**

```js
var getUserInfo= new Promise((resolve,reject)=>{
    $.ajax({
        type:'get',
        url:'index,aspx',
        success(){
            if(res.code == 'ok'){
                resolve(res.msg)
            }else{
                reject(res.msg)
            }
        }
    })
})
getUserInfo.then((resultJson)=>{
    //通过拿到的数据渲染
}).catch((errMsg)=>{
    //获取数据失败的处理逻辑
})

```

## async

**作为一个关键字放到函数前面，用于表示函数是一个异步函数，因为async就是异步的意思，异步函数也就意味着该函数的执行不会阻塞后面的代码的执行**

```js
async funciton test(){
    return 'hello world'
}
test().then(res=>{
    console.log(res)
})
console.log('虽然在后面，但是我先执行')
//虽然在后面，但是我先执行
//hello world
```

**async输出的是一个promise对象,如果在函数中return一个直接量，async会把这个直接量通过Promise.resolve()封装成Promise对象**

+ 函数return一个直接量

```js
async function testAsync(){
    return 'hello async'
}
let result = testAsync()
console.log(result) //Promise{<resolve>:'hello async'}
```

+ 函数没有return一个直接量

```js
async function testAsync1(){
    console.log('hello async')
}
let reuslt1 = testAsync1()
console.log(result1)  //Promise.resolve(undefined)
```

**async函数返回的是一个Promise对象，所以在最外层不能用await获取其返回值的情况下，我们可以使用then()来处理这个Promise对象**

**Promise的特点是无等待，所以在没有await的情况下执行async函数，它会立即执行，返回一个Promise对象，并且不会阻塞后面的语句**

------

**如果async函数中返回一个值，当调用该函数的时候，内部会调用Promise.resolve()方法把它转化成一个Promise对象作为返回；如果函数内部抛出错误，那么就会调用Promise.reject()返回一个promise对象**

```js
async function timeout(flag){
    if(flag){
        return 'hello world'
    }else{
        throw 'my god,failure'
    }
}
console.log(tiemout(true))//调用Promise.resolve(),返回promise对象,Promise{<resolved>:'hello world'}
console.log(timeout(false))//调用Promise.reject()，返回promise对象,Promise{<rejected>:'my god,failure'}
```

## await

**await 可以用于等待一个async函数的返回值**

**await 不仅仅用于等Promise对象，它可以等仍以表达式的结果，所以await 后面实际是可以接普通函数调用或者promise对象**

**await命令只能用在async函数之中，如果用在普通函数，就会报错**

```js
function getSomething(){
    return "something"
}
async function testAsync(){
    return Promise.resolve("hello async")
}
async function test(){
    const v1 = await getSomething //await 后面接普通函数
    const v2 = await testAsync()  //await 后面接async promise对象
    console.log(v1,v2)
}
test() //打印结果something hello async
```

<<<<<<< HEAD
await 是个运算符，用于组成表达式，await表达式的运算结果取决于它等的东西。

如果它等的不是promise对象，那么await表达式的运算结果就是它等到值

如果它等的是一个promise对象，它就会阻塞后面的代码，等着Promise执行resolve，然后得到resolve的值，作为await表达式的运算结果，promise执行的结果可能是rejected
=======
**await 是个运算符，用于组成表达式，await表达式的运算结果取决于它等的东西。**

**如果它等的不是promise对象，那么await表达式的运算结果就是它等到的值**

**如果它等的是一个promise对象，它就会阻塞后面的代码，等着Promise执行resolve，然后得到resolve的值，作为await表达式的运算结果，promise执行的结果可能是rejected，所以最好把await命令放在try...catch代码块中**

```js
async function myFunction(){
    try{
        await somethingThatReturnsAPromise()
    }catch(err){
        console.log(err)
    }
}
//另一种写法
async function myFunction(){
    await somethingThatReturnsAPromise().catch((err)=>{
        console.log(err)
    })
}
```
>>>>>>> async/await

