# webpack配置

## 项目分为生产模式，开发模式，全局模式均在build文件夹下
* webpack.base.conf.js文件为全局下的
* webpack.dev.conf.js文件为开发模式
* webpack.prod.conf.js文件为生产模式
## webpack.base.conf文件
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
## webpack.dev.conf.js
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
## webpack.prod.conf.js
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