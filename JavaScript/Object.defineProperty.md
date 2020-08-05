# object.defineProperty()

Object.defineProperty(obj, prop,desc)

+ obj =>在该对象上定义属性

+ prop=> 要定义或修改的属性名

+ desc =>要定义或修改的属性的描述符

  | 属性描述符   | 说明                                                         | 默认值    |
  | :----------- | ------------------------------------------------------------ | --------- |
  | value        | 该属性的值                                                   | undefined |
  | get          | 一个给属性提供 getter 的方法，如果没有 getter 则为 undefined | undefined |
  | set          | 一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。当属性值修改时，触发执行该方法 | undefined |
  | writable     | 当且仅当该属性的writable为true时，value才能被赋值运算符改变。默认为 false | false     |
  | enumerable   | enumerable定义了对象的属性是否可以在 for...in 循环和 Object.keys() 中被枚举 | false     |
  | Configurable | configurable特性表示对象的属性是否可以被删除，以及除value和writable特性外的其他特性是否可以被修改 | false     |


### 手写const

```javascript
function _const(key,value)
{
  let desc = {
    value,
    writable:false
  }
  Object.defineProperty(window,key,desc)
}
_const('qq',666)

console.log(qq) // 666

qq = 888 //Identifier 'qq' has already been declared (已声明标识符'qq')
```

以上相当于

```javascript
const qq = 666
```

### 拦截对象

```js
let obj = {name:'',age:'',sex:''},
    defaultName = ["这是姓名默认值1","这是年龄默认值1","这是性别默认值1"];
Object.keys(obj).forEach(key=>{
    Object.defineProperty(obj,key,{
        get(){
            return defaultName
        },
        set(value){
            defaultName = value
        }
    })
})
 console.log(obj.name); //["这是姓名默认值1", "这是年龄默认值1", "这是性别默认值1"]
  console.log(obj.age); //["这是姓名默认值1", "这是年龄默认值1", "这是性别默认值1"]
  console.log(obj.sex); //["这是姓名默认值1", "这是年龄默认值1", "这是性别默认值1"]
  obj.name = "这是改变值1";
  console.log(obj.name); //这是改变值1
  console.log(obj.age);  //这是改变值1
  console.log(obj.sex)   //这是改变值1

```

```js
let a={};
bValue=1;
Object.defineProperty(a,"b",{
    set:function(value){
        bValue=value;
        console.log("setted");
    },
    get:function(){
        return bValue;
    }
});
a.b;//1
a.b=[];//setted
a.b=[1,2,3];//setted
a.b[1]=10;//无输出
a.b.push(4);//无输出
a.b.length=5;//无输出
a.b;//[1,10,3,4,undefined];

```

不能监听数组索引赋值和改变长度的变化 必须深层遍历嵌套的对象,因为defineProterty只能劫持对象的属性,因此我们需要对每个对象的每个属性进行遍历，如果属性值也是对象那么需要深度遍历,显然能劫持一个完整的对象是更好的选择