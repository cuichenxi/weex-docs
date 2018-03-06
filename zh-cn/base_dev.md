### 项目结构介绍
`src` 目录如下:
* iconfont：存放本地 iconfont，自动会打包到内置包中，可以通过 bmlocal的方式来进行加载 `bmlocal://iconfont/*.ttf`。
* assets：存放内置版本的 iconfont ，自动会打包到内置包中，可以通过 bmlocal的方式来进行加载 `bmlocal://assets/*.png`。
* mediator：所有页面的中介者，类似全局公用bus，你的页面都可以订阅上面的事件，也可以向中介者发布事件，这个页面是常驻内存的，但不会很耗内存，请谨慎而又潇洒的使用。
js：主要开发目录。
* mock：本地mock数据的地方，当启动服务时候便可以进行访问。


`js` 目录如下:
* components：组件存放。
* config：配置相关。
  * apis：请求别名配置。
  * routes：页面路由相关配置。
  * index：公共逻辑存放处，我们再此`实例化了 widget`。
  * push: 个推相关，对接了个推之后客户端都会把消息推动到这里的 **pushMessage** 事件里。
* css：样式存放，支持 `scss`、`stylus`、`less`等，可根据业务自行修改。
* pages：所有页面都放在这里，**这里就是你的playground！**我们内置了 demo，可以查看源码。
* widget：widget源码，由于 appboard 的机制，我们可以在开发中实时修改。
* utils：存放工具函数的地方，可以随意增删改。
js 中的文件夹可以根据你的业务做横向拓展。


### Hello World

1. 关闭页面上选项按钮中的拦截器选项，让我们 app 准备好访问我们的电脑上的开发 js bundle。

2. 告诉 `eros-cli` 你 hello world 页面的地址
```
// eros.dev.js
...
exports: [
   ...
   'js/pages/hello/index.vue'
]
...
```

3. 我们修改 `eros.native.js` 中的 `page.homePage` 为 `pages/hello/index.js`
> 因为 native.js 中是配置的 dist 文件下 js bundle 的路径，所以是以 .js 为结尾的。

4. 因为修改了 `eros.native.js` ，需要让客户端同学知道文件的变动，所以需要 `eros pack` 到对应的平台

5. 在 `pages` 下建立对应文件夹和文件

6. 项目根目录下执行 `eros dev`

7. `index.vue` 中就可以愉快的写vue代码了：
```js
<template>
    <div>hello world!</div>      
</template>
<script>
    export default {

    }
</script>
```

8. 点击模拟器中的**调试/刷新**，就能看到页面的变动了

9. 我们在刚刚 `index.vue` 中添加简单的弹窗逻辑，这里是使用了 widget 的写法，如果不使用 widget，可以参考 module 的 api 来编写。
```javascript
    created() {
      this.$notice.alert({
        title: '这是一个弹窗',
        message: '消息',
        okTitle: '确认',
        callback() {
          // 点击确认按钮的回调
        }
      })
    }
```
这样页面一进来就会打开一个弹窗。
10. 这样每次你在 hello 这个文件夹里做修改，每次保存，在 app 上点击调试并刷新，就可以实时看到改动啦。

`再次提醒`： 

> * **eros.native.js** 里面配置的是路由跳转的地址，也就是 dev 生成的 dist 文件夹下的 js bundle 路径，从 dist/js 开始，每次 `eros.native.js` 的变动，都需要重新 `eros pack`。
> * **eros.dev.js** 的 `exports` 需要打包的 js 地址，填入 src 的需要被打包成 js bundle 的地址，从 src 开始， 每次 `eros.dev.js` 文件的变动，都需要重新启动服务 `eros dev`。


