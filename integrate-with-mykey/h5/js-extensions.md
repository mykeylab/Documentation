# JS Extensions

JSBridge is injected javascript code in MYKEY dapp browser enviroment by default, which support Scatter protocol, and web3js protocol.

In order to control the MYKEY application conveniently, MYKEY adds the following methods:

| Method | Description |
| :--- | :--- |
| [closeWindow](js-extensions.md#close-app-window) | Close App window and return to MYKEY |
| [openFullScreen](js-extensions.md#open-full-screen) | Open full screen |
| [closeFullScreen](js-extensions.md#close-full-screen) | Close full screen |
| [forbidPhysicalBack](js-extensions.md#disable-physical-back-button) | Disable physical back button\(Android Only\) |
| [allowPhysicalBack](js-extensions.md#allow-physical-back-button) | Allow physical back button\(Android Only\) |
| [getAccountInfo](js-extensions.md#get-mykey-account-information) | Get MYKEY Account Information |
| [getClientConfig](js-extensions.md#get-client-configuration) | Get client configuration |
| [sendTransaction](js-extensions.md#send-transaction) | Send transaction |
| [sign](js-extensions.md#sign) | Apply MYKEY sign transactions |
| [setTitle](js-extensions.md#set-application-title) | Set application title |
| [showLoading](js-extensions.md#show-loading-animation) | Show loading animation |
| [hiddenLoading](js-extensions.md#hidden-loading-animation) | Hidden loading animation |
| [encodeFunctionCall](js-extensions.md#serialize-specified-function-call) | serialize specified function call |



### Close App window

Close App window and return to MYKEY.

```javascript
window.MyKey.Browser.closeWindow
```

### 

### Open full screen

Open full screen

```javascript
window.MyKey.Browser.openFullScreen
```

#### Params

<table>
  <thead>
    <tr>
      <th style="text-align:left">Param</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">isLandscape</td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">
        <p>true(Landscape)</p>
        <p>fasle</p>
      </td>
    </tr>
  </tbody>
</table>

### \*\*\*\*

### Close full screen

Close full screen.

```javascript
window.MyKey.Browser.closeFullScreen
```

###  ****

### Disable physical back button

Disable physical back button, only for Android

```javascript
window.MyKey.Browser.forbidPhysicalBack
```

### 

### Allow physical back button

Allow physical back button, only for Android

```javascript
window.MyKey.Browser.allowPhysicalBack
```

### 

### Get MYKEY Account Information

Get MYKEY Account Information

```javascript
window.MyKey.Browser.getAccountInfo
```

#### Params

<table>
  <thead>
    <tr>
      <th style="text-align:left">Param</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">openChain</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>true|fasle</p>
        <p>true: open creating page if no chains for an account</p>
      </td>
    </tr>
  </tbody>
</table>

#### Return

```javascript
Return format: Promise
{
	"data": {
		"accountName": "",     //Nickname in MYKEY
		"chainInfoList": ["account": "", "chain": ""],
		"id": "",    //unique id in MYKEY
		"operationKeys": ["","",""],   //public keys of three operation keys
		"xpubOperationKeys": ["","",""]  //public keys of three operation keys in Xpub format
	},
	"errorCode": 0
}
```



### Get client configuration

Get client configuration

```javascript
window.MyKey.Browser.getClientConfig
```

#### Return

```javascript
Return format: Promise
{
	"data": {
		"currency": "CNY",
		"locale": "zh-CN",
		"maxKycBindAccount": 1,
		"regioin": "CN",
		"userAgent": "",       //include field channel:MYKEY
		"recaptchaUserKey":""  //only use for MYKEY redpackage
	},
	"errorCode": 0
}
```



### Send transaction

Send transaction

```javascript
window.MyKey.Browser.sendTransaction(transaction) => Promise
```

#### Params

| Param | Type | Description |
| :--- | :--- | :--- |
| transaction | string | Specify chain and actions |

#### Return

```javascript
Return format: Promise
result: {
	"errorCode": 0,
	"errorMsg": "",
	"data": {
		"transactionId": "",
		"signature": ""
	}
}
```

#### Example：

Buy ram on EOS.

```javascript
window.MyKey.Browser.sendTransaction('{"actions":[{"account":"eosio","name":"buyram","data":{"payer":"","receiver":"","quant":"1.0000 EOS"}}],"chain":"EOS"},"extra":{"key":"value"}}')
```

New field extra will be effected from Android:2.5.0，IOS:2.5.0



### Sign

Apply MYKEY sign transactions

```javascript
window.MyKey.Browser.sign(message) => Promise
```

#### Params

| Param | Type | Description |
| :--- | :--- | :--- |
| message | string | unsigned message |

#### Return

```javascript
Return format: Promise
result: {
	"errorCode": 0,
	"errorMsg": "",
	"data": {
		"signature": ""
	}
}
```



### Get Transaction Progress

```javascript
window.MyKey.Browser.getTransactionProgress
```

#### Params

| Param | Type | Description |
| :--- | :--- | :--- |
| chain | string | EOS\|ETH |
| transactionId | string |  |
| blockNum | int | blockNum, optional |

#### Return

```javascript
Return format: Promise
result: {
	"errorCode": 0,
	"errorMsg": "",
	"data": {
		"percent": ,   //[0 - 100]
		"blockNum":    //[blockNum]
	}
}
```

Effective from: Android:2.5.0，iOS:2.5.0



### Set application title

Set application title

```javascript
window.MyKey.Browser.setTitle(title)
```

#### Params

| Param | Type | Description |
| :--- | :--- | :--- |
| title | string | application title |

### 

### Show loading animation

Show loading animation

```javascript
window.MyKey.Browser.showLoading
```



### Hidden loading animation

hidden loading animation

```javascript
window.MyKey.Browser.hiddenLoading
```

### 

### serialize specified function call

serialize specified function call

```javascript
window.MyKey.Browser.encodeFunctionCall(abi, method, param) => Promise
```

#### Return

result:{"errorCode":0,"errorMsg":"","data":"\[serialized data\]"}

#### Params

<table>
  <thead>
    <tr>
      <th style="text-align:left">Param</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">abi</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>abi for the function, type string, eg:&quot;[{\&quot;constant\&quot;:false,\&quot;inputs\&quot;:[</p>
        <p>{\&quot;name\&quot;:\&quot;_to\&quot;,\&quot;type\&quot;:\&quot;address\&quot;},{\&quot;name\&quot;:\&quot;_value\&quot;,\&quot;type\&quot;:\&quot;uint256\&quot;}],\&quot;name\&quot;:\&quot;transfer\&quot;,\&quot;outputs\&quot;:[{\&quot;name\&quot;:\&quot;\&quot;,\&quot;type\&quot;:\&quot;bool\&quot;}],\&quot;payable\&quot;:false,</p>
        <p>\&quot;stateMutability\&quot;:\&quot;nonpayable\&quot;,\&quot;type\&quot;:\&quot;function\&quot;}]&quot;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">method</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">method name, type string, eg:transfer&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">param</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>params, eg:{_to:&quot;0xc4ED1B3f31acadbE3c14B20fA766B6C4B1FAB208&quot;,</p>
        <p>_value:&quot;20000000000000000000&quot;}</p>
      </td>
    </tr>
  </tbody>
</table>



