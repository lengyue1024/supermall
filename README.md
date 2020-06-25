子传父->自定义事件

``` html
<!-- 父组件模板 -->
<div id="app">
<!-- 父组件调用子组件，并且监听事件来接收子组件传递的数据 -->
<!-- 在子组件这里使用@(this.$emit自定义的事件)，后面接收来自父组件的事件 -->
    <cpn @itemClick="cpnClick"></cpn>
</div>

<!-- 子组件模板 -->
<template>
 <div>
    <button v-for="item in categories" @click="btnClick(item)">
    {{item.name}}
    </button>
</div>
</template>

<script>
//  子组件
    const cpn = {
        template: '#cpn',
        data(){
            return {
                categories:[
                    {id: '001',name:'热门推荐'},
                    {id: '002',name:'手机数码'},
                    {id: '003',name:'家用电器'}
                ]
            }
        },
        methods:{
            btnClick(item){
                // 发射出去事件，itemClick是自定义事件名字,item是传递的参数
                this.$emit('itemClick',item)
            }
        }
    }
// 父组件
const app = new Vue({
    el: '#app',
    data:{
        message:'你好啊'
    },
    components:{
        cpn
    },
    methods:{
        cpnClick(){
            console.log('cpnClick',item)
        }
    }
})
</script>
```



自定义事件流程：
1. 在子组件中，通过$emit()来触发事件
2. 在父组件中，通过v-on来监听子组件事件



父组件访问子组件：
父组件拿到子组件的对象，然后操作这些对象(比如数据或者方法等)
父组件直接访问子组件，使用$children<非重点，不常用>或$refs<重点，必掌握>
子组件直接访问父组件，使用$parent

this.$children是一个数组类型，包含所有子组件对象。
所以在获取其中某一个数据时候，需要使用数组的访问方式，下标来完成。

使用 `$children` 并不好用，因为使用下标会导致程序维护困难。
this.$children[2] // 获取第三个子组件的对象.
但是如果出现其他子组件时候，这个下标就会发生改变。


所以我们需要使用$refs ---- 对象类型，默认是一个空的对象。
ref：reference(引用)
需要在子组件上添加属性ref.

``` html
<!-- 这里的res就相当于给子组件起一个名字，唯一标识一下子组件-->
<children-component ref="tip"></children-component>
<children-component ref="news"></children-component>
<!-- 获取子组件对象 -->
console.log(this.$refs) // 获取到两个children-component的子组件对象内容
console.log(this.$refs.tip) // 获取到标识为tip的children-component的子组件对象内容
console.log(this.$refs.news) // 获取到标识为news的children-component的子组件对象内容
```


子组件访问父组件$parent[特别不常用]



## 插槽的使用

slot(插槽)

为什么使用插槽？
1. 在生活中很多地方都有插槽，电脑的USB插槽，插板当中的电源插槽
2. 插槽的目的是让原来我们的设备具有更多的扩展性
3. 比如电脑的USB我们可以插入U盘，硬盘，手机，音响，键盘，鼠标等

组件的插槽：
1. 组件的插槽也是为了让我们封装的组件更加具有扩展性
2. 让使用者可以决定组件内部的一些**内容**到底展示什么

插槽就是预留一个控件，就好比是占位符一样。

应用例子：移动网站中的导航栏
移动开发中，几乎每个页面都有导航栏
导航栏我们必然封装成一个插件，比如nav-bar组件
一旦有了这个组件，我们就可以在多个页面中复用了
但是，每个页面中的导航都是一样的吗？并不是，每个页面的导航拉栏都有自己的样式和不同的文字


那么，如何封装这类组件呢？
它们有很多区别，但是也有很多共性。
如果，我们每一个都单独区封装一个组件，显然并不合适；比如每个页面都返回这部分内容，我们就要重复区封装。
但是，如果我们封装成一个，也不合理；因为有些左侧是菜单，有些是返回，有些中间是搜索框，有些是文字等等。


如何封装呢？**抽取共性，保留不同**
- 最好的封装方式，就是将共性抽取到组件中，将不同暴漏为插槽
- 一旦我们预留了插槽，就可以让使用者根据自己的需求，决定插槽中插入什么内容
- 是搜索框，还是文字，还是菜单，由调用者自己来决定


```  html


```

如何使用插槽，我们只需要在插槽的位置即<slot>所在的位置。添加所需的内容即可，可以是标签或是文字也可以是标签+文字，比如<button>按钮</button>

我们在组件中也可以给插槽设置默认值。只需要在<slot>这里插入默认值</slot>中间插入默认值

``` html
<template>
 <div>
 <h2>组件</h2>
 <slot><button>按钮</button></slot>
 </div>
</template>
```
这里就给插槽，设置了一个按钮的默认值。

