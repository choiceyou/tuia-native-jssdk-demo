# 推啊（短链接）JS 说明文档

## sdk基础信息

**SDK 大小:** SDK根据版本不同大小在200K ～ 260K

**SDK 抓取:** 只上报入参信息，不抓取额外信息（不具备用户信息抓取功能）

**SDK 代码:** 完全透明，代码可通过浏览器调试工具查看。


## 概述

通过使用推啊（短链接）JS，媒体可在自身 H5 页面内载入推啊互动活动。

## 使用说明

> 步骤零：确认是否可用
请确保webview支持ES5语法，并且支持iframe。

> 步骤一：引入 JS 文件

在需要初始化 JS 的页面上引入如下 JS 文件，(支持 https): http://yun.tuisnake.com/h5-mami/short-link/v1.1.1/short-link.min.js

> 步骤二：通过 init 接口初始化

如果需要使用 JS 的方法，使用的页面必须先初始化，否则将无法调用（请使用合法的有效参数）

```
Tsdk.init({
  appKey: '加密的appid',  //  系统分配 (在推啊后台‘我的媒体’获取appkey)
  slotId: '10000',       //  系统分配的广告位Id (在推啊后台‘我的广告位’获取slotId) 
  deviceId: '123456',    //  android传入imea号，ios传idfa，若无法获取请传入userId
  dom: '#index',         //  dom节点（该dom节点必须要有宽高，确保在调用方法是该dom节点已存在与页面上)
  debug: false,          //  选择是否开启debug模式
  userId: 'fgdfgdfg'     //  用户id，用于对接虚拟奖品，确定用户身份
})
```

> 步骤三：通过 ready 接口处理成功验证

```
Tsdk.ready(function() {
  // 如果需要在页面加载时就调用相关接口，则须把相关接口放在ready函数中调用来确保正确执行。
  // 对于用户触发时才调用的接口，则可以直接调用，不需要放在ready函数中。
})
```

> 步骤四：监听 error 事件处理执行过程中的错误信息

```
Tsdk.on('error', function(err) {
  // 在里面处理err
})

> 步骤五：监听 tuia:close 事件，处理活动关闭之后的事情

```
Tsdk.on('tuia:close', function(err) {
  // 处理关闭之后的业务代码
})
```

## 接口说明

> init

用于 sdk 初始化

```
Tsdk.init({
  appKey: '加密的appid',
  slotId: '10000',
  deviceId: '123456',
  dom: '#index',
  debug: false,
  userId: 'fgdfgdfg'
})
```

| 参数名 | 必填 | 类型   | 示例值    | 描述               |
| ------ | -- | ------ | --------- | ------------------ |
| appKey |  是  | string | iamappkey | 系统分配 (在推啊后台‘我的媒体’获取appkey) |
| slotId |  是  | string | iamappkey | 系统分配的广告位Id (在推啊后台‘我的广告位’获取slotId) |
| deviceId |  是  | string | iamappkey | android传入imea号，ios传idfa，若无法获取请传入userId |
| dom | 是 | string | '.dom' | 媒体传入挂载点 |
| debug |  否  | boolean | false | 是否开启debug模式 |
| userId |  是  | string | 'dgfgdf' | 媒体用户id |


说明：
  deviceId为设备号，在为空的情况下，会自动根据规则生成。
  userId为媒体的自身用户，用于对接虚拟奖品，确定用户身份。
  (如果用户体系中只存在userId或者deviceId, 两者传入相同的userId或者deviceId即可)

## 事件说明

> showError

```
用于上报错误信息
```
| 参数名 | 必填 | 类型   | 示例值    | 描述               |
| ------ | :--: | ------ | --------- | ------------------ |
| type | 是 | string | iamappkey | 错误类型 |
| code | 是 | string | iamappkey | 错误状态码 |

```
主要错误码：
'0000000': '成功';
'0000001': '参数缺失或无效';
'1000001': 'appKey 错误或者不存在';
'1000002': 'slotId 错误或者不存在';
'1000003': 'token 过期';
'1000004': '订单不存在';
'1000005': '系统错误';
'9999999': '发生未知错误';
```
> ready

```
JS 在初始化完毕的时候会触发
```
如果需要在页面加载时就调用相关接口，则须把相关接口放在ready函数中调用来确保正确执行。对于用户触发时才调用的接口，则可以直接调用，不需要放在ready函数中。

> tuia:close

```
活动的关闭按钮权限我们将要回收，媒体不需要提供关闭按钮，我们会提供统一的关闭按钮，
请监听Tsdk.on('tuia:close', ()=>{} // 回调函数)，在关闭活动的同时，我们会调用传入的方法。

```

## 额外的JS API
如果在引用JSSDK的页面注入jsapi, 在特定的时候优先去调用它。（该方法需要媒体注入在页面内，供JSSDK调用）

1、需要新打开页面时（例如：跳转我的奖品页，跳转广告落地页）。
```
  window.TAHandler.openUrl(JSON)
    JSON <string> "{'url': '//www.tuia.cn'}"
```

## 常见错误及解决方法
在jssdk中报错会触发error事件，
```
Tsdk.on('error', function(err) {
  // 这里会将错误信息捕获
})
```
调用 init 接口的时候传入参数 debug: true 可以开启 debug 模式，页面会出现调试工具。
debug所需模块会异步加载到页面。
