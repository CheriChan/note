# Proxy

**Vue3.x改用Proxy实现响应式数据原理**

Proxy可以直接监听对象和数组的变化，有13种拦截方法

而Vue2.x用Object.defineProperty监听对象和数组的变化用到了递归

**Vue2.x如何监测数组变化**

使用了函数劫持，Vue将data中的数组惊醒了原型链重写，指向了自己定义的数组原型方法，指向了自己定义的数组原型方法。这样当调用数组api时，可以通知依赖更新。如果数组中包含着引用类型，会对数组中的引用类型再次递归遍历进行监控。这样就实现了监测数组变化。

```js
export function set(target:Array<any>|Object,key:any,val:any):any{
    if(Array.isArray(target)&&isValidArrayIndex(key)){
        target.length = Math.max(target.length,key)
        target.splice(key,1,val)
        return val
    }
    if(key in target && !(key in Object.prototype)){
        target[key] = val
        return val
    }
    const ob = {target:any}._ob_
    if(!ob){
        target[key] = val
        return var
    }
    defineReactive(ob.value, key, val)
    ob.dep.notify()
    return val
}
```