如果有多个值，同时放到组件进行替换时，会一起作为替换元素进行插槽替换。

如何使用slot
- 在子组件中，使用特殊的元素<slot>就可以为子组件开启一个插槽
- 该插槽插入什么内容取决于父组件如何使用

## 具名插槽的使用

比如一个导航栏，分为如下样式：
[左边][中间][右边]
这样有三个区域需要单独设置值，显然一个slot是不行的。

所以我们有几个区域，我们就给几个插槽。


``` html
<!-- 子组件 -->
<template id="cpn"> 
    <div>
        <solt name="left"><span>左边</span></slot>
        <solt name="center"><span>中间</span></slot>
        <solt name="right"><span>右边</span></slot>
    </div>
</template>
```

我们需要给每个插槽取一个名字用以区分这是哪个插槽。

``` html
<!-- 父组件 -->
<div id="app">
    <cpn><span slot="center">标题</span></cpn>
</div>
```
在父组件上写上slot属性就可以替换到对应name的slot并进行指定插槽替换了。



## 作用域插槽
<难点，重点>
编译作用域的理解

> 补充：ES6对象字面量增强写法

函数：
ES5写法
``` js
const obj = {
    run: function(){
        console.log(obj)
    },
    eat: function(){
        // 
    }
}
```
ES6增强写法
``` js
const obj = {
    run(){

    },
    eat(){

    }
}
```


箭头函数的使用和this指向

自定义函数的方式：
1. const obj = function(){

}

调用的时候：
obj() //这就是函数的调用

2. 对象字面量中定义函数
const obj = {
    bbb: function(){
        <!--ES5写法  -->
    },
    ccc(){
        <!-- ES6写法 -->
    }
}


3. 箭头函数
const ddd = (参数列表)=>{

}


 讲解：
 1. 参数问题
 1.1 放入两个或多个参数
 const sum = (num1,num2)=>{
     return num1+num2
 }

 1.2 放入一个参数
 const power = num =>{
     return num*num
     <!-- 可省略() -->
 }

 2. 返回值问题



 什么时候使用箭头函数？
 当我们把函数作为一个参数传入另外一个函数里面时候。
eg
``` js
setTimeout(function(){
    console.log(this)
},1000)

or

setTimeout(()=>{
    console.log(this)
},1000)
```
以上的this都是window


箭头函数中的this<stackoverflow>

箭头函数中的this引用就是最近作用域中的this。

箭头函数中的this是如何查找的？
**向外层作用域中，一层层查找this，直到有this的定义。**
``` js
const obj = {
    aaa(){
        setTimeout(function(){
    console.log(this) // window
},1000)
    }
    bbb(){
       setTimeout(()=>{
       console.log(this)
    },1000) // object对象
},1000)
}
}
```
function()中的this都是指向window。







## 路由
<**重点，难点**>

路由在网络工程里，就是通过互联的网络把信息从源地址传输到目的地址的活动
生活中的路由器，
路由器提供了两种机制：路由和转送(转发，因为有路由表)
路由是决定数据包从来源到目的地的路径
转送将输入端的数据转移到合适的输出端

路由中有一个非常重要的概念叫做路由表
 路由表本质上是一个映射表，决定了包的指向。


路由就是一种映射关系。

后端渲染：

html+css+java ： java代码作用，是从数据库中读取数据，并且将它动态的放在页面中。从后端进行渲染，然后从服务器出来的就是直接渲染好的页面。



1. 第一种情况（早期开发）
后端路由：后端处理url和页面之间的映射关系

早期的网站开发整个html页面时由服务器来渲染的。
- 服务器直接生产渲染好对应的html页面，返回给客户端进行展示。

但是，一个网站，那么多页面服务器如何处理呢 ？
- 一个页面有自己对应的网址，也就是url
- url会发送到服务器，服务器会通过正则对该url进行匹配，并且最后交给一个Controller进行处理。
- Controller进行各种处理，最终生成html或者数据，返回给前端。
- 这就完成了一个IO操作。

上面的这种操作，就是后端路由
- 当我们页面中需要请求不同的路径内容时，交给服务器来进行处理，服务器渲染好整个页面，并且将页面返回给客户端
- 这种情况下渲染好的页面，不需要单独加载任何js和css，可以直接交给浏览器展示，这样有利于seo的优化。


后端路由的缺点：
- 一种情况是整个页面的模块由后端人员来编写和维护
- 另一种情况是前端开发人员如果要开发页面，需要通过php，Java，.Net等语言来编写页面代码
- 而且通常情况下html代码和数据以及对应的逻辑会混在一起，编写和维护都是非常麻烦的事情

