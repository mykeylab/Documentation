# EOS上的MYKEY

MYKEY是一个自主身份系统。底层协议为KEY ID。

本文档的目的是解释MYKEY帐户和传统EOS帐户之间的区别。 同时，文档将演示使您的代码与MYKEY App兼容的简单方法。

## MYKEY有啥不同？ <a id="mykey&#x6709;&#x5565;&#x4E0D;&#x540C;&#xFF1F;"></a>

为了改善用户体验并降低EOS使用门槛，MYKEY旨在解决基于KEY ID协议的私钥管理和资源消耗管理的问题。更多详细信息请参考[MYKEY白皮书](mykey-bai-pi-shu.md)

MYKEY通过KEY ID智能合约重新设计了EOS帐户，该EOS主网上的合约包含两部分：

* [mykeymanager](https://bloks.io/account/mykeymanager)
* [mykeylogica1](https://bloks.io/account/mykeylogica1)

在EOS中，MYKEY支持诸如密钥恢复，层级权限，免费帐户，免费使用，身份认证，子帐户等功能。为便于理解，MYKEY尝试为最终用户隐藏上述技术概念。在MYKEY APP中，用户不必关心其EOS帐户的CPU / NET / RAM。

但与此同时，用户的帐户仍然保持自主权，权限属于用户。更多细节将在后续的文档和开源计划中披露。

## MYKEY的帐户结构 <a id="mykey&#x5E10;&#x6237;&#x7ED3;&#x6784;"></a>

MYKEY帐户结构中有两种密钥：admin key（管理密钥）和operation key（操作密钥）。在KEYID智能合约中，“Admin Key”不能签署任何常规交易，只能签署带时间锁的交易来替换“Operation Key”。

![](../.gitbook/assets/account_model%20%281%29.png)

Admin Key对应的私钥由移动设备生成，可以在账户注册期间通过恢复码或硬件钱包进行冷备份。Operation Key对应的私钥由移动设备生成并加密存储。

Admin Key和Operation Key的公钥用于注册帐户，存储在KEY ID智能合约中，以进行进一步的链上签名验证。

凭借这些创新的设计，MYKEY可以帮助DAPP项目方构建自己的原生DAPP，在DAPP和第三方钱包之间都不需要显示的进行签名。 此外，用户可以通过不同的Operation Key安全地管理资产、vault帐户、DAPP帐户、数据等，这些操作密钥在一个帐户中，我们称其为“自主身份”。 _vault账户可以像普通账户一样接收资金，但也可以通过添加可选的安全步骤来防止立即撤回存储的资金_

## keydata表中的密钥 <a id="keydata&#x8868;&#x4E2D;&#x7684;&#x5BC6;&#x94A5;"></a>

MYKEY帐户的每对密钥的公钥都将存储在合约[mykeymanager](https://bloks.io/account/mykeymanager)的表keydata中，我们可以使用ACCOUNT\_NAME作为范围来查询MYKEY帐户中的所有密钥。

例如，查询帐户mykeyhulu521，

[https://eosq.app/account/mykeymanager/tables?scope=mykeyhulu521&tableName=keydata](https://eosq.app/account/mykeymanager/tables?scope=mykeyhulu521&tableName=keydata)

在注册期间，MYKEY为每个帐户预定义了四个密钥。下表中显示了详细信息:

| 索引 | 用途0 |
| :--- | :--- |
| 0 | Admin Key: 最高权限，可以管理其它 Operation Key，**不能转出资产** |
| 1 | Operation Key 1/资产密钥: 只能用于转出资产 |
| 2 | Operation Key 2/带创建功能的密钥: 用于创建新的 Operation Key |
| 3 | Operation Key 3/Reserved Key\(预留密钥\): 用于没有特定操作密钥的其它动作 |

## 将EOS DAPP与MYKEY集成

因为MYKEY对EOS帐户结构的进行了一些定制，这里有必要为DAPP开发者提供一些小技巧。

### 1. 忽略CPU和NET

因为MYKEY会处理用户的所有资源。 每个MYKEY帐户在EOS链上始终拥有0个CPU和0个NET。因此，DAPP用户界面应忽略MYKEY帐户的资源警告。

### 2. 对于与Scatter兼容的DAPP

MYKEY与Scatter协议的接口几乎完全兼容。与传统的钱包唯一的区别是MYKEY使用Operation Key索引为3的Reserved Key 签署交易，而不是EOS的owner/active密钥。

发起人的任何操作都将由MYKEY 封装，然后发送给[mykeymanager](https://eosq.app/account/mykeymanager/tables?scope=mykeyhulu521&tableName=keydata)进行签名验证，然后转发给目标合约。在大多数情况下，任意操作都可以正常工作而无需更改代码。

#### 如何检查dapps是否在MYKEY Webview中运行 <a id="&#x5982;&#x4F55;&#x68C0;&#x67E5;dapps&#x662F;&#x5426;&#x5728;mykey-webview&#x4E2D;&#x8FD0;&#x884C;"></a>

有两种方法可以检查DAPP是否在MYKEY客户端中运行。

1. 使用navigator.userAgent进行检查。MYKEY Webview将在原userAgent之后附加MYKEY / \[version\]。

```java
function isMYKEY(){
    return navigator.userAgent.indexOf("MYKEY") > -1;
}
```

2. 使用scatter.getIdentity检查响应数据是否包含类型："MYKEY"

```java
{
    accounts: [{authority: 'active', blockchain: 'eos', name: [ACCOUNT_NAME], type: 'MYKEY'}],
    publicKey: [OperationKey 3/Reserved key]
};
```

如果DAPP依赖于getArbitrarySignature或其他服务器端身份验证 对于DAPP，请使用scatter.getArbitrarySignature方法或其他方法在服务器端验证帐户权限。应当为使用MYKEY帐户而完善用于验证签名的代码。

在这种情况下，MYKEY使用索引为3的Reserved Key通过scatter.getArbitrarySignature进行签名。DAPPs的服务器端需要使用Reserved Key来验证签名，在后端代码中，使用范围ACCOUNT\_NAME和合约mykeymanager的keydata表来查询。

我们已经实现了一些示例代码以供参考： [https://github.com/mykeylab/mykey-js-sdk/blob/master/src/index.js\#L79-L86](https://github.com/mykeylab/mykey-js-sdk/blob/master/src/index.js#L79-L86)

### 3. 对于原生DAPP <a id="3----&#x5BF9;&#x4E8E;&#x539F;&#x751F;dapp"></a>

MYKEY Development Toolkit，又名MDT，是基于MYKEY帐户系统开发应用程序的工具包。它包含MYKEY Client SDK，JSBridge Lib和SimpleWallet协议扩展。该开发工具套件可以帮助应用程序唤醒MYKEY进行登录，转账，合约调用，签名和其他链上操作。到目前为止，它只支持EOS链，以后会随着MYKEY的多链路线图扩展到其他公链。

原生DAPP可以使用MYKEY开发工具包来使用MYKEY进行授权，转账，合约调用，签名等。

### 4. 应用程序支持操作密钥（即将推出） <a id="4----&#x5E94;&#x7528;&#x7A0B;&#x5E8F;&#x652F;&#x6301;&#x64CD;&#x4F5C;&#x5BC6;&#x94A5;&#xFF08;&#x5373;&#x5C06;&#x63A8;&#x51FA;&#xFF09;"></a>

MYKEY支持DAPP开发自己的原生应用。DAPPs可以在自己的应用程序中生成一个密钥对，然后通过应用程序间的调用将生成的公钥绑定到MYKEY帐户。

在此模型中，DAPPs可以使用MYKEY SDK在本地签名和推送交易，而无需唤醒第三方钱包。通过层级权限设计，用户可以决定为一个特定的DAPP放入多少资产到其子账户中，可以轻松控制资产的风险。

更多详细信息即将披露！

