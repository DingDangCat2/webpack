1.webpack项目内安装只能使用终端进行操作，除非在全局也安装相同版本的webpack。
2.webpack配置文件（webpack-config.js）请在项目根文件夹下安装。
其可以对webpack进行配置。
module.exports={
       devtool: 'eval-source-map',//devtool的作用是生成source map，可以知道出现错误的地方是在原文件的哪一行列，而不是打包后的文件行列。
        entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
    output: {
      path: __dirname + "/public",//打包后的文件存放的地方
      filename: "bundle.js"//打包后输出文件的文件名
                            //“__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录
    },
        devServer: {
        contentBase:"./public",//为public文件夹设置本地服务器。
        historyApiFallback:true,//单页面SPA应用时有用，所有history跳转都指向index.html
        inline:true,        //自动刷新页面

    }
                            weback-dev-server插件能提供一个本地服务器，请单独安装此依赖，相关配置能监听代码的改变并自动刷新页面。//
}
//启动本地服务器：webpack-dev-server --open
//注意：启动本地服务器依赖webpack-cli,且使用webpack4的话webpack-cli请用3.


Loaders需要单独安装并且需要在webpack.config.js中的modules关键字下进行配置，Loaders的配置包括以下几方面：
test：一个用以匹配loaders所处理文件的拓展名的正则表达式（必须）
loader：loader的名称（必须）
include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
query：为loaders提供额外的设置选项（可选）

例如：转换es6的代码以及解析jsx语法。
使用babel：需要安装babel需要的依赖：npm install --save-dev babel-core babel-loader babel-preset-env babel-preset-react
module: {
        rules: [
            {
                test: /(\.jsx|\.js)$/,
                use: {
                    loader: "babel-loader",
                    options: {
                        presets: [
                            "env", "react"
                        ]
                    }
                },
                exclude: /node_modules/
            }
        ]
    }


插件：plugins和loaders是不一样的，
Loaders和Plugins常常被弄混，但是他们其实是完全不同的东西，可以这么来说，loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。

Webpack有很多内置插件，同时也有很多第三方插件
  plugins: [
        new webpack.BannerPlugin('版权所有，翻版必究')
    ],


    热模块替换：
     devServer: {
        contentBase: "./public",//本地服务器所加载的页面所在的目录
        historyApiFallback: true,//不跳转
        inline: true,
        hot: true
    },
       plugins: [
        new webpack.BannerPlugin('版权所有，翻版必究'),
        new HtmlWebpackPlugin({
            template: __dirname + "/app/index.tmpl.html"//new 一个这个插件的实例，并传入相关的参数
        }),
        new webpack.HotModuleReplacementPlugin()//热加载插件
    ],