# Android Classes

#### Class InitRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| context | android.content.Context | dapp application context |
| appKey | String | unique id assigned to each dapp, contact us |
| userId | String | the unique user id in dapp server side, recommend to use uuid |
| dappName | String | dapp name |
| dappIcon | String | dapp icon logo, no small than 144x144px |
| disableInstall\(default false\) | boolean | Whether to disable the default install page when MYKEY is not installed |
| callback | String | Deeplink MYKEY callback to dapp,defined in [AndroidManifest.xml](../../integrate-with-mykey/integration-android/preconditions.md#5-add-mykey-activity), e.g. customscheme://customhost/custompath |
| showUpgradeTip\(default false\) | boolean | Toast tip when MYKEY is not the latest version |
| mykeyServer | String | Environment URL endpoint of MYKEY server |

#### Class InitSimpleRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| context | android.content.Context | dapp application context |
| dappName | String | dapp name |
| dappIcon | String | dapp icon logo, no small than 144x144px |
| disableInstall\(default false\) | boolean | Whether to disable the default install page when MYKEY is not installed |
| callback | String | Deeplink MYKEY callback to dapp,defined in [AndroidManifest.xml](../../integrate-with-mykey/integration-android/preconditions.md#5-add-mykey-activity), e.g. customscheme://customhost/custompath |

#### Class AuthorizeRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| userName | String | Custom user name |
| callBackUrl\(optional\) | String | Optional, Callback endpoint url of dapp server，MYKEY will callback to dapp server after authorize request success at first, then wake up mobile client |
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
| info | String | Semantic description of MYKEY display to the user about this action |
| orderId | String | The order id from dapp, optional, can be null e.g. "20190606001" |
| callbackUrl\(optional\) | String | Optional, callback endpoint url of dapp server，MYKEY will callback to dapp server after transfer request success at first, then wake up mobile client |

#### Class ContractRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| orderId | String | The order id from dapp, optional, can be null |
| info | String | Semantic description of MYKEY display to the user about this action |
| callbackUrl\(optional\) | String | Optional, callback endpoint url of dapp server，MYKEY will callback to dapp server after contract request success at first, then wake up mobile client |
| actions | [ContractAction](android.md#class-contractaction) or [TransferAction](android.md#class-transferaction) | List of contract actions |

#### Class ContractAction

| properties | Type | Description |
| :--- | :--- | :--- |
| account | String | contract code name |
| name | String | contract action name |
| info | String | Semantic description of MYKEY display to the user about this action |
| data | Object | The parameter object passed according to the contract abi definition e.g. {key1: value1, key2: value2 } |

#### Class TransferAction

| properties | Type | Description |
| :--- | :--- | :--- |
| account | String | contract code name |
| name | String | contract action name, use "transfer" |
| info | String | Semantic description of MYKEY display to the user about this action |
| transferObj | [TransferData](android.md#class-transferdata) | Transfer info object |

#### Class TransferData

| properties | Type | Description |
| :--- | :--- | :--- |
| from | String | From account name |
| to | String | To account name |
| quantity | String | Amount and Symbol |
| memo | String | Memo |

#### Class SignRequest

| properties | Type | Description |
| :--- | :--- | :--- |
| message | String | Unsigned messages |
| callbackUrl | String | Optional, callback endpoint url of dapp server，MYKEY will callback to dapp server after sign request success at first, then wake up mobile client |

#### Class MYKEYWalletCallback

| methods | Description |
| :--- | :--- |
| onSuccess | Success Callback |
| onError | Failure Callback, [errorCode list](../error-code.md) |
| onCancel | Cancel Callback |

