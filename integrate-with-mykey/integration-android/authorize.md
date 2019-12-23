# Authorize

Pull up MYKEY for authentication binding. See the class definition for the parameters: [AuthorizeRequest](../../dive-into-mykey/classes-and-methods/#class-authorizerequest) and [MYKEYWalletCallback](../../dive-into-mykey/classes-and-methods/#class-mykeywalletcallback)

For better security, dapp can set CallBackUrl for server-side verification.

MYKEY will post the signed data to CallBackUrl which provided by dapp, server-side of dapp should verify the signature, dapp server should query the user's ReserveKey from MYKEY SmartContract data to verify the signature, see detail in [KEYS in MYKEY](../../dive-into-mykey/mykey-on-eos.md#mykey-account-structure) and [MYKEY Verify Sign](../../dive-into-mykey/mykey-on-eos.md#integrate-eos-dapps-with-mykey)

The format of the data post to CallBackUrl:

```java
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

```java
// generate unsignedMessage
let unsignedData = timestamp + account + uuID + ref
// publicKey: ReserveKey of MYKEY，can be quired from SmartContract https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#keys-in-table-keydata
ecc.verify(signature, unsignedData, pubkey) === true
```

dapp should provide response of CallBackUrl call to MYKEY:

```java
{
    "code":0,       // error code，=0 is success. >0, dapp should describe error in message.
    "message":""    // message
}
```

Sample:

```java
AuthorizeRequest authorizeRequest = new AuthorizeRequest()
    // EOS use ChainCons.EOS，ETH use ChainCons.ETH
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
    // DApp server query whether the binding is succussful
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

