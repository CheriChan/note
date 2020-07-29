#### Set

set对象是值得集合

```js
let mySet = new Set()
mySet.add(1) 
console.log(mySet) //{1}
let o = {a:1,b:2}
mySet.add(o)
console.log(mySet) //{1,{a:1,b:2}}
console.log(mySet.has(1)) //true
console.log(mySet.size) //2
mySet.delete(1); 
console.log(mySet) //{{a:1,b:2}}

let mySet2 =  new Set([1, 2, 3, 4]);
console.log(mySet2) //{1,2,3,4}
```

数组去重

```js
let arr = [1,1,2,3,4,4,5,5,5,6]
let newArr = [...new Set(arr)]
console.log(newArr) //[1,2,3,4,5,6]
```

#### Map

```js
let obj = new Map()
obj.set('color','red')
console.log(obj) // {"color" => "red"}
obj.get('color') // "red"
```

