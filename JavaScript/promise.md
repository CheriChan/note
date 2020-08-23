#  Promise



手写Promise对象

```js
class MyPromise(){
    constructor(executor){
        this._resolveQueue= [] // then收集的执行成功的回调队列
        this.rejectQueue = []  // then收集的执行失败的回调队列
        
        let _resolve = (val)=>{
            while(this._resolveQueue.length){
                const callback = this._resolveQueue.shift() //shift() 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值
                callback(val)
            }
        }
        let _reject = (val)=>{
            while(this._rejectQueue.length){
                const callback = this._rejectQueue.shift()
                callback(val)
            }
        }
         // new Promise()时立即执行executor,并传入resolve和reject
    executor(_resolve, _reject)
    }
    then(resolveFn,rejectFn){
        this._resolveQueue.push(resolveFn)
        this._rejectQueue.push(rejectFn)
    }
}

```

### 临时总结Promise A+规范

promise本质是一个状态机，且状态只能为Pending(等待状态)，Fulfilled（执行状态），Reject（拒绝状态），状态的变更是单向且不可逆的，只能从Pending->Fulfilled或Pending->Rejected

then 方法接收两个参数，分别对应状态变更时触发的回调。then方法返回一个promise，then方法可以被同一个promise调用很多次

根据规范，补充代码

```js
//Promise/A+规范的三种状态
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'

class MyPromise {
  // 构造方法接收一个回调
  constructor(executor) {
    this._status = PENDING     // Promise状态
    this._resolveQueue = []    // 成功队列, resolve时触发
    this._rejectQueue = []     // 失败队列, reject时触发

    // 由于resolve/reject是在executor内部被调用, 因此需要使用箭头函数固定this指向, 否则找不到this._resolveQueue
    let _resolve = (val) => {
      if(this._status !== PENDING) return   // 对应规范中的"状态只能由pending到fulfilled或rejected"
      this._status = FULFILLED              // 变更状态

      // 这里之所以使用一个队列来储存回调,是为了实现规范要求的 "then 方法可以被同一个 promise 调用多次"
      // 如果使用一个变量而非队列来储存回调,那么即使多次p1.then()也只会执行一次回调
      while(this._resolveQueue.length) {    
        const callback = this._resolveQueue.shift()
        callback(val)
      }
    }
    // 实现同resolve
    let _reject = (val) => {
      if(this._status !== PENDING) return   // 对应规范中的"状态只能由pending到fulfilled或rejected"
      this._status = REJECTED               // 变更状态
      while(this._rejectQueue.length) {
        const callback = this._rejectQueue.shift()
        callback(val)
      }
    }
    // new Promise()时立即执行executor,并传入resolve和reject
    executor(_resolve, _reject)
  }

  // then方法,接收一个成功的回调和一个失败的回调
  then(resolveFn, rejectFn) {
    this._resolveQueue.push(resolveFn)
    this._rejectQueue.push(rejectFn)
  }
}

```

### Promise的then方法

+ then方法可以被同一个promise调用很多次，所以then需要返回一个Promise，这样才能找到then方法，我们需要把then方法的返回值宝珠啊工程Promise

+ then方法需要拿到上一个then()的返回值

