# vueConfigTravis
[![Build Status](https://travis-ci.org/wuhaohao1234/vueConfigTravis.svg?branch=dev)](https://travis-ci.org/wuhaohao1234/vueConfigTravis)
## 包含

1. webpack4
2. travis-ci
3. jest
4. vue
5. typescript
6. git-commit格式规范

## 起步

### 依赖项
* babel-core 
* babel-loader 
* babel-preset-env 
* clean-webpack-plugin 每次清理打包的文件

* css-loader vue-loader必须的
* html-webpack-plugin html模板，用于展示信息 
* ts-loader ts预处理器
* tslint ts语法测试
* typescript 本地安装ts
* vue 
* vue-class-component 
* vue-loader 
* vue-property-decorator 
* vue-template-compiler vue模板字符串

* webpack 
* webpack-cli 
* webpack-merge 

## webpack配置

1. webpack.base.conf.js模板
```
const path = require('path')
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    entry: {
        bundle: path.resolve(__dirname, '../src/main.ts')
    },
    output: {
        path: path.resolve(__dirname, '../dist'),
        publicPath: '/dist/',
        filename: '[name].js'
    },
    module: {
        rules: [
            {
                test: /\.tsx?$/,
                loader: 'ts-loader',
                exclude: /node_modules/,
                options: {
                    transpileOnly: true,
                    appendTsSuffixTo: [/.vue$/]
                }
            },
            {
                test: /\.vue$/,
                loader: 'vue-loader'
            },
            {
                test: /\.css$/,
                use: ['vue-style-loader', 'css-loader']
            }
        ]
    },
    plugins: [
        new VueLoaderPlugin(),
        new HtmlWebpackPlugin({
            template: path.resolve(__dirname, '../public/index.html')
        })
    ],
    resolve: {
        extensions: ['.ts', '.js', '.vue', '.json'],
        alias: {
            'vue$': 'vue/dist/vue.esm.js'
        }
    }
}
```
2. 生产环节配置webpack配置
```
const merge = require('webpack-merge');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const baseConfig = require('./webpack.base.conf');

module.exports = merge(baseConfig,{
    mode: 'production',
    devtool: 'source-map',
    plugins: [
        new CleanWebpackPlugin()
    ]
})
```
3. 开发环境配置
```
const merge = require('webpack-merge');
const path = require('path');
const baseConfig = require('./webpack.base.conf');
const webpack = require('webpack')
module.exports = merge(baseConfig, {
    mode: 'development',
    devtool: 'inline-source-map',
    devServer: {
        contentBase: path.resolve(__dirname, '../dist'),
        open: true
    },
    plugins: [
        new webpack.HotModuleReplacementPlugin()
    ]
});
```
## 3