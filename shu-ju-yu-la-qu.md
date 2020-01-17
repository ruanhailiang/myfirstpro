# 数据预拉取

### 数据预拉取 <a id="&#x6570;&#x636E;&#x9884;&#x62C9;&#x53D6;"></a>

预拉取能够在小程序冷启动的时候通过微信后台提前向第三方服务器拉取业务数据，当代码包加载完时可以更快地渲染页面，减少用户等待时间，从而提升小程序的打开速度 。

####  使用流程 <a id="&#x4F7F;&#x7528;&#x6D41;&#x7A0B;"></a>

 **1. 配置数据下载地址**

登录小程序 MP 管理后台，进入设置 -&gt; 开发设置 -&gt; 数据预加载，点击开启，填写数据下载地址，只支持 HTTPS 。 ![](https://res.wx.qq.com/wxdoc/dist/assets/img/pre-fetch.1ed75aeb.png)

 **2. 设置 TOKEN**

第一次启动小程序时，调用 [wx.setBackgroundFetchToken\(\)](https://developers.weixin.qq.com/miniprogram/dev/api/storage/background-fetch/wx.setBackgroundFetchToken.html) 设置一个 TOKEN 字符串，可以跟用户态相关，会在后续微信客户端向开发者服务器请求时带上，便于给后者校验请求合法性。

示例：

```text
App({
  onLaunch() {
    wx.setBackgroundFetchToken({
      token: 'xxx'
    })
  }
})
```

 **3. 微信客户端提前拉取数据**

当用户打开小程序时，微信服务器将向开发者服务器（上面配置的数据下载地址）发起一个 HTTP GET 请求，其中包含的 query 参数如下，数据获取到后会将整个 HTTP body 缓存到本地。

| 参数 | 类型 | 必填 | 说明 |
| :--- | :--- | :--- | :--- |
| appid | String | 是 | 小程序标识。 |
| token | String | 否 | 前面设置的 TOKEN。 |
| code | String | 否 | 用户登录凭证，未设置TOKEN时由微信侧预生成，可在开发者后台调用 auth.code2Session，换取 openid 等信息。 |
| timestamp | Number | 是 | 时间戳，微信客户端发起请求的时间 |
| path | String | 否 | 打开小程序的路径。 |
| query | String | 否 | 打开小程序的query。 |
| scene | Number | 否 | 打开小程序的场景值。 |

> query 参数会使用 urlencode 处理

> token和code只会存在一个，用于标识用户身份。

> 开发者服务器接口返回的数据类型应为字符串，且大小应不超过 `256KB`，否则将无法缓存数据

 **4. 读取数据**

用户启动小程序时，调用 [wx.getBackgroundFetchData\(\)](https://developers.weixin.qq.com/miniprogram/dev/api/storage/background-fetch/wx.getBackgroundFetchData.html) 获取已缓存到本地的数据。

示例：

```text
App({
  onLaunch() {
    wx.getBackgroundFetchData({
      fetchType: 'pre',
      success(res) {
        console.log(res.fetchedData) // 缓存数据
        console.log(res.timeStamp) // 客户端拿到缓存数据的时间戳
        console.log(res.path) // 页面路径
        console.log(res.query) // query 参数
        console.log(res.scene) // 场景值
      }
    })
  }
})
```

####  调试方法 <a id="&#x8C03;&#x8BD5;&#x65B9;&#x6CD5;"></a>

为了方便调试数据预拉取，工具提供了下面的调试能力给到开发者，具体可查看[预拉取数据调试](https://developers.weixin.qq.com/miniprogram/dev/devtools/prefetch-data.html)。

