# 认证

使用MYKEY Android SDK进行认证，需要先[准备环境](preconditions.md)和[初始化SDK](initiate-sdk.md)。

第三方应用唤起MYKEY进行认证绑定。参数请详见类定义:[AuthorizeRequest](../../dive-into-mykey/classes-and-methods/#lei-authorizerequest) 和 [MYKEYWalletCallback](../../dive-into-mykey/classes-and-methods/#lei-mykeywalletcallback)。（为了更强的安全性，可以设置CallBackUrl进行服务端验签。）

MYKEY将签名后的数据POST到第三方应用提供的CallBackUrl，请求第三方应用服务端验证，第三方应用服务端需要从合约数据中获取该用户MYKEY账号的Reserve Key进行验签，获取方式参考[KEYS in MYKEY](../../dive-into-mykey/mykey-on-eos.md#keydata表中的密钥) 和 [MYKEY Verify Sign](../../dive-into-mykey/mykey-on-eos.md#如何检查dapps是否在mykey-webview中运行)

第三方应用提供的CallBackUrl接口参数：

```java
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

验签公式：

认证时，MYKEY会返回mykeyUID和mykeyUIDSignature字段给第三方应用。mykeyUID是用户在MYKEY的唯一标识。

{% tabs %}
{% tab title="ETH" %}
```javascript
// 构造未签名数据
let message = hex(timestamp + account + uuID + ref)
let unsignedData =  "\x19Ethereum Signed Message:\n" + message.length + message

// 构造mykeyUID的未签名数据
let messageForMykeyUID = timestamp + account + uuID + ref + mykeyUID
let unsignedDataForMykeyUID =  "\x19Ethereum Signed Message:\n" + messageForMykeyUID.length + messageForMykeyUID
```
{% endtab %}

{% tab title="EOS" %}
```javascript
// 构造未签名数据
let unsignedData = timestamp + account + uuID + ref
// 使用ReserveKey验证签名
ecc.verify(signature, unsignedData, pubkey) === true

// 构造mykeyId的未签名数据
let unsignedDataForMykeyId = timestamp + account + uuID + ref + mykeyId
// 使用ReserveKey验证mykeyId签名
ecc.verify(signature, unsignedDataForMykeyId, pubkey) === true
```
{% endtab %}
{% endtabs %}

返回格式：

```javascript
{
 "account": "MYKEY账号",  
 "chain": "EOS",   //EOS或者ETH
 "dappKey": "xxxxxxxxx",  //MYKEY用于通信加密，DAPP方可忽略
 "dappUserId": "xxxxxx",  //MYKEY用于通信加密，DAPP方可忽略
 "mykeyUID": "xxxxxxxxx",  //用户在MYKEY的唯一标记符
 "accountType": 1,  //1：有紧急联系人的账户，2：无紧急联系人的账户
 //mykeyUID的签名，为了确保mykeyUID不被篡改。签名格式为：unsignedData = timestamp + account + uuID + ref + mykeyUID
 "mykeyUIDSignature": "SIG_K1_K6AnPSrBfsmoTjo9Dec4QgZPKLHenhPm1rmrsNNN5sxhoa2ERQ7jySYb1NKqG5LrafTRBDe2fAEJkD1xMWYaUQYuygJbL3",  
 "origin": "mykey",
 "protocol": "MYKEYSimple",
 "pubKey": "EOS7FDwQ3Jkxu4dCJnAMJ3Na2V4GYL9YcwGCkhCp6cvTjjtLW5ZGA",  //用户的ReservedKey
 //签名，签名格式为：unsignedData = timestamp + account + uuID + ref
 "signature": "SIG_K1_KfD3MdkVRaShXCBvpi3ueLZwUZtUQyqyCS5V2EDhudfXxHvxvS7fSwZHo7aSs7WXLjfLezpThaEbFbk2yafUTzR53kwc2x",
 "timestamp": 1585650292,
 "version": "1.0"
}
```

调用例子：

```java
AuthorizeRequest authorizeRequest = new AuthorizeRequest()
    // chain的值可以为EOS（默认）, ETH，或者ANY。如果是“ANY”，MYKEY任选一条可用的链签名并修改chain为可用链的值(例如：ETH或EOS)，并返回给SDK接入方
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

