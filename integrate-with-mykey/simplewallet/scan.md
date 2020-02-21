# PC Web通过扫码接入

### 登录

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

WEB端使用签名账号的**Reserved公钥**进行验签。Reserved公钥需要通过读取mykey合约获得，见[文档](../../dive-into-mykey/mykey-on-eos.md#mykey帐户结构)。

### 合约调用

#### Web扫码调用合约时序图

![](../../.gitbook/assets/image%20%286%29.png)

请传递给MYKEY如下的数据，数据格式为json：

```java
// 合约调用数据格式
{
    protocol    string   // 协议名，本协议默认 SimpleWallet
    version     string   // 协议版本信息，如1.0
    action      string   // 操作类型，为 transaction
    dappName    string   // DApp应用名
    dappIcon    string   // DApp应用图标
    desc        string   // MYKEY显示给用户合约调用语义描述
    callback    string   // MYKEY回调DApp的深度链接，e.g. custom://custom.com/contract
    notifyUrl   string   // 合约调用成功通知DAppServer的回调URL接口
    ContractRequest [  //合约操作数组，包含转账与非转账合约操作
      { //非转账
          account   string // 合约名
          name      string // 合约方法
          info      string // MYKEY显示给用户的语义化描述
          data      object // 根据合约abi定义所传的参数对象 e.g. {key1: value1, key2: value2 }
      },
      { //转账
          account   string // 合约名
          name      string // 合约方法
          info      string // MYKEY显示给用户的语义化描述
          TransferDataRequest {
            from     string // 转账From
            to       string // 转账To
            quantity string // 转账金额和单位，e.g. "1.0000 EOS"
            memo     string // 链上备注
          }
      }
    ]
    expired        number   // 仅Web二维码模式可用，过期时间，unix时间戳
}
```

### 签名

#### Web扫码调用签名时序图

![](../../.gitbook/assets/image%20%282%29.png)

请传递给MYKEY如下的数据，数据格式为json：

```java
// 签名调用数据格式
{
    protocol    string   // 协议名，本协议默认 SimpleWallet
    version     string   // 协议版本信息，如1.0
    action      string   // 操作类型，为 sign
    dappName    string   // DApp应用名
    dappIcon    string   // DApp应用图标
    desc        string   // MYKEY显示给用户签名调用语义描述
    message     string   // 需要签名的内容
    callback    string   // MYKEY回调DApp的深度链接，e.g. custom://custom.com/contract
    notifyUrl   string   // 签名成功通知DAppServer的回调URL接口
    expired     number   // 仅Web二维码模式可用，过期时间，unix时间戳
}
```

