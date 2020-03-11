# 数据驱动
Vue.js的核心思想是数据驱动。数据驱动是指由数据驱动生成的，对视图的修改，不会直接操作DOM，而是通过修改数据。相比于jQuery大大简化了代码量，非常利于维护。
在Vue.js中可以采用简洁的模板语法来声明式的将数据渲染为DOM：
```JavaScript
<div id="app">
  {{ message }}
</div>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```
最终会在页面上渲染出`Hello Vue!`。