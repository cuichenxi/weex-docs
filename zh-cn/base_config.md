

> 整个项目中，只有两个配置文件，都放在了项目根目录的 `config` 文件夹下。

## eros.native.js -> 客户端

客户端需要读取的配置文件。

> 注意:  **`eros.native.js` 每次变动，都需要执行命令 `eros pack` 打内置包到对应平台，并重新运行你的 app 。**

```javascript
{
    appName: "eros-demo",
    appBoard: "/config/index.js",
    androidIsListenHomeBack: "true",
    version: {
        android: "1.0.0",
        iOS: "1.0.0"
    },
    page: {
        homePage: "/pages/eros-demos/index.js",
        mediatorPage: "/mediator/index.js",
        navBarColor: "#3385ff"，
        navItemColor:"#ffffff"
    },
    url: {
        jsServer:"https://app.weex-eros.com:8889",
        image: "https://lev-inf.benmu-health.com/xxx/xxx",
        bundleUpdate: "http://localhosts:3001/app/check",
        debugServer:"ws://192.168.15.110:8088/debugProxy/native"
    },
    zipFolder: {
        server: "home/app",
        iOS: "/ios/WeexEros/WeexEros",
        android: "/android/WeexFrameworkWrapper/app/src/main/assets"
    },
    getui: {
        enabled: "false",
        appId: "",
        appKey: "",
        appSecret: ""
    },
    umeng: {
        enabled: "false",
        iOSAppKey: "",
        androidAppKey: ""
    },
    wechat: {
        enabled: "false",
        appId: "",
        appSecret: ""
    },
    amap: {
        enabled: "false",
        appKey: ""
    }
}
```
### 配置说明

#### **`appName`**: 脚手架自动生成 app 名称。

#### **`version`**: app 对应版本号，脚手架会自动初始化填写。
* iOS: iOS 对应版本号。`(增量发布相关)`
* android: andorid 对应版本号。`(增量发布相关)`

#### **`androidIsListenHomeBack`**: 安卓平台 是否监听第一个页面的物理返回键。

#### **`page`** 页面配置相关: 
* homePage: 主页的 js 相对地址。
* mediatorPage: 中介者页面的相对地址，中介者页面是为了让我们在多个页面（js）中间可以通过事件来通信。
* navBarColor: 原生导航条的默认颜色。
* navItemColor:  原生导航栏字体颜色。

#### **`url`**: 路经相关: 
* jsServer: 本地 js 的服务路径。
* image: 图片上传绝对路径。
* bundleUpdate: 检测jsBundle更新接口。`(增量发布相关)`

#### **`zipFolder`** 内置包存放地址。
* server: 自动生成差分包的目录，可将脚手架部署在服务器上，做增量发布。
* iOS: ios内置包地址，如果需要变更项目目录，请对应修改，如果不需要，就不要修改。
* android: android内置包地址，如果需要变更项目目录，请对应修改，如果不需要，就不要修改。

#### **`getui`** 个推 （目前项目推送服务是基于个推实现的）
* enabled: 是否启用个推服务。
* appId、appKey、appSecret 在个推平台申请。

#### **`umeng`** 友盟统计相关
* enabled: 是否启用友盟统计，友盟分享服务；
* iOSAppKey、androidAppKey  友盟后台申请的appkey，分别对应iOS、Android。

#### **`wechat`** 微信支付相关
* enabled: 是否启用微信支付，微信分享。
* appId、appSecret 微信开放平台申请。

#### **`amap`** 高德（工具集成了 高德地图SDK）
* enabled: 是否启用高德地图。
* appkey 填写从高德开发平台申请的appkey。

> 增量发布相关的配置可以结合着增量发布文章结合着一起看。

## eros.dev.js -> 脚手架

脚手架会去读取此项配置来让开发者进行开发，调试，语法检测，mock，生成增量包和全量包等能力

> 注意: **，`eros.dev.js` 每次变动，都需要重新启 eros dev 开发服务。**`(因为此项配置，是在脚手架启动的时候会回去读取)`。

```javascript
{
    exports: [
        "js/service/bus.js",
        "js/pages/home/index.js",
        "js/pages/demo/index.js",
        "js/pages/home/tab1/index.js",
        "js/pages/home/tab2/index.js",
        "js/pages/home/tab2/components/router/index.js",
        "js/pages/home/tab3/index.js"
    ],
    alias: {
        "Components": "js/components",
        "Common": "js/common",
        "Config": "js/config",
        "Widget": "js/widget",
        "Pages": "js/pages",
        "Utils": "js/utils"
    },
    eslint: false,
    server: { 
        "path": "./", 
        "port": 8889 
    },
    diff: {
        "pwd": "/Users/yangmingzhe/Work/opensource/eros-diff-folder",
        "proxy": "https://app.weex-eros.com/source"
    },
    proxy: [{ 
        "route": "/test", 
        "target": "127.0.0.1:52077/test" 
    }],
    mockServer: { 
        "port": 52077, 
        "mockDir": "./dist/mock" 
    }
}
```

##### **`exports (重要)`** 
暴露出你的页面对应的 js 地址，app中的每个页面本质上都对应一个 js，**所以你每次添加一个页面，都需要再次添加需要打包成 js bundle 的文件**。

##### **`alias`**
文件夹别名，方便快速访问。

```js
import component from 'Components' //对应 js/components
```
##### **`eslint`**
**项目编译时是否需要进行 eslint 检查**。

##### **`server`**
**脚手架运行 `eros dev` 服务的路径和端口，使用默认即可**。

##### **`proxy`**
代理相关，会把 `/test`路径对应的请求代理的到 `127.0.0.1:52077/test`，默认即可，无需修改。

##### **`mockServer`**
本地`mock`数据服务，因为`proxy`中已经把`/test`路径代理带`52077`端口，而我们在`52077`端口起了`mock`服务，所以能在本地`mock`数据。



