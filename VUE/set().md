# Vue.set()

向响应式对象中添加一个 property，并确保这个新 property 同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新 property，因为 Vue 无法探测普通的新增 property (比如 `this.myObject.newProperty = 'hi'`)```

```vue
<template>
   <div>
    <p v-for="item in data">
        
    </p>
    <button @click="btn()">
        添加
    </button>
    </div>
</template>
<script>
export default{
    data(){
        return {
            data:['a','b','c']
        }
    },
    methods：{
    btn(){
        this.$set(this.data,3,'e')
    }
  }
}
</script>
```