2. 第二种情况（中期开发）
前后端分离阶段：
- 随着Ajax的出现，有了前后端分离的开发模式
- 后端只提供API来返回数据，前端通过Ajax获取数据，并且可以通过js将数据渲染到页面中
- 这样做最大的优点是前后端责任的清晰，后端专注于数据上，前端专注于交互和可视化上
- 并且当移动端（IOS/Android）出现后，后端不需要进行任何处理，依然使用之前的一套API即可
- 目前很多网站依然采用这种模式开发

后端只负责提供数据，不负责任何阶段的内容

静态资源服务器（放置html,css,js，图片资源等）
提供API接口的服务器
API服务器连接数据库，从数据库中进行数据请求和返回数据

所以这种请求过程：
输入url，从静态资源服务器返回html+css+js.
然后js运行在浏览器中，执行后遇到$.ajax(url:api接口)这种Ajax请求开始执行，开始请求API接口，然后就连接提供API接口的服务器进行请求，然后提供API的服务器返回数据，然后将返回的数据渲染到html网页中。

前端渲染：浏览器中显示的网页中的大部分内容，都是由前端写的js代码在浏览器中执行，最终渲染出的网页。
【每一个url都是一个路由】

3. 第三种情况（现在开发）
单页面富应用阶段：
- 其实SPA最主要的特点就是在前后端分离的基础上加了一层前端路由
- 也就是前端来维护一套路由规则


SPA：整个网页只有一个html页面。

一个url对应一套组件

前端路由的核心：
- 改变url，但是页面不进行整体的刷新

### vue-router
适合用于构建单页面应用。

安装和使用vue-router
1. 安装
npm install vue-router --save
2. 在模块化工程中使用它（因为是一个插件，所以可以通过Vue.use()来安装路由功能）
- 导入路由对象，并且调用Vue.use(VueRouter)
- 创建路由实例，并且传入路由映射配置
- 在Vue实例中挂载创建的路由实例


开始创建：
在src目录下创建router文件夹，用于放置路由的配置文件。
创建index.js文件。
``` js
// index.js
// 配置路由相关的信息
import VueRouter from 'vue-router'
import Vue from 'vue'

// 1.通过Vue.use(插件),安装插件
Vue.use(VueRouter)

// 配置路由和组件的应用关系
const routes = []
// 2. 创建VueRouter对象
// {}里传入optios选项
const router = new VueRouter({
   // ES6写法，ES5写法，routes：routes（第一个是VueRouetr实例的options属性，第二个是上面的数组routes
    routes
})

// 3. 将router对象传入到Vue实例中(导出)
export default router
```
4. 在main.js中进行导入，然后使用

``` js
// main.js
import Vue from 'vue'
import App from './APP'
import router from './router'

new Vue({
    el: '#app',
    router: router,
    render: h=>h(App)
})
```

使用vue-router步骤:
- 创建路由组件
- 配置路由映射：组件和路径映射关系
- 使用路由：通过<router-link>和<router-view>


#### 配置路由映射
``` js
import Home from '../components/Home'
import About from '../components/About'

const routes = [
    {
        // path选项的意思不是组件的路径，而是url的路径，当出现这个路径的适合，我们显示这个组件
        path: '/home',
        component: Home
    },
    {
        path: '/about',
        component: About
    }
]
```
使用路由：
在APP.vue中使用
``` js
<template>
    <div id="app">
        <router-link to="/home">首页</router-link>
        <router-link to="/about">关于</router-link>
        <router-view></router-view>
    </div>
</template>
```

<router-view>其实和html标签一样，也是起到一个占位的目的，在哪里放置<router-view>标签，就意味着你需要在哪里展示路由的页面内容。
显示路由内容信息。


<router-link>：该标签是一个vue-router已经内置的组件，它会被渲染成一个<a>标签
<router-view>：该标签会根据当前的路径，动态选染出不同的组件（其实就是给组件内容站一个位）

网页的其他内容，比如顶部的标题/导肮，或者底部的一些版权信息等会和<router-view>处于同一个等级。
在路由切换时，切换的是<router-view>挂载的组件，其他内容不会发生改变。



#### 路由的默认路径
我们这里还有一个不太好的实现：
- 默认情况下，进入网站的首页，我们希望<router-view>渲染首页的内容
- 但是我们的实现中，默认没有显示首页组件，必须让用户点击才可以

如何可以让路径默认跳到首页，并且<router-view>渲染首页组件呢？
非常简单，我们只需要配置多配置一个映射就可以了。

