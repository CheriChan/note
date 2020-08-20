```vue
<input v-model = "something"/>
 //等价于
<input v-bind:value="something" v-on:input="something=$event.target.value"/>
```

如果在自定义组件中，v-model 默认会利用名为 value 的 prop 和名为 input 的事件

```vue
//父组件
<template>
	<div id= "app">
        <Children v-model = "number"></Children>
        <!--
可以写成
<Children :value = "number" @input = "number = arguments[0]"></Children>
-->
        <div>
            {{number}}
        </div>
    </div>
</template>
<script>
import Children from 'children.vue'
    export default{
        data(){
            return {
                number:0
            }
        },
        name:"App",
        components:{Children}
    }
</script>
```

```vue
<template>
<div @click = "$emit('input',value+1)">
    孩子
    </div>
</template>
<script>
    export default{
        props:["value"]
    }
</script>
```

