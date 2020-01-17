# 自定义组件

## 自定义组件 <a id="&#x81EA;&#x5B9A;&#x4E49;&#x7EC4;&#x4EF6;"></a>

从小程序基础库版本 [1.6.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始，小程序支持简洁的组件化编程。所有自定义组件相关特性都需要基础库版本 [1.6.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 或更高。

开发者可以将页面内的功能模块抽象成自定义组件，以便在不同的页面中重复使用；也可以将复杂的页面拆分成多个低耦合的模块，有助于代码维护。自定义组件在使用时与基础组件非常相似。

####  创建自定义组件 <a id="&#x521B;&#x5EFA;&#x81EA;&#x5B9A;&#x4E49;&#x7EC4;&#x4EF6;"></a>

类似于页面，一个自定义组件由 `json` `wxml` `wxss` `js` 4个文件组成。要编写一个自定义组件，首先需要在 `json` 文件中进行自定义组件声明（将 `component` 字段设为 `true` 可将这一组文件设为自定义组件）：

```text
{
  "component": true
}
```

同时，还要在 `wxml` 文件中编写组件模板，在 `wxss` 文件中加入组件样式，它们的写法与页面的写法类似。具体细节和注意事项参见 [组件模板和样式](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/wxml-wxss.html) 。

**代码示例：**

```text
<!-- 这是自定义组件的内部WXML结构 -->
<view class="inner">
  {{innerText}}
</view>
<slot></slot>
```

```text
/* 这里的样式只应用于这个自定义组件 */
.inner {
  color: red;
}
```

**注意：在组件wxss中不应使用ID选择器、属性选择器和标签名选择器。**

在自定义组件的 `js` 文件中，需要使用 `Component()` 来注册组件，并提供组件的属性定义、内部数据和自定义方法。

组件的属性值和内部数据将被用于组件 `wxml` 的渲染，其中，属性值是可由组件外部传入的。更多细节参见 [Component构造器](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/component.html) 。

**代码示例：**

```text
Component({
  properties: {
    // 这里定义了innerText属性，属性值可以在组件使用时指定
    innerText: {
      type: String,
      value: 'default value',
    }
  },
  data: {
    // 这里是一些组件内部数据
    someData: {}
  },
  methods: {
    // 这里是一个自定义方法
    customMethod: function(){}
  }
})
```

####  使用自定义组件 <a id="&#x4F7F;&#x7528;&#x81EA;&#x5B9A;&#x4E49;&#x7EC4;&#x4EF6;"></a>

使用已注册的自定义组件前，首先要在页面的 `json` 文件中进行引用声明。此时需要提供每个自定义组件的标签名和对应的自定义组件文件路径：

```text
{
  "usingComponents": {
    "component-tag-name": "path/to/the/custom/component"
  }
}
```

这样，在页面的 `wxml` 中就可以像使用基础组件一样使用自定义组件。节点名即自定义组件的标签名，节点属性即传递给组件的属性值。

> 开发者工具 1.02.1810190 及以上版本支持在 app.json 中声明 usingComponents 字段，在此处声明的自定义组件视为全局自定义组件，在小程序内的页面或自定义组件中可以直接使用而无需再声明。

**代码示例：**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/OMfVAKmZ6KZT)

```text
<view>
  <!-- 以下是对一个自定义组件的引用 -->
  <component-tag-name inner-text="Some text"></component-tag-name>
</view>
```

自定义组件的 `wxml` 节点结构在与数据结合之后，将被插入到引用位置内。

####  细节注意事项 <a id="&#x7EC6;&#x8282;&#x6CE8;&#x610F;&#x4E8B;&#x9879;"></a>

一些需要注意的细节：

* 因为 WXML 节点标签名只能是小写字母、中划线和下划线的组合，所以自定义组件的标签名也只能包含这些字符。
* 自定义组件也是可以引用自定义组件的，引用方法类似于页面引用自定义组件的方式（使用 `usingComponents` 字段）。
* 自定义组件和页面所在项目根目录名不能以“wx-”为前缀，否则会报错。

注意，是否在页面文件中使用 `usingComponents` 会使得页面的 `this` 对象的原型稍有差异，包括：

* 使用 `usingComponents` 页面的原型与不使用时不一致，即 `Object.getPrototypeOf(this)` 结果不同。
* 使用 `usingComponents` 时会多一些方法，如 `selectComponent` 。
* 出于性能考虑，使用 `usingComponents` 时， `setData` 内容不会被直接深复制，即 `this.setData({ field: obj })` 后 `this.data.field === obj` 。（深复制会在这个值被组件间传递时发生。）

如果页面比较复杂，新增或删除 `usingComponents` 定义段时建议重新测试一下。

## 组件模板和样式 <a id="&#x7EC4;&#x4EF6;&#x6A21;&#x677F;&#x548C;&#x6837;&#x5F0F;"></a>

类似于页面，自定义组件拥有自己的 `wxml` 模板和 `wxss` 样式。

####  组件模板 <a id="&#x7EC4;&#x4EF6;&#x6A21;&#x677F;"></a>

组件模板的写法与页面模板相同。组件模板与组件数据结合后生成的节点树，将被插入到组件的引用位置上。

在组件模板中可以提供一个 `<slot>` 节点，用于承载组件引用时提供的子节点。

**代码示例：**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/1udXLnmi6KY2)

```text
<!-- 组件模板 -->
<view class="wrapper">
  <view>这里是组件的内部节点</view>
  <slot></slot>
</view>
```

```text
<!-- 引用组件的页面模板 -->
<view>
  <component-tag-name>
    <!-- 这部分内容将被放置在组件 <slot> 的位置上 -->
    <view>这里是插入到组件slot中的内容</view>
  </component-tag-name>
</view>
```

注意，在模板中引用到的自定义组件及其对应的节点名需要在 `json` 文件中显式定义，否则会被当作一个无意义的节点。除此以外，节点名也可以被声明为[抽象节点](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/generics.html)。

####  模板数据绑定 <a id="&#x6A21;&#x677F;&#x6570;&#x636E;&#x7ED1;&#x5B9A;"></a>

与普通的 WXML 模板类似，可以使用数据绑定，这样就可以向子组件的属性传递动态数据。

**代码示例：**

```text
<!-- 引用组件的页面模板 -->
<view>
  <component-tag-name prop-a="{{dataFieldA}}" prop-b="{{dataFieldB}}">
    <!-- 这部分内容将被放置在组件 <slot> 的位置上 -->
    <view>这里是插入到组件slot中的内容</view>
  </component-tag-name>
</view>
```

在以上例子中，组件的属性 `propA` 和 `propB` 将收到页面传递的数据。页面可以通过 `setData` 来改变绑定的数据字段。

