---
layout:     post
title:     vue初见
subtitle:   (todolist,组件,生命周期,数据请求)
date:       2019-08-14
author:     xieerwa
header-img: img/post-bg-vue.jpg
catalog: true
tags:
    - 前端  
    - vue
---
### todolist

html部分

```html
<input type="text" v-model='todo'>
    <button @click="doadd()">增加</button>
    <br>
    <h2>未完成</h2>
    <li v-for="(item,key) in list" v-if="!item.checked">
      <input type="checkbox" v-model='item.checked' @change='saveList()'>{{item.title}} ----- <button @click="remove(key)">删除</button>
    </li>
    <br>
    <h2>完成</h2>
    <li v-for="(item,key) in list" v-if="item.checked">
      <input type="checkbox" v-model='item.checked'  @change='saveList()'>{{item.title}} ----- <button @click="remove(key)">删除</button>
    </li>
```

方法部分

```vue
<script>
  import storage from './model/storage.js'; //打包好的storage
  
  export default {
    data() {
      return {
        todo: '你好',
        list: [],
        flag: true
      }
    },
    methods: {
      doadd() {
        this.list.push({
          title: this.todo,
          checked: false
        });
        storage.set('list', this.list);
        this.todo = '';
      },
      remove(key) {
        this.list.splice(key, 1);
        //H5localstorage缓存数据
        storage.set('list', this.list);
      },
      saveList() {
        storage.set('list', this.list);
      },whatkey(e) {
        console.log(e.keyCode);
        if (e.keyCode == 13) {
          this.list.push({
            title: this.todo,
            checked: false
          });
          this.todo = '';
        }
      }
    },
    mounted() {
      /* 生命周期函数  一刷新就触发*/
      var list =  storage.get('list');
      if (list) {
        this.list = list;
      }
    }
  }

</script>
```

storage

```js
//  封装localstorage本地储存
var storage={
    set(key,value){
        localStorage.setItem(key, JSON.stringify(value));
    },get(key){
        return JSON.parse(localStorage.getItem(key));
    },removed(key){
        localStorage.removeItem(key);
    }
}
export default storage;
```

### 组件

