# 基于MYKEY的第三方授权登录

MYKEY提供SDK和API，帮助第三方应用实现方便的用户认证，降低用户进入门槛。

MYKEY登录允许使用MYKEY帐号登录访问第三方应用，发送区块链交易，具有如下特点：

* 受保障 的非托管区块链账户
* 免费的 区块链使用
* 多链支持及多语言服务

## MYKEY登录技术原理

MYKEY登录依赖ECDSA-secp256k1的签名和验签方法。由第三方应用发起登录请求，用户通过MYKEY提供指定的签名，第三方应用验证签名的合法性，完成认证。

## MYKEY登录认证流程

![](../.gitbook/assets/image%20%284%29.png)

### 1. 发起认证请求

第三方应用发起认证请求，需要传递五个参数：动作、动作详情的摘要、时间戳、应用的名称/id、回调URL、指定区块链\(可选\)，并根据渠道采取不同的处理策略。

例如：request={login, hash{login}, 1581410193, bihu.com, bihu\_callback\_url, ETH}

（1）H5页面

通过JS Bridge与MYKEY进行通信。

（2）PC WEB

将请求参数编码成QR Code并展示。

（3）APP

如接入SDK，可直接调用相关方法唤醒MYKEY；否则，通过api方式传递参数。  


### 2. 签名/返回回执

MYKEY接收并解析参数，向用户展示第三方应用的名称和动作。

（1）判断开通情况

如第三方应用指定了区块链，而用户未在该链上创建账户，则提示开通。如未指定或已开通则进入下一步。

（2）验证权限及签名

用户输入密码或验证指纹/人脸，调用Reserved Key对请求内容进行签名。

（3）返回回执

回执除了签名外，还需包含用户UUID和用户地址信息，如第三方应用未指定区块链，则返回所有链上地址，便于第三方应用鉴别用户。

例：

指定ETH：receipt={sig, uuid:ETH:0xFFFF }

未指定区块链：receipt={sig, uuid:{EOS:null; ETH:0xFFFF...; Tron:0xEEEE}  }

### 3. 解析回执，请求公钥

第三方应用解析请求，获得用户uuid和用户地址。

（1）检查该uuid是否注册过。

如果该uuid未注册过，则在本地数据库登记uuid和对应链的用户地址，并选择一条链作为查询地址。

如果已注册过，则直接选择原uuid所关联的用户地址作为查询地址。

（2）查询公钥。

第三方应用可以选择直接通过区块链服务，如infura查询用户公钥，也可以通过MYKEY提供的API查询对应地址的公钥。

### 4. 返回公钥

区块链服务商或MYKEY返回对应地址的公钥信息。

![\(warning\)](https://confluence.inner-bihu.com/s/en_US/7901/04c8b7bf0a5b4889210956b8230224e43d124b25/_/images/icons/emoticons/warning.svg)备注: 公钥可能被冻结，下一步MYKEY会检测公钥状态.

### 5. 验证签名

如果公钥状态不是可用状态，则直接返回失败。

如果公钥是可用状态，检查（1）签名与用户公钥是否对应；（2）签名内容是否与请求内容相同。如验证通过，则返回成功，否则返回验签失败。

## 不同接入方式的认证方法

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x63A5;&#x5165;&#x65B9;&#x5F0F;</th>
      <th style="text-align:left">&#x8BA4;&#x8BC1;&#x65B9;&#x6CD5;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">MYKEY SDK</td>
      <td style="text-align:left">
        <p><a href="../integrate-with-mykey/integration-android/transfer.md">Android SDK</a>
        </p>
        <p><a href="../integrate-with-mykey/integration-ios/transfer.md">iOS SDK</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">H5</td>
      <td style="text-align:left">
        <p><a href>ETH Web3</a>
        </p>
        <p><a href>EOS Scatter</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">SimpleWallet&#x534F;&#x8BAE;</td>
      <td style="text-align:left"><a href="../integrate-with-mykey/simplewallet.md#qian-ming">SimpleWallet</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">PC Web&#x626B;&#x7801;</td>
      <td style="text-align:left"><a href="../integrate-with-mykey/scan.md#qian-ming">&#x626B;&#x7801;</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Deeplink&#x534F;&#x8BAE;</td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

