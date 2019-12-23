# iOS类

### 类 InitRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| appKey | String | 为每一个dapp指定的唯一key |
| UUID | UUID | dapp为该用户提供的唯一ID,建议使用uuid |
| dappName | String | dapp的名称 |
| dappIcon | String | dapp的logo, 建议不低于144x144px |
| disableInstall | boolean | 是否禁用MYKEY未安装时显示默认引导页面 |
| scheme | String | MYKEY完成授权操作后返回Dapp的跳转链接，[参考scheme定义 in 1.2](../../integrate-with-mykey/integration-ios/preconditions.md#2-tian-jia-url-scheme), e.g. demoscheme |
| mykeyServer | String | MYKEY服务端环境Endpoint |

### 类 InitSimpleRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| dappName | String | dapp的名称 |
| dappIcon | String | dapp的logo, 建议不低于144x144px |
| disableInstall | boolean | 是否禁用MYKEY未安装时显示默认引导页面 |
| scheme | String | MYKEY完成授权操作后返回Dapp的跳转链接，[参考scheme定义 in 1.2](../../integrate-with-mykey/integration-ios/preconditions.md#2-tian-jia-url-scheme), e.g. demoscheme |

### 类 AuthorizeRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| userName | String | 自定义用户名 |
| callBackUrl | String | dapp server的回调url，MYKEY绑定成功会先回调dapp server,然后再唤醒移动端 |
| info | String | 备注信息，用于绑定认证页面的语义化描述 |

### 类 TransferRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| from | String | 转账账户 |
| to | String | 接受账户 |
| amount | String | 转账数量 |
| symbol | String | 币种Symbol, e.g. "EOS" |
| contractName | String | 合约名称, e.g. "eosio.token" |
| decimal | String | 币种对应的小数位数 |
| memo | String | 链上的MEMO备注 |
| info | String | 备注信息，用于语义化该笔转账交易 |
| orderId | String | 订单ID，dapp提供的订单ID，可为空 e.g. "20190606001" |
| callbackUrl | String | dapp server的回调url，MYKEY转账成功会先回调dapp server,然后再唤醒移动端 |

### 类 ContractRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| orderId | String | dapp 提供的订单ID, 可为空 |
| info | String | 备注信息，用于语义化该笔操作 |
| callbackUrl | String | dapp server的回调url，MYKEY合约调用成功会先回调dapp server,然后再唤醒移动端 |
| actions: \[BaseAction\] | [ContractAction](ios.md#lei-contractaction) 或者 [TransferAction](ios.md#lei-transferaction) | 合约操作action的列表 |

### 类 ContractAction

| properties | Type | Description |
| :--- | :--- | :--- |
| account | String | 合约名 |
| name | String | 合约方法 |
| info | String | 备注信息，用于语义化该笔操作 |
| data | Any | 根据合约abi定义所传的参数，类型需能通过JSONSerialization.isValidJSONObject\(\_ obj: Any\)验证， e.g. {key1: value1, key2: value2 } |

### 类 TransferAction

| properties | Type | Description |
| :--- | :--- | :--- |
| account | String | 合约名 |
| name | String | 合约方法，填写"transfer" |
| info | String | 备注信息，用于语义化该笔操作 |
| data | [TransferData](ios.md#lei-transferdata) | 转账信息对象 |

### 类 TransferData

| properties | Type | Description |
| :--- | :--- | :--- |
| from | String | 转账支出账号 |
| to | String | 转账接收账号 |
| quantity | String | 转账金额与单位 |
| memo | String | 链上备注信息 |

### 类 SignRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| message | String | 需要签名的数据 |
| callbackUrl | String | dapp server的回调url，MYKEY签名成功会先回调dapp server,然后再唤醒移动端 |

### 类 MYKEYResponse

| properties | Description |
| :--- | :--- |
| success | 成功的回调 |
| failure | 失败的回调， [errorCode ](../error-code.md) |
| cancelled | 取消交易的回调 |

### 类 MYKEYApiResponse

| properties | Description |
| :--- | :--- |
| success | 成功的回调 |
| failure | 失败的回调 |

