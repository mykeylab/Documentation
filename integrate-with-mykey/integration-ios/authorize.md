# Authorize

Pull up MYKEY for authentication binding. See the class definition for the parameters:[AuthorizeRequest](../../dive-into-mykey/classes-and-methods/#class-authorizerequest) and [MYKEYResponse](../../dive-into-mykey/classes-and-methods/#class-mykeyresponse)

For greater security, dapp can set CallBackUrl for server-side verification.

MYKEY will post the signed data to CallBackUrl which provided by dapp, server-side of dapp should verify the signature, dapp server should query the user's ReserveKey from MYKEY SmartContract data to verify the signature, see detail in [KEYS in MYKEY](../../dive-into-mykey/mykey-on-eos.md#mykey-account-structure) and [MYKEY Verify Sign](../../dive-into-mykey/mykey-on-eos.md#integrate-eos-dapps-with-mykey)

The format of the data post to CallBackUrl:

```swift
{
    "protocol":"",  // protocol name，Use init method, protocol name is 'MYKEY', use initSimple to init, protocol name is 'MYKEYSimple'
    "version":"",   // Version，1.0
    "dapp_key":"",  // DAPP_KEY assigned by MYKEY，contact MYKEY team to apply. In simple mode, it is null
    "uuID":"",      // user id，dapp passed it in init method；In simple mode, it is device id
    "sign":"",      // eos signature, sign data：timestamp + account + uuID + ref
    "ref":"",       // ref, mykey
    "timestamp":"", // UNIX timestamp, accurate to second
    "account":""    // eos account name
}
```

Verify signature：

```swift
// generate unsignedMessage
let unsignedData = timestamp + account + uuID + ref
// publicKey: ReserveKey of MYKEY，can be quired from SmartContract https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#keys-in-table-keydata
ecc.verify(signature, unsignedData, pubkey) === true
```

dapp should provide response of CallBackUrl call to MYKEY：

```swift
{
    "code":0,       // error code，=0 is success. >0, dapp should describe error in message.
    "message":""    // message
}
```

Sample：

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

