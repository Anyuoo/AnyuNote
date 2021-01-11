#  vue 

## 1. 0 前端发展史

###  1.1 基本分析

- **逻辑**
  - 判断
  - 循环

- **事件**
  - 浏览器事件：window document
  - Dom事件：增删改遍历，修改节点元素
  - JQuery VUE

- **视图**
  - HTML
  - CSS

- **通信**
  - Ajax
  - Axios

### 1.2 CSS预处理器

==**CSS预处理器**:本质上，预处理器是具有自己独特语法的程序。您编写代码后，他们会将其编译为纯CSS==

**Less 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。Less 可以运行在 Node 或浏览器端**

**Sass是预处理器的鼻祖，它诞生于2006年，很多后来的预处理器都从它这里借鉴了大量思想。**

****

### 1.3 javascript 框架

==**VUE: 结合了Angular(模块化)和React(虚拟DOM)的优点**==



## 2. 0 vue 语法

### 2.0 vue 基础语法

- `v-if`
- `v-else-if`
- `v-else`
- `v-for`
- `v-on ` 可简写为`@` 绑定事件
- `v-bind:key="value"` 可简写`:key="value"` 给组件参数绑定
- `v-model` 数据双向绑定

**组件化**

- 组合组件插槽
- 组件内部绑定事件 `this.emit("事件名"，参数)`
- 计算属性的特色，缓存数据

==遵循SoC关注度分离原则，vue是纯粹的试图框架，并不包含Ajax值类的异步通信功能，解决此问题需要Axios框架做异步通信==

### 2.1 data ，el属性

```html
<body>
<!--view层:html+css-->
<div id="app">
    {{message}}
</div>

</body>
<!--导入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>
<script>

    var vm = new Vue({
        el: "#app",
        //model层
        data:{
            message: "hello,vue"
        }
    });
</script>
```



### 2.2 methods， v-on

```html
<!--view层-->
<div id="app">
    <!--v-on 绑定事件-->
   <button type="button" v-on:click="hello">click me</button>
</div>
```



```javascript
var vm = new Vue({
    el: "#app",
    //model层
    data: {
        message: "hello vue"
    },
    //js方法写在methods中
    methods: {
        hello: function () {
            alert(this.message)
        }
    }
});
```

### 2.3  v-model 双向绑定

```html
<!--view层-->
<div id="app">
    <!--修改input中的内容，message内容也会改变-->
    <input type="text" v-model="message"> {{message}}
</div>
```



```javascript
<script>
    var vm = new Vue({
        el: "#app",
        //model层
        data: {
            message: "hello vue"
        }
    });
</script>
```

### 2.4 component props 参数传递给组件

```HTML
<body>
<!--view层-->
<div id="app">
    //遍历items，将item绑定给i
    <he v-for="item in items" v-bind:i="item"></he>
</div>

</body>
<!--导入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>
<script>
// 定义component
    Vue.component("he",{
        //props接受v-bind传入的值
        props: ['i'],
        template: '<li>{{i}}</li>'
    });
    var vm = new Vue({
        el: "#app",
        //model层
        data: {
            items: ["java","linux","golang"]
        }
    });
</script>
```

### 2.5 异步请求 axois

`vue add axios`

```html
<!DOCTYPE html>
<html lang="en">
<body>
<!--view层-->
<div id="app">
    <span >{{info.golang}}</span>
</div>
</body>
<!--导入vue.js axios-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>
    var vm = new Vue({
        el: "#app",
        //data函数，return得到axios的数据，接受之后可以赋值或者绑定给view层
        data() {
            return {
                //接受格式必须一模一样
                info: {
                    java: null,
                    golang: null
                }
            };
        },
        //挂载函数，在模板渲染之前调用
        mounted(){
           // =>  es6 的语法，链式
            // axios.get(文件).then(响应response => (赋值this.info = response.data))
            axios.get("data.json").then(response => (this.info = response.data));
            //$.psot(); ajax也可以
        }
    });
</script>
</html>
```



### 2.6 computed 属性计算

