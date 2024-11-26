

> ​ Vue 中的路由用于实现单页应用（SPA）中的页面导航。它允许你在不刷新整个页面的情况下，根据不同的 URL 路径显示不同的组件，提供了类似于多页面应用的用户体验。例如，在一个电商应用中，可以通过路由实现从首页到商品详情页、购物车页和用户个人中心页等不同页面的切换
> 
> 
> ​ Vue Router（Vue 官方的路由库）通过监听浏览器的 URL 变化，根据预先定义的路由规则，动态地加载和显示相应的组件。它利用了浏览器的`History` API 或者`Hash`模式来实现 URL 的管理，前端的路由key是路径，value是组件或者一个function，访问指定路径显示对应内容



> SPA 应用的理解:
> 
> 
> ​ SPA（Single \- Page Application）即单页应用，是一种现代的网页应用架构模式。在 SPA 中，整个应用只有一个 HTML 页面，当用户与应用进行交互（如点击链接、提交表单等）时，不会像传统的多页面应用那样进行整页刷新，而是通过 JavaScript 动态地更新页面的部分内容来呈现不同的视图或功能。例如，像一些大型的 Web 应用，如谷歌文档（Google Docs）和 GitHub，它们的用户体验很流畅，在使用过程中基本感觉不到页面的刷新，这就是典型的 SPA 应用


#### 路由的基本使用


1. 安装vue\-router


vue2使用vue\-router3版本，vue3使用vue\-router4版本



```
npm i vue-router@3  

```
2. 使用vue\-router



```
import  VueRouter from "vue-router"
// vue-router是一个插件库,使用了VueRouter之后，vue可以配置router选项
Vue.use(VueRouter)

```
3. 创建router


新建一个router文件夹/index.js



```
import VueRouter from "vue-router"

// 自定义的组件
import Apage from "@/components/Bpage.vue";
import Bpage from "@/components/Apage.vue";


// 创建并暴露一个路由
export default new VueRouter({
    routes: [
        // 如果请求路径是/a，触发a组件
        {
            path: "/a",
            component: Apage
        },
        // 如果请求路径是/b，触发b组件
        {
            path: "/b",
            component: Bpage
        }
    ]

})



```
4. 配置router



```
import router from "./router"
new Vue({
  render: h => h(App),
  // 配置引入的router
  router:router
}).$mount('#app')

```
5. 路由状态


路由配置成功，浏览器地址栏的url后会带一个\#



```
http://localhost:8080/#/

```
6. 实现路由跳转、显示



```
<template>
  <div id="app">

    

    <router-link to="/a"  active-class="active">Apagerouter-link>
    <router-link to="/b"  active-class="active">Bpagerouter-link>

    
    <router-view>router-view>
  div>
template>


<style>
/* 路由被激活时用的样式 */
.active{
  font-size: 20px;
  color: red;
}
style>

```
7. 注意事项


	* 路由组件(通过路由匹配切换)通常存放在`pages`文件夹
	* 一般组件(使用组件标签应用)通常存放在`components`文件夹。
	* 通过切换，隐藏了的路由组件，默认是被销毁掉的，需要的时候再去挂载。
	* 每个组件都有自己的`$route`属性，里面存储着自己的路由信息。
	* 整个应用只有一个router，可以通过组件的`$router`属性获取到


#### 嵌套路由



```
// 创建并暴露一个路由
export default new VueRouter({
    routes: [
        // 如果请求路径是/a，触发a组件
        {
            path: "/a",
            component: Apage,
            // 通过children 配置多级路由，可以配置多个路由
            children: [
                {
                    path: "c",  // 路径不需要写/
                    component: Cpage
                }


            ]
        },
        // 如果请求路径是/b，触发b组件
        {
            path: "/b",
            component: Bpage
        }
    ]

})


```


```
<!--  父路由 -- >
<template>
  <div>
    <p>a pagep>
    
    <router-link to="/a/c">to c pagerouter-link>
    
    <router-view>router-view>
  div>
template>

```

#### 路由query参数


路由跳转的时候在url上携带参数


1. 传递参数\-字符串写法



```
    
    
    <router-link :to="`/a/c?id=${id}&size=${num}`">to c pagerouter-link>

```
2. 传递参数\-对象写法



```
    <router-link :to="{
      // 路径
      path:'/a/c',
      // 使用query选项指定参数
      query:{
        id:id,
        size:num
      }
    }">to c pagerouter-link>

```
3. 接收参数


在对应的路由页面里



