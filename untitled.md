# 插件调用 API 的限制

## 插件调用 API 的限制 <a id="&#x63D2;&#x4EF6;&#x8C03;&#x7528;-API-&#x7684;&#x9650;&#x5236;"></a>

插件可以调用的 API 与小程序不同，主要有两个区别：

* 插件的请求域名列表与小程序相互独立；
* 一些 API 不允许插件调用（这些函数不存在于 `wx` 对象下）。

有些接口虽然在插件中不能使用，但可以通过插件功能页来达到目的，请参考[插件功能页](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/functional-pages.html)。

目前，允许插件调用的 API 及其对应版本要求如下：

####  基础 <a id="&#x57FA;&#x7840;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.arrayBufferToBase64](https://developers.weixin.qq.com/miniprogram/dev/api/base/wx.arrayBufferToBase64.html) |  |  |
| [wx.base64ToArrayBuffer](https://developers.weixin.qq.com/miniprogram/dev/api/base/wx.base64ToArrayBuffer.html) |  |  |

####  发起请求 <a id="&#x53D1;&#x8D77;&#x8BF7;&#x6C42;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.request](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  上传、下载 <a id="&#x4E0A;&#x4F20;&#x3001;&#x4E0B;&#x8F7D;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.downloadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network/download/wx.downloadFile.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.uploadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network/upload/wx.uploadFile.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  WebSocket <a id="WebSocket"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.connectSocket](https://developers.weixin.qq.com/miniprogram/dev/api/network/websocket/wx.connectSocket.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  图片 <a id="&#x56FE;&#x7247;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.previewImage](https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.previewImage.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.chooseImage](https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.chooseImage.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getImageInfo](https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.getImageInfo.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.saveImageToPhotosAlbum](https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.saveImageToPhotosAlbum.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  录音 <a id="&#x5F55;&#x97F3;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.startRecord](https://developers.weixin.qq.com/miniprogram/dev/api/media/recorder/wx.startRecord.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.stopRecord](https://developers.weixin.qq.com/miniprogram/dev/api/media/recorder/wx.stopRecord.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  实时音视频 <a id="&#x5B9E;&#x65F6;&#x97F3;&#x89C6;&#x9891;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.createLivePlayerContext](https://developers.weixin.qq.com/miniprogram/dev/api/media/live/wx.createLivePlayerContext.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.createLivePusherContext](https://developers.weixin.qq.com/miniprogram/dev/api/media/live/wx.createLivePusherContext.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  录音管理 <a id="&#x5F55;&#x97F3;&#x7BA1;&#x7406;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.getRecorderManager](https://developers.weixin.qq.com/miniprogram/dev/api/media/recorder/wx.getRecorderManager.html) | [1.9.94](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  音频播放控制 <a id="&#x97F3;&#x9891;&#x64AD;&#x653E;&#x63A7;&#x5236;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.pauseVoice](https://developers.weixin.qq.com/miniprogram/dev/api/media/audio/wx.pauseVoice.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.playVoice](https://developers.weixin.qq.com/miniprogram/dev/api/media/audio/wx.playVoice.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.stopVoice](https://developers.weixin.qq.com/miniprogram/dev/api/media/audio/wx.stopVoice.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  音乐播放控制 <a id="&#x97F3;&#x4E50;&#x64AD;&#x653E;&#x63A7;&#x5236;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.onBackgroundAudioPlay](https://developers.weixin.qq.com/miniprogram/dev/api/media/background-audio/wx.onBackgroundAudioPlay.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getBackgroundAudioPlayerState](https://developers.weixin.qq.com/miniprogram/dev/api/media/background-audio/wx.getBackgroundAudioPlayerState.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onBackgroundAudioStop](https://developers.weixin.qq.com/miniprogram/dev/api/media/background-audio/wx.onBackgroundAudioStop.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.stopBackgroundAudio](https://developers.weixin.qq.com/miniprogram/dev/api/media/background-audio/wx.stopBackgroundAudio.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onBackgroundAudioPause](https://developers.weixin.qq.com/miniprogram/dev/api/media/background-audio/wx.onBackgroundAudioPause.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.seekBackgroundAudio](https://developers.weixin.qq.com/miniprogram/dev/api/media/background-audio/wx.seekBackgroundAudio.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.playBackgroundAudio](https://developers.weixin.qq.com/miniprogram/dev/api/media/background-audio/wx.playBackgroundAudio.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.pauseBackgroundAudio](https://developers.weixin.qq.com/miniprogram/dev/api/media/background-audio/wx.pauseBackgroundAudio.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  背景音频播放管理 <a id="&#x80CC;&#x666F;&#x97F3;&#x9891;&#x64AD;&#x653E;&#x7BA1;&#x7406;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.getBackgroundAudioManager](https://developers.weixin.qq.com/miniprogram/dev/api/media/background-audio/wx.getBackgroundAudioManager.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  音频组件控制 <a id="&#x97F3;&#x9891;&#x7EC4;&#x4EF6;&#x63A7;&#x5236;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.createInnerAudioContext](https://developers.weixin.qq.com/miniprogram/dev/api/media/audio/wx.createInnerAudioContext.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.createAudioContext](https://developers.weixin.qq.com/miniprogram/dev/api/media/audio/wx.createAudioContext.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  视频 <a id="&#x89C6;&#x9891;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.chooseVideo](https://developers.weixin.qq.com/miniprogram/dev/api/media/video/wx.chooseVideo.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.saveVideoToPhotosAlbum](https://developers.weixin.qq.com/miniprogram/dev/api/media/video/wx.saveVideoToPhotosAlbum.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  视频组件控制 <a id="&#x89C6;&#x9891;&#x7EC4;&#x4EF6;&#x63A7;&#x5236;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.createVideoContext](https://developers.weixin.qq.com/miniprogram/dev/api/media/video/wx.createVideoContext.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  相机组件控制 <a id="&#x76F8;&#x673A;&#x7EC4;&#x4EF6;&#x63A7;&#x5236;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.createCameraContext](https://developers.weixin.qq.com/miniprogram/dev/api/media/camera/wx.createCameraContext.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  数据缓存 <a id="&#x6570;&#x636E;&#x7F13;&#x5B58;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.setStorage](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.setStorage.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getStorage](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.getStorage.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.removeStorage](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.removeStorage.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.setStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.setStorageSync.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.getStorageSync.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.removeStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.removeStorageSync.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  获取位置 <a id="&#x83B7;&#x53D6;&#x4F4D;&#x7F6E;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.getLocation](https://developers.weixin.qq.com/miniprogram/dev/api/location/wx.getLocation.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.chooseLocation](https://developers.weixin.qq.com/miniprogram/dev/api/location/wx.chooseLocation.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onLocationChange](https://developers.weixin.qq.com/miniprogram/dev/api/location/wx.onLocationChange.html) | [2.8.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offLocationChange](https://developers.weixin.qq.com/miniprogram/dev/api/location/wx.offLocationChange.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.stopLocationUpdate](https://developers.weixin.qq.com/miniprogram/dev/api/location/wx.stopLocationUpdate.html) | [2.8.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.startLocationUpdate](https://developers.weixin.qq.com/miniprogram/dev/api/location/wx.startLocationUpdate.html) | [2.8.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  查看位置 <a id="&#x67E5;&#x770B;&#x4F4D;&#x7F6E;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.openLocation](https://developers.weixin.qq.com/miniprogram/dev/api/location/wx.openLocation.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  地图组件控制 <a id="&#x5730;&#x56FE;&#x7EC4;&#x4EF6;&#x63A7;&#x5236;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.createMapContext](https://developers.weixin.qq.com/miniprogram/dev/api/media/map/wx.createMapContext.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  系统信息 <a id="&#x7CFB;&#x7EDF;&#x4FE1;&#x606F;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.getSystemInfoSync](https://developers.weixin.qq.com/miniprogram/dev/api/base/system/system-info/wx.getSystemInfoSync.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getSystemInfo](https://developers.weixin.qq.com/miniprogram/dev/api/base/system/system-info/wx.getSystemInfo.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  屏幕亮度 <a id="&#x5C4F;&#x5E55;&#x4EAE;&#x5EA6;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.setKeepScreenOn](https://developers.weixin.qq.com/miniprogram/dev/api/device/screen/wx.setKeepScreenOn.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.setScreenBrightness](https://developers.weixin.qq.com/miniprogram/dev/api/device/screen/wx.setScreenBrightness.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getScreenBrightness](https://developers.weixin.qq.com/miniprogram/dev/api/device/screen/wx.getScreenBrightness.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  用户截屏事件 <a id="&#x7528;&#x6237;&#x622A;&#x5C4F;&#x4E8B;&#x4EF6;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.onUserCaptureScreen](https://developers.weixin.qq.com/miniprogram/dev/api/device/screen/wx.onUserCaptureScreen.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.offUserCaptureScreen](https://developers.weixin.qq.com/miniprogram/dev/api/device/screen/wx.offUserCaptureScreen.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |

####  振动 <a id="&#x632F;&#x52A8;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.vibrateLong](https://developers.weixin.qq.com/miniprogram/dev/api/device/vibrate/wx.vibrateLong.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.vibrateShort](https://developers.weixin.qq.com/miniprogram/dev/api/device/vibrate/wx.vibrateShort.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  手机联系人 <a id="&#x624B;&#x673A;&#x8054;&#x7CFB;&#x4EBA;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.addPhoneContact](https://developers.weixin.qq.com/miniprogram/dev/api/device/contact/wx.addPhoneContact.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  NFC <a id="NFC"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.sendHCEMessage](https://developers.weixin.qq.com/miniprogram/dev/api/device/nfc/wx.sendHCEMessage.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.stopHCE](https://developers.weixin.qq.com/miniprogram/dev/api/device/nfc/wx.stopHCE.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onHCEMessage](https://developers.weixin.qq.com/miniprogram/dev/api/device/nfc/wx.onHCEMessage.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offHCEMessage](https://developers.weixin.qq.com/miniprogram/dev/api/device/nfc/wx.offHCEMessage.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.startHCE](https://developers.weixin.qq.com/miniprogram/dev/api/device/nfc/wx.startHCE.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getHCEState](https://developers.weixin.qq.com/miniprogram/dev/api/device/nfc/wx.getHCEState.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  网络状态 <a id="&#x7F51;&#x7EDC;&#x72B6;&#x6001;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.onNetworkStatusChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/network/wx.onNetworkStatusChange.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offNetworkStatusChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/network/wx.offNetworkStatusChange.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getNetworkType](https://developers.weixin.qq.com/miniprogram/dev/api/device/network/wx.getNetworkType.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  加速度计 <a id="&#x52A0;&#x901F;&#x5EA6;&#x8BA1;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.startAccelerometer](https://developers.weixin.qq.com/miniprogram/dev/api/device/accelerometer/wx.startAccelerometer.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.stopAccelerometer](https://developers.weixin.qq.com/miniprogram/dev/api/device/accelerometer/wx.stopAccelerometer.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onAccelerometerChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/accelerometer/wx.onAccelerometerChange.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offAccelerometerChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/accelerometer/wx.offAccelerometerChange.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  设备方向 <a id="&#x8BBE;&#x5907;&#x65B9;&#x5411;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.startDeviceMotionListening](https://developers.weixin.qq.com/miniprogram/dev/api/device/motion/wx.startDeviceMotionListening.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.stopDeviceMotionListening](https://developers.weixin.qq.com/miniprogram/dev/api/device/motion/wx.stopDeviceMotionListening.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offDeviceMotionChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/motion/wx.offDeviceMotionChange.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onDeviceMotionChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/motion/wx.onDeviceMotionChange.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  陀螺仪 <a id="&#x9640;&#x87BA;&#x4EEA;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.startGyroscope](https://developers.weixin.qq.com/miniprogram/dev/api/device/gyroscope/wx.startGyroscope.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.stopGyroscope](https://developers.weixin.qq.com/miniprogram/dev/api/device/gyroscope/wx.stopGyroscope.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offGyroscopeChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/gyroscope/wx.offGyroscopeChange.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onGyroscopeChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/gyroscope/wx.onGyroscopeChange.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  罗盘 <a id="&#x7F57;&#x76D8;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.onCompassChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/compass/wx.onCompassChange.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offCompassChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/compass/wx.offCompassChange.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.stopCompass](https://developers.weixin.qq.com/miniprogram/dev/api/device/compass/wx.stopCompass.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.startCompass](https://developers.weixin.qq.com/miniprogram/dev/api/device/compass/wx.startCompass.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  拨打电话 <a id="&#x62E8;&#x6253;&#x7535;&#x8BDD;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.makePhoneCall](https://developers.weixin.qq.com/miniprogram/dev/api/device/phone/wx.makePhoneCall.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  扫码 <a id="&#x626B;&#x7801;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.scanCode](https://developers.weixin.qq.com/miniprogram/dev/api/device/scan/wx.scanCode.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  剪贴板 <a id="&#x526A;&#x8D34;&#x677F;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.setClipboardData](https://developers.weixin.qq.com/miniprogram/dev/api/device/clipboard/wx.setClipboardData.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getClipboardData](https://developers.weixin.qq.com/miniprogram/dev/api/device/clipboard/wx.getClipboardData.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  蓝牙 <a id="&#x84DD;&#x7259;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.writeBLECharacteristicValue](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.writeBLECharacteristicValue.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.startBluetoothDevicesDiscovery](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.startBluetoothDevicesDiscovery.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getConnectedBluetoothDevices](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.getConnectedBluetoothDevices.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.notifyBLECharacteristicValueChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.notifyBLECharacteristicValueChange.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onBluetoothDeviceFound](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.onBluetoothDeviceFound.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offBluetoothDeviceFound](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.offBluetoothDeviceFound.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.readBLECharacteristicValue](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.readBLECharacteristicValue.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.openBluetoothAdapter](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.openBluetoothAdapter.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getBLEDeviceCharacteristics](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.getBLEDeviceCharacteristics.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.stopBluetoothDevicesDiscovery](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.stopBluetoothDevicesDiscovery.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onBLEConnectionStateChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.onBLEConnectionStateChange.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getBluetoothDevices](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.getBluetoothDevices.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getBluetoothAdapterState](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.getBluetoothAdapterState.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onBluetoothAdapterStateChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.onBluetoothAdapterStateChange.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offBluetoothAdapterStateChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.offBluetoothAdapterStateChange.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getBLEDeviceServices](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.getBLEDeviceServices.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onBLECharacteristicValueChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.onBLECharacteristicValueChange.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offBLECharacteristicValueChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.offBLECharacteristicValueChange.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.createBLEConnection](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.createBLEConnection.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.closeBluetoothAdapter](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth/wx.closeBluetoothAdapter.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.closeBLEConnection](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.closeBLEConnection.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.notifyBLECharacteristicValueChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.notifyBLECharacteristicValueChange.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onBLEConnectionStateChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.onBLEConnectionStateChange.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offBLEConnectionStateChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/bluetooth-ble/wx.offBLEConnectionStateChange.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  iBeacon <a id="iBeacon"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.getBeacons](https://developers.weixin.qq.com/miniprogram/dev/api/device/ibeacon/wx.getBeacons.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.startBeaconDiscovery](https://developers.weixin.qq.com/miniprogram/dev/api/device/ibeacon/wx.startBeaconDiscovery.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onBeaconServiceChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/ibeacon/wx.onBeaconServiceChange.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offBeaconServiceChange](https://developers.weixin.qq.com/miniprogram/dev/api/device/ibeacon/wx.offBeaconServiceChange.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onBeaconUpdate](https://developers.weixin.qq.com/miniprogram/dev/api/device/ibeacon/wx.onBeaconUpdate.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offBeaconUpdate](https://developers.weixin.qq.com/miniprogram/dev/api/device/ibeacon/wx.offBeaconUpdate.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.stopBeaconDiscovery](https://developers.weixin.qq.com/miniprogram/dev/api/device/ibeacon/wx.stopBeaconDiscovery.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  Wi-Fi <a id="Wi-Fi"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.connectWifi](https://developers.weixin.qq.com/miniprogram/dev/api/device/wifi/wx.connectWifi.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getConnectedWifi](https://developers.weixin.qq.com/miniprogram/dev/api/device/wifi/wx.getConnectedWifi.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getWifiList](https://developers.weixin.qq.com/miniprogram/dev/api/device/wifi/wx.getWifiList.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offGetWifiList](https://developers.weixin.qq.com/miniprogram/dev/api/device/wifi/wx.offGetWifiList.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offWifiConnected](https://developers.weixin.qq.com/miniprogram/dev/api/device/wifi/wx.offWifiConnected.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onEvaluateWifi](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/%28wx.onEvaluateWifi%29) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onGetWifiList](https://developers.weixin.qq.com/miniprogram/dev/api/device/wifi/wx.onGetWifiList.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onWifiConnected](https://developers.weixin.qq.com/miniprogram/dev/api/device/wifi/wx.onWifiConnected.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.presetWifiList](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/%28wx.presetWifiList%29) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.setWifiList](https://developers.weixin.qq.com/miniprogram/dev/api/device/wifi/wx.setWifiList.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.startWifi](https://developers.weixin.qq.com/miniprogram/dev/api/device/wifi/wx.startWifi.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.stopWifi](https://developers.weixin.qq.com/miniprogram/dev/api/device/wifi/wx.stopWifi.html) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  VoIP <a id="VoIP"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.exitVoIPChat](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/%28wx.exitVoIPChat%29) | [2.9.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.joinVoIPChat](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/%28wx.joinVoIPChat%29) | [2.9.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offVoIPChatInterrupted](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/%28wx.offVoIPChatInterrupted%29) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offVoIPChatMembersChanged](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/%28wx.offVoIPChatMembersChanged%29) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.offVoIPChatSpeakersChanged](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/%28wx.offVoIPChatSpeakersChanged%29) | [2.9.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onVoIPChatInterrupted](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/%28wx.onVoIPChatInterrupted%29) | [2.9.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onVoIPChatMembersChanged](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/%28wx.onVoIPChatMembersChanged%29) | [2.9.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.onVoIPChatSpeakersChanged](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/%28wx.onVoIPChatSpeakersChanged%29) | [2.9.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.updateVoIPChatMuteConfig](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/%28wx.updateVoIPChatMuteConfig%29) | [2.9.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  交互反馈 <a id="&#x4EA4;&#x4E92;&#x53CD;&#x9988;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.hideLoading](https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.hideLoading.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.showActionSheet](https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showActionSheet.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.showLoading](https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showLoading.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.hideToast](https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.hideToast.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.showToast](https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showToast.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.showModal](https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showModal.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  设置导航条 <a id="&#x8BBE;&#x7F6E;&#x5BFC;&#x822A;&#x6761;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.showNavigationBarLoading](https://developers.weixin.qq.com/miniprogram/dev/api/ui/navigation-bar/wx.showNavigationBarLoading.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.hideNavigationBarLoading](https://developers.weixin.qq.com/miniprogram/dev/api/ui/navigation-bar/wx.hideNavigationBarLoading.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.setNavigationBarColor](https://developers.weixin.qq.com/miniprogram/dev/api/ui/navigation-bar/wx.setNavigationBarColor.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.setNavigationBarTitle](https://developers.weixin.qq.com/miniprogram/dev/api/ui/navigation-bar/wx.setNavigationBarTitle.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |

####  背景 <a id="&#x80CC;&#x666F;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.setBackgroundColor](https://developers.weixin.qq.com/miniprogram/dev/api/ui/background/wx.setBackgroundColor.html) | [2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.setBackgroundTextStyle](https://developers.weixin.qq.com/miniprogram/dev/api/ui/background/wx.setBackgroundTextStyle.html) | [2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |

####  WXML节点信息 <a id="WXML&#x8282;&#x70B9;&#x4FE1;&#x606F;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.createSelectorQuery](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/wx.createSelectorQuery.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  WXML节点布局相交状态 <a id="WXML&#x8282;&#x70B9;&#x5E03;&#x5C40;&#x76F8;&#x4EA4;&#x72B6;&#x6001;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.createIntersectionObserver](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/wx.createIntersectionObserver.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  导航 <a id="&#x5BFC;&#x822A;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.navigateBack](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.navigateBack.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.navigateTo](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.navigateTo.html) | [2.2.2](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.redirectTo](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.redirectTo.html) | [2.2.2](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.switchTab](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.switchTab.html) | [2.3.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.reLaunch](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.reLaunch.html) | [2.3.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |

####  动画 <a id="&#x52A8;&#x753B;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.createAnimation](https://developers.weixin.qq.com/miniprogram/dev/api/ui/animation/wx.createAnimation.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  位置 <a id="&#x4F4D;&#x7F6E;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.pageScrollTo](https://developers.weixin.qq.com/miniprogram/dev/api/ui/scroll/wx.pageScrollTo.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |

####  绘图 <a id="&#x7ED8;&#x56FE;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.drawCanvas](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/%28wx.drawCanvas%29) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.createOffscreenCanvas](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/wx.createOffscreenCanvas.html) | [2.7.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.canvasPutImageData](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/wx.canvasPutImageData.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.canvasToTempFilePath](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/wx.canvasToTempFilePath.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.createCanvasContext](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/wx.createCanvasContext.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.canvasGetImageData](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/wx.canvasGetImageData.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  下拉刷新 <a id="&#x4E0B;&#x62C9;&#x5237;&#x65B0;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.stopPullDownRefresh](https://developers.weixin.qq.com/miniprogram/dev/api/ui/pull-down-refresh/wx.stopPullDownRefresh.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.startPullDownRefresh](https://developers.weixin.qq.com/miniprogram/dev/api/ui/pull-down-refresh/wx.startPullDownRefresh.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |

####  当前帐号信息 <a id="&#x5F53;&#x524D;&#x5E10;&#x53F7;&#x4FE1;&#x606F;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.getAccountInfoSync](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/account-info/wx.getAccountInfoSync.html) | [2.2.2](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

####  转发 <a id="&#x8F6C;&#x53D1;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.hideShareMenu](https://developers.weixin.qq.com/miniprogram/dev/api/share/wx.hideShareMenu.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.getShareInfo](https://developers.weixin.qq.com/miniprogram/dev/api/share/wx.getShareInfo.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.showShareMenu](https://developers.weixin.qq.com/miniprogram/dev/api/share/wx.showShareMenu.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |
| [wx.updateShareMenu](https://developers.weixin.qq.com/miniprogram/dev/api/share/wx.updateShareMenu.html) | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 仅限插件页面中调用 |

####  其他 <a id="&#x5176;&#x4ED6;"></a>

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.getSetting](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/setting/wx.getSetting.html) | [2.6.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.reportAnalytics](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/data-analysis/wx.reportAnalytics.html) | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) | 见下方备注 |

####  登录和获取用户信息 <a id="&#x767B;&#x5F55;&#x548C;&#x83B7;&#x53D6;&#x7528;&#x6237;&#x4FE1;&#x606F;"></a>

**这一组接口仅限在用户信息功能页中获得用户授权之后调用。否则将返回 fail 。详见** [**用户信息功能页**](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/functional-pages/user-info.html) **。**

| API | 最低版本 | 备注 |
| :--- | :--- | :--- |
| [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) | [2.3.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |
| [wx.getUserInfo](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/user-info/wx.getUserInfo.html) | [2.3.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |  |

 **Bugs & Tips**

* [wx.reportAnalytics](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/data-analysis/wx.reportAnalytics.html) 可以被正常调用，但目前不会进行统计展示。

