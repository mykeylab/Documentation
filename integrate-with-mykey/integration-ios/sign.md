# Sign

Pull up MYKEY for Signature operation. See the class definition for the parameters: [SignRequest](../../dive-into-mykey/classes-and-methods.md#class-signrequest)

dapp server or client should query the user's ReserveKey from MYKEY SmartContract data to verify the signature, see detail in [KEYS in MYKEY](../../dive-into-mykey/mykey-on-eos.md#mykey-account-structure) and [MYKEY Verify Sign](../../dive-into-mykey/mykey-on-eos.md#integrate-eos-dapps-with-mykey)

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