[参考官方](https://cn.vuejs.org/v2/guide/components.html "vue组件")

#### 父组件给子组件传值

1. 父组件在调用子组件时候绑定动态属性

   ```vue
   <v-header :title="title"></v-header>
   ```
2.在子组件中用 props接受父组件传过来的数据
### 生命周期函数

![vue生命周期](https://cn.vuejs.org/images/lifecycle.png)

```vue
<div id="container">
  <button @click="changeMsg()">change</button>
  <span>{{msg}}</span>
 </div>

<script type="text/javascript">
 var vm = new Vue({
  el:'#container',
  data:{
    msg:'TigerChain'
  },
  beforeCreate(){
    console.group("%c%s","color:red",'beforeCreate--实例创建前状态')
    console.log("%c%s","color:blue","el  :"+this.$el)
    console.log("%c%s","color:blue","data  :"+this.$data)
    console.log("%c%s","color:blue","message  :"+this.msg)
  },
  created() {
    console.group("%c%s","color:red",'created--实例创建完成状态')
    console.log("%c%s","color:blue","el  :"+this.$el)
    console.log("%c%s","color:blue","data  :"+this.$data)
    console.log("%c%s","color:blue","message  :"+this.msg)
  },
  beforeMount() {
    console.group("%c%s","color:red",'beforeMount--挂载之前的状态')
    console.log("%c%s","color:blue","el  :"+this.$el)
    console.log(this.$el);
    console.log("%c%s","color:blue","data  :"+this.$data)
    console.log("%c%s","color:blue","message  :"+this.msg)
  },
  mounted() {
    console.group("%c%s","color:red",'mounted--已经挂载的状态')
    console.log("%c%s","color:blue","el  :"+this.$el)
    console.log(this.$el);
    console.log("%c%s","color:blue","data  :"+this.$data)
    console.log("%c%s","color:blue","message  :"+this.msg)
  },
  beforeUpdate(){
    console.group("%c%s","color:red",'beforeUpdate--数据更新前的状态')
    console.log("%c%s","color:blue","el  :"+this.$el.innerHTML)
    console.log(this.$el);
    console.log("%c%s","color:blue","data  :"+this.$data)
    console.log("%c%s","color:blue","message  :"+this.msg)
    console.log("%c%s","color:green","真实的 DOM 结构:"+document.getElementById('container').innerHTML)
  },
  updated() {
    console.group("%c%s","color:red",'updated--数据更新完成时状态')
    console.log("%c%s","color:blue","el  :"+this.$el.innerHTML)
    console.log(this.$el);
    console.log("%c%s","color:blue","data  :"+this.$data)
    console.log("%c%s","color:blue","message  :"+this.msg)
    console.log("%c%s","color:green","真实的 DOM 结构:"+document.getElementById('container').innerHTML)
  },
  activated() {
    console.group("%c%s","color:red",'activated-- keep-alive 组件激活时调用')
    console.log("%c%s","color:blue","el  :"+this.$el)
    console.log(this.$el);
    console.log("%c%s","color:blue","data  :"+this.$data)
    console.log("%c%s","color:blue","message  :"+this.msg)
  },
  deactivated(){
    console.group("%c%s","color:red",'deactivated-- keep-alive 停用时调用')
    console.log("%c%s","color:blue","el  :"+this.$el)
    console.log(this.$el);
    console.log("%c%s","color:blue","data  :"+this.$data)
    console.log("%c%s","color:blue","message  :"+this.msg)
  },
  beforeDestroy() {
    console.group("%c%s","color:red",'beforeDestroy-- vue实例销毁前的状态')
    console.log("%c%s","color:blue","el  :"+this.$el)
    console.log(this.$el);
    console.log("%c%s","color:blue","data  :"+this.$data)
    console.log("%c%s","color:blue","message  :"+this.msg)
  },
  destroyed() {
    console.group("%c%s","color:red",'destroyed-- vue实例销毁完成时调用')
    console.log("%c%s","color:blue","el  :"+this.$el)
    console.log(this.$el);
    console.log("%c%s","color:blue","data  :"+this.$data)
    console.log("%c%s","color:blue","message  :"+this.msg)
  },
  methods: {
    changeMsg() {
      this.msg = 'TigerChain111'
    }
  }
})
</script>
```

### vue数据请求

#### vue-resource 是官方提供的一个vue的插件

首先需要在项目中安装vue-resource

```shell
cnpm install vue-resource --save
```

然后会把vue-resource写入到package.json里面去

```json
 "dependencies": {
    "vue": "^2.5.11",
    "vue-resource": "^1.5.1"
  }
```

在main.js里面引用和使用我们的vue-resource

```js
import VueResource from 'vue-resource' //引入
Vue.use(VueResource); //使用
```

在组件中使用

```js
methods:{
      getDate(){
           //请求数据
          var api="http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1";
          this.$http.get(api).then(function(response){
              console.log(response);
          },function(err){
            console.log(err);
          })
      }
```

#### axios数据请求插件

axios的使用

首先安装

```shell
npm install axios --save
或者
cnpm install axios --save
```

引用axios,和vue-resource的不同为不用单独在写一个使用

```js
import Axios from 'Axios' //引入
```

在组件中使用

```js
methods:{
      getDate(){
           //请求数据
          var api="http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1";
          axios.get(api).then(function(response){
              console.log(response);
          },function(err){
            console.log(err);
          })
      }
```

#### fetch-jsonp数据请求插件(支持jsonp)

[fetch-jsonp github说明](https://github.com/camsong/fetch-jsonp "fetch-jsonp数据请求插件")

 