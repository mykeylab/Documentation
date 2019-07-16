# MYKEY iOS SDK

## 1.如何集成

### 1.1 下载MYKEYSDK.zip文档，解压后，拖进工程目录；
### 1.2 在Xcode设置URL scheme: Project->TARGETS->info->URL Types->添加 URL scheme；

例如使用demoscheme，详见下图

![](./image/iOS-URLTypes.jpg)

这个配置会自动生成MYEKY回掉的深度链接，将会被用于MYKEY SDK的初始化方法中. [详见 initWalletSimple](#initwalletsimple).

### 1.3 在info.plist中LSApplicationQueriesSchemes下添加一项，值为mykey；
![](./image/iOS-LSApplicationQueriesSchemes.jpg)

### 1.4 Build Settings 下 Enable Bitcode置为false
![](./image/iOS-bitcode.jpg)

### 1.5 备注

该库使用swift编写，Objective-C工程需要配置Bridging-Header.h文件，如果工程中没有，可以创建Empty.swift(一个空的swift文件)，工程会自动生成Bridging-Header.h。
![](./image/iOS-bridging.jpg)



## 2.初始化

### 2.1 在 AppDelegate.m 中添加头文件
#### Swift
```
import MYKEYWalletLib
```

#### Objective-C
```
#import <MYKEYWalletLib/MYKEYWalletLib.h>
```
### 执行初始化

```
let initData = InitRequest()
initData.appKey = "xxxx"
initData.dappName = "DappNameA"
initData.dappIcon = "https:.../xx.png"
initData.UUID = "uuidxxxxx"
initData.scheme = "demoscheme"
initData.disableInstall = true
MYKEYWallet.shared.initWallet(initData: initData)
```
其中,scheme参数[参考1.2中的scheme配置](#12-在xcode设置url-scheme-project-targets-info-url-types-添加-url-scheme)

### 2.3 在 application:openURL: 中添加handleUrl方法

```
MYKEYWallet.shared.handleUrl(url: url)
```

## Class MYKEYWallet

MYKEY iOS主要的逻辑封装在MYKEYWallet类中, 包含initWallet、initWalletSimple、authorize、transfer、contract、sign.

### 方法描述 Method Summary


| Methods          |         
|:-----------------:|
| [initWallet](#initwallet)      |
| [initWalletSimple](#initwalletsimple) |
| [authorize](#authorize) |
| [transfer](#transfer)  |
| [contract](#contract)  |
| [sign](#sign) |

### initWallet

实例化MYKEYWallet类，在主进程中进行SDK初始化，使用此初始化方法，dapp如果存在账户体系，可以与MYKEY进行绑定操作。参数请详见类定义:  [InitRequest](#class-initrequest)

```
let initData = InitRequest()
initData.appKey = "xxxx"
initData.dappName = "DappNameA"
initData.dappIcon = "https:.../xx.png"
initData.UUID = UUID
initData.scheme = "demoscheme"
initData.disableInstall = true
MYKEYWallet.shared.initWallet(initData: initData)
```
Scheme参数，[参考1.2 scheme](#12-在xcode设置url-scheme-project-targets-info-url-types-添加-url-scheme)

### initWalletSimple

实例化MYKEYWallet类，在主进程中进行SDK初始化，底层使用simplewallet协议逻辑，使用此初始化方法，dapp可以没有账户体系，不需要与MYKEY进行绑定操作。参数请详见类定义:  [InitSimpleRequest](#class-initsimplerequest)

```
let initSimpleData = InitSimpleRequest()
initSimpleData.dappName = "DappNameA"
initSimpleData.dappIcon = "https:.../xx.png"
initSimpleData.scheme = "demoscheme" 
initSimpleData.disableInstall = true
MYKEYWallet.shared.initWalletSimple(initSimpleData: initSimpleData)

```
Scheme参数，[参考1.2 scheme](#12-在xcode设置url-scheme-project-targets-info-url-types-添加-url-scheme)

### authorize

唤起MYKEY进行认证绑定。参数请详见类定义: [AuthorizeRequest](#class-authorizerequest) 和 [MYKEYResponse](#class-mykeyresponse)

```
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

### transfer

唤起MYKEY进行转账。参数请详见类定义: [TransferRequest](#class-transferrequest) 和 [MYKEYResponse](#class-mykeyresponse)

```
let transferData = TransferData()
transferData.from = "mykeyhulu525"
transferData.to = "madaraxliang"
transferData.quantity = "1.31 EOS"
transferData.memo = "transfer-memo"

let transferActionData = TransferAction()
transferActionData.account = "eosio.token"
transferActionData.name = WalletActionConstants.TRANSFER.rawValue
transferActionData.info = "contract-transfer-info"
transferActionData.data = transferData

let contractActionData = ContractAction()
contractActionData.account = "eosio"
contractActionData.name = "buyram"
contractActionData.info = "contract-contract-info"
contractActionData.data = ["payer":"mykeyhulu123","receiver":"mykeyhulu111","quant":"1.01 EOS"]

let contractRequest = ContractRequest()
contractRequest.info = "Perform the mortgage REX operation"
contractRequest.orderId = "BH19004"
// param：{"protocol": "", "version": "", "tx_id": "", "ref": "", "account": ""}
contractRequest.callbackUrl = "https://dappserver.xxx.url"
contractRequest.actions = [transferActionData,contractActionData]

MYKEYWallet.shared.contract(contractRequest: contractRequest, response: MYKEYResponse.init(success: { (response) in
    self.view.makeToast("success")
}, failure: { (errorValue) in
    self.view.makeToast("failure")
}, cancelled: {
    self.view.makeToast("cancelled")
}))
```

### contract

唤起MYKEY进行合约调用, 支持多Action组合调用, 支持ContractAction和TransferAction两种形式的action类型。
参数请详见类定义:  [ContractRequest](#class-contractrequest), [ContractAction](#class-contractaction) , [TransferAction](#class-transferaction) 和 [MYKEYResponse](#class-mykeyresponse).

```
let transferData = TransferData()
transferData.from = "mykeyhulu525"
transferData.to = "madaraxliang"
transferData.quantity = "1.31 EOS"
transferData.memo = "transfer-memo"

let transferActionData = TransferAction()
transferActionData.account = "eosio.token"
transferActionData.name = WalletActionConstants.TRANSFER.rawValue
transferActionData.info = "contract-transfer-info"
transferActionData.data = transferData

let contractActionData = ContractAction()
contractActionData.account = "eosio"
contractActionData.name = "buyram"
contractActionData.info = "contract-contract-info"
contractActionData.data = ["payer":"mykeyhulu123","receiver":"mykeyhulu111","quant":"1.01 EOS"]

let contractRequest = ContractRequest()
contractRequest.info = "Perform the mortgage REX operation"
contractRequest.orderId = "BH19004"
// param：{"protocol": "", "version": "", "tx_id": "", "ref": "", "account": ""}
// return: same as SimpleWallet {"code": [0-2], "message": ""}
contractRequest.callbackUrl = "https://dappserver.xxx.url"
contractRequest.actions = [transferActionData,contractActionData]

MYKEYWallet.shared.contract(contractRequest: contractRequest, response: MYKEYResponse.init(success: { (response) in
    self.presentDataView(data: response)
    self.view.makeToast("success")
}, failure: { (errorValue) in
    self.presentDataView(data: errorValue)
    self.view.makeToast("failure")
}, cancelled: {
    self.view.makeToast("cancelled")
}))
```

### sign

唤起MYKEY进行Sign签名操作。参数请详见类定义: [SignRequest](#class-signrequest)

```
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

## 其他类定义

### Class InitRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| appKey |    String   |  为每一个dapp指定的唯一key |
| UUID |  UUID |   dapp为该用户提供的唯一ID,建议使用uuid |
| dappName | String |    dapp的名称 |
| dappIcon | String |    dapp的logo, 建议不低于144x144px |
| disableInstall | boolean |    是否禁用MYKEY未安装时显示默认引导页面 |
| scheme | String |  MYKEY完成授权操作后返回Dapp的跳转链接，[参考scheme定义 in 1.2](#12-在xcode设置url-scheme-project-targets-info-url-types-添加-url-scheme), e.g. demoscheme |
| mykeyServer | String |   MYKEY服务端环境Endpoint |

### Class InitSimpleRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| dappName | String |    dapp的名称 |
| dappIcon | String |    dapp的logo, 建议不低于144x144px |
| disableInstall | boolean |    是否禁用MYKEY未安装时显示默认引导页面 |
| scheme | String |  MYKEY完成授权操作后返回Dapp的跳转链接，[参考scheme定义 in 1.2](#12-在xcode设置url-scheme-project-targets-info-url-types-添加-url-scheme), e.g. demoscheme |

### Class AuthorizeRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| userName |  String | 自定义用户名|
| callBackUrl | String |  dapp server的回调url，MYKEY绑定成功会先回调dapp server,然后再唤醒移动端  |
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
| callbackUrl     | String | dapp server的回调url，MYKEY转账成功会先回调dapp server,然后再唤醒移动端 |

### Class ContractRequest

| properties   |      Type      | Description |
|----------|:-------------:|------|
| orderId |  String | dapp 提供的订单ID, 可为空|
| info     | String | 备注信息，用于语义化该笔操作 |
| callbackUrl     | String | dapp server的回调url，MYKEY合约调用成功会先回调dapp server,然后再唤醒移动端 |
| actions: [BaseAction] | [ContractAction](#class-contractaction) 或者 [TransferAction](#class-transferaction) | 合约操作action的列表

### Class ContractAction
| properties   |      Type      | Description |
|----------|:-------------:|------|
| account |  String | 合约名 |
| name     | String | 合约方法 |
| info     | String | 备注信息，用于语义化该笔操作 |
| data | Any | 根据合约abi定义所传的参数，类型需能通过JSONSerialization.isValidJSONObject(_ obj: Any)验证， e.g. {key1: value1, key2: value2 }|

### Class TransferAction
| properties   |      Type      | Description |
|----------|:-------------:|------|
| account |  String | 合约名 |
| name     | String | 合约方法，填写"transfer" |
| info     | String | 备注信息，用于语义化该笔操作 |
| data | [TransferData](#class-transferdata) | 转账信息对象|

### Class TransferData
| properties   |      Type      | Description |
|----------|:-------------:|------|
| from |  String | 转账支出账号|
| to     | String | 转账接收账号 |
| quantity     | String | 转账金额与单位 |
| memo | String| 链上备注信息 |

### Class SignRequest
| properties   |      Type      | Description |
|----------|:-------------:|------|
| message |  String | 需要签名的数据|
| callbackUrl     | String | dapp server的回调url，MYKEY签名成功会先回调dapp server,然后再唤醒移动端 |

### Class MYKEYResponse

| properties  | Description |
|-----------|-------------|
| success | 成功的回调    |
| failure   | 失败的回调， [errorCode list](#error-code) |
| cancelled | 取消交易的回调 |


### Class MYKEYApiResponse

| properties   | Description |
|-----------|-------------|
| success | 成功的回调    |
| failure   | 失败的回调   |


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
