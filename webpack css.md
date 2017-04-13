项目目录

* dist 打包后的文件路径
* node_modules 包路径
* src 源代码路径 

  --components --layer (layer.html,layer.js,layer.less) 组件

  --css(common.css) css文件
  --app.js 入口js文件
* index.html
* webpack.config.js webpack 配置文件
* package.json 项目配置文件
1. 参考简介，安装css-loader和style-loader
2. 在入口文件 app.js 中引入 要处理的 common.css 
  `import './css/common.css';`
3. 安装postcss-loader `npm install postcss-loader --save-dev`
   postcss-loader 是一个后处理我们的css，譬如加兼容性写法的前缀
3. 配置`webpack.config.js`
```
var webpack =require('html-webpack-plugin');
var path = require('path');
module.exports = {
    entry: './src/app.js',
    output: {
        path: __dirname + '/dist',
        filename: 'js/[name]-bundle.js'
    },
    module:{
        rules:[
            {
                test: /\.js$/,
                loader: 'babel-loader',

                exclude:  path.resolve(__dirname, 'node_modules'), 
                include: path.resolve(__dirname, 'src'),
                query:{
                    presets: ['latest']
                }
            },
            {
                test: /\.css$/,
                use: ['style-loader','css-loader?importLoaders=1',
                    {
                        loader: 'postcss-loader',
                        options: {
                            plugins: function () {
                                return [require('autoprefixer')];
                            }
                        }
                    }
                ]
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

## 关于[postcss-loader](https://www.npmjs.com/package/postcss-loader) 
* autoprefixer 是其中的一个插件，用于给某些css加兼容性前缀，
* ?importLoaders=1 作用是，当我们在css文件中引用其他css文件，使postcss-loader也作用于引入的css文件。
