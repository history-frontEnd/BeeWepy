# BeeWepy

BeeWepy是基于[wepy](https://github.com/Tencent/wepy)的一套小程序开发模板，有一些方便的设置。

## 几个约定：
- 代码风格规范请使用 [JavaScript Standard Style](https://github.com/feross/standard)
- Less mixins请使用 [lesshat](https://github.com/madebysource/lesshat#size)
- 命名规范使用驼峰命名方式，且命名尽量能够顾名思义，如：变量名`userInfo`，方法名`getUserInfo`，类名、组件名`UserInfo`

## 全局inject

- $link: 注册为wepy.page的页面跳转方式
```javascript
  this.$link('/page/line/itemDetail')
```

- $back: 返回上一页，非跳转
```javascript
  this.$back()
```

- $track: 埋点(未实现)
```javascript
  this.$track('page_index_click?a=b')
```

- $toast：吐司提示
```javascript
  this.$toast('成功了')
```

- $loading：正在加载提示
```javascript
  this.$loading() // 显示
  this.$loading(false) //隐藏
```

- $modal: 模态框
```javascript
  let resp = await this.$modal('确定？', '子标题', true)
```

- $getSystemInfo: 获取用户手机信息
```javascript
  let resp = await this.$getSystemInfo()
```

- $db: 同步方式获取以及设置storage
```javascript
  this.$db.get('name')
  this.$db.set('name', '子标题')
```

- $d/$debug: debug消息
```javascript
  this.$d('消息')
  this.$debug('消息')
```

## Api相关

- 在`config/index.js`中定义是需要全局mock数据(isMock),也可以在特定的请求中覆盖,生产环境自动覆盖。isMock决定是否使用mock数据，当`isMock=true`时根据`src/mock/mockConfig.js`的设置获取mock数据，当`isMock=false`时会发送网络请求，并且请求体会删除`isMock`字段。
```javascript
let requestData = {
  isMock: false,
  mobile: this.monPhone
}
await this.POST('/userregistermodify', requestData)
```

- 不明确定义`usertoken`,请求中会默认带上localstorage中的`usertoken`,一般不需要自带`usertoken`
```javascript
let requestData = {
  usertoken: this.$parent.globalData.token
  mobile: this.monPhone
}
await this.POST('/userregistermodify', requestData)
```

- 请求url以http或才https开头，不会拼接`domain`, mockConfig中定义key时使用全url
```javascript
//url为 domain + /userregistermodify
const resp = await this.POST('/userregistermodify', requestData)
//url为 http://www.baidu.com/userregistermodify
const resp = await this.POST('http://www.baidu.com/userregistermodify', requestData)
```

- 参数 `showToast`
默认为`false`时, 调用`wepy.showNavigationBarLoading()`, 为`true`时, 调用`wepy.showLoading()`

## 踩坑:

- 使用wepy-cli 生成项目，运行后报: `Error: module "npm/lodash/_nodeUtil.js" is not defined`

解决方法：`npm i util --no-save && wepy build --no-cache`

https://github.com/Tencent/wepy/issues/1294
不保存依赖,安装util,同时 不使用缓存构建

- 微信开发者工具打开dist运行时报TypeError: Cannot read property 'Promise' of undefined

解决方法：
> 微信开发者工具-->项目-->关闭ES6转ES5。重要：漏掉此项会运行报错。
微信开发者工具-->项目-->关闭上传代码时样式自动补全 重要：某些情况下漏掉此项会也会运行报错。
微信开发者工具-->项目-->关闭代码压缩上传 重要：开启后，会导致真机`computed`, `props.sync` 等等属性失效。（参考[开发者工具编译报错](https://github.com/Tencent/wepy/issues/273)）


## 相关文档：
- [小程序开发文档](https://developers.weixin.qq.com/miniprogram/dev/)
- [wepy文档](https://tencent.github.io/wepy/)
