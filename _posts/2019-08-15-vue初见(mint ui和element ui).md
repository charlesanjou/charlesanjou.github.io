---
layout:     post
title:     vue初见
subtitle:   (mint ui和element ui)
date:       2019-08-16
author:     xieerwa
header-img: img/post-bg-vue.jpg
catalog: true
tags:
    - 前端  
    - vue
    - mint ui
    - element ui	
---

### mint ui(移动端)

#### mint ui 的安装和引入

##### 安装

```shell
npm i mint-ui -S
//cdn方法
<!-- 引入样式 -->
<link rel="stylesheet" href="https://unpkg.com/mint-ui/lib/style.css">
<!-- 引入组件库 -->
<script src="https://unpkg.com/mint-ui/lib/index.js"></script>
```

##### 在vue中引用

```js
// 在main.js
import Vue from 'vue'
import MintUI from 'mint-ui'
import 'mint-ui/lib/style.css'
import App from './App.vue'

Vue.use(MintUI)

new Vue({
  el: '#app',
  components: { App }
})
```

