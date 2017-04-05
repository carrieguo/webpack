# webpack
## 安装 
1. 在工程目录下初始化npm `npm init`,生成一个package.json文件 `注意package.json文件中"name": "webpack",会报错，“name”字段不能命名为“webpack”`
2. npm install webpack，生成一个node_modules目录
## npm命令打包文件
* webpack + 要打包的文件名 + 打包后的文件名 `webpack hello.js hello.bundle.js`
* 如果没有全局安装webpark,可以使用项目目录下的webpack运行  `.\node_modules\.bin\webpack hello.js hello.bundle.js`
* 全局安装webpack `npm install webpack -g`
### 打包多个js文件
假如有两个js文件需要打包，`hello.js`,`world.js`
1. 在hello.js中 `require('./world.js');`
2. 打包 `webpack hello.js hello.bundle.js`
### 打包css文件
假如分别有一个js文件和一个css文件
1. 由于webpack不支持css类型，需要借助loader。我们需要在项目目录下安装loader `npm install css-loader style-loader`
2. 在hello.js文件中，引入css并指定相应的loader `require('style-loader!css-loader!./style.css');//loader的顺序貌似不能改`
3. 打包 `webpack hello.js hello.bundle.js`
4. 新建一个index.html 文件，只需要引用 `hello.bundle.js`文件即可查看效果。
* css-loader 作用是使webpack能处理css文件
* style-loader 将处理后的css文件插入到`<head></head>`标签中
### 命令行语句打包
我们也可以在第2步中不指定loader,在命令行工具中打包时临时指定
`webpack hello.js hello.bundle.js --module-bind 'css=style-loader!css-loader'`
其他参数：
* 检测到文件改变时自动更新并打包 `--watch`
* 打包时显示过程 `--progress`
* 打包时列出引用的模块 `--display-modules`
* 列出打包模块的原因 `--display-reasons`
## 配置文件
每次都需要在打包时规定参数太麻烦了，下面我们来使用配置文件。
项目目录：
* dist  `打包后的文件路径`
* node_modules  `包路径`
* src `源代码路径` --js --css
* index.html
* webpack.config.js `webpack 配置文件`
* package.json  `项目配置文件`
```
//webpack.config.js
module.exports = {
	entry: __dirname + "/src/js/hello.js",
	output:{
		path: __dirname + "/dist/js",
		filename: 'bundle.js'
	}
}
```
这样只需要敲入`webpack`就会自动打包
我们也可以修改配置文件名称为`webpack.dev.config.js`,运行命令时通过config 命令指定配置文件 `webpack --config webpack.dev.config.js`。
说明一下entry参数，
* entry 参数为数组
```
entry: ['./entry1','./entry2'] //将两个单独的文件（不存在引用）打包为一个文件
```
* entry 参数为对象
```
entry:{
	page1: './page1',
	page2: ['./entry1','./entry2']
}，
output:{
	filename: '[name].js',         //参数为对象时，使用[id]	[name] [hash] [chunkhash]，可以生成多文件	
kepath: __dirname + '/build'
}
//writes to disk: ./build/page1.js, ./build/search.js
```
### 在package.json中配置运行参数
自定义脚本
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
	"webpacka": "webpack --config webpack.config.js --progress --colors"
  },
```
运行`npm run webpack`

## 使用插件 html-webpack-plugin
### 在html文件中自动引用打包后的文件
使用参数打包文件之后生成的文件需要一个个手动在html文件中添加引用，太麻烦了，我们可以使用插件来解决。
1. 安装插件 `npm install html-webpack-plugin`
2. 配置webpack.config.js文件
```
var webpack =require('html-webpack-plugin'); //声明插件
module.exports = {
    entry: ['./src/js/wold.js', './src/js/main.js'],
    output: {
        path: __dirname + '/dist',  
        filename: 'js/[name]-[chunkhash].js'   //将打包后的js文件生成到js目录下
    },
    plugins:[
        new webpack({
            filename: 'index-[hash].html',  //文件名格式
            template: 'index.html',         //会依照根目录下的index.html文件为模板，在/dist目录下生成.html文件
            inject: 'body'  //将引用的标签放到head标签中or放在body标签中
        })
    ]
}
```
