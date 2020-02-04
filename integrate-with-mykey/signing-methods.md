# MYKEY验签方式汇总

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
        <p><a href="integration-android/transfer.md">Android SDK</a>
        </p>
        <p><a href="integration-ios/transfer.md">iOS SDK</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">H5</td>
      <td style="text-align:left">
        <p><a href="h5/eth.md#mykey-yan-qian-fang-shi">ETH Web3</a>
        </p>
        <p><a href="h5/eos.md#mykey-yan-qian-fang-shi">EOS Scatter</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">SimpleWallet&#x534F;&#x8BAE;</td>
      <td style="text-align:left"><a href="simplewallet/#qian-ming">SimpleWallet</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">PC Web&#x626B;&#x7801;</td>
      <td style="text-align:left"><a href="simplewallet/scan.md#qian-ming">&#x626B;&#x7801;</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Deeplink&#x534F;&#x8BAE;</td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>