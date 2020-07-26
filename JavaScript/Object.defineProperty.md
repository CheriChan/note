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

