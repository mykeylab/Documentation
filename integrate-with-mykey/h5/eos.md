# EOS

**特别注意:** [MYKEY的账号体系](../../dive-into-mykey/mykey-on-eos.md#mykey帐户结构)与其他的EOS账号有所差异，需要在服务端验签时使用**Reserved公钥**进行验签，详细请查阅[文档](../../dive-into-mykey/mykey-on-eos.md#2-dui-yu-yu-scatter-jian-rong-de-dapp)。

## 兼容Scatter

MYKEY兼容Scatter协议，您可以直接开发兼容Scatter协议的dapp，再通过mykey内置浏览器访问。也可以参考以下链接，了解更多Scatter协议的内容：

[https://get-scatter.com/developers/settingupforwebapps](https://get-scatter.com/developers/settingupforwebapps)

## MYKEY验签方式

使用[authorize](../integration-android/sign.md)方法，MYKEY生成签名信息，返回用户的账户与签名信息，dapp服务端负责验签。

