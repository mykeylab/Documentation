# 认证

唤起MYKEY进行认证绑定。参数请详见类定义:[AuthorizeRequest](../../dive-into-mykey/classes-and-methods.md#lei-authorizerequest) 和 [MYKEYWalletCallback](../../dive-into-mykey/classes-and-methods.md#lei-mykeywalletcallback)

为了更强的安全性，可以设置CallBackUrl进行服务端验签

MYKEY将签名后的数据POST到dapp提供的CallBackUrl，请求dapp服务端验证，dapp服务端需要从合约数据中获取该用户在MYKEY的ReserveKey进行验签，获取方式参考[KEYS in MYKEY](../../dive-into-mykey/mykey-on-eos.md#keydata表中的密钥) 和 [MYKEY Verify Sign](../../dive-into-mykey/mykey-on-eos.md#如何检查dapps是否在mykey-webview中运行)

dapp提供的CallBackUrl接口参数：

```java
{
    "protocol":"",  // 协议名，使用MYKEY双向绑定方式协议为MYKEY，使用MYKEY轻量级方式协议为MYKEYSimple
    "version":"",   // 协议版本信息，如1.0
    "dapp_key":"",  // MYKEY分配的DAPP_KEY，使用MYKEY双向绑定方式时提供，由MYKEY服务端分配，从dapp客户端初始化方法传入
    "uuID":"",      // 用户id，MYKEY双向绑定方式此字段为dapp客户端初始化时传入的uuid；MYKEY轻量级方式此字段为用户的设备ID；
    "sign":"",      // eos签名, 签名数据：timestamp + account + uuID + ref
    "ref":"",       // 来源, mykey
    "timestamp":"", // 当前UNIX时间戳, 精确到秒
    "account":""    // eos账户名
}
```

验签方式：

```java
// generate unsignedMessage
let unsignedData = timestamp + account + uuID + ref
// publicKey: ReserveKey of MYKEY，can be quired from SmartContract https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#keys-in-table-keydata
ecc.verify(signature, unsignedData, pubkey) === true
```

dapp提供的CallBackUrl接口返回值：

```java
{
    "code":0, // 错误符，等于0是成功，大于0说明请求失败，dapp返回具体的错误码
    "message":"" // 返回的提示信息
}
```

调用例子：

```java
AuthorizeRequest authorizeRequest = new AuthorizeRequest()
    // EOS用 ChainCons.EOS，ETH用 ChainCons.ETH
    .setChain(ChainCons.EOS)      
    .setUserName("bobbobbobbob")
    // DApp CallbackUrl
    // param：{"protocol": "", "version": "", "dapp_key": "", "uuID": "", "public_key": "", "sign": "", "ref": "", "timestamp": "", "account": ""}
    // return: same as SimpleWallet {"code": [0-2], "message": ""}
    .setCallbackUrl(Config.DAPP_CALLBACK_URL)
    .setInfo("Perform the binding of dapp and MYKEY");

MYKEYSdk.getInstance().authorize(authorizeRequest, new MYKEYWalletCallback() {

@Override
public void onSuccess(String dataJson) {
    // dataJson：{"protocol": "", "version": "", "dapp_key": "", "uuID": "", "public_key": "", "sign": "", "ref": "", "timestamp": "", "account": ""}
    LogUtil.e(TAG, "onSuccess");
    Toast.makeText(activity, "success", Toast.LENGTH_LONG).show();
    // DApp此时需要去DApp server查询是否绑定成功
}

@Override
public void onError(String payloadJson) {
    // payloadJson: {"errorCode":,"errorMsg":""}
    LogUtil.e(TAG, "onError payloadJson:" + payloadJson);

    Toast.makeText(activity, "error，payloadJson:" + payloadJson,Toast.LENGTH_LONG).show();
}

@Override
public void onCancel() {
    LogUtil.e(TAG, "cancel");
        Toast.makeText(activity, "cancelled", Toast.LENGTH_LONG).show();
    }
});
```