```html
<!DOCTYPE html>
<html lang="en">
<body>
<div id="vue">
    {{currentTime1()}}
    {{currentTime2}}
</div>
</body>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>
<script>
    var vm = new Vue({
        el: "#vue",
        //方法每次调用都重新计算
        methods: {
            currentTime1: function () {
                return Date.now();
            }
        },
        //计算属性，结果缓存到内存中，每次调用，如果内容不变，结果不会重新计算
        computed: {
            currentTime2: function () {
                return Date.now();
            }
        }
    });
</script>
</html>
```

### 2.7 slot 插槽

1. 定义包含插槽的模板
2. 定义插槽
3. 实例化插槽 并传入数据

```html
<!DOCTYPE html>
<html lang="en">
<body>
<div id="vue">
    //实例化插槽
    <bookstore>
        //对于绑定的参数命名，不能使用-如（b-name,BName）
        //solt 属性不能缺省
        //将data中的值绑定，component便可以通过props属性接受
        <store-name slot="store-name" :sname="Name"></store-name>
        <book-name slot="bookName" v-for="BName in Names" :Bname="BName"></book-name>
    </bookstore>
</div>
</body>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>
<script>
    //组件定义必须在实例化Vue对象之前
    Vue.component("bookstore",{
        //定义包含插槽的模板
        template:
            "<div>\
                <slot name='store-name'></slot>\
                <ul>\
                <slot name='bookName'></slot>\
                </ul>\
            </div>"
    });
    //定义插槽
    Vue.component("store-name",{
        props: ['sname'],
        template: '<div>{{sname}}</div>'
    });
    Vue.component("bookName",{
        props: ['Bname'],
        template: '<li>{{Bname}}</li>'
    });
    new Vue({
        el: "#vue",
        data: {
            "Name": "玄幻",
            "Names": ["book1","book2","book3","BBH","15"]
        }
    });
</script>
</html>
```

### 2.8  组件调用vue对象中方法

**子向父传参** ： this.$emit(自定义事件名（父），参数)

```html
<!DOCTYPE html>
<html lang="en">
<body>
<div id="vue">
    <bookstore>
        <store-name slot="store-name" :type="type"></store-name>
        <book-name slot="book-name" v-for="(bookname,index) in books"
                   :index="index" :bookname="bookname" v-					 on:remove="removeBook(index)">
        </book-name>
    </bookstore>

</div>
</body>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>
<script>
    Vue.component("bookstore",{
        template:
            "<div>\
                <slot name='store-name'></slot>\
                <ul>\
                <slot name='book-name'></slot>\
                </ul>\
            </div>"
    });
    Vue.component("store-name",{
        props: ['type'],
        template: '<div>{{type}}</div>'
    });
    Vue.component("book-name",{
        props: ['bookname','index'],
        //delete是关键字！！！
        template: '<li>{{bookname}} <button @click="removebook">删除</button> </li>',
        methods: {
            removebook: function(index) {
                //调自定义事件
                this.$emit("remove", index);
            },
        }
    });
    var vm= new Vue({
        el: "#vue",
        data: {
            "type": "玄幻",
            "books": ["book1","book2","book3","BBH"]
        },
        methods: {
            removeBook: function (index) {
                console.log("deleted book:"+this.books[index]+"!")
                //从index开始删除一个，添加0
                this.books.splice(index, 1);
            }
        }
    });
</script>
</html>
```

### 2.8　具体结构

```javascript
new Vue({
        el: '#app',
        data: {
            //定義變量和初始值
        },
        created: {
            //頁面渲染錢執行，調用定義的方法


        },
        methods: {
            //定義方法，編寫具體的方法

        },
        mounted: {
            //頁面渲染後執行
        }
    })
```



## 3.0  第一个vue项目

Vue CLI 是一个基于 Vue.js 进行快速开发的完整系统，提供：

