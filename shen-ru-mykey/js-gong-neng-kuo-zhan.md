# JS功能扩展

JSBridge为MYKEY应用中心内嵌的浏览器环境中默认支持的JS注入库，其支持Scatter协议，开发者可以使用[Scatter文档](https://get-scatter.com/docs/api-reference)进行H5 DApp的开发。在支持以太坊后也会支持web3协议。

**特别注意:**[MYKEY的账号体系](eos-shang-de-mykey.md#mykey帐户结构)与其他的EOS账号有所差异，需要在服务端验签时使用Reserved公钥进行验签，详细请查阅[文档](eos-shang-de-mykey.md#2-dui-yu-yu-scatter-jian-rong-de-dapp)

此外为了方便控制MYKEY应用中心浏览器的一些额外属性，如全屏，签名弹框方向，关闭应用等

## 开启全屏 <a id="&#x5F00;&#x542F;&#x5168;&#x5C4F;"></a>

进入全屏模式，应用可以通过isLandscape指定签名弹出框的方向

```java
// isLandscape: 全屏模式中MYKEY签名弹出框的方向, ture为横屏，false为竖屏
window.MyKey.Browser.openFullScreen(isLandscape)
```

## 退出全屏 <a id="&#x9000;&#x51FA;&#x5168;&#x5C4F;"></a>

非全屏模式下，MYKEY签名弹出框永远为竖屏状态

```java
window.MyKey.Browser.closeFullScreen()
```

## 退出 <a id="&#x9000;&#x51FA;"></a>

退出DApp,销毁当前DApp窗口

```java
window.MyKey.Browser.closeWindow()
```

## 禁用返回 <a id="&#x7981;&#x7528;&#x8FD4;&#x56DE;"></a>

禁用系统原生返回按钮

```java
window.MyKey.Browser.forbidPhysicalBack()
```

## 允许返回 <a id="&#x5141;&#x8BB8;&#x8FD4;&#x56DE;"></a>

允许系统原生返回按钮

```java
window.MyKey.Browser.allowPhysicalBack()
```

