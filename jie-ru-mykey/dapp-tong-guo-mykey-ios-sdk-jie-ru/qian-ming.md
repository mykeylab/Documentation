# 签名

唤起MYKEY进行Sign签名操作。参数请详见类定义:[SignRequest](https://github.com/mykeylab/Documentation/blob/master/Chinese/MYKEY_iOS_SDK.md#class-signrequest)

dapp服务端或者客户端需要从合约数据中获取该用户在MYKEY的ReserveKey进行验签，获取方式参考[KEYS in MYKEY](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#keys-in-table-keydata) 和 [MYKEY Verify Sign](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#if-dapp-dependents-on-getarbitrarysignature-or-other-server-side-authentication)

```swift
let signRequest = SignRequest()
signRequest.message = "Messages that need to be signed, [it could be random which come from dapp server]"
// DApp CallbackUrl
// param：{"protocol": "", "version": "", "message": "", "sign": "", "ref": "", "account": ""}
// return: same as SimpleWallet {"code": [0-2], "message": ""}
signRequest.callbackUrl = "https://dappserver.xxx.url"

MYKEYWallet.shared.sign(signRequest: signRequest, response: MYKEYResponse.init(success: { (response) in
    self.view.makeToast("success")
}, failure: { (errorValue) in
    self.view.makeToast("failure")
}, cancelled: {
    self.view.makeToast("cancelled")
}))
```

