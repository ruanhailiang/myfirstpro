# 目录结构

## 目录结构 <a id="&#x76EE;&#x5F55;&#x7ED3;&#x6784;"></a>

小程序包含一个描述整体程序的 `app` 和多个描述各自页面的 `page`。

一个小程序主体部分由三个文件组成，必须放在项目的根目录，如下：

| 文件 | 必需 | 作用 |
| :--- | :--- | :--- |
| [app.js](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/app.html) | 是 | 小程序逻辑 |
| [app.json](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html) | 是 | 小程序公共配置 |
| [app.wxss](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxss.html) | 否 | 小程序公共样式表 |

一个小程序页面由四个文件组成，分别是：

| 文件类型 | 必需 | 作用 |
| :--- | :--- | :--- |
| [js](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html) | 是 | 页面逻辑 |
| [wxml](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/) | 是 | 页面结构 |
| [json](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#页面配置) | 否 | 页面配置 |
| [wxss](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxss.html) | 否 | 页面样式表 |

**注意：为了方便开发者减少配置项，描述页面的四个文件必须具有相同的路径与文件名。**

####  允许上传的文件 <a id="&#x5141;&#x8BB8;&#x4E0A;&#x4F20;&#x7684;&#x6587;&#x4EF6;"></a>

在项目目录中，以下文件会经过编译，因此上传之后无法直接访问到：_.js、app.json、_.wxml、\*.wxss（其中 wxml 和 wxss 文件仅针对在 app.json 中配置了的页面）。除此之外，只有后缀名在白名单内的文件可以被上传，不在白名单列表内文件在开发工具能被访问到，但无法被上传。具体白名单列表如下：

1. wxs
2. png
3. jpg
4. jpeg
5. gif
6. svg
7. json
8. cer
9. mp3
10. aac
11. m4a
12. mp4
13. wav
14. ogg
15. silk
