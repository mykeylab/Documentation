# Mobile H5页面接入

## 在MYKEY中两种H5的开发方式

#### 原生方式

如果你对区块链应用开发有经验，你可以直接开发支持web3协议\(ETH\)和Scatter协议\(EOS\)的H5页面。这种方式的优势是，你开发的应用在任何支持该类协议的钱包和浏览器中都可以打开。

更多ETH web3协议的内容请参考：[https://web3js.readthedocs.io/en/v1.2.0/web3-eth.html](https://web3js.readthedocs.io/en/v1.2.0/web3-eth.html)

更多Scatter协议的内容请参考：[https://get-scatter.com/developers/settingupforwebapps](https://get-scatter.com/developers/settingupforwebapps)

#### MYKEY 专有方式

开发者也可以直接使用MYKEY提供的一系列包装接口进行应用开发，MYKEY应用中心内嵌的浏览器环境中默认支持的JS注入库，对不同链的各种协议进行了包装，同时支持了web3和Scatter等协议。这类接口还同时支持MYKEY内部中心化托管资产\(BTC, DOT\)的交易。

本方式的特点是开发的应用只支持在MYKEY环境中使用，但是开发难度较低。

脚手架参考代码示例：

[https://github.com/mykeylab/dapp\_react\_boilerplate](https://github.com/mykeylab/dapp_react_boilerplate)

更多方法，请参考[JS功能扩展](js-extensions.md)。

## 特殊的验签方式

由于MYKEY是一个智能钱包，有别与传统的公私钥验签模式。服务端验证MYKEY签名请参考 [验签代码示例](../../sign-in-with-mykey/verify-example.md)。

## 如何识别是MYKEY打开的应用

有两种方法可以检查DAPP是否在MYKEY客户端中运行。

1. 使用navigator.userAgent进行检查。MYKEY Webview将在原userAgent之后附加MYKEY / \[version\]。

```java
function isMYKEY(){
    return navigator.userAgent.indexOf("MYKEY") > -1;
}
```

2. 对于EOS，可使用scatter.getIdentity检查响应数据是否包含类型："MYKEY"

```java
{
    accounts: [{authority: 'active', blockchain: 'eos', name: [ACCOUNT_NAME], type: 'MYKEY'}],
    publicKey: [OperationKey 3/Reserved key]
};
```



\*\*\*\*