```
{{$route.query.id}}
{{$route.query.size}}

```


#### 命名路由



> 命名路由是给路由定义一个名称。这就像给一个人起名字一样，方便在代码的其他地方通过这个名字来引用特定的路由，而不是使用冗长的路径字符串。它使得路由的跳转和操作更加灵活和可读


1. 给路由命名\-通过name属性



```
export default new VueRouter({
    routes: [
        // 如果请求路径是/a，触发a组件
        {
            path: "/a",
            component: Apage,
            // 通过children 配置多级路由，可以配置多个路由
            children: [
                {

                    path: "c",  // 子路由跳转路由，不需要写/
                    component: Cpage,
                    name:"c_page" // 给路由命名
                }
            ]
        }
    ]
})

```
2. 路由跳转



```
    
    <router-link :to="{name:'c_page'}">to c pagerouter-link>


```


#### 路由params参数



> `params`主要用于在路由路径中定义动态片段，这些动态片段是路由路径的一部分，用于区分不同的资源,`query`主要用于传递一些额外的、不影响资源定位的信息，比如搜索关键词、排序方式、分页信息等


1. 声明params参数



```
 children: [
                {

                    path: "c/:id",  // 使用占位符声明接收的params参数
                    component: Cpage,
                    name: "c_page", // 给路由命名
                    
                    
                }


            ]

```
2. 传递参数\-字符串写法



```
 <router-link :to="`/a/c/${id}`">to c pagerouter-link>

```
3. 传递参数\-对象写法


对象写法传递params参数 路由跳转必须用name别名，不能用path



```
  <router-link :to="{
    name:'c_page',
    params:{
      id:id
    }

  }">router-link>

```
4. 读取参数



```
{{$route.params.id}}

```


#### 路由props配置



> props是一个用于将路由参数以props的形式传递给组件的选项，让路由组件更方便的收到参数


1. 对象写法\-传递的是写死的数据



```
            // 通过children 配置多级路由，可以配置多个路由
            children: [
                {

                    path: "c/:id",  // 使用占位符声明接收的params参数
                    component: Cpage,
                    name: "c_page", // 给路由命名
                    // 对象写法,key和value会以props形式传递给Cpage组件，需要在组件内接收
                    props:{
                        id:1
                    }
                }
            ]

```


```
export default {
  name: "Cpage",
  props:["id"]
}

```
2. 布尔值开关写法



```
 // 通过children 配置多级路由，可以配置多个路由
            children: [
                {

                    path: "c/:id",  // 使用占位符声明接收的params参数
                    component: Cpage,
                    name: "c_page", // 给路由命名
                    // 开启了该选项，会把该路由组件收到的params参数以props的形式传给组件，需要在组件接收
                    props:true
                }
            ]

```


```
export default {
  name: "Cpage",
  props:["id"]
}

```
3. 函数写法\-同时支持动态params、query



```
 children: [
        {

            path: "c",  // 使用占位符声明接收的params参数
            component: Cpage,
            name: "c_page", // 给路由命名

            // 函数返回值中的每一组key-value都会通过props传递给组件
            props($route){
            // query参数、params参数

                return {id:$route.query.id,p:$route.params.p}
            }

        }
    ]

```


```
export default {
  name: "Cpage",
  props:["id","p"]
}

```


#### router\-link的replace



> 控制路由跳转时操作浏览器历史记录的模式
> 
> 
> 浏览器的历史记录有两种写入方式：分别为push和replace，默认为push



> `push`是指将一个新的路由记录添加到浏览器的历史记录栈中。这意味着当用户进行导航操作时，会在历史记录中新增一条记录，就好像在栈顶放入了一个新的元素。例如，在一个网页应用中，用户从首页点击链接进入产品详情页，使用`push`操作后，浏览器的历史记录栈中就会新增一条记录，表示用户访问了产品详情页，用户可以通过浏览器的 “后退” 按钮回到之前的首页



> 与`push`不同，`replace`操作是替换当前的路由记录，而不是添加新的记录。它就像是修改了历史记录栈顶的元素，而不是在栈顶添加新元素。还是以网页应用为例，如果用户在登录页面登录成功后，使用`replace`操作跳转到用户主页，那么在历史记录中，登录页面的记录会被用户主页的记录替换，用户在用户主页时，点击 “后退” 按钮不会回到登录页面



```
   ```html

to c page
   ```

```

#### 编程式路由导航



