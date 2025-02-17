
# 简介
**my.compressImage** 是压缩图片的 API。

## 使用限制

- 基础库 [1.4.0](https://opendocs.alipay.com/mini/framework/lib) 或更高版本；支付宝客户端  10.1.8 或更高版本 ，若版本较低，建议采取 [兼容处理](https://opendocs.alipay.com/mini/framework/compatibility)。
- 此 API 支持个人支付宝小程序、企业支付宝小程序使用。

## 扫码体验
![|127x157](https://cdn.nlark.com/yuque/0/2021/png/179989/1625190959093-f5836ae3-634e-4880-8594-2c8d4f0c603e.png#align=left&display=inline&height=157&margin=%5Bobject%20Object%5D&name=1.png&originHeight=157&originWidth=127&size=13508&status=done&style=stroke&width=127)

## 效果示例
![|723x407](https://cdn.nlark.com/yuque/0/2021/png/179989/1625190968147-0a952a95-d39e-438d-8cc5-4595113f6b8f.png#align=left&display=inline&height=576&margin=%5Bobject%20Object%5D&name=2.png&originHeight=720&originWidth=1280&size=33056&status=done&style=stroke&width=1024)

# 接口调用

## 示例代码

### .axml 示例代码
```html
<!-- API-DEMO page/API/compress-image/compress-image.axml-->
<view class="page">
  <view class="page-description">压缩图片 API</view>
  <view class="page-section">
    <view class="page-section-title">my.compressImage</view>
    <view class="page-section-demo">
      <button type="primary" onTap="selectImage" hover-class="defaultTap">选择图片</button>
      <image
        src="{{compressedSrc}}" 
        mode="{{mode}}" />
    </view>
  </view>
</view>
```

### .js 示例代码
```javascript
// API-DEMO page/API/compress-image/compress-image.js
Page({
  data: {
    compressedSrc: '',
    mode: 'aspectFit',
  },
  selectImage() {
    my.chooseImage({
      count: 1,
      success: (res) => {
        my.compressImage({
          apFilePaths: res.apFilePaths,
          compressLevel: 1,
          success: data => {
            console.log(data);
            this.setData({
              compressedSrc: data.apFilePaths[0],
            })
          }
        })
      }
    })
  },
});
```

## 入参
Object 类型，属性如下：

| **属性** | **类型** | **必填** | **描述** |
| --- | --- | --- | --- |
| apFilePaths | String Array | 是 | 要压缩的图片地址数组。 |
| compressLevel | Number | 否 | 压缩级别，支持 0 ~ 4 的整数，默认为 4。详见下文 compressLevel 表说明。 |
| success | Function | 否 | 调用成功的回调函数。 |
| fail | Function | 否 | 调用失败的回调函数。 |
| complete | Function | 否 | 调用结束的回调函数（调用成功、失败都会执行）。 |


### success 回调函数
| **属性** | **类型** | **描述** |
| --- | --- | --- |
| apFilePaths | String Array | 压缩后的路径数组。 |


### compressLevel 表
| **compressLevel** | **描述** |
| --- | --- |
| 0 | 低质量 |
| 1 | 中等质量 |
| 2 | 高质量 |
| 3 | 不压缩 |
| 4 | 根据网络适应 |

