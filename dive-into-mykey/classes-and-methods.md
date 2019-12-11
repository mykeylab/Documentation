# 类和方法定义

## 类 MyKeySdk

MYKEY Android主要的逻辑封装在MyKeySdk类中, 实现了6个方法，分别是init, initSimple, authorize, transfer, contract, signature, jumpToGuideInstall.

### 方法 jumpToGuideInstall

跳转到弹出引导安装MYKEY页面，当用户没有安装MYKEY时进行引导。

#### 字段描述 Field Summary

| Field | Type | Description |
| :--- | :---: | :--- |
| initHandle | com.mykey.sdk.handle.InitHandle | 实例化类的处理逻辑 |
| authorizeHandle | com.mykey.sdk.handle.AuthorizeHandle | 认证操作的处理逻辑 |
| transferHandle | com.mykey.sdk.handle.TransferHandle | 转账操作的处理逻辑 |
| contractHandle | com.mykey.sdk.handle.ContractHandle | 合约操作的处理逻辑 |
| signatureHandle | com.mykey.sdk.handle.SignatureHandle | 签名操作的处理逻辑 |

## 其他类定义

### 类 InitRequest

| properties | Type | Description |
| :--- | :---: | :--- |
| context | android.content.Context | 可传入dapp应用上下文 |
| appKey | String | 为每一个dapp指定的唯一key |
| uuid | UUID | dapp为该用户提供的唯一ID,建议使用uuid |
| dappName | String | dapp的名称 |
| dappIcon | String | dapp的logo, 建议不低于144x144px |
| disableInstall（默认false） | boolean | 是否禁用MYKEY未安装时显示默认引导页面 |
| callback | String | MYKEY调用成功后回调dapp的深度链接,在[AndroidManifest.xml中定义](classes-and-methods.md#4-复制下面的代码到你的androidmanifestxml并设置符合你包名或规则的schemehost和path), e.g. customscheme://customhost/custompath |
| showUpgradeTip（默认false） | boolean | MYKEY非最新版本是否显示更新提示，提示为系统默认Toast |
| mykeyServer | String | MYKEY服务端环境Endpoint |
| contractPromptFree | Boolean | 除转账行为之外的合约方法免提示开关 |

### 类 InitSimpleRequest

| properties | Type | Description |
| :--- | :---: | :--- |
| context | android.content.Context | 可传入dapp应用上下文 |
| dappName | String | dapp的名称 |
| dappIcon | String | dapp的logo, 建议不低于144x144px |
| disableInstall（默认false） | boolean | 是否禁用MYKEY未安装时显示默认引导页面 |
| callback | String | MYKEY调用成功后回调dapp的深度链接,在[AndroidManifest.xml中定义](classes-and-methods.md#4-复制下面的代码到你的androidmanifestxml并设置符合你包名或规则的schemehost和path), e.g. customscheme://customhost/custompath |
| contractPromptFree | Boolean | 除转账行为之外的合约方法免提示开关 |

### 类 AuthorizeRequest

| properties | Type | Description |
| :--- | :---: | :--- |
| userName | String | 自定义用户名 |
| callBackUrl（可选） | String | dapp server的回调url，MYKEY绑定成功会先回调dapp server,然后再唤醒移动端 |
| info | String | 备注信息，用于绑定认证页面的语义化描述 |

### 类 TransferRequest

| properties | Type | Description |
| :--- | :---: | :--- |
| from | String | 转账账户 |
| to | String | 接受账户 |
| amount | String | 转账数量 |
| symbol | String | 币种Symbol, e.g. "EOS" |
| contractName | String | 合约名称, e.g. "eosio.token" |
| decimal | String | 币种对应的小数位数 |
| memo | String | 链上的MEMO备注 |
| info | String | 备注信息，用于语义化该笔转账交易 |
| orderId | String | 订单ID，dapp提供的订单ID，可为空 e.g. "20190606001" |
| callbackUrl（可选） | String | dapp server的回调url，上链成功会先回调dapp server,然后再唤醒移动端 |

### 类 ContractRequest

| properties | Type | Description |
| :--- | :---: | :--- |
| orderId | String | dapp 提供的订单ID, 可为空 |
| info | String | 备注信息，用于语义化该笔操作 |
| callbackUrl（可选） | String | dapp server的回调url，上链成功会先回调dapp server,然后再唤醒移动端 |
| list\ | [ContractAction](classes-and-methods.md#class-contractaction) 或者 [TransferAction](classes-and-methods.md#class-transferaction) | 合约操作action的列表 |

### 类 ContractAction

| properties | Type | Description |
| :--- | :---: | :--- |
| account | String | 合约名 |
| name | String | 合约方法 |
| info | String | 备注信息，用于语义化该笔操作 |
| data | Object | 根据合约abi定义所传的参数对象 e.g. {key1: value1, key2: value2 } |

### 类 TransferAction

| properties | Type | Description |
| :--- | :---: | :--- |
| account | String | 合约名 |
| name | String | 合约方法，填写"transfer" |
| info | String | 备注信息，用于语义化该笔操作 |
| transferObj | [TransferData](classes-and-methods.md#class-transferdata) | 转账信息对象 |

### 类 TransferData

| properties | Type | Description |
| :--- | :---: | :--- |
| from | String | 转账支出账号 |
| to | String | 转账接收账号 |
| quantity | String | 转账金额与单位 |
| memo | String | 链上备注信息 |

### 类 SignRequest

| properties | Type | Description |
| :--- | :---: | :--- |
| message | String | 需要签名的数据 |
| callbackUrl | String | dapp server的回调url，MYKEY绑定成功会先回调dapp server,然后再唤醒移动端 |

### 类 MYKEYResponse

| properties | Description |
| :--- | :--- |
| success | 成功的回调 |
| failure | 失败的回调, 错误列表[errorCode list](https://github.com/mykeylab/Documentation/blob/master/english/mykey_ios_sdk_en.md#error-code) |
| cancelled | 取消的回调 |

### 类 MYKEYApiResponse

| properties | Description |
| :--- | :--- |
| success | 成功的回调 |
| failure | 失败的回调 |

### 类 MYKEYWalletCallback

| methods | Description |
| :--- | :--- |
| onSuccess | 成功的回调 |
| onError | 失败的回调,[errorCode列表](classes-and-methods.md#error-code) |
| onCancel | 取消交易的回调 |

