# Mobile H5页面接入

JSBridge 为MYKEY应用中心内嵌的浏览器环境中默认支持的JS注入库，支持web3协议；也支持Scatter协议。

您可以直接开发兼容web3协议和Scatter协议的H5页面，再通过mykey内置浏览器访问。

更多eth web3协议的内容请参考：[https://web3js.readthedocs.io/en/v1.2.0/web3-eth.html](https://web3js.readthedocs.io/en/v1.2.0/web3-eth.html)

更多Scatter协议的内容请参考：[https://get-scatter.com/developers/settingupforwebapps](https://get-scatter.com/developers/settingupforwebapps)

服务端验证MYKEY签名请参考 [验签代码示例](../../sign-in-with-mykey/verify-example.md)。JSBridge支持的更多方法，请参考[JS功能扩展](js-extensions.md)。

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



