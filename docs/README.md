# webpack搭建vue

> [!项目地址](https://github.com/wuhaohao1234/vueConfigTravis/tree/dev)

> webpack4

> typescript

> vue

## 一、webpack4基础

### 环境部署

1. 项目初始化以及增加webpack

`yarn init -y && yarn add webpack webpack-cli -D`

#### 1、在webpack4中主要分为生产环境(production)与开发环境(development)

    本项目所有webpack配置均在build文件中,一共分为3个文件
    * webpack.base.conf.js开发环境与生产环境都需要继承的webpack文件
    * webpack.prod.conf.js生产环境使用的webpack
    * webpack.dev.conf.js开发环境使用的webpack

#### 2、webpack所用到的知识点

1. 文件的入口与出口

```
const path = require('path')

module.exports = {
    entry: {
        bundle: path.resolve(__dirname,'../src/main.ts') //文件入口地址
    },
    output: {
        path: path.resolve(__dirname,'../dist'), //打包后到dist目录
        publicPath: '/dist/',
        filename: '[name].js'  //新的文件名
    }
}
```

2. 上述文件提到入口文件是main.ts文件，所以需要本地安装typescript,以及将ts编译为js的loader
`yarn add typescript ts-loader -D`

ts编译为js需要tsconfig.json文件

`tsc --init`

* tsconfig.json修改后的配置

```
<script>
{
  "compilerOptions": {
    /* Basic Options */
    "target": "es5", /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017','ES2018' or 'ESNEXT'. */
    "module": "commonjs", /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'. */
    // "lib": [],                             /* Specify library files to be included in the compilation. */
    // "allowJs": true,                       /* Allow javascript files to be compiled. */
    // "checkJs": true,                       /* Report errors in .js files. */
    // "jsx": "preserve",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    "declaration": true, /* 自动生成.d.ts的声明文件. */
    // "declarationMap": true,                /* Generates a sourcemap for each corresponding '.d.ts' file. */
    "sourceMap": true, /* Generates corresponding '.map' file. */
    // "outFile": "./",                       /* Concatenate and emit output to single file. */
    "outDir": "./dist", /* Redirect output structure to the directory. */
    // "rootDir": "./",                       /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    // "composite": true,                     /* Enable project compilation */
    "removeComments": true, /* 编译后的文件没有注释. */
    // "noEmit": true,                        /* Do not emit outputs. */
    // "importHelpers": true,                 /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,            /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,               /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */
    /* Strict Type-Checking Options */
    "strict": true, /* Enable all strict type-checking options. */
    "noImplicitAny": true, /* any类型的变量不通过 */
    // "strictNullChecks": true,              /* Enable strict null checks. */
    // "strictFunctionTypes": true,           /* Enable strict checking of function types. */
    // "strictPropertyInitialization": true,  /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                  /* Parse in strict mode and emit "use strict" for each source file. */
    /* Additional Checks */
    // "noUnusedLocals": true,                /* Report errors on unused locals. */
    // "noUnusedParameters": true,            /* Report errors on unused parameters. */
    // "noImplicitReturns": true,             /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,    /* Report errors for fallthrough cases in switch statement. */
    /* Module Resolution Options */
    "moduleResolution": "node", /* 使用的模块: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    // "baseUrl": "./",                       /* Base directory to resolve non-absolute module names. */
    // "paths": {},                           /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    // "rootDirs": [],                        /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                       /* List of folders to include type definitions from. */
    // "types": [],                           /* Type declaration files to be included in compilation. */
    // "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true, /* 通过为所有导入创建命名空间对象，实现commonjs和es模块之间的互操作性 */
    // "preserveSymlinks": true,              /* Do not resolve the real path of symlinks. */
    /* Source Map Options */
    // "sourceRoot": "",                      /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                         /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,               /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                 /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */
    /* Experimental Options */
    "experimentalDecorators": true, /* 可以 使用es7装饰器. */
    // "emitDecoratorMetadata": true,         /* Enables experimental support for emitting type metadata for decorators. */
  },
  "include": [ /* 入口文件 */
    "src/**/*.ts",
    "src/**/*.tsx",
    "src/**/*.vue",
    "tests/**/*.ts",
    "tests/**/*.tsx"
  ],
  "exclude": [
    "node_modules",
    "dist",
    "**/*.spec.ts"
  ]
}
</script>
```

* webpack.base.conf.js中使用ts-loader去识别.ts文件

```
<script>
const path = require('path')
module.exports = {
    entry: {
        bundle: path.resolve(__dirname,'../src/main.ts')
    },
    output: {
        path: path.resolve(__dirname,'../dist'),
        publicPath: '/dist/',
        filename: '[name].js'
    },
    module: {  //使用的模块
        rules: [  //匹配的规则
            {
                test: /\.tsx?$/,  //以.ts文件结尾或者.tsx文件结尾，这里使用正则表达式
                loader: 'ts-loader', //使用的loader
                exclude: /node_modules/, //排除的目录
                options: { //选项，这里可以增加.vue结尾的文件
                    transpileOnly: true, 
                    appendTsSuffixTo: [/.vue$/]
                }
            }
        ]
    }
}
</script>
```

* 新建src目录并在src下新建main.ts文件,package.json中增加scripts字段

* src/main.ts

```
<script>
console.log('webpack vue ts')
</script>
```

* package.json

```
"scripts": {
    "build": "webpack --config build/webpack.base.conf.js"
}
```
* 执行build命令

`yarn build`

## 二、结合vue 

### 需要安装的模块

* vue

* vue-loader

* css-loader(由于vue中必须有css-loader)

* vue-template-compiler(识别template的模板字符串)

* html-webpack-plugin(为了展示到页面上，必须有一个html文件)

### 在webpack.base.conf.js中添加

```
<script>
const path = require('path')
const VueLoaderPlugin = require('vue-loader/lib/plugin')  
const HtmlWebpackPlugin = require('html-webpack-plugin')  //html模板
module.exports = {
    entry: {
        bundle: path.resolve(__dirname,'../src/main.ts')
    },
    output: {
        path: path.resolve(__dirname,'../dist'),
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
        new VueLoaderPlugin(),  //这里必须要加入
        new HtmlWebpackPlugin({   //下面会创建public/index.html作为模板文件
            template: path.resolve(__dirname, '../public/index.html')
        })
    ],
    resolve: {  
        extensions: ['.ts', '.js', '.vue', '.json'],  //扩展
        alias: {
            'vue$': 'vue/dist/vue.esm.js'
        }
    }
}
</script>
```

### 全局下创建模板public/index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>vue-webpack-ts</title>
</head>
<body>
    <div id="root"></div>  //作为入口
</body>
</html>
```

### 写入src/App.vue,并给App.vue写声明文件App.d.ts

    在ts中，为了识别例如图片,其它文件，必须写入声明，可以到typescript官网

* src/App.vue

```
<template>
    <div>
        <h1>webpack-vue-ts</h1>
    </div>
</template>

<script>
export default {
    name: 'App'
}
</script>

```

* src/App.d.ts

```
<script>
declare module '*.vue' {
    import Vue from 'vue'
    export default Vue
}
</script>
```

### src/main.ts引入

```
<script>
import Vue from 'vue';
import App from './App.vue';

new Vue({
    render: (h) => h(App),
}).$mount('#root'); 挂载到div#root
</script>
```

## 三、加入vue-property-decorator

    vue-property-decorator基于vue-class-component，可以参考github上面的说明

`yarn add vue-property-decorator vue-class-component -D`

* 修改App.vue

```
<template>
    <div class="app" >
        <h1>webpack搭建ts中的vue</h1>
    </div>
</template>

<script lang="ts">
import {Vue,Component} from "vue-property-decorator"
@Component({})
export default class App extends Vue {
    
}
</script>
<style>
html,body{
    height: 100%;
}
.app{
    padding: 2rem;
}
body{
    background:linear-gradient(to bottom, #1867c0, #5CBBF6);
    color: #fff;
    font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
}
h1{
    text-align: center;
}
</style>
```

## 四、分别增加生产环境与开发环境

### 需要安装的插件

1. webpack-merge合并webpack配置

2. clean-webpack-plugin清理dist目录下的文件

* build/webpack.dev.conf.js

```
<script>
const merge = require('webpack-merge');
const path = require('path');
const baseConfig = require('./webpack.base.conf');
const webpack = require('webpack')
module.exports = merge(baseConfig, {  //合并webpack.base.conf的配置
    mode: 'development',  //声明这是开发环境
    devtool: 'inline-source-map',  //开发时候需要对应的每一行代码
    devServer: {
        contentBase: path.resolve(__dirname, '../dist'),  //这里用了一个devServer,可以开启一个服务，dist里面的文件会被显示
        open: true
    },
    plugins: [  //这里是一个热重载，每次编辑文件页面都会更新
        new webpack.HotModuleReplacementPlugin()
    ]
});
</script>

```

* build/webpack.prod.conf.js

```
<script>

const merge = require('webpack-merge');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const baseConfig = require('./webpack.base.conf');

module.exports = merge(baseConfig, {
    mode: 'production',
    devtool: 'source-map',
    plugins: [
        new CleanWebpackPlugin()  //每次清理dist目录
    ]
})
</script>
```

* 修改package.json中scripts字段

```
"scripts": {
    "build": "webpack --config build/webpack.prod.conf.js",
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js"
}
```

## 后续:

1. 可以尝试加入vuex

2. 可以尝试加入vue-router

3. 可以尝试加入sass

4. 可以优化

5. tslint代码格式

6. jest单元测试