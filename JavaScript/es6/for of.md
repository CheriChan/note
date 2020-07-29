for...of 可以遍历 数组，Set，Map，类数组对象arguments，Generator对象，字符串

```js
let arr = ['a','b','c','d']
for (let v of arr){
    console.log(v) //a b c d
}
for (let [i,v] of arr.entries()){
    console.log(i,v) //0 'a' 1 "b" 2 "c" 3 "d"
}
```

