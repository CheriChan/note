# Vue中key的作用

key是vue中vnode的唯一标记，通过这个key，diff操作可以更快速更准确

利用 key 的唯一性生成 map 对象来获取对应节点，比遍历方式更快

由于在浏览器中操作DOM是很昂贵的，频繁的操作DOM，会产生一定的性能问题，这就是虚拟DOM的产生原因。

虚拟DOM （Virtual DOM）本质就是用一个原生的JS对象去描述哟个DOM节点。是对真是DOM的一层抽象

VirtualDOM映射到真实的DOM要经历VNode的create，diff，patch

Key的作用是尽可能的复用DOM元素

