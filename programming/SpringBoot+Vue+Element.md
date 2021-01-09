# SpringBoot+Vue+Element

## 1.0 创建Vue项目

### 1.1 创建方式

- 脚手架(推荐)

  ```shell
  #启动图形界面
  vue ui
  ```

- ​     命令

  ```shell
  vue create
  ```

- idea

  ```
  File -> New Project -> JavaScript -> Vue.js
  ```

  

### 1.2 JS 箭头函数

- 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
- 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
- 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
- 不可以使用yield命令，因此箭头函数不能用作 Generator 函数

```js
//this === obj ,函数体为返回
obj = {
    data: ['John Backus', 'John Hopcroft'],
    init: function() {
        document.onclick = ev => {
            alert(this.data) // ['John Backus', 'John Hopcroft']
        }
        // 非箭头函数
        // document.onclick = function(ev) {
        //     alert(this.data) // undefined
        // }
    }
}
```

### 1.3 axois

1. **使用**

   ```js
   this.rq.get('/api/product/get?productChannelId='+this.productChannelId).then(res=>{
       console.log(res)
   })
   
   // 传对象参数
   // get请求记得加params
   this.rq.get('/api/product/get,{params:{name:'abc'}}).then(res=>{
       console.log(res)
   })
   
   this.rq.post('/api/product/get,{name:'abc'}).then(res=>{
       console.log(res)
   })
   ```

   

2. **跨域问题**

   ```js
   //开发环境如果要跨域，解决跨域问题（设置代理）：
   //vue-cli 3.0的在 package.json  同级目录新建一个 vue.config.js 文件，加入下面代码，其他版本找到配置文件的devServer加入代码
   module.exports = {
       //axios域代理，解决axios跨域问题
       baseUrl: '/',
       devServer: {
           proxy: {
               '': {
                   target: 'http://192.168.0.108:8090',
                   changeOrigin: true,
                   ws: true,
                   pathRewrite: {
   
                   }
               }
           }
       }
   }
   
   ```

   

### 1.4 created()与mounted

![](C:\Users\Administrator\Desktop\note\SpringBoot+Vue+Element\20170919221428421.png)

**created**:在模板渲染成html前调用，即通常初始化某些属性值，然后再渲染成视图。
**mounted**:在模板渲染成html后调用，通常是初始化页面完成后，再对html的dom节点进行一些需要的操作。
其实两者比较好理解，通常created使用的次数多，而mounted通常是在一些插件的使用或者组件的使用中进行操作，比如插件chart.js的使用: var ctx = document.getElementById(ID);通常会有这一步，而如果你写入组件中，你会发现在created中无法对chart进行一些初始化配置，一定要等这个html渲染完后才可以进行，那么mounted就是不二之选

### 1.5 Element UI

- **el-container 构建整个页面框架**
- **el-aside 侧边栏菜单**
- **el-menu 侧边栏菜单内容**
  - :default-openeds="['1', '3']" 默认选中 `index字符类型`
  - class="el-icon-menu" 图标
  - ：default-active 默认选中

### 1.6 menu与router绑定

1. <el-menu>标签添加router属性
2. 在页面添加<router-view>
3. <el-menu-item>标签的index值==router中的path
4. `:class="$route.path==item2.path ? 'isactive' : ''`  **$route当前路由**

1.7 真实demo

```vue
  <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
      <!--router路由跳转 :index 绑定path-->
      <el-menu router :default-openeds="['0']">
          <el-submenu v-for="(submenu,index) in $router.options.routes"
                      :index="index+''">
              <template slot="title"><i class="el-icon-message"> {{submenu.name}}
                  </i></template>
              <el-menu-item-group>
                  <el-menu-item :class="$route.path === item.path ? 'is-active' : ''" 
                                v-for="item in submenu.children" :index="item.path">
                      {{item.name}}
                  </el-menu-item>
              </el-menu-item-group>
          </el-submenu>
      </el-menu>
</el-aside>

```



## 2.0 问题

### 2.1 axios方法调用

```js
this.rq.get('/api/product/get,{params:{name:'abc'}}).then(res=>{
    console.log(res)
})
```

1. **参数一：url或uri 参数二：回调函数将res中的data赋值给其他变量返回**

2. ```js
   (res=>{console.log(res)})
   //等价？
   (res=>(console.log(res))
   ```

### 2.2 Failed to mount component

1. 组件没有正确引入
2. 挂载点顺序错了
3. 路由组件名字 component: 组件名 (is not components)

### 2.3 route节点不被被遍历

添加在router中`show`属性