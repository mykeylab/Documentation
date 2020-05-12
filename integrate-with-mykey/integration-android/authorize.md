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
    "account":"",    // eos account name
    "chain": ""      // value could be ANY, EOS, ETH，or not pass this param
}
```

Verify signature：

MYKEY will return mykeyUID and mykeyUIDSignature fields.  mykeyUID is the unique identifier for users in MYKEY. 

{% tabs %}
{% tab title="ETH" %}
```javascript
// generate unsigned data
let message = hex(timestamp + account + uuID + ref)
let unsignedData =  "\x19Ethereum Signed Message:\n" + message.length + message

// generate unsigned data for mykeyUID
let messageForMykeyUID = timestamp + account + uuID + ref + mykeyUID
let unsignedDataForMykeyUID =  "\x19Ethereum Signed Message:\n" + messageForMykeyUID.length + messageForMykeyUID
```
{% endtab %}

{% tab title="EOS" %}
```javascript
// generate unsigned data
let unsignedData = timestamp + account + uuID + ref
// use ReserveKey to verify signature
ecc.verify(signature, unsignedData, pubkey) === true

// generate unsinged data for mykeyId
let unsignedDataForMykeyId = timestamp + account + uuID + ref + mykeyId
// use ReserveKey to verify mykeyId signature
ecc.verify(signature, unsignedDataForMykeyId, pubkey) === true
```
{% endtab %}
{% endtabs %}

Response format：

```javascript
{
 "account": "MYKEY Account",  
 "chain": "EOS",   //EOS or ETH
 "dappKey": "xxxxxxxxx",  //MYKEY is used for communication encryption, DAPP can ignore it
 "dappUserId": "xxxxxx",  //MYKEY is used for communication encryption, DAPP can ignore it
 "mykeyUID": "xxxxxxxxx",  //User's unique ID in MYKEY
 "accountType": 1,  //1: Account with emergency contact, 2: Account without emergency contact
 //The signature of mykeyUID, in order to ensure that mykeyUID cannot be tampered with. The signature format is: unsignedData = timestamp + account + uuID + ref + mykeyUID
 "mykeyUIDSignature": "SIG_K1_K6AnPSrBfsmoTjo9Dec4QgZPKLHenhPm1rmrsNNN5sxhoa2ERQ7jySYb1NKqG5LrafTRBDe2fAEJkD1xMWYaUQYuygJbL3",  
 "origin": "mykey",
 "protocol": "MYKEYSimple",
 "pubKey": "EOS7FDwQ3Jkxu4dCJnAMJ3Na2V4GYL9YcwGCkhCp6cvTjjtLW5ZGA",  //用户的ReservedKey
 //Signature, the signature format is: unsignedData = timestamp + account + uuID + ref
 "signature": "SIG_K1_KfD3MdkVRaShXCBvpi3ueLZwUZtUQyqyCS5V2EDhudfXxHvxvS7fSwZHo7aSs7WXLjfLezpThaEbFbk2yafUTzR53kwc2x",
 "timestamp": 1585650292,
 "version": "1.0"
}
```

Sample:

```java
AuthorizeRequest authorizeRequest = new AuthorizeRequest()
    // chain could be ANY, EOS, and ETH. EOS is the default value. If it's ANY, MYKEY will try EOS first, then ETH to return an account
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