``` js
const routes = [
    // 添加路由映射
    {
        path: '/',
        redirect: '/home'
    }
]
```
配置解析：
- 我们在routes中又配置了一个映射
- path配置的是根路径： /
- redirect是重定向，也就是我们将根路径重定向到/home路径下，这样就可以得到我们想要的结果了


改变路径的方式有两种：
1. url的hash
2. HTML5的history
3. 默认情况下，路径的改变使用的url的hash

如果希望使用html5的history模式，非常简单，进行如下配置即可。
``` js
const routes = new VueRouter({
    routes,
    mode: 'history'
})
```

router-link的其他属性:
- to:用于指定跳转的路径
- tag：指定<router-link>之后渲染成什么组件，比如上面的代码会被渲染成一个<li>元素，而不是<a>。
`<router-link tag="link">`
- 





## Promise
### 基本介绍和使用
ES6中一个非常重要的概念和好用的特性就是Promise。

将以往的树形结构代码编写，做成了线性链式结构代码编写。


Promise是用来做什么的？
Promise是异步编程的一种解决方案。


什么时候我们会来处理异步事件呢？
- 一种很常见的场景应该是网络请求
- 我们封装一个网络请求的函数，因为不能立即拿到结果，所以不能像简单的3+4=7一样将结果返回
- 所以往往我们会传入另外一个函数，在数据请求成功时，将数据通过传入的函数回调出去。
- 如果只是一个简单的网络请求，那么这种方案不会给我们带来很大的麻烦。

但是，当网络请求非常复杂时，就会出现回调地狱（多重回调）。

网络请求的回调地狱



Promise基本语法
 参数->函数（resolve，reject)
 resolve，解决,reject，拒绝 这是两个参数,而且它们本身又是函数
new Promise((resolve,reject)=>{

})

其中()=>{}为箭头函数，因为promise需要传入一个函数作为参数。所以正好可以使用箭头函数。
基本使用示例
``` js
setTime(()=>{

})
```

什么情况下会使用promise？
一般情况下是有异步操作的，使用promise对这个异步操作进行封装。

new -> 构造函数(1. 保存了一些信息 2. 执行传入的函数)
在执行传入的回调函数时候，会传入两个参数，resolve，reject 。本身又是函数
``` js
new Promise((resolve,reject)=>{
    setTimeout(()=>{
        // 成功的时候调用resolve
        // resolve('hello world)
        // 失败的时候调用reject
        reject('error message')
    },1000)
}).then((data)=>{
    console.log(data)
}).catch((error)=>{
    console.log(error)
}
)
```

Promise三种状态
首先，当我们开发中有异步操作时，就可以给异步操作包装一个Promise
异步操作之后有三种状态
- pending：等待状态，比如正在进行网络请求，或者定时器没有到时间
- fulfill：满足状态，当我们主动回调了resolve时，就处于该状态，并且回调了.then()
- reject：拒绝状态，当我们主动回调了reject时，就处于该状态，并且回调.catch()


sync-同步，
async-异步




## Vuex
<重点，难点>
Vuex是一个转为vuejs应用程序开发的状态管理模式
特点：
它采用 集中式存储管理 应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化
Vuex也集成到vue的官方调试工具devtools。

>国内大部分技术网站都是用各种高大上术语来解释术语。劝退了好多人 。也给很多刚入行的同学读完一遍后一脸懵逼，导致出现畏难情绪。文档描述及其不友好。


状态管理是什么？
- 状态管理模式，集中式存储管理 这些名词看起来就非常高大上，让人捉摸不透。
- 其实，可以简单的将其看作把需要多个组件共享的变量全部存储在一个对象里面。
- 然后，将这个对象放在顶层的vue实例中，让其组件可以使用。
- 那么，多个组件是不是就可以共享这个对象中的所有变量属性呢？



## Vue的options选项

创建vue实例传入的options
我们在创建vue实例的时候，传入了一个对象options
这个options可以包含哪些选项呢？

- el
类型：string | HTMLElement
作用：决定之后vue实例会管理哪一个DOM

- data
类型： Object(Vue实例是对象) | Function（组件当中data必须是一个函数）
作用：Vue实例对应的数据对象

- methods
类型：{[key:string]:Function}
作用：定义属于Vue的一些方法，可以在其他地方调用，也可以在指令中使用


### 生命周期

生命周期：


## template
模板语法


三种方式添加css

- 外部样式表
> css保存在.css文件中
>在html里使用<link>标签引用

- 内部样式表
> 不使用外部css文件
> 将css放在html<style>里

- 内联样式
> 仅影响一个元素
> 在html元素的style属性中添加





































科技改变人生，技术改变生活。


校招薪水
南大就业
南大好实习
微软招聘


参加华为 腾讯挑战赛ACM赛

