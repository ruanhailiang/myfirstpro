# 周期性更新

### 周期性更新 <a id="&#x5468;&#x671F;&#x6027;&#x66F4;&#x65B0;"></a>

> 基础库 2.8.0 开始支持，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。

> 生效条件：用户七天内使用过的小程序

周期性更新能够在用户未打开小程序的情况下，也能从服务器提前拉取数据，当用户打开小程序时可以更快地渲染页面，减少用户等待时间，增强在弱网条件下的可用性。

####  使用流程 <a id="&#x4F7F;&#x7528;&#x6D41;&#x7A0B;"></a>

 **1. 配置数据下载地址**

登录小程序 MP 管理后台，进入设置 -&gt; 开发设置 -&gt; 数据周期性更新，点击开启，填写数据下载地址。 ![](https://res.wx.qq.com/wxdoc/dist/assets/img/background-fetch-1.1f8624b7.png)

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

 **3. 微信客户端定期拉取数据**

微信客户端会在一定的网络条件下，每隔 12 小时（以上一次成功更新的时间为准）向配置的数据下载地址发起一个 HTTP GET 请求，其中包含的 query 参数如下，数据获取到后会将整个 HTTP body 缓存到本地。

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| appid | String | 小程序标识 |
| token | String | 前面设置的 TOKEN |
| timestamp | Number | 时间戳，微信客户端发起请求的时间 |

> query 参数会使用 urlencode 处理

> 开发者服务器接口返回的数据类型应为字符串，且大小应不超过 `256KB`，否则将无法缓存数据

 **4. 读取数据**

用户启动小程序时，调用 [wx.getBackgroundFetchData\(\)](https://developers.weixin.qq.com/miniprogram/dev/api/storage/background-fetch/wx.getBackgroundFetchData.html) 获取已缓存到本地的数据。

示例：

```text
App({
  onLaunch() {
    wx.getBackgroundFetchData({
      fetchType: 'periodic',
      success(res) {
        console.log(res.fetchedData) // 缓存数据
        console.log(res.timeStamp) // 客户端拿到缓存数据的时间戳
      }
    })
  }
})
```

####  调试方法 <a id="&#x8C03;&#x8BD5;&#x65B9;&#x6CD5;"></a>

由于微信客户端每隔 12 个小时才会发起一次请求，调试周期性更新功能会显得不太方便。 因此为了方便调试周期性数据，工具提供了下面的调试能力给到开发者，具体可查看[周期性数据调试](https://developers.weixin.qq.com/miniprogram/dev/devtools/periodic-data.html)。

