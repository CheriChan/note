# 父子组件通信

[TOC]



## ① props/$emit 适用父子组件通信

------

## ②ref与$parent/$children适用与父子组件通信

+ ref：如果在普通的DOM元素上使用，引用指向就是DOM元素；如果用在子组件上，引用指向就是组件实例

+ $parent/$children ：访问父/子实例

$refs 的使用方法就是在元素或组件标签上添加ref属性指定一个引用信息，引用信息将会注册在父组件的$refs对象上，在js中使用$refs来指向DOM元素或组件实例；

```vue
<template>
<div class="parent">
    <p ref="testTxt">
        {{oldMsg}}
    </p>
    <button @cilck="changeMsg()">
        点击修改段落内容
    </button>
 </div>
</template>
<script>
export default{
    data(){
        return {
            oldMsg:"这是原有段落数据内容",
            newMsg:"hello,这是新的段落数据内容"
        }
    }，
  methods:{
    changeMsg(){
        this.$refs.testTxt.innerHTML = this.newMsg
        
    }
}
}
</script>
```

**通过$ref实现对子组件操作**

```vue
<template>
  <div class="parent">
    <Childone ref = "childItemId"></Childone>
    <p style="color:blue">{{msgFromChild}}</p>
    <button @click = "getChildMsg()"> 使用$refs获取子组件的数据 </button>
  </div>
</template>
<script>
import Childone from './childone'
    export default {
        components:{Childone},
        data(){
            return {msgFromChild:""}
        },
        methods:{
           getChildMsg(){
                this.msgFromChild = this.$refs.childItemId.childMsg
            }
        }
    }
</script>
```

```vue
<template>
<div class = "child">
    
    </div>
</template>
<script>
    export default{
        data(){
            return{
                childMsg:"这是子组件的参数"
            }
        }
    }
</script>
```

**$parent,$children 的使用**

使用$children获取子组件的属性或函数，我们可以使用$children[i].paramsName 来获取某个子组件的属性值或函数，$children返回的是一个子组件数组

```vue
//父组件
<template>
	<div>
        <Childone></Childone>
        <Childtwo></Childtwo>
        <p style="color:blue;">
            {{msgFromChild}}
	    </p>
        <button @click="getChildMsg()">
            使用$children 来获取子组件数据
   	    </button>
    </div>
</template>
<script>
import Childone from './childone'
    import Childtwo from './childtwo'
    export default {
    components:{Childone,Childtwo},
        data(){
            return {
                msgFromChild:""
            }
        },
        methods:{
            getChildMsg(){
                this.msgFromChild = this.$children[1].child2Msg
            }
        }
    }
</script>
```

使用$parent 来获取修改父组件的数据内容

```vue
//父组件
<template>
	<div class="parent">
        <Childone></Childone>
    </div>
</template>
<script>
import Childone from './childone'
export default {
    components:{Childone},
    data(){
        return {parentMsg:'这是父组件的数据'}
    }
}
</script>
```

```vue
//子组件
<template>
	<div class="child">
        <p class="color:red;">
            {{msgFromParent}}
   	    </p>
        <button @click="getParentMsg()">
            点击使用$parent获取父组件数据
        </button>
    </div>
</template>
<script>
export default{
    data(){
        return {msgFromParent:''}
    },
    methods:{
        getParentMsg(){
            this.$parent.parentMsg="子组件可以修改父组件的内容，这是通过子组件修改所得"
            this.msgFromParent = this.$parent.parentMsg
        }
    }
}
</script>
```

**WARN**

当使用v-for的元素或组件，引用信息$refs将时包含DOM节点的或组件实例的数组，类似$children的使用

$refs不能在created生命周期中使用，因为在组件创建的时候，ref还没有绑定元素

$refs是非响应的，避免在模板或者计算属性中使用$refs，这是一个直接操作组件的应急方案

## ③EventBus ($emit/$on) 适用于父子，隔代，兄弟组件通信

EventBus是一个开源库，利用发布/订阅着模式来对项目进行解耦。利用少量的代码实现多组件通信



