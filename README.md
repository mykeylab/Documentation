# 简介

欢迎来到MYKEY开发者中心。

MYKEY是基于多条公有区块链的自主身份系统，是基于Key ID自主身份协议的首个应用实例，目标是降低公有链和区块链应用的使用门槛，实现区块链的大规模应用，并成为Web3.0信任网络的一部分。

MYKEY作为多链钱包，将支持多种智能合约平台。由于MYKEY账户在每一条区块链上均以智能合约的方式存在，因此MYKEY的钱包功能暂时不支持非智能合约平台。目前已支持**EOS**和**ETH**。

在这里，你可以了解到如何快速接入海量KYC用户的多链钱包MYKEY，也可以获取到最有用的开发资源。 此文档包含以下内容：

* [接入MYKEY](integrate-with-mykey/integration-android/) 说明使用MYKEY SDK，SimpleWallet，Scatter协议，web3协议等方式接入MYKEY。
* [深入MYKEY](dive-into-mykey/mykey-on-eos.md) 详细说明MYKEY的设计原理、功能扩展等。
  * [合约充值](dive-into-mykey/contracts-deposit/) 介绍交易所和DAPP项目方如何识别MYKEY交易。
* [开发资源](development-resources/eos.md) 最实用的EOS、ETH开发资源。



与一般EOS/ETH账号不同，MYKEY账户默认拥有多个权限，MYKEY使用一把叫**Reserved Key**的权限进行签名，DApp验证签名时，需要先从智能合约中找到MYKEY账户的Reserved Key公钥，再根据算法验证签名。MYKEY支持以下验签形式：

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x63A5;&#x5165;&#x65B9;&#x5F0F;</th>
      <th style="text-align:left">&#x9A8C;&#x7B7E;&#x65B9;&#x6CD5;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">MYKEY SDK</td>
      <td style="text-align:left">
        <p><a href="integrate-with-mykey/integration-android/transfer.md">Android SDK</a>
        </p>
        <p><a href="integrate-with-mykey/integration-ios/transfer.md">iOS SDK</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">H5</td>
      <td style="text-align:left">
        <p><a href="integrate-with-mykey/h5/eth.md#mykey-yan-qian-fang-shi">ETH Web3</a>
        </p>
        <p><a href="integrate-with-mykey/h5/eos.md#mykey-yan-qian-fang-shi">EOS Scatter</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">SimpleWallet&#x534F;&#x8BAE;</td>
      <td style="text-align:left"><a href="integrate-with-mykey/simplewallet/#qian-ming">SimpleWallet</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">PC Web&#x626B;&#x7801;</td>
      <td style="text-align:left"><a href="integrate-with-mykey/simplewallet/scan.md#qian-ming">&#x626B;&#x7801;</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Deeplink&#x534F;&#x8BAE;</td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>