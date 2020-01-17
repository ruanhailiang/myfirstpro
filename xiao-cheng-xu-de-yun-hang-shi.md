# 小程序的运行时

## 小程序的运行环境 <a id="&#x5C0F;&#x7A0B;&#x5E8F;&#x7684;&#x8FD0;&#x884C;&#x73AF;&#x5883;"></a>

微信小程序运行在多种平台上：iOS（iPhone/iPad）微信客户端、Android 微信客户端、PC 微信客户端、Mac 微信客户端和用于调试的微信开发者工具。

各平台脚本执行环境以及用于渲染非原生组件的环境是各不相同的：

* 在 iOS 上，小程序逻辑层的 javascript 代码运行在 JavaScriptCore 中，视图层是由 WKWebView 来渲染的，环境有 iOS 12、iOS 13 等；
* 在 Android 上，小程序逻辑层的 javascript 代码运行在 [V8](https://developers.google.com/v8/) 中，视图层是由自研 XWeb 引擎基于 Mobile Chrome 内核来渲染的；
* 在 开发工具上，小程序逻辑层的 javascript 代码是运行在 [NW.js](https://nwjs.io/) 中，视图层是由 Chromium Webview 来渲染的。

####  平台差异 <a id="&#x5E73;&#x53F0;&#x5DEE;&#x5F02;"></a>

尽管各运行环境是十分相似的，但是还是有些许区别：

* `JavaScript` 语法和 API 支持不一致：语法上开发者可以通过开启 `ES6` 转 `ES5` 的功能来规避（[详情](https://developers.weixin.qq.com/miniprogram/dev/devtools/codecompile.html#es6-%E8%BD%AC-es5)）；此外，小程序基础库内置了必要的Polyfill，来弥补API的差异（[详情](https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/js-support.html)\)。
* `WXSS` 渲染表现不一致：尽管可以通过开启[样式补全](https://developers.weixin.qq.com/miniprogram/dev/devtools/codecompile.html#%E6%A0%B7%E5%BC%8F%E8%A1%A5%E5%85%A8)来规避大部分的问题，还是建议开发者需要在 iOS 和 Android 上分别检查小程序的真实表现。

**开发者工具仅供调试使用，最终的表现以客户端为准。**

## JavaScript 支持情况 <a id="JavaScript-&#x652F;&#x6301;&#x60C5;&#x51B5;"></a>

###  运行限制 <a id="&#x8FD0;&#x884C;&#x9650;&#x5236;"></a>

基于安全考虑，小程序中不支持动态执行 JS 代码，即：

* 不支持使用 `eval` 执行 JS 代码
* 不支持使用 `new Function` 创建函数

###  客户端 ES6 API 支持情况 <a id="&#x5BA2;&#x6237;&#x7AEF;-ES6-API-&#x652F;&#x6301;&#x60C5;&#x51B5;"></a>

微信小程序已经支持了绝大部分的 ES6 API，已支持的 API 如下（部分API依赖系统版本）：

| String | iOS8 | iOS9 | iOS10+ | Android |
| :--- | :--- | :--- | :--- | :--- |
| codePointAt |  |  |  |  |
| normalize | ✘ | ✘ |  |  |
| includes |  |  |  |  |
| startsWith |  |  |  |  |
| endsWith |  |  |  |  |
| repeat |  |  |  |  |
| String.fromCodePoint |  |  |  |  |

| Array | iOS8 | iOS9 | iOS10+ | Android |
| :--- | :--- | :--- | :--- | :--- |
| copyWithin |  |  |  |  |
| find |  |  |  |  |
| findIndex |  |  |  |  |
| fill |  |  |  |  |
| entries |  |  |  |  |
| keys |  |  |  |  |
| values | ✘ |  |  | ✘ |
| includes | ✘ |  |  |  |
| Array.from |  |  |  |  |
| Array.of |  |  |  |  |

| Number | iOS8 | iOS9 | iOS10+ | Android |
| :--- | :--- | :--- | :--- | :--- |
| isFinite |  |  |  |  |
| isNaN |  |  |  |  |
| parseInt |  |  |  |  |
| parseFloat |  |  |  |  |
| isInteger |  |  |  |  |
| EPSILON |  |  |  |  |
| isSafeInteger |  |  |  |  |

| Math | iOS8 | iOS9 | iOS10+ | Android |
| :--- | :--- | :--- | :--- | :--- |
| trunc |  |  |  |  |
| sign |  |  |  |  |
| cbrt |  |  |  |  |
| clz32 |  |  |  |  |
| imul |  |  |  |  |
| fround |  |  |  |  |
| hypot |  |  |  |  |
| expm1 |  |  |  |  |
| log1p |  |  |  |  |
| log10 |  |  |  |  |
| log2 |  |  |  |  |
| sinh |  |  |  |  |
| cosh |  |  |  |  |
| tanh |  |  |  |  |
| asinh |  |  |  |  |
| acosh |  |  |  |  |
| atanh |  |  |  |  |

| Object | iOS8 | iOS9 | iOS10+ | Android |
| :--- | :--- | :--- | :--- | :--- |
| is |  |  |  |  |
| assign |  |  |  |  |
| getOwnPropertyDescriptor |  |  |  |  |
| keys |  |  |  |  |
| getOwnPropertyNames |  |  |  |  |
| getOwnPropertySymbols |  |  |  |  |

| Other | iOS8 | iOS9 | iOS10+ | Android |
| :--- | :--- | :--- | :--- | :--- |
| Symbol |  |  |  |  |
| Set |  |  |  |  |
| Map |  |  |  |  |
| Proxy | ✘ | ✘ |  | ✘ |
| Reflect |  |  |  |  |
| Promise |  |  |  |  |



### 小程序运行机制 <a id="&#x5C0F;&#x7A0B;&#x5E8F;&#x8FD0;&#x884C;&#x673A;&#x5236;"></a>

####  前台/后台状态 <a id="&#x524D;&#x53F0;-&#x540E;&#x53F0;&#x72B6;&#x6001;"></a>

小程序启动后，界面被展示给用户，此时小程序处于**前台**状态。

当用户点击右上角胶囊按钮关闭小程序，或者按了设备 Home 键离开微信时，小程序并没有完全终止运行，而是进入了**后台**状态，小程序还可以运行一小段时间。

当用户再次进入微信或再次打开小程序，小程序又会从后台进入**前台**。但如果用户很久没有再进入小程序，或者系统资源紧张，小程序可能被**销毁**，即完全终止运行。

####  小程序启动 <a id="&#x5C0F;&#x7A0B;&#x5E8F;&#x542F;&#x52A8;"></a>

这样，小程序启动可以分为两种情况，一种是**冷启动**，一种是**热启动**。

* 冷启动：如果用户首次打开，或小程序销毁后被用户再次打开，此时小程序需要重新加载启动，即冷启动。
* 热启动：如果用户已经打开过某小程序，然后在一定时间内再次打开该小程序，此时小程序并未被销毁，只是从后台状态进入前台状态，这个过程就是热启动。

####  小程序销毁时机 <a id="&#x5C0F;&#x7A0B;&#x5E8F;&#x9500;&#x6BC1;&#x65F6;&#x673A;"></a>

通常，只有当小程序进入后台一定时间，或者系统资源占用过高，才会被销毁。具体而言包括以下几种情形：

* 当小程序进入后台，可以会维持一小段时间的运行状态，如果这段时间内都未进入前台，小程序会被销毁。
* 当小程序占用系统资源过高，可能会被系统销毁或被微信客户端主动回收。
  * 在 iOS 上，当微信客户端在一定时间间隔内连续收到系统内存告警时，会根据一定的策略，主动销毁小程序，并提示用户 「运行内存不足，请重新打开该小程序」。具体策略会持续进行调整优化。
  * 建议小程序在必要时使用 [wx.onMemoryWarning](https://developers.weixin.qq.com/miniprogram/dev/api/device/performance/wx.onMemoryWarning.html) 监听内存告警事件，进行必要的内存清理。

> 基础库 1.1.0 及以上，1.4.0 以下版本： 当用户从扫一扫、转发等入口（[场景值](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/scene.html)为1007, 1008, 1011, 1025）进入小程序，且没有置顶小程序的情况下退出，小程序会被销毁。

####  启动场景分类 <a id="&#x542F;&#x52A8;&#x573A;&#x666F;&#x5206;&#x7C7B;"></a>

用户打开小程序时，场景可分为以下 A、B 两类：

A. 保留上次的浏览状态。[场景值](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/scene.html)有以下几项：

| 场景值ID | 说明 |
| :--- | :--- |
| 1001 | 发现栏小程序主入口，「最近使用」列表（基础库2.2.4版本起包含「我的小程序」列表） |
| 1003 | 星标小程序列表 |
| 1023 | 系统桌面小图标打开小程序 |
| 1038 | 从其他小程序返回小程序 |
| 1056 | 聊天顶部音乐播放器右上角菜单，打开小程序 |
| 1080 | 客服会话菜单小程序入口，打开小程序 |
| 1083 | 公众号会话菜单小程序入口 ，打开小程序（只有腾讯客服小程序有） |
| 1089 | 聊天主界面下拉，打开小程序/微信聊天主界面下拉，「最近使用」栏（基础库2.2.4版本起包含「我的小程序」栏） |
| 1090 | 长按小程序右上角菜单，打开小程序 |
| 1103 | 发现-小程序主入口我的小程序，打开小程序 |
| 1104 | 聊天主界面下拉，从我的小程序，打开小程序 |
| 1113 | 安卓手机负一屏，打开小程序 |
| 1114 | 安卓手机侧边栏，打开小程序 |
| 1117 | 后台运行小程序的管理页中，打开小程序 |

* 若进入的场景中带有 path，则每次打开小程序时都进入对应的 path 页面
* 若进入的场景中不带 path：
  1. 若小程序是热启动，则保留原来状态
  2. 若小程序是冷启动，则遵循下一节的重启策略，可能是首页或上次退出的页面

B. relaunch 到指定页或首页

包括除 A 类外的其他场景

* 若进入的场景中带有 path，则每次点击时都进入对应的 path 页面
* 若进入的场景中不带 path，则每次进入都打开首页

####  A 类场景的重新启动策略 <a id="A-&#x7C7B;&#x573A;&#x666F;&#x7684;&#x91CD;&#x65B0;&#x542F;&#x52A8;&#x7B56;&#x7565;"></a>

> 基础库 2.8.0 开始支持，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。

小程序被销毁后，下次冷启动如果属于 B 类场景，将会进入特定的页面。

下次冷启动如果属于 A 类场景，默认情况下将会进入小程序的首页。在页面对应的 json 文件中（也可以全局配置在 app.json 的 window 段中），指定 `restartStrategy` 配置项可以改变这个默认的行为，使得从某个页面退出后，下次 A 类场景的冷启动可以回到这个页面。

**代码示例：**

```text
{
  "restartStrategy": "homePage"
}
```

`restartStrategy` 可选值：

| 可选值 | 含义 |
| :--- | :--- |
| homePage | （默认值）如果从这个页面退出小程序，下次将从首页冷启动 |
| homePageAndLatestPage | 如果从这个页面退出小程序，下次冷启动后立刻加载这个页面，页面的参数保持不变（不可用于 tab 页） |

注意：即使不配置为 `homePage` ，小程序如果退出过久（当前默认一天时间，可以使用**退出状态**来调整），下次冷启动时也将不再遵循 `restartStrategy` 的配置，而是直接从首页冷启动。

无论如何，页面中的状态并不会被保留，如输入框中的文本内容、 checkbox 的勾选状态等都不会还原。如果需要还原或部分还原，需要利用**退出状态**。

####  退出状态 <a id="&#x9000;&#x51FA;&#x72B6;&#x6001;"></a>

每当小程序可能被销毁之前，页面回调函数 `onSaveExitState` 会被调用。如果想保留页面中的状态，可以在这个回调函数中“保存”一些数据，下次启动时可以通过 `exitState` 获得这些已保存数据。

**代码示例：**

```text
{
  "restartStrategy": "homePageAndLatestPage"
}
```

```text
Page({
  onLoad: function() {
    var prevExitState = this.exitState // 尝试获得上一次退出前 onSaveExitState 保存的数据
    if (prevExitState !== undefined) { // 如果是根据 restartStrategy 配置进行的冷启动，就可以获取到
      prevExitState.myDataField === 'myData' 
    }
  },
  onSaveExitState: function() {
    var exitState = { myDataField: 'myData' } // 需要保存的数据
    return {
      data: exitState,
      expireTimeStamp: Date.now() + 24 * 60 * 60 * 1000 // 超时时刻
    }
  }
})
```

`onSaveExitState` 返回值可以包含两项：

| 字段名 | 类型 | 含义 |
| :--- | :--- | :--- |
| data | Any | 需要保存的数据（只能是 JSON 兼容的数据） |
| expireTimeStamp | Number | 超时时刻，在这个时刻后，保存的数据保证一定被丢弃，默认为 \(当前时刻 + 1 天\) |

一个更完整的示例：[在开发者工具中预览效果](https://developers.weixin.qq.com/s/ELP5uTmN7E8l)

注意事项：

* 如果超过 `expireTimeStamp` ，保存的数据将被丢弃，且冷启动时不遵循 `restartStrategy` 的配置，而是直接从首页冷启动。
* `expireTimeStamp` 有可能被自动提前，如微信客户端需要清理数据的时候。
* 在小程序存活期间， `onSaveExitState` 可能会被多次调用，此时以最后一次的调用结果作为最终结果。
* 在某些特殊情况下（如微信客户端直接被系统杀死），这个方法将不会被调用，下次冷启动也不遵循 `restartStrategy` 的配置，而是直接从首页冷启动。



### 小程序更新机制 <a id="&#x5C0F;&#x7A0B;&#x5E8F;&#x66F4;&#x65B0;&#x673A;&#x5236;"></a>

####  未启动时更新 <a id="&#x672A;&#x542F;&#x52A8;&#x65F6;&#x66F4;&#x65B0;"></a>

开发者在管理后台发布新版本的小程序之后，如果某个用户本地有小程序的历史版本，此时打开的可能还是旧版本。微信客户端会有若干个时机去检查本地缓存的小程序有没有更新版本，如果有则会静默更新到新版本。总的来说，开发者在后台发布新版本之后，无法立刻影响到所有现网用户，但最差情况下，也在发布之后 24 小时之内下发新版本信息到用户。用户下次打开时会先更新最新版本再打开。

####  启动时更新 <a id="&#x542F;&#x52A8;&#x65F6;&#x66F4;&#x65B0;"></a>

小程序每次**冷启动**时，都会检查是否有更新版本，如果发现有新版本，将会异步下载新版本的代码包，并同时用客户端本地的包进行启动，即新版本的小程序需要等下一次冷启动才会应用上。

如果需要马上应用最新版本，可以使用 [wx.getUpdateManager](https://developers.weixin.qq.com/miniprogram/dev/api/base/update/wx.getUpdateManager.html) API 进行处理。

```text
const updateManager = wx.getUpdateManager()

updateManager.onCheckForUpdate(function (res) {
  // 请求完新版本信息的回调
  console.log(res.hasUpdate)
})

updateManager.onUpdateReady(function () {
  wx.showModal({
    title: '更新提示',
    content: '新版本已经准备好，是否重启应用？',
    success(res) {
      if (res.confirm) {
        // 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
        updateManager.applyUpdate()
      }
    }
  })
})

updateManager.onUpdateFailed(function () {
  // 新版本下载失败
})
```

