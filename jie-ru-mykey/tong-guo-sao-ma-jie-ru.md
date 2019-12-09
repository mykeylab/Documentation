# 通过扫码接入

对于WEB应用，只要遵循SimpleWallet协议，即可通过MYKEY APP扫描二维码登录。

WEB应用通过二维码传递给MYKEY如下的数据，数据格式为json：

```javascript
// 签名调用数据格式
{
    protocol string     // 协议名，本协议默认 SimpleWallet
    version string      // 协议版本信息，如1.0
    dappName string     // dapp名称
    dappIcon string     // dapp应用图标url
    action string       // 操作类型，为 login
    uuID string         // dapp服务器为本次登录产生的uuid
    loginUrl string     // dapp服务器验证登录的url
    expired number      // 仅Web二维码模式可用，过期时间，unix时间戳
    loginMemo string    // 登录备注信息
}
```

MYKEY APP通过扫描二维码，请求用户授权登录，并把签名数据提交到WEB应用指定的loginUrl。

WEB应用收到的MYKEY如下的数据，格式为json:

```javascript
{
    protocol string     // 协议名，本协议默认 SimpleWallet
    version string      // 协议版本信息，如1.0
    timestamp number    // 当前unix时间戳
    sign string         // eos签名信息
    uuID string         // dapp服务器为本次登录产生的uuid
    Account string      // eos账户名
    Ref string          // 钱包名
}
```

WEB端使用签名账号的**Reserved公钥**进行验签。Reserved公钥需要通过读取mykey合约获得，见[文档](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#if-dapp-dependents-on-getarbitrarysignature-or-other-server-side-authentication)。

