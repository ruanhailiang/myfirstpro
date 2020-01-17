# 存储

### 存储 <a id="&#x5B58;&#x50A8;"></a>

每个微信小程序都可以有自己的本地缓存，可以通过 [wx.setStorage](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.setStorage.html)/[wx.setStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.setStorageSync.html)、[wx.getStorage](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.getStorage.html)/[wx.getStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.getStorageSync.html)、[wx.clearStorage](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.clearStorage.html)/[wx.clearStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.clearStorageSync.html)，[wx.removeStorage](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.removeStorage.html)/[wx.removeStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.removeStorageSync.html) 对本地缓存进行读写和清理。

####  隔离策略 <a id="&#x9694;&#x79BB;&#x7B56;&#x7565;"></a>

同一个微信用户，同一个小程序 storage 上限为 10MB。storage 以用户维度隔离，同一台设备上，A 用户无法读取到 B 用户的数据；不同小程序之间也无法互相读写数据。

####  清理策略 <a id="&#x6E05;&#x7406;&#x7B56;&#x7565;"></a>

本地缓存的清理时机跟代码包一样，只有在代码包被清理的时候本地缓存才会被清理。

