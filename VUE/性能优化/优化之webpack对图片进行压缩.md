# webpack对图片进行压缩

[TOC]



> vue项目中可以在webpack.base.conf.js中的url-loader中设置limit大小来对图片处理，对小于limit的图片转化为base64格式，其余的不做操作。对有些较哒的图片资源，在请求资源的时候加载会很慢，我们可以用image-webpack-loader来压缩图片

## 安装Image-webpack-loader

```node
npm install image-webpack-loader --save-dev
```

配置webpack.base.conf.js文件

```js
{
    test:/\.(png|jpe?g|gif|svg)(\?.*)?$/,
        use:[
            {
                loader:'url-loader',
                options:{
                    limit:10000,
                    name:utils.assetsPath('img/[name].[hash:7].[ext]')
                }
            },
            {
                loader:'image-webpack-loader',
                options:{
                    bypassOnDebug:true
                }
                
            }
        ]
}
```

