# iOS Classes



#### Class InitRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| appKey | String | unique id assigned to each dapp, contact us |
| UUID | UUID | the unique user id in dapp server side, recommend to use uuid |
| dappName | String | dapp name |
| dappIcon | String | dapp icon logo, no small than 144x144px |
| disableInstall | boolean | Default: false, Whether to disable the default install page when MYKEY is not installed |
| scheme | String | Deeplink MYKEY callback to dapp,defined in [refer scheme configuration in 1.2](../../integrate-with-mykey/integration-ios/preconditions.md#2-add-url-scheme), e.g. demoscheme |

#### Class InitSimpleRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| dappName | String | dapp name |
| dappIcon | String | dapp icon logo, no small than 144x144px |
| disableInstall | boolean | Default: false, Whether to disable the default install page when MYKEY is not installed |
| scheme | String | Deeplink MYKEY callback to dapp,defined in [refer scheme configuration in 1.2](../../integrate-with-mykey/integration-ios/preconditions.md#2-add-url-scheme), e.g. demoscheme |

#### Class AuthorizeRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| userName | String | Custom user name |
| callBackUrl | String | Optional, Callback endpoint url of dapp server，MYKEY will callback to dapp server after authorize request success at first, then wake up mobile client |
| info | String | Info, Semantic description of MYKEY display to the user for authorization page |

#### Class TransferRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| from | String | From account |
| to | String | To account |
| amount | String | Amount, e.g "1.0000" |
| symbol | String | Symbol, e.g. "EOS" |
| contractName | String | contract code name, e.g. "eosio.token" |
| decimal | String | Decimal |
| memo | String | Memo |
| info | String | Semantic description of MYKEY display to the user about this transfer action |
| orderId | String | The order id from dapp, optional, can be null, e.g. "20190606001" |
| callbackUrl | String | Optional, callback endpoint url of dapp server，MYKEY will callback to dapp server after transfer request success at first, then wake up mobile client |

#### Class ContractRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| orderId | String | The order id from dapp, optional, can be null |
| info | String | Semantic description of MYKEY display to the user about this action |
| callbackUrl | String | Optional, callback endpoint url of dapp server，MYKEY will callback to dapp server after contract request success at first, then wake up mobile client |
| actions: \[BaseAction\] | [ContractAction](ios.md#class-contractaction) or [TransferAction](ios.md#class-transferaction) | List of contract actions |

#### Class ContractAction

| properties | Type | Description |
| :--- | :--- | :--- |
| account | String | contract code name |
| name | String | contract action name |
| info | String | Semantic description of MYKEY display to the user about this action |
| data | Any | The parameter object passed according to the contract abi definition，this any type should be valid by JSONSerialization.isValidJSONObject\(\_ obj: Any\)， e.g. {key1: value1, key2: value2 } |

#### Class TransferAction

| properties | Type | Description |
| :--- | :--- | :--- |
| account | String | contract code name |
| name | String | contract action name, use "transfer" |
| info | String | Semantic description of MYKEY display to the user about this action |
| data | [TransferData](ios.md#class-transferdata) | Transfer info object |

#### Class TransferData

| properties | Type | Description |
| :--- | :--- | :--- |
| from | String | From account |
| to | String | To account |
| quantity | String | Amount and Symbol |
| memo | String | Memo |

#### Class SignRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| message | String | Unsigned messages |
| callbackUrl | String | Optional, callback endpoint url of dapp server，MYKEY will callback to dapp server after sign request success at first, then wake up mobile client |

#### Class MYKEYResponse

| properties | Description |
| :--- | :--- |
| success | Success Callback |
| failure | Failure Callback, [errorCode list](../error-code.md) |
| cancelled | Cancel Callback |

#### Class MYKEYApiResponse

| properties | Description |
| :--- | :--- |
| success | Success Callback |
| failure | Failure Callback |