+ then的回调需要顺序执行

  ```js
  // then方法
  then(resolveFn, rejectFn) {
    //return一个新的promise
    return new MyPromise((resolve, reject) => {
      //把resolveFn重新包装一下,再push进resolve执行队列,这是为了能够获取回调的返回值进行分类讨论
      const fulfilledFn = value => {
        try {
          //执行第一个(当前的)Promise的成功回调,并获取返回值
          let x = resolveFn(value)
          //分类讨论返回值,如果是Promise,那么等待Promise状态变更,否则直接resolve
          //这里resolve之后，就能被下一个.then()的回调获取到返回值，从而实现链式调用
          x instanceof MyPromise ? x.then(resolve, reject) : resolve(x)
        } catch (error) {
          reject(error)
        }
      }
      //把后续then收集的依赖都push进当前Promise的成功回调队列中(_rejectQueue), 这是为了保证顺序调用
      this._resolveQueue.push(fulfilledFn)
  
      //reject同理
      const rejectedFn  = error => {
        try {
          let x = rejectFn(error)
          x instanceof MyPromise ? x.then(resolve, reject) : resolve(x)
        } catch (error) {
          reject(error)
        }
      }
      this._rejectQueue.push(rejectedFn)
    })
  }
  
  ```

  ...直接粘贴完整代码

  ```js
  //Promise/A+规定的三种状态
  const PENDING = 'pending'
  const FULFILLED = 'fulfilled'
  const REJECTED = 'rejected'
  
  class MyPromise {
    // 构造方法接收一个回调
    constructor(executor) {
      this._status = PENDING     // Promise状态
      this._value = undefined    // 储存then回调return的值
      this._resolveQueue = []    // 成功队列, resolve时触发
      this._rejectQueue = []     // 失败队列, reject时触发
  
      // 由于resolve/reject是在executor内部被调用, 因此需要使用箭头函数固定this指向, 否则找不到this._resolveQueue
      let _resolve = (val) => {
        //把resolve执行回调的操作封装成一个函数,放进setTimeout里,以兼容executor是同步代码的情况
        const run = () => {
          if(this._status !== PENDING) return   // 对应规范中的"状态只能由pending到fulfilled或rejected"
          this._status = FULFILLED              // 变更状态
          this._value = val                     // 储存当前value
  
          // 这里之所以使用一个队列来储存回调,是为了实现规范要求的 "then 方法可以被同一个 promise 调用多次"
          // 如果使用一个变量而非队列来储存回调,那么即使多次p1.then()也只会执行一次回调
          while(this._resolveQueue.length) {    
            const callback = this._resolveQueue.shift()
            callback(val)
          }
        }
        setTimeout(run)
      }
      // 实现同resolve
      let _reject = (val) => {
        const run = () => {
          if(this._status !== PENDING) return   // 对应规范中的"状态只能由pending到fulfilled或rejected"
          this._status = REJECTED               // 变更状态
          this._value = val                     // 储存当前value
          while(this._rejectQueue.length) {
            const callback = this._rejectQueue.shift()
            callback(val)
          }
        }
        setTimeout(run)
      }
      // new Promise()时立即执行executor,并传入resolve和reject
      executor(_resolve, _reject)
    }
  
    // then方法,接收一个成功的回调和一个失败的回调
    then(resolveFn, rejectFn) {
      // 根据规范，如果then的参数不是function，则我们需要忽略它, 让链式调用继续往下执行
      typeof resolveFn !== 'function' ? resolveFn = value => value : null
      typeof rejectFn !== 'function' ? rejectFn = reason => {
        throw new Error(reason instanceof Error? reason.message:reason);
      } : null
    
      // return一个新的promise
      return new MyPromise((resolve, reject) => {
        // 把resolveFn重新包装一下,再push进resolve执行队列,这是为了能够获取回调的返回值进行分类讨论
        const fulfilledFn = value => {
          try {
            // 执行第一个(当前的)Promise的成功回调,并获取返回值
            let x = resolveFn(value)
            // 分类讨论返回值,如果是Promise,那么等待Promise状态变更,否则直接resolve
            x instanceof MyPromise ? x.then(resolve, reject) : resolve(x)
          } catch (error) {
            reject(error)
          }
        }
    
        // reject同理
        const rejectedFn  = error => {
          try {
            let x = rejectFn(error)
            x instanceof MyPromise ? x.then(resolve, reject) : resolve(x)
          } catch (error) {
            reject(error)
          }
        }
    
        switch (this._status) {
          // 当状态为pending时,把then回调push进resolve/reject执行队列,等待执行
          case PENDING:
            this._resolveQueue.push(fulfilledFn)
            this._rejectQueue.push(rejectedFn)
            break;
          // 当状态已经变为resolve/reject时,直接执行then回调
          case FULFILLED:
            fulfilledFn(this._value)    // this._value是上一个then回调return的值(见完整版代码)
            break;
          case REJECTED:
            rejectedFn(this._value)
            break;
        }
      })
    }
  
    //catch方法其实就是执行一下then的第二个回调
    catch(rejectFn) {
      return this.then(undefined, rejectFn)
    }
  
    //finally方法
    finally(callback) {
      return this.then(
        value => MyPromise.resolve(callback()).then(() => value),             //执行回调,并returnvalue传递给后面的then
        reason => MyPromise.resolve(callback()).then(() => { throw reason })  //reject同理
      )
    }
  
    //静态的resolve方法
    static resolve(value) {
      if(value instanceof MyPromise) return value //根据规范, 如果参数是Promise实例, 直接return这个实例
      return new MyPromise(resolve => resolve(value))
    }
  
    //静态的reject方法
    static reject(reason) {
      return new MyPromise((resolve, reject) => reject(reason))
    }
  
    //静态的all方法
    static all(promiseArr) {
      let index = 0
      let result = []
      return new MyPromise((resolve, reject) => {
        promiseArr.forEach((p, i) => {
          //Promise.resolve(p)用于处理传入值不为Promise的情况
          MyPromise.resolve(p).then(
            val => {
              index++
              result[i] = val
              if(index === promiseArr.length) {
                resolve(result)
              }
            },
            err => {
              reject(err)
            }
          )
        })
      })
    }
  
    //静态的race方法
    static race(promiseArr) {
      return new MyPromise((resolve, reject) => {
        //同时执行Promise,如果有一个Promise的状态发生改变,就变更新MyPromise的状态
        for (let p of promiseArr) {
          MyPromise.resolve(p).then(  //Promise.resolve(p)用于处理传入值不为Promise的情况
            value => {
              resolve(value)        //注意这个resolve是上边new MyPromise的
            },
            err => {
              reject(err)
            }
          )
        }
      })
    }
  }
  
  ```

  ### 最简实现promise

