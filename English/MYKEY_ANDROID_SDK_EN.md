# MYKEY Android SDK

## How to integration

### 1. Download 'MYKEYWalletLib.aar' from following link, copy to libs directory of your app module

https://github.com/mykeylab/MYKEY-Client-SDK/tree/master/Android

### 2. Add following code to file build.gradle:
```
repositories {
    flatDir {
        dirs 'libs'
    }
}
```  
### 3. In file build.gradle, add config for Jni directory
```
android {
    ...
    sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
        }
    }

    defaultConfig {
        ndk {
            abiFilters "armeabi-v7a"
        }
    }

}
```
### 4. Add following dependency in file build.gradle
```
dependencies{
    implementation(name: 'MYKEYWalletLib', ext: 'aar')
    implementation "com.alibaba:fastjson:1.1.70.android"
}
```
### 5. Copy following code to AndroidManifest.xml, and set the callback deeplink, composed by scheme、host and path
```xml
<activity android:name="com.mykey.sdk.connect.scheme.callback.MYKEYCallbackActivity">
    <intent-filter>
        <data
            android:scheme="customscheme"
            android:host="customhost"
            android:path="/custompath"/>
        <data/>

        <category android:name="android.intent.category.DEFAULT"/>
        <action android:name="android.intent.action.VIEW"/>
        <category android:name="android.intent.category.BROWSABLE"/>
    </intent-filter>
</activity>
```
This configuration will generate a deeplink for MYKEY callback, which will be used in MYKEK SDK initlization, [init](#init) [initSimple](#initSimple).
### 6. Proguard rules
```
-keep class com.mykey.sdk**{*;}
-dontwarn com.mykey.sdk**

-keep class go**{*;}
-dontwarn go**

-keep class mykeycore**{*;}
-dontwarn mykeycore**
```
## Class MyKeySdk

MYKEY Android SDK's main logic is encapsulated in the MyKeySdk class, which implements six methods, namely init, initSimple, authorize, transfer, contract, signature, jumpToGuideInstall.

### Method Summary


| Methods          |         
|:-----------------:|
| [init](#init)      |
| [initSimple](#initsimple) |
| [authorize](#authorize) |
| [transfer](#transfer)  |
| [contract](#contract)  |
| [sign](#sign) |
| [jumpToGuideInstall](#jumptoguideinstall) |

### init

Instantiate the class MyKeySdk, initialize the SDK in the main process, use this initialization method, if dapp already have an account system, you can bind with MYKEY. See the class definition for the parameters: [InitRequest](#class-initrequest)


```java
MYKEYSdk.getInstance().init(new InitRequest().setAppKey(Config.SAMPLE_DAPP_APP_KEY)
    .setDappName(Config.SAMPLE_DAPP_NAME)
    .setUuid(Config.SAMPLE_DAPP_USER_ID)
    .setDappIcon(Config.SAMPLE_DAPP_ICON)
    .setCallback(Config.SAMPLE_DAPP_CALLBACK)
    .setDsiableInstall(false)
    .setContractPromptFree(true)
    .setContext(this));
```

### initSimple

Instantiate the class MyKeySdk, initialize the SDK in the main process, and use the simplewallet protocol logic at the bottom layler. Using this initialization method, the dapp can have no account system, without deep bind with MYKEY account. See the class definition for the parameters: [InitSimpleRequest](#class-initsimplerequest)

```java
MYKEYSdk.getInstance().initSimple(new InitSimpleRequest().setDappName(Config.SAMPLE_DAPP_NAME)
    .setDappIcon(Config.SAMPLE_DAPP_ICON)
    .setCallback(Config.SAMPLE_DAPP_CALLBACK)
    .setDisableInstall(false)
    .setContractPromptFree(true)
    .setContext(this));
```

### authorize

Pull up MYKEY for authentication binding. See the class definition for the parameters: [AuthorizeRequest](#class-authorizerequest) and [MYKEYWalletCallback](#class-mykeywalletcallback)

For greater security, dapp can set CallBackUrl for server-side verification.

MYKEY will post the signed data to CallBackUrl which provided by dapp, server-side of dapp should verify the signature, dapp server should query the user's ReserveKey from MYKEY SmartContract data to verify the signature, see detail in [KEYS in MYKEY](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#keys-in-table-keydata) and [MYKEY Verify Sign](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#if-dapp-dependents-on-getarbitrarysignature-or-other-server-side-authentication)

The format of the data post to CallBackUrl

```java
{
	"protocol": "", // protocol name，Use init method, protocol name is 'MYKEY', use initSimple to init, protocol name is 'MYKEYSimple'
	"version": "",  // Version，1.0
	"dapp_key": "", // DAPP_KEY assigned by MYKEY，contact MYKEY team to apply. In simple mode, it is null
	"uuID": "",     // user id，dapp passed it in init method；In simple mode, it is device id
	"sign": "",     // eos signature, sign data：timestamp + account + uuID + ref
	"ref": "",      // ref, mykey
	"timestamp": "",// UNIX timestamp, accurate to second
	"account": ""   // eos account name
}
```
Verify signature：
```javascript
// generate unsignedMessage
let unsignedData = timestamp + account + uuID + ref
// publicKey: ReserveKey of MYKEY，can be quired from SmartContract https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#keys-in-table-keydata
ecc.verify(signature, unsignedData, pubkey) === true
```

dapp should provide response of CallBackUrl call to MYKEY
```java
{
	"code": 0,     // error code，=0 is success. >0, dapp should describe error in message.
	"message": ""  // message
}
```
Sample

```java
AuthorizeRequest authorizeRequest = new AuthorizeRequest()
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
        //Add check code here, Check if bind success in dapp server
    }

    @Override
    public void onError(String payloadJson) {
        // payloadJson: {"errorCode":,"errorMsg":""}
        LogUtil.e(TAG, "onError payloadJson:" + payloadJson);
        Toast.makeText(activity, "error，payloadJson:" + payloadJson, Toast.LENGTH_LONG).show();
    }

    @Override
    public void onCancel() {
        LogUtil.e(TAG, "cancel");
        Toast.makeText(activity, "cancelled", Toast.LENGTH_LONG).show();
    }
});
```

### transfer

Pull up MYKEY to transfer. See the class definition for the parameters: [TransferRequest](#class-transferrequest) 和 [MYKEYWalletCallback](#class-mykeywalletcallback)

```java
TransferRequest transferRequest = new TransferRequest()
        .setFrom("bobbobbobbob")
        .setTo("alicealice11")
        .setAmount(0.0001)
        .setMemo("memo")
        .setContractName("eosio.token")
        .setSymbol("EOS")
        .setInfo("transfer to alicealice11")
        .setDecimal(4)
        // order ID which come from dapp server
        .setOrderId("BH1000001")
        // DApp CallbackUrl
        // param：{"protocol": "", "version": "", "tx_id": "", "ref": "", "account": ""}
        // return: same as SimpleWallet {"code": [0-2], "message": ""}
        .setCallBackUrl(Config.DAPP_CALLBACK_URL);
MYKEYSdk.getInstance().transfer(transferRequest, new MYKEYWalletCallback() {
    @Override
    public void onSuccess(String dataJson) {
        // dataJson：{"txId":""}
        LogUtil.e(TAG, "onSuccess data：" + dataJson);
        Toast.makeText(activity, "success", Toast.LENGTH_LONG).show();
    }

    @Override
    public void onError(String payloadJson) {
        // payloadJson: {"errorCode":,"errorMsg":""}
        LogUtil.e(TAG, "onError payloadJson:" + payloadJson);
        Toast.makeText(activity, "error, payloadJson:" + payloadJson, Toast.LENGTH_LONG).show();
    }

    @Override
    public void onCancel() {
        LogUtil.e(TAG, "cancel");
        Toast.makeText(activity, "cancelled", Toast.LENGTH_LONG).show();
    }
});
```


### contract

Pull up MYKEY for contract calls, support multiple action combination calls, support ContractAction and TransferAction two types of action types. Please refer to the class definition for the parameters [ContractRequest](#class-contractrequest) and [MYKEYWalletCallback](#class-mykeywalletcallback)

```java

ContractRequest contractRequest = new ContractRequest()
        .setInfo("Perform the mortgage REX operation")
        // order ID which come from dapp server
        .setOrderId("BH1000001")
        // DApp CallbackUrl
        // param：{"protocol": "", "version": "", "tx_id": "", "ref": "", "account": ""}
        // return: same as SimpleWallet {"code": [0-2], "message": ""}
        .setCallBackUrl(Config.DAPP_CALLBACK_URL);
ContractAction contractActionRequest = new ContractAction();
contractActionRequest.setAccount("eosio")
        .setName("buyram")
        .setInfo("buy ram")
        // JSON string support, eg:setData("{\"player\":\"bobbobbobbob\",\"receiver\":\"alicealice11\",\"quant\":\"1.0000 EOS\"}")
        .setData(new BuyRamDataEntity().setPayer("bobbobbobbob").setReceiver("alicealice11").setQuant("1.0000 EOS"));
contractRequest.addAction(contractActionRequest);

TransferAction transferActionRequest = new TransferAction();
transferActionRequest.setAccount("eosio.token")
        .setName("transfer")
        .setInfo("transfer to alicealice11")
        .setData(new TransferData().setFrom("bobbobbobbob").setTo("alicealice11").setQuantity("0.0001 EOS").setMemo("memo"));
contractRequest.addAction(transferActionRequest);

MYKEYSdk.getInstance().contract(contractRequest, new MYKEYWalletCallback() {
    @Override
    public void onSuccess(String dataJson) {
        // dataJson：{"txId":""}
        LogUtil.e(TAG, "onSuccess");
        Toast.makeText(activity, "success", Toast.LENGTH_LONG).show();
    }

    @Override
    public void onError(String payloadJson) {
        // payloadJson: {"errorCode":,"errorMsg":""}
        LogUtil.e(TAG, "onError payloadJson:" + payloadJson);
        Toast.makeText(activity, "error, payloadJson:" + payloadJson, Toast.LENGTH_LONG).show();
    }

    @Override
    public void onCancel() {
        LogUtil.e(TAG, "cancel");
        Toast.makeText(activity, "cancelled", Toast.LENGTH_LONG).show();
    }
});
```

### sign

Pull up MYKEY for Signature operation. See the class definition for the parameters: [SignRequest](#class-signrequest) and [MYKEYWalletCallback](#class-mykeywalletcallback)

dapp server or client should query the user's ReserveKey from MYKEY SmartContract data to verify the signature, see detail in [KEYS in MYKEY](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#keys-in-table-keydata) and [MYKEY Verify Sign](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#if-dapp-dependents-on-getarbitrarysignature-or-other-server-side-authentication)

```java
SignRequest signRequest = new SignRequest().setMessage("Messages that need to be signed, [it could be random which come from dapp server]")
        // DApp CallbackUrl
        // param：{"protocol": "", "version": "", "message": "", "sign": "", "ref": "", "account": ""}
        // return: same as SimpleWallet {"code": [0-2], "message": ""}
        .setCallBackUrl(Config.DAPP_CALLBACK_URL);
MYKEYSdk.getInstance().sign(signRequest, new MYKEYWalletCallback() {
    @Override
    public void onSuccess(String dataJson) {
        // dataJson：{"sign":""}
        LogUtil.e(TAG, "onSuccess data：" + dataJson);
        Toast.makeText(activity, "success", Toast.LENGTH_LONG).show();
    }

    @Override
    public void onError(String payloadJson) {
        // payloadJson: {"errorCode":,"errorMsg":""}
        LogUtil.e(TAG, "onError payloadJson:" + payloadJson);
        Toast.makeText(activity, "error, payloadJson:" + payloadJson, Toast.LENGTH_LONG).show();
    }

    @Override
    public void onCancel() {
        LogUtil.e(TAG, "cancel");
        Toast.makeText(activity, "cancelled", Toast.LENGTH_LONG).show();
    }
});
```

### jumpToGuideInstall

Jump to the pop-up to MYKEY installation page and boot when the user does not have MYKEY installed.

### Other Field Summary

| Field           |      Type                             |  Description       |
|-----------------|:------------------------------------:|-------------|
| initHandle      | com.mykey.sdk.handle.InitHandle      | initialization implemented logic   |
| authorizeHandle | com.mykey.sdk.handle.AuthorizeHandle | Authorize implemented logic   |
| transferHandle  | com.mykey.sdk.handle.TransferHandle  | Transfer implemented logic   |
| contractHandle  | com.mykey.sdk.handle.ContractHandle  | Contract implemented logic   |
| signatureHandle | com.mykey.sdk.handle.SignatureHandle | Sign implemented logic   |

## Other Class Defination

### Class InitRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| context |  android.content.Context | dapp application context|
| appKey |    String   | unique id assigned to each dapp, contact us |
| uuid | UUID |   the unique user id in dapp server side, recommend to use uuid |
| dappName | String |    dapp name |
| dappIcon | String |    dapp icon logo, no small than 144x144px |
| disableInstall(default false) | boolean |    Whether to disable the default install page when MYKEY is not installed |
| callback | String |    Deeplink MYKEY callback to dapp,defined in [AndroidManifest.xml](#5-copy-following-code-to-androidmanifestxml-and-set-the-callback-deeplink-composed-by-schemehost-and-path), e.g. customscheme://customhost/custompath |
| showUpgradeTip(default false) | boolean |    Toast tip when MYKEY is not the latest version |
| mykeyServer | String |    Environment URL endpoint of MYKEY server |
| contractPromptFree | Boolean | Do not display prompt for contract action except transfer |

### Class InitSimpleRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| context |  android.content.Context | dapp application context|
| dappName | String |    dapp name |
| dappIcon | String |    dapp icon logo, no small than 144x144px  |
| disableInstall(default false) | boolean |     Whether to disable the default install page when MYKEY is not installed |
| callback | String |    Deeplink MYKEY callback to dapp,defined in [AndroidManifest.xml](), e.g. customscheme://customhost/custompath |
| contractPromptFree | Boolean | Do not display prompt for contract action except transfer |

### Class AuthorizeRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| userName |  String | Custom user name|
| callBackUrl(optional) | String |  Optional, Callback endpoint url of dapp server，MYKEY will callback to dapp server after authorize request success at first, then wake up mobile client  |
| info     | String | Info, Semantic description of MYKEY display to the user for authorization page |

### Class TransferRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| from |  String | From account|
| to | String |  To account  |
| amount     | String | Amount, e.g "1.0000" |
| symbol     | String | Symbol, e.g. "EOS" |
| contractName     | String | contract code name, e.g. "eosio.token" |
| decimal     | String | Decimal |
| memo     | String | Memo |
| info     | String |  Semantic description of MYKEY display to the user about this action |
| orderId     | String | The order id from dapp, optional, can be null e.g. "20190606001" |
| callbackUrl(optional)     | String | Optional, callback endpoint url of dapp server，MYKEY will callback to dapp server after transfer request success at first, then wake up mobile client |

### Class ContractRequest

| properties   |      Type      | Description |
|----------|:-------------:|------|
| orderId |  String | The order id from dapp, optional, can be null|
| info     | String | Semantic description of MYKEY display to the user about this action |
| callbackUrl(optional)  | String | Optional, callback endpoint url of dapp server，MYKEY will callback to dapp server after contract request success at first, then wake up mobile client |
| list\<BaseAction\> | [ContractAction](#class-contractaction) or [TransferAction](#class-transferaction) | List of contract actions

### Class ContractAction
| properties   |      Type      | Description |
|----------|:-------------:|------|
| account |  String | contract code name |
| name     | String | contract action name |
| info     | String | Semantic description of MYKEY display to the user about this action |
| data | Object | The parameter object passed according to the contract abi definition e.g. {key1: value1, key2: value2 }|

### Class TransferAction
| properties   |      Type      | Description |
|----------|:-------------:|------|
| account |  String | contract code name |
| name     | String | contract action name, use "transfer" |
| info     | String | Semantic description of MYKEY display to the user about this action |
| transferObj | [TransferData](#class-transferdata) | Transfer info object|

### Class TransferData
| properties   |      Type      | Description |
|----------|:-------------:|------|
| from |  String | From account name|
| to     | String | To account name |
| quantity     | String | Amount and Symbol |
| memo | String| Memo |

### Class SignRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| message |  String | Unsigned messages|
| callbackUrl     | String |  Optional, callback endpoint url of dapp server，MYKEY will callback to dapp server after sign request success at first, then wake up mobile client|

### Class MYKEYWalletCallback

| methods   | Description |
|-----------|-------------|
| onSuccess | Success Callback    |
| onError   | Failure Callback,[errorCode list](#error-code)    |
| onCancel | Cancel Callback |

## Error Code

0-2 are defined by SimpleWallet

10001-X are defined by MYKEYSdk

| code   | Description |
|-----------|-------------|
|   0       |  User cancel the transaction  |
|   1	      |  Success  |
|   2	      |  Failure |
|   10001   | unknow issue lead can not wakeup MYKEY |
|   10002	  | MYKEY not installed yet |
|   10003	  | MYKEY account is frozen |
|   10004	  | Uninitialized |
|   10005	  | Push transaction timeout |
|   10006	  | Binded, triggered at authorize |
|   10007	  | Unbind, triggered at transaction, trasnfer, sign |
|   10008	  | dapp binded, and MYKEY unbind, triggered at transaction, trasnfer, sign |
|   10009	  | MYKEY binded, triggered at authorize |
|   10010	  | dapp binded, and MYKEY binded, but not match |
|   10011	  | MYKEY unregistered, triggered at transaction, trasnfer, sign |
|   10012	  | Illegal param |
|   10013	  | Insufficient balance |

        main {
            jniLibs.srcDirs = ['libs']
        }
    }

    defaultConfig {
        ndk {
            abiFilters "armeabi-v7a"
        }
    }

}
```
### 4. Add following dependency in file build.gradle
```
dependencies{
    implementation(name: 'MYKEYWalletLib', ext: 'aar')
    implementation "com.alibaba:fastjson:1.1.70.android"
}
```
### 5. Copy following code to AndroidManifest.xml, and set the callback deeplink, composed by scheme、host and path
```xml
<activity android:name="com.mykey.sdk.callback.MYKEYCallbackActivity">
    <intent-filter>
        <data
            android:scheme="customscheme"
            android:host="customhost"
            android:path="/custompath"/>
        <data/>

        <category android:name="android.intent.category.DEFAULT"/>
        <action android:name="android.intent.action.VIEW"/>
        <category android:name="android.intent.category.BROWSABLE"/>
    </intent-filter>
</activity>
```
This configuration will generate a deeplink for MYKEY callback, which will be used in MYKEK SDK initlization, [init](#init) [initSimple](#initSimple).
### 6. Proguard rules
```
-keep class com.mykey.sdk**{*;}
-dontwarn com.mykey.sdk**
```
## Class MyKeySdk

MYKEY Android SDK's main logic is encapsulated in the MyKeySdk class, which implements six methods, namely init, initSimple, authorize, transfer, contract, signature, jumpToGuideInstall.

### Method Summary


| Methods          |         
|:-----------------:|
| [init](#init)      |
| [initSimple](#initsimple) |
| [authorize](#authorize) |
| [transfer](#transfer)  |
| [contract](#contract)  |
| [sign](#sign) |
| [jumpToGuideInstall](#jumptoguideinstall) |

### init

Instantiate the class MyKeySdk, initialize the SDK in the main process, use this initialization method, if dapp already have an account system, you can bind with MYKEY. See the class definition for the parameters: [InitRequest](#class-initrequest)


```java
MYKEYSdk.getInstance().init(new InitRequest().setAppKey(Config.SAMPLE_DAPP_APP_KEY)
    .setDappName(Config.SAMPLE_DAPP_NAME)
    .setUserId(Config.SAMPLE_DAPP_USER_ID)
    .setDappIcon(Config.SAMPLE_DAPP_ICON)
    .setCallback(Config.SAMPLE_DAPP_CALLBACK)
    .setDsiableInstall(false)
    .setContext(this));
```

### initSimple

Instantiate the class MyKeySdk, initialize the SDK in the main process, and use the simplewallet protocol logic at the bottom layler. Using this initialization method, the dapp can have no account system, without deep bind with MYKEY account. See the class definition for the parameters: [InitSimpleRequest](#class-initsimplerequest)

```java
MYKEYSdk.getInstance().initSimple(new InitSimpleRequest().setDappName(Config.SAMPLE_DAPP_NAME)
    .setDappIcon(Config.SAMPLE_DAPP_ICON)
    .setCallback(Config.SAMPLE_DAPP_CALLBACK)
    .setDisableInstall(false)
    .setContext(this));
```

### authorize

Pull up MYKEY for authentication binding. See the class definition for the parameters: [AuthorizeRequest](#class-authorizerequest) and [MYKEYWalletCallback](#class-mykeywalletcallback)


```java
AuthorizeRequest authorizeRequest = new AuthorizeRequest()
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
        //Add check code here, Check if bind success in dapp server
    }

    @Override
    public void onError(String payloadJson) {
        // payloadJson: {"errorCode":,"errorMsg":""}
        LogUtil.e(TAG, "onError payloadJson:" + payloadJson);
        Toast.makeText(activity, "error，payloadJson:" + payloadJson, Toast.LENGTH_LONG).show();
    }

    @Override
    public void onCancel() {
        LogUtil.e(TAG, "cancel");
        Toast.makeText(activity, "cancelled", Toast.LENGTH_LONG).show();
    }
});
```

### transfer

Pull up MYKEY to transfer. See the class definition for the parameters: [TransferRequest](#class-transferrequest) 和 [MYKEYWalletCallback](#class-mykeywalletcallback)

```java
TransferRequest transferRequest = new TransferRequest()
        .setFrom("bobbobbobbob")
        .setTo("alicealice11")
        .setAmount(0.0001)
        .setMemo("memo")
        .setContractName("eosio.token")
        .setSymbol("EOS")
        .setInfo("transfer to alicealice11")
        .setDecimal(4)
        // order ID which come from dapp server
        .setOrderId("BH1000001")
        // DApp CallbackUrl
        // param：{"protocol": "", "version": "", "tx_id": "", "ref": "", "account": ""}
        // return: same as SimpleWallet {"code": [0-2], "message": ""}
        .setCallBackUrl(Config.DAPP_CALLBACK_URL);
MYKEYSdk.getInstance().transfer(transferRequest, new MYKEYWalletCallback() {
    @Override
    public void onSuccess(String dataJson) {
        // dataJson：{"txId":""}
        LogUtil.e(TAG, "onSuccess data：" + dataJson);
        Toast.makeText(activity, "success", Toast.LENGTH_LONG).show();
    }

    @Override
    public void onError(String payloadJson) {
        // payloadJson: {"errorCode":,"errorMsg":""}
        LogUtil.e(TAG, "onError payloadJson:" + payloadJson);
        Toast.makeText(activity, "error, payloadJson:" + payloadJson, Toast.LENGTH_LONG).show();
    }

    @Override
    public void onCancel() {
        LogUtil.e(TAG, "cancel");
        Toast.makeText(activity, "cancelled", Toast.LENGTH_LONG).show();
    }
});
```


### contract

Pull up MYKEY for contract calls, support multiple action combination calls, support ContractAction and TransferAction two types of action types. Please refer to the class definition for the parameters [ContractRequest](#class-contractrequest) and [MYKEYWalletCallback](#class-mykeywalletcallback)

```java

ContractRequest contractRequest = new ContractRequest()
        .setInfo("Perform the mortgage REX operation")
        // order ID which come from dapp server
        .setOrderId("BH1000001")
        // DApp CallbackUrl
        // param：{"protocol": "", "version": "", "tx_id": "", "ref": "", "account": ""}
        // return: same as SimpleWallet {"code": [0-2], "message": ""}
        .setCallBackUrl(Config.DAPP_CALLBACK_URL);
ContractAction contractActionRequest = new ContractAction();
contractActionRequest.setAccount("eosio")
        .setName("buyram")
        .setInfo("buy ram")
        // JSON string support, eg:setData("{\"player\":\"bobbobbobbob\",\"receiver\":\"alicealice11\",\"quant\":\"1.0000 EOS\"}")
        .setData(new BuyRamDataEntity().setPayer("bobbobbobbob").setReceiver("alicealice11").setQuant("1.0000 EOS"));
contractRequest.addAction(contractActionRequest);

TransferAction transferActionRequest = new TransferAction();
transferActionRequest.setAccount("eosio.token")
        .setName("transfer")
        .setInfo("transfer to alicealice11")
        .setData(new TransferData().setFrom("bobbobbobbob").setTo("alicealice11").setQuantity("0.0001 EOS").setMemo("memo"));
contractRequest.addAction(transferActionRequest);

MYKEYSdk.getInstance().contract(contractRequest, new MYKEYWalletCallback() {
    @Override
    public void onSuccess(String dataJson) {
        // dataJson：{"txId":""}
        LogUtil.e(TAG, "onSuccess");
        Toast.makeText(activity, "success", Toast.LENGTH_LONG).show();
    }

    @Override
    public void onError(String payloadJson) {
        // payloadJson: {"errorCode":,"errorMsg":""}
        LogUtil.e(TAG, "onError payloadJson:" + payloadJson);
        Toast.makeText(activity, "error, payloadJson:" + payloadJson, Toast.LENGTH_LONG).show();
    }

    @Override
    public void onCancel() {
        LogUtil.e(TAG, "cancel");
        Toast.makeText(activity, "cancelled", Toast.LENGTH_LONG).show();
    }
});
```

### sign

Pull up MYKEY for Signature operation. See the class definition for the parameters: [SignRequest](#class-signrequest) and [MYKEYWalletCallback](#class-mykeywalletcallback)

```java
SignRequest signRequest = new SignRequest().setMessage("Messages that need to be signed, [it could be random which come from dapp server]")
        // DApp CallbackUrl
        // param：{"protocol": "", "version": "", "message": "", "sign": "", "ref": "", "account": ""}
        // return: same as SimpleWallet {"code": [0-2], "message": ""}
        .setCallBackUrl(Config.DAPP_CALLBACK_URL);
MYKEYSdk.getInstance().sign(signRequest, new MYKEYWalletCallback() {
    @Override
    public void onSuccess(String dataJson) {
        // dataJson：{"sign":""}
        LogUtil.e(TAG, "onSuccess data：" + dataJson);
        Toast.makeText(activity, "success", Toast.LENGTH_LONG).show();
    }

    @Override
    public void onError(String payloadJson) {
        // payloadJson: {"errorCode":,"errorMsg":""}
        LogUtil.e(TAG, "onError payloadJson:" + payloadJson);
        Toast.makeText(activity, "error, payloadJson:" + payloadJson, Toast.LENGTH_LONG).show();
    }

    @Override
    public void onCancel() {
        LogUtil.e(TAG, "cancel");
        Toast.makeText(activity, "cancelled", Toast.LENGTH_LONG).show();
    }
});
```

### jumpToGuideInstall

Jump to the pop-up to MYKEY installation page and boot when the user does not have MYKEY installed.

### Other Field Summary

| Field           |      Type                             |  Description       |
|-----------------|:------------------------------------:|-------------|
| initHandle      | com.mykey.sdk.handle.InitHandle      | initialization implemented logic   |
| authorizeHandle | com.mykey.sdk.handle.AuthorizeHandle | Authorize implemented logic   |
| transferHandle  | com.mykey.sdk.handle.TransferHandle  | Transfer implemented logic   |
| contractHandle  | com.mykey.sdk.handle.ContractHandle  | Contract implemented logic   |
| signatureHandle | com.mykey.sdk.handle.SignatureHandle | Sign implemented logic   |

## Other Class Defination

### Class InitRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| context |  android.content.Context | dapp application context|
| appKey |    String   | unique id assigned to each dapp, contact us |
| userId | String |   the unique user id in dapp server side, recommend to use uuid |
| dappName | String |    dapp name |
| dappIcon | String |    dapp icon logo, no small than 144x144px |
| disableInstall(default false) | boolean |    Whether to disable the default install page when MYKEY is not installed |
| callback | String |    Deeplink MYKEY callback to dapp,defined in [AndroidManifest.xml](#5-copy-following-code-to-androidmanifestxml-and-set-the-callback-deeplink-composed-by-schemehost-and-path), e.g. customscheme://customhost/custompath |
| showUpgradeTip(default false) | boolean |    Toast tip when MYKEY is not the latest version |
| mykeyServer | String |    Environment URL endpoint of MYKEY server |

### Class InitSimpleRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| context |  android.content.Context | dapp application context|
| dappName | String |    dapp name |
| dappIcon | String |    dapp icon logo, no small than 144x144px  |
| disableInstall(default false) | boolean |     Whether to disable the default install page when MYKEY is not installed |
| callback | String |    Deeplink MYKEY callback to dapp,defined in [AndroidManifest.xml](), e.g. customscheme://customhost/custompath |

### Class AuthorizeRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| userName |  String | Custom user name|
| callBackUrl(optional) | String |  Optional, Callback endpoint url of dapp server，MYKEY will callback to dapp server after authorize request success at first, then wake up mobile client  |
| info     | String | Info, Semantic description of MYKEY display to the user for authorization page |

### Class TransferRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| from |  String | From account|
| to | String |  To account  |
| amount     | String | Amount, e.g "1.0000" |
| symbol     | String | Symbol, e.g. "EOS" |
| contractName     | String | contract code name, e.g. "eosio.token" |
| decimal     | String | Decimal |
| memo     | String | Memo |
| info     | String |  Semantic description of MYKEY display to the user about this action |
| orderId     | String | The order id from dapp, optional, can be null e.g. "20190606001" |
| callbackUrl(optional)     | String | Optional, callback endpoint url of dapp server，MYKEY will callback to dapp server after transfer request success at first, then wake up mobile client |

### Class ContractRequest

| properties   |      Type      | Description |
|----------|:-------------:|------|
| orderId |  String | The order id from dapp, optional, can be null|
| info     | String | Semantic description of MYKEY display to the user about this action |
| callbackUrl(optional)  | String | Optional, callback endpoint url of dapp server，MYKEY will callback to dapp server after contract request success at first, then wake up mobile client |
| list\<BaseAction\> | [ContractAction](#class-contractaction) or [TransferAction](#class-transferaction) | List of contract actions

### Class ContractAction
| properties   |      Type      | Description |
|----------|:-------------:|------|
| account |  String | contract code name |
| name     | String | contract action name |
| info     | String | Semantic description of MYKEY display to the user about this action |
| data | Object | The parameter object passed according to the contract abi definition e.g. {key1: value1, key2: value2 }|

### Class TransferAction
| properties   |      Type      | Description |
|----------|:-------------:|------|
| account |  String | contract code name |
| name     | String | contract action name, use "transfer" |
| info     | String | Semantic description of MYKEY display to the user about this action |
| transferObj | [TransferData](#class-transferdata) | Transfer info object|

### Class TransferData
| properties   |      Type      | Description |
|----------|:-------------:|------|
| from |  String | From account name|
| to     | String | To account name |
| quantity     | String | Amount and Symbol |
| memo | String| Memo |

### Class SignRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| message |  String | Unsigned messages|
| callbackUrl     | String |  Optional, callback endpoint url of dapp server，MYKEY will callback to dapp server after sign request success at first, then wake up mobile client|

### Class MYKEYWalletCallback

| methods   | Description |
|-----------|-------------|
| onSuccess | Success Callback    |
| onError   | Failure Callback,[errorCode list](#error-code)    |
| onCancel | Cancel Callback |

## Error Code

0-2 are defined by SimpleWallet

10001-X are defined by MYKEYSdk

| code   | Description |
|-----------|-------------|
|   0       |  User cancel the transaction  |
|   1	      |  Success  |
|   2	      |  Failure |
|   10001   | unknow issue lead can not wakeup MYKEY |
|   10002	  | MYKEY not installed yet |
|   10003	  | MYKEY account is frozen |
|   10004	  | Uninitialized |
|   10005	  | Push transaction timeout |
|   10006	  | Binded, triggered at authorize |
|   10007	  | Unbind, triggered at transaction, trasnfer, sign |
|   10008	  | dapp binded, and MYKEY unbind, triggered at transaction, trasnfer, sign |
|   10009	  | MYKEY binded, triggered at authorize |
|   10010	  | dapp binded, and MYKEY binded, but not match |
|   10011	  | MYKEY unregistered, triggered at transaction, trasnfer, sign |
|   10012	  | Illegal param |
|   10013	  | Insufficient balance |
|   10014	  | MYKEY account unavailable |
