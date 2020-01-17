# 硬件能力

### 蓝牙 <a id="&#x84DD;&#x7259;"></a>

> iOS 微信客户端 6.5.6 版本开始支持，Android 6.5.7 版本开始支持

蓝牙适配器模块生效周期为调用 [wx.openBluetoothAdapter](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.openBluetoothAdapter.html) 至调用 [wx.closeBluetoothAdapter](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.closeBluetoothAdapter.html) 或小程序被销毁为止。

在小程序蓝牙适配器模块生效期间，开发者才能够正常调用蓝牙相关的小程序 API，并收到蓝牙模块相关的事件回调。

 **注意**

* 由于系统限制，Android 上获取到的 `deviceId` 为设备 MAC 地址，iOS 上则为设备 uuid。因此 `deviceId` 不能硬编码到代码中。
* 目前不支持在开发者工具上进行蓝牙功能的调试，需要使用真机才能正常调用小程序蓝牙接口。

 **低功耗蓝牙（BLE）注意事项**

* iOS 上对特征值的 `read`、`write`、`notify`操作，由于系统需要获取特征值实例，传入的 `serviceId` 与 `characteristicId` 必须由 [wx.getBLEDeviceServices](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.getBLEDeviceServices.html) 与 [wx.getBLEDeviceCharacteristics](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.getBLEDeviceCharacteristics.html) 中获取到后才能使用。建议双平台统一在建立连接后先执行 [wx.getBLEDeviceServices](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.getBLEDeviceServices.html) 与 [wx.getBLEDeviceCharacteristics](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.getBLEDeviceCharacteristics.html) 后再进行与蓝牙设备的数据交互。

 **示例代码**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/OF4Y9Gme6rZ4)

### NFC <a id="NFC"></a>

暂仅支持 HCE（基于主机的卡模拟）模式，即将安卓手机模拟成实体智能卡。

* 适用机型：支持 NFC 功能，且系统版本为 Android 5.0 及以上的手机
* 适用卡范围：符合ISO 14443-4 标准的 CPU 卡



### Wi-Fi <a id="Wi-Fi"></a>

在小程序中支持搜索周边的 Wi-Fi，同时可以针对指定 Wi-Fi，传入密码发起连接。

该系列接口为系统原生能力，如需查看「微信连Wi-Fi」能力及配置跳转小程序，请参考[文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=215135855720FBA0)。

连接指定 Wi-Fi 接口调用时序：

* Android：startWifi —&gt; connectWifi —&gt; onWifiConnected
* iOS（仅iOS 11及以上版本支持）：startWifi —&gt; connectWifi —&gt; onWifiConnected

连周边 Wi-Fi 接口调用时序：

* Android：startWifi —&gt; getWifiList —&gt; onGetWifiList —&gt; connectWifi —&gt; onWifiConnected
* iOS（iOS 11.0及11.1版本因系统原因暂不支持）：startWifi —&gt; getWifiList —&gt; onGetWifiList —&gt; setWifiList —&gt; onWifiConnected

**注意：**

* Wi-Fi 相关接口暂不可用 [wx.canIUse](https://developers.weixin.qq.com/miniprogram/dev/api/base/wx.canIUse.html) 接口判断。
* Android 6.0 以上版本，在没有打开定位开关的时候会导致设备不能正常获取周边的 Wi-Fi 信息。