- 通过 `@vue/cli` 实现的交互式的项目脚手架。
- 通过 `@vue/cli` + `@vue/cli-service-global` 实现的零配置原型开发。
- 一个运行时依赖 (`@vue/cli-service`)，该依赖：
  - 可升级；
  - 基于 webpack 构建，并带有合理的默认配置；
  - 可以通过项目内的配置文件进行配置；
  - 可以通过插件进行扩展。
- 一个丰富的官方插件集合，集成了前端生态中最好的工具。
- 一套完全图形化的创建和管理 Vue.js 项目的用户界面。

Vue CLI 致力于将 Vue 生态中的工具基础标准化。它确保了各种构建工具能够基于智能的默认配置即可平稳衔接，这样你可以专注在撰写应用上，而不必花好几天去纠结配置的问题。与此同时，它也为每个工具提供了调整配置的灵活性，无需 eject。

### 3.1  安装vue

```shell
# 最新稳定版
$ npm install vue
```

```shell
#安装
npm install -g @vue/cli @vue/cli-service-global
# or
yarn global add @vue/cli @vue/cli-service-global
#创建项目
vue create hello-world
#使用GUI
vue ui
```

```yaml
-root
	-node_modules #library root
	-public #当前项目的公有文件
		-faviconico # 标签图标
		-index.html #当前项目唯一的页面
	-src #源码目录 
		-assert #静态资源，这里的资源会被wabpack构建
		-components #组件目录， .vue文件
		-views #大组件或页面
		-router
			-index.js #路由脚本文件（配置路由 url链接 与 页面组件的映射关系）
		-store
			-index.js #仓库脚本文件（vuex插件的配置文件，数据仓库）
		-main.js #全局脚本文件，入口js文件
		-App.vue #根组件
	-bable.config.js
	-vue.config.js #需要自己添加，vue的配置
	-package.json # npm包配置文件，里面定义了项目的npm脚本，依赖包等信息
	-package-lock.json
```

### 3.2 安装webpack

```shell
#项目中安装
npm install --save-dev webpack
npm install --save-dev webpack@<version>
#如果你使用 webpack 4+ 版本，你还需要安装 CLI。
npm install --save-dev webpack-cli

#全局安装
npm install webpack -g
npm install webpack-cli -g
```

### 3.3 vue.config.js 配置

```js
// vue.config.js
const path = require('path')
const debug = process.env.NODE_ENV !== 'production'

module.exports = {
    // 基本路径
    baseUrl: '/',
    // 输出文件目录
    outputDir: 'dist',
    assetsDir: 'assets', // 静态资源目录 (js, css, img, fonts)
    // eslint-loader 是否在保存的时候检查
    lintOnSave: true,
    // use the full build with in-browser compiler?
    // https://vuejs.org/v2/guide/installation.html#Runtime-Compiler-vs-Runtime-only
    // compiler: false,

    // webpack配置
    // see https://github.com/vuejs/vue-cli/blob/dev/docs/webpack.md   webpack链接API，用于生成和修改webapck配置
    chainWebpack: () => {
        if (debug) {
            // 本地开发配置
        } else {
            // 生产开发配置
        }
    },
    configureWebpack: (config) => {// webpack配置，值位对象时会合并配置，为方法时会改写配置
        if (debug) { // 开发环境配置
            config.devtool = 'cheap-module-eval-source-map'
        } else { // 生产环境配置

        }
        Object.assign(config, { // 开发生产共同配置
            resolve: {
                alias: {
                    '@': path.resolve(__dirname, './src')//设置路径别名
                    //...
                }
            }
        })
    },
    // vue-loader 配置项
    // https://vue-loader.vuejs.org/en/options.html
    // vueLoader: {},

    // 生产环境是否生成 sourceMap 文件
    productionSourceMap: true,
    // css相关配置 配置高于chainWebpack中关于css loader的配置
    css: {
        // 是否使用css分离插件 ExtractTextPlugin
        extract: true,
        // 开启 CSS source maps?是否在构建样式地图，false将提高构建速度
        sourceMap: false,
        // css预设器配置项
        loaderOptions: {},
        // 启用 CSS modules for all css / pre-processor files.
        modules: false
    },
    // use thread-loader for babel & TS in production build
    // enabled by default if the machine has more than 1 cores 构建时开启多进程处理babel编译
    parallel: require('os').cpus().length > 1,
    // 是否启用dll
    // See https://github.com/vuejs/vue-cli/blob/dev/docs/cli-service.md#dll-mode
    // dll: false,

    // PWA 插件相关配置
    // see https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-pwa
    pwa: {},
    // webpack-dev-server 相关配置
    devServer: {
        open: process.platform === 'darwin',
        host: '0.0.0.0',
        port: 8080,
        https: false,
        hotOnly: false,
        proxy: null, // 设置代理
        // before: app => {}
    },
    // 第三方插件配置
    pluginOptions: {
        // ...
    }
}
```

