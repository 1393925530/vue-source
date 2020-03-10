# Vue.js源码目录设计
Vue.js的源码在src目录下，目录结构如下。
```
src
├── compiler        # 编译相关
├── core            # 核心代码
├── platforms       # 不同平台的支持
├── server          # 服务端渲染
├── sfc             # .vue 文件解析
├── shared          # 共享代码
```
## compiler
compiler目录包含Vue.js所有编译相关的代码。它包括把模板解析成ast语法树，ast语法树优化，代码生成等功能。
编译的工作可以在构建时做，也可以在运行时做，使用包含构建功能的Vue.js。编译推荐前者——离线编译。
## core
core目录包含了核心代码，包括内置组件、全局API封装，需要重点分析。
## platform
Vue.js是一个跨平台MVVM框架，可以跑在web上，也可以配合weex跑在native客户端上。platform是Vue.js的入口，2个目录代表2个主要入口，分别打包运行在web上和weex上的Vue.js。
## server
Vue.js2.0支持了服务器端渲染，所有服务器渲染的逻辑都在这个目录下。
服务器渲染主要的工作是把组件渲染成服务器端的HTML字符串，将他们直接发送到浏览器，最后将静态标记“混合”为客户端上完全交互的应用程序。
## sfc
开发Vue.js都会借助webpack创建，然后通过.vue单文件的编写组件。此目录下的代码逻辑会把.vue文件内容解析成一个JavaScript对象。
## shared
Vue.js会定义一些工具方法，这里定义的工具方法是被浏览器端的Vue.js和服务器端的Vue.js共享的。
# 总结
功能模块拆分清楚带来的效果就是代码的阅读性和可维护性都很强。