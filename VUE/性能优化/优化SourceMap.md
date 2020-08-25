# 优化SourceMap

[TOC]



> 经过压缩，去掉多余空格，babel编译后。最终将编译得到的代码用于线上环境，处理后的代码和源代码会有很大的差别。有bug时，只能定位到压缩处理后的代码位置，无法定位到开发环境中的代码，对于开发来说不好调试定位问题，因此sourceMap出现了。
>
> SourceMap是一种映射关系。当项目运行后，如果出现错误，我们可以利用sourceMap反向定位到源码。

 

| 模式                    | 解释                                                         |
| ----------------------- | ------------------------------------------------------------ |
| eval                    | 每个module会封装到 eval 里包裹起来执行，并且会在末尾追加注释 `//@ sourceURL`. |
| source-map              | 生成一个**SourceMap**文件（编译速度最慢）                    |
| hidden-source-map       | 和 source-map 一样，但不会在 bundle 末尾追加注释.            |
| inline-source-map       | 生成一个 **DataUrl** 形式的 SourceMap 文件.                  |
| eval-source-map         | 每个module会通过eval()来执行，并且生成一个DataUrl形式的SourceMap. |
| cheap-source-map        | 生成一个没有列信息（column-mappings）的SourceMaps文件，不包含loader的 sourcemap（譬如 babel 的 sourcemap） |
| cheap-module-source-map | 生成一个没有列信息（column-mappings）的SourceMaps文件，同时 loader 的 sourcemap 也被简化为只包含对应行的。 |



## 开发环境推荐： cheap-module-eval-source-map

## 生产环境推荐： cheap-module-source-map

**cheap**： 源代码中的列信息是没有任何作用，因此我们打包后的文件不希望包含列相关信息，只有行信息能建立打包前后的依赖关系。因此不管是开发环境或生产环境，我们都希望添加 cheap 的基本类型来忽略打包前后的列信息；

**module** ：不管是开发环境还是正式环境，我们都希望能定位到bug的源代码具体的位置，比如说某个 Vue 文件报错了，我们希望能定位到具体的 Vue 文件，因此我们也需要 module 配置；

**soure-map** ：source-map 会为每一个打包后的模块生成独立的 soucemap 文件 ，因此我们需要增加source-map 属性；

**eval-source-map**：eval 打包代码的速度非常快，因为它不生成 map 文件，但是可以对 eval 组合使用 eval-source-map 使用会将 map 文件以 DataURL 的形式存在打包后的 js 文件中。在正式环境中不要使用 eval-source-map, 因为它会增加文件的大小，但是在开发环境中，可以试用下，因为他们打包的速度很快

## webpack2.0+配置sourceMap

```js
const buildConfig= {
    mode:"production",
    output:{
        path:distPath,
        fileName:"./js/[name].[hash].min.js",
        pubulicPath:"./"
    },
    plugins:[
        new UglifyJSPlugin({
            sourceMap:true
        })
    ].concat(baseCibfug.htmlArray),
    devtool:"source-map"
}
```

## webpack4.0+配置sourceMap

 webpack配置 devtool: "source-map" 生成的map代码没有 sourcesContent，没有sourcesContent的结果是你只能定位要压缩代码的位置，无法定位到源码的位置

```js
const buildConfig={
    mode:"production",
    output :{
        path:distPath,
        filename:"./js/[name].[hash].min.js",
        publicPath:"./"
    },
    optimization:{
        minimize:false
    },
    plugins:[
        
    ].concat(baseConfig.htmlArray),
    devtoll:"source-map"
}
```