<<<<<<< HEAD
=======
  ```js
function MyPromise(fn){
      this.cbs = []
      const resolve = (value)=>{
          setTimeout(()=>{
              this.data = value
              this.cbs.forEach((cb)=>{
                  cb(value)
              })
        })
      }
      fn(resolve)
  }
  MyPromise.prototype.then = function (onResolved){
      return new MyPromise((resolve)=>{
          this.cbs.push(()=>{
              const res = onResolved(this.data)
              if(res instanceof MyPromise){
                  res.then(resolve)
              }else{
                  resolve(res)
              }
          })
      })
  }
  let p1 = new MyPromise((res)=>{
      setTimeout(()=>{
          res(1)
      })
  })
  console.log(p1)// MyPromise {cbs: [] ,data: 1}
  let p2 = p1.then((res)=>{
      console.log(res)
      return res
  })  // 1
  console.log(p2) //MyPromise {cbs:[],data:""}  
  
  //创建实例，实例里面会声明回调函数的集合，以及resolve函数，同时执行入参的用户自定义函数；调用then方法，里面同样返回一个promise对象，对象函数参数执行了向回调函数集合push内容，在这个push里面，会调用resolve函数
  //先将then方法放到队列里面，然后resolve的时候执行队列里的方法
  //then是在做准备工作，将队列排好，resolve就是开始执行的指令
  
  
  
  ```

  

  ### 关于promise 的面试题

  

>>>>>>> 21686ecbfb62d0b9dc26959ed157f318bf7c8393
  ```js
const promise1 = new Promise((res,rej)=>{
      console.log(111)
  })
  console.log(promise1)
  // 111 Promise{(pending)}
  ```
<<<<<<< HEAD
=======

>>>>>>> 21686ecbfb62d0b9dc26959ed157f318bf7c8393


