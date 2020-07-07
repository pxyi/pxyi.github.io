---
layout: article
title: Vue-Router核心实现原理
date: 2019-05-13 00:00:00 Z
categories: mvvm
---

随着前端应用的业务功能越来越复杂、用户对于使用体验的要求越来越高，单页应用（SPA）成为前端应用的主流形式。大型单页应用最显著特点之一就是采用前端路由系统，通过改变URL，在不重新请求页面的情况下，更新页面视图。

“更新视图但不重新请求页面”是前端路由原理的核心之一

---

## 在vue中使用vue-router

{% highlight javascript %}

// router.js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../views/Home.vue'

Vue.use(VueRouter)

  const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router

{% endhighlight %}

{% highlight javascript %}
// main.js
import Vue from 'vue'
import App from './App.vue'
import router from './router'

Vue.config.productionTip = false

new Vue({
  router,
  render: h => h(App)
}).$mount('#app')

{% endhighlight %}
{% highlight html %}
// app.vue
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view></router-view>
  </div>
</template>
{% endhighlight %}

---

## 以上的几个步骤，vue-router内部都做了什么

首先vue-router 定义了两个全局组件，即 router-link和router-view，他们分别用来跳转路由和展示路由对应的组件。

我们点击router-link，路由发生改变, vue-router监听路由变化，再router-view中渲染对象组件。

结果是，url地址发生改变，但页面并没有刷新，从而实现单页面应用（SPA）。

---

## vue-router核心实现原理：
> 
1. 实现一个静态install方法，因为作为插件都必须有这个方法，给Vue.use()去调用；
2. 可以监听路由变化；
3. 解析配置的路由，即解析router的配置项routes，能根据路由匹配到对应组件；
4. 实现两个全局组件router-link和router-view；（最终落地点）

---

### 核心代码实现简版：

{% highlight javascript %}
let Vue;
class VueRouter {
    constructor(options){
        this.$options=options;
        this.$routerMap={};//{"/":{component:...}}
        // url 响应式，当值变化时引用的地方都会刷新
        this.app = new Vue({
            data:{
                current:"/"
            }
        });
    }
    // 初始化
    init(){
        // 监听事件
        this.bindEvent();
        // 解析路由
        this.createRouteMap();
        // 声明组件
        this.initComponent();
    }
    bindEvent(){
        window.addEventListener('hashchange',this.onHashchange.bind(this));
    }
    onHashchange(){
        this.app.current = window.location.hash.slice(1) || "/";
    }
    createRouteMap(){
        this.$options.routes.forEach(route=>{
            this.$routerMap[route.path]=route;
        })
    }
    initComponent(){
        Vue.component('router-link',{
            props:{
                to:String,
            },
            render(h){
                return h('a',{attrs:{href:'#'+this.to}},[this.$slots.default])
            }
        });
        Vue.component('router-view',{
            render:(h)=>{
                const Component = this.$routerMap[this.app.current].component;
                return h(Component)
            }
        });
    }
}
// 参数是vue构造函数，Vue.use(router)时,执行router的install方法并把Vue作为参数传入
VueRouter.install = function(_vue){
    Vue = _vue;
    //全局混入
    Vue.mixin({
        beforeCreate(){//拿到router的示例，挂载到vue的原型上
            if (this.$options.router) {
                Vue.prototype.$router=this.$options.router;
                this.$options.router.init();
            }
        }
    })
}
export default VueRouter;
{% endhighlight %}
