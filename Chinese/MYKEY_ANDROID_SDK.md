# MYKEY Android SDK

## 如何集成

### 1. 在以下项目链接中，下载MYKEYWalletLib.aar文件，复制到到你app模块的libs目录下

https://github.com/mykeylab/MYKEY-Client-SDK/tree/master/Android

### 2. 在app模块的build.gradle文件的空白处添加如下代码：
```
repositories {
    flatDir {
        dirs 'libs'
    }
}
```  
### 3. 在app模块的build.gradle文件android下添加Jni文件夹配置
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
### 4. 在app模块的build.gradle文件中添加依赖
```
dependencies{
    implementation(name: 'MYKEYWalletLib', ext: 'aar')
    implementation "com.alibaba:fastjson:1.1.70.android"
}
```
### 5. 复制下面的代码到你的AndroidManifest.xml，并设置符合你包名或规则的scheme、host和path
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
此设置会为你生成一个供MYKEY调用的深度链接，在MYKEY初始化时会用到，[init](#init) [initSimple](#initSimple)。

### 6. 添加混淆配置
```
-keep class com.mykey.sdk**{*;}
-dontwarn com.mykey.sdk**

-keep class go**{*;}
-dontwarn go**

-keep class mykeycore**{*;}
-dontwarn mykeycore**
```

## Class MyKeySdk

MYKEY Android主要的逻辑封装在MyKeySdk类中, 实现了6个方法，分别是init, initSimple, authorize, transfer, contract, signature, jumpToGuideInstall.

### 方法描述 Method Summary


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

实例化MyKeySdk类，MYKEY双向绑定认证方式调用，在主进程中进行SDK初始化，使用此初始化方法，dapp如果存在账户体系，可以与MYKEY进行绑定操作。参数请详见类定义:  [InitRequest](#class-initrequest)


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

实例化MyKeySdk类，MYKEY轻量级认证方式调用，在主进程中进行SDK初始化，底层使用simplewallet协议逻辑，使用此初始化方法，dapp可以没有账户体系，不需要与MYKEY进行绑定操作。参数请详见类定义:  [InitSimpleRequest](#class-initsimplerequest)


```java
MYKEYSdk.getInstance().initSimple(new InitSimpleRequest().setDappName(Config.SAMPLE_DAPP_NAME)
    .setDappIcon(Config.SAMPLE_DAPP_ICON)
    .setCallback(Config.SAMPLE_DAPP_CALLBACK)
    .setDisableInstall(false)
    .setContractPromptFree(true)
    .setContext(this));
```

### authorize

唤起MYKEY进行认证绑定。参数请详见类定义: [AuthorizeRequest](#class-authorizerequest) 和 [MYKEYWalletCallback](#class-mykeywalletcallback)

为了更强的安全性，可以设置CallBackUrl进行服务端验签

MYKEY将签名后的数据POST到dapp提供的CallBackUrl，请求dapp服务端验证，dapp服务端需要从合约数据中获取该用户在MYKEY的ReserveKey进行验签，获取方式参考[KEYS in MYKEY](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#keys-in-table-keydata) and [MYKEY Verify Sign](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#if-dapp-dependents-on-getarbitrarysignature-or-other-server-side-authentication)

dapp提供的CallBackUrl接口参数：
```java
{
	"protocol": "", // 协议名，使用MYKEY双向绑定方式协议为MYKEY，使用MYKEY轻量级方式协议为MYKEYSimple
	"version": "",  // 协议版本信息，如1.0
	"dapp_key": "", // MYKEY分配的DAPP_KEY，使用MYKEY双向绑定方式时提供，由MYKEY服务端分配，从dapp客户端初始化方法传入
	"uuID": "",     // 用户id，MYKEY双向绑定方式此字段为dapp客户端初始化时传入的uuid；MYKEY轻量级方式此字段为用户的设备ID；
	"sign": "",     // eos签名, 签名数据：timestamp + account + uuID + ref
	"ref": "",      // 来源, mykey
	"timestamp": "",// 当前UNIX时间戳, 精确到秒
	"account": ""   // eos账户名
}
```
验签方式：
```javascript
// generate unsignedMessage
let unsignedData = timestamp + account + uuID + ref
// publicKey: ReserveKey of MYKEY，can be quired from SmartContract https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#keys-in-table-keydata
ecc.verify(signature, unsignedData, pubkey) === true
```
dapp提供的CallBackUrl接口返回值：
```java
{
	"code": 0,     // 错误符，等于0是成功，大于0说明请求失败，dapp返回具体的错误码
	"message": ""  // 返回的提示信息
}
```
调用例子：
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
        // DApp此时需要去DApp server查询是否绑定成功
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
唤起MYKEY进行转账。参数请详见类定义: [TransferRequest](#class-transferrequest) 和 [MYKEYWalletCallback](#class-mykeywalletcallback)

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

唤起MYKEY进行合约调用, 支持多Action组合调用, 支持ContractAction和TransferAction两种形式的action类型。
参数请详见类定义: [ContractRequest](#class-contractrequest) 和 [MYKEYWalletCallback](#class-mykeywalletcallback)

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
唤起MYKEY进行Sign签名操作。参数请详见类定义: [SignRequest](#class-signrequest) 和 [MYKEYWalletCallback](#class-mykeywalletcallback)

dapp服务端或者客户端需要从合约数据中获取该用户在MYKEY的ReserveKey进行验签，获取方式参考[KEYS in MYKEY](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#keys-in-table-keydata) and [MYKEY Verify Sign](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#if-dapp-dependents-on-getarbitrarysignature-or-other-server-side-authentication)

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
跳转到弹出引导安装MYKEY页面，当用户没有安装MYKEY时进行引导。

### 字段描述 Field Summary

| Field           |      Type                             |  Description       |
|-----------------|:------------------------------------:|-------------|
| initHandle      | com.mykey.sdk.handle.InitHandle     | 实例化类的处理逻辑   |
| authorizeHandle | com.mykey.sdk.handle.AuthorizeHandle | 认证操作的处理逻辑   |
| transferHandle  | com.mykey.sdk.handle.TransferHandle  | 转账操作的处理逻辑   |
| contractHandle  | com.mykey.sdk.handle.ContractHandle  | 合约操作的处理逻辑   |
| signatureHandle | com.mykey.sdk.handle.SignatureHandle | 签名操作的处理逻辑   |

## 其他类定义

### Class InitRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| context |  android.content.Context | 可传入dapp应用上下文|
| appKey |    String   |  为每一个dapp指定的唯一key |
| uuid | UUID |   dapp为该用户提供的唯一ID,建议使用uuid |
| dappName | String |    dapp的名称 |
| dappIcon | String |    dapp的logo, 建议不低于144x144px |
| disableInstall（默认false） | boolean |    是否禁用MYKEY未安装时显示默认引导页面 |
| callback | String |    MYKEY调用成功后回调dapp的深度链接,在[AndroidManifest.xml中定义](#4-复制下面的代码到你的androidmanifestxml并设置符合你包名或规则的schemehost和path), e.g. customscheme://customhost/custompath |
| showUpgradeTip（默认false） | boolean |    MYKEY非最新版本是否显示更新提示，提示为系统默认Toast |
| mykeyServer | String |    MYKEY服务端环境Endpoint |
| contractPromptFree | Boolean | 除转账行为之外的合约方法免提示开关 |

### Class InitSimpleRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| context |  android.content.Context | 可传入dapp应用上下文|
| dappName | String |    dapp的名称 |
| dappIcon | String |    dapp的logo, 建议不低于144x144px |
| disableInstall（默认false） | boolean |    是否禁用MYKEY未安装时显示默认引导页面 |
| callback | String |    MYKEY调用成功后回调dapp的深度链接,在[AndroidManifest.xml中定义](#4-复制下面的代码到你的androidmanifestxml并设置符合你包名或规则的schemehost和path), e.g. customscheme://customhost/custompath |
| contractPromptFree | Boolean | 除转账行为之外的合约方法免提示开关 |

### Class AuthorizeRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| userName |  String | 自定义用户名|
| callBackUrl（可选） | String |  dapp server的回调url，MYKEY绑定成功会先回调dapp server,然后再唤醒移动端  |
| info     | String | 备注信息，用于绑定认证页面的语义化描述 |

### Class TransferRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| from |  String | 转账账户|
| to | String |  接受账户  |
| amount     | String | 转账数量 |
| symbol     | String | 币种Symbol, e.g. "EOS" |
| contractName     | String | 合约名称, e.g. "eosio.token" |
| decimal     | String | 币种对应的小数位数 |
| memo     | String | 链上的MEMO备注 |
| info     | String | 备注信息，用于语义化该笔转账交易 |
| orderId     | String | 订单ID，dapp提供的订单ID，可为空 e.g. "20190606001" |
| callbackUrl（可选）     | String | dapp server的回调url，上链成功会先回调dapp server,然后再唤醒移动端 |

### Class ContractRequest

| properties   |      Type      | Description |
|----------|:-------------:|------|
| orderId |  String | dapp 提供的订单ID, 可为空|
| info     | String | 备注信息，用于语义化该笔操作 |
| callbackUrl（可选）     | String | dapp server的回调url，上链成功会先回调dapp server,然后再唤醒移动端 |
| list\<BaseAction\> | [ContractAction](#class-contractaction) 或者 [TransferAction](#class-transferaction) | 合约操作action的列表

### Class ContractAction
| properties   |      Type      | Description |
|----------|:-------------:|------|
| account |  String | 合约名 |
| name     | String | 合约方法 |
| info     | String | 备注信息，用于语义化该笔操作 |
| data | Object | 根据合约abi定义所传的参数对象 e.g. {key1: value1, key2: value2 }|

### Class TransferAction
| properties   |      Type      | Description |
|----------|:-------------:|------|
| account |  String | 合约名 |
| name     | String | 合约方法，填写"transfer" |
| info     | String | 备注信息，用于语义化该笔操作 |
| transferObj | [TransferData](#class-transferdata) | 转账信息对象|

### Class TransferData
| properties   |      Type      | Description |
|----------|:-------------:|------|
| from |  String | 转账支出账号|
| to     | String | 转账接收账号 |
| quantity     | String | 转账金额与单位 |
| memo | String| 链上备注信息

### Class SignRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| message |  String | 需要签名的数据|
| callbackUrl     | String | dapp server的回调url，MYKEY绑定成功会先回调dapp server,然后再唤醒移动端 |

### Class MYKEYWalletCallback

| methods   | Description |
|-----------|-------------|
| onSuccess | 成功的回调    |
| onError   | 失败的回调,[errorCode列表](#error-code)    |
| onCancel | 取消交易的回调 |

## Error Code

0-2为SimpleWallet定义字段

10001-X为MYKEYSdk定义字段

| code   | Description |
|-----------|-------------|
|   0       |   用户主动取消操作  |
|   1	      |  操作成功  |
|   2	      | 操作失败 |
|   10001   | 未知异常导致无法唤醒MYKEY |
|   10002	  | MYKEY未安装 |
|   10003	  | MYKEY账户被冻结 |
|   10004	  | 未初始化 |
|   10005	  | 转账或合约方法调用上链超时 |
|   10006	  | 已经绑定过，执行绑定操作时抛出 |
|   10007	  | 未绑定，执行方法操作时抛出 |
|   10008	  | dapp已绑定，但是MYKEY未绑定，执行方法操作时抛出 |
|   10009	  | MYKEY已绑定，执行绑定操作时抛出 |
|   10010	  | dapp与MYKEY都已绑定，但是并不匹配 |
|   10011	  | MYKEY未注册，执行方法操作时抛出 |
|   10012	  | 参数非法 |
|   10013	  | 余额不足 |
|   10014	  | MYKEY账户不可用 |
