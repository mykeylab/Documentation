# SimpleWallet协议接入

### 使用SimpleWallet跳转MYKEY代码示例

**特别注意:** Android端SimpleWallet跳转MYKEY时请使用如下代码（设置MYKEY的包名）

```java
try {
    Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
    intent.setPackage("com.mykey.id");
    intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    context.startActivity(intent);
} catch (Exception e) {
    e.printStackTrace();
}
```

### 登录和支付

MYKEY遵循SimpleWallet协议实现，详细请见以下文档:

[https://github.com/southex/SimpleWallet/blob/master/README\_en.md](https://github.com/southex/SimpleWallet/blob/master/README_en.md)

除了支持SimpleWallet规范的**登录**和**支付**，MYKEY还额外增强支持了**合约**和**签名**的调用。

**特别注意:** [MYKEY的账号体系](../dive-into-mykey/shen-ru-mykey-zhang-hu.md#mykey有啥不同？)与其他的EOS账号有所差异，需要在服务端验签时使用Reserved公钥进行验签。

### 移动端App调用合约时序图

![](../.gitbook/assets/image%20%282%29.png)

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

![](../.gitbook/assets/image%20%284%29.png)

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

