## es6 to es5
项目目录
* dist  `打包后的文件路径`
* node_modules  `包路径`
* src `源代码路径`
  --components --layer (layer.html,layer.js,layer.less) `组件`
* index.html
* webpack.config.js webpack 配置文件
* package.json  项目配置文件
1. 安装babel `npm install --save-dev babel-loader babel-core`  
babel能将es6语法转为浏览器识别的es5
2. 安装插件babel Latest preset `npm install --save-dev babel-preset-latest`
Latest Presets 能通过参数的设置识别es7,es6，es5语法,而不必单独设置
3. webpack.config.js文件
```
var webpack =require('html-webpack-plugin');
module.exports = {
    entry: './src/app.js',
    output: {
        path: __dirname + '/dist',
        filename: 'js/[name]-bundle.js'
    },
    module:{
        loaders:[
            {
                test: /\.js$/,
                loader: 'babel-loader',
                query:{
                    presets: ['latest']
                }
            }
        ]
    },
    plugins:[
        new webpack({
            filename: 'index.html',
            template: 'index.html',
            inject: 'body'
        })
    ]
}
```
上述配置文件我们使用了babel-loader，利用test参数匹配到的所有的.js文件，使用babel将他们转为浏览器识别的es5代码。
之前我们打包css文件时使用的css-loader，也可以使用这种方式打包
