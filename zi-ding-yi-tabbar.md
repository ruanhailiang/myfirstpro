# 自定义 tabBar

### 自定义 tabBar <a id="&#x81EA;&#x5B9A;&#x4E49;-tabBar"></a>

> 基础库 2.5.0 开始支持，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。

自定义 tabBar 可以让开发者更加灵活地设置 tabBar 样式，以满足更多个性化的场景。

在自定义 tabBar 模式下

* 为了保证低版本兼容以及区分哪些页面是 tab 页，tabBar 的相关配置项需完整声明，但这些字段不会作用于自定义 tabBar 的渲染。
* 此时需要开发者提供一个自定义组件来渲染 tabBar，所有 tabBar 的样式都由该自定义组件渲染。推荐用 fixed 在底部的 [cover-view](https://developers.weixin.qq.com/miniprogram/dev/component/cover-view.html) + [cover-image](https://developers.weixin.qq.com/miniprogram/dev/component/cover-image.html) 组件渲染样式，以保证 tabBar 层级相对较高。
* 与 tabBar 样式相关的接口，如 [wx.setTabBarItem](https://developers.weixin.qq.com/miniprogram/dev/api/ui/tab-bar/wx.setTabBarItem.html) 等将失效。
* **每个 tab 页下的自定义 tabBar 组件实例是不同的**，可通过自定义组件下的 `getTabBar` 接口，获取当前页面的自定义 tabBar 组件实例。

**注意：如需实现 tab 选中态，要在当前页面下，通过 `getTabBar` 接口获取组件实例，并调用 setData 更新选中态。可参考底部的代码示例。**

####  使用流程 <a id="&#x4F7F;&#x7528;&#x6D41;&#x7A0B;"></a>

 **1. 配置信息**

* 在 `app.json` 中的 `tabBar` 项指定 `custom` 字段，同时其余 `tabBar` 相关配置也补充完整。
* 所有 tab 页的 json 里需声明 `usingComponents` 项，也可以在 `app.json` 全局开启。

示例：

```text
{
  "tabBar": {
    "custom": true,
    "color": "#000000",
    "selectedColor": "#000000",
    "backgroundColor": "#000000",
    "list": [{
      "pagePath": "page/component/index",
      "text": "组件"
    }, {
      "pagePath": "page/API/index",
      "text": "接口"
    }]
  },
  "usingComponents": {}
}
```

####  2. 添加 tabBar 代码文件 <a id="_2-&#x6DFB;&#x52A0;-tabBar-&#x4EE3;&#x7801;&#x6587;&#x4EF6;"></a>

在代码根目录下添加入口文件:

```text
custom-tab-bar/index.js
custom-tab-bar/index.json
custom-tab-bar/index.wxml
custom-tab-bar/index.wxss
```

 **3. 编写 tabBar 代码**

用自定义组件的方式编写即可，该自定义组件完全接管 tabBar 的渲染。另外，自定义组件新增 `getTabBar` 接口，可获取当前页面下的自定义 tabBar 组件实例。

####  示例代码 <a id="&#x793A;&#x4F8B;&#x4EE3;&#x7801;"></a>

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/jiSARvmF7i55)