注意：这样的数据绑定只能传递 JSON 兼容数据。自基础库版本 [2.0.9](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始，还可以在数据中包含函数（但这些函数不能在 WXML 中直接调用，只能传递给子组件）。

####  组件wxml的slot <a id="&#x7EC4;&#x4EF6;wxml&#x7684;slot"></a>

在组件的wxml中可以包含 `slot` 节点，用于承载组件使用者提供的wxml结构。

默认情况下，一个组件的wxml中只能有一个slot。需要使用多slot时，可以在组件js中声明启用。

```text
Component({
  options: {
    multipleSlots: true // 在组件定义时的选项中启用多slot支持
  },
  properties: { /* ... */ },
  methods: { /* ... */ }
})
```

此时，可以在这个组件的wxml中使用多个slot，以不同的 `name` 来区分。

```text
<!-- 组件模板 -->
<view class="wrapper">
  <slot name="before"></slot>
  <view>这里是组件的内部细节</view>
  <slot name="after"></slot>
</view>
```

使用时，用 `slot` 属性来将节点插入到不同的slot上。

```text
<!-- 引用组件的页面模板 -->
<view>
  <component-tag-name>
    <!-- 这部分内容将被放置在组件 <slot name="before"> 的位置上 -->
    <view slot="before">这里是插入到组件slot name="before"中的内容</view>
    <!-- 这部分内容将被放置在组件 <slot name="after"> 的位置上 -->
    <view slot="after">这里是插入到组件slot name="after"中的内容</view>
  </component-tag-name>
</view>
```

####  组件样式 <a id="&#x7EC4;&#x4EF6;&#x6837;&#x5F0F;"></a>

组件对应 `wxss` 文件的样式，只对组件wxml内的节点生效。编写组件样式时，需要注意以下几点：

* 组件和引用组件的页面不能使用id选择器（`#a`）、属性选择器（`[a]`）和标签名选择器，请改用class选择器。
* 组件和引用组件的页面中使用后代选择器（`.a .b`）在一些极端情况下会有非预期的表现，如遇，请避免使用。
* 子元素选择器（`.a>.b`）只能用于 `view` 组件与其子节点之间，用于其他组件可能导致非预期的情况。
* 继承样式，如 `font` 、 `color` ，会从组件外继承到组件内。
* 除继承样式外， `app.wxss` 中的样式、组件所在页面的的样式对自定义组件无效（除非更改组件样式隔离选项）。

```text
#a { } /* 在组件中不能使用 */
[a] { } /* 在组件中不能使用 */
button { } /* 在组件中不能使用 */
.a > .b { } /* 除非 .a 是 view 组件节点，否则不一定会生效 */
```

除此以外，组件可以指定它所在节点的默认样式，使用 `:host` 选择器（需要包含基础库 [1.7.2](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 或更高版本的开发者工具支持）。

**代码示例：**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/jAgvwKm16bZD)

```text
/* 组件 custom-component.wxss */
:host {
  color: yellow;
}
```

```text
<!-- 页面的 WXML -->
<custom-component>这段文本是黄色的</custom-component>
```

####  组件样式隔离 <a id="&#x7EC4;&#x4EF6;&#x6837;&#x5F0F;&#x9694;&#x79BB;"></a>

默认情况下，自定义组件的样式只受到自定义组件 wxss 的影响。除非以下两种情况：

* `app.wxss` 或页面的 `wxss` 中使用了标签名选择器（或一些其他特殊选择器）来直接指定样式，这些选择器会影响到页面和全部组件。通常情况下这是不推荐的做法。
* 指定特殊的样式隔离选项 `styleIsolation` 。

```text
Component({
  options: {
    styleIsolation: 'isolated'
  }
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/xPQhJcm37e7h)

`styleIsolation` 选项从基础库版本 [2.6.5](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始支持。它支持以下取值：

* `isolated` 表示启用样式隔离，在自定义组件内外，使用 class 指定的样式将不会相互影响（一般情况下的默认值）；
* `apply-shared` 表示页面 wxss 样式将影响到自定义组件，但自定义组件 wxss 中指定的样式不会影响页面；
* `shared` 表示页面 wxss 样式将影响到自定义组件，自定义组件 wxss 中指定的样式也会影响页面和其他设置了 `apply-shared` 或 `shared` 的自定义组件。（这个选项在插件中不可用。）

**使用后两者时，请务必注意组件间样式的相互影响。**

如果这个 [Component 构造器用于构造页面](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/component.html) ，则默认值为 `shared` ，且还有以下几个额外的样式隔离选项可用：

* `page-isolated` 表示在这个页面禁用 app.wxss ，同时，页面的 wxss 不会影响到其他自定义组件；
* `page-apply-shared` 表示在这个页面禁用 app.wxss ，同时，页面 wxss 样式不会影响到其他自定义组件，但设为 `shared` 的自定义组件会影响到页面；
* `page-shared` 表示在这个页面禁用 app.wxss ，同时，页面 wxss 样式会影响到其他设为 `apply-shared` 或 `shared` 的自定义组件，也会受到设为 `shared` 的自定义组件的影响。

此外，小程序基础库版本 [2.2.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 以上支持 `addGlobalClass` 选项，即在 `Component` 的 `options` 中设置 `addGlobalClass: true` 。 这个选项等价于设置 `styleIsolation: apply-shared` ，但设置了 `styleIsolation` 选项后这个选项会失效。

**代码示例：**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/yNPLQvm87b1F)

```text
/* 组件 custom-component.js */
Component({
  options: {
    addGlobalClass: true,
  }
})
```

```text
<!-- 组件 custom-component.wxml -->
<text class="red-text">这段文本的颜色由 `app.wxss` 和页面 `wxss` 中的样式定义来决定</text>
```

```text
/* app.wxss */
.red-text {
  color: red;
}
```

####  外部样式类 <a id="&#x5916;&#x90E8;&#x6837;&#x5F0F;&#x7C7B;"></a>

> 基础库 1.9.90 开始支持，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。

有时，组件希望接受外部传入的样式类。此时可以在 `Component` 中用 `externalClasses` 定义段定义若干个外部样式类。

这个特性可以用于实现类似于 `view` 组件的 `hover-class` 属性：页面可以提供一个样式类，赋予 `view` 的 `hover-class` ，这个样式类本身写在页面中而非 `view` 组件的实现中。

**注意：在同一个节点上使用普通样式类和外部样式类时，两个类的优先级是未定义的，因此最好避免这种情况。**

**代码示例：**

```text
/* 组件 custom-component.js */
Component({
  externalClasses: ['my-class']
})
```

```text
<!-- 组件 custom-component.wxml -->
<custom-component class="my-class">这段文本的颜色由组件外的 class 决定</custom-component>
```

这样，组件的使用者可以指定这个样式类对应的 class ，就像使用普通属性一样。在 [2.7.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 之后，可以指定多个对应的 class 。

**代码示例：**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/rbgNNKmE6bZK)

```text
<!-- 页面的 WXML -->
<custom-component my-class="red-text" />
<custom-component my-class="large-text" />
<!-- 以下写法需要基础库版本 2.7.1 以上 -->
<custom-component my-class="red-text large-text" />
```

```text
.red-text {
  color: red;
}
.large-text {
  font-size: 1.5em;
}
```

####  引用页面或父组件的样式 <a id="&#x5F15;&#x7528;&#x9875;&#x9762;&#x6216;&#x7236;&#x7EC4;&#x4EF6;&#x7684;&#x6837;&#x5F0F;"></a>

> 基础库 2.9.2 开始支持，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。

即使启用了样式隔离 `isolated` ，组件仍然可以在局部引用组件所在页面的样式或父组件的样式。

例如，如果在页面 wxss 中定义了：

```text
.blue-text {
  color: blue;
}
```

在这个组件中可以使用 `~` 来引用这个类的样式：

```text
<view class="~blue-text"> 这段文本是蓝色的 </view>
```

如果在一个组件的父组件 wxss 中定义了：

```text
.red-text {
  color: red;
}
```

在这个组件中可以使用 `^` 来引用这个类的样式：

```text
<view class="^red-text"> 这段文本是红色的 </view>
```

也可以连续使用多个 `^` 来引用祖先组件中的样式。

**注意：如果组件是比较独立、通用的组件，请优先使用外部样式类的方式，而非直接引用父组件或页面的样式。**

\*\*\*\*

## Component 构造器 <a id="Component-&#x6784;&#x9020;&#x5668;"></a>

`Component` 构造器可用于定义组件，调用 `Component` 构造器时可以指定组件的属性、数据、方法等。

详细的参数含义和使用请参考 [Component 参考文档](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Component.html)。

```text
Component({

  behaviors: [],

  properties: {
    myProperty: { // 属性名
      type: String,
      value: ''
    },
    myProperty2: String // 简化的定义方式
  },
  
  data: {}, // 私有数据，可用于模板渲染

  lifetimes: {
    // 生命周期函数，可以为函数，或一个在methods段中定义的方法名
    attached: function () { },
    moved: function () { },
    detached: function () { },
  },

  // 生命周期函数，可以为函数，或一个在methods段中定义的方法名
  attached: function () { }, // 此处attached的声明会被lifetimes字段中的声明覆盖
  ready: function() { },

  pageLifetimes: {
    // 组件所在页面的生命周期函数
    show: function () { },
    hide: function () { },
    resize: function () { },
  },

  methods: {
    onMyButtonTap: function(){
      this.setData({
        // 更新属性和数据的方法与更新页面数据的方法类似
      })
    },
    // 内部方法建议以下划线开头
    _myPrivateMethod: function(){
      // 这里将 data.A[0].B 设为 'myPrivateData'
      this.setData({
        'A[0].B': 'myPrivateData'
      })
    },
    _propertyChange: function(newVal, oldVal) {

    }
  }

})
```

###  使用 Component 构造器构造页面 <a id="&#x4F7F;&#x7528;-Component-&#x6784;&#x9020;&#x5668;&#x6784;&#x9020;&#x9875;&#x9762;"></a>

事实上，小程序的页面也可以视为自定义组件。因而，页面也可以使用 `Component` 构造器构造，拥有与普通组件一样的定义段与实例方法。但此时要求对应 json 文件中包含 `usingComponents` 定义段。

此时，组件的属性可以用于接收页面的参数，如访问页面 `/pages/index/index?paramA=123&paramB=xyz` ，如果声明有属性 `paramA` 或 `paramB` ，则它们会被赋值为 `123` 或 `xyz` 。

页面的生命周期方法（即 `on` 开头的方法），应写在 `methods` 定义段中。

**代码示例：**

```text
{
  "usingComponents": {}
}
```

```text
Component({

  properties: {
    paramA: Number,
    paramB: String,
  },

  methods: {
    onLoad: function() {
      this.data.paramA // 页面参数 paramA 的值
      this.data.paramB // 页面参数 paramB 的值
    }
  }

})
```

使用 `Component` 构造器来构造页面的一个好处是可以使用 `behaviors` 来提取所有页面中公用的代码段。

例如，在所有页面被创建和销毁时都要执行同一段代码，就可以把这段代码提取到 `behaviors` 中。

**代码示例：**

```text
// page-common-behavior.js
module.exports = Behavior({
  attached: function() {
    // 页面创建时执行
    console.info('Page loaded!')
  },
  detached: function() {
    // 页面销毁时执行
    console.info('Page unloaded!')
  }
})
```

```text
// 页面 A
var pageCommonBehavior = require('./page-common-behavior')
Component({
  behaviors: [pageCommonBehavior],
  data: { /* ... */ },
  methods: { /* ... */ },
})
```

```text
// 页面 B
var pageCommonBehavior = require('./page-common-behavior')
Component({
  behaviors: [pageCommonBehavior],
  data: { /* ... */ },
  methods: { /* ... */ },
})
```



## 组件间通信与事件 <a id="&#x7EC4;&#x4EF6;&#x95F4;&#x901A;&#x4FE1;&#x4E0E;&#x4E8B;&#x4EF6;"></a>

###  组件间通信 <a id="&#x7EC4;&#x4EF6;&#x95F4;&#x901A;&#x4FE1;"></a>

组件间的基本通信方式有以下几种。

* WXML 数据绑定：用于父组件向子组件的指定属性设置数据，仅能设置 JSON 兼容数据（自基础库版本 [2.0.9](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始，还可以在数据中包含函数）。具体在 [组件模板和样式](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/wxml-wxss.html) 章节中介绍。
* 事件：用于子组件向父组件传递数据，可以传递任意数据。
* 如果以上两种方式不足以满足需要，父组件还可以通过 `this.selectComponent` 方法获取子组件实例对象，这样就可以直接访问组件的任意数据和方法。

###  监听事件 <a id="&#x76D1;&#x542C;&#x4E8B;&#x4EF6;"></a>

事件系统是组件间通信的主要方式之一。自定义组件可以触发任意的事件，引用组件的页面可以监听这些事件。关于事件的基本概念和用法，参见 [事件](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html) 。

监听自定义组件事件的方法与监听基础组件事件的方法完全一致：

**代码示例：**

```text
<!-- 当自定义组件触发“myevent”事件时，调用“onMyEvent”方法 -->
<component-tag-name bindmyevent="onMyEvent" />
<!-- 或者可以写成 -->
<component-tag-name bind:myevent="onMyEvent" />
```

```text
Page({
  onMyEvent: function(e){
    e.detail // 自定义组件触发事件时提供的detail对象
  }
})
```

###  触发事件 <a id="&#x89E6;&#x53D1;&#x4E8B;&#x4EF6;"></a>

自定义组件触发事件时，需要使用 `triggerEvent` 方法，指定事件名、detail对象和事件选项：

**代码示例：**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/DFfYSKmI6vZD)

```text
<!-- 在自定义组件中 -->
<button bindtap="onTap">点击这个按钮将触发“myevent”事件</button>
```

```text
Component({
  properties: {},
  methods: {
    onTap: function(){
      var myEventDetail = {} // detail对象，提供给事件监听函数
      var myEventOption = {} // 触发事件的选项
      this.triggerEvent('myevent', myEventDetail, myEventOption)
    }
  }
})
```

触发事件的选项包括：

| 选项名 | 类型 | 是否必填 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- | :--- |
| bubbles | Boolean | 否 | false | 事件是否冒泡 |
| composed | Boolean | 否 | false | 事件是否可以穿越组件边界，为false时，事件将只能在引用组件的节点树上触发，不进入其他任何组件内部 |
| capturePhase | Boolean | 否 | false | 事件是否拥有捕获阶段 |

关于冒泡和捕获阶段的概念，请阅读 [事件](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html) 章节中的相关说明。

**代码示例：**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/UGfljKm66zZ1)

```text
// 页面 page.wxml
<another-component bindcustomevent="pageEventListener1">
  <my-component bindcustomevent="pageEventListener2"></my-component>
</another-component>
```

```text
// 组件 another-component.wxml
<view bindcustomevent="anotherEventListener">
  <slot />
</view>
```

```text
// 组件 my-component.wxml
<view bindcustomevent="myEventListener">
  <slot />
</view>
```

```text
// 组件 my-component.js
Component({
  methods: {
    onTap: function(){
      this.triggerEvent('customevent', {}) // 只会触发 pageEventListener2
      this.triggerEvent('customevent', {}, { bubbles: true }) // 会依次触发 pageEventListener2 、 pageEventListener1
      this.triggerEvent('customevent', {}, { bubbles: true, composed: true }) // 会依次触发 pageEventListener2 、 anotherEventListener 、 pageEventListener1
    }
  }
})
```

## 组件生命周期 <a id="&#x7EC4;&#x4EF6;&#x751F;&#x547D;&#x5468;&#x671F;"></a>

组件的生命周期，指的是组件自身的一些函数，这些函数在特殊的时间点或遇到一些特殊的框架事件时被自动触发。

其中，最重要的生命周期是 `created` `attached` `detached` ，包含一个组件实例生命流程的最主要时间点。

* 组件实例刚刚被创建好时， `created` 生命周期被触发。此时，组件数据 `this.data` 就是在 `Component` 构造器中定义的数据 `data` 。 **此时还不能调用 `setData` 。** 通常情况下，这个生命周期只应该用于给组件 `this` 添加一些自定义属性字段。
* 在组件完全初始化完毕、进入页面节点树后， `attached` 生命周期被触发。此时， `this.data` 已被初始化为组件的当前值。这个生命周期很有用，绝大多数初始化工作可以在这个时机进行。
* 在组件离开页面节点树后， `detached` 生命周期被触发。退出一个页面时，如果组件还在页面节点树中，则 `detached` 会被触发。

###  定义生命周期方法 <a id="&#x5B9A;&#x4E49;&#x751F;&#x547D;&#x5468;&#x671F;&#x65B9;&#x6CD5;"></a>

生命周期方法可以直接定义在 `Component` 构造器的第一级参数中。

自小程序基础库版本 [2.2.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 起，组件的的生命周期也可以在 `lifetimes` 字段内进行声明（这是推荐的方式，其优先级最高）。

**代码示例：**

```text
Component({
  lifetimes: {
    attached: function() {
      // 在组件实例进入页面节点树时执行
    },
    detached: function() {
      // 在组件实例被从页面节点树移除时执行
    },
  },
  // 以下是旧式的定义方式，可以保持对 <2.2.3 版本基础库的兼容
  attached: function() {
    // 在组件实例进入页面节点树时执行
  },
  detached: function() {
    // 在组件实例被从页面节点树移除时执行
  },
  // ...
})
```

在 behaviors 中也可以编写生命周期方法，同时不会与其他 behaviors 中的同名生命周期相互覆盖。但要注意，如果一个组件多次直接或间接引用同一个 behavior ，这个 behavior 中的生命周期函数在一个执行时机内只会执行一次。

可用的全部生命周期如下表所示。

| 生命周期 | 参数 | 描述 | 最低版本 |
| :--- | :--- | :--- | :--- |
| created | 无 | 在组件实例刚刚被创建时执行 | [1.6.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| attached | 无 | 在组件实例进入页面节点树时执行 | [1.6.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| ready | 无 | 在组件在视图层布局完成后执行 | [1.6.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| moved | 无 | 在组件实例被移动到节点树另一个位置时执行 | [1.6.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| detached | 无 | 在组件实例被从页面节点树移除时执行 | [1.6.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| error | `Object Error` | 每当组件方法抛出错误时执行 | [2.4.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

###  组件所在页面的生命周期 <a id="&#x7EC4;&#x4EF6;&#x6240;&#x5728;&#x9875;&#x9762;&#x7684;&#x751F;&#x547D;&#x5468;&#x671F;"></a>

还有一些特殊的生命周期，它们并非与组件有很强的关联，但有时组件需要获知，以便组件内部处理。这样的生命周期称为“组件所在页面的生命周期”，在 `pageLifetimes` 定义段中定义。其中可用的生命周期包括：

| 生命周期 | 参数 | 描述 | 最低版本 |
| :--- | :--- | :--- | :--- |
| show | 无 | 组件所在的页面被展示时执行 | [2.2.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| hide | 无 | 组件所在的页面被隐藏时执行 | [2.2.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| resize | `Object Size` | 组件所在的页面尺寸变化时执行 | [2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

**代码示例：**

```text
Component({
  pageLifetimes: {
    show: function() {
      // 页面被展示
    },
    hide: function() {
      // 页面被隐藏
    },
    resize: function(size) {
      // 页面尺寸变化
    }
  }
})
```

## behaviors

`behaviors` 是用于组件间代码共享的特性，类似于一些编程语言中的“mixins”或“traits”。

每个 `behavior` 可以包含一组属性、数据、生命周期函数和方法。**组件引用它时，它的属性、数据和方法会被合并到组件中，生命周期函数也会在对应时机被调用。** 每个组件可以引用多个 `behavior` ，`behavior` 也可以引用其他 `behavior` 。

详细的参数含义和使用请参考 [Behavior 参考文档](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Behavior.html)。

###  组件中使用 <a id="&#x7EC4;&#x4EF6;&#x4E2D;&#x4F7F;&#x7528;"></a>

组件引用时，在 `behaviors` 定义段中将它们逐个列出即可。

**代码示例：**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/6SfsUKmE6iZC)

```text
// my-component.js
var myBehavior = require('my-behavior')
Component({
  behaviors: [myBehavior],
  properties: {
    myProperty: {
      type: String
    }
  },
  data: {
    myData: {}
  },
  attached: function(){},
  methods: {
    myMethod: function(){}
  }
})
```

在上例中， `my-component` 组件定义中加入了 `my-behavior` ，而 `my-behavior` 中包含有 `myBehaviorProperty` 属性、 `myBehaviorData` 数据字段、 `myBehaviorMethod` 方法和一个 `attached` 生命周期函数。这将使得 `my-component` 中最终包含 `myBehaviorProperty` 、 `myProperty` 两个属性， `myBehaviorData` 、 `myData` 两个数据字段，和 `myBehaviorMethod` 、 `myMethod` 两个方法。当组件触发 `attached` 生命周期时，会依次触发 `my-behavior` 中的 `attached` 生命周期函数和 `my-component` 中的 `attached` 生命周期函数。

###  字段的覆盖和组合规则 <a id="&#x5B57;&#x6BB5;&#x7684;&#x8986;&#x76D6;&#x548C;&#x7EC4;&#x5408;&#x89C4;&#x5219;"></a>

组件和它引用的 `behavior` 中可以包含同名的字段，对这些字段的处理方法如下：

* 如果有同名的属性或方法，组件本身的属性或方法会覆盖 `behavior` 中的属性或方法，如果引用了多个 `behavior` ，在定义段中靠后 `behavior` 中的属性或方法会覆盖靠前的属性或方法；
* 如果有同名的数据字段，如果数据是对象类型，会进行对象合并，如果是非对象类型则会进行相互覆盖；
* 生命周期函数不会相互覆盖，而是在对应触发时机被逐个调用。如果同一个 `behavior` 被一个组件多次引用，它定义的生命周期函数只会被执行一次。

###  内置 behaviors <a id="&#x5185;&#x7F6E;-behaviors"></a>

自定义组件可以通过引用内置的 `behavior` 来获得内置组件的一些行为。

**代码示例：**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/JgF61qmL69YR)

```text
Component({
  behaviors: ['wx://form-field']
})
```

在上例中， `wx://form-field` 代表一个内置 `behavior` ，它使得这个自定义组件有类似于表单控件的行为。

内置 `behavior` 往往会为组件添加一些属性。在没有特殊说明时，组件可以覆盖这些属性来改变它的 `type` 或添加 `observer` 。

####  wx://form-field <a id="wx-form-field"></a>

使自定义组件有类似于表单控件的行为。 form 组件可以识别这些自定义组件，并在 submit 事件中返回组件的字段名及其对应字段值。这将为它添加以下两个属性。

| 属性名 | 类型 | 描述 | 最低版本 |
| :--- | :--- | :--- | :--- |
| name | String | 在表单中的字段名 | [1.6.7](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| value | 任意 | 在表单中的字段值 | [1.6.7](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

####  wx://component-export <a id="wx-component-export"></a>

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/ZtosuRmK741Y)

从基础库版本 [2.2.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始提供支持。

使自定义组件支持 `export` 定义段。这个定义段可以用于指定组件被 `selectComponent` 调用时的返回值。

未使用这个定义段时， `selectComponent` 将返回自定义组件的 `this` （插件的自定义组件将返回 `null` ）。使用这个定义段时，将以这个定义段的函数返回值代替。

代码示例：

```text
// 自定义组件 my-component 内部
Component({
  behaviors: ['wx://component-export'],
  export() {
    return { myField: 'myValue' }
  }
})
```

```text
<!-- 使用自定义组件时 -->
<my-component id="the-id" />
```

```text
this.selectComponent('#the-id') // 等于 { myField: 'myValue' }
```

## 组件间关系 <a id="&#x7EC4;&#x4EF6;&#x95F4;&#x5173;&#x7CFB;"></a>

###  定义和使用组件间关系 <a id="&#x5B9A;&#x4E49;&#x548C;&#x4F7F;&#x7528;&#x7EC4;&#x4EF6;&#x95F4;&#x5173;&#x7CFB;"></a>

有时需要实现这样的组件：

```text
<custom-ul>
  <custom-li> item 1 </custom-li>
  <custom-li> item 2 </custom-li>
</custom-ul>
```

这个例子中， `custom-ul` 和 `custom-li` 都是自定义组件，它们有相互间的关系，相互间的通信往往比较复杂。此时在组件定义时加入 `relations` 定义段，可以解决这样的问题。示例：

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/0kfvzKm56NZy)

```text
// path/to/custom-ul.js
Component({
  relations: {
    './custom-li': {
      type: 'child', // 关联的目标节点应为子节点
      linked: function(target) {
        // 每次有custom-li被插入时执行，target是该节点实例对象，触发在该节点attached生命周期之后
      },
      linkChanged: function(target) {
        // 每次有custom-li被移动后执行，target是该节点实例对象，触发在该节点moved生命周期之后
      },
      unlinked: function(target) {
        // 每次有custom-li被移除时执行，target是该节点实例对象，触发在该节点detached生命周期之后
      }
    }
  },
  methods: {
    _getAllLi: function(){
      // 使用getRelationNodes可以获得nodes数组，包含所有已关联的custom-li，且是有序的
      var nodes = this.getRelationNodes('path/to/custom-li')
    }
  },
  ready: function(){
    this._getAllLi()
  }
})
```

```text
// path/to/custom-li.js
Component({
  relations: {
    './custom-ul': {
      type: 'parent', // 关联的目标节点应为父节点
      linked: function(target) {
        // 每次被插入到custom-ul时执行，target是custom-ul节点实例对象，触发在attached生命周期之后
      },
      linkChanged: function(target) {
        // 每次被移动后执行，target是custom-ul节点实例对象，触发在moved生命周期之后
      },
      unlinked: function(target) {
        // 每次被移除时执行，target是custom-ul节点实例对象，触发在detached生命周期之后
      }
    }
  }
})
```

**注意：必须在两个组件定义中都加入relations定义，否则不会生效。**

###  关联一类组件 <a id="&#x5173;&#x8054;&#x4E00;&#x7C7B;&#x7EC4;&#x4EF6;"></a>

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/LFEVaqmh6zYU)

有时，需要关联的是一类组件，如：

```text
<custom-form>
  <view>
    input
    <custom-input></custom-input>
  </view>
  <custom-submit> submit </custom-submit>
</custom-form>
```

`custom-form` 组件想要关联 `custom-input` 和 `custom-submit` 两个组件。此时，如果这两个组件都有同一个behavior：

```text
// path/to/custom-form-controls.js
module.exports = Behavior({
  // ...
})
```

```text
// path/to/custom-input.js
var customFormControls = require('./custom-form-controls')
Component({
  behaviors: [customFormControls],
  relations: {
    './custom-form': {
      type: 'ancestor', // 关联的目标节点应为祖先节点
    }
  }
})
```

```text
// path/to/custom-submit.js
var customFormControls = require('./custom-form-controls')
Component({
  behaviors: [customFormControls],
  relations: {
    './custom-form': {
      type: 'ancestor', // 关联的目标节点应为祖先节点
    }
  }
})
```

则在 `relations` 关系定义中，可使用这个behavior来代替组件路径作为关联的目标节点：

```text
// path/to/custom-form.js
var customFormControls = require('./custom-form-controls')
Component({
  relations: {
    'customFormControls': {
      type: 'descendant', // 关联的目标节点应为子孙节点
      target: customFormControls
    }
  }
})
```

###  relations 定义段 <a id="relations-&#x5B9A;&#x4E49;&#x6BB5;"></a>

`relations` 定义段包含目标组件路径及其对应选项，可包含的选项见下表。

| 选项 | 类型 | 是否必填 | 描述 |
| :--- | :--- | :--- | :--- |
| type | String | 是 | 目标组件的相对关系，可选的值为 `parent` 、 `child` 、 `ancestor` 、 `descendant` |
| linked | Function | 否 | 关系生命周期函数，当关系被建立在页面节点树中时触发，触发时机在组件attached生命周期之后 |
| linkChanged | Function | 否 | 关系生命周期函数，当关系在页面节点树中发生改变时触发，触发时机在组件moved生命周期之后 |
| unlinked | Function | 否 | 关系生命周期函数，当关系脱离页面节点树时触发，触发时机在组件detached生命周期之后 |
| target | String | 否 | 如果这一项被设置，则它表示关联的目标节点所应具有的behavior，所有拥有这一behavior的组件节点都会被关联 |



## 数据监听器 <a id="&#x6570;&#x636E;&#x76D1;&#x542C;&#x5668;"></a>

数据监听器可以用于监听和响应任何属性和数据字段的变化。从小程序基础库版本 [2.6.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始支持。

###  使用数据监听器 <a id="&#x4F7F;&#x7528;&#x6570;&#x636E;&#x76D1;&#x542C;&#x5668;"></a>

有时，在一些数据字段被 setData 设置时，需要执行一些操作。

例如， `this.data.sum` 永远是 `this.data.numberA` 与 `this.data.numberB` 的和。此时，可以使用数据监听器进行如下实现。

```text
Component({
  attached: function() {
    this.setData({
      numberA: 1,
      numberB: 2,
    })
  },
  observers: {
    'numberA, numberB': function(numberA, numberB) {
      // 在 numberA 或者 numberB 被设置时，执行这个函数
      this.setData({
        sum: numberA + numberB
      })
    }
  }
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/FUZF9ams7g6N)

###  监听字段语法 <a id="&#x76D1;&#x542C;&#x5B57;&#x6BB5;&#x8BED;&#x6CD5;"></a>

数据监听器支持监听属性或内部数据的变化，可以同时监听多个。一次 setData 最多触发每个监听器一次。

同时，监听器可以监听子数据字段，如下例所示。

```text
Component({
  observers: {
    'some.subfield': function(subfield) {
      // 使用 setData 设置 this.data.some.subfield 时触发
      // （除此以外，使用 setData 设置 this.data.some 也会触发）
      subfield === this.data.some.subfield
    },
    'arr[12]': function(arr12) {
      // 使用 setData 设置 this.data.arr[12] 时触发
      // （除此以外，使用 setData 设置 this.data.arr 也会触发）
      arr12 === this.data.arr[12]
    },
  }
})
```

如果需要监听所有子数据字段的变化，可以使用通配符 `**` 。

```text
Component({
  observers: {
    'some.field.**': function(field) {
      // 使用 setData 设置 this.data.some.field 本身或其下任何子数据字段时触发
      // （除此以外，使用 setData 设置 this.data.some 也会触发）
      field === this.data.some.field
    },
  },
  attached: function() {
    // 这样会触发上面的 observer
    this.setData({
      'some.field': { /* ... */ }
    })
    // 这样也会触发上面的 observer
    this.setData({
      'some.field.xxx': { /* ... */ }
    })
    // 这样还是会触发上面的 observer
    this.setData({
      'some': { /* ... */ }
    })
  }
})
```

特别地，仅使用通配符 `**` 可以监听全部 setData 。

```text
Component({
  observers: {
    '**': function() {
      // 每次 setData 都触发
    },
  },
})
```

**Bugs & Tips:**

* 数据监听器监听的是 setData 涉及到的数据字段，即使这些数据字段的值没有发生变化，数据监听器依然会被触发。
* 如果在数据监听器函数中使用 setData 设置本身监听的数据字段，可能会导致死循环，需要特别留意。
* 数据监听器和属性的 observer 相比，数据监听器更强大且通常具有更好的性能。



## 抽象节点 <a id="&#x62BD;&#x8C61;&#x8282;&#x70B9;"></a>

这个特性自小程序基础库版本 [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始支持。

###  在组件中使用抽象节点 <a id="&#x5728;&#x7EC4;&#x4EF6;&#x4E2D;&#x4F7F;&#x7528;&#x62BD;&#x8C61;&#x8282;&#x70B9;"></a>

有时，自定义组件模板中的一些节点，其对应的自定义组件不是由自定义组件本身确定的，而是自定义组件的调用者确定的。这时可以把这个节点声明为“抽象节点”。

例如，我们现在来实现一个“选框组”（selectable-group）组件，它其中可以放置单选框（custom-radio）或者复选框（custom-checkbox）。这个组件的 wxml 可以这样编写：

**代码示例：**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/ztPzoImW7E7P)

```text
<!-- selectable-group.wxml -->
<view wx:for="{{labels}}">
  <label>
    <selectable disabled="{{false}}"></selectable>
    {{item}}
  </label>
</view>
```

其中，“selectable”不是任何在 json 文件的 `usingComponents` 字段中声明的组件，而是一个抽象节点。它需要在 `componentGenerics` 字段中声明：

```text
{
  "componentGenerics": {
    "selectable": true
  }
}
```

###  使用包含抽象节点的组件 <a id="&#x4F7F;&#x7528;&#x5305;&#x542B;&#x62BD;&#x8C61;&#x8282;&#x70B9;&#x7684;&#x7EC4;&#x4EF6;"></a>

在使用 selectable-group 组件时，必须指定“selectable”具体是哪个组件：

```text
<selectable-group generic:selectable="custom-radio" />
```

这样，在生成这个 selectable-group 组件的实例时，“selectable”节点会生成“custom-radio”组件实例。类似地，如果这样使用：

```text
<selectable-group generic:selectable="custom-checkbox" />
```

“selectable”节点则会生成“custom-checkbox”组件实例。

注意：上述的 `custom-radio` 和 `custom-checkbox` 需要包含在这个 wxml 对应 json 文件的 `usingComponents` 定义段中。

```text
{
  "usingComponents": {
    "custom-radio": "path/to/custom/radio",
    "custom-checkbox": "path/to/custom/checkbox"
  }
}
```

###  抽象节点的默认组件 <a id="&#x62BD;&#x8C61;&#x8282;&#x70B9;&#x7684;&#x9ED8;&#x8BA4;&#x7EC4;&#x4EF6;"></a>

抽象节点可以指定一个默认组件，当具体组件未被指定时，将创建默认组件的实例。默认组件可以在 `componentGenerics` 字段中指定：

```text
{
  "componentGenerics": {
    "selectable": {
      "default": "path/to/default/component"
    }
  }
}
```

**Tips:**

* 节点的 generic 引用 `generic:xxx="yyy"` 中，值 `yyy` 只能是静态值，不能包含数据绑定。因而抽象节点特性并不适用于动态决定节点名的场景。



## 自定义组件扩展 <a id="&#x81EA;&#x5B9A;&#x4E49;&#x7EC4;&#x4EF6;&#x6269;&#x5C55;"></a>

为了更好定制自定义组件的功能，可以使用自定义组件扩展机制。从小程序基础库版本 [2.2.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始支持。

###  扩展后的效果 <a id="&#x6269;&#x5C55;&#x540E;&#x7684;&#x6548;&#x679C;"></a>

为了更好的理解扩展后的效果，先举一个例子：

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/STePQRmH7Q5H)

```text
// behavior.js
module.exports = Behavior({
  definitionFilter(defFields) {
    defFields.data.from = 'behavior'
  },
})

// component.js
Component({
  data: {
    from: 'component'
  },
  behaviors: [require('behavior.js')],
  ready() {
    console.log(this.data.from) // 此处会发现输出 behavior 而不是 component
  }
})
```

通过例子可以发现，自定义组件的扩展其实就是提供了修改自定义组件定义段的能力，上述例子就是修改了自定义组件中的 `data` 定义段里的内容。

###  使用扩展 <a id="&#x4F7F;&#x7528;&#x6269;&#x5C55;"></a>

`Behavior()` 构造器提供了新的定义段 `definitionFilter` ，用于支持自定义组件扩展。 `definitionFilter` 是一个函数，在被调用时会注入两个参数，第一个参数是使用该 behavior 的 component/behavior 的定义对象，第二个参数是该 behavior 所使用的 behavior 的 `definitionFilter` 函数列表。

以下举个例子来说明：

```text
// behavior3.js
module.exports = Behavior({
    definitionFilter(defFields, definitionFilterArr) {},
})

// behavior2.js
module.exports = Behavior({
  behaviors: [require('behavior3.js')],
  definitionFilter(defFields, definitionFilterArr) {
    // definitionFilterArr[0](defFields)
  },
})

// behavior1.js
module.exports = Behavior({
  behaviors: [require('behavior2.js')],
  definitionFilter(defFields, definitionFilterArr) {},
})

// component.js
Component({
  behaviors: [require('behavior1.js')],
})
```

上述代码中声明了1个自定义组件和3个 behavior，每个 behavior 都使用了 `definitionFilter` 定义段。那么按照声明的顺序会有如下事情发生：

1. 当进行 behavior2 的声明时就会调用 behavior3 的 `definitionFilter` 函数，其中 `defFields` 参数是 behavior2 的定义段， `definitionFilterArr` 参数即为空数组，因为 behavior3 没有使用其他的 behavior 。
2. 当进行 behavior1 的声明时就会调用 behavior2 的 `definitionFilter` 函数，其中 `defFields` 参数是 behavior1 的定义段， `definitionFilterArr` 参数是一个长度为1的数组，`definitionFilterArr[0]` 即为 behavior3 的 `definitionFilter` 函数，因为 behavior2 使用了 behavior3。用户在此处可以自行决定在进行 behavior1 的声明时要不要调用 behavior3 的 `definitionFilter` 函数，如果需要调用，在此处补充代码 `definitionFilterArr[0](defFields)` 即可，`definitionFilterArr` 参数会由基础库补充传入。
3. 同理，在进行 component 的声明时就会调用 behavior1 的 `definitionFilter` 函数。

简单概括，`definitionFilter` 函数可以理解为当 A 使用了 B 时，A 声明就会调用 B 的 `definitionFilter` 函数并传入 A 的定义对象让 B 去过滤。此时如果 B 还使用了 C 和 D ，那么 B 可以自行决定要不要调用 C 和 D 的 `definitionFilter` 函数去过滤 A 的定义对象。

**代码示例：**

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/WaqPbxmN7E1j)

###  真实案例 <a id="&#x771F;&#x5B9E;&#x6848;&#x4F8B;"></a>

下面利用扩展简单实现自定义组件的计算属性功能:

```text
// behavior.js
module.exports = Behavior({
  lifetimes: {
    created() {
      this._originalSetData = this.setData // 原始 setData
      this.setData = this._setData // 封装后的 setData
    }
  },
  definitionFilter(defFields) {
    const computed = defFields.computed || {}
    const computedKeys = Object.keys(computed)
    const computedCache = {}

    // 计算 computed
    const calcComputed = (scope, insertToData) => {
      const needUpdate = {}
      const data = defFields.data = defFields.data || {}

      for (let key of computedKeys) {
        const value = computed[key].call(scope) // 计算新值
        if (computedCache[key] !== value) needUpdate[key] = computedCache[key] = value
        if (insertToData) data[key] = needUpdate[key] // 直接插入到 data 中，初始化时才需要的操作
      }

      return needUpdate
    }

    // 重写 setData 方法
    defFields.methods = defFields.methods || {}
    defFields.methods._setData = function (data, callback) {
      const originalSetData = this._originalSetData // 原始 setData
      originalSetData.call(this, data, callback) // 做 data 的 setData
      const needUpdate = calcComputed(this) // 计算 computed
      originalSetData.call(this, needUpdate) // 做 computed 的 setData
    }

    // 初始化 computed
    calcComputed(defFields, true) // 计算 computed
  }
})
```

在组件中使用：

```text
const beh = require('./behavior.js')
Component({
  behaviors: [beh],
  data: {
    a: 0,
  },
  computed: {
    b() {
      return this.data.a + 100
    },
  },
  methods: {
    onTap() {
      this.setData({
        a: ++this.data.a,
      })
    }
  }
})
```

```text
<view>data: {{a}}</view>
<view>computed: {{b}}</view>
<button bindtap="onTap">click</button>
```

实现原理很简单，对已有的 setData 进行二次封装，在每次 setData 的时候计算出 computed 里各字段的值，然后设到 data 中，已达到计算属性的效果。

> 此实现只是作为一个简单案例来展示，请勿直接在生产环境中使用。

###  官方扩展包 <a id="&#x5B98;&#x65B9;&#x6269;&#x5C55;&#x5305;"></a>

* [computed](https://github.com/wechat-miniprogram/computed)



## 开发第三方自定义组件 <a id="&#x5F00;&#x53D1;&#x7B2C;&#x4E09;&#x65B9;&#x81EA;&#x5B9A;&#x4E49;&#x7EC4;&#x4EF6;"></a>

小程序从基础库版本 [2.2.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始支持使用 npm 安装第三方包，因此也支持开发和使用第三方自定义组件包。关于 npm 功能的详情可先阅读\[相关文档\]\(\(npm 支持\)\)。

###  准备 <a id="&#x51C6;&#x5907;"></a>

开发一个开源的自定义组件包给他人使用，首先需要明确他人是要如何使用这个包的，如果只是拷贝小程序目录下直接使用的话，可以跳过此文档。此文档中后续内容是以 npm 管理自定义组件包的前提下进行说明的。

在开发之前，要求开发者具有基础的 node.js 和 npm 相关的知识，同时需要准备好支持 npm 功能的开发者工具，[点此下载](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)。

###  下载模板 <a id="&#x4E0B;&#x8F7D;&#x6A21;&#x677F;"></a>

为了方便开发者能够快速搭建好一个可用于开发、调试、测试的自定义组件包项目，官方提供了一个[项目模板](https://github.com/wechat-miniprogram/miniprogram-custom-component)，下载使用模板的方式有三种：

* 直接从 github 上下载 zip 文件并解压。
* 直接将 github 上的仓库 clone 下来。
* 使用官方提供的命令行工具初始化项目，下面会进行介绍。

项目模板中的构建是基于 gulp + webpack 来执行的，支持开发、构建、测试等命令，详情可参阅项目模板的 [README.md](https://github.com/wechat-miniprogram/miniprogram-custom-component/blob/master/README.md) 文件。

###  命令行工具 <a id="&#x547D;&#x4EE4;&#x884C;&#x5DE5;&#x5177;"></a>

官方提供了[命令行工具](https://github.com/wechat-miniprogram/miniprogram-cli)，用于快速初始化一个项目。执行如下命令安装命令行工具：

```text
npm install -g @wechat-miniprogram/miniprogram-cli
```

然后新建一个空目录作为项目根目录，在此根目录下执行：

```text
miniprogram init --type custom-component
```

命令执行完毕后会发现项目根目录下生成了许多文件，这是根据官方的[项目模板](https://github.com/wechat-miniprogram/miniprogram-custom-component)生成的完整项目，之后开发者可直接在此之上进行开发修改。

命令行工具的更多用法可以查看 github 仓库上的 [README.md](https://github.com/wechat-miniprogram/miniprogram-cli/blob/master/README.md) 文件。

> PS：第一次使用 `miniprogram init` 初始化项目会去 github 上拉取模板，因此需要保证网络畅通。

###  测试工具 <a id="&#x6D4B;&#x8BD5;&#x5DE5;&#x5177;"></a>

针对自定义组件的单元测试，可参阅文档[单元测试](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/unit-test.html)。

###  自定义组件示例 <a id="&#x81EA;&#x5B9A;&#x4E49;&#x7EC4;&#x4EF6;&#x793A;&#x4F8B;"></a>

以下为官方提供的自定义组件，可以参考并使用：

* [weui-miniprogram](https://github.com/wechat-miniprogram/weui-miniprogram)
* [recycle-view](https://github.com/wechat-miniprogram/recycle-view)

###  自定义组件扩展示例 <a id="&#x81EA;&#x5B9A;&#x4E49;&#x7EC4;&#x4EF6;&#x6269;&#x5C55;&#x793A;&#x4F8B;"></a>

以下为官方提供的自定义组件扩展，可以参考并使用：

* [computed](https://github.com/wechat-miniprogram/computed)

## 单元测试 <a id="&#x5355;&#x5143;&#x6D4B;&#x8BD5;"></a>

在编写高质量的自定义组件过程中，单元测试是永远避不开的一个话题。完善的测试用例是提高自定义组件可用性的保证，同时测试代码覆盖率也是必不可少的一个环节。小程序从基础库版本 [2.2.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始拥抱开源，支持使用 npm 安装自定义组件，那针对自定义组件的单元测试也是必须支持的。

以下就来介绍如何对自定义组件进行单元测试。

###  测试框架 <a id="&#x6D4B;&#x8BD5;&#x6846;&#x67B6;"></a>

现在市面上流行的测试框架均可使用，只要它能兼顾 nodejs 端和 dom 环境。因为我们需要依赖到 nodejs 的一些库来完善测试环境，同时 dom 环境也是必须的，因为我们需要建成完整的 dom 树结构，才能更好的模拟自定义组件的运行。例如可以选用 mocha + jsdom 的组合，亦可选用 jest，下述例子选用 jest 作为测试框架来说明。

###  自定义组件测试工具集 <a id="&#x81EA;&#x5B9A;&#x4E49;&#x7EC4;&#x4EF6;&#x6D4B;&#x8BD5;&#x5DE5;&#x5177;&#x96C6;"></a>

小程序的运行环境比较特殊，不同于常见的浏览器环境，它采用的是双线程的架构。而在进行单元测试时，我们并不需要用到这样复杂的架构带来的利好，我们进行的是功能测试而无需苛求性能、安全等因素，因此我们提供了一个测试工具集以支持自定义组件在 nodejs 单线程中也能运行起来。

我们先安装一下测试工具集——[miniprogram-simulate](https://github.com/wechat-miniprogram/miniprogram-simulate)：

```text
npm i --save-dev miniprogram-simulate
```

###  编写测试用例 <a id="&#x7F16;&#x5199;&#x6D4B;&#x8BD5;&#x7528;&#x4F8B;"></a>

假设我们有如下自定义组件：

```text
<!-- /components/index.wmxl -->
<view class="index">{{prop}}</view>
```

```text
// /components/index.js
Component({
  properties: {
    prop: {
      type: String,
      value: 'index.properties'
    },
  },
})
```

```text
/* /components/index.wxss */
.index {
  color: green;
}
```

我们想要测试渲染的结果，可以按照如下方式编写测试用例：

```text
// /test/components/index.test.js
const simulate = require('miniprogram-simulate')

test('components/index', () => {
    const id = simulate.load('/components/index') // 此处必须传入绝对路径
    const comp = simulate.render(id) // 渲染成自定义组件树实例

    const parent = document.createElement('parent-wrapper') // 创建父亲节点
    comp.attach(parent) // attach 到父亲节点上，此时会触发自定义组件的 attached 钩子

    const view = comp.querySelector('.index') // 获取子组件 view
    expect(view.dom.innerHTML).toBe('index.properties') // 测试渲染结果
    expect(window.getComputedStyle(view.dom).color).toBe('green') // 测试渲染结果
})
```

> PS：测试工具集中的 wx 对象和内置组件都不会实现真正的功能，如果需要测试一些特殊场景的话，可以自行覆盖掉测试工具集中的 api 接口和内置组件。
>
> PS：目前因为有部分自定义组件功能仍未支持（如抽象节点等），故测试工具暂无法全部覆盖自定义组件的特性，后续会继续完善。

测试工具集中提供了一些方便测试的接口，比如：

* 模拟 touch 事件、自定义事件触发
* 选取子节点
* 更新自定义组件数据
* 触发生命周期
* ...

更多详细的用法可以参阅 [github 仓库](https://github.com/wechat-miniprogram/miniprogram-simulate)上的文档