```js
const promise = new Promise((res,rej)=>{
    console.log(1)
    res('success')
    console.log(2)
})
promise.then(()=>{
    console.log(3)
})
console.log(4)
//1 2 4 3 
//从上至下，先遇到new Promise，执行其中的同步代码1
//再遇到resolve('success')， 将promise的状态改为了resolved并且将值保存下来
//继续执行同步代码2
//跳出promise，往下执行，碰到promise.then这个微任务，将其加入微任务队列
//执行同步代码4
//本轮宏任务全部执行完毕，检查微任务队列，发现promise.then这个微任务且状态为resolved，执行它。
```

```js
const promise = new Promise((res,rej)=>{
    console.log(1)
    console.log(2)
})
promise.then(()=>{
    console.log(3)
})
console.log(4)
//1 2 4
//promise中并没有resolve或者reject
//因此promise.then并不会执行，它只有在被改变了状态之后才会执行
```

```js
const promise1 = new Promise((res,rej)=>{
    console.log('promise1')
    resolve('resolve1')
})
const promise2 = promise1.then(res=>{
    console.log(res)
})
console.log(1,promise1)
console.log(2.promise2)
// promise1
// Promise {<fulfilled>: "resolve1"}
// Promise {<pending>}
// resolve1
//从上至下，先遇到new Promise，执行该构造函数中的代码promise1
//碰到resolve函数, 将promise1的状态改变为resolved, 并将结果保存下来
//碰到promise1.then这个微任务，将它放入微任务队列
//promise2是一个新的状态为pending的Promise
//执行同步代码1， 同时打印出promise1的状态是fulfilled
//执行同步代码2，同时打印出promise2的状态是pending
//宏任务执行完毕，查找微任务队列，发现promise1.then这个微任务且状态为resolved，执行它。

```

```js
const fn = ()=>(new Promise((res,rej)=>{
    console.log(1)
    res('success')
}))
fn().then(res=>{
    console.log(res)
})
console.log('start')
//1 start success
```

```js
console.log('start')
setTimeout(()=>{
    console.log('time')
})
Promise.resolve().then(()=>{
    console.log('resolve')
})
console.log('end')
// start end resolve time
//刚开始整个脚本作为一个宏任务来执行，对于同步代码直接压入执行栈进行执行，因此先打印出start和end。
//setTimout作为一个宏任务被放入宏任务队列(下一个)
//Promise.then作为一个微任务被放入微任务队列
//本次宏任务执行完，检查微任务，发现Promise.then，执行它
//接下来进入下一个宏任务，发现setTimeout，执行。

```

```js
const promise = new Promise((res,rej)=>{
    console.log(1)
    setTimeout(()=>{
        console.log('timeStart')
        res('success')
        console.log('timeEnd')
    },0)
    console.log(2)
}) 
promise.then(res=>{
    console.log(res)
})
console.log(4)
//1 2 4 timeStart timeEnd success
//从上至下，先遇到new Promise，执行该构造函数中的代码1
//然后碰到了定时器，将这个定时器中的函数放到下一个宏任务的延迟队列中等待执行
//执行同步代码2
//跳出promise函数，遇到promise.then，但其状态还是为pending，这里理解为先不执行
//执行同步代码4
//一轮循环过后，进入第二次宏任务，发现延迟队列中有setTimeout定时器，执行它
//首先执行timerStart，然后遇到了resolve，将promise的状态改为resolved且保存结果并将之前的promise.then推入微任务队列
//继续执行同步代码timerEnd
//宏任务全部执行完毕，查找微任务队列，发现promise.then这个微任务，执行它。

```

```js
Promise.resolve().then(()=>{
    console.log('promise1')
    const timer2 = setTimeout(()=>{
        console.log('time2')
    },0)
})
const timer1 = setTimeout(()=>{
    console.log('time1')
    Promise.resolve().then(()=>{
        console.log('promise2')
    })
})
console.log('start')
//start p1 t1 p2 t2

```

![image-20200731175246009](C:\Users\chenxy56\AppData\Roaming\Typora\typora-user-images\image-20200731175246009.png)