### 3.4 webpack 使用

1. **webpack目录**

```yaml
-root
	-dist #打包压缩后的资源文件，自己配置 output
	-modules #编写的源文件
		-main.js
		-xxx.js
	-webpack.config.js #配置文件
```

2. **暴露方法**

```js
//暴露方法 exports.function name
exports.sayHi =function () {
    document.write("<h1>anyu</h1>")
}
exports.sayHi1 =function () {
    document.write("<h1>anyu</h1>")
}
```

3. **引入方法**

```js
//引入方法 ./ --> 当前目录 require("js") 类似于Java import 类
var hello = require("./hello")
//调用js文件中的方法
hello.sayHi()
hello.sayHi1()
```

4. **webpack.config.js配置**

```js
//打包配置
module.exports ={
    //入口
    entry: './modules/main.js',
    //输出
    output: {
        //输出文件
        filename: "./js/bundle.js"
    }
}
```

5. **打包命令**

命令直接输：`webpack`

### 3.5 vue-router

1. **安装**

```shell 
npm install vue-router
```

2. **如果在一个模块化工程中使用它，必须要通过 `Vue.use()` 明确地安装路由功能：**

```js
import Vue from 'vue'
//导入组件
import VueRouter from 'vue-router'
//显式申明
Vue.use(VueRouter)
```

3. **编写router/index.js配值文件**

```js
import Vue from 'vue'
//导入组件
import VueRouter from 'vue-router'
//显式申明
Vue.use(VueRouter)

export default new VueRouter(options: {
    //routes而不是routers，否则地址栏会变，但是找不到组件
    routes: [
          {
            //路由路径
            path: '/content',
            //名字
            name: 'content',
            //跳转组件
            component: Content
        },
        {
            path: '/main',
            name: 'main',
            component: Main
        }                  
    ]
});
```

4. **main.js全局脚本文件，入口js文件，配置路由**

```js
import Vue from 'vue'
import App from './App.vue'
//引入
import router from './router'
Vue.config.productionTip = false

new Vue({
  render: h => h(App),
    //直接使用
  router,
}).$mount('#app')

```

5. **组件中使用**

```vue
<template>
  <div id="app">
  <h1>anyu</h1>
    <router-link to="/content">内容页</router-link>
    <br>
     //链接
    <router-link to="/main">首页</router-link>
     //视图
    <router-view></router-view>
  </div>
</template>
```

## 4.0 Element UI

### 4.1 项目创建

```shell
#安装vue-router
npm install vue-router --save-dev
#安装element-ui
npm i element-ui -S
#安装依赖
npm install
#安装SASS加载器
cnpm install sass-loader node-sass --save-dev
#测试启动
npm run serve
```

### 4.2 动态路由

```js
const User = {
  template: '<div>User</div>'
}

const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})

const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
```

|           **模式**            |    **匹配路径**     |          **$route.params**           |
| :---------------------------: | :-----------------: | :----------------------------------: |
|        /user/:username        |     /user/evan      |         { username: 'evan' }         |
| /user/:username/post/:post_id | /user/evan/post/123 | { username: 'evan', post_id: '123' } |

### 4.3 重定向

```js
//重定向也是通过 routes 配置来完成，下面例子是从 /a 重定向到 /b：
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
//重定向的目标也可以是一个命名的路由
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})
```