用EventBus通信的优点

+ 简化了组件间交流的方式
+ 对事件通信双方进行解耦
+ 可以灵活方便的指定工作线程
+ 速度快，性能好
+ 库比较小，不占内存
+ 功能多，使用方便

如何使用EventBus？

创建方法一

```js
import Vue from 'vue'
export  const EventBus = new Vue()
//引入 Vue 并导出它的一个实例。实质上它是一个不具备 DOM 的组件，它具有的仅仅只是它实例方法而已，因此它非常的轻便
```

创建方法二

```js
Vue.prototype.$EventBus = new Vue()
//这种方式初始化的 EventBus 是一个 全局的事件总线 
```



使用到eventBus的组件都引入eventBus.js 文件

```javascript
import eventBus from './eventBus.js'
```

假设你有两个子组件： `DecreaseCount` 和 `IncrementCount` ，分别在按钮中绑定了 `decrease()`和 `increment()` 方法。这两个方法做的事情很简单，就是数值递减（增） `1` ，以及角度值递减（增） `180` 。在这两个方法中，通过 `EventBus.$emit(channel: string, callback(payload1,…))` 监听 `decreased` （和 `incremented` ）频道

```vue
<!--DecreaseCount.vue-->
<template>
	<button @click= "decrease()">
    减
    </button>
</template>
<script>
import {EventBus } from 'eventBus.js'
    export default{
        data(){
            return {
                num:1,
                deg:180
            }
        },
        methods:{
            decrease(){
            EventBus.$emit('decreased',{
                num:this.num,
                deg:this.deg
            })
        }
        }
    }
</script>
```

```vue
<!--IncreasementCount.vue-->
<template>
<button @click='increase()'>
    加
    </button>
</template>
<script>
import {EventBus} from 'eventBus.js'
    export default{
        data(){
            return {
                num:1,
                deg:180
            }
        },
        methods:{
            increment(){
                EventBus.$emit("incremented", {
                    num:this.num,
                    deg:this.deg
                })
            }
        }
    }
</script>
```

`DecreaseCount` 和 `IncrementCount` 分别发送出了 `decreased` 和 `incremented`频道。接下来，我们需要在另一个组件中接收这两个事件，保持数据在各组件之间的通讯。

```vue
<!--父组件-->
<template>
    <div id="app">
        <div class="container" :style="{transform: 'rotateY(' + degValue + 'deg)'}">
            <div class="front">
                <div class="increment">
                    <IncrementCount />
                </div>
                <div class="show-front"> {{fontCount}} </div>
                <div class="decrement">
                    <DecreaseCount />
                </div>
            </div>
 
            <div class="back">
                <div class="increment">
                    <IncrementCount />
                </div>
                <div class="show-back"> {{backCount}} </div>
                <div class="decrement">
                    <DecreaseCount />
                </div>
            </div> 
        </div>
    </div>
</template>
<script>
 import IncrementCount from "./components/IncrementCount";
    import DecreaseCount from "./components/DecreaseCount";
    import { EventBus } from "eventBus.js";
     export default {
        name: "App",
        components: {
            IncrementCount,
            DecreaseCount
        },
        data() {
            return {
                degValue:0,
                fontCount:0,
                backCount:0
            };
        },
        mounted() {
            EventBus.$on("incremented", ({num,deg}) => {
                this.fontCount += num
                this.$nextTick(()=>{
                    this.backCount += num
                    this.degValue += deg;
                })
            });
            EventBus.$on("decreased", ({num,deg}) => {
                this.fontCount -= num
                this.$nextTick(()=>{
                    this.backCount -= num
                    this.degValue -= deg;
                })
            });
        }
    }; 

</script>
```

移除事件监听

```js
import {EventBus} from 'eventBus'
EventBus.$off('decreased',{})
```

`EventBus.$off(‘decreased’)` 来移除应用内所有对此事件的监听。或者直接调用`EventBus.$off()` 来移除所有事件频道， **注意不需要添加任何参数**

**全局EventBus**



