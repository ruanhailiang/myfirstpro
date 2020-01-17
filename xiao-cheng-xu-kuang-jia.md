# 小程序框架

## 场景值 <a id="&#x573A;&#x666F;&#x503C;"></a>

> 基础库 1.1.0 开始支持，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。

场景值用来描述用户进入小程序的路径。完整场景值的含义请查看[场景值列表](https://developers.weixin.qq.com/miniprogram/dev/reference/scene-list.html)。

由于Android系统限制，目前还无法获取到按 Home 键退出到桌面，然后从桌面再次进小程序的场景值，对于这种情况，会保留上一次的场景值。

 **获取场景值**

开发者可以通过下列方式获取场景值：

* 对于小程序，可以在 `App` 的 `onLaunch` 和 `onShow`，或[wx.getLaunchOptionsSync](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/life-cycle/wx.getLaunchOptionsSync.html) 中获取上述场景值。
* 对于小游戏，可以在 [wx.getLaunchOptionsSync](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/life-cycle/wx.getLaunchOptionsSync.html) 和 [wx.onShow](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/%28wx.onShow%29) 中获取上述场景值

 **返回来源信息的场景**

部分场景值下还可以获取来源应用、公众号或小程序的appId。获取方式请参考对应API的参考文档。

| 场景值 | 场景 | appId含义 |
| :--- | :--- | :--- |
| 1020 | 公众号 profile 页相关小程序列表 | 来源公众号 |
| 1035 | 公众号自定义菜单 | 来源公众号 |
| 1036 | App 分享消息卡片 | 来源App |
| 1037 | 小程序打开小程序 | 来源小程序 |
| 1038 | 从另一个小程序返回 | 来源小程序 |
| 1043 | 公众号模板消息 | 来源公众号 |

## 逻辑层 App Service <a id="&#x903B;&#x8F91;&#x5C42;-App-Service"></a>

小程序开发框架的逻辑层使用 `JavaScript` 引擎为小程序提供开发者 `JavaScript` 代码的运行环境以及微信小程序的特有功能。

逻辑层将数据进行处理后发送给视图层，同时接受视图层的事件反馈。

开发者写的所有代码最终将会打包成一份 `JavaScript` 文件，并在小程序启动的时候运行，直到小程序销毁。这一行为类似 [ServiceWorker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)，所以逻辑层也称之为 App Service。

在 `JavaScript` 的基础上，我们增加了一些功能，以方便小程序的开发：

* 增加 `App` 和 `Page` 方法，进行[程序注册](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/app.html)和[页面注册](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html)。
* 增加 `getApp` 和 `getCurrentPages` 方法，分别用来获取 `App` 实例和当前页面栈。
* 提供丰富的 [API](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/api.html)，如微信用户数据，扫一扫，支付等微信特有能力。
* 提供[模块化](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/module.html#模块化)能力，每个页面有独立的[作用域](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/module.html#文件作用域)。

**注意：小程序框架的逻辑层并非运行在浏览器中，因此 `JavaScript` 在 web 中一些能力都无法使用，如 `window`，`document` 等。**

## 注册小程序 <a id="&#x6CE8;&#x518C;&#x5C0F;&#x7A0B;&#x5E8F;"></a>

每个小程序都需要在 `app.js` 中调用 `App` 方法注册小程序实例，绑定生命周期回调函数、错误监听和页面不存在监听函数等。

详细的参数含义和使用请参考 [App 参考文档](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html) 。

```text
// app.js
App({
  onLaunch (options) {
    // Do something initial when launch.
  },
  onShow (options) {
    // Do something when show.
  },
  onHide () {
    // Do something when hide.
  },
  onError (msg) {
    console.log(msg)
  },
  globalData: 'I am global data'
})
```

整个小程序只有一个 App 实例，是全部页面共享的。开发者可以通过 `getApp` 方法获取到全局唯一的 App 实例，获取App上的数据或调用开发者注册在 `App` 上的函数。

```text
// xxx.js
const appInstance = getApp()
console.log(appInstance.globalData) // I am global data
```

## 注册页面 <a id="&#x6CE8;&#x518C;&#x9875;&#x9762;"></a>

对于小程序中的每个页面，都需要在页面对应的 `js` 文件中进行注册，指定页面的初始数据、生命周期回调、事件处理函数等。

###  使用 Page 构造器注册页面 <a id="&#x4F7F;&#x7528;-Page-&#x6784;&#x9020;&#x5668;&#x6CE8;&#x518C;&#x9875;&#x9762;"></a>

简单的页面可以使用 `Page()` 进行构造。

**代码示例：**

```text
//index.js
Page({
  data: {
    text: "This is page data."
  },
  onLoad: function(options) {
    // 页面创建时执行
  },
  onShow: function() {
    // 页面出现在前台时执行
  },
  onReady: function() {
    // 页面首次渲染完毕时执行
  },
  onHide: function() {
    // 页面从前台变为后台时执行
  },
  onUnload: function() {
    // 页面销毁时执行
  },
  onPullDownRefresh: function() {
    // 触发下拉刷新时执行
  },
  onReachBottom: function() {
    // 页面触底时执行
  },
  onShareAppMessage: function () {
    // 页面被用户分享时执行
  },
  onPageScroll: function() {
    // 页面滚动时执行
  },
  onResize: function() {
    // 页面尺寸变化时执行
  },
  onTabItemTap(item) {
    // tab 点击时执行
    console.log(item.index)
    console.log(item.pagePath)
    console.log(item.text)
  },
  // 事件响应函数
  viewTap: function() {
    this.setData({
      text: 'Set some data for updating view.'
    }, function() {
      // this is setData callback
    })
  },
  // 自由数据
  customData: {
    hi: 'MINA'
  }
})
```

详细的参数含义和使用请参考 [Page 参考文档](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html) 。

###  在页面中使用 behaviors <a id="&#x5728;&#x9875;&#x9762;&#x4E2D;&#x4F7F;&#x7528;-behaviors"></a>

> 基础库 2.9.2 开始支持，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。

页面可以引用 behaviors 。 behaviors 可以用来让多个页面有相同的数据字段和方法。

```text
// my-behavior.js
module.exports = Behavior({
  data: {
    sharedText: 'This is a piece of data shared between pages.'
  },
  methods: {
    sharedMethod: function() {
      this.data.sharedText === 'This is a piece of data shared between pages.'
    }
  }
})
```

```text
// page-a.js
var myBehavior = require('./my-behavior.js')
Page({
  behaviors: [myBehavior],
  onLoad: function() {
    this.data.sharedText === 'This is a piece of data shared between pages.'
  }
})
```

具体用法参见 [behaviors](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/behaviors.html) 。

###  使用 Component 构造器构造页面 <a id="&#x4F7F;&#x7528;-Component-&#x6784;&#x9020;&#x5668;&#x6784;&#x9020;&#x9875;&#x9762;"></a>

> 基础库 1.6.3 开始支持，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。

`Page` 构造器适用于简单的页面。但对于复杂的页面， `Page` 构造器可能并不好用。

此时，可以使用 `Component` 构造器来构造页面。 `Component` 构造器的主要区别是：方法需要放在 `methods: { }` 里面。

**代码示例：**

```text
Component({
  data: {
    text: "This is page data."
  },
  methods: {
    onLoad: function(options) {
      // 页面创建时执行
    },
    onPullDownRefresh: function() {
      // 下拉刷新时执行
    },
    // 事件响应函数
    viewTap: function() {
      // ...
    }
  }
})
```

这种创建方式非常类似于 [自定义组件](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/) ，可以像自定义组件一样使用 `behaviors` 等高级特性。

具体细节请阅读 [`Component` 构造器](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/component.html) 章节。

### 生命周期 <a id="&#x751F;&#x547D;&#x5468;&#x671F;"></a>

**以下内容你不需要立马完全弄明白，不过以后它会有帮助。**

下图说明了页面 `Page` 实例的生命周期。

![](.gitbook/assets/page-lifecycle.2e646c86.png)



## 页面路由 <a id="&#x9875;&#x9762;&#x8DEF;&#x7531;"></a>

在小程序中所有页面的路由全部由框架进行管理。

####  页面栈 <a id="&#x9875;&#x9762;&#x6808;"></a>

框架以栈的形式维护了当前的所有页面。 当发生路由切换的时候，页面栈的表现如下：

| 路由方式 | 页面栈表现 |
| :--- | :--- |
| 初始化 | 新页面入栈 |
| 打开新页面 | 新页面入栈 |
| 页面重定向 | 当前页面出栈，新页面入栈 |
| 页面返回 | 页面不断出栈，直到目标返回页 |
| Tab 切换 | 页面全部出栈，只留下新的 Tab 页面 |
| 重加载 | 页面全部出栈，只留下新的页面 |

开发者可以使用 `getCurrentPages()` 函数获取当前页面栈。

####  路由方式 <a id="&#x8DEF;&#x7531;&#x65B9;&#x5F0F;"></a>

对于路由的触发方式以及页面生命周期函数如下：

| 路由方式 | 触发时机 | 路由前页面 | 路由后页面 |
| :--- | :--- | :--- | :--- |
| 初始化 | 小程序打开的第一个页面 |  | onLoad, onShow |
| 打开新页面 | 调用 API [wx.navigateTo](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.navigateTo.html)  使用组件 [`<navigator open-type="navigateTo"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html) | onHide | onLoad, onShow |
| 页面重定向 | 调用 API [wx.redirectTo](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.redirectTo.html)  使用组件 [`<navigator open-type="redirectTo"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html) | onUnload | onLoad, onShow |
| 页面返回 | 调用 API [wx.navigateBack](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.navigateBack.html)  使用组件[`<navigator open-type="navigateBack">`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html)  用户按左上角返回按钮 | onUnload | onShow |
| Tab 切换 | 调用 API [wx.switchTab](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.switchTab.html)  使用组件 [`<navigator open-type="switchTab"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html)  用户切换 Tab |  | 各种情况请参考下表 |
| 重启动 | 调用 API [wx.reLaunch](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.reLaunch.html)  使用组件 [`<navigator open-type="reLaunch"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html) | onUnload | onLoad, onShow |

Tab 切换对应的生命周期（以 A、B 页面为 Tabbar 页面，C 是从 A 页面打开的页面，D 页面是从 C 页面打开的页面为例）：

| 当前页面 | 路由后页面 | 触发的生命周期（按顺序） |
| :--- | :--- | :--- |
| A | A | Nothing happend |
| A | B | A.onHide\(\), B.onLoad\(\), B.onShow\(\) |
| A | B（再次打开） | A.onHide\(\), B.onShow\(\) |
| C | A | C.onUnload\(\), A.onShow\(\) |
| C | B | C.onUnload\(\), B.onLoad\(\), B.onShow\(\) |
| D | B | D.onUnload\(\), C.onUnload\(\), B.onLoad\(\), B.onShow\(\) |
| D（从转发进入） | A | D.onUnload\(\), A.onLoad\(\), A.onShow\(\) |
| D（从转发进入） | B | D.onUnload\(\), B.onLoad\(\), B.onShow\(\) |

**Tips**:

* `navigateTo`, `redirectTo` 只能打开非 tabBar 页面。
* `switchTab` 只能打开 tabBar 页面。
* `reLaunch` 可以打开任意页面。
* 页面底部的 tabBar 由页面决定，即只要是定义为 tabBar 的页面，底部都有 tabBar。
* 调用页面路由带的参数可以在目标页面的`onLoad`中获取。

### 模块化 <a id="&#x6A21;&#x5757;&#x5316;"></a>

可以将一些公共的代码抽离成为一个单独的 js 文件，作为一个模块。模块只有通过 [`module.exports`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/module.html) 或者 `exports` 才能对外暴露接口。

注意：

* `exports` 是 [`module.exports`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/module.html) 的一个引用，因此在模块里边随意更改 `exports` 的指向会造成未知的错误。所以更推荐开发者采用 `module.exports` 来暴露模块接口，除非你已经清晰知道这两者的关系。
* 小程序目前不支持直接引入 `node_modules` , 开发者需要使用到 `node_modules` 时候建议拷贝出相关的代码到小程序的目录中，或者使用小程序支持的 [npm](https://developers.weixin.qq.com/miniprogram/dev/devtools/npm.html) 功能。

```text
// common.js
function sayHello(name) {
  console.log(`Hello ${name} !`)
}
function sayGoodbye(name) {
  console.log(`Goodbye ${name} !`)
}

module.exports.sayHello = sayHello
exports.sayGoodbye = sayGoodbye
```

​在需要使用这些模块的文件中，使用 `require` 将公共代码引入

```text
var common = require('common.js')
Page({
  helloMINA: function() {
    common.sayHello('MINA')
  },
  goodbyeMINA: function() {
    common.sayGoodbye('MINA')
  }
})
```

####  文件作用域 <a id="&#x6587;&#x4EF6;&#x4F5C;&#x7528;&#x57DF;"></a>

在 JavaScript 文件中声明的变量和函数只在该文件中有效；不同的文件中可以声明相同名字的变量和函数，不会互相影响。

通过全局函数 `getApp` 可以获取全局的应用实例，如果需要全局的数据可以在 `App()` 中设置，如：

```text
// app.js
App({
  globalData: 1
})
```

```text
// a.js
// The localValue can only be used in file a.js.
var localValue = 'a'
// Get the app instance.
var app = getApp()
// Get the global data and change it.
app.globalData++
```

```text
// b.js
// You can redefine localValue in file b.js, without interference with the localValue in a.js.
var localValue = 'b'
// If a.js it run before b.js, now the globalData shoule be 2.
console.log(getApp().globalData)
```

## API <a id="API"></a>

小程序开发框架提供丰富的微信原生 API，可以方便的调起微信提供的能力，如获取用户信息，本地存储，支付功能等。详细介绍请参考 [API 文档](https://developers.weixin.qq.com/miniprogram/dev/api/index.html)。

通常，在小程序 API 有以下几种类型：

###  事件监听 API <a id="&#x4E8B;&#x4EF6;&#x76D1;&#x542C;-API"></a>

我们约定，以 `on` 开头的 API 用来监听某个事件是否触发，如：[wx.onSocketOpen](https://developers.weixin.qq.com/miniprogram/dev/api/network/websocket/wx.onSocketOpen.html)，[wx.onCompassChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/compass/wx.onCompassChange.html) 等。

这类 API 接受一个回调函数作为参数，当事件触发时会调用这个回调函数，并将相关数据以参数形式传入。

**代码示例**

```text
wx.onCompassChange(function (res) {
  console.log(res.direction)
})
```

###  同步 API <a id="&#x540C;&#x6B65;-API"></a>

我们约定，以 `Sync` 结尾的 API 都是同步 API， 如 [wx.setStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.setStorageSync.html)，[wx.getSystemInfoSync](https://developers.weixin.qq.com/miniprogram/dev/api/base/system/system-info/wx.getSystemInfoSync.html) 等。此外，也有一些其他的同步 API，如 [wx.createWorker](https://developers.weixin.qq.com/miniprogram/dev/api/worker/wx.createWorker.html)，[wx.getBackgroundAudioManager](https://developers.weixin.qq.com/miniprogram/dev/api/media/background-audio/wx.getBackgroundAudioManager.html) 等，详情参见 API 文档中的说明。

同步 API 的执行结果可以通过函数返回值直接获取，如果执行出错会抛出异常。

**代码示例**

```text
try {
  wx.setStorageSync('key', 'value')
} catch (e) {
  console.error(e)
}
```

###  异步 API <a id="&#x5F02;&#x6B65;-API"></a>

大多数 API 都是异步 API，如 [wx.request](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html)，[wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) 等。这类 API 接口通常都接受一个 `Object` 类型的参数，这个参数都支持按需指定以下字段来接收接口调用结果：

**Object 参数说明**

| 参数名 | 类型 | 必填 | 说明 |
| :--- | :--- | :--- | :--- |
| success | function | 否 | 接口调用成功的回调函数 |
| fail | function | 否 | 接口调用失败的回调函数 |
| complete | function | 否 | 接口调用结束的回调函数（调用成功、失败都会执行） |
| 其他 | Any | - | 接口定义的其他参数 |

**回调函数的参数**

`success`，`fail`，`complete` 函数调用时会传入一个 `Object` 类型参数，包含以下字段：

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| errMsg | string | 错误信息，如果调用成功返回 `${apiName}:ok` |
| errCode | number | 错误码，仅部分 API 支持，具体含义请参考对应 API 文档，成功时为 `0`。 |
| 其他 | Any | 接口返回的其他数据 |

异步 API 的执行结果需要通过 `Object` 类型的参数中传入的对应回调函数获取。部分异步 API 也会有返回值，可以用来实现更丰富的功能，如 [wx.request](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html)，[wx.connectSocket](https://developers.weixin.qq.com/miniprogram/dev/api/network/websocket/wx.connectSocket.html) 等。

**代码示例**

```text
wx.login({
  success(res) {
    console.log(res.code)
  }
})
```

## 视图层 View <a id="&#x89C6;&#x56FE;&#x5C42;-View"></a>

框架的视图层由 WXML 与 WXSS 编写，由组件来进行展示。

将逻辑层的数据反应成视图，同时将视图层的事件发送给逻辑层。

WXML\(WeiXin Markup language\) 用于描述页面的结构。

WXS\(WeiXin Script\) 是小程序的一套脚本语言，结合 `WXML`，可以构建出页面的结构。

WXSS\(WeiXin Style Sheet\) 用于描述页面的样式。

组件\(Component\)是视图的基本组成单元。

## WXML <a id="WXML"></a>

WXML（WeiXin Markup Language）是框架设计的一套标签语言，结合[基础组件](https://developers.weixin.qq.com/miniprogram/dev/component/)、[事件系统](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html)，可以构建出页面的结构。

要完整了解 WXML 语法，请参考[WXML 语法参考](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/)。

用以下一些简单的例子来看看 WXML 具有什么能力：

####  数据绑定 <a id="&#x6570;&#x636E;&#x7ED1;&#x5B9A;"></a>

```text
<!--wxml-->
<view> {{message}} </view>
```

```text
// page.js
Page({
  data: {
    message: 'Hello MINA!'
  }
})
```

####  列表渲染 <a id="&#x5217;&#x8868;&#x6E32;&#x67D3;"></a>

```text
<!--wxml-->
<view wx:for="{{array}}"> {{item}} </view>
```

```text
// page.js
Page({
  data: {
    array: [1, 2, 3, 4, 5]
  }
})
```

####  条件渲染 <a id="&#x6761;&#x4EF6;&#x6E32;&#x67D3;"></a>

```text
<!--wxml-->
<view wx:if="{{view == 'WEBVIEW'}}"> WEBVIEW </view>
<view wx:elif="{{view == 'APP'}}"> APP </view>
<view wx:else="{{view == 'MINA'}}"> MINA </view>
```

```text
// page.js
Page({
  data: {
    view: 'MINA'
  }
})
```

####  模板 <a id="&#x6A21;&#x677F;"></a>

```text
<!--wxml-->
<template name="staffName">
  <view>
    FirstName: {{firstName}}, LastName: {{lastName}}
  </view>
</template>

<template is="staffName" data="{{...staffA}}"></template>
<template is="staffName" data="{{...staffB}}"></template>
<template is="staffName" data="{{...staffC}}"></template>
```

```text
// page.js
Page({
  data: {
    staffA: {firstName: 'Hulk', lastName: 'Hu'},
    staffB: {firstName: 'Shang', lastName: 'You'},
    staffC: {firstName: 'Gideon', lastName: 'Lin'}
  }
})
```

具体的能力以及使用方式在以下章节查看：

[数据绑定](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/data.html)、[列表渲染](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/list.html)、[条件渲染](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/conditional.html)、[模板](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/template.html)、[引用](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/import.html)

## WXSS <a id="WXSS"></a>

WXSS \(WeiXin Style Sheets\)是一套样式语言，用于描述 WXML 的组件样式。

WXSS 用来决定 WXML 的组件应该怎么显示。

为了适应广大的前端开发者，WXSS 具有 CSS 大部分特性。同时为了更适合开发微信小程序，WXSS 对 CSS 进行了扩充以及修改。

与 CSS 相比，WXSS 扩展的特性有：

* 尺寸单位
* 样式导入

####  尺寸单位 <a id="&#x5C3A;&#x5BF8;&#x5355;&#x4F4D;"></a>

* rpx（responsive pixel）: 可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。

| 设备 | rpx换算px \(屏幕宽度/750\) | px换算rpx \(750/屏幕宽度\) |
| :--- | :--- | :--- |
| iPhone5 | 1rpx = 0.42px | 1px = 2.34rpx |
| iPhone6 | 1rpx = 0.5px | 1px = 2rpx |
| iPhone6 Plus | 1rpx = 0.552px | 1px = 1.81rpx |

**建议：** 开发微信小程序时设计师可以用 iPhone6 作为视觉稿的标准。

**注意：** 在较小的屏幕上不可避免的会有一些毛刺，请在开发时尽量避免这种情况。

####  样式导入 <a id="&#x6837;&#x5F0F;&#x5BFC;&#x5165;"></a>

使用`@import`语句可以导入外联样式表，`@import`后跟需要导入的外联样式表的相对路径，用`;`表示语句结束。

**示例代码：**

```text
/** common.wxss **/
.small-p {
  padding:5px;
}
```

```text
/** app.wxss **/
@import "common.wxss";
.middle-p {
  padding:15px;
}
```

####  内联样式 <a id="&#x5185;&#x8054;&#x6837;&#x5F0F;"></a>

框架组件上支持使用 style、class 属性来控制组件的样式。

* style：静态的样式统一写到 class 中。style 接收动态的样式，在运行时会进行解析，请尽量避免将静态的样式写进 style 中，以免影响渲染速度。

```text
<view style="color:{{color}};" />
```

* class：用于指定样式规则，其属性值是样式规则中类选择器名\(样式类名\)的集合，样式类名不需要带上`.`，样式类名之间用空格分隔。

```text
<view class="normal_view" />
```

####  选择器 <a id="&#x9009;&#x62E9;&#x5668;"></a>

目前支持的选择器有：

| 选择器 | 样例 | 样例描述 |
| :--- | :--- | :--- |
| .class | `.intro` | 选择所有拥有 class="intro" 的组件 |
| \#id | `#firstname` | 选择拥有 id="firstname" 的组件 |
| element | `view` | 选择所有 view 组件 |
| element, element | `view, checkbox` | 选择所有文档的 view 组件和所有的 checkbox 组件 |
| ::after | `view::after` | 在 view 组件后边插入内容 |
| ::before | `view::before` | 在 view 组件前边插入内容 |

####  全局样式与局部样式 <a id="&#x5168;&#x5C40;&#x6837;&#x5F0F;&#x4E0E;&#x5C40;&#x90E8;&#x6837;&#x5F0F;"></a>

定义在 app.wxss 中的样式为全局样式，作用于每一个页面。在 page 的 wxss 文件中定义的样式为局部样式，只作用在对应的页面，并会覆盖 app.wxss 中相同的选择器。



## WXS <a id="WXS"></a>

WXS（WeiXin Script）是小程序的一套脚本语言，结合 `WXML`，可以构建出页面的结构。

####  注意 <a id="&#x6CE8;&#x610F;"></a>

1. WXS 不依赖于运行时的基础库版本，可以在所有版本的小程序中运行。
2. WXS 与 JavaScript 是不同的语言，有自己的语法，并不和 JavaScript 一致。
3. WXS 的运行环境和其他 JavaScript 代码是隔离的，WXS 中不能调用其他 JavaScript 文件中定义的函数，也不能调用小程序提供的API。
4. WXS 函数不能作为组件的事件回调。
5. 由于运行环境的差异，在 iOS 设备上小程序内的 WXS 会比 JavaScript 代码快 2 ~ 20 倍。在 android 设备上二者运行效率无差异。

以下是一些使用 WXS 的简单示例，要完整了解 WXS 语法，请参考[WXS 语法参考](https://developers.weixin.qq.com/miniprogram/dev/reference/wxs/)。

####  页面渲染 <a id="&#x9875;&#x9762;&#x6E32;&#x67D3;"></a>

```text
<!--wxml-->
<wxs module="m1">
var msg = "hello world";

module.exports.message = msg;
</wxs>

<view> {{m1.message}} </view>
```

页面输出：

```text
hello world
```

####  数据处理 <a id="&#x6570;&#x636E;&#x5904;&#x7406;"></a>

```text
// page.js
Page({
  data: {
    array: [1, 2, 3, 4, 5, 1, 2, 3, 4]
  }
})
```

```text
<!--wxml-->
<!-- 下面的 getMax 函数，接受一个数组，且返回数组中最大的元素的值 -->
<wxs module="m1">
var getMax = function(array) {
  var max = undefined;
  for (var i = 0; i < array.length; ++i) {
    max = max === undefined ?
      array[i] :
      (max >= array[i] ? max : array[i]);
  }
  return max;
}

module.exports.getMax = getMax;
</wxs>

<!-- 调用 wxs 里面的 getMax 函数，参数为 page.js 里面的 array -->
<view> {{m1.getMax(array)}} </view>
```

页面输出：

```text
5
```

## 事件 <a id="&#x4E8B;&#x4EF6;"></a>

###  什么是事件 <a id="&#x4EC0;&#x4E48;&#x662F;&#x4E8B;&#x4EF6;"></a>

* 事件是视图层到逻辑层的通讯方式。
* 事件可以将用户的行为反馈到逻辑层进行处理。
* 事件可以绑定在组件上，当达到触发事件，就会执行逻辑层中对应的事件处理函数。
* 事件对象可以携带额外信息，如 id, dataset, touches。

###  事件的使用方式 <a id="&#x4E8B;&#x4EF6;&#x7684;&#x4F7F;&#x7528;&#x65B9;&#x5F0F;"></a>

* 在组件中绑定一个事件处理函数。

如`bindtap`，当用户点击该组件的时候会在该页面对应的Page中找到相应的事件处理函数。

```text
<view id="tapTest" data-hi="WeChat" bindtap="tapName"> Click me! </view>
```

* 在相应的Page定义中写上相应的事件处理函数，参数是event。

```text
Page({
  tapName: function(event) {
    console.log(event)
  }
})
```

* 可以看到log出来的信息大致如下：

```text
{
  "type":"tap",
  "timeStamp":895,
  "target": {
    "id": "tapTest",
    "dataset":  {
      "hi":"WeChat"
    }
  },
  "currentTarget":  {
    "id": "tapTest",
    "dataset": {
      "hi":"WeChat"
    }
  },
  "detail": {
    "x":53,
    "y":14
  },
  "touches":[{
    "identifier":0,
    "pageX":53,
    "pageY":14,
    "clientX":53,
    "clientY":14
  }],
  "changedTouches":[{
    "identifier":0,
    "pageX":53,
    "pageY":14,
    "clientX":53,
    "clientY":14
  }]
}
```

###  使用WXS函数响应事件 <a id="&#x4F7F;&#x7528;WXS&#x51FD;&#x6570;&#x54CD;&#x5E94;&#x4E8B;&#x4EF6;"></a>

> 基础库 2.4.4 开始支持，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。

从基础库版本`2.4.4`开始，支持使用WXS函数绑定事件，WXS函数接受2个参数，第一个是event，在原有的event的基础上加了`event.instance`对象，第二个参数是`ownerInstance`，和`event.instance`一样是一个`ComponentDescriptor`对象。具体使用如下：

* 在组件中绑定和注册事件处理的WXS函数。

```text
<wxs module="wxs" src="./test.wxs"></wxs>
<view id="tapTest" data-hi="WeChat" bindtap="{{wxs.tapName}}"> Click me! </view>
**注：绑定的WXS函数必须用{{}}括起来**

```

* test.wxs文件实现tapName函数

```text
function tapName(event, ownerInstance) {
  console.log('tap wechat', JSON.stringify(event))
}
module.exports = {
  tapName: tapName
}
```

`ownerInstance`包含了一些方法，可以设置组件的样式和class，具体包含的方法以及为什么要用WXS函数响应事件，请[点击查看详情](https://developers.weixin.qq.com/miniprogram/dev/framework/view/interactive-animation.html)。

###  事件详解 <a id="&#x4E8B;&#x4EF6;&#x8BE6;&#x89E3;"></a>

####  事件分类 <a id="&#x4E8B;&#x4EF6;&#x5206;&#x7C7B;"></a>

事件分为冒泡事件和非冒泡事件：

1. 冒泡事件：当一个组件上的事件被触发后，该事件会向父节点传递。
2. 非冒泡事件：当一个组件上的事件被触发后，该事件不会向父节点传递。

WXML的冒泡事件列表：

| 类型 | 触发条件 | 最低版本 |
| :--- | :--- | :--- |
| touchstart | 手指触摸动作开始 |  |
| touchmove | 手指触摸后移动 |  |
| touchcancel | 手指触摸动作被打断，如来电提醒，弹窗 |  |
| touchend | 手指触摸动作结束 |  |
| tap | 手指触摸后马上离开 |  |
| longpress | 手指触摸后，超过350ms再离开，如果指定了事件回调函数并触发了这个事件，tap事件将不被触发 | [1.5.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| longtap | 手指触摸后，超过350ms再离开（推荐使用longpress事件代替） |  |
| transitionend | 会在 WXSS transition 或 wx.createAnimation 动画结束后触发 |  |
| animationstart | 会在一个 WXSS animation 动画开始时触发 |  |
| animationiteration | 会在一个 WXSS animation 一次迭代结束时触发 |  |
| animationend | 会在一个 WXSS animation 动画完成时触发 |  |
| touchforcechange | 在支持 3D Touch 的 iPhone 设备，重按时会触发 | [1.9.90](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

**注：除上表之外的其他组件自定义事件如无特殊声明都是非冒泡事件，如** [**form**](https://developers.weixin.qq.com/miniprogram/dev/component/form.html) **的`submit`事件，**[**input**](https://developers.weixin.qq.com/miniprogram/dev/component/input.html) **的`input`事件，**[**scroll-view**](https://developers.weixin.qq.com/miniprogram/dev/component/scroll-view.html) **的`scroll`事件，\(详见各个**[**组件**](https://developers.weixin.qq.com/miniprogram/dev/component/)**\)**

####  普通事件绑定 <a id="&#x666E;&#x901A;&#x4E8B;&#x4EF6;&#x7ED1;&#x5B9A;"></a>

事件绑定的写法类似于组件的属性，如：

```text
<view bindtap="handleTap">
    Click here!
</view>
```

如果用户点击这个 view ，则页面的 `handleTap` 会被调用。

事件绑定函数可以是一个数据绑定，如：

```text
<view bindtap="{{ handlerName }}">
    Click here!
</view>
```

此时，页面的 `this.data.handlerName` 必须是一个字符串，指定事件处理函数名；如果它是个空字符串，则这个绑定会失效（可以利用这个特性来暂时禁用一些事件）。

自基础库版本 [1.5.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 起，在大多数组件和自定义组件中， `bind` 后可以紧跟一个冒号，其含义不变，如 `bind:tap` 。基础库版本 [2.8.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 起，在所有组件中开始提供这个支持。

####  绑定并阻止事件冒泡 <a id="&#x7ED1;&#x5B9A;&#x5E76;&#x963B;&#x6B62;&#x4E8B;&#x4EF6;&#x5192;&#x6CE1;"></a>

除 `bind` 外，也可以用 `catch` 来绑定事件。与 `bind` 不同， `catch` 会阻止事件向上冒泡。

例如在下边这个例子中，点击 inner view 会先后调用`handleTap3`和`handleTap2`\(因为tap事件会冒泡到 middle view，而 middle view 阻止了 tap 事件冒泡，不再向父节点传递\)，点击 middle view 会触发`handleTap2`，点击 outer view 会触发`handleTap1`。

```text
<view id="outer" bindtap="handleTap1">
  outer view
  <view id="middle" catchtap="handleTap2">
    middle view
    <view id="inner" bindtap="handleTap3">
      inner view
    </view>
  </view>
</view>
```

####  互斥事件绑定 <a id="&#x4E92;&#x65A5;&#x4E8B;&#x4EF6;&#x7ED1;&#x5B9A;"></a>

自基础库版本 [2.8.2](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 起，除 `bind` 和 `catch` 外，还可以使用 `mut-bind` 来绑定事件。一个 `mut-bind` 触发后，如果事件冒泡到其他节点上，其他节点上的 `mut-bind` 绑定函数不会被触发，但 `bind` 绑定函数和 `catch` 绑定函数依旧会被触发。

换而言之，所有 `mut-bind` 是“互斥”的，只会有其中一个绑定函数被触发。同时，它完全不影响 `bind` 和 `catch` 的绑定效果。

例如在下边这个例子中，点击 inner view 会先后调用 `handleTap3` 和 `handleTap2` ，点击 middle view 会调用 `handleTap2` 和 `handleTap1` 。

```text
<view id="outer" mut-bind:tap="handleTap1">
  outer view
  <view id="middle" bindtap="handleTap2">
    middle view
    <view id="inner" mut-bind:tap="handleTap3">
      inner view
    </view>
  </view>
</view>
```

####  事件的捕获阶段 <a id="&#x4E8B;&#x4EF6;&#x7684;&#x6355;&#x83B7;&#x9636;&#x6BB5;"></a>

自基础库版本 [1.5.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 起，触摸类事件支持捕获阶段。捕获阶段位于冒泡阶段之前，且在捕获阶段中，事件到达节点的顺序与冒泡阶段恰好相反。需要在捕获阶段监听事件时，可以采用`capture-bind`、`capture-catch`关键字，后者将中断捕获阶段和取消冒泡阶段。

在下面的代码中，点击 inner view 会先后调用`handleTap2`、`handleTap4`、`handleTap3`、`handleTap1`。

```text
<view id="outer" bind:touchstart="handleTap1" capture-bind:touchstart="handleTap2">
  outer view
  <view id="inner" bind:touchstart="handleTap3" capture-bind:touchstart="handleTap4">
    inner view
  </view>
</view>
```

如果将上面代码中的第一个`capture-bind`改为`capture-catch`，将只触发`handleTap2`。

```text
<view id="outer" bind:touchstart="handleTap1" capture-catch:touchstart="handleTap2">
  outer view
  <view id="inner" bind:touchstart="handleTap3" capture-bind:touchstart="handleTap4">
    inner view
  </view>
</view>
```

####  事件对象 <a id="&#x4E8B;&#x4EF6;&#x5BF9;&#x8C61;"></a>

如无特殊说明，当组件触发事件时，逻辑层绑定该事件的处理函数会收到一个事件对象。

**BaseEvent 基础事件对象属性列表：**

| 属性 | 类型 | 说明 | 基础库版本 |
| :--- | :--- | :--- | :--- |
| [type](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html#type) | String | 事件类型 |  |
| [timeStamp](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html#timeStamp) | Integer | 事件生成时的时间戳 |  |
| [target](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html#target) | Object | 触发事件的组件的一些属性值集合 |  |
| [currentTarget](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html#currenttarget) | Object | 当前组件的一些属性值集合 |  |
| [mark](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html#mark) | Object | 事件标记数据 | [2.7.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

**CustomEvent 自定义事件对象属性列表（继承 BaseEvent）：**

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| [detail](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html#detail) | Object | 额外的信息 |

**TouchEvent 触摸事件对象属性列表（继承 BaseEvent）：**

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| [touches](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html#touches) | Array | 触摸事件，当前停留在屏幕中的触摸点信息的数组 |
| [changedTouches](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html#changedTouches) | Array | 触摸事件，当前变化的触摸点信息的数组 |

**特殊事件：** [**canvas**](https://developers.weixin.qq.com/miniprogram/dev/component/canvas.html) **中的触摸事件不可冒泡，所以没有 currentTarget。**

####  type <a id="type"></a>

代表事件的类型。

####  timeStamp <a id="timeStamp"></a>

页面打开到触发事件所经过的毫秒数。

####  target <a id="target"></a>

触发事件的源组件。

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | String | 事件源组件的id |
| [dataset](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html#dataset) | Object | 事件源组件上由`data-`开头的自定义属性组成的集合 |

####  currentTarget <a id="currentTarget"></a>

事件绑定的当前组件。

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| id | String | 当前组件的id |
| [dataset](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html#dataset) | Object | 当前组件上由`data-`开头的自定义属性组成的集合 |

**说明： target 和 currentTarget 可以参考上例中，点击 inner view 时，`handleTap3` 收到的事件对象 target 和 currentTarget 都是 inner，而 `handleTap2` 收到的事件对象 target 就是 inner，currentTarget 就是 middle。**

####  dataset <a id="dataset"></a>

在组件节点中可以附加一些自定义数据。这样，在事件中可以获取这些自定义的节点数据，用于事件的逻辑处理。

在 WXML 中，这些自定义数据以 `data-` 开头，多个单词由连字符 `-` 连接。这种写法中，连字符写法会转换成驼峰写法，而大写字符会自动转成小写字符。如：

* `data-element-type` ，最终会呈现为 `event.currentTarget.dataset.elementType` ；
* `data-elementType` ，最终会呈现为 `event.currentTarget.dataset.elementtype` 。

**示例：**

```text
<view data-alpha-beta="1" data-alphaBeta="2" bindtap="bindViewTap"> DataSet Test </view>
```

```text
Page({
  bindViewTap:function(event){
    event.currentTarget.dataset.alphaBeta === 1 // - 会转为驼峰写法
    event.currentTarget.dataset.alphabeta === 2 // 大写会转为小写
  }
})
```

####  mark <a id="mark"></a>

在基础库版本 [2.7.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 以上，可以使用 `mark` 来识别具体触发事件的 target 节点。此外， `mark` 还可以用于承载一些自定义数据（类似于 `dataset` ）。

当事件触发时，事件冒泡路径上所有的 `mark` 会被合并，并返回给事件回调函数。（即使事件不是冒泡事件，也会 `mark` 。）

**代码示例：**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/boDQoKmu7M7G)

```text
<view mark:myMark="last" bindtap="bindViewTap">
  <button mark:anotherMark="leaf" bindtap="bindButtonTap">按钮</button>
</view>
```

在上述 WXML 中，如果按钮被点击，将触发 `bindViewTap` 和 `bindButtonTap` 两个事件，事件携带的 `event.mark` 将包含 `myMark` 和 `anotherMark` 两项。

```text
Page({
  bindViewTap: function(e) {
    e.mark.myMark === "last" // true
    e.mark.anotherMark === "leaf" // true
  }
})
```

`mark` 和 `dataset` 很相似，主要区别在于： `mark` 会包含从触发事件的节点到根节点上所有的 `mark:` 属性值；而 `dataset` 仅包含一个节点的 `data-` 属性值。

细节注意事项：

* 如果存在同名的 `mark` ，父节点的 `mark` 会被子节点覆盖。
* 在自定义组件中接收事件时， `mark` 不包含自定义组件外的节点的 `mark` 。
* 不同于 `dataset` ，节点的 `mark` 不会做连字符和大小写转换。

####  touches <a id="touches"></a>

touches 是一个数组，每个元素为一个 Touch 对象（canvas 触摸事件中携带的 touches 是 CanvasTouch 数组）。 表示当前停留在屏幕上的触摸点。

 **Touch 对象**

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| identifier | Number | 触摸点的标识符 |
| pageX, pageY | Number | 距离文档左上角的距离，文档的左上角为原点 ，横向为X轴，纵向为Y轴 |
| clientX, clientY | Number | 距离页面可显示区域（屏幕除去导航条）左上角距离，横向为X轴，纵向为Y轴 |

 **CanvasTouch 对象**

| 属性 | 类型 | 说明 | 特殊说明 |
| :--- | :--- | :--- | :--- |
| identifier | Number | 触摸点的标识符 |  |
| x, y | Number | 距离 Canvas 左上角的距离，Canvas 的左上角为原点 ，横向为X轴，纵向为Y轴 |  |

####  changedTouches <a id="changedTouches"></a>

changedTouches 数据格式同 touches。 表示有变化的触摸点，如从无变有（touchstart），位置变化（touchmove），从有变无（touchend、touchcancel）。

####  detail <a id="detail"></a>

自定义事件所携带的数据，如表单组件的提交事件会携带用户的输入，媒体的错误事件会携带错误信息，详见[组件](https://developers.weixin.qq.com/miniprogram/dev/component)定义中各个事件的定义。

点击事件的`detail` 带有的 x, y 同 pageX, pageY 代表距离文档左上角的距离。



### WXS响应事件 <a id="WXS&#x54CD;&#x5E94;&#x4E8B;&#x4EF6;"></a>

> 基础库 2.4.4 开始支持，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。

####  背景 <a id="&#x80CC;&#x666F;"></a>

有频繁用户交互的效果在小程序上表现是比较卡顿的，例如页面有 2 个元素 A 和 B，用户在 A 上做 touchmove 手势，要求 B 也跟随移动，[movable-view](https://developers.weixin.qq.com/miniprogram/dev/component/movable-view.html) 就是一个典型的例子。一次 touchmove 事件的响应过程为：

a、touchmove 事件从视图层（Webview）抛到逻辑层（App Service）

b、逻辑层（App Service）处理 touchmove 事件，再通过 setData 来改变 B 的位置

一次 touchmove 的响应需要经过 2 次的逻辑层和渲染层的通信以及一次渲染，通信的耗时比较大。此外 setData 渲染也会阻塞其它脚本执行，导致了整个用户交互的动画过程会有延迟。

####  实现方案 <a id="&#x5B9E;&#x73B0;&#x65B9;&#x6848;"></a>

本方案基本的思路是减少通信的次数，让事件在视图层（Webview）响应。小程序的框架分为视图层（Webview）和逻辑层（App Service），这样分层的目的是管控，开发者的代码只能运行在逻辑层（App Service），而这个思路就必须要让开发者的代码运行在视图层（Webview），如下图所示的流程：

![](https://res.wx.qq.com/wxdoc/dist/assets/img/interative-model.b746ab92.png)

使用 [WXS](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxs/) 函数用来响应小程序事件，目前只能响应内置组件的事件，不支持自定义组件事件。WXS 函数的除了纯逻辑的运算，还可以通过封装好的`ComponentDescriptor` 实例来访问以及设置组件的 class 和样式，对于交互动画，设置 style 和 class 足够了。WXS 函数的例子如下：

```text
var wxsFunction = function(event, ownerInstance) {
    var instance = ownerInstance.selectComponent('.classSelector') // 返回组件的实例
    instance.setStyle({
        "font-size": "14px" // 支持rpx
    })
    instance.getDataset()
    instance.setClass(className)
    // ...
    return false // 不往上冒泡，相当于调用了同时调用了stopPropagation和preventDefault
}
```

其中入参 `event` 是小程序[事件对象](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html)基础上多了 `event.instance` 来表示触发事件的组件的 `ComponentDescriptor` 实例。`ownerInstance` 表示的是触发事件的组件所在的组件的 `ComponentDescriptor` 实例，如果触发事件的组件是在页面内的，`ownerInstance` 表示的是页面实例。

`ComponentDescriptor`的定义如下：

| 方法 | 参数 | 描述 |
| :--- | :--- | :--- |
| selectComponent | selector对象 | 返回组件的 `ComponentDescriptor` 实例。 |
| selectAllComponents | selector对象数组 | 返回组件的 `ComponentDescriptor` 实例数组。 |
| setStyle | Object/string | 设置组件样式，支持`rpx`。设置的样式优先级比组件 wxml 里面定义的样式高。不能设置最顶层页面的样式。 |
| addClass/removeClass/ hasClass | string | 设置组件的 class。设置的 class 优先级比组件 wxml 里面定义的 class 高。不能设置最顶层页面的 class。 |
| getDataset | 无 | 返回当前组件/页面的 dataset 对象 |
| callMethod | \(funcName:string, args:object\) | 调用当前组件/页面在逻辑层（App Service）定义的函数。funcName表示函数名称，args表示函数的参数。 |
| requestAnimationFrame | Function | 和原生 `requestAnimationFrame` 一样。用于设置动画。 |
| getState | 无 | 返回一个object对象，当有局部变量需要存储起来后续使用的时候用这个方法。 |
| triggerEvent | \(eventName, detail\) | 和组件的[triggerEvent](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/events.html)一致。 |

WXS 运行在视图层（Webview），里面的逻辑毕竟能做的事件比较少，需要有一个机制和逻辑层（App Service）开发者的代码通信，上面的 `callMethod` 是 WXS 里面调用逻辑层（App Service）开发者的代码的方法，而 `WxsPropObserver` 是逻辑层（App Service）开发者的代码调用 WXS 逻辑的机制。

####  使用方法 <a id="&#x4F7F;&#x7528;&#x65B9;&#x6CD5;"></a>

* WXML定义事件：

```text
<wxs module="test" src="./test.wxs"></wxs>
<view change:prop="{{test.propObserver}}" prop="{{propValue}}" bindtouchmove="{{test.touchmove}}" class="movable"></view>
```

上面的`change:prop`（属性前面带change:前缀）是在 prop 属性被设置的时候触发 WXS 函数，值必须用`{{}}`括起来。类似 Component 定义的 properties 里面的 observer 属性，在`setData({propValue: newValue})`调用之后会触发。

**注意**：WXS函数必须用`{{}}`括起来。当 prop 的值被设置 WXS 函数就会触发，而不只是值发生改变，所以在页面初始化的时候会调用一次`WxsPropObserver`的函数。

* WXS文件`test.wxs`里面定义并导出事件处理函数和属性改变触发的函数：

```text
module.exports = {
    touchmove: function(event, instance) {
        console.log('log event', JSON.stringify(event))
    },
    propObserver: function(newValue, oldValue, ownerInstance, instance) {
        console.log('prop observer', newValue, oldValue)
    }
}
```

更多示例请查看[在开发者工具中预览效果](https://developers.weixin.qq.com/s/L1G0Dkmc7G8a)

####  Tips <a id="Tips"></a>

1. 目前还不支持[原生组件](https://developers.weixin.qq.com/miniprogram/dev/component/native-component.html)的事件、[input](https://developers.weixin.qq.com/miniprogram/dev/component/input.html)和[textarea](https://developers.weixin.qq.com/miniprogram/dev/component/textarea.html)组件的 bindinput 事件
2. 1.02.1901170及以后版本的开发者工具上支持交互动画，最低版本基础库是2.4.4
3. 目前在WXS函数里面仅支持console.log方式打日志定位问题，注意连续的重复日志会被过滤掉。

## 基础组件 <a id="&#x57FA;&#x7840;&#x7EC4;&#x4EF6;"></a>

框架为开发者提供了一系列基础组件，开发者可以通过组合这些基础组件进行快速开发。详细介绍请参考[组件文档](https://developers.weixin.qq.com/miniprogram/dev/component/)。

什么是组件：

* 组件是视图层的基本组成单元。
* 组件自带一些功能与微信风格一致的样式。
* 一个组件通常包括 `开始标签` 和 `结束标签`，`属性` 用来修饰这个组件，`内容` 在两个标签之内。

```text
<tagname property="value">
Content goes here ...
</tagname>
```

**注意：所有组件与属性都是小写，以连字符`-`连接**

####  属性类型 <a id="&#x5C5E;&#x6027;&#x7C7B;&#x578B;"></a>

| 类型 | 描述 | 注解 |
| :--- | :--- | :--- |
| Boolean | 布尔值 | 组件写上该属性，不管是什么值都被当作 `true`；只有组件上没有该属性时，属性值才为`false`。 如果属性值为变量，变量的值会被转换为Boolean类型 |
| Number | 数字 | `1`, `2.5` |
| String | 字符串 | `"string"` |
| Array | 数组 | `[ 1, "string" ]` |
| Object | 对象 | `{ key: value }` |
| EventHandler | 事件处理函数名 | `"handlerName"` 是 [Page](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html) 中定义的事件处理函数名 |
| Any | 任意属性 |  |

####  公共属性 <a id="&#x516C;&#x5171;&#x5C5E;&#x6027;"></a>

所有组件都有以下属性：

| 属性名 | 类型 | 描述 | 注解 |
| :--- | :--- | :--- | :--- |
| id | String | 组件的唯一标示 | 保持整个页面唯一 |
| class | String | 组件的样式类 | 在对应的 WXSS 中定义的样式类 |
| style | String | 组件的内联样式 | 可以动态设置的内联样式 |
| hidden | Boolean | 组件是否显示 | 所有组件默认显示 |
| data-\* | Any | 自定义属性 | 组件上触发的事件时，会发送给事件处理函数 |
| bind\* / catch\* | EventHandler | 组件的事件 | 详见[事件](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html) |

####  特殊属性 <a id="&#x7279;&#x6B8A;&#x5C5E;&#x6027;"></a>

几乎所有组件都有各自定义的属性，可以对该组件的功能或样式进行修饰，请参考各个[组件](https://developers.weixin.qq.com/miniprogram/dev/component/)的定义。

## 获取界面上的节点信息 <a id="&#x83B7;&#x53D6;&#x754C;&#x9762;&#x4E0A;&#x7684;&#x8282;&#x70B9;&#x4FE1;&#x606F;"></a>

###  WXML节点信息 <a id="WXML&#x8282;&#x70B9;&#x4FE1;&#x606F;"></a>

[节点信息查询 API](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/wx.createSelectorQuery.html) 可以用于获取节点属性、样式、在界面上的位置等信息。

最常见的用法是使用这个接口来查询某个节点的当前位置，以及界面的滚动位置。

**示例代码：**

```text
const query = wx.createSelectorQuery()
query.select('#the-id').boundingClientRect(function(res){
  res.top // #the-id 节点的上边界坐标（相对于显示区域）
})
query.selectViewport().scrollOffset(function(res){
  res.scrollTop // 显示区域的竖直滚动位置
})
query.exec()
```

上述示例中， `#the-id` 是一个节点选择器，与 CSS 的选择器相近但略有区别，请参见 [SelectorQuery.select](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/SelectorQuery.select.html) 的相关说明。

在自定义组件或包含自定义组件的页面中，推荐使用 `this.createSelectorQuery` 来代替 [wx.createSelectorQuery](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/wx.createSelectorQuery.html) ，这样可以确保在正确的范围内选择节点。

###  WXML节点布局相交状态 <a id="WXML&#x8282;&#x70B9;&#x5E03;&#x5C40;&#x76F8;&#x4EA4;&#x72B6;&#x6001;"></a>

[节点布局相交状态 API](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/wx.createIntersectionObserver.html) 可用于监听两个或多个组件节点在布局位置上的相交状态。这一组API常常可以用于推断某些节点是否可以被用户看见、有多大比例可以被用户看见。

这一组API涉及的主要概念如下。

* 参照节点：监听的参照节点，取它的布局区域作为参照区域。如果有多个参照节点，则会取它们布局区域的 **交集** 作为参照区域。页面显示区域也可作为参照区域之一。
* 目标节点：监听的目标，默认只能是一个节点（使用 `selectAll` 选项时，可以同时监听多个节点）。
* 相交区域：目标节点的布局区域与参照区域的相交区域。
* 相交比例：相交区域占参照区域的比例。
* 阈值：相交比例如果达到阈值，则会触发监听器的回调函数。阈值可以有多个。

以下示例代码可以在目标节点（用选择器 `.target-class` 指定）每次进入或离开页面显示区域时，触发回调函数。

**示例代码：**

```text
Page({
  onLoad: function(){
    wx.createIntersectionObserver().relativeToViewport().observe('.target-class', (res) => {
      res.id // 目标节点 id
      res.dataset // 目标节点 dataset
      res.intersectionRatio // 相交区域占目标节点的布局区域的比例
      res.intersectionRect // 相交区域
      res.intersectionRect.left // 相交区域的左边界坐标
      res.intersectionRect.top // 相交区域的上边界坐标
      res.intersectionRect.width // 相交区域的宽度
      res.intersectionRect.height // 相交区域的高度
    })
  }
})
```

以下示例代码可以在目标节点（用选择器 `.target-class` 指定）与参照节点（用选择器 `.relative-class` 指定）在页面显示区域内相交或相离，且相交或相离程度达到目标节点布局区域的20%和50%时，触发回调函数。

**示例代码：**

```text
Page({
  onLoad: function(){
    wx.createIntersectionObserver(this, {
      thresholds: [0.2, 0.5]
    }).relativeTo('.relative-class').relativeToViewport().observe('.target-class', (res) => {
      res.intersectionRatio // 相交区域占目标节点的布局区域的比例
      res.intersectionRect // 相交区域
      res.intersectionRect.left // 相交区域的左边界坐标
      res.intersectionRect.top // 相交区域的上边界坐标
      res.intersectionRect.width // 相交区域的宽度
      res.intersectionRect.height // 相交区域的高度
    })
  }
})
```

注意：与页面显示区域的相交区域并不准确代表用户可见的区域，因为参与计算的区域是“布局区域”，布局区域可能会在绘制时被其他节点裁剪隐藏（如遇祖先节点中 overflow 样式为 hidden 的节点）或遮盖（如遇 fixed 定位的节点）。

在自定义组件或包含自定义组件的页面中，推荐使用 `this.createIntersectionObserver` 来代替 [wx.createIntersectionObserver](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/wx.createIntersectionObserver.html) ，这样可以确保在正确的范围内选择节点。



## 响应显示区域变化 <a id="&#x54CD;&#x5E94;&#x663E;&#x793A;&#x533A;&#x57DF;&#x53D8;&#x5316;"></a>

###  显示区域尺寸 <a id="&#x663E;&#x793A;&#x533A;&#x57DF;&#x5C3A;&#x5BF8;"></a>

显示区域指小程序界面中可以自由布局展示的区域。在默认情况下，小程序显示区域的尺寸自页面初始化起就不会发生变化。但以下两种方式都可以改变这一默认行为。

####  在手机上启用屏幕旋转支持 <a id="&#x5728;&#x624B;&#x673A;&#x4E0A;&#x542F;&#x7528;&#x5C4F;&#x5E55;&#x65CB;&#x8F6C;&#x652F;&#x6301;"></a>

从小程序基础库版本 [2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始，小程序在手机上支持屏幕旋转。使小程序中的页面支持屏幕旋转的方法是：在 `app.json` 的 `window` 段中设置 `"pageOrientation": "auto"` ，或在页面 json 文件中配置 `"pageOrientation": "auto"` 。

以下是在单个页面 json 文件中启用屏幕旋转的示例。

**代码示例：**

```text
{
  "pageOrientation": "auto"
}
```

如果页面添加了上述声明，则在屏幕旋转时，这个页面将随之旋转，显示区域尺寸也会随着屏幕旋转而变化。

从小程序基础库版本 [2.5.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始， `pageOrientation` 还可以被设置为 `landscape` ，表示固定为横屏显示。

####  在 iPad 上启用屏幕旋转支持 <a id="&#x5728;-iPad-&#x4E0A;&#x542F;&#x7528;&#x5C4F;&#x5E55;&#x65CB;&#x8F6C;&#x652F;&#x6301;"></a>

从小程序基础库版本 [2.3.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始，在 iPad 上运行的小程序可以支持屏幕旋转。使小程序支持 iPad 屏幕旋转的方法是：在 `app.json` 中添加 `"resizable": true` 。

**代码示例：**

```text
{
  "resizable": true
}
```

如果小程序添加了上述声明，则在屏幕旋转时，小程序将随之旋转，显示区域尺寸也会随着屏幕旋转而变化。注意：在 iPad 上不能单独配置某个页面是否支持屏幕旋转。

###  Media Query <a id="Media-Query"></a>

有时，对于不同尺寸的显示区域，页面的布局会有所差异。此时可以使用 media query 来解决大多数问题。

**代码示例：**

```text
.my-class {
  width: 40px;
}

@media (min-width: 480px) {
  /* 仅在 480px 或更宽的屏幕上生效的样式规则 */
  .my-class {
    width: 200px;
  }
}
```

###  屏幕旋转事件 <a id="&#x5C4F;&#x5E55;&#x65CB;&#x8F6C;&#x4E8B;&#x4EF6;"></a>

有时，仅仅使用 media query 无法控制一些精细的布局变化。此时可以使用 js 作为辅助。

在 js 中读取页面的显示区域尺寸，可以使用 [selectorQuery.selectViewport](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/SelectorQuery.selectViewport.html) 。

页面尺寸发生改变的事件，可以使用页面的 `onResize` 来监听。对于自定义组件，可以使用 resize 生命周期来监听。回调函数中将返回显示区域的尺寸信息。（从基础库版本 [2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始支持。）

**代码示例：**

```text
Page({
  onResize(res) {
    res.size.windowWidth // 新的显示区域宽度
    res.size.windowHeight // 新的显示区域高度
  }
})
```

```text
Component({
  pageLifetimes: {
    resize(res) {
      res.size.windowWidth // 新的显示区域宽度
      res.size.windowHeight // 新的显示区域高度
    }
  }
})
```

此外，还可以使用 [wx.onWindowResize](https://developers.weixin.qq.com/miniprogram/dev/api/ui/window/wx.onWindowResize.html) 来监听（但这不是推荐的方式）。

**Bug & tips:**

* Bug： Android 微信版本 6.7.3 中， `live-pusher` 组件在屏幕旋转时方向异常。

## 动画 <a id="&#x52A8;&#x753B;"></a>

###  界面动画的常见方式 <a id="&#x754C;&#x9762;&#x52A8;&#x753B;&#x7684;&#x5E38;&#x89C1;&#x65B9;&#x5F0F;"></a>

在小程序中，通常可以使用 [CSS 渐变](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions) 和 [CSS 动画](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Animations/Using_CSS_animations) 来创建简易的界面动画。

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/oHKxDPm47h5k)

动画过程中，可以使用 `bindtransitionend` `bindanimationstart` `bindanimationiteration` `bindanimationend` 来监听动画事件。

| 事件名 | 含义 |
| :--- | :--- |
| transitionend | CSS 渐变结束或 [wx.createAnimation](https://developers.weixin.qq.com/miniprogram/dev/api/ui/animation/wx.createAnimation.html) 结束一个阶段 |
| animationstart | CSS 动画开始 |
| animationiteration | CSS 动画结束一个阶段 |
| animationend | CSS 动画结束 |

注意：这几个事件都不是冒泡事件，需要绑定在真正发生了动画的节点上才会生效。

同时，还可以使用 [wx.createAnimation](https://developers.weixin.qq.com/miniprogram/dev/api/ui/animation/wx.createAnimation.html) 接口来动态创建简易的动画效果。（新版小程序基础库中推荐使用下述的关键帧动画接口代替。）

###  关键帧动画 <a id="&#x5173;&#x952E;&#x5E27;&#x52A8;&#x753B;"></a>

> 基础库 2.9.0 开始支持，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。

从小程序基础库 [2.9.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始支持一种更友好的动画创建方式，用于代替旧的 [wx.createAnimation](https://developers.weixin.qq.com/miniprogram/dev/api/ui/animation/wx.createAnimation.html) 。它具有更好的性能和更可控的接口。

在页面或自定义组件中，当需要进行关键帧动画时，可以使用 `this.animate` 接口：

```text
this.animate(selector, keyframes, duration, callback)
```

**参数说明**

| 属性 | 类型 | 默认值 | 必填 | 说明 |
| :--- | :--- | :--- | :--- | :--- |
| selector | String |  | 是 | 选择器（同 [SelectorQuery.select](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/SelectorQuery.select.html) 的选择器格式） |
| keyframes | Array |  | 是 | 关键帧信息 |
| duration | Number |  | 是 | 动画持续时长（毫秒为单位） |
| callback | function |  | 否 | 动画完成后的回调函数 |

**keyframes 中对象的结构**

| 属性 | 类型 | 默认值 | 必填 | 说明 |
| :--- | :--- | :--- | :--- | :--- |
| offset | Number |  | 否 | 关键帧的偏移，范围\[0-1\] |
| ease | String | linear | 否 | 动画缓动函数 |
| transformOrigin | String | 否 | 基点位置，即 CSS transform-origin |  |
| backgroundColor | String |  | 否 | 背景颜色，即 CSS background-color |
| bottom | Number/String |  | 否 | 底边位置，即 CSS bottom |
| height | Number/String |  | 否 | 高度，即 CSS height |
| left | Number/String |  | 否 | 左边位置，即 CSS left |
| width | Number/String |  | 否 | 宽度，即 CSS width |
| opacity | Number |  | 否 | 不透明度，即 CSS opacity |
| right | Number |  | 否 | 右边位置，即 CSS right |
| top | Number/String |  | 否 | 顶边位置，即 CSS top |
| matrix | Array |  | 否 | 变换矩阵，即 CSS transform matrix |
| matrix3d | Array |  | 否 | 三维变换矩阵，即 CSS transform matrix3d |
| rotate | Number |  | 否 | 旋转，即 CSS transform rotate |
| rotate3d | Array |  | 否 | 三维旋转，即 CSS transform rotate3d |
| rotateX | Number |  | 否 | X 方向旋转，即 CSS transform rotateX |
| rotateY | Number |  | 否 | Y 方向旋转，即 CSS transform rotateY |
| rotateZ | Number |  | 否 | Z 方向旋转，即 CSS transform rotateZ |
| scale | Array |  | 否 | 缩放，即 CSS transform scale |
| scale3d | Array |  | 否 | 三维缩放，即 CSS transform scale3d |
| scaleX | Number |  | 否 | X 方向缩放，即 CSS transform scaleX |
| scaleY | Number |  | 否 | Y 方向缩放，即 CSS transform scaleY |
| scaleZ | Number |  | 否 | Z 方向缩放，即 CSS transform scaleZ |
| skew | Array |  | 否 | 倾斜，即 CSS transform skew |
| skewX | Number |  | 否 | X 方向倾斜，即 CSS transform skewX |
| skewY | Number |  | 否 | Y 方向倾斜，即 CSS transform skewY |
| translate | Array |  | 否 | 位移，即 CSS transform translate |
| translate3d | Array |  | 否 | 三维位移，即 CSS transform translate3d |
| translateX | Number |  | 否 | X 方向位移，即 CSS transform translateX |
| translateY | Number |  | 否 | Y 方向位移，即 CSS transform translateY |
| translateZ | Number |  | 否 | Z 方向位移，即 CSS transform translateZ |

###  示例代码 <a id="&#x793A;&#x4F8B;&#x4EE3;&#x7801;"></a>

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/P73kJ7mi7UcA)

```text

  this.animate('#container', [
    { opacity: 1.0, rotate: 0, backgroundColor: '#FF0000' },
    { opacity: 0.5, rotate: 45, backgroundColor: '#00FF00'},
    { opacity: 0.0, rotate: 90, backgroundColor: '#FF0000' },
    ], 5000, function () {
      this.clearAnimation('#container', { opacity: true, rotate: true }, function () {
        console.log("清除了#container上的opacity和rotate属性")
      })
  }.bind(this))

  this.animate('.block', [
    { scale: [1, 1], rotate: 0, ease: 'ease-out'  },
    { scale: [1.5, 1.5], rotate: 45, ease: 'ease-in', offset: 0.9},
    { scale: [2, 2], rotate: 90 },
  ], 5000, function () {
    this.clearAnimation('.block', function () {
      console.log("清除了.block上的所有动画属性")
    })
  }.bind(this))

```

调用 animate API 后会在节点上新增一些样式属性覆盖掉原有的对应样式。如果需要清除这些样式，可在该节点上的动画全部执行完毕后使用 `this.clearAnimation` 清除这些属性。

```text
this.clearAnimation(selector, options, callback)
```

**参数说明**

| 属性 | 类型 | 默认值 | 必填 | 说明 |
| :--- | :--- | :--- | :--- | :--- |
| selector | String |  | 是 | 选择器（同 [SelectorQuery.select](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/SelectorQuery.select.html) 的选择器格式） |
| options | Object |  | 否 | 需要清除的属性，不填写则全部清除 |
| callback | Function |  | 否 | 清除完成后的回调函数 |

###  滚动驱动的动画 <a id="&#x6EDA;&#x52A8;&#x9A71;&#x52A8;&#x7684;&#x52A8;&#x753B;"></a>

我们发现，根据滚动位置而不断改变动画的进度是一种比较常见的场景，这类动画可以让人感觉到界面交互很连贯自然，体验更好。因此，从小程序基础库 [2.9.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始支持一种由滚动驱动的动画机制。

基于上述的关键帧动画接口，新增一个 `ScrollTimeline` 的参数，用来绑定滚动元素（目前只支持 scroll-view）。接口定义如下：

```text
this.animate(selector, keyframes, duration, ScrollTimeline)
```

**ScrollTimeline 中对象的结构**

| 属性 | 类型 | 默认值 | 必填 | 说明 |
| :--- | :--- | :--- | :--- | :--- |
| scrollSource | String |  | 是 | 指定滚动元素的选择器（只支持 scroll-view），该元素滚动时会驱动动画的进度 |
| orientation | String | vertical | 否 | 指定滚动的方向。有效值为 horizontal 或 vertical |
| startScrollOffset | Number |  | 是 | 指定开始驱动动画进度的滚动偏移量，单位 px |
| endScrollOffset | Number |  | 是 | 指定停止驱动动画进度的滚动偏移量，单位 px |
| timeRange | Number |  | 是 | 起始和结束的滚动范围映射的时间长度，该时间可用于与关键帧动画里的时间 \(duration\) 相匹配，单位 ms |

###  示例代码 <a id="&#x793A;&#x4F8B;&#x4EE3;&#x7801;-2"></a>

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/994o8jmY7FcQ)

```text
  this.animate('.avatar', [{
    borderRadius: '0',
    borderColor: 'red',
    transform: 'scale(1) translateY(-20px)',
    offset: 0,
  }, {
    borderRadius: '25%',
    borderColor: 'blue',
    transform: 'scale(.65) translateY(-20px)',
    offset: .5,
  }, {
    borderRadius: '50%',
    borderColor: 'blue',
    transform: `scale(.3) translateY(-20px)`,
    offset: 1
  }], 2000, {
    scrollSource: '#scroller',
    timeRange: 2000,
    startScrollOffset: 0,
    endScrollOffset: 85,
  })

  this.animate('.search_input', [{
    opacity: '0',
    width: '0%',
  }, {
    opacity: '1',
    width: '100%',
  }], 1000, {
    scrollSource: '#scroller',
    timeRange: 1000,
    startScrollOffset: 120,
    endScrollOffset: 252
  })
```

###  高级的动画方式 <a id="&#x9AD8;&#x7EA7;&#x7684;&#x52A8;&#x753B;&#x65B9;&#x5F0F;"></a>

在一些复杂场景下，上述的动画方法可能并不适用。

[WXS 响应事件](https://developers.weixin.qq.com/miniprogram/dev/framework/view/interactive-animation.html) 的方式可以通过使用 WXS 来响应事件的方法来动态调整节点的 style 属性。通过不断改变 style 属性的值可以做到动画效果。同时，这种方式也可以根据用户的触摸事件来动态地生成动画。

连续使用 setData 来改变界面的方法也可以达到动画的效果。这样可以任意地改变界面，但通常会产生较大的延迟或卡顿，甚至导致小程序僵死。此时可以通过将页面的 setData 改为 [自定义组件](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/) 中的 setData 来提升性能。下面的例子是使用 setData 来实现秒表动画的示例。

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/cRTvdPmO7d5T)



