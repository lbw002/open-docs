

# 简介
针对小程序不能外跳 H5 页面，提供 web-view 页面承载组件将 H5 嵌套进小程序，实现在小程序内打开 H5 页面。小程序跳转相关内容请参见 [小程序跳转 FAQ](https://opendocs.alipay.com/mini/api/xqvxl4)，调试页面访问受限解决方法请参见 [web-view 调试报错页面访问受限处理方法](https://opendocs.alipay.com/mini/component/access)，开发过程中遇到的问题可参考 [web-view 常见问题](/mini/component/mg7rvg)，涵盖了web-view 与 H5 、web-view 页面设计相关、开放能力等方面。

## 使用限制

- 版本要求支付宝客户端 10.1.35 或更高版本。
- 每个页面只能有一个 web-view ，请不要渲染多个 web-view ，会自动铺满整个页面，并覆盖其它组件。
- 不支持个人小程序使用，仅支持企业小程序。
- 不支持添加阿里域名（天猫、淘宝等）到白名单且域名添加数量不超过 20 个。
- 不支持多个页面 web-view 间通讯。不支持横屏以及全屏展示。
- 不支持 pushWindow。
- 不支持嵌套授权逻辑。web-view 跳到 H5 的白名单链接如需嵌套第三方的 iframe，可以把嵌入的三方 url 也添加进h5域名白名单。
- 支持 web-view 的 postMessage 传递多个参数。
- URL 传参中除 ASCLL 字母、数字、~!* ()'以外的字符（如中文字符），请使用 encodeURIComponent() 函数进行编码，及使用 decodeURIComponent() 函数进行解码。
- 调试请以真机效果为准。
- 使用该组件之前，请确保 H5 页面中所有的域名地址（含静态资源地址，例如图片、.js 文件地址等）已经加入到 web-view 的 H5 域名白名单内。如未添加，参见 [**配置 H5 域名**](https://opendocs.alipay.com/mini/component/idfvg6) 进行添加。
- 域名添加或删除后仅对新版本生效，老版本仍使用修改前的域名配置。

## 扫码体验
![](https://gw.alipayobjects.com/mdn/miniapp_de/afts/img/A*iCMFTpfHrxoAAAAAAAAAAABjARQnAQ#align=left&display=inline&height=1906&margin=%5Bobject%20Object%5D&originHeight=1906&originWidth=1540&status=done&style=none&width=128)

## 效果示例
![](https://gw.alipayobjects.com/zos/skylark-tools/public/files/30af88e3aee4fad81364495deabe6307.png#align=left&display=inline&height=720&margin=%5Bobject%20Object%5D&originHeight=720&originWidth=1280&status=done&style=none&width=1280)

# 使用

## 示例代码

### .axml 示例代码
```html
<!-- API-DEMO page/component/webview/webview.axml -->
<view class="page">
  <web-view src="https://render.alipay.com/p/s/web-view/index" onMessage="onmessage"></web-view>
</view>
```

### .js 示例代码
```javascript
// API-DEMO page/component/webview/webview.js
Page({
  data: {},
   onShareAppMessage(options) {
    my.alert({content:JSON.stringify(options.webViewUrl)});
    return {
      title: '分享 web-View 组件',
      desc: 'View 组件很通用',
      path: 'page/component/component-pages/webview/baidu',
      'web-view': options.webViewUrl,
    };
  },
  onmessage(e){
    my.alert({
      content: '拿到数据'+JSON.stringify(e), // alert 框的标题
    });
  }
});
```

## 属性说明
| **属性** | **类型** | **描述** |
| --- | --- | --- |
| src | String | web-view 要渲染的 H5 网页 URL ，需要在如下路径中 **支付宝小程序管理中心 **>** 设置 **>** 开发设置 **> **H5域名配置** 进行 H5 域名白名单配置。请参见 [**配置 H5 白名单流程**](https://opendocs.alipay.com/mini/component/idfvg6)**。** |
| onMessage | EventHandle | 网页向小程序 postMessage 消息。`e.detail = { data }` |
| onLoad | EventHandle | 网页加载成功时触发此事件。`e.detail = { src }`<br />**版本要求**：基础库 [2.7.3](https://opendocs.alipay.com/mini/framework/lib-upgrade-v2) 及以上 |
| onError | EventHandle | 网页加载失败时触发此事件。`e.detail = { src }`<br />**版本要求**：基础库 [2.7.3](https://opendocs.alipay.com/mini/framework/lib-upgrade-v2) 及以上 |

可以通过判断 `userAgent` 中包含 `miniProgram` 字样来判断小程序 web-view 环境。

### 可用 API
web-view 载入的 H5 页面可以使用手动引入 https://appx/web-view.min.js（此链接仅支持在支付宝客户端内访问），提供了相关的 API 供您使用（**调试请以真机效果为准**）。

**说明：** 如需嵌入 H5 页面请使用表格中支持的 API，表格中如不支持请调用原生 js。

| **接口类别** | **接口名** | **描述** |
| --- | --- | --- |
| 导航栏 | [my.navigateTo](/mini/api/zwi8gx) | 保留当前页面，跳转到应用内的某个指定页面。 |
| 导航栏 | [my.navigateBack](/mini/api/kc5zbx) | 关闭当前页面，返回上一级或多级页面。 |
| 导航栏 | [my.switchTab](/mini/api/ui-tabbar) | 跳转到指定 tabBar 页面，并关闭其他所有非 tabBar 页面。 |
| 导航栏 | [my.reLaunch](/mini/api/hmn54z) | 关闭当前所有页面，跳转到应用内的某个指定页面。 |
| 导航栏 | [my.redirectTo](/mini/api/fh18ky) | 关闭当前页面，跳转到应用内的某个指定页面。 |
| 图片 | [my.chooseImage](https://opendocs.alipay.com/mini/api/media/image/my.chooseimage) | 拍照或从手机相册中选择图片（可将获取到的图片路径通过 `my.postMessage()` 将相关数据传递给小程序后进行图片上传）。 |
| 图片 | [my.previewImage](https://opendocs.alipay.com/mini/api/media/image/my.previewimage) | 预览图片。 |
| 位置 | [my.getLocation](/mini/api/mkxuqd) | 获取用户当前的地理位置信息。 |
| 位置 | [my.openLocation](/mini/api/as9kin) | 使用支付宝内置地图查看位置。 |
| 交互反馈 | [my.alert](/mini/api/ui-feedback) | 警告框。 |
| 交互反馈 | [my.showLoading](/mini/api/bm69kb) | 显示加载提示。 |
| 交互反馈 | [my.hideLoading](/mini/api/nzf540) | 隐藏加载提示。 |
| 缓存 | [my.setStorage](/mini/api/eocm6v) | 将数据存储在本地缓存中指定的 key 中，会覆盖掉原来该 key 对应的数据。 |
| 缓存 | [my.getStorage](/mini/api/azfobl) | 获取缓存数据。 |
| 缓存 | [my.removeStorage](/mini/api/of9hze) | 删除缓存数据。 |
| 缓存 | [my.clearStorage](/mini/api/storage) | 清除本地数据缓存。 |
| 缓存 | [my.getStorageInfo](/mini/api/zvmanq) | 异步获取当前缓存的相关信息。 |
| 网络状态 | [my.getNetworkType](/mini/api/network-status) | 获取当前网络状态。 |
| 分享 | my.startShare | 分享当前页面,当执行my.startShare() 时会唤起当前小程序页面的分享功能。 |
| 唤起支付 | [my.tradePay](/mini/api/openapi-pay) | 唤起支付（仅支持使用该 API 唤起支付，不支持使用 H5 进行支付） |
| 向小程序发送消息 | my.postMessage | 向小程序发送消息，自定义一组或多组 key 、 value 数据，格式为 JSON ，如：`my.postMessage({name:"测试web-view"})`。 |
| 监听小程序发过来的消息 | my.onMessage | 监听小程序发过来的消息， [webview组件控制](/mini/api/webview-context)。 |
| 获取当前环境 | my.getEnv | 获取当前环境。 |


### 示例代码
web-view  H5 页面代码：
```javascript
<script type="text/javascript" src="https://appx/web-view.min.js"></script>
<!-- 如该 H5 页面需要同时在非支付宝客户端内使用，为避免该请求404，可参考以下写法 -->
<!-- 请尽量在 html 头部执行以下脚本 -->
<script>
  if (navigator.userAgent.indexOf('AliApp') > -1) {
    document.writeln('<script src="https://appx/web-view.min.js"' + '>' + '<' + '/' + 'script>');
  }
</script>
<script>
  my.navigateTo({url: '../get-user-info/get-user-info'});
  // 网页向小程序 postMessage 消息
  my.postMessage({name:"测试web-view"});
  // 接收来自小程序的消息。
  my.onMessage = function(e) {
    console.log(e); // {'sendToWebView': '1'}
  }
  // 判断是否运行在小程序环境里
  my.getEnv(function(res) {
    console.log(res.miniprogram) // true
  });
  my.startShare();
</script>
```
my.postMessage 信息发送后，小程序页面接收信息时，会执行 onMessage 配置的方法：
```html
<!-- .axml -->
<view>
  <web-view id="web-view-1" src="..." onMessage="test"></web-view>
</view>
```


```javascript
// 小程序页面对应的 page.js 声明 test 方法，
// 由于 page.axml 里的 web-view 组件设置了 onMessage="test",
// 当页面里执行完 my.postMessage 后，test 方法会被执行
Page({
  onLoad(e){
    this.webViewContext = my.createWebViewContext('web-view-1');    
  },
  test(e){
    my.alert({
      content:JSON.stringify(e.detail),
    });  
    this.webViewContext.postMessage({'sendToWebView': '1'});
  },
});
```
用户分享时可获取当前 web-view 的 URL ，即在 onShareAppMessage 回调中返回 webViewUrl 参数。

```javascript
Page({
  onShareAppMessage(options) {
    console.log(options.webViewUrl)
  }
});
```

# 相关文档

- [web-view 常见问题](/mini/component/mg7rvg)<br />
- [小程序跳转 FAQ](https://opendocs.alipay.com/mini/api/xqvxl4)<br />
- [配置 H5 白名单流程](https://opendocs.alipay.com/mini/component/idfvg6)<br />
- [web-view 组件控制 my.createWebViewContext](https://opendocs.alipay.com/mini/api/webview-context)<br />