> 编程式路由导航是指在 JavaScript 代码中通过调用相关的方法来实现路由跳转，不借助实现路由跳转，让路由跳转更加灵活


1. push形式路由跳转



```
export default {
  name: "Apage",
  methods:{
    puscCpage(){
      //  以push的形式跳转到c_page路由
      this.$router.push({
        name:"c_page", // name是路由命名name的值
        query:{
          id:1
        }

      })
    }
  }
}

```


```
<template>
  <div>
    <p>a pagep>


    <button @click="puscCpage">button>

    <router-view>router-view>
  div>
template>

```

2. replace形式路由跳转



```



  
    a page




    

    
  


```
3. 路由前进、后退



```
// 触发前进
this.$router.forward()
// 触发后退
this.$router.back()
// 可以前进也可以后退，传正数 前进指定数的记录，传负数 后退指定数的记录
this.$router.go(3)

```


#### 缓存路由组件



> 让不展示的路由组件保持挂载，不被销毁，使用kepp\-alive



```



<keep-alive include="Cpage"> 
    <router-view>router-view>
keep-alive>

```

#### 路由的生命周期钩子



> 路由组件所独有的两个钩子，用于捕获路由组件的激活状态



```
export default {
  name: "Apage",
  // activated:路由组件被激活时触发
  activated(){
    console.log("activated")
  },
  //deactivated:路由组件失活时触发
  deactivated(){
    console.log("deactivated")
    
  }


}

```

#### 路由守卫



> 路由守卫是 Vue Router（以及其他路由框架也有类似概念）提供的一种机制，用于在路由跳转过程中进行拦截和处理。它就像是在路由的各个关键节点设置的 “关卡”，可以检查用户是否有权限访问某个页面、在进入或离开页面时执行一些特定的操作，如数据获取、页面过渡动画控制等


##### 路由元信息



> 路由元信息（`meta`）是在定义路由时可以添加的额外信息，它可以是任何你想要的数据类型，如对象、字符串、数字等。这些信息主要用于在路由守卫或者其他需要获取路由额外信息的场景中使用，是一种灵活的方式来为每个路由配置自定义的属性



```
const routes = [
  {
    path: '/login',
    component: HomeComponent,
    // 自定义的元信息
    meta: {
      title: '首页',
      requiresAuth: false
    }
  },
  {
    path: '/admin',
    component: AdminComponent,
    meta: {
      title: '管理页面',
      requiresAuth: true
    }
  }
];

```


```
// 读取元信息
this.$route.meta

```

##### 全局路由守卫



```
// 创建并暴露一个路由
const router = new VueRouter({
  routes:[]
})

export  default router

```

1. beforeEach前置守卫


在初始化、每次路由跳转之前被调用，可以用来进行全局的权限检查、加载显示等操作



```
router.beforeEach((to, from, next) => {
  // to是即将要进入的目标路由对象
  // from是当前正要离开的路由对象
  // next是一个函数，用于决定是否继续路由跳转
  // 检查用户是否登录，通过检查localStorage中的token来判断
  const token = localStorage.getItem('token');
  if (to.meta.requiresAuth &&!token) {
    // 如果目标路由需要认证（通过路由元信息meta中的requiresAuth字段判断）
    
    // 跳转登录页
    next('/login');
  } else {
    // 跳转要跳转的页面
    next();
  }
});


```
2. afterEach后置守卫


初始化、每次路由跳转完成后被调用，主要用于执行一些不需要中断路由跳转的操作，如页面标题更新、统计页面访问次数等



```
router.afterEach((to, from) => {
  // 更新页面标题，假设每个路由的meta信息中有title字段用于定义页面标题
  document.title = to.meta.title || '默认标题';
});

```


##### 路由独享守卫



> 可以在单个路由配置中定义，只对该路由生效。它和全局`beforeEach`守卫类似，但作用范围仅限于特定的路由



```
routes:[
  {
    path: '/admin',
    component: AdminComponent,
    beforeEnter: (to, from, next) => {
      // 检查用户是否是管理员，假设通过检查localStorage中的adminToken来判断
      const adminToken = localStorage.getItem('adminToken');
      if (adminToken) {
        next();
      } else {
        next('/login');
      }
    }
  }
]

```

##### 组件内路由守卫


1. beforeRouteEnter



> 在组件被渲染之前调用，此时组件实例还没有被创建，所以不能访问组件的this,但是可以通过传递给`next`函数的回调来访问组件实例



