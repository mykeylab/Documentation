# 认证

使用MYKEY iOS SDK进行认证，需要先[准备环境](preconditions.md)和[初始化SDK](initiate-sdk.md)。

第三方应用唤起MYKEY进行认证绑定。参数请详见类定义:[AuthorizeRequest](../../dive-into-mykey/classes-and-methods/#lei-authorizerequest)和[MYKEYResponse](../../dive-into-mykey/classes-and-methods/#lei-mykeyresponse)。（为了更强的安全性，可以设置CallBackUrl进行服务端验签。）

MYKEY将签名后的数据POST到dapp提供的CallBackUrl，请求第三方应用服务端验证，第三方应用服务端需要从合约数据中获取该用户MYKEY账号的Reserve Key进行验签，获取方式参考[KEYS in MYKEY](../../dive-into-mykey/mykey-on-eos.md#keydata表中的密钥)and[MYKEY Verify Sign](../../dive-into-mykey/mykey-on-eos.md#2-dui-yu-yu-scatter-jian-rong-de-dapp)

第三方应用提供的CallBackUrl接口参数：

```swift
{
    "protocol":"",  // 协议名，使用MYKEY双向绑定方式协议为MYKEY，使用MYKEY轻量级方式协议为MYKEYSimple
    "version":"",   // 协议版本信息，如1.0
    "dapp_key":"",  // MYKEY分配的DAPP_KEY，使用MYKEY双向绑定方式时提供，由MYKEY服务端分配，从dapp客户端初始化方法传入
    "uuID":"",      // 用户id，MYKEY双向绑定方式此字段为dapp客户端初始化时传入的uuid；MYKEY轻量级方式此字段为用户的设备ID；
    "sign":"",      // eos签名, 签名数据：timestamp + account + uuID + ref
    "ref":"",       // 来源, mykey
    "timestamp":"", // 当前UNIX时间戳, 精确到秒
    "account":"",    // eos账户名
    "chain": ""      // 值为ANY, EOS, ETH，或者不传递该参数
}
```

验签方式：

```swift
// generate unsignedMessage
let unsignedData = timestamp + account + uuID + ref
// publicKey: ReserveKey of MYKEY，can be quired from SmartContract https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#keys-in-table-keydata
ecc.verify(signature, unsignedData, pubkey) === true
```

dapp提供的CallBackUrl接口返回值：

```swift
{
    "code":0,       // 错误符，等于0是成功，大于0说明请求失败，dapp返回具体的错误码
    "message":""    // 返回的提示信息
}
```

调用例子：

```swift
let authorizeRequest = AuthorizeRequest()
authorizeRequest.userName = "uchihamadara"
authorizeRequest.info = "Test information"
// param：{"protocol": "", "version": "", "dapp_key": "", "uuID": "", "public_key": "", "sign": "", "ref": "", "timestamp": "", "account": ""}
// return: same as SimpleWallet {"code": [0-2], "message": ""}
authorizeRequest.callbackUrl = "https://dappserver.xxx.url"

MYKEYWallet.shared.authorize(authorizeRequest: authorizeRequest, response: MYKEYResponse.init(success: { (response) in
    self.view.makeToast("success")
}, failure: { (errorValue) in
    self.view.makeToast("failure")
}, cancelled: {
    self.view.makeToast("cancelled")
}))
```

