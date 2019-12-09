# MYKEY\_JSBridge

JSBridge 为MYKEY应用中心内嵌的浏览器环境中默认支持的JS注入库，其支持Scatter协议，开发者可以使用[Scatter文档](https://get-scatter.com/docs/api-reference)进行H5 DApp的开发。在支持以太坊后也会支持web3协议。

**特别注意:** [MYKEY的账号体系](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#mykey-account-structure)与其他的EOS账号有所差异，需要在服务端验签时使用Reserved公钥进行验签，详细请查阅[文档](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#if-dapp-dependents-on-getarbitrarysignature-or-other-server-side-authentication)

此外为了方便控制MYKEY应用中心浏览器的一些额外属性，如全屏，签名弹框方向，关闭应用等

## 开启全屏

进入全屏模式，应用可以通过isLandscape指定签名弹出框的方向

```text
// isLandscape: 全屏模式中MYKEY签名弹出框的方向, ture为横屏，false为竖屏
window.MyKey.Browser.openFullScreen(isLandscape)
```

## 退出全屏

非全屏模式下，MYKEY签名弹出框永远为竖屏状态

```text
window.MyKey.Browser.closeFullScreen()
```

## 退出

退出DApp,销毁当前DApp窗口

```text
window.MyKey.Browser.closeWindow()
```

## 禁用返回

禁用系统原生返回按钮

```text
window.MyKey.Browser.forbidPhysicalBack()
```

## 允许返回

允许系统原生返回按钮

```text
window.MyKey.Browser.allowPhysicalBack()
```