```
export default {
  name: 'Admin',
  // 通过路由规则，进入该组件时被调用
  beforeRouteEnter(to, from, next) {
    // 检查用户是否有权限访问该组件
    const authService = {
      // 权限检查函数，根据用户的角色或其他条件返回布尔值
      checkAccess: () => {
        const userRole = 'admin'
        // 根据角色判断是否有权限访问
        return userRole === 'admin'
      }
    }

    // 检查用户是否有权限访问该组件
    if (authService.checkAccess()) {
      // 如果有权限，则继续导航到目标路由
      next()
    } else {
      // 如果没有权限，则重定向到无权访问页面
      next('/unauthorized')
    }
  },
  data() {
    return {
      message: 'Welcome'
    }
  }
}

```

​
2. beforeRouteUpdate



> 在当前路由改变，但是该组件被复用时调用。这对于动态路由参数的组件很有用，可以在这里响应路由参数的变化并更新组件数据
> 
> 
> 例如，有一个用户详情组件，其路由路径为`/user/:id`，当从`/user/1`跳转到`/user/2`时，由于组件是复用的（都是`UserDetailComponent`），`beforeRouteUpdate`就会被调用



```
export default {

  beforeRouteUpdate(to, from, next) {
    //  方法updateData用于更新数据
    this.updateData(to.params.id);
    next();
  }
}

```
3. beforeRouteLeave



> 在离开当前组件对应的路由时调用，可以用于保存未完成的数据、询问用户是否确认离开等操作



```
export default {
  // 通过路由规则，离开该组件时被调用
  beforeRouteLeave(to, from, next) {
    // 检查是否有未保存的数据
    const unsavedData = this.checkUnsavedData();
    if (unsavedData) {
      // 如果有未保存的数据，弹出确认框询问用户是否确认离开
      const confirmLeave = window.confirm('你有未保存的数据，确定要离开吗？');
      if (confirmLeave) {
        next()
      } else {
        // false代表中断当前路由操作
        next(false)
      }
    } else {
      next()
    }
  },
}

```


#### history和hash模式


1. Hash 模式



> * vue路由默认是hash模式
> * 在 URL 中，\#符号及其后面的部分被称为哈希（hash）部分
> * Hash 模式的路由是基于 URL 的哈希值变化来实现的。当哈希值改变时，浏览器不会向服务器发送请求，而是会触发浏览器的hashchange事件，JavaScript 可以通过监听这个事件来实现页面的局部更新，从而达到切换页面（路由）的效果
> * 兼容性好：几乎所有的浏览器都支持hash模式，包括一些较老的浏览器。
> * 不会发送请求到服务器：这使得在单页应用（SPA）中可以实现无刷新的页面切换，因为服务器不会对哈希部分的变化做出响应，所有的路由切换逻辑都在前端完成。
> * URL 相对不美观：带有\#符号的 URL 可能看起来不够简洁、美观，并且在某些场景下可能会让人感觉比较 “奇怪”，例如在分享链接或者生成静态资源链接时。


2. History 模式



> * History 模式利用了 HTML5 新增的History API，特别是pushState和replaceState方法
> * 通过这些方法，JavaScript 可以改变浏览器的历史记录栈，并且在改变 URL 时不会引起页面的刷新（除非手动刷新），浏览器不会自动发送请求到服务器，而是由前端应用来处理这个路由变化。
> * URL 更美观：没有\#符号，使得 URL 看起来更像传统的多页应用的链接，更符合用户的认知习惯，在分享链接、搜索引擎优化（SEO）等方面有一定优势。
> * 需要服务器配置支持：因为改变后的 URL 和普通的 URL 没有区别，所以当用户直接访问或者刷新一个通过History API修改后的 URL 时，浏览器会向服务器发送请求。这就需要服务器进行配置，将所有的前端路由请求都重定向到应用的入口文件（例如index.html），否则会出现 404 错误。
> * 兼容性稍差：虽然现代浏览器都支持History API，但一些较老的浏览器可能不支持，在使用时需要考虑兼容性问题


3. 切换history模式


前端配置



```
import Vue from 'vue';
import Router from 'vue-router';
Vue.use(Router);
const router = new Router({
  // 指定History
  mode: 'History',
  routes: [

  ]
});
export default router;

```

服务端配置



> 当使用History模式时，因为浏览器会把修改后的 URL 当作普通的请求发送给服务器，所以需要服务器进行相应的配置,具体参考使用对应的Server的解决方案


 本博客参考[FlowerCloud机场节点订阅](https://dahelaoshi.com)。转载请注明出处！
