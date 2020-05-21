---
description: 一种类似于OpenID的区块链第三方授权登录方案
---

# 基于MYKEY的第三方授权登录

MYKEY登录，即通过MYKEY账号登录APP和网站，同时享受MYKEY账号的好处：免费使用，支持多链。

目前MYKEY支持ETH和EOS两条主流公链。有的用户只开通了一条公链，也就是只有一条链上的账号和公钥地址；有的用户开通了两条公链，授权登录时，可选择任意一条公链来签名和验证签名。

MYKEY传给第三方应用的数据包括：用户的MYKEY ID，区块链账号及对应的公钥地址。

MYKEY授权登录，**类似于传统互联网的OpenID技术**，第三方应用通过MYKEY ID可识别用户，通过区块链上的账号可获取到用户的余额信息，通过签名和验证签名来确定用户拥有账号的操作权限。

## 第三方授权登录流程

不同的第三方应用，使用MYKEY作为第三方登录的方法也不同。

{% tabs %}
{% tab title="H5页面" %}
H5页面，兼容Scatter或者Web3协议，即可获取到用户的MYKEYID和区块链账号信息，完成登录。

以某去中心化交易所的H5页面为例：

{% embed url="https://v.qq.com/x/page/h0969b16hpd.html?start=0" %}

接入步骤：

a. 针对Scatter协议，先使用scatter.connect方法，再使用login方法即可获得用户的账号信息，示例代码见：[H5 EOS登录](../integrate-with-mykey/h5/h5-eos.md#deng-lu)

b. 针对Web3协议，使用web3.eth的givenProvider方法即可获得用户的账号信息，示例代码见：[H5 ETH登录](../integrate-with-mykey/h5/h5-eth.md#deng-lu)
{% endtab %}

{% tab title="Web应用" %}
Web应用遵守SimpleWallet协议（EOS），或者WalletConnect协议（ETH），MYKEY通过扫描二维码，会自动识别协议，完成授权登录。

以某去中心化交易所使用MYKEY扫码登录为例：

{% embed url="https://v.qq.com/x/page/w0970u75ocr.html?start=1" %}

接入流程：

1. Web应用根据需要传递的JSON数据，生成二维码。要传递的数据结构见 [Web应用扫码登录](../integrate-with-mykey/scan.md#deng-lu)。
2. MYKEY用户扫描二维码，授权登录。
3. MYKEY回调Web应用。
{% endtab %}

{% tab title="Android应用" %}
Android应用，集成[MYKEY Android SDK](https://github.com/mykeylab/MYKEY-Client-SDK/tree/master/Android)，调用SDK的authorize方法，即可完成登录。

以某大型区块链游戏全民炮战为例：

{% embed url="https://v.qq.com/x/page/d09704geqm2.html?start=1" %}

接入流程：

1. 环境准备，添加MYKEY Android SDK
2. 使用initSimple方法初始化SDK
3. 使用authorize方法授权登录
4. APP收到用户的ID和区块链上的地址
5. （可选）APP根据用户账号从公链上查询到地址，并进行签名验证，也称为验签。针对不同的编程语言，我们提供了多种[验签示例代码](verify-example.md)。
{% endtab %}

{% tab title="iOS应用" %}
iOS应用，集成[MYKEY iOS SDK](https://github.com/mykeylab/MYKEY-Client-SDK/tree/master/iOS)，调用SDK的authorize方法，即可完成登录。

接入流程：

1. 环境准备，添加MYKEY iOS SDK
2. 使用initSimple方法初始化SDK
3. 使用authorize方法授权登录
4. APP收到用户的ID和区块链上的地址
5. （可选）APP根据用户账号从公链上查询到地址，并进行签名验证，也称为验签。针对不同的编程语言，我们提供了多种[验签示例代码](verify-example.md)。
{% endtab %}
{% endtabs %}







