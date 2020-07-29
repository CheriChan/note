构造函数Class

```js
class Foo{
    static bar(){
        this.baz()
    }
    static baz(){
        console.log('hello')
    }
    baz(){
        console.log('world')
    }
}
Foo.bar() // hello
```

