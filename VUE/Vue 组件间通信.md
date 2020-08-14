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



