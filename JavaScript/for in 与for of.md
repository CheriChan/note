## for...in 

for..in 并不适合用来遍历数组

+ 遍历的是数组的索引
+ 索引为字符串型数字，不能直接进行集合运算
+ 可以遍历数组的所有可枚举属性，包括原型
+ 遍历的顺序可能不是按照实际数组的内部顺序

```js
Array.prototype.method = function(){
    console.log(this.length)
}
var myArr = [1,2,3,4,5]
myArr.name = "array"
for(var value in myArr){
    console.log(value)
}
//0 1 2 3 4 name method
```

for...in 遍历对象是遍历的键名

```javascript
Object.prototype.method = function(){
console.log(this)
}
var myObj = {
    a:1,b:2,c:3
}
for(var key in myObj){
    console.log(key)
}
//a b c method
```

for in 可以遍历到myObject的原型方法method,如果不想遍历原型方法和属性的话，可以在循环内部判断一下,hasOwnPropery方法可以判断某属性是否是该对象的实例属性

```js
for (var key in myObj) {
　　if（myObj.hasOwnProperty(key)){
　　　　console.log(key);
　　}
}
```

同样可以通过ES5的Object.keys(myObject)获取对象的实例属性组成的数组，不包括原型方法和属性

```js
Object.prototype.method=function(){
　　console.log(this);
}
var myObj={
　　a:1,
　　b:2,
　　c:3
}
Object.keys(myObj) //["a","b","c"]
```

## for...of

for..of适用遍历数组对象/字符串/map/set等拥有迭代器对象的集合.但是不能遍历对象,因为没有迭代器对象.与forEach()不同的是，它可以正确响应break、continue和return语句

for-of循环不支持普通对象，但如果你想迭代一个对象的属性，你可以用for-in循环（这也是它的本职工作）或内建的Object.keys()方法：

```js
let obj = {a:1,b:2,c:3}
for(var key of Object.keys(obj)){
    console.log(key)
}
//a b c
```

for...of 遍历数组

```js
let arr = [1,2,3,4]
for(let val of arr){
    console.log(val)
}
//1 2 3 4
```

遍历字符串

```js
let str = "abcdefg"
for(let val of str){
    console.log(val) // a b c d e f g 
}
```

